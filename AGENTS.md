# Instructions for AI assistants

A bite-size game theory course: plain JavaScript and DOM, no build step, static files on
Cloudflare, installable as an app. The plan is in [docs/SPEC.md](docs/SPEC.md); read it first.

- The audience is 13 and up. Nothing leaves the device: no accounts, analytics, ads, server or
  AI calls. A change that adds any of them needs the owner's explicit yes.
- Content, art and sound are our own. Ideas from *The Evolution of Trust* and the science are
  fine, credited; no copied text, art, sounds or fonts, and no language-learning app's name,
  mascot, colors or coined terms.
- Every claim a lesson makes about a game or strategy is checked by the solver or the claims
  test, never only by prose.
- Never put API keys, account or tenant IDs, domains other than the app's own, IPs, emails,
  secrets, or employer, customer or private-project names in the repo, commits, issues or PR
  text.
- Never ask for, show or store a secret in chat. The owner runs the step that handles it.
- Propose changes as pull requests. Never change a real account, deployment or spend setting
  without the owner's explicit yes.

## How to make a change

Four rules after the community "Karpathy guidelines" (MIT), reworded for agents that work unattended.

- Say what you assumed. When the ask leaves a detail open, pick the reasonable default, say which,
  and keep going. Stop to ask only when the choice changes the goal, something the owner will
  notice, or a step no one can undo; if no one is there to answer, do the reversible part and leave
  the question in the PR body. If a simpler way exists than the one asked for, say so.
- Write the least code that does the job: no features, options or abstractions nobody asked for,
  and no handling for cases that can't happen.
- Touch only what the task needs and match the style around it. Every changed line should trace
  back to the ask. Mention unrelated dead code instead of deleting it; remove only what your own
  change left unused.
- Before you start, name the check that proves the task is done (a failing test, the "Checks to
  run" below, an expected output), and don't call it done until it passes in front of you.

## Checks to run

```bash
node --test
```
