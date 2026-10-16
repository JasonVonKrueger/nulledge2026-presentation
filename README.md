# nullEDGE 2026: Stop Bad Code at the Door

Companion repository for my **nullEDGE 2026** presentation on using ServiceNow **Instance Scan** as a quality gate for development. It contains the update sets, checks and documentation shown in the talk, so you can install them, try them, or borrow ideas for your own instance.

**What it does:** when a developer finishes an update set and clicks **Scan & Complete**, the platform runs the Enterprise Coding Standard (ECS) checks against everything in that update set:

- Problems of priority 1–2 block completion. Each finding says what is wrong, on which line, and links to an article explaining the fix.
- Lower-priority findings go to a peer reviewer.
- A clean update set gets a peer review task. Once the review passes, the update set can be deployed to the test instance.

Built and validated on **Zurich**, in the **global** scope. Install it on a non-production instance first.

- Slide deck: [deck/Enterprise Coding Standard.pptx](deck/Enterprise%20Coding%20Standard.pptx)
