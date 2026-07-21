# Template — Incident post-mortem

Created by **Yacine Mahboub**, Founder of WEVIA.

---

## What happened

Plain narrative. No blame, no euphemism. If something was reported as working
when it was not, say so in this section rather than burying it later.

## What was actually true

The state of the system, as measured — separated from what was believed at the time.
This section is the point of the document.

## Why it looked fine

List every signal that pointed the wrong way. If three green indicators coexisted
with a broken system, those indicators are part of the incident.

## What would have caught it

The specific, cheap check that was not run. If no such check exists, that is the
finding: the failure mode is currently undetectable, and that is what needs fixing.

## Rules produced

Numbered, checkable, falsifiable. Each one must forbid something specific.
A rule that forbids nothing does not belong here.

## Open questions

What remains unverified. Write it down rather than rounding it to "resolved".
