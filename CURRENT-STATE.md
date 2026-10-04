Current State — Week 01

Date: 2026-10-03

Purpose

This document is the current source of truth for the Agentic Software Engineering Lab at the end of the first part of Week 01.

The goal of the laboratory is not to learn a particular coding assistant. The goal is to develop a repeatable engineering process in which software-development agents can investigate, plan, implement, test and review software while the human engineer retains control of requirements, architecture, important decisions and verification.

Core principle:

An agent producing code is not equivalent to producing quality software.

1. Program objective

The 12-month program is designed around approximately 10 hours per week.

The target capability is Agentic Software Engineering:

delegate progressively more implementation to agents;

work across multiple technology stacks;

minimize direct code writing without minimizing technical understanding;

concentrate human effort on architecture, requirements, context, decisions, verification and responsibility for the result;

develop a reusable agentic development methodology rather than dependence on one model or tool.

The intended progression is approximately:

Weeks

Focus

1–6

Direct agents, context engineering and baseline experiments

7–12

Agents, subagents, skills, hooks and MCP

13–18

Verification engineering, testing, evals, benchmarks and security

19–24

Java/Spring Boot + PostgreSQL

25–30

Flutter + backend

31–36

Swift/SwiftUI + Kotlin/Jetpack Compose

37–44

Agentic architecture, requirements, observability, distributed systems and cost/latency

45–52

Portfolio product, production engineering, flagship case study and professional positioning

Weekly allocation:

4 h — build software through agents;

2 h — concepts and learning;

2 h — build the agentic development system;

1 h — experiments and benchmarks;

1 h — documentation and public communication.

Rule:

No week should consist only of study. Every important concept should result in code, an experiment, an architectural decision, a metric, or documented evidence.

2. Tool strategy

The initial tool is OpenCode using free models.

The initial model selected for the laboratory is:

MiMo-V2.6-Flash Free

The model is intentionally treated as an interchangeable component.

The laboratory is not intended to become a model-ranking exercise. Later experiments may compare models, but the primary evaluation target is the agentic engineering process:

repository comprehension;

evidence quality;

reasoning and assumptions;

tool use;

planning;

implementation quality;

tests;

verification;

human interventions;

defects;

cost and iteration count.

3. Laboratory repository

Repository:

agentic-software-engineering-lab/

Current working structure:

agentic-software-engineering-lab/
├── README.md
├── experiments/
│   └── week-01/
├── notes/
├── metrics/
└── projects/
    └── task-management-api/

Longer-term target structure:

agentic-software-engineering-lab/
├── agents/
├── skills/
├── mcp/
├── hooks/
├── memory/
├── evals/
├── examples/
├── docs/
└── projects/

The larger structure is intentionally not being built yet. It should emerge from evidence gathered during the experiments.

4. Week 01 objective

The first week is designed to move from:

"using AI to write code"

towards:

"directing and evaluating an agent that performs software engineering work."

The agent should eventually be able to:

understand an unfamiliar repository;

identify relevant context;

investigate a requirement;

distinguish facts from assumptions;

propose a solution;

state uncertainties;

make a plan;

use tools;

implement a task;

run tests;

report evidence;

identify remaining uncertainty.

The human engineer remains responsible for accepting or rejecting important decisions and evidence.

5. Initial project: task-management-api

The experimental application is a small Java backend.

Technology baseline:

Java 21;

Gradle Kotlin DSL;

Spring Boot 3.5.6;

Spring Dependency Management 1.1.7;

spring-boot-starter-web;

spring-boot-starter-test;

JUnit Platform.

The initial source tree was deliberately minimal:

src/main/java/com/raulferrer/Main.java

There were no:

Spring Boot application annotations;

controllers;

services;

repositories;

domain classes;

persistence;

validation;

security;

tests;

application configuration.

The Gradle project declared Spring Boot, but the source code initially remained a plain Java main() application.

6. Initial Spring Boot verification

The repository contained a potentially misleading configuration: Spring Boot was declared in Gradle, but the source code did not contain a Spring Boot application entry point.

The agent inferred that bootRun would execute the existing Java main() rather than start a Spring context.

This was manually verified by running:

./gradlew bootRun

Observed result:

Hello and welcome!i = 1
i = 2
i = 3
i = 4
i = 5

BUILD SUCCESSFUL

There was no Spring Boot banner, embedded Tomcat startup or persistent HTTP server.

This produced the first important verification pattern:

