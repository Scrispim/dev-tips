# Claude Code for .NET Development — Course

A step-by-step guide to building a production project with Claude Code.

> **Work in progress.** This document covers lessons 1–2. Further lessons are added here as they are written.

## Summary

The production workflow has **5 phases** and **10 steps**:

### Phase 1: Requirements and Planning

- **Step 1:** Define Business Requirements
- **Step 2:** Define Non-Functional Requirements

### Phase 2: Architecture and Specification

- **Step 3:** Design the Architecture
- **Step 4:** Generate the Backend Specification

### Phase 3: Backend Implementation

- **Step 5:** Code the Backend
- **Step 6:** Review and Refine

### Phase 4: Frontend Implementation

- **Step 7:** Generate the Frontend Specification
- **Step 8:** Code the Frontend

### Phase 5: Deployment

- **Step 9:** Automate Deployment with CI/CD
- **Step 10:** Human Review, Test, and Deploy

## Workflow at a Glance

| Phase | Steps | Outcome |
| --- | --- | --- |
| 1. Requirements and Planning | 1–2 | Business and non-functional requirements documented |
| 2. Architecture and Specification | 3–4 | Architecture decided and backend spec generated |
| 3. Backend Implementation | 5–6 | Backend coded, reviewed and refined |
| 4. Frontend Implementation | 7–8 | Frontend spec generated and implemented |
| 5. Deployment | 9–10 | CI/CD automated, reviewed, tested and deployed |

## Steps in Detail

### Phase 1: Requirements and Planning

#### Step 1: Define Business Requirements

Every project starts with requirements (regardless of whether you use AI). Skip this step, and you will spend more time fixing misunderstandings than writing code.

A **Business Requirements Document (BRD)** describes what the system should do from the user's perspective. It covers user roles, core features, workflows, business rules, and edge cases.

**Prompt — analyze an existing BRD:**

```
You are a Senior Business Analyst reviewing a Business Requirements Document.

Read the attached BRD and analyze it thoroughly.

For each feature, identify and document:
1. All user roles and their permissions
2. Core entities and their relationships
3. Complete user workflows (step by step)
4. Business rules and validation constraints
5. Edge cases and error scenarios
6. Any ambiguities or missing requirements

Present your analysis as a structured document.
Flag every ambiguity with a [CLARIFICATION NEEDED] tag
so we can resolve it before moving to architecture.
```

If you do not have a BRD, you can create one interactively. Describe your project idea, and ask Claude to generate a BRD by asking you clarifying questions:

**Prompt — create a BRD from scratch:**

```
I want to build [describe your project in 2-3 sentences].

Help me create a Business Requirements Document by asking me clarifying questions about:
- User roles and permissions
- Core features and workflows
- Business rules and constraints
- Integration requirements

Ask questions one section at a time.
After I answer, I summarize my answers and move to the next section.
```

#### Step 2: Define Non-Functional Requirements

Most developers skip non-functional requirements and pay for them later during production incidents.

**Non-functional requirements (NFRs)** define the quality attributes of your system:

- **Availability** — will it be up when users need it?
- **Scalability** — can it handle 10x more users without a rewrite?
- **Performance** — how fast does it respond under load?
- **Cost** — how much will it cost to run at scale?
- **Security** — who can access what, and how is data protected?
- **Maintainability** — how easy is it to change or extend later?
- **Reliability** — does it recover gracefully when things go wrong?
- **Testability** — can you verify the code is easily testable?

These constraints directly affect architecture decisions in the next step.

**Prompt — define the NFRs:**

```
Based on the business requirements we defined, help me define
non-functional requirements for this project.

For each category below, extract from the document,
and if missing, suggest specific, measurable targets:

1. Availability: Uptime SLA, acceptable downtime per month
2. Performance: API response time targets (p50, p95, p99),
   page load time targets
3. Scalability: Expected concurrent users, data growth rate,
   peak load scenarios
4. Security: Authentication method, authorization model,
   data encryption requirements, compliance needs
5. Cost: Infrastructure budget constraints, hosting preferences
   (cloud provider, self-hosted)

Present each requirement with a measurable target and the reasoning behind it.
```

### Phase 2: Architecture and Specification

#### Step 3: Design the Architecture

This is where many developers make their biggest mistake with AI: they accept the first architecture suggestion without questioning it.

Use Claude as an **Architecture Consultant, not as an authority**. Present the requirements and NFRs, ask for options with trade-offs, and then make the decision yourself.

**Prompt — compare architecture options:**

