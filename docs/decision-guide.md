# Choosing the right identity model

The key question is not simply:

> Which credential can the agent use?

The better architectural question is:

> Whose authority should this action run under?

That distinction helps avoid giving agents broader access than the business operation actually requires.

## Decision guide

| Scenario | Recommended authority model | Why |
|---|---|---|
| Agent accesses a Google Cloud resource that should trust the agent itself | Agent Identity | Permissions can be assigned directly to the agent through IAM and kept independent of the end user. |
| Agent calls an external service for data or actions specific to a user | User-delegated authority | The target operation needs to respect the authorization granted by that specific user. |
| Agent invokes a governed MCP server, API, or registered endpoint | Agent Identity + Agent Gateway policy | The gateway can authorize the agent as a distinct principal before allowing access to the destination. |
| Agent must access a user-specific external tool through OAuth | Agent Identity + delegated user authority | The agent keeps its own identity while Auth Manager manages the user's delegated authorization for the external service. |
| Multiple agents need different access to the same resource | Separate Agent Identities | Each agent can receive only the permissions required for its own responsibility. |

## A simple rule of thumb

Use **Agent Identity** when the resource should trust the agent.

Use **user-delegated authority** when the resource must honor the permissions of a specific user.

Use both concepts together when the system needs to know:

- which agent is executing the action, and
- which user authorized access to the target service.

## What I would avoid

### Passing user credentials directly through the agent

The agent should not become a credential transport layer.

Where managed delegation is available, credentials should be handled through the platform's authentication capabilities rather than exposed directly to agent logic.

### Giving every agent the same broad identity

Shared permissions make least privilege and auditability harder.

Agent Identity exists specifically to make agent-level access boundaries possible.

### Using delegated authority by default

Delegation should be introduced because the business operation requires user-specific authorization—not simply because a user started the conversation.

## Architectural principle

My default design position is:

> Give the agent its own identity first. Add delegated user authority only when the downstream operation genuinely needs the user's authorization context.

This keeps the trust model easier to reason about and gives security teams clearer control over what each agent can do.