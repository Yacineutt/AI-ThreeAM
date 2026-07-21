# Governance

A doctrine that nobody owns is a suggestion. This document defines ownership.

Created by **Yacine Mahboub**, Founder of WEVIA.

## Roles

| Role | Can propose | Can approve | Can execute |
|---|---|---|---|
| **Validator** (human) | yes | **yes — exclusively** | no |
| **Operator** (agent) | yes | no | yes |

The separation is the point. An actor that can both approve and execute has no meaningful control over itself.

## Lifecycle of a doctrine

1. **Incident** — something breaks, or nearly does.
2. **Post-mortem** — the causal chain is written down, without blame and without euphemism.
3. **Proposal** — a rule is drafted that would have prevented it. It must be checkable, not aspirational. "Be careful" is not a doctrine.
4. **Validation** — the human validator approves, amends, or rejects.
5. **Enforcement** — the rule enters the operator's standing instructions.
6. **Review** — a doctrine that has never triggered in six months is either perfect or dead. Find out which.

## Violation handling

A violated doctrine is an incident in itself, and enters at step 1.

Two failure modes matter more than the violation:

- **Silent violation** — the rule was broken and nobody noticed. This means the doctrine is unverifiable and must be rewritten to be checkable.
- **Retroactive justification** — the operator explains why the rule did not apply *this time*. This is the most dangerous pattern in agent operations, because it is always locally reasonable and globally fatal.

## Writing a good doctrine

- **Checkable.** Someone other than the author must be able to determine, mechanically, whether it was followed.
- **Falsifiable.** It must forbid something specific. A doctrine that forbids nothing is decoration.
- **Small.** One rule, one failure mode.
- **Explained.** Include the incident that produced it. Rules without stories get discarded under pressure.