```
You are a Solution Architect. I need help designing the architecture
for the project described in the attached requirements.

Based on the business requirements and non-functional requirements,
propose 2-3 architecture options. 

For each option, provide:
1. Architecture style (Modular Monolith, Microservices, Serverless)
2. Project structure and module/service boundaries
3. Database choice and schema approach
4. API design approach (REST, GraphQL, gRPC, Events)
5. Authentication and authorization strategy
6. Deployment model

For each option, clearly state:
- Pros and cons
- Which NFRs does it satisfy, and which ones does it trade off
- Team size and expertise requirements
- Estimated complexity

Recommend one option and explain your reasoning.
```

#### Step 4: Generate the Backend Specification

A **backend specification** is a detailed Markdown file that describes every aspect of the implementation. It is so detailed that a developer could implement the entire backend without asking many questions.

When you feed Claude a specification file, the generated code is consistent, follows your patterns, and requires minimal rework.

> **Set up `CLAUDE.md` first.** Before generating any specification or code, create a `CLAUDE.md` file in the root of your project. This is the single most impactful thing you can do.
>
> `CLAUDE.md` is a Markdown file that Claude Code automatically loads into context when you open a project. It tells Claude your tech stack, architecture, coding conventions, and patterns, so every generated file is consistent from the start. (How to create one is covered in a future lesson.)

With `CLAUDE.md` in place, your specification prompt becomes much shorter. Claude already knows your project structure, file naming, patterns, and conventions. The prompt only needs to describe the business-specific content: entities, endpoints, and business rules.

**Prompt — generate the backend specification:**

```
Based on the architecture we designed, create a detailed Backend
Implementation Specification as a Markdown file.

The specification must include:

1. Entity Definitions
   - All entities as C# classes with property types
   - Value objects and enums
   - Entity relationships

2. Database
   - EF Core mapping configuration for each entity
   - EF Core Migrations
   - Seeding strategy with sample data using Bogus

3. API Endpoints
   - For each endpoint: HTTP method, route, request body,
     response body with C# record definitions
   - Business rules and validation rules per endpoint
   - Error scenarios and expected error responses

4. Authentication and Authorization
   - Which endpoints require authentication
   - Which endpoints require specific policies or roles
```

There is no need to describe file naming, handler patterns, DI registration, or project structure. Claude already knows all of that from `CLAUDE.md` and your codebase.

### Phase 3: Backend Implementation

#### Step 5: Code the Backend

This is the core implementation step. Claude already knows your architecture, patterns, file naming, and conventions.

As a result of the previous step, you will get an MD file with the exact implementation plan.

The only thing we need to do in this step is press the **"Approve and Start the Implementation"** button in the plan window.

#### Step 6: Review and Refine

After generating the backend code and all the tests pass, review it.

You want a fresh look at the code during the review — so open a **new session** in Claude to reset the context before reviewing.

### Phase 4: Frontend Implementation

#### Step 7: Generate the Frontend Specification

The frontend needs its own specification, separate from the backend.

This one is a **Product Document Requirement (PDR)**. The PDR references the backend API and includes everything a frontend developer needs to build the UI.

**Prompt — generate the PDR:**

```
Based on the implemented backend, create a Frontend Product
Document Requirement (PDR) as a Markdown file.

The PDR must include:

1. Authentication Flow
   - Login page: POST /api/users/login request/response
   - Store JWT token, call GetMe after login
   - Refresh token flow: intercept 401 responses,
     call POST /api/users/refresh, retry original request
   - Logout: clear tokens and redirect to login

2. For Each Page
   - API endpoints it calls with an exact JSON request
     and response examples
   - UI layout: which fields to display, form inputs,
     table columns
   - Actions: create, update, delete buttons
     with their API calls
   - Validation rules matching backend validators
   - Error handling: how to display API errors to the user

3. Conditional Rendering
   - Read authorization policies from
     @file:UserPolicyConstants.cs
   - Show/hide action buttons based on HATEOAS links
     in API responses
   - Show/hide pages in navigation based on user claims

4. Paging
   - How to call paginated endpoints
   - Request parameters (page, pageSize)
   - Response shape (items, totalCount, totalPages)
   - UI: page controls, page size selector

Include complete JSON examples for every request and response
so the frontend implementation requires zero guessing about
the API contract.
```

Notice the `@file:UserPolicyConstants.cs` reference. This tells Claude to read the actual backend policy constants and use the exact claim names in the PDR.

#### Step 8: Code the Frontend

Just like the backend, press the **"Approve and Start the Implementation"** button in the plan window.

> **Tip:** If you use Claude, you can connect a **Figma MCP**, let your AI talk to Figma, and generate UI code that follows the exact design (it can even download images from Figma).

### Phase 5: Deployment

#### Step 9: Automate Deployment with CI/CD

With the backend and frontend working, the next step is to automate deployment.

