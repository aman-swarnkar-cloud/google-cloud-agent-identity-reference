# Agent identity architecture review checklist

Use this checklist when reviewing identity and authorization for an enterprise AI agent built on Google Cloud.

The goal is to keep three questions explicit:

1. Who initiated the interaction?
2. Which agent is performing the action?
3. Whose authority should the downstream operation use?

## 1. User and agent identity separation

- [ ] Is the human user identity clearly separated from the Agent Identity?
- [ ] Does each deployed agent have its own Agent Identity where supported?
- [ ] Can the design distinguish the user who initiated an interaction from the agent that executed the action?
- [ ] Is Agent Identity used as the security principal for agent-level authentication, authorization, and auditing?

## 2. Agent-level IAM

- [ ] Are permissions granted to the individual Agent Identity where agent-specific access is required?
- [ ] Does each agent receive only the permissions needed for its responsibility?
- [ ] Are broadly shared identities avoided where per-agent authorization would provide a clearer boundary?
- [ ] Are sensitive resource permissions reviewed separately for each agent?

## 3. Delegated user authority

- [ ] Does the downstream operation genuinely require authorization from a specific user?
- [ ] Is delegated authority introduced only for those user-specific operations?
- [ ] Does the agent retain its own Agent Identity while acting with delegated user authority?
- [ ] Where OAuth delegation is required, is Agent Identity Auth Manager used where applicable instead of building custom credential handling by default?
- [ ] Can the system distinguish the user who authorized the operation from the agent that executed it?

## 4. Governed destination access

- [ ] Are MCP servers, endpoints, tools, and other agents treated as separate authorization targets?
- [ ] Are approved destinations registered in Agent Registry where appropriate?
- [ ] Is Agent Gateway used where centralized runtime policy enforcement is required?
- [ ] Are IAM policies defined between Agent Identity principals and the destinations they are allowed to access?
- [ ] Is the default posture restrictive, with access granted explicitly rather than assumed?

## 5. Policy enforcement

- [ ] Are Agent Gateway policies evaluated using the identity of the source agent?
- [ ] Are least-privilege policies applied to agent-to-tool, agent-to-service, and agent-to-agent communication?
- [ ] Where stronger boundaries are required, have Principal Access Boundary policies been considered for the Agent Identity?
- [ ] Are policy changes validated in a non-production environment before enforcement?

## 6. Auditability

For each important downstream action, can the architecture answer:

- [ ] Which user initiated or authorized the workflow?
- [ ] Which Agent Identity executed the action?
- [ ] Which destination was accessed?
- [ ] Which authorization policy allowed or denied the request?
- [ ] Was delegated user authority used?
- [ ] Can the relevant decision be reconstructed from available audit and observability data?

## Architecture review questions

Before approving the design, I would ask:

> Should this resource trust the agent itself, or should it honor the authority of a specific user?

Then:

> If this agent is compromised or behaves unexpectedly, is its authorization boundary limited to only what this agent actually needs?

If either answer is unclear, the identity model probably needs further refinement.

## Design principle

My default position is:

> Give each agent its own identity and least-privilege authorization boundary. Add delegated user authority only where the downstream operation genuinely requires the user's authorization context.

This keeps user authority, agent authority, and destination policy explicit instead of collapsing them into a single trust model.