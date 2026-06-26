# Skill: REST API Architect

## Description
Instructs the agent on how to plan, structure, and document strict RESTful API endpoints and database schemas. It enforces data validation boundaries and prevents decoupled modules from breaking.

## Execution Rules
1. **Schema First:** Never generate endpoint code without first defining the exact JSON request payload and response payload structures.
2. **Use Type Safety:** Explicitly require primitive types (e.g., String, Integer, Boolean) or specific object definitions for every data field.
3. **Enforce Idempotency:** Design data-modifying endpoints (POST, PUT, DELETE) to handle duplicate requests safely without corrupting data state.

## Output Structure Specification
Every API design document output must contain:
1. **Endpoint Route:** HTTP Method and URL path (e.g., `POST /v1/prd/generate`).
2. **Request Headers:** Authentication standards (e.g., `Authorization: Bearer <token>`).
3. **Request Body Schema:** A strict JSON block defining the expected data fields.
4. **Success Response (200/201 OK):** Exact JSON structure returned on a successful execution.
5. **Error Responses (400/401/422/500):** Clear error codes and user-readable message payload formats.
