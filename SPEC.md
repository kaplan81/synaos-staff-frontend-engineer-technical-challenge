# Staff Frontend Engineer — Technical Challenge

## Format

- Take-home: Written design document (max 4-6 pages).
- Time: 1 week, spend no more than 4-6 hours.
- Follow-up: 60-minute discussion where the candidate presents and defends their approach.

## The Scenario

You are the Staff Frontend Engineer at an industrial automation company. Your platform manages fleets of mobile robots, human workers, and warehouse equipment on factory shopfloors. The frontend is a real-time operations dashboard used 24/7 by shopfloor supervisors and control room operators.

The CEO has just closed a deal with a major automotive manufacturer that will 5x the number of robots on a single shopfloor (from ~60 to ~300), introduce 3 new vehicle types the system has never managed before, and require a second concurrent shopfloor view for a supervisory control room in a different building.

The current frontend works — but it was designed for the original scale. The shopfloor map renders robot positions via WebGL at ~30-37 FPS under full load with 60 robots. State updates arrive via WebSocket several times per second. The frontend is a micro-frontend architecture (Module Federation) with an Angular-based monorepo, shared component libraries, and an NGXS state store.

You have 5 months until the customer go-live. Your engineering organization has 5 product teams, none of which are exclusively frontend teams — frontend work is distributed across all of them. There is no existing Staff Frontend Engineer; you are the first.

## The Ask

Write a design document that addresses the following:

1. Stakeholder & Persona Analysis

Who are the people affected by this initiative — both users of the product and people inside your engineering organization? What are their distinct needs, and where do those needs conflict? How does this analysis shape your technical priorities?

2. Technical Assessment

Given the constraints described, identify the top 3-5 technical risks to delivering this successfully. For each, explain why it's a risk, what the blast radius is if unaddressed, and how you would investigate or mitigate it. Be explicit about what you don't know and what you'd need to learn.

3. Architectural Approach

Propose a high-level architecture for handling the increased scale. You don't need to write code — we want to see how you think about:

- Rendering 300+ moving entities in real-time.
- State management under high-frequency updates.
- The relationship between frontend architecture and the backend data delivery model.
- What you would change vs. what you would deliberately leave alone and why.

4. Delivery & Organizational Strategy

You have 5 months, 5 teams, and no frontend platform team. How do you sequence the work? What do you deliver first and why? How do you drive cross-team alignment on architectural changes when you have no direct authority over any of the teams? What would you propose to engineering leadership regarding team structure or process changes?

5. Trade-offs & Business Framing

Every technical decision has a business consequence. Pick 2-3 of your key technical recommendations and frame them as you would present them to a VP of Product or a customer-facing stakeholder: What does this decision enable? What does it cost? What happens if we don't do it?

## What We're Evaluating

| **Dimension**           | **What great looks like**                                                                                                                                                                                |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Business acumen**     | Connects every technical recommendation to a user outcome or business constraint. Understands that "the right architecture" is the one that ships value within the timeline.                             |
| **Architectural depth** | Demonstrates real understanding of WebGL rendering pipelines, state management at scale, and real-time data flow — not just framework-level knowledge. Goes below the abstraction layer when it matters. |
| **Pragmatism**          | Makes hard choices. Says "we won't do X because Y matters more right now" rather than proposing an idealized system. Distinguishes between day-1 requirements and future improvements.                   |

| **Dimension**                | **What great looks like**                                                                                                                                                                       |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Organizational awareness** | Recognizes that the hardest part of a Staff role is driving change across teams without direct authority. Proposes realistic strategies for alignment, not just technical blueprints.           |
| **Communication**            | The document itself is the proof. Is it clear? Would a product manager understand the trade-off section? Would a senior engineer find the technical sections credible?                          |
| **Comfort with ambiguity**   | The scenario is intentionally underspecified. Strong candidates identify what's missing, state their assumptions explicitly, and explain how different assumptions would change their approach. |

## What We're NOT Evaluating

- Angular-specific API knowledge.
- Pixel-perfect implementation details.
- Whether you arrive at "our" answer (there isn't one).
- Length or visual polish of the document.

## Guidance for the Follow-up Discussion

In the 60-minute session, you'll walk us through your design (~20 min), then we'll challenge your assumptions, explore alternatives, and go deeper on specific areas (~40 min). We may introduce new constraints ("what if the timeline was 3 months instead?", "what if one of the vehicle types requires a 3D view?") to see how you adapt your thinking in real time.
