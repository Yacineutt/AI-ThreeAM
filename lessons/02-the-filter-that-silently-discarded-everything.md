# Lesson 02 — The filter that restarted cleanly and threw everything away

**Doctrines involved:** 5 (verify, do not assume), 3 (honesty)

## What happened

A logging daemon was flooding the system log, consuming gigabytes per day. The fix was routine: add a rule routing that program's output to a dedicated file, and stop it from reaching the main log.

The configuration validated. The service restarted without error. The flood stopped.

Reported as fixed. It was not.

## What was actually happening

The logging daemon runs as its own unprivileged user. The destination directory belonged to a different user, with no write permission for others.

So the rule did exactly half its job: it **stopped** the messages from reaching the main log, and **failed** to write them to the destination. The messages were discarded. Every one of them.

Worse, the daemon then logged its own write failures — into the main log — at a rate that partially replaced the flood it had just removed.

## Why it looked like success

Three signals all pointed the wrong way:

- Configuration validation passed. It checks syntax, not permissions.
- The service restarted with a clean status.
- The original spam disappeared from the log, which is exactly what "fixed" looks like.

Nothing in the normal success path distinguishes *routed correctly* from *discarded silently*.

## What would have caught it in ten seconds

```
# Does the destination file actually exist and grow?
ls -l <destination>

# Did the daemon report suspended output actions after the restart?
grep -c 'suspended' <main-log>   # expected: 0
```

Both checks were run — but only after a later, unrelated inspection raised suspicion. Had they been run at the moment of the fix, the false "fixed" report would never have been issued.

## The rules that came out of it

1. **A clean restart is not evidence that a rule works.** Verify the output artifact exists and is growing.
2. **When adding a routing rule with a terminal action, confirm the destination is writable by the daemon's own user**, not by the operator's.
3. **Check for the daemon's own error messages after the change.** A component complaining about itself is the cheapest available signal.
4. **When a previous report turns out to be wrong, correct it explicitly.** In this case the same misconfiguration was found on two other pre-existing rules, discarding their output too — undetected for months, precisely because nobody had gone back to check.
