# Day 1 — 25 May 2026

# Microsoft Internship Learning Notes

Today was my first day as a Software Engineering Intern at Microsoft.  
The day mainly focused on understanding:


- AI Agents
- Agentic Workflows
- Multi-Agent Systems
- Microsoft Agent Framework
- MCP and A2A interoperability
- Basics of C#
- OOPS concepts
- Introduction to .NET Core


---

# AI Agents

## What are AI Agents?

AI agents are intelligent systems capable of:

- Understanding tasks
- Planning actions
- Using tools
- Taking actions autonomously
- Iterating based on feedback
- Producing final outcomes

Unlike traditional LLMs, AI agents can actually perform actions.

---

# Difference Between LLMs and AI Agents

## LLMs

LLMs mainly provide information and generate responses.

Example:

Asking:
> "What are the things to do in Hyderabad?"

The LLM provides:
- recommendations
- information
- suggestions

But usually does not perform actions.

Examples:
- GPT
- Claude
- Gemini

---

## AI Agents

AI agents can:
- understand goals
- plan tasks
- use tools
- execute actions
- improve responses through iteration

Example:

Instead of only suggesting events in Hyderabad, an AI agent can:
- book tickets
- add events to calendar
- reserve passes
- send reminders

This makes agents action-oriented systems instead of only information systems.

---

# Important Characteristics of AI Agents

## 1. Autonomy

Agents can work independently with minimal human intervention.

---

## 2. Planning

Agents can break complex tasks into smaller steps.

---

## 3. Iteration

Agents can retry tasks if the result quality is poor.

---

## 4. Feedback

Agents can evaluate outputs and improve future responses.

---

## 5. Human in the Loop

Humans can intervene during important decision-making stages.

This improves:
- safety
- accuracy
- reliability

![Agent Workflow](../assets/day_1/ss1.png)

---

# Agent Coordination

Sometimes multiple agents need to coordinate with each other.

Important considerations include:
- which agent should handle which task
- how information should be passed
- order of execution
- when to skip unnecessary iterations

Efficient coordination improves system performance.

---

# Orchestration Methods

Orchestration refers to how agents coordinate and communicate with each other.

---

## 1. Sequential Orchestration

Tasks are performed one after another in sequence.

Example:
- Agent 1 collects information
- Agent 2 analyzes it
- Agent 3 generates final response

---

## 2. Concurrent Orchestration

Multiple agents work simultaneously on different tasks.

This improves speed and parallelism.

---

## 3. Hierarchical Orchestration

A manager/supervisor agent controls other specialized agents.

The manager decides:
- task distribution
- priorities
- coordination

---

## 4. Handoff Orchestration

One agent transfers control or context to another specialized agent.

Example:
- booking agent hands off to payment agent

---

## 5. Group Chat Orchestration

Multiple agents collaborate like participants in a discussion.

Agents exchange:
- reasoning
- suggestions
- feedback

before generating final output.

---

# Components Required for AI Agents

AI agents require several important components.

![Agent Components](../assets/day_1/ss2.png)

---

## 1. LLM (Brain)

The LLM acts as the reasoning engine of the agent.

Examples:
- GPT
- Claude
- Gemini

The LLM helps in:
- understanding queries
- reasoning
- generating plans

---

## 2. Memory / Storage

Agents require memory to:
- maintain context
- remember conversations
- store information

Memory management is still an active research area because long-term contextual memory remains challenging.

---

## 3. Tooling / APIs

Agents use tools to perform actions.

Tools are essentially APIs or external functions.

Examples:
- booking tickets
- sending emails
- calendar scheduling
- database access

---

## 4. NLP (Natural Language Processing)

NLP helps agents:
- understand human language
- interpret intent
- process instructions

---

# Agentic Workflow

![Agent Workflow](../assets/day_1/ss3.png)

## What is Agentic Workflow?

Agentic workflow refers to the complete process followed by an AI agent to solve tasks.

---

# Steps in Agentic Workflow

## Step 1 — User Query

The user provides a request.

Example:
> "Book tickets for an event in Hyderabad."

---

## Step 2 — Planning

The agent analyzes the task and creates an execution plan.

---

## Step 3 — Action Execution

The agent uses tools/APIs to perform required actions.

---

## Step 4 — Reflection / Evaluation

