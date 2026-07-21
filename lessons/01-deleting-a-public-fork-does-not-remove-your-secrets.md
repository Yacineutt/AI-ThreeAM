# Lesson 01 — Deleting a public fork does not remove your secrets

**Doctrines involved:** 5 (verify, do not assume), 3 (honesty)

## What happened

An organization kept every repository private except one: a fork of a well-known open-source project, created to carry a handful of internal customizations.

Those customizations included a small relay script. The script contained an admin key and the URL of an internal endpoint that executes commands on production hosts.

The fork had been public for months.

## The first wrong assumption

The obvious fix was to flip the fork to private. The platform refused: **a fork of a public repository cannot be made private.** This is a documented constraint, not a permissions problem.

## The second wrong assumption

The fallback was to delete the fork entirely. That worked — the repository page returned 404, and so did the raw file URL.

At that point the incident looked closed. It was not.

## What was actually still true

Commits pushed to a fork are stored in the **fork network** of the upstream repository. They remain reachable through the *parent* repository, by commit SHA, **after the fork is deleted**.

Verification, run after deletion:

```
GET https://raw.githubusercontent.com/<upstream-owner>/<repo>/<our-commit-sha>/<path>
-> 200 OK, full file content, key included
```

Deleting the fork removed the convenient path to the secret. It did not remove the secret.

## What this costs

The exposure window is not "until deletion". It is **permanent**, unless the platform's support team is asked to purge the fork network — and even then, anyone who cloned or indexed it in the meantime still has it.

## The rules that came out of it

1. **Never commit an infrastructure credential, not even into a fork you consider throwaway.** A fork is a public repository with a misleading name.
2. **After any remediation, verify the actual reachability of the artifact**, not the reachability of the page that used to show it. A 404 on the repository is not a 404 on the content.
3. **Treat any credential that has ever been public as compromised.** Rotation is the only true remediation; deletion alone is theatre.
4. **Audit fork visibility separately from repository visibility.** A visibility audit that counts "public repositories" will happily report a clean bill of health while a fork leaks.

## The uncomfortable part

The exposure was found *while* performing an unrelated routine task — making all repositories private. Nobody was looking for it. There was no alert, no scanner, and no reason to suspect it.

Assume the same is true of yours until you have checked.
