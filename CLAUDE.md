# CLAUDE.md

All tasks in this project must follow the five-phase workflow below. Each phase ends with a clear **completion criterion (gate)** — if a gate is not met, do not proceed to the next phase.

```
Task request → ① Grilling → ② User searches first → ③ Plan confirmation → ④ karpathy-guidelines implementation → ⑤ Verification passed
```

---

## ① Grilling: interrogate the requirements before touching anything

On receiving any task request, the **first step** is to invoke the `mattpocock-skills:grilling` skill and interrogate the requirements like a cross-examination: what is the goal, what counts as success, where are the boundaries, what assumptions and risks exist.

- Never skip this because "the request looks simple."
- You may only skip if the user explicitly says "skip grilling" — record the reason in the conversation.

**Gate**: The user's answers have clarified the goal, the success criteria, and the boundaries. Keep asking if the user cannot answer; never decide on their behalf.

## ② User searches first, then Claude advises

Before giving any technical advice, require the user to **search the browser themselves**: technology choices, alternatives, known pitfalls, up-to-date sources.

- Claude must not perform this research in the user's place — the user's findings are an input to the decision and cannot be delegated.
- Only after the user shares their findings (key points + source links) may the next phase begin.

**Gate**: The user has provided their search findings, or explicitly declared "skip searching, proceed to the next step."

## ③ Plan confirmation (including the verification plan)

Based on the user's findings, Claude gives its recommendation: the proposed approach, trade-offs, and risks, then discusses with the user.

- The recommendation must respond to the user's findings; if it contradicts something the user found, explain why.
- This phase discusses the plan only — no implementation code.
- The plan must include a **verification plan**: how to verify (commands/tests/run steps), the success criteria (what result counts as passing), and who verifies (Claude runs automatically / user verifies manually).

**Gate**: The user has explicitly confirmed the plan (in writing), and has seen the verification plan.

## ④ Implementation: follow karpathy-guidelines

Once the plan is confirmed, invoke the `karpathy-guidelines` skill and implement per all of its principles: avoid overcomplication, make surgical changes, surface assumptions proactively, define verifiable success criteria.

**Gate**: Implementation is complete and self-checked against karpathy-guidelines — minimal changes, no over-engineering, assumptions noted in the implementation.

## ⑤ Verification: must actually pass before the next step

Execute the verification plan agreed in phase ③ and report the results truthfully.

- The verification plan (agreed in ③) decides who verifies: Claude runs automatically, or the user verifies manually.
- If the user chooses to verify themselves: Claude's job is to hand over a **concrete verification method** — exact steps/commands, what to look at, and the success criteria (what result counts as passing). Claude must not execute it on the user's behalf, and must not claim it passed.
- Verification fails → return to ④, fix, verify again; loop until it passes.
- If verification was not run or did not pass → do not proceed to the "next step" (new features, commits, and wrap-up reports all count as next steps).
- Report truthfully: never claim a verification "passed" that was not run (whether by Claude or by the user); attach the failure output for failed verifications.

**Gate**: The verification plan was actually executed, the results meet the success criteria, and the report is based on real evidence.
