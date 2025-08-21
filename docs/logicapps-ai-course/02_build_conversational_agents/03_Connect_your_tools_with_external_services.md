---
title: Connect tools to external services (Module 3)
description: Learn how to connect a conversational Azure Logic Apps agent to external services using connectors and expose connector actions as tools.
ms.service: logic-apps
ms.topic: tutorial
ms.date: 08/13/2025
author: edwardyhe
ms.author: edwardyhe
---

# Connect your tools with external services (Module 3)

In this module, you will learn how to connect your conversational Azure Logic Apps agent to external systems using connectors, and how to expose connector operations to the agent as tools it can call during a conversation.

By the end, you will:
- Understand key connector concepts (actions, connections, authentication, limits).
- Add one read-only connector tool your agent can call automatically (no authentication required).
- Optionally add an authenticated connector tool and handle inputs and errors.
- Apply best practices for safe, reliable tool use in conversational scenarios.

We will keep the primary flow simple (no agent parameters and no on-behalf-of (OBO) yet). In Module 4 you will parameterize inputs, and in Module 5 you will add user context.

Helpful resources:
- Azure Logic Apps connectors overview: https://learn.microsoft.com/en-us/connectors/overview

---

## Why connectors add value for conversational agents

Azure Logic Apps connectors provide a fast, secure, and reliable way to let an agent take action in the real world without custom integration code.

- Breadth of integrations: Access a large catalog of prebuilt connectors across Microsoft 365, Azure services, SaaS apps, developer tools, and data platforms.
- Faster time to value: Use declarative actions instead of building and maintaining custom API clients. Reduce boilerplate, errors, and maintenance overhead.
- Secure connections: Standardize authentication with connections (Open Authorization (OAuth 2.0), API keys, managed identity). Store secrets securely and reuse connections across tools.
- Reliability features: Take advantage of built-in retries, pagination, and robust error surfaces exposed in run history and monitoring.
- Consistent patterns: A uniform action model simplifies how the agent invokes operations and how you compose flows around them.
- Observability: Use run history, monitoring, and telemetry to trace tool calls, diagnose failures, and tune prompts.
- Enterprise readiness: Combine connectors with network isolation and other platform controls to meet organizational requirements.

For conversational scenarios, connectors turn external system operations into agent tools. Provide a clear tool name and description, and the agent can decide when to call the connector and how to summarize results back to the user.

> [!TIP]
> Prefer managed connectors over raw HTTP when available to benefit from built-in authentication, throttling guidance, schemas, and monitoring.

---

## What are connectors, and how do they relate to tools?

- Connectors
  - Reusable integrations that let Azure Logic Apps call external services (Microsoft 365, GitHub, Azure services, SaaS apps, and more).
  - Expose operations as triggers (event-based starts) and actions (steps you can call).
  - Use a connection that holds authentication and environment-specific settings.
  - Managed connectors vs. built-in actions: managed connectors often require connections; built-in actions (HTTP, Compose, Variables) do not.

- Tools (in a conversational agent)
  - A "tool" is a capability the agent can invoke to satisfy a user's request (for example, "get weather," "look up a ticket").
  - In Azure Logic Apps agents, a tool can be backed by a connector action, a built-in action, or another workflow.
  - You provide the tool’s name and description so the agent knows when to call it. Good descriptions drive better tool selection by the agent.

> [!NOTE]
> Connector triggers are not supported in conversational agents. Conversational agents start when a chat session is initiated (for example, from an integrated chat client in Azure Logic Apps). There is no separate workflow trigger inside the agent; tool actions are invoked by the agent during the conversation.
---

## Prerequisites

- An Azure subscription with permissions to create logic apps and connections.
- A conversational logic app agent set up in the previous module(s) with model/provider configured.
- For the exercises:
  - Primary example: MSN Weather connector (no authentication required).
  - Optional authenticated example: a GitHub account and either OAuth 2.0 or a personal access token (PAT) with minimal read scopes.

> [!NOTE]
> This module focuses on connector-backed tools. Agent parameters and OBO patterns come later.

---

## Connector concepts you will use

- Actions vs. triggers: in conversational agents, connector triggers are not supported. Use actions as tools. Connector triggers are supported for autonomous agents.
- Connections and authentication: OAuth 2.0, API key, managed identity, or none depending on the connector.
- Inputs and outputs: actions define parameters and return JSON; your agent summarizes outputs for the user.
- Limits and reliability: expect throttling (HTTP 429), timeouts, retries, and pagination; design prompts and tools to handle gracefully.
- Connection references: your workflow binds to a specific connection at runtime.

---

## Part 1 (primary): Expose a simple read-only connector as a tool (MSN Weather)

We will start with a no-auth example to see the pattern end to end without parameters.

### Step 1 — Create your agent
- Open your logic app (Standard) and navigate to your conversational agent workflow (Agent Loop).
- Configure your agent with a model and system instructions, for example:
  - System Instructions: "You are a helpful agent".

> [!TIP]
> Review [Module 1](./01-create-first-conversational-agent.md) for a refresher on creating a conversational agent.

