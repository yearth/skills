---
name: brainstorming
description: "Explore ideas, clarify ambiguous requirements, and compare design approaches before implementation when meaningful choices remain. Use for brainstorming requests and features with unresolved design trade-offs; clear small changes, bug fixes, and execution of an agreed plan do not require this workflow."
---

# Brainstorming Ideas Into Designs

Help the user turn an idea into a clear, practical direction through natural dialogue. Scale discussion to the uncertainty and consequences of the decisions, not a fixed task category.

## Understand and Explore

- Establish the goal, relevant constraints, and what success means. For existing projects, inspect enough context to ground the discussion in the actual system and its conventions.
- Ask only questions whose answers could materially change the approach. Prioritize the most consequential uncertainty; closely related questions may be grouped. When an assumption is reasonable and reversible, state it and continue.
- Compare alternatives when there are real trade-offs. Recommend an approach and explain why; do not manufacture options to meet a quota.
- Keep the scope focused. Break up an oversized idea when that helps make progress, without requiring a separate formal process for every piece.
- Adjust the depth as evidence arrives. A complex-looking request may become simple after inspection; a newly discovered risk may need more discussion.

## Reach a Useful Decision

Summarize the intended outcome, recommended approach, scope, and any unresolved decisions at the level of detail the user needs. Include interfaces, data flow, failure handling, or validation only when they help assess the design.

Use the conversation as the default output. Create a design document only when the user requests one; there is no required spec, fixed document path, automatic commit, or separate document review gate.

Follow the user's existing authorization and applicable project approval rules. Do not ask for approval again merely because brainstorming was used. Pause when a consequential choice needs the user's decision, the scope would exceed their authorization, or an applicable rule requires approval.

If the user requested discussion only, finish with the conclusion and remaining questions. If implementation is already authorized and the important decisions are resolved, continue through the normal development workflow. Interest in an idea alone does not authorize implementation.

## Visual Companion

Keep the browser-based visual companion available for mockups, wireframes, layout comparisons, diagrams, and other questions that are easier to understand visually.

- Offer it when a concrete visual question arises, rather than automatically at the start of every discussion. Briefly explain what you would show and obtain consent before first use, unless the user has already requested or approved it.
- The offer can accompany relevant context; it does not need a separate message. After acceptance, use it when helpful without repeatedly asking to enable it.
- Read [visual-companion.md](visual-companion.md) before starting the companion. Resolve its script paths relative to this skill directory. Follow its operational instructions for starting the server, showing screens, reading events, and stopping the session, while respecting the environment's browser routing rules.
- The guide's conversational ceremony is optional: use the lightweight dialogue rules here instead of mandatory per-screen turn endings, repeated URL reminders, or approval after every section. Ask for feedback when an actual visual decision requires it.
- Choose visual or text presentation per question. Enabling the companion does not move all discussion into the browser.
- Use browser selections together with the user's messages. Resolve conflicting or ambiguous feedback before treating a visual choice as settled; visual selection does not grant unrelated implementation authorization.
