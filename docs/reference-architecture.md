# Reference architecture

This reference separates three concerns that are often mixed together in enterprise agent designs:

1. the identity of the human user,
2. the identity of the agent,
3. the authority used to access a downstream resource.

The diagrams below show three common patterns on Google Cloud.

## Pattern 1 — agent accesses Google Cloud using its own identity

```mermaid
flowchart LR
    U[User] --> AR[Agent Runtime]
    AR --> AI[Agent Identity]
    AI --> IAM[Google Cloud IAM]
    IAM --> GCR[Google Cloud Resource]
```

In this pattern, the downstream resource trusts the **agent itself**.

The agent receives its own managed Agent Identity, and IAM determines what that specific agent is allowed to access.

This is a strong default for machine-to-machine access because permissions can be assigned at the individual agent level rather than through a shared service identity.

---

## Pattern 2 — agent acts on behalf of a user

```mermaid
flowchart LR
    U[User] --> AR[Agent Runtime]
    AR --> AI[Agent Identity]
    AI --> AM[Agent Identity Auth Manager]
    AM --> OA[User OAuth consent / token]
    OA --> EXT[External Tool or Service]
```

Here, the external service needs the authorization of the **specific user**, not only the identity of the agent.

The agent keeps its own Agent Identity while Auth Manager handles the delegated authentication required by the target service.

This preserves an important distinction:

- the user authorizes the action,
- the agent executes the action,
- the delegated credential defines the permitted user-level access.

---

## Pattern 3 — governed access to tools and agents

```mermaid
flowchart LR
    AR[Agent Runtime] --> AI[Agent Identity]
    AI --> AG[Agent Gateway]
    REG[Agent Registry] --> AG
    POL[IAM / Governance Policies] --> AG
    AG --> MCP[MCP Server]
    AG --> API[API / Endpoint]
    AG --> A2[Another Agent]
```

In this pattern, Agent Gateway acts as the enforcement point for governed agent communication.

The agent's identity can be evaluated against policies before access to registered tools, MCP servers, endpoints, or other agents is allowed.
---

## Composite example — one workflow using both authority models

A single agent workflow does not need to use the same authority model for every downstream operation.

The appropriate authority should be selected based on what each target system needs to trust.

```mermaid
flowchart LR
    U[User] --> AR[Agent Runtime]

    AR --> S1[1. Read configuration]
    S1 -->|Agent Identity| GCR[Google Cloud Resource]

    GCR --> S2[2. Retrieve user-specific data]
    S2 -->|User-delegated authority| EXT1[External Service]

    EXT1 --> S3[3. Invoke internal processing]
    S3 -->|Agent Identity| INT[Internal / Google Cloud Service]

    INT --> S4[4. Perform user-specific action]
    S4 -->|User-delegated authority| EXT2[External Service]

## My design principle

The architecture should answer two separate questions for every downstream call:

> **Which agent is making the request?**

and

> **Whose authority should the target operation use?**

Those questions should not be treated as interchangeable.

Agent Identity establishes the agent as a distinct security principal.

Delegated user authority should be added only when the downstream system genuinely needs the authorization context of a specific user.

Agent Gateway adds another control layer where agent-to-resource communication needs centralized governance and policy enforcement.

## Scope

These diagrams are conceptual reference patterns based on publicly documented Google Cloud capabilities.

They are not intended to prescribe a single topology for every enterprise implementation.