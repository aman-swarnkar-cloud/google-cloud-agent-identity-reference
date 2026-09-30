# Grant IAM access to an Agent Identity

This example shows how to grant a specific Agent Runtime agent access to a Google Cloud resource using its Agent Identity.

The important point is that the IAM role is granted to the **agent principal itself**, not to the human user who deployed the agent.

## Agent Identity principal format

For a Google Cloud project that belongs to an organization, an Agent Identity has a principal identifier similar to:

```text
principal://agents.global.org-ORGANIZATION_ID.system.id.goog/resources/aiplatform/projects/PROJECT_NUMBER/locations/LOCATION/reasoningEngines/AGENT_ENGINE_ID
```

The identity is tied to the Agent Runtime resource and represents that individual agent.

## Grant a role to one agent

```bash
gcloud RESOURCE_TYPE add-iam-policy-binding RESOURCE_ID \
  --member="principal://agents.global.org-ORGANIZATION_ID.system.id.goog/resources/aiplatform/projects/PROJECT_NUMBER/locations/LOCATION/reasoningEngines/AGENT_ENGINE_ID" \
  --role="ROLE_NAME"
```

Replace:

- `RESOURCE_TYPE` — the Google Cloud resource type, such as `projects`
- `RESOURCE_ID` — the resource receiving the IAM binding
- `ORGANIZATION_ID` — your Google Cloud organization ID
- `PROJECT_NUMBER` — the project number containing the agent
- `LOCATION` — the Agent Runtime region
- `AGENT_ENGINE_ID` — the Agent Runtime resource ID
- `ROLE_NAME` — the IAM role required by the agent

## Example

The following example grants an agent permission to view Agent Registry resources at the project level:

```bash
gcloud projects add-iam-policy-binding PROJECT_NUMBER \
  --member="principal://agents.global.org-ORGANIZATION_ID.system.id.goog/resources/aiplatform/projects/PROJECT_NUMBER/locations/LOCATION/reasoningEngines/AGENT_ENGINE_ID" \
  --role="roles/agentregistry.viewer"
```

## Why this matters

A common mistake is to grant the required permission to the user deploying the agent or to a broadly shared runtime identity.

That does not answer the real authorization question:

> Which agent should be allowed to access this resource?

Granting permissions directly to the Agent Identity makes the authorization boundary explicit and allows different agents to receive different permissions.

For sensitive resources, I would prefer narrow permissions for the individual agent rather than granting the same broad access to every agent in the project.

## Project-wide agent permissions

Google Cloud also supports Agent Identity principal sets for granting common permissions to all agents within a project.

That can be useful for baseline capabilities such as logging, metrics, quota usage, or other common runtime requirements.

For access to sensitive data or privileged operations, individual Agent Identity bindings provide a stronger least-privilege boundary.

## Official reference

Google Cloud — Use Agent Identity with Agent Runtime:

https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/agent-identity