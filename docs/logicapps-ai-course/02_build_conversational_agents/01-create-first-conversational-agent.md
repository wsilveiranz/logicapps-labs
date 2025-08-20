---
title: Build your first conversational agent in Azure Logic Apps (Module 01)
description: Learn how to create a conversational agent in Azure Logic Apps with AI reasoning and add your first agent tool.
ms.service: logic-apps
ms.topic: tutorial
ms.date: 08/19/2025
author: absaafan
ms.author: absaafan
---

# Module 01 - Create your first conversational agent in Azure Logic Apps

In this module, you will learn how to create your first conversational agent in Logic Apps. We will create a workflow with the new 'Agent' kind and then connect the AI model and add a tool that the agent can use.

By the end, you will:

- Have a conversational agent that you can interact with in the Azure Portal
- Understand key concepts concerning conversational agents in Azure Logic Apps
- The basics of tool execution and agent reasoning
- The basics of A2A agent protocol

We will keep the agent and tool simple to illustrate the concepts without getting overly complex. In later modules, we will build on top of these concepts to add more complex tool and agent logic.

The agent workflow we are creating itself is an A2A agent. In module 10, we will go into more details about this concept and configuration options.

---

## Prerequisites 

- An Azure subscription with permissions to create logic apps.
- [An Azure Logic App standard SKU workflow application](https://learn.microsoft.com/en-us/azure/logic-apps/create-single-tenant-workflows-azure-portal).
- [A deployed Azure OpenAI model](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)

---

## Part 1: Creating your new conversational agent

### Step 1 - Create your new conversational agent workflow
- Open your logic app (Standard) and navigate to the 'workflows' blade.
- Click on the "+ Add" action and select "Add".
- The "Create workflow" experience will come up on the right side of your screen.
- Enter a workflow name.
- Select "Conversational Agents".
- Click on the "Create" button.
- The workflow will be created and you will be redirected to this new workflow.

### Step 2 - Configure your agent loop connection
- Click on the "Default Agent" agent loop action. This will bring up connection configuration.
- Select "Add new" to add a new connection.
- In this module, we will create a connection to your existing Azure OpenAI resource.
- Select "Subscription" and select your subscription from the dropdown.
- Selecting your subscription will load the Azure OpenAI resources you have in that subscription. Select the resource you want to use.
    - Note that this resource should have a deployed GPT model. In this module, we will be using "GPT-4.1".
- Click on the "Create new" button.

### Step 3 - Configure your agent loop inputs
- Click on the "Deployment Model Name" dropdown and select your deployment model to use for this agent loop.
- Now enter the "System Instructions" for this agent. In this module, we are building a simple agent that 

### Step 4 - Adding a tool to your agent loop

### Note about system prompts and tool descriptions

### Step 5 - Chatting with your agent