**Prompt — generate the CI/CD pipeline:**

```
Create a GitHub Actions CI/CD pipeline for this project.

The pipeline should:

1. Trigger on push to main and pull requests
2. Backend (.NET):
   - Restore, build, and run all the tests
   - Build Docker image
   - Push to private GitHub container registry
3. Frontend (React):
   - Install dependencies
   - Run linter and tests
   - Build production bundle
4. Deploy to [your target environment]

Use separate jobs for backend and frontend so they run in parallel.
Cache NuGet packages and npm modules for faster builds.
Use environment variables for secrets (never hard-code them).
```

Adjust the deployment target to match your infrastructure. Whether it is Azure App Service, AWS ECS, a Kubernetes cluster, or a simple VPS with Docker Compose, the prompt should specify the exact target.

> **The human runs the deployment.** The one who executes commands to deploy your app, or runs Terraform or Pulumi scripts, should be you — and almost never the AI.
>
> You have probably heard the scary stories about AI dropping production databases. They all come from the same cause: commands that were not carefully reviewed and were allowed to execute without confirmation from a human.

#### Step 10: Human Review, Test, and Deploy

This final step is entirely human.

**Run the application end to end.** Test every user flow manually. Click through the UI. Call every API endpoint. Verify that error messages make sense. Check that authorization works correctly for different user roles.

**Review the generated code yourself.** Read through the handlers, validators, and endpoints. Look for business logic errors that the AI might have missed. AI is excellent at following patterns, but it does not understand your business domain as well as you do.

**Test edge cases.** What happens when you submit an empty form? What happens when two users edit the same record simultaneously? What happens when the database is down?

---

## The Plan File: Your Single Source of Truth

A Claude session is a terrible place to store a plan.

The context window fills up. The session ends. Your laptop reboots. And your teammate has no idea what you and Claude agreed on yesterday.

So stop treating the conversation as the source of truth. Every piece of real work starts with a **plan file**: a Markdown document in a `plans/` folder in the repository root.

The plan file records what to do, what is done, how it was verified, and what a future session needs to know.

**Prompt — create every plan:**

````
# Real Work

Turn planning into a durable, resumable artifact. The plan file — not the
conversation — is the source of truth: it records what to do, what's done, how it
was verified, and how to deploy. Any future agent can resume from it with zero
prior context.

**Use when** planning multi-step / multi-session work that may outlive the current
session. Skip for trivial single-session tasks.

## 1. Reach complete understanding first

Do **not** write the plan until scope is fully understood. Relentlessly ask the
user questions until you both share a complete understanding with **no gaps** —
treat an unasked question as a future bug.

- Don't stop at the first round; keep going until no ambiguity, assumption, or
  open decision remains. Probe edges: scope boundaries (in/out), dependencies,
  constraints, success criteria, data, environments, deployment, failure cases.
- Surface every assumption for the user to confirm. If an answer opens a new
  unknown, ask the follow-up — drill down recursively.
- Use `AskUserQuestion` for concrete choices. When done, summarize the full scope
  back and only proceed once the user confirms nothing is missing.

## 2. Write the plan

Save to `plans/<descriptive-name>.md` in the **repository root** (create `plans/`
if needed). Use this self-documenting template:

"""
# <Work Title>

<1-2 sentence goal and scope.>

## For Future Agents
As work proceeds: mark checkboxes `- [x]` as items complete; when a phase is done,
set its status to `Complete` and write its **Phase Summary** (what was done, key
decisions, anything needed to continue with zero context); run the phase's
**Verification Plan** and record the result before moving on. When all phases are
done, fill in **Final Recap** and **Deployment Plan**.

## Phase 1: <Title>
Status: Not started   <!-- Not started | In progress | Complete -->

- [ ] <concrete, actionable item>
- [ ] <concrete, actionable item>

### Verification Plan
- <command/check the agent can run autonomously, with expected result>

### Phase Summary
_(write when phase completes)_

## Phase 2: <Title>
Status: Not started
- [ ] <actionable item>
### Verification Plan
- <autonomous check>
### Phase Summary
_(write when phase completes)_

## Final Recap
_(write when all phases complete: summary of the entire piece of work)_

## Deployment Plan
_(write when all phases complete: step-by-step deployment instructions)_
"""

## Common mistakes

- **Vague items** — each checkbox is a concrete task ("Add retry logic to
  `PaymentClient.Charge`"), not a theme ("improve payments").
- **Non-autonomous verification** — give runnable commands with expected output,
  not "test it manually".
- **Wrong location** — always the repo-root `plans/` folder.
- **Pre-filling summaries** — phase summaries, recap, and deployment plan stay as
  placeholders until that work actually completes.
````
