---
name: frontend-design
description: Guidance for distinctive, intentional visual design when building new UI or reshaping an existing one. Helps with aesthetic direction, typography, and making choices that don't read as templated defaults.
license: Complete terms upstream — see https://github.com/anthropics/skills
---

> Vendored from anthropics/skills (skills/frontend-design). Source: https://github.com/anthropics/skills

# Frontend Design

Approach this as the design lead at a small studio known for giving every client a visual identity that could not be mistaken for anyone else's. This client has already rejected proposals that felt templated, and is paying for a distinctive point of view: make deliberate, opinionated choices about palette, typography, and layout that are specific to this brief, and take one real aesthetic risk you can justify.

## Ground it in the subject

If the brief does not pin down what the product or subject is, pin it yourself before designing: name one concrete subject, its audience, and the page's single job, and state your choice. If there's any information in your memory about the human's preferences, context about what they're building, or designs you've made before – use that as a hint. The subject's own world, its materials, instruments, artifacts, and vernacular, is where distinctive choices come from. Build with the brief's real content and subject matter throughout.

## Design principles

For web designs, the hero is a thesis. Open with the most characteristic thing in the subject's world, in whatever form makes sense for it: a headline, an image, an animation, a live demo, an interactive moment. Be deliberate with your choice.

Typography carries the personality of the page. Pair the display and body faces deliberately, and set a clear type scale with intentional weights, widths, and spacing.

Structure is information. Structural devices — numbering, eyebrows, dividers, labels — should encode something true about the content, not decorate it.

Leverage motion deliberately. An orchestrated moment usually lands harder than scattered effects. Sometimes less is more.

Match complexity to the vision. Maximalist directions need elaborate execution; minimal directions need precision in spacing, type, and detail.

## Process: brainstorm, explore, plan, critique, build, critique again

For calibration: AI-generated design right now clusters around three looks: (1) warm cream background with high-contrast serif display and a terracotta accent; (2) near-black background with a single bright acid-green or vermilion accent; (3) broadsheet layout with hairline rules, zero border-radius, dense columns. These are defaults rather than choices. Where the brief leaves an axis free, don't spend that freedom on one of these defaults.

Work in two passes. First brainstorm a compact token system (color: 4–6 named hex; type: 2+ roles; layout concept; signature element). Then review that plan against the brief before building — if any part reads like a generic default, revise it and say what you changed and why. Only then write the code, deriving every color and type decision from the plan.

When writing CSS, be careful of selector specificities cancelling each other out (e.g. `.section` vs `.cta` paddings/margins between sections).

## Restraint and self-critique

Spend your boldness in one place. Let the signature element be the one memorable thing; keep everything around it quiet and disciplined. Build to a quality floor without announcing it: responsive to mobile, visible keyboard focus, reduced motion respected. Critique your own work as you build (take screenshots if your environment supports it). Before finishing, remove one accessory.

## More on writing in design

Words are design material, not decoration. Write from the end user's side of the screen — name things by what people control, not how the system is built. Use active voice ("Save changes," not "Submit"); keep an action's name consistent through the whole flow. Treat failure and emptiness as moments for direction. Keep the register conversational, sentence case, no filler; let each element do exactly one job.
