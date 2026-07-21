<p align="center"><img src="cover.svg" alt="Agent Ops Doctrines" width="100%"></p>

# Agent Ops Doctrines

**A field-tested discipline framework for AI agents that touch production systems.**

Created by **Yacine Mahboub**, Founder of WEVIA.

---

## Why this exists

Most agent frameworks tell you how to make an agent *act*. Almost none tell you how to stop it from quietly destroying something at 3 a.m.

This repository is not a framework, a library, or a product. It is a set of **operational doctrines** — hard rules, refined through real incidents, that govern how an autonomous agent is allowed to operate on live infrastructure.

They were written the expensive way: by running agents against production for months, and writing down the rule every time something broke.

## Who this is for

- Teams putting LLM agents in front of real systems (infrastructure, ERP, databases, CI/CD)
- CTOs and engineering leads who need an answer to *"what stops it from doing something stupid?"*
- Anyone who has watched an agent report success on work it never did

## The doctrines

| # | Doctrine | One-line rule |
|---|---|---|
| 0 | **Separation of powers** | The human validates. The agent executes. Never the same actor. |
| 1 | **Backup before mutation** | No modification without a restorable copy made first. |
| 2 | **Zero regression** | A change that breaks an existing passing test is not a change, it is an incident. |
| 3 | **Honesty over fluency** | Absent is absent. A missing result is reported as missing, never fabricated. |
| 4 | **Sandbox before production** | Nothing reaches production that has not been validated in an isolated copy first. |
| 5 | **Verify, do not assume** | A command that returned 0 is not proof the intent was achieved. Check the actual state. |
| 6 | **Bounded blast radius** | Mass operations are forbidden by default. Small batches, each verified. |
| 7 | **Repair, never delete** | When something is broken, fix it. Deleting an application is not a repair. |

Full text with rationale and failure modes: **[DOCTRINES.md](DOCTRINES.md)**

## The part most people skip

Doctrines are worthless if nobody defines who writes them, who approves them, and what happens when one is violated. See **[GOVERNANCE.md](GOVERNANCE.md)**.

## Lessons from real incidents

Each doctrine exists because something broke. The anonymized post-mortems are in **[lessons/](lessons/)** — including the one where deleting a public repository did not remove the leaked credentials.

## Using these

Copy them. Adapt them. Argue with them. They are deliberately written to be transposable to any stack, because none of them depend on a specific tool.

If you adapt them, attribution is appreciated but not required.

## License

Apache-2.0 — see [LICENSE](LICENSE).

---

*Maintained by [WEVIA](https://weval-consulting.com) — sovereign AI platform engineering.*
