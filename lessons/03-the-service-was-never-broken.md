# Lesson 03 — The service was never broken, the check was

**Doctrines involved:** 5 (verify, do not assume), 7 (repair, never delete)

## What happened

A containerized service had been reporting `unhealthy` for two months. It was scheduled for a restart, and if that failed, redeployment.

Both would have been the wrong action.

## What was actually true

The service was running perfectly. Its process had been up continuously since deployment, with a restart count of zero, answering requests normally.

The health check shipped with the container image probed a **hardcoded default port**. This particular deployment had been configured to listen on a different port through an environment variable. The check was querying a port nothing was bound to, and had been failing for exactly as long as the deployment had existed.

## The trap

Restarting would have fixed nothing and risked an outage on a healthy service. Redeploying would have destroyed a working configuration to solve a reporting artifact.

Both actions are the reflexive response to `unhealthy`. Both are destructive. Neither addresses the cause.

## What actually diagnosed it

```
# What does the check actually run?
inspect the container's health check definition

# What port is the service actually configured for?
read the environment variable, do not assume the default

# Does the service answer on that port, from inside the container?
probe it directly
```

The last one returned a clean success — which proves the service is healthy and the check is wrong, in a single command.

## The fix

The health check script was patched to read the configured port instead of the hardcoded default, falling back to the default when unset. Applied in place, without restarting the container, without touching any data.

Status flipped to healthy within one check interval. Total downtime: zero.

## The rules that came out of it

1. **`unhealthy` is a claim about the check, not only about the service.** Test the service independently before acting on the check.
2. **Never restart to fix a status you have not diagnosed.** Restarting destroys the evidence and sometimes the service.
3. **Prefer the fix that requires no restart.** If a config file inside a container can be corrected in place, that is strictly safer than a redeploy.
4. **A long-standing `unhealthy` that never caused a user-visible problem is a strong signal that the check is wrong** — a genuinely broken service produces other symptoms.
