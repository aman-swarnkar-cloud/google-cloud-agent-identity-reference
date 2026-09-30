# Identity model for enterprise AI agents

Enterprise AI agents can operate under different security contexts. A secure design should keep the identity of the human user separate from the identity assigned to the agent and introduce delegated authority only when the use case requires it.

This reference uses three concepts.

## 1. User identity

The user identity represents the human who initiated an interaction.

It helps answer:

- Who initiated the request?
- Has the user authenticated?
- What is the user authorized to access?
- Should an action be attributable to this user?

User identity should remain logically distinct from the identity of the AI agent executing the workflow.

## 2. Agent Identity

Agent Identity provides a unique identity for an AI agent.

On Google Cloud, Agent Identity is based on the SPIFFE standard and can be used by agents to authenticate to Google Cloud resources, MCP servers, endpoints, and other agents.

Agent Identity can participate directly in Google Cloud IAM, allowing administrators to grant permissions to an individual agent rather than relying only on shared workload credentials.

This helps support:

- least-privilege authorization
- per-agent access control
- stronger attribution
- auditing of agent activity
- governed agent-to-tool and agent-to-service communication

The agent identity answers:

> Which agent is performing this action?

This remains separate from the identity of the user who initiated the interaction.

## 3. User-delegated authority

Some workflows require an agent to access an external service on behalf of a specific end user.

For these scenarios, Google Cloud Agent Identity Auth Manager supports authentication models such as 3-legged OAuth.

With user-delegated authority:

- the user grants consent to the external service
- Auth Manager manages the relevant OAuth credentials and tokens
- the agent performs the action within the authority delegated by that user
- the user identity and agent identity remain distinct

Delegation does not mean that the agent becomes the user.

Instead, the architecture preserves three separate concepts:

**User** — who authorized the action  
**Agent** — which agent executed the action  
**Delegated authority** — what the agent is permitted to do on behalf of that user

## Choosing the authority model

A useful starting principle is:

> Use the agent's own identity when the resource should trust the agent itself. Use delegated user authority when the target operation must be performed within a specific user's authorization context.

The decision should be based on the authorization semantics required by the target system, not simply on which credential is easiest to obtain.

## Core security principle

Keep user identity and agent identity separate by default.

Introduce delegated user authority only where the business operation genuinely requires the agent to act on behalf of a user.

This separation provides clearer trust boundaries, supports least privilege, and improves the ability to reason about authorization and audit agent activity.

## Current Google Cloud capability status

As of September 2026:

- Agent Identity: Generally Available
- Agent Identity Auth Manager: Generally Available
- Agent Identity APIs: Generally Available
- Legacy IAM Connectors API: Preview and not planned for General Availability

New implementations should use the Agent Identity API rather than the legacy IAM Connectors API.