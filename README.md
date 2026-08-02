# Daily Knowledge

A daily general-knowledge digest delivered to Telegram and ntfy.

## How it works

This isn't a script you run — it's a **Claude scheduled cloud routine**
(see https://claude.ai/code/routines). Once a day, at a fixed time, a cloud
agent wakes up, picks the day's topic (deterministic rotation, see below),
writes 1-2 concise, non-obvious facts or ideas about it, and sends the
result to both:

- **Telegram** — via the Bot API (`sendMessage`)
- **ntfy.sh** — via a plain HTTP POST to a private topic

The routine is self-contained (its prompt embeds the topic list and the
rotation logic), since cloud agents don't have access to this machine, this
folder, or any local files. This folder exists purely as a human-readable
record of the setup and the topic list — it is not read by the routine.

## Topic rotation

14 topics, one per day, cycling on `day_of_year % 14`. See [topics.md](topics.md)
for the list. Because the routine writes fresh content each run (not
pulling from a fixed database), repeats of a topic later in the cycle
surface different facts, not the same ones.

## Delivery channels

- **Telegram bot**: created via @BotFather. Token + chat ID are stored in
  the routine config on claude.ai, not in this repo.
- **ntfy topic**: a private, hard-to-guess topic name (ntfy has no auth on
  the free tier — anyone who knows the exact topic name can subscribe to
  it, so treat the topic name itself as a secret).

## Managing the routine

- View/edit: https://claude.ai/code/routines
- To change the topic list, schedule, or delivery channels, edit the
  routine's prompt directly (or ask Claude to update it) and update
  [topics.md](topics.md) here to match.
