# ADR-0001: Metadata-Immune PostgreSQL RLS and Schema-Qualified Cryptography

* **Status:** Accepted
* **Date:** 2026-11-14
* **Security Domain:** Identity & Access / Data Isolation

## Context & Operational Constraints

Multi-tenant applications handling protected youth and mentor records under strict privacy mandates (COPPA) cannot rely on application-layer filtering alone to isolate tenant data. In managed PostgreSQL architectures using JWT authentication:

1. Claims inside `user_metadata` can be modified by authenticated clients via standard profile-update endpoints, making them unsafe for role or tier authorization.
2. Privileged database functions (`SECURITY DEFINER`) that call cryptographic helpers such as `digest()` without explicit schema qualification are vulnerable to `search_path` shadowing if a rogue function is created earlier in the resolution path.
3. Under a single-engineer maintenance model and micro-budget hosting ceiling, authorization invariants must be enforced deterministically inside the database engine rather than duplicated across client builds.

## Decision & Enforced Invariant

We enforce a strict **server-bound authorization and schema-isolation invariant** across all tenant-scoped tables:

1. **Metadata-Immune RLS Policies:** Row-Level Security (RLS) policies evaluate authorization strictly against server-controlled claims (`app_metadata`) and immutable `BEFORE UPDATE` trigger guards—never client-writable `user_metadata`.
2. **Schema-Qualified Cryptographic Calls:** All hashing operations explicitly invoke `extensions.digest(payload, 'sha256')` inside functions locked with `SET search_path = ''`.

```sql
-- Reference invariant pattern (sanitized)
CREATE OR REPLACE FUNCTION private.prevent_role_escalation()
RETURNS TRIGGER
LANGUAGE plpgsql
SECURITY DEFINER
SET search_path = ''
AS $$
BEGIN
  IF NEW.account_role IS DISTINCT FROM OLD.account_role
     AND current_setting('request.jwt.claim.role', true) <> 'service_role' THEN
    RAISE EXCEPTION 'Unauthorized modification of server-managed role metadata';
  END IF;
  RETURN NEW;
END;
$$;
```

## Alternatives Considered & Rejected

* **Application-Layer Role Checks Only:** Rejected because any new API route, background worker, or direct client query bypassing middleware creates a silent horizontal or vertical privilege-escalation vector.
* **RLS Policies Reading `user_metadata`:** Rejected because client SDKs allow end users to mutate their own `user_metadata` object, allowing trivial self-promotion of role attributes.
* **Unqualified `digest()` Calls with Default `search_path`:** Rejected due to classic PostgreSQL `search_path` hijacking risks inside `SECURITY DEFINER` execution contexts.

## Consequences & Verification Gate

* **Positive:** Eliminates client-side privilege escalation vectors at the storage layer; ensures cryptographic primitives resolve deterministically regardless of session `search_path`.
* **Trade-off:** Role transitions require an explicit server-side service context rather than direct client mutations, and all migrations must follow strict schema-qualification rules.
* **Verification Gate:** Automated pre-deployment SQL integration tests attempt cross-tenant `SELECT` queries and unauthorized `UPDATE` mutations under an authenticated end-user JWT, failing the release pipeline if any row leaks or role mutation succeeds.
