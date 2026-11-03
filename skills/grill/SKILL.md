---
name: grill
description: Stress-test the user's plan, decision or idea by questioning it in rounds, each question with a recommended answer. Only when the user asks ("grill", "grill me", /grill).
disable-model-invocation: true
---

# Grill

Question the user's plan until you both understand it the same way. Do not build or act
on it until they confirm that point is reached.

- Treat the plan as a tree of decisions. Each round, ask every question whose
  prerequisites are already settled (the frontier), not ones that depend on answers you
  don't have yet. Keep rounds small enough to answer on a phone: at most 3–4 questions.
- Each question: a short title, the question with concrete options, then your recommended
  answer. Word it so "yes" accepts your recommendation.
- Facts are your job, never the user's: look them up (files, tools, docs) instead of asking.
- After each round, recompute the frontier: answers settle some branches and open others.
  Drop questions that an answer made moot.
- Done when no open branch is left and nothing is silently assumed. End with a short
  summary of the decisions, then ask whether to act on it.
- In a Telegram session: plain text, one round per message, and buttons for the
  recommended answer where it fits.

Adapted from mattpocock/skills (grilling; MIT, © 2026 Matt Pocock).