![Designer start for an agent workflow showing the agent loop](./media/03_Connect_your_tools_with_external_services/Start.png)

### Step 2 — Add a tool from a connector action
- Add a new Tool.
- Choose to create from a Connector action and search for "MSN Weather".
- Select "Get current weather".
- This adds the action to the tool.
- Name and describe the tool clearly, for example:
  - Tool name: GetCurrentSeattleWeather
  - Description: "Gets the current weather in Seattle."

> [!TIP]
> The LLM uses the tool description to decide when to call the tool. Keep it short, specific, and outcome-focused.

![Adding the "MSN Weather" connector action as a tool](./media/03_Connect_your_tools_with_external_services/AddMsnWeather.png)

### Step 3 — Configure the connector action
- Set the required parameters for the "Get current weather" action, for example:
  - Location: Seattle, US.
  - Units: Imperial.
- Save.

![MSN Weather "Get current weather action" configured for Seattle](./media/03_Connect_your_tools_with_external_services/GetCurrentWeather.png)

### Step 4 — Test in chat (see [Module 1](./01-create-first-conversational-agent.md))
- Ask: "What is the current weather in Seattle?"
- Verify output is expected.

![Testing the weather tool in the integrated chat client](./media/03_Connect_your_tools_with_external_services/PortalChat.png)

### Step 5 — Verify in monitoring view (see [Module 2](./02-debug-agent.md))
- Verify the tool is invoked and the agent summarizes the items with links.
- Use Monitoring/Run history to confirm tool calls and action success.

![Monitoring view showing successful tool invocation and run history](./media/03_Connect_your_tools_with_external_services/WeatherMonitoringView.png)

> [!NOTE]
> Inputs are hardcoded to avoid parameters in this module. You will parameterize them in Module 04. You can substitute any no-auth connector using the same pattern.

---

## Part 2 (optional): Expose an authenticated connector as a tool (GitHub)

Add a tool that requires authentication. Keep inputs static for now; parameterize in Module 04.

### Step 1 — Add the GitHub connector action
- Add a Tool -> From Connector -> search "GitHub".
- Pick a read operation such as "Lists all repositories for the authenticated user".

### Step 2 — Create the GitHub connection
- Create a new GitHub connection.
- Authenticate by following the consent flow using your existing GitHub account.

### Step 3 — Configure the tool (static inputs for this module)
- Tool name: GetGitHubRepositories
- Description: "Gets my public and private GitHub repositories".
- Save.

![GitHub connector action to list repositories for the authenticated user](./media/03_Connect_your_tools_with_external_services/ListGitHubRepositories.png)

### Step 4 — Test in chat
- Ask: "What repositories do I have?"
- Verify the tool is called and the result summarized.

> [!TIP]
> For private repositories, ensure the connection identity has access. In Module 04, replace static values with agent parameters so the user can specify owner/repo/number.

---

## Best practices for tools and connectors in agents

- Favor read-first: start with read-only tools; add writes later with confirmations.
- Clear descriptions: be specific about when to use (and not use) a tool.
- Provide examples and hints: improve parameter extraction with formats (for example, owner/repo) and sample utterances.
- Validate inputs: constrain enums and set defaults. Let the agent ask clarifying questions when required.
- Handle errors gracefully: plan for 401/403, 404, 429, timeouts. Keep responses concise and helpful.
- Respect limits: avoid fetching huge pages; summarize instead of retrieving everything.
- Least privilege: use managed identity or scoped tokens; store secrets securely.
- Require confirmation for writes: for create/update/delete, require explicit user confirmation.
- Observability: use run history/telemetry to verify tool calls and tune prompts.

---

## Troubleshooting

- Connection shows "Not authorized"
    - Re-authenticate with required scopes and verify target resource permissions.
- The agent does not call the tool
    - Strengthen the tool description and reduce overlap with other tools; verify input mappings.
- Wrong or missing parameters
    - Provide stronger hints and examples; make key inputs required in Module 04.
- 429/Throttling
    - Limit frequency, cache, or ask the user to narrow the query.
- Not found (404)
    - Verify owner/repo/number or location format; suggest alternatives.

---

## Adapting this to other connectors

- ServiceNow: list groups or create incidents (see Agent-in-a-Day samples in this repo for screenshots and patterns).
- Microsoft 365: get profile or list recent mail (requires OAuth and tenant permissions).
- Any REST (Representational State Transfer) API: use HTTP built-in, or managed connector if available.

Pattern recap: add the action, create/select the connection, set safe defaults, map compact outputs, test in chat, and observe runs. In Module 04 you will parameterize inputs; in Module 05 you will add user context (OBO) so the agent can act as the signed-in user where supported.

---

## Cleanup

- Remove demo tools if not needed.
- If you created new connections (for example, GitHub), remove or rotate credentials per your organization's policy.

---

## Next steps

- Module 04 — Add parameters to your tools (dynamic inputs from the agent).
- Module 05 — Add user context to your tools (OBO patterns).