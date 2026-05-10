# Case 001 - Defense Drill

## Objective

This drill supports English writing and speaking practice for explaining Case 001 in a SOC analyst interview context.

The goal is to describe the SSH failed-login investigation using short, clear, and controlled analyst sentences.

## Core Sentence Pattern

```text
I worked on...
I reviewed...
The logs showed...
This pattern may indicate...
The next step is...
```

## Model Answer

```text
I worked on an SSH authentication investigation.

I reviewed Linux journal logs.

The logs showed repeated failed SSH login attempts from the same source IP.

This pattern may indicate brute-force or username-guessing activity.

The next step is to verify whether the successful logins were expected and authorized.
```

## Key Analyst Phrases

- SSH authentication investigation
- Linux journal logs
- repeated failed SSH login attempts
- same source IP
- brute-force activity
- username-guessing activity
- successful SSH logins
- expected and authorized
- reviewed further
- next step

## Notes

The answer should stay short and controlled.

The investigation should not be described as a Wazuh alert investigation because Wazuh was not installed on the current VM during this phase.

The safest analyst language is:

```text
This pattern may indicate...
The activity should be reviewed...
The next step is to verify whether...
```

## Final Interview Version

```text
I worked on an SSH authentication investigation in my home SOC lab.

I reviewed Linux journal logs and identified repeated failed SSH login attempts from the same source IP.

The attempts targeted invalid usernames such as fakeuser and Fakeuser.

This pattern may indicate brute-force or username-guessing activity.

I also checked for successful SSH logins using the Accepted password filter.

The next step is to verify whether the successful logins were expected and authorized.
```
