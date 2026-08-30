# Notes for AI Agents

## Git workflow

- Commit message style: `type: summary` as the first line (Conventional
  Commits, enforced by `.config/committed.toml`; keep the subject at or
  under 72 characters). A short body paragraph explaining non-obvious
  rationale is fine when it adds real context; don't pad a trivial change with
  one just to have one.

## Renovate `schedule` cron syntax is not standard cron

Renovate's `schedule` option (see `renovate.json`)
looks like 5-field cron but isn't interpreted the same way; this applies to
any platform Renovate runs on:

- The minutes field **must** be `*`: Renovate doesn't support minute-level
  granularity, and a schedule that restricts it is invalid.
- A cron schedule defines an allowed **time window**, not an exact trigger
  instant: Renovate itself runs on its own polling cadence (external to
  this project) and only acts during the window the schedule describes.

So `"* 0 1 * *"` is the *correct* idiomatic form for "once a month, on the
1st, during hour 0"; it is not a bug, even though it looks like a broken
"every minute" cron at a glance. Do not change it to `"0 0 1 * *"` (a
standard-cron-style fix): that's invalid for Renovate specifically, since it
constrains the minutes field.

Reference: <https://docs.renovatebot.com/key-concepts/scheduling/>