OBSERVATION
    ↓
INFERENCE
    ↓
HYPOTHESIS
    ↓
EXECUTION / TEST
    ↓
EVIDENCE
    ↓
ACCEPT / REJECT

Working rule:

An agent's claim is a hypothesis until sufficient evidence exists to accept it.

7. Experiment 01 — Repository Analysis

Objective

Evaluate how an agent investigates an unfamiliar repository before modifying it.

Prompt constraints

The agent was explicitly instructed:

understand the repository only;

do not modify any file;

do not create any file;

do not run commands that change the project;

distinguish evidence, inference and unknowns;

stop after analysis.

Agent exploration

The agent inspected:

repository structure;

Git status/history;

tracked files;

Gradle configuration;

Gradle wrapper;

source tree;

Java version;

project entry point;

dependencies;

tests.

Observed agent behavior

The agent respected the no-modification constraint.

It correctly identified:

Main.java as the only application source;

absence of Spring annotations;

absence of tests;

Spring Boot and web/test dependencies;

Java 21;

absence of persistence;

absence of validation/security;

lack of established architectural conventions.

It also identified the important mismatch between the Gradle Spring Boot configuration and the plain Java entry point.

Human verification

The predicted bootRun behavior was manually executed and confirmed.

Result

Experiment 01 demonstrated useful repository investigation and evidence/inference separation.

It was committed and pushed to the repository.

8. Experiment 02 — Task domain research and planning

Requirement supplied to the agent

The application needs a Task domain containing:

id;

title;

description;

status;

creation date.

Initial statuses:

TODO
IN_PROGRESS
DONE

The agent was explicitly instructed:

Do not implement anything.

It had to produce:

relevant existing files;

proposed architecture;

domain model;

API design;

validation rules;

testing strategy;

potential risks;

assumptions;

unknowns;

step-by-step implementation plan.

For every assumption it was asked to explain the supporting evidence.

9. Experiment 02 — repository investigation

During the investigation the agent found:

notes/FIRST_TASK_PROPOSAL.md

as an untracked file at the repository root.

It read the file and determined that it only restated the task prompt and added no new requirements.

It then re-verified that the repository remained unchanged apart from that note.

This was a useful example of the agent checking additional repository context rather than assuming that only tracked source files matter.

10. Experiment 02 — strengths

The agent produced a detailed technical proposal.

It correctly identified important unknowns, including:

application entry point;

persistence technology;

schema management;

timestamp type and timezone;

status transition rules;

PUT/PATCH semantics;

error format;

listing and ordering;

quality gates;

field constraints;

deletion requirements;

configuration.

It proposed a coherent possible architecture involving:

task/
├── domain/
├── application/
└── api/

with concepts such as:

domain model;

repository port;

application service;

REST controller;

DTOs;

exception handling;

unit tests;

web-layer tests;

integration tests.

It also identified the existing Spring Boot entry-point problem and the Gradle/Spring Boot compatibility question as risks requiring verification.

11. Experiment 02 — problem detected

The most important issue was not an obvious hallucination.

The agent frequently filled unspecified requirements with technically reasonable design choices.

Examples included:

Long as the identifier type;

database-generated IDs;

/api/tasks as the REST base path;

title maximum length of 100;

description maximum length of 2000;

TODO as the initial status;

Instant as the creation timestamp;

DELETE endpoint;

particular status-transition semantics;

a specific error response shape;

specific update semantics.

These are all defensible engineering choices.

However, most were not requirements present in the supplied specification.

The important problem is therefore not:

"The design choice is wrong."

The problem is:

"The agent turned unspecified requirements into design decisions without an explicit decision point."

12. Important example: DELETE

The agent itself recognized that deletion was not specified:

Is DELETE required at all in the first slice?

Yet it included:

DELETE /api/tasks/{id}

in its proposed API.

This illustrates a subtle but important agent behavior:

An agent can recognize an uncertainty and still proceed as if the uncertainty had already been resolved.

This behavior must be controlled rather than merely asking the model not to hallucinate.

13. New requirements-engineering model

The experiments produced a more precise distinction:

REQUIREMENT
    ↓
FACT
    ↓
CONSTRAINT
    ↓
DESIGN OPTION
    ↓
DECISION

Example:

Requirement:
"Task has an id."

        ↓

Fact:
"The specification does not define the type."

        ↓

Design options:
Long / UUID / String

        ↓

Decision:
"Use UUID."

