# Google Cloud Agent Identity Reference

A practical reference for separating **user identity**, **delegated user authority**, and **Agent Identity** in enterprise Agentic AI solutions built on Google Cloud and the Gemini Enterprise Agent Platform.

The central architectural question behind this repository is simple:

> **Whose authority should an agent action run under?**

That decision determines whether a downstream resource should trust the agent itself, the authority delegated by a specific user, or a combination of both.

## Why this repository exists

Enterprise agents can call APIs, invoke tools, access cloud resources, interact with MCP servers, and perform actions for users.

As those capabilities grow, identity can easily become blurred.

A user starting a conversation does not automatically mean every downstream operation should run with that user's authority. Likewise, giving multiple agents a common broad identity weakens least privilege and makes authorization harder to reason about.

This repository explores a cleaner model:

**User identity** — who initiated or authorized the operation  
**Agent Identity** — which agent is executing it  
**Delegated authority** — what an agent may do on behalf of a specific user

## Start here

### Understand the identity model

[`docs/identity-model.md`](docs/identity-model.md)

Explains user identity, Agent Identity, delegated authority, and how they relate.

### Choose the authority model

[`docs/decision-guide.md`](docs/decision-guide.md)

A practical decision framework for choosing between Agent Identity and user-delegated authority.

### Explore the reference architectures

[`docs/reference-architecture.md`](docs/reference-architecture.md)

Three conceptual patterns covering:

1. agent access using its own identity,
2. user-delegated access,
3. governed access through Agent Gateway.

It also includes a composite workflow example showing how the same agent can use Agent Identity for some downstream operations and user-delegated authority for others.

### Review an agent identity design

[`docs/identity-review-checklist.md`](docs/identity-review-checklist.md)

A practical checklist for reviewing user identity separation, Agent Identity permissions, delegated authority, governed destination access, policy enforcement, and auditability.

## Implementation examples

### Grant IAM access to an Agent Identity

[`examples/grant-agent-iam-access.md`](examples/grant-agent-iam-access.md)

Shows the Google Cloud IAM principal model and an example of granting a role directly to an individual Agent Identity.

### User-delegated OAuth

[`examples/user-delegated-oauth.md`](examples/user-delegated-oauth.md)

Explains when user delegation is appropriate and how Agent Identity Auth Manager fits into the OAuth flow.

## Official Google Cloud references

[`docs/references.md`](docs/references.md)

The technical content in this repository is grounded in publicly available Google Cloud documentation.

## Design position

My default architectural position is:

> **Give the agent its own identity first. Add delegated user authority only when the downstream operation genuinely requires the user's authorization context.**

This keeps the trust model explicit and makes least privilege, governance, and auditing easier to reason about.

## Scope

This is a generic technical reference based on publicly available Google Cloud capabilities and general security principles.

It is not intended to represent any specific customer implementation or prescribe a single architecture for every enterprise environment.
