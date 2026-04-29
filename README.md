
# The Architectural Cage for AI: How to Build an Enterprise-Grade App in 200 Hours

**By NXS8** | [core@nxs8.net](mailto:core@nxs8.net)

----

If you follow IT news, it might seem like programming is dead. Every day, videos pop up showing AI assembling a Tetris clone or a landing page from a napkin sketch in a minute. A dangerous illusion is born: just put a Product Owner in front of a chat window, and they'll "prompt" a new Uber into existence.

Reality hits much harder.

When you move beyond To-Do lists and try to build a real Enterprise product—with a complex role model, hundreds of screens, intricate UI, and tight integrations—classic AI prompting breaks down. **AI starts losing context, spawning kludges, breaking the layout, and generating uncontrollable legacy code faster than an entire team of juniors.**

This article isn't theoretical musings on the future of technology. It’s a practical manifesto (and, at times, a survival log) written based on a real project.

We rewrote the frontend of a complex service marketplace (Nexus-8) from scratch, creating a web app with 100+ screens that feels like native iOS. And we did it in roughly 200 man-hours working evenings and weekends.

Here, I’ll explain why advanced IDEs like Cursor struggle with scaling, how the shift to autonomous agents (Claude Code) changes the game, and most importantly—why **the new bottleneck in development is no longer writing code, but designing a rigid "Architectural Cage" for neural networks.**

Welcome to real AI engineering.

_Note: Project names and specific domain details have been abstracted to protect proprietary business logic while maintaining technical accuracy of the methodology._