The final step is a decision.

It should not be silently inferred from convention.

14. Methodological rules derived so far

These are provisional rules. They should eventually be incorporated into the formal agentic development method after more experiments.

Rule 1 — Evidence before acceptance

An agent's claim is a hypothesis until sufficient evidence supports it.

Rule 2 — Recommendation is not requirement

A technical recommendation from an agent must not silently become a product or system requirement.

Rule 3 — Preserve uncertainty

Unknown information should remain explicitly unknown until a human or an explicit project constraint resolves it.

Rule 4 — Separate facts from design

Repository facts and requirements must be kept separate from architecture proposals.

Rule 5 — Verification is part of development

Testing a claim is part of the engineering process, not an optional step after implementation.

15. Current agentic workflow

The workflow currently emerging from the experiments is:

REQUIREMENT
     ↓
AGENT RESEARCH
     ↓
FACTS / EVIDENCE
     ↓
INFERENCES
     ↓
UNKNOWNs
     ↓
HUMAN DECISIONS
     ↓
AGENT PLAN
     ↓
IMPLEMENTATION
     ↓
TESTS
     ↓
EVIDENCE
     ↓
HUMAN REVIEW

The important change from conventional AI-assisted coding is that implementation is deliberately delayed until the relevant uncertainty has been resolved.

16. Experiment status

Experiment 01

Status: Completed

Repository analysis performed.

No code modification by agent.

Agent claims reviewed.

bootRun hypothesis manually verified.

Experiment documented.

Changes committed and pushed.

Experiment 02

Status: Research/planning completed; human review pending

Task requirement investigated.

Repository context investigated.

Architecture proposed.

API proposed.

Testing strategy proposed.

Risks identified.

Assumptions identified.

Unknowns identified.

No implementation performed.

Current position:

Experiment 02
     ↓
Requirements Decision Matrix
     ↓
HUMAN REVIEW       ← CURRENT STATE
     ↓
Experiment 03
Controlled implementation

17. What we are deliberately NOT doing yet

We are not yet:

implementing the Task domain;

choosing JPA;

choosing PostgreSQL/H2;

deciding UUID vs Long;

deciding API paths;

adding CRUD operations not required;

adding authentication;

adding pagination;

adding Flyway/Liquibase;

adding OpenAPI;

adding CI;

adding coverage tooling;

optimizing prompts;

comparing many models.

Those decisions should be introduced only when justified by requirements or by a controlled experiment.

18. Metrics

Metrics are being collected for each experiment.

Current metric categories:

Human time
Agent time
Agent iterations
Files modified
Files created
Tests executed
Initial failures
Human interventions
Agent errors

Planned additional metrics:

Cost
Files/lines changed
Review findings
Security findings
Defects discovered later
Rework

The purpose is not to produce marketing claims about AI productivity.

The purpose is to understand:

where agents work reliably;

where they fail;

how much context they require;

which controls reduce failure;

how much human supervision remains necessary.

19. Public communication

A first LinkedIn case study has been drafted around the first two experiments.

The intended framing is evidence-based:

what was tested;

what the agent did;

what was verified;

what went wrong;

what methodological change resulted.

The post deliberately avoids unsupported claims about productivity or superior AI performance.

The central message is:

Writing less code does not mean understanding less code.

For an agentic engineer, understanding, architecture, requirements and verification become more important as implementation becomes more delegated.

20. Next action

Before Experiment 03, create a Requirements Decision Matrix from the Experiment 02 output.

The matrix should classify every proposed element as one of:

REQUIRED
SUPPORTED BY REPOSITORY
DESIGN OPTION
REQUIRES HUMAN DECISION
OUT OF SCOPE
UNKNOWN

Only after the required decisions have been made should the agent be allowed to implement the Task domain.

21. Current strategic objective

The purpose of Week 01 is not to finish a task-management API.

The purpose is to establish the first version of a reliable agentic development loop:

Observe
  ↓
Understand
  ↓
Distinguish evidence from inference
  ↓
Preserve uncertainty
  ↓
Decide
  ↓
Delegate
  ↓
Verify
  ↓
Measure
  ↓
Learn

This loop is the foundation for the rest of the 12-month Agentic Software Engineering program.

22. Source-of-truth rule

This file is intended to preserve the current state of the program independently of any individual chat session.

When the methodology, experiments, decisions or project status change, update the repository documentation accordingly.

Do not rely exclusively on conversational memory for the state of the laboratory.