The agent evaluates:
- response quality
- correctness
- missing information

If required, the agent can reiterate and improve the result.

---

## Step 5 — Final Response

The final validated output is provided to the user.

---

# Multi-Agent Workflow

![Multi-Agent Workflow](../assets/day_1/ss4.png)

## What is Multi-Agent Workflow?

In a multi-agent system, multiple agents collaborate to solve problems.

Different agents may specialize in:
- planning
- reasoning
- tool execution
- memory handling
- validation

Some systems may even use different LLM models for different tasks.

---

# Multi-Agent Workflow Process

## 1. User Input

The user provides the task.

---

## 2. Agent Selection

The system determines which agents should participate.

---

## 3. Classification

The task is categorized and routed appropriately.

---

## 4. Memory Handling

Conversational history and context are maintained.

---

## 5. Agent Collaboration

Multiple agents coordinate and exchange information.

---

## 6. Final Response

The system generates the final response after collaboration.

---

# Generative AI vs Agentic AI

## Generative AI

Generative AI mainly focuses on:
- content generation
- text generation
- answering questions
- creating images/code/content

It is mostly information-oriented.

---

## Agentic AI

Agentic AI focuses on:
- decision making
- execution
- taking actions
- tool usage
- automation

It is action-oriented instead of only information-oriented.

---

# Microsoft Agent Factory

## What is Microsoft Agent Factory?

Microsoft Agent Factory helps organizations transition from:
- AI experimentation
to
- production-scale AI execution

It supports enterprise-level deployment and governance.

---

# Features

- RBAC (Role Based Access Control)
- Training workflows
- Enterprise deployment
- Prepaid plans
- Scalable AI infrastructure
- Organizational AI adoption

---

# Microsoft Agent Framework

## What is Microsoft Agent Framework?

Microsoft Agent Framework is a unified framework for building production-ready AI agents.

It helps developers:
- create
- manage
- orchestrate
- deploy

AI agent systems efficiently.

---

# Interoperability in Microsoft Agent Framework

The framework supports interoperability between different agent systems.

---

# A2A (Agent-to-Agent)

A2A enables communication between multiple AI agents.

This allows:
- collaboration
- task delegation
- distributed problem solving

between agents.

---

# MCP (Model Context Protocol)

MCP is a standardized protocol that helps AI systems communicate with:
- tools
- APIs
- external systems
- data sources

in a structured and reusable manner.

MCP improves:
- interoperability
- tool integration
- extensibility of AI systems

---

# Key Takeaways

- AI agents are action-oriented systems.
- LLMs mainly generate information.
- Agents use planning, memory, tools, and feedback loops.
- Multi-agent systems improve scalability and specialization.
- Agent orchestration is important for coordination.
- Microsoft provides frameworks for enterprise-grade AI agent development.

---

# Topics to Explore Further

- Long-term memory in AI agents
- Tool calling architectures
- Agent security and governance
- Multi-agent optimization
- AI orchestration frameworks
- Production-ready agent deployment


---

# C# Basics

## What is C#?

C# is an object-oriented programming language developed by Microsoft.  
It is widely used for:

- Backend development
- Web applications
- Cloud services
- APIs
- Enterprise applications
- Desktop applications

C# works closely with the .NET ecosystem.

---

# Concepts Learned in C#

## Functions / Methods

Functions are reusable blocks of code used to perform specific tasks.

Example:

```csharp
using System;

class Program
{
    static void Greet()
    {
        Console.WriteLine("Hello");
    }

    static void Main()
    {
        Greet();
    }
}
```

---

# OOPS Concepts

OOPS stands for Object Oriented Programming System.

The main concepts are:

## 1. Encapsulation

Binding data and methods together into a single unit (class).

---

## 2. Abstraction

Showing only necessary details while hiding implementation complexity.

---

## 3. Inheritance

One class acquiring properties and methods from another class.

---

## 4. Polymorphism

Same method behaving differently in different situations.

---

# Introduction to .NET Core

## What is .NET Core?

.NET Core is Microsoft's cross-platform development framework used to build:

- Web applications
- APIs
- Cloud applications
- Microservices
- Backend systems

It supports:
- Windows
- Linux
- macOS

---

# Important Features of .NET Core

- Cross-platform
- High performance
- Open source
- Scalable
- Cloud friendly
- 
