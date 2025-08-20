---
title: Overview of building a conversational agent in Azure Logic Apps
description: An overview of the lesson that helps you build and deploy a conversational agent in Azure Logic Apps.
ms.service: logic-apps
ms.topic: tutorial
ms.date: 08/19/2025
author: absaafan
ms.author: absaafan
---

# Overview

This lesson introduces what’s possible when you build conversational and autonomous agents using Azure Logic Apps. It explains the unique capabilities that Azure Logic Apps brings to agent architectures and gives a short outline of this course.

## Introduction

Agents combine reasoning, state, and tools to take actions in response to user inputs or events. Using Azure Logic Apps for agents lets you separate the agent’s decision logic (the model, prompts, and orchestration) from reliable, production-grade execution and integration.

Azure Logic Apps provides a visual workflow engine that runs in a cloud service designed for integration and orchestration. Adding agent loop(s) into your workflow unlocks powerful AI reasoning coupled with the extensive features and proven reliability that Azure Logic Apps are known for.

## Building agents with Azure Logic Apps

Here are the main reasons to choose Azure Logic Apps when you build agents:

- Durable execution: logic app workflows can persist state and resume after service restarts, enabling long-running sessions, retries, and compensation patterns without losing context.
- Enterprise scale: the platform supports high concurrency, regional deployments, and resilient infrastructure to run many agent instances reliably.
- Monitoring and diagnostics: native integration with Azure Monitor, Log Analytics, and diagnostic settings gives you structured telemetry, traces, and the ability to create alerts and dashboards for agent behavior.
- Security and compliance: use Azure RBAC, managed identities, private endpoints, network controls, and platform encryption to protect secrets, connectors, and data flows.
- Rich connectivity: hundreds of built-in connectors, custom connectors, and HTTP actions let agents call SaaS APIs, databases, and on-premises systems without custom plumbing.
- Developer experience and automation: visual designer, ARM/Bicep/ Terraform support, and CI/CD integration help you ship and maintain agent workflows as code.

## Conversational vs autonomous agents

This course starts with conversational agents: systems that engage in back-and-forth interactions with a user (chat, help desk scenarios, guided workflows). Many of the same patterns also apply to autonomous agents, which take independent action on behalf of a user or system. Examples of overlap:

- both need durable state, retries, and secure access to connectors
- both benefit from observability and audit trails
- both require clear tool boundaries and safe error handling

Differences to keep in mind:

- conversational agents are typically user-driven, short-lived or session-based, and focus on natural language interaction.
- autonomous agents run longer, make independent decisions, and often require stronger governance, isolation, and automated rollback or compensation strategies.

## Agent loops in Azure Logic Apps

An agent loop is the orchestration pattern where an agent receives input (user message or event), decides on an action (generate text, call a tool, ask a clarification), executes that action, and then repeats this loop while maintaining context. In Azure Logic Apps, you implement the loop as a combination of model prompts, workflow logic, and connector actions so the agent can safely interact with external systems and persist progress.

For a short video overview, see the following introduction:

<iframe width="560" height="315" src="https://www.youtube.com/embed/rR1QjQTfCCg?si=BgW-1oa05NhOIovl" title="Introduction to logic apps agent loop" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Course Outline

This course will help you create your first conversational agent. Starting from a basic conversational agent in the first lesson and then building on that agent with tools and patterns. The later lessons in this course will go through the deployment of the agent and a conversational chat client.