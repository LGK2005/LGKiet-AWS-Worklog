---
title: "Event 1"
date: "2025-10-03"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: “AI-Driven Development Life Cycle: Reimagining Software Engineering”

### Event Objectives

- Discuss how generative AI is reshaping modern software development.
- Present the AI-Driven Development Life Cycle (AI-DLC) and its main ideas.
- Showcase Kiro and Amazon Q Developer in practical demos.

### Speakers

- **Toan Huynh** – Specialist SA, PACE  
- **My Nguyen** – Sr. Prototyping Architect, Amazon Web Services – ASEAN  

### Key Highlights

- The core theme was AI-DLC, a model where AI helps coordinate the development lifecycle (planning, breaking down work, suggesting architecture), while engineers stay accountable for review, decisions, and governance.

**- AI-DLC Core Concept:** AI-DLC is human-centered: AI plays the role of a collaborator that amplifies developer productivity, with delivery cycles shortened from weeks/months down to hours/days.

**- AI-DLC Workflow:** The flow is iterative, alternating between AI steps (drafting plans, implementing changes, asking questions) and human steps (clarifying intent, approving plans), so AI only executes once humans confirm the direction.

**- AI-DLC Stages:** The lifecycle is grouped into three phases—Inception, Construction, and Operation—where each phase enriches the context for the next:

+ Inception: Capture context, refine intent via User Stories, and organize Units of Work.

+ Construction: Perform Domain Modeling, generate and test code, introduce architecture components, and deploy with IaC plus automated tests.

+ Operation: Run in production and handle incidents and runtime operations.

**- Challenges AI-DLC Addresses:**

+ Scaling AI development: Existing AI coding tools often struggle as systems grow more complex.

+ Limited control: Current tools offer limited ways to supervise and coordinate AI agents effectively.

+ Code quality: Maintaining production-grade quality when evolving from a PoC is difficult without more structure.

## Deep Dive: Kiro – The AI IDE from Prototype to Production

- Kiro is an AI-first IDE built around AI-DLC concepts, emphasizing spec-driven development.

**- Spec-driven Development:** From a single high-level prompt (for example, “build a Slack-like chat app”), Kiro derives structured artifacts such as requirements (requirements.md), architecture and design (design.md), and task breakdowns (tasks.md), shifting the workflow from ad‑hoc “vibe coding” to a traceable, spec-first process. Developers work with these specs as the primary source of truth.

**- Agentic Workflows:** Kiro’s agents execute against the spec while keeping developers in charge:

**+ Implementation Plan:** Kiro produces a concrete implementation plan with starting tasks and subtasks (for example, “add user registration and login endpoints”, “implement JWT middleware”) and links each back to the original requirements for validation.

**+ Agent Hooks:** Hooks dispatch work to AI agents on triggers like “file save”, allowing background automation such as documentation generation, unit test creation, or performance tuning.

### Key Takeaways

**- AI for Production-Grade Outputs:** By generating detailed designs (for example, data flows and API contracts) and tests before writing code, Kiro helps ensure AI-generated code is closer to production-ready software rather than throwaway prototypes.

**- Human Control via Artifacts:** Developers steer the system primarily by shaping and approving the artifacts—requirements, design docs, and task plans—rather than manually writing all implementation details, while AI agents handle execution.

### Applying to Work

**- Use Amazon Q Developer / Similar Tools:** Adopt AI coding assistants in academic or side projects to handle boilerplate and repetitive tasks, improving overall throughput.

**- Prioritize High-Value Work:** Offload routine work to AI to focus on higher-value activities like Domain Modeling and Architectural Design, which remain key human responsibilities in the Construction phase.  

### Event Experience

The **AI-Driven Development Life Cycle: Reimagining Software Engineering** session gave a clear view of where software engineering is heading. Generative AI was framed not just as a helper for writing code, but as an engine that can coordinate large parts of the lifecycle. The flow of the talk moved from the AI-DLC concepts into live demos with Amazon Q Developer and Kiro. The Kiro demo stood out, illustrating how a short prompt can be expanded into a complete, executable, and auditable development plan inside the IDE.

#### Lessons learned

- The pain points around scaling AI development, controlling AI behavior, and ensuring code quality make a structured, human-validated approach like AI-DLC feel both practical and necessary.

#### Some event photos

![Group picture during the event](/images/4-Event/TheBois-AIDLC.jpg)