## Table of Contents
1. [The Scale of the Disaster: 100+ Screens, 200 Hours, and Zero Budget for Errors](#1-the-scale-of-the-disaster-100-screens-200-hours-and-zero-budget-for-errors)
2. [The Given: Legacy, API, and the AI Stack Evolution (Why 200k Tokens is a Blessing)](#2-the-given-legacy-api-and-the-ai-stack-evolution-why-200k-tokens-is-a-blessing)
3. [Designing the "Skeleton": Architecture, Tech Stack, and Scaffolding](#3-designing-the-skeleton-architecture-tech-stack-and-scaffolding)
4. [The Illusion of Control: UI Failure, the Cursor Trap, and the Architectural Pivot](#4-the-illusion-of-control-ui-failure-the-cursor-trap-and-the-architectural-pivot)
5. [UI-First and the Regression Trap: Why Design Must Be "Bolted Down" at the Start](#5-ui-first-and-the-regression-trap-why-design-must-be-bolted-down-at-the-start)
6. [From PoC to Implementation: First Trials — The Atomic Approach](#6-from-poc-to-implementation-first-trials--the-atomic-approach)
7. [Mind Flashing: The 12 Hours That Changed Everything](#7-mind-flashing-the-12-hours-that-changed-everything)
8. [Advancing on a Broad Front: The Industrial Flow](#8-advancing-on-a-broad-front-the-industrial-flow)
9. [Constant Grooming and Taming Regressions: The Architectural Cage in Action](#9-constant-grooming-and-taming-regressions-architectural-cage-in-action)
10. [The "Centaur" Team: A New Anthropology of Development](#10-the-centaur-team-a-new-anthropology-of-development)
11. [The Price of Speed and Unexplored Territories: A Retrospective](#11-the-price-of-speed-and-unexplored-territories-a-retrospective)
12. [Epilogue: The Bottleneck Shift and the Tech Race](#epilogue-the-bottleneck-shift-and-the-tech-race)

## 1. The Scale of the Disaster: 100+ Screens, 200 Hours, and Zero Budget for Errors

| Parameter | Analysis (Stage: Problem Statement) |
| :--- | :--- |
| **Project** | **Nexus-8 (Local service marketplace)** |
| **Scope** | **100+ unique screens and states** |
| **Complexity** | **State-based Workflow, Bid-Ask Logic** |
| **Goal** | **Web app with a native iOS feel** |

When we decided to rewrite our product's frontend from scratch using AI exclusively, we didn't pick a "sandbox" project. We set out to build **Nexus-8**—a complex On-demand Service Platform (a local services marketplace).

The nature of the task didn't allow for cutting corners. The app had rigid business requirements:

1. **Complex Role Model:** Two complementary roles—End-Users (clients) and Service Partners (partners). The system had to support dual usage, where a single account could act in both roles simultaneously.
2. **Apple-oriented Design:** A strict UI/UX requirement. The web version had to look, animate, and feel like a native iOS app, as we planned to containerize it for the App Store in the future.
3. **Volume and Flow:** We had to implement over 100 screens. The core of the application was a complex contract lifecycle (Engagement Contracts: Draft -> Pending -> In-Progress -> Verification -> Completion) with auction-based logic (Bid-Ask).

**This was no landing page or simple CRUD interface. It was a full-scale Enterprise product. And we had only two to three months of evenings and weekends to launch it.**

## 2. The Given: Legacy, API, and the AI Stack Evolution (Why 200k Tokens is a Blessing)

| Parameter | Analysis (Stage: Starting Conditions) |
| :--- | :--- |
| **Backend & API** | **100% Ready (Prod Environment)** |
| **Business Logic** | **Reference from Android App (95% Match)** |
| **AI Stack v1** | **Cursor IDE + Model Zoo (Scaling Crisis)** |
| **AI Stack v2** | **Claude Code + Opus 4.5/4.6 (Success on a 200k Window)** |

Context is vital: we weren't starting the business from a clean slate. We already had a fully functional and production-launched Android app, a ready backend, polished databases, and a stable API.

The business logic of the new web application matched the Android version by 95% (although the UX/UI concept was new and matched only by 60%). This gave us a huge advantage: we didn't need to invent data models—they already existed.

Nevertheless, even with a ready backend, research into similar cases and our personal development experience clearly showed: creating such a web client the classic way with our resources would have taken at least 7 months, and realistically, one should have planned for 10-12 months. For us, such timelines were absolutely unacceptable. The only way to break through this barrier was a radical acceleration via the AI stack.

### AI Stack Chronology

The AI toolkit evolved at such a rate that our stack mutated right in the middle of the development process.

1. **Phase 1: Model Zoo and Cursor (Scaling Failure).** We started with Cursor, switching between different LLMs in search of the best result (I will detail this pain in [Chapter 4](#4-the-illusion-of-control-ui-failure-the-cursor-trap-and-the-architectural-pivot)). It was a manual, painful "copiloting" process.
2. **Phase 2: Claude Code and Opus 4.5 / 4.6 (Breakthrough).** The real work began with the arrival of Claude Code autonomous agents based on Opus models (versions 4.5 and then 4.6). They operated with a 200k token context window.
3. **Phase 3: The 1-Megabyte Era.** We received the 1M token context window when the project was already 80% complete.

### Important Conclusion: Orchestration Beats Brute Force

It might seem that since the project was written using "old" 200k models, this experience will quickly become obsolete. But in fact, this is **our main achievement**.

>***Architectural Axiom***: *A large context window (1M+ tokens) without a rigid structure is just a way to generate a megabyte of spaghetti code all at once. Engineering orchestration and domain isolation are more important than the "raw" memory of the model.*

The fact that we successfully built 100+ screens on a tightly limited context window proves: **our approach with an architectural cage (Nx + DDD) works**. We didn't try to 
feed the AI the entire codebase at once ("brute force"). We created a system where the AI received exactly the isolated domain it needed.

Models change, limits grow, but **the engineering orchestration of AI agents remains the primary skill**.

## 3. Designing the "Skeleton": Architecture, Tech Stack, and Scaffolding

| Parameter | Analysis (Stage: Foundation) | 
| ----- | ----- | 
| **Framework** | **Angular 19 + Ionic (iOS Mode)** | 
| **Monorepo** | **Nx (Modular Architecture)** | 
| **Patterns** | **Strategic DDD + Shell Architecture + Layered Architecture** | 
| **Core Services** | **Identity Provider, Push Notification Service, Geolocation Provider, Identity & Payment Gateways** |

### Starting Point:

* **Project:** Nexus-8 — a large-scale local service marketplace (On-demand Service Platform with Bid-Ask Logic).
* **Complex Role Model:** Two complementary roles (End-Users and Service Partners) with the ability to use a single account for both roles simultaneously.
* **Apple-oriented Design:** A strict UI/UX requirement — the web application must look and feel like a native iOS app (with an eye toward future containerization).
* **Scalability:** Over 100 screens of complex UI and numerous repeating business flows.

### Choosing the Foundation and Frameworks: Angular + Nx + Ionic

For a project of this scale, we needed a rock-solid foundation. Despite my extensive experience with declarative UI approaches (React, SwiftUI, Jetpack Compose), the choice was made in favor of **Angular**. Unlike React, which is essentially just a UI library and requires long-term "tuning" with dozens of third-party packages, Angular provides a comprehensive Enterprise solution "out of the box." Furthermore, in the world of AI development, structural predictability acts as "rigid rails" for the AI.

>***Architectural Axiom:*** *Models (LLMs) are much less likely to hallucinate and make mistakes in connections when the architecture is strictly typed, formalized, and has robust Dependency Injection.*

However, given the expected volume of business logic, Angular's inherent "verbosity" could quickly spiral into chaos. Therefore, the next logical step was choosing **Nx Monorepo**. Nx allowed us to build a modular structure and keep the architecture crystal clear through a strict separation of concerns.

As for the visual framework (**Ionic**), I initially made a mistake. Thinking that Ionic was overkill for a web application, I tried to "shoehorn" Apple design onto standard Material UI (Angular Material). This was a painful decision that cost us speed at the start. But I will detail how we eventually returned to Ionic and why it saved the project in the next chapter.

### Tech Stack and "Heritage"

The project wasn't created in a vacuum. We had a strict requirement to use existing services in the backend infrastructure: a corporate **Identity Provider (IdP)** for authentication, a cloud-based **Push Notification Service**, an external **Geolocation & Mapping Provider**, as well as complex Enterprise solutions: an **Identity Verification Service** and a secured **Payment Gateway**.

### Architectural Patterns: Our "Cage"

We weren't just creating folders; we were building a system of filters for AI generation. Traditional patterns became our **"Architectural Cage"**:

1. **Strategic Design (Domain-Driven Design):** Separation into Core Domain (e.g., State-based Workflow for Engagement Contracts: Draft -> Pending -> In-Progress -> Verification -> Completion — this is what makes money), Supporting Domain (profile), and Generic Domains (maps, addresses, media uploads).

2. **Shell Architecture (App Shell Pattern):** The app itself (`apps/portal`) is merely an empty container. All routing logic and page assembly are moved to `libs/shells`. This allowed us to isolate the End-User and Service Partner contexts while remaining in a single physical bundle.

3. **Layered Architecture:** A strict vertical hierarchy of dependencies. `Feature` layers know about `Service` layers, but never vice versa. We entrusted the enforcement of this rule to ESLint (more details in [Chapter 9](#9-constant-grooming-and-taming-regressions-architectural-cage-in-action).

4. **Faux-Application Pattern:** For example, a profile module opening inside a modal window lives as an independent logical "micro-frontend" with its own internal routing. This solves the problem of "hijacking" global routing when using smart components on top of main screens by using Relative Routing and providing an isolated `Routes` array directly in the providers of the wrapper component.

>***Design Summary:*** *We created a project tree that became a "navigation map" for the AI. Later, thanks to Nx and DDD, Claude Code could navigate through hundreds of files absolutely autonomously without violating domain boundaries.*

### AI-Driven Scaffolding: From 0 to the First Commit

An important nuance: in the role of **AI Architect**, I didn't build this structure by hand. The entire project launch process, from generating the Nx Workspace to the first code commits to the remote repository, was delegated to the AI (Cursor at that time).

I acted exclusively as the **Orchestrator**: issuing commands to create libraries according to our DDD strategy (`nx g lib...`) and validating the results.

The following is a fragment of the final structure we prepared for the AI:

* `apps/portal` — entry point (web application).
* `libs/domains/` — the heart of the business logic. Divided at the top level by subject areas (Contracts, Profile, Messaging, Contacts, Media), and isolated within those by role contexts: eusr (End-User), sprt (Service Partner), and cmn (common domain logic).
* `libs/shells/` — entry points.
* `libs/shared/` — reusable UI components (UI Kit) and API clients.
* `libs/core/` — core libraries.

This project tree (in conjunction with linter rules) became the reliable map by which Claude Code later navigated absolutely autonomously.

## 4. The Illusion of Control: UI Failure, the Cursor Trap, and the Architectural Pivot

| Parameter | Analysis (Stage: UI Integration / POC) |
| :--- | :--- |
| **Starting Toolkit** | **Cursor IDE (Copilot Approach) — Scaling Crisis** |
| **Final Toolkit** | **Claude Code (Autonomous Agents) — Successful Pivot** |
| **UI Framework** | **Angular Material (Discarded) ➔ Ionic (iOS Mode)** |
| **Process** | **From a single POC to systemic implementation** |

### Starting Point: The POC Phase
After setting up the macro-architecture, it was time to implement the Proof of Concept (POC). The idea was simple: implement the application’s basic skeleton—the "shell," the profile, and user settings. At this stage, I worked alone to prove the viability of the chosen stack.

At the start, the configuration looked like this:
* **Status:** Project scaffolded. A bare monorepo that successfully builds and runs but contains zero business logic.
* **Toolkit:** Cursor as the primary IDE (prior to the arrival of Claude autonomous agents).
* **UI Framework:** Angular Material (instead of Ionic).
* **Result:** A sluggish start and heavy stalling. The expected AI acceleration didn't happen; the layout broke constantly, and the tools simply weren't up to the task.

### The Cursor Trap: Why "Copilots" Can't Handle Architecture
At the beginning of the project, Cursor felt like a breakthrough. After manual work via copy-pasting from LLM web chats, the IDE-integrated context seemed like salvation. I already had experience with GitHub Copilot, and at that point, Cursor felt miles ahead.

However, the devil is in the details. Cursor, at that point, was brilliant for writing isolated functions, small components, or regular expressions. But as soon as you enter the zone of complex monorepo architecture, the tool begins to "stutter." When trying to refactor a complex UI component, it often loses the macro-context: it forgets Dependency Injection, ignores global design tokens, and spawns inline styles.

I found myself constantly switching between models, hoping to find the one that would finally "get it." Endless new chats when the current one hit a dead end, sincere hope that *"well, maybe this time it'll manage"...*

>***Architectural Axiom:*** *Cursor is a "scalpel on steroids." It is ideal for local fixes, but building a system of 100+ screens requires an autonomous construction crew (agents), not a fancy autocomplete or a solitary, limited builder.*

### The Angular Material Catastrophe: Design Specifics

This technical stalling coincided with my main strategic error. Believing that implementing Ionic for the web version was excessive "overhead," I decided to stretch an Apple-like design over Angular Material.

The logic was: we suffer through this once, set up the UI Kit and styles, and then everything will go "like clockwork." I fought with this for about a week during the POC stage. Initial results were encouraging, and that alone kept me from abandoning the idea immediately. But the price turned out to be too high: constant regressions, manual style tuning (on things that come out-of-the-box in native frameworks), mobile behavior issues, and thousands of lines of generated CSS kludges.

At some point, the process simply stopped. Maintaining this "Frankenstein" took more time than writing the logic itself.

>***Stage Summary:*** *AI doesn't know how to say, "Boss, this is a bad idea." It will obediently generate thousands of lines of CSS hacks over an unsuitable framework until you realize the dead end for yourself.*

### Shifting the Vector: Ionic and Claude Code

A sense of hopelessness and the proximity of a dead end forced me into an experiment—trying to rewrite one complex screen in Ionic. The result was shocking: what I had struggled with for days in Material fell into place in Ionic in a single prompt. I immediately decided to scrap all the Material code. Ionic provided that exact iOS styling (and behavior) out-of-the-box. There was no longer a need to write kludges—I just had to use the right tags (`ion-card`, `ion-modal`).

>***AI-Orchestrator Tip:*** *Don't try to "teach" an AI to mimic the behavior of one framework using the tools of another. Choose tools that are as close as possible to the target result "out of the box"—the AI will unlock their potential ten times faster.*

It was at this exact moment that Claude Code stepped onto the stage... Unlike Cursor, it understood the Ionic context perfectly. However, having acquired this powerful tool, I made a new mistake—I began using it as an obedient junior, micromanaging every single button (which I will discuss in [Chapter 6](#6-from-poc-to-implementation-first-trials--the-atomic-approach)).

## 5. UI-First and the Regression Trap: Why Design Must Be "Bolted Down" at the Start

| Parameter | Analysis (Stage: UI Development) | 
| ----- | ----- | 
| **Approach** | **UI-First (Creating the UI Kit before business logic)** | 
| **Problem** | **Regressions when mixing UI and logic in prompts** | 
| **Solution** | **Infrastructural UI and Design Tokens** | 
| **Status** | **Base ready for business domain injection** |

After switching to Ionic (as discussed in the previous chapter), we breathed a sigh of relief: the components finally began to look like native iOS. However, we soon encountered a new structural problem in AI-driven development.

### The Mixed Context Error

Initially, I tried giving the agents combined tasks: 
`"Create the Engagement Contract screen component, make it match the design, and connect it to the API to fetch data."`

In response, the AI would produce working code, but the UI was constantly "drifting." Margins would break, colors wouldn't match the design tokens, and the mobile layout would fall apart.

>***Architectural Axiom:*** *AI loses focus when trying to be both a designer and a backend integrator simultaneously. Any attempt to "fix a margin" in a mixed prompt can lead to an accidental break in business logic.*

### Foundation Before Business Logic

I quickly realized that presentation logic is just as fundamental a base as the Data Layer or Core libraries that will be reused throughout the project.

While a project is small, it's easy to debug styles and component architecture. If you start "piling" functionality onto a raw UI, the number of regressions at a scale of 100+ screens will kill all AI generation speed. The more complex your design, the stricter the rule must be: **UI first, logic second**.

### Beyond the UI Kit: Infrastructure Patterns

It wasn't just about buttons and cards. We had to debug the macro-behavior of the application:
* **Breakpoints:** Seamless transition between desktop and mobile modes.
* **Native Patterns:** The requirement that the app behaves like a phone app on mobile devices (e.g., using Bottom Sheets instead of desktop modals).
* **Complex Service Domains:** We needed to design an infrastructure where we could launch an independent micro-frontend (a series of screens with its own flow and deep links) inside a modal window. And this same micro-frontend had to be able to open not just in a modal, but directly in the main app shell. All of this had to work like Swiss clockwork.

The process was broken down into strict stages:

1. **Prototyping on Mock Data.** We forced the AI to create components and entire service flows exclusively using mock data. At this stage, we debugged internal routing within modals and interface behavior.
2. **Bolting Down (UI Kit) and Design Tokens.** Once an element looked perfect, we rigidly fixed the **Design Tokens** system—style constants (margins, typography, colors)—to forbid the AI from "fantasizing" with inline values.

>***Stage Summary:*** *Presentation logic is just as much a foundation as the Data Layer. The more complex your design, the stricter the rule: UI first, logic second.*

### The Ideal Pipeline (Advice for the Future)

Therefore, the ideal startup algorithm looks like this:

>***AI-Orchestrator Tip:*** *First, have the AI implement all complex UI nodes using mock data. Once the mechanics (routing, modals, Bottom Sheets) work flawlessly—"bolt down" the UI Kit and forbid the model from changing styles while writing business functions.*

I’ll be honest: we were far from this ideal ourselves and took quite a few hits in the process. I supervised the AI at every step, giving precise commands: 
`"Now take this finished UI component, change nothing in its HTML/CSS, and simply bind the business logic to it."`

But it is precisely this experience (and the mistakes made) that allows me to state for certain now: by polishing the UI Kit and infrastructure patterns at the start, you limit the degrees of freedom for the model and guarantee that when writing complex functionality, the agents will no longer be able to break your layout.

## 6. From PoC to Implementation: First Trials — The Atomic Approach

| Parameter | Analysis |
| :--- | :--- |
| **Approach** | **Atomic (screen by screen)** |
| **Role** | **Micromanager / Total Control** |
| **Speed** | **2-3x faster than manual work** |
| **Status** | **Unviable** |

### Starting Point:
* **Project scaffolded** (the basic monorepo structure is ready).
* **Tech stack approved** (including Ionic for iOS-mode).
* **Architecture designed** (Nx, Multi-domain, Clean Architecture).
* **UI foundation laid:** several basic screens implemented (back during the struggle with Angular Material), the first UI Kit components, design tokens, and breakpoints for three screen resolutions are fixed.
* **Backend connection:** API connected only to the extent necessary for the PoC.

By the start of this stage, a small portion (part of the user profile) had already been implemented during the PoC. It seemed to me that the path forward was clear and the time had come to simply build the app—starting with the remainder of the profile and onboarding as the simplest phase of the flow.

I also felt that I knew better than Claude how to handle the implementation, leaving him with just the "dumb coding". I knew the architecture, I knew the design, and I knew how to implement that design.

The idea was to implement screen by screen; on each one, I would instruct Claude on which UI solutions to implement, which UI kit elements to use, or which new ones to create. I would also have him create API endpoints as needed while testing everything in parallel. I stepped on a classic rake: I decided that my experience was the best documentation, mistakenly believing that describing functionality wouldn't take much time, that he would pick up the UI himself from screenshots, and that sooner or later the UI Kit would be filled (developed), making the screens I spent so much time on references for subsequent development so the work would go faster.

>***Architectural Axiom:*** *Passing knowledge in every prompt is the most expensive way to develop. Describing navigation rules, data exchange, and UI constraints anew in every chat is a waste of time and an overload of the architect's internal "stack."*

When functionality is already described one way or another (in our case, it was an Android app; in others, it might be Miro diagrams or functional requirements), scrupulously re-describing it in every prompt—along with the rules and constraints of navigation, display, backend data exchange, and so on—is a waste of time and overloads my internal stack with details.

**Problem #1: Lack of trust in Claude.** I was afraid to delegate the entire process to him, maintaining a role not just as an orchestrator and controller, but as a total supervisor and decision-maker at every level except the coding itself! To put it another way, I worked with him like a straight-A novice who had zero experience but had passed the TypeScript and Angular course exam with flying colors.

- `"This code should be in this module."`
- `"Move these styles to a common file."`
- `"Move these style constants to a design token."`
- `"Move this code to the UI Kit and (even!) name it such-and-such."`
- `"If the user has the consumer role, this button is unavailable."`
- `"When we are in mobile phone mode, the header transforms..."`
- and so on…

Of course, this is faster than manual programming and doesn't require expert knowledge of the language (which I happen to have), but it is *slow* programming.

I also performed meticulous PR reviews at this stage.

**Problem #2: Lack of clear rules for Claude.** Even though the project was initially structured in Nx according to Domain-Driven Design canons, it didn't save us from chaos during the atomic development stage because the rules of the game lived in my head, not in the AI configs.

>*AI is like a gas; it fills the entire provided volume. If the volume has no rigid walls (architecture), the gas turns into chaos.*

Nx provides a rigid structure for a human, but for an AI, it’s just a set of folders where it can dump trash unless there are clear rules. Constant regressions in both UI and functionality slowed the process down even more. Sometimes I spent at least an hour trying to guide Claude so that one problem was solved without regressions elsewhere, ensuring the app worked across all screen sizes (devices).

As a reminder, at this stage, the project descriptor existed in a minimal form (Claude created it himself after studying the project), and the rules and frameworks for development and architecture were not yet written into Claude.

>***Stage Summary:*** *A week was spent working with 100% cognitive overload. I was burning mental fuel dictating variable names to the AI instead of thinking about the product. In this mode, the project would have taken 6 months. Yes, that is two to three times faster than classic manual coding, but we didn't have those months.*

The conclusion was stark: either we radically change the approach, or we close the project. I needed to build an AI conveyor that would allow scaling the development to two pairs of hands—bringing in my colleague to help with the frontend generation. He knew the business logic and backend perfectly but had no much experience in web development, and the process had to neutralize that gap.


## 7. Mind Flashing: The 12 Hours That Changed Everything

| Parameter | Analysis (Stage: Mass Start) |
| :--- | :--- |
| **Approach** | **System-Prompt Engineering and Contextual Boundaries** |
| **Role** | **Architect / System Orchestrator** |
| **Speed** | **10-20x (10 screens / 15 minutes)** |
| **Status** | **Scalable and High Quality** |

### Starting Point:
By this point, the project was in a state of "limbo":
* Profile and onboarding functionality were partially implemented.
* The architecture (DDD + Clean Architecture) was fully defined during the PoC.
* Existing screens were responsive (Desktop/Tablet/Mobile) and connected to the backend.
* **The main deficit**: There was no system configuration for Claude and no direct link to the design in Figma.

Our PO (Product Owner) convinced me: instead of "chewing over" every button, we needed to explain the global task to Claude and let him go into "free solo" mode. I was skeptical—as an architect, I understood that without rigid boundaries, the task wouldn't converge. In the end, we found a compromise: implementation in large blocks (domains), but on the condition of creating a total architectural manifesto in `CLAUDE.md`.

### The 12-Hour Marathon
Everything was decided by 12 hours of continuous "four-handed" collaboration. We started with the standard `/init` initialization, allowing Claude to study the project, but 90% of the success depended on manual prompt configuration.

>***Architectural Axiom:*** *Instead of "spoon-feeding" the AI every button, invest time in creating a total architectural manifesto. This transforms the model from a "junior on call" into an autonomous engineer who knows the rules of the game.*

We divided the instructions into four critical vectors:

1. **Business**: Without this, the model starts to "hallucinate." You need to speak the same language and understand the specifics of Bid-Ask logic.
2. **Architecture**: This is the guarantee against regressions. A large task will fall apart if the AI doesn't know encapsulation rules (for example, not calling payment gateway methods when rendering an address form).
3. **UI/UX**: To ensure the app doesn't look like the work of ten different novices, all rules (modals, forms, Ionic specifics) must be locked in.
4. **Tooling**: These are the "hands and eyes" of the AI. Build commands, linters, and MCP servers allow the model to see the result and verify itself.

### Composition of the Nexus-8 Architectural Manifesto:

#### 1. Business Instructions
* **Overview**: "Nexus-8 — a local On-Demand platform (B2B/B2C)."
* **Domains and Roles**: Clear separation into **Service Partners** and **End-users**, plus common zones (Onboarding, Showroom).
* **Lifecycle**: A detailed description of contract states (**Engagement Contracts**) and the transitions between them.
* **The Source of Truth**: A rigid directive—the primary source of truth is the Android App. *"Unless explicitly stated otherwise—refer to Android. When in doubt—ask."* No improvisation in logic.
* **Terminology**: An internal glossary to avoid explaining the basics in every prompt.

#### 2. Architecture (Core of the System Prompt)
To prevent the AI from turning the project into "spaghetti," we embedded a rigid architectural frame into `CLAUDE.md`. Without understanding the rules of the game, the model might produce working code that is impossible to maintain. We solidified:

- **Tech Stack and Constraints**: The model must clearly understand the capabilities of Angular and Ionic to avoid suggesting irrelevant solutions.
- **Clean Architecture and Layers**: A description of dependency directions and layer hierarchy (Core, Shared, Utility, Feature). This is the foundation that eliminates circular imports.
- **Data Layer**: Specifications for services and domain stores. The AI must know where the data lives versus where the business logic resides.
- **Shell Architecture (Entry Points)**: Structure for the two main roles (Service Partner/End-user), ensuring isolation of user contexts.
- **Library Structure in Nx Monorepo**: To prevent the AI from scattering code everywhere, we gave it a map of our libraries (detailed in Chapter 3).
- **API Conventions**: A description of RESTful API patterns. This included pagination rules, endpoint naming standards, parameter passing principles (Body vs Query params), and the authentication protocol (OpenID Connect).
- **The Source of Truth Principle**: This is our "killer feature." Instead of vague phrases like "make it like the mobile app," we implemented a direct functional navigation method. In our API client code, each method contains metadata pointers to specific files in the Android project source. When encountering the `@android` tag, the AI didn't guess the logic—it followed the "navigation map" directly into the Kotlin code, extracted the algorithm, and translated it into an Angular service.
- **Build Environments**: A description of the differences between dev, preprod, and prod.
- **Localization (i18n)**: We immediately locked in the default language (English) and the library used. This is critical: internationalization is a massive task that can instantly "clog" the AI's context. If you don't build in i18n from the start, subsequent reviews and refactoring of the entire project become hell.

#### 3. UI/UX: Presentation Rules and Dynamics
In this section, we configured the interactivity model and visual standards of the application. It was important for the AI to understand not just *what* to draw, but *how* it should behave under different conditions. Instructions included:
- **Responsive Design**: A clear definition of breakpoints for desktop, tablets, and mobile devices.
- **Theming**: Rules for switching color schemes based on the business domain (Service Partner / End-User).
- **Apple HIG Compliance**: Strict adherence to Apple's design and UX guidelines. We built this into the foundation to ensure the future ability to package the web app into a container and deploy it to the App Store without issues.
- **Form Saving Strategy**: Rules for interface behavior during data entry (e.g., autosave on exit or explicit confirmation).
- **Design System**: Rigid requirements to use exclusively the ready-made UI Kit and Design Tokens, with paths to these libraries in the project specified.
- **Mobile Interface Specifics**: Rules for working with modal windows and the use of Bottom Sheets for the mobile version.
- **Ionic Framework**: Specific instructions for effective use of Ionic components and navigation.
- **Backend-Driven State Machine**: Our key architectural principle. To avoid describing button logic for 100+ screens, we moved interface management to the backend. The server sends a list of available actions for the current role, and the AI instructions rigidly fix the patterns for processing this stream (instant execution, confirmation, or complex custom scenarios). As a result, the AI didn't "draw" the interface from scratch; it calculated its visual state based on the contract phase and user permissions.

#### 4. Tooling
* **Commands**: Build, local serve, linting.
* **Code Style**: Naming and commenting rules (English only).
* **Context**: Setting up `.cursorignore` so the model doesn't waste limits on junk.
* **MCP Servers**: Connecting the Figma MCP ("eyes") and the Chrome DevTools MCP (the ability to see the DOM and debug code without user feedback).

### Result: A Quantum Leap
Preparing the entire base took practically a full working day. First, we tested the "flashing" on small changes in already finished screens. Once convinced that the instructions worked and small fixes achieved results without destroying the architecture, we decided on the first large-scale task.

The request was extremely brief: `"Implement screens for working with Contract Proposals. Figma: [List IDs]. Refer to the Android App for functionality."`

No chewing over API or navigation—it was important for us to see how the network of agents would handle it on its own using the context embedded in `CLAUDE.md`.

Claude went to work. Periodically, he requested confirmations (eventually, we allowed auto-applying all changes, checking only the commit diff), and this process took about 15 minutes. After reporting a successful build, we launched the dev server.

**The result: about 10 screens were created and were fully functional.** Yes, there were many "glitches" requiring fixes, but the walls were up. The entire chain—from the UI to the backend endpoints—worked.

>*Without deep analysis, it was clear: in these 15 minutes, the agents churned out a volume that would have taken a* ***week*** *in classic mode. This concluded our 12-hour marathon, turning the page in the development of Nexus-8.*

### Post-scriptum: Architectural Cage
One of the most important steps was taken slightly later. To maintain the cleanliness of Nexus-8 amidst such explosive code growth, I set up an automated architectural compliance system (**Architectural Cage**).

Through static code analysis, we solidified the AI agent's obligation to check dependencies after each iteration in the configuration. If the AI, in pursuit of speed, tried to violate domain boundaries (for example, importing Service Partner logic into the End-user domain), the linter blocked the changes. This allowed us to effectively separate roles: my colleague focused on the rapid assembly of functional blocks, while I, through control rules, guaranteed that the project maintained modularity and did not turn into "spaghetti."

We will discuss this mechanism in more detail in the following chapters.

## 8. Advancing on a Broad Front: The Industrial Flow

| Parameter | Analysis (Stage: Nexus-8 "Caging") |
| :--- | :--- |
| **Approach** | **System-Prompt Engineering and Contextual Boundaries** |
| **Role** | **Architect / System Orchestrator** |
| **Speed** | **10-20x (Scalable Flow)** |
| **Status** | **Scalability and High Quality** |

### Starting Point:
- **Configuration**: The first 12-hour marathon of setting up `CLAUDE.md` is complete. The model has received the "firmware" of our architectural principles and rules of the game.
- **First Success**: The first business feature has been successfully implemented. We have confirmed that the "cage" works and the AI is capable of producing high-quality code within the specified boundaries.
- **PoC Foundation**: We already had a UI Kit and Design Tokens ready, laid down during the proof-of-concept phase, as well as a minimum viable data layer serving the initial screens.

Now that the foundation has withstood the load, it’s time to build the entire structure.

After the first success with the autonomous implementation of an entire domain, we felt a real surge of momentum. There was a temptation to turn the AI loose on the whole project at once, but architectural intuition suggested that such a task would be too much for the model to handle.

We stood between two extremes:
1. **The Atomic Approach** ([Chapter 6](#6-from-poc-to-implementation-first-trials--the-atomic-approach)) — slow micromanagement of every pixel.
2. **The "Big Bang" Approach** — the ultimate fever dream of every PO and manager: a magical "Do Everything Now" button fueled by the naive hope that it births a unicorn instead of a pile of digital shrapnel for the architect to dig out later.

The truth, as always, was in the middle. We needed to divide **Nexus-8** into several large "wings" or technological floors to work collaboratively without blocking each other.

We divided the entire process into two strategic echelons:

### I. The Infrastructure Echelon: Services and Shared Libs

This is the foundation reused by all business domains.

- **Data Layer and API**: We gave one massive prompt to generate all endpoint logic in the data layer, using the Android code as a functional blueprint.

- **System Services**: Authentication (OIDC), Push Notifications, and Messaging. We set the task: "Implement the logic 1-to-1 as in the mobile version, but strictly within the framework of our new NX architecture".

- **UI Kit and Design Tokens**: Since the designer had left the team before the migration started, we had to "extract" the remainder of the design system manually. We fed the AI typical interfaces from Figma and asked it to identify common components based on Ionic. This was the final push to formalize everything that hadn't been covered in the earlier stages; it gave birth to variables for typography, spacing, and colors that became the absolute law for all subsequent screens.

- **Service Domains**: Isolated modules with their own flow:
  - Address Book and Geo-services.
  - Media and Avatar management system.
  - Multi-factor verification and Identity SDK.
  - Payment gateway integration.

>***AI-Orchestrator Tip:*** *For every module (payments, geo-services, media), the AI is required to write internal documentation (`README.md`). This allows the model to "recall" its own developments in the future and use them correctly without human intervention.*

### II. The Business Echelon: Functional Domains
Once the foundation was ready, we moved on to the "meat" of the application:
- Profiles and account settings.
- Complex multi-step onboarding.
- The lifecycle of Engagement Contracts (the heart of Nexus-8).

The algorithm was ruthless: the first prompt took a batch of screens from Figma and required the implementation of functionality based on the Android reference. The data layer and UI Kit were picked up automatically.

Of course, this wasn't a "magic button." The AI sometimes fantasized or copied Android specifics too literally. This is where the new role emerged—the **AI-Orchestrator**. When the architecture and the rules of the game (`CLAUDE.md`) are set correctly, the work turns into iterative tuning. The speed gain remained colossal—dozens of times faster compared to classic coding.

### III. Technological Vision and New Rules
The **Chrome DevTools MCP server** became a huge help. It literally gave the AI "eyes," allowing the model to independently analyze the DOM model and debug the application in real-time, rather than relying solely on my error descriptions.

In the heat of the "battle," our `CLAUDE.md` was constantly updated with new "commandments":
- Rigid conventions for the API client and pagination.
- Specifics for handling media content (sizes, caching).
- Apple HIG standards for forms and modal windows.

This is how we reached the mode of industrial code production. However, this flow would have instantly devolved into chaos without two critical factors: **constant architectural grooming** and **a clear separation of team roles**.

But that's for the next chapters.

## 9. Constant Grooming and Taming Regressions: Architectural Cage in Action

| Parameter | Analysis (Stage: Architectural Grooming) | 
| :--- | :--- | 
| **Approach** | **Automated Linting and AI-Assisted Refactoring** | 
| **Role** | **Architect / System Orchestrator (The "Cleaner")** | 
| **Impact on Speed** | **Prevention of Critical Tech Debt** | 
| **Status** | **Critically Important for Survival** |

### Starting Point:

* **Mass Start Launched:** Claude generates code in entire features; development speed has increased 10x+.
* **New Problem:** The generation speed of hidden technical debt (AI-Legacy) has grown proportionally.
* **AI Behavior:** Despite the presence of `CLAUDE.md`, during long sessions, the AI starts to get "lazy," lose context, or choose the path of least resistance.

Even though we established fairly rigid boundaries for Claude in the system prompt, this didn't guarantee 100% compliance. AI is a probabilistic system. While things improved significantly, regressions still popped up periodically like weeds.

>***Architectural Axiom:*** *AI writes code incredibly fast, but without rigid control, it creates AI-Legacy even faster.*

We primarily fought entropy on three fronts:

1. **UI/UX Regressions:**
   * Violation of design system rules. For example, we had two color themes based on user roles. While unique screens were described unambiguously, Claude might mistakenly render reusable screens (like profile settings) in the wrong theme.
   * Reverting to hardcoded values (`16px`, `#333333`) instead of using established Design Tokens (`var(--spacing-md)`, `var(--theme-text)`).
   * Incorrect behavior of complex forms (lack of validation, broken button states).

2. **Architectural Violations:**
   * Attempts to create circular dependencies.
   * Violating the dependency vector (importing logic "up" through the layers).
   * Ignoring existing library components and "reinventing the wheel."

3. **Functional Bugs:**
   * Distorted business logic behavior when connecting to the backend.

### Iterative Cleanup and AI-Assisted Code Review

Since the application was correctly designed from the start (DDD + Clean Architecture), iterative fixes didn't break the project but led to its constant improvement.

How did we structure the grooming process?

**First, continuous manual testing.** Daily work with the application to identify "glitches."

>***AI-Orchestrator Tip:*** *Don't try to fix minor flaws immediately. Fix blockers instantly, but accumulate "visual noise" and missing design tokens in a backlog for batch refactoring.*

**Second, regular AI reviews.** Ranging from once a week to daily sessions. I used Claude himself to audit his own code. Typical grooming prompts looked like this:

- `"Review the recent changes for hardcoded constants. Replace everything with design tokens from our UI Kit. If a required token is missing, add a new one to styles.scss."`
- `"Run the linter across the entire project and report critical and major errors. Propose a plan for fixing them."`
- `"Review the project for new visual components that are not part of the UI Kit."`

In the latter case, as the architect, I would decide whether to extract this new component into the shared UI Kit library or leave it local to the domain if it was too specific.

### The Architectural Cage: The Linter as Overseer

It's time to talk more about the mechanism itself.

>***The core mechanism*** *that physically kept the project from falling apart was the linter rules in the NX Monorepo. This was our "Architectural Cage."*

In Claude's settings, we established a rigid hook: he must run a lint check across the entire project after major changes. In the root configuration file (`eslint.config.mjs`), we defined strict dependency directions using a tagging system. Each library was marked with its own tags upon creation:

* `type:shell`
* `type:domain.business`
* `type:domain.service`
* `type:shared`
* `type:core`

**The rule was ironclad: dependencies can only go from top to bottom.** For example, `domain.business` could import `shared` or `core`, but two different business domains could not import each other (only through a `domain.service`).

Due to context window limitations, Claude often got "lazy." When generating a new library at my request, he might forget to add the correct tags in `project.json`. Therefore, my regular grooming sessions included mandatory steps:

1. Check if all new libraries were correctly tagged.
2. Run the linter.
3. Force Claude to rewrite imports or move common logic to `shared` if the linter flagged an architectural boundary violation.

This was my primary task—monitoring the cleanliness of the application's internal world and maintaining the framework. I was extremely satisfied with how this combination worked.

It was at this stage that we finally arrived at the ideal distribution of roles in our tandem: one of us "piled on code" on a broad front, generating business features, while the second (me) followed behind, continuously cleaning, refactoring, and maintaining the architectural boundaries.

How this team dynamic worked in practice and where the line of responsibility was drawn between the engineer and the AI-Orchestrator—all in the next chapter.

## 10. The "Centaur" Team: A New Anthropology of Development

| Parameter | Analysis (Stage: Team Dynamics) |
| :--- | :--- |
| **Approach** | **Team Compression** |
| **Structure** | **Architect + Product Owner + AI-Orchestrator** |
| **Efficiency** | **1 Architect ≈ 5–7 Linear Developers** |
| **Status** | **A New Standard for Startups** |

### Starting Point:
* **Infrastructure ready:** We have the "Architectural Cage," a polished `CLAUDE.md`, and an industrial code generation flow.
* **The Human Factor:** AI doesn't work in a vacuum. We needed to figure out how to divide tasks between two people so that development wouldn't turn into an endless battle with merge conflicts.
* **Expertise Deficit:** The project started without the active involvement of a designer (all designs were done beforehand), which required us to have "visual orchestration" skills.

### The Construction Metaphor: Walls, Engineering, and Finishing

There were two of us on this project. If we use a construction metaphor, our division of labor looked like this: 
My colleague primarily **"raised the walls"** (piling on the bulk of the business code), while I handled the **"structural engineering"** (designing the frame, setting the "cage," and ensuring the building didn't collapse). We both handled the **"finishing"** (polishing the UI and logic to a final shine).

### Team Compression: How to Compress a Department into Two People

The main conclusion of our experience: the classical development hierarchy is dying. Where a department of 5–7 people was previously required, two "centaurs" who know how to properly ride the AI are now sufficient.

We identified the key roles (expertise) necessary for an AI-Native project:

1. **AI System Architect / Orchestrator (My role):**
   * Designs architectural boundaries and sets up the "cage."
   * Creates and updates system prompts (`CLAUDE.md`).
   * Performs architectural grooming and resolves complex technical conflicts.

2. **Product Owner / Domain Expert (Colleague's role):**
   * Formulates business logic and the rules of the game.
   * Validates functionality for compliance with product requirements.
   * Acts as the "client" for the AI agents.

3. **Centaur-Developer (AI-Assisted Engineer):**
   
   We both performed this role. This is an engineer who doesn't just "push a button" but deeply understands the platform (Angular/Ionic).

     >***The Centaur Rule:*** *Letting the AI go solo without understanding what it's coding is a recipe for disaster. An engineer must be able to read and validate every single line generated by the model.*

4. **UI/UX Designer:**

   A critical role at the start.
   >***AI-Orchestrator Tip:*** *AI is a magnificent executor but a terrible UX inventor. For success, a high-quality Figma base is mandatory at the input stage. Without it, the AI will start assembling a "Frankenstein," trying to guess the designer's intentions.*

### AI-Native Roadmap: The Efficiency Conveyor

To avoid interfering with each other or the AI, we built the following process:

**Phase 1: Foundation (Preparation)**
* **Functional Requirements:** Clear definition of logic (Product Owner).
* **Design:** Complete rendering of the UI Kit and screens in Figma (Designer).
* **Architectural Scaffolding:** Creating the Nx project and PoC (Architect).
* **Setting the Cage:** "Flashing" rules into `CLAUDE.md` and configuring linters (Architect/Orchestrator).

**Phase 2: Population (Implementation)**
* **Core & Shared:** Creating the base API client and shared libraries.
* **Domain Isolation:** The key moment. We divided the application into non-overlapping functional domains. This allowed us to work in parallel: AI agents in an Nx monorepo are very sensitive to changes in the same files. Different domains = zero conflicts.
* **Grooming and Tests:** Constant code cleanup and verification (in our case, testing was manual, but ideally, there should be an automated Test Framework here).

**Phase 3: Finalization**
* **Integration:** Connecting domains into a unified Shell architecture.
* **Polishing:** Final UI tweaks and bug fixes.

### Summary
>***Conclusion:*** *The number of people on a team now depends not on the volume of code (code is now a cheap resource), but on the number of **non-overlapping business domains** and the complexity of the architecture.*

For a startup of our level, one contract AI-Architect can replace an entire outsourced team, provided the client has a clear design and an understanding of the product.

## 11. The Price of Speed and Unexplored Territories: A Retrospective

| Parameter | Analysis (Stage: Retrospective) |
| :--- | :--- |
| **Approach** | **Workflow Evolution** |
| **Labor Costs** | **~200 man-hours for 100+ screens** |
| **Speed** | **Stable 10x** |
| **Status** | **Production Ready** |

### Starting Point:
* **Result:** The application is completely rewritten and launched. 
* **Costs:** The entire process took 2 months of "evenings and weekends" with four hands (totaling roughly 160-240 man-hours).
* **Chapter Goal:** An honest breakdown of how our approaches changed during the race and where we see growth areas.

When you put development on AI rails, the process is constantly evolving. What seemed ideal at the start transforms under the pressure of reality and deadlines.

### 1. Conscious Decision to Skip Automated Tests
We sacrificed automated testing (Unit/E2E) in favor of Time-to-Market. Within the scale of our startup at this stage, it was effective: manual testing plus rigid architectural grooming saved us from fatal regressions. However, for large projects, integrating AI-Driven Testing (where the AI writes tests for its own features) is a mandatory step for the next iteration.

### 2. Tooling Evolution: From MCP to Smart Injection
In previous chapters, I mentioned how MCP servers for Figma and Chrome became our "eyes" and "hands." This is the absolute truth—without them, we wouldn't have been able to "flash" Claude at the start. But at some point, our approach became more segmented.

It is important to understand that AI orchestration is not a dogma, but a search for the shortest path to the result:
* **PO's Role (Business Features):** My colleague continued to actively use Figma MCP. For him, it was the shortest path to "piling on" functionality according to ready-made layouts.
* **Architect's Role (Debugging and Structure):** Over time, I began to use MCP more selectively. In the daily routine of UI layout, it was often faster for me to simply drop a screenshot or copy-paste the DOM tree into the chat—this provided instant context without the delays that the MCP protocol had at that time.
* **Surgical Debugging:** Meanwhile, the Chrome DevTools MCP remained indispensable in "hopeless" situations. When an error wasn't caught by conventional analysis, I gave Claude control over debugging. It’s funny, though, that sometimes during such "collaborative debugging," I found the cause faster myself, simply by analyzing the DOM model while Claude was building hypotheses.

We didn't "abandon" tools—we shifted to a **Smart Injection** mode. Automation where it accelerates, and manual context where it is more effective.

### 3. Asynchronous Development and Limits
Working with LLMs requires a change in rhythm. We often launched heavy generations overnight and checked the results in the morning. We learned to manage not only our own time but also context windows and token quotas, which became a separate skill in our "centaur-stack."

>***AI-Orchestrator Tip:*** *Leave bulky tasks for the AI overnight so you can start verification and grooming in the morning. This is the best way to bypass limits and maintain personal productivity.*

### 4. Growth Areas: AI Reviewers and Architectural Oversight
The main thing I missed was fully autonomous AI agents at the CI/CD stage. Ideally, checking hardcodes, design tokens, and layer boundaries should be handled by a dedicated "Senior Reviewer" agent for each commit. This would make our "Architectural Cage" even more impenetrable.

### Summary
Our "Centaur + Rigid Architecture" configuration provided the maximum ROI. We proved the main point: ~200 hours for a highly complex web application is the new reality of engineering. We didn't just use AI; we learned to change tools and approaches in time to avoid losing pace in this mad race.

## Epilogue: The Bottleneck Shift and the Tech Race

While I was writing, editing, and gathering material for this article, the industry has not stood still. Just the other day, new model releases came out: an updated Opus 4.7, announcements of closed security models, and powerful design generators that turn text into prototypes in seconds. Reading the news, it might seem that in another few months, AI will be assembling complete businesses from a single prompt from a Product Owner.

But that is an illusion. By rewriting a complex web application of 100+ screens in ~200 man-hours, we didn’t just save a budget. We confirmed, in practice, a fundamental shift in the development paradigm.

The era where a startup's main resource was hundreds of hours of linear coders is truly over.

>***Architectural Axiom:*** *Code generation today is a cheap, fast, and virtually infinite resource. AI is the gas pedal. The new "bottleneck" is the steering wheel—**orchestration and macro-architecture.***

If you let even the most brilliant model sail freely without a rigid "Architectural Cage" (DDD, layered architecture, a strict UI Kit), it will produce not a scalable product, but uncontrolled chaos. It will simply write bad legacy code a thousand times faster than a human.

>***Architectural Prediction:*** *Technologies, token limits, and LLM versions will change every month, but the* ***rules of engineering will not.***

Today, launching a complex product doesn't require a massive team. You need 1–2 AI Architects capable of designing a rock-solid foundation and managing the generation flow, adapting any new neural networks to their system.

We have proven that 200 hours for an Enterprise-grade application is no longer a fantasy. It is the new reality of engineering. And the winner is the one who knows how to tame this chaos: to build fast, clean, and within the right boundaries.

---

*If you are looking for a way to build a reliable AI-Native pipeline in your project, or if you need a Fractional AI-Architect to design your own "Architectural Cage"—I'm open for discussion.*

*NXS8 ([core@nxs8.net](mailto:core@nxs8.net)), 2026*
