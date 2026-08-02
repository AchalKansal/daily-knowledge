# Daily Knowledge

A daily general-knowledge digest delivered to Telegram and ntfy.

## How it works

This is a **Claude scheduled cloud routine** (see
https://claude.ai/code/routines) paired with a GitHub Actions delivery
pipeline, because the routine's cloud sandbox blocks outbound requests to
arbitrary hosts (confirmed: `api.telegram.org` and `ntfy.sh` both 403 at
the sandbox's network egress proxy).

**How it actually runs:**

1. Once a day, the Claude routine wakes up, researches one real, recent
   news item/development for each of the 14 categories in
   [topics.md](topics.md) (via WebSearch), writes the formatted digest to
   `latest/digest.txt`, and commits + pushes to this repo.
2. That push triggers `.github/workflows/deliver.yml` on GitHub's own
   runners (unrestricted network access), which reads `latest/digest.txt`
   and delivers it to:
   - **Telegram** — via the Bot API (`sendMessage`)
   - **ntfy.sh** — via a plain HTTP POST to a private topic

The routine's prompt is self-contained (topic list embedded directly),
since cloud agents don't have access to this machine or any local files.

## Delivery channels

- **Telegram bot**: created via @BotFather.
- **ntfy topic**: a private, hard-to-guess topic name (ntfy has no auth on
  the free tier — anyone who knows the exact topic name can subscribe to
  it, so treat the topic name itself as a secret).
- Both are stored as **GitHub Actions secrets** on this repo
  (`TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, `NTFY_TOPIC`) — not in the
  routine config and not committed to this repo's code.

## Managing the routine

- View/edit: https://claude.ai/code/routines
- To change the topic list, schedule, or delivery channels, edit the
  routine's prompt directly (or ask Claude to update it) and update
  [topics.md](topics.md) here to match.
