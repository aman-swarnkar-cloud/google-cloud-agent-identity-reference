# Official Google Cloud references

This repository is based on publicly available Google Cloud documentation. Product behavior and capability status should always be validated against the latest official documentation before implementation.

## Gemini Enterprise Agent Platform

### Agent Platform overview
https://docs.cloud.google.com/gemini-enterprise-agent-platform/overview

Covers the overall Gemini Enterprise Agent Platform architecture, including Agent Runtime, Agent Identity, Agent Registry, Agent Gateway, governance, and observability.

### Agent Identity overview
https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/agent-identity-overview

Defines Agent Identity, SPIFFE-based agent identity, agent authority, user-delegated authority, and supported authentication models.

### Agent Identity with Agent Runtime
https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/agent-identity

Covers per-agent identity, least-privilege access, IAM integration, and credential security for agents running on Agent Runtime.

### Agent Gateway overview
https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/gateways/agent-gateway-overview

Covers Agent Gateway as the runtime enforcement point for agent communication, IAM policies, Agent Registry integration, Model Armor, and governance controls.

## Identity and authentication

### Agent Identity Auth Manager
https://docs.cloud.google.com/iam/docs/auth-manager-overview

Covers outbound authentication for agents, including OAuth flows, credential management, API keys, and user-delegated access.

## Documentation principle

Where Google Cloud documentation evolves or multiple pages describe different product generations, this repository follows the latest generally available APIs and current Gemini Enterprise Agent Platform documentation.

Legacy or preview capabilities will be explicitly identified when referenced.