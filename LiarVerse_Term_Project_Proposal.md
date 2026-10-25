# LiarVerse: An AI-Powered Multi-Agent Social Deduction Game

**Term Project Proposal — Open Source Software**  
**Project Type:** Individual software development project  
**Target Completion:** Mid-December 2026  
**Planned Deliverable:** Open-source web application

## 1. Project Overview

LiarVerse is a browser-based social deduction game in which human players compete and collaborate with autonomous AI agents powered by a locally hosted large language model (LLM). The project adapts the familiar Liar Game format: most participants know a secret word, while one participant—the liar—must infer it without being directly informed. Players provide indirect hints, discuss suspicious behavior, and vote to identify the liar. Depending on the outcome, the liar may attempt to guess the secret word.

The primary objective is **to build and deploy an engaging open-source software application**, rather than to develop a new language model. LiarVerse treats LLMs as active participants with individual identities, roles, and conversational memory. This allows a human player to enjoy a multiplayer-style social deduction experience even when other human participants are unavailable.

## 2. Motivation and Problem Statement

Social deduction games are engaging because players must interpret ambiguous clues, defend their statements, and make decisions under uncertainty. However, traditional multiplayer games often require several people to be available simultaneously. AI agents offer an opportunity to fill vacant player slots while introducing dynamic and unpredictable interactions.

Building such a system presents challenges beyond simply calling an LLM. AI participants must respect private role information, avoid revealing the secret word, reference actual game events, and respond within a reasonable time. Unstructured or repetitive model outputs can disrupt gameplay. LiarVerse therefore combines generative AI with conventional software engineering techniques, including authoritative server-side state management, structured output validation, and real-time communication.

## 3. Project Objectives

The project has five principal objectives:

1. **Playable web experience:** Implement rooms, player management, role assignment, hint submission, discussion, voting, and game resolution.
2. **Autonomous AI players:** Enable agents to generate hints, participate in discussions, and make role-dependent decisions.
3. **Local LLM integration:** Use Ollama-hosted inference rather than requiring paid external model APIs.
4. **Reliable game behavior:** Detect invalid outputs, reduce repetition, and prevent direct disclosure of protected information.
5. **Open-source delivery:** Publish a maintainable codebase with installation instructions, testing guidance, and a working demonstration.

## 4. Core Features and Gameplay

### 4.1 Gameplay Flow

A user creates a game room and adds human or AI participants. When the game begins, citizens receive the secret word, while the liar does not. The game proceeds through four phases:

- **Hint phase:** Each participant submits a clue related to the word without naming it.
- **Discussion phase:** Players interpret clues, ask questions, and challenge suspicious statements.
- **Voting phase:** Players vote for the person they believe is the liar.
- **Resolution phase:** The outcome is revealed; the liar may receive a final opportunity to guess the word.

### 4.2 AI Agents

Each AI participant is represented by an agent with its own identity, role, and memory. Role-aware context construction ensures that the liar is not explicitly given the secret word. Agents produce structured outputs that the backend can interpret consistently.

A planned enhancement is **strategic discussion**. Instead of only reacting to the previous message, an agent will evaluate evidence from actual hints and discussion history, select an action (question, accusation, defense, or counterargument), and then generate a corresponding response. Voting decisions will likewise be grounded in observable game events.

### 4.3 Validation and Game Integrity

LLMs can violate rules even when prompted carefully. The backend will therefore validate generated messages independently. Checks will include secret-word and known-alias disclosure, repeated messages, malformed responses, and unsuitable hints. This design keeps the server—not the model—as the authority on game rules. Because synonym detection cannot be guaranteed by a simple word list, the project will prioritize robust coverage of its supported word catalog and test known failure cases.

## 5. System Architecture and Technology Stack

LiarVerse uses a client–server architecture with a separate local inference service.

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend | React, Vite | Browser UI, room interactions, game screens |
| Backend | Python, FastAPI | Game logic, room state, agent orchestration, REST endpoints |
| Real-time messaging | WebSocket | Live discussion and state updates |
| Model serving | Ollama, Qwen3 8B (baseline) | Local AI text generation |
| Agent modules | Python | Identity, memory, role-specific prompts, validation |
| Version control | Git, GitHub | Source management, documentation, public release |

The FastAPI backend maintains authoritative game state and controls which information is passed to each agent. Ollama handles local inference, while the frontend displays game events and receives updates through HTTP and WebSocket communication. This separation also makes it possible to replace the underlying model without redesigning the game interface.

Development currently uses a GPU-equipped local server. A larger quantized model may be tested if it improves output quality without making gameplay unacceptably slow. Fine-tuning is considered an optional extension, not a requirement for the final application.

## 6. Current Progress and Development Schedule

A functional prototype already includes room and player management, a React interface, FastAPI APIs, local LLM integration, AI hint generation, and autonomous AI discussion. Agent identity and memory components have been introduced. Early tests confirm that agents can generate structured discussion messages and that some secret-word disclosures and repeated messages are rejected.

The prototype also reveals issues to address before release: hint evaluation occasionally accepts overly explicit or unnatural clues, and AI discussion can become repetitive or rely on unsupported interpretations. These observations inform the following plan.

| Period | Planned Work | Target Milestone |
|---|---|---|
| Late October | Improve hint validation; compare model quality and latency | More reliable hint generation |
| Early November | Implement evidence-aware strategic discussion | More coherent agent interactions |
| Mid-November | Refine voting, game-state consistency, and error handling | Stable end-to-end gameplay |
| Late November | Improve UI, game pacing, and multiplayer experience | User-facing playable build |
| Early December | Run scenario tests, fix bugs, document deployment | Release candidate |
| Mid-December | Publish repository and demonstrate the application | Final course submission |

A complete, stable game loop will take priority over optional AI experiments if time becomes limited.

## 7. Evaluation Plan

The project will be assessed primarily as a software product. Functional tests will verify game-state transitions, role assignment, hint submission, discussion, voting, and resolution. AI-specific tests will measure the rate of rejected messages, secret-word leakage, repeated statements, and references to events that did not occur. Runtime measurements will examine response latency and its impact on game pacing.

Informal user playtests will evaluate clarity of instructions, ease of interaction, perceived fairness, and entertainment value. Where feasible, identical game scenarios will be run using the baseline model and a larger local model to determine whether improved generation quality justifies increased latency. These comparisons will guide implementation choices rather than serve as the project's main research contribution.

## 8. Expected Outcomes and Significance

The final submission will include a playable web application, a public GitHub repository, setup and execution instructions, an architecture overview, and a demonstration of human–AI gameplay. An appropriate open-source license will be selected before release.

LiarVerse illustrates a practical application of generative AI in interactive software: LLM-based agents participate in a shared environment, while deterministic backend logic enforces rules and preserves consistency. The expected outcome is a creative and accessible social deduction game that demonstrates full-stack development, local AI integration, multi-agent orchestration, and open-source software practices.
