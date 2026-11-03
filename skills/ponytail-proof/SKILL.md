---
name: ponytail-proof
description: >
  Proof discipline on top of ponytail: no "done/works/fixed" without fresh evidence,
  a red-first loop for bugs, and one-item-first for bulk or irreversible actions.
  Use before claiming any task complete, when something is broken/failing/slow,
  and before any bulk, destructive or hard-to-reverse change (mass updates,
  deletes, migrations, API writes across many records).
---

# Ponytail proof

Ponytail decides *what* to change (the smallest complete change). This skill decides
*when you may say it works*. Both apply; ponytail's rules stay as they are.

## 1. Evidence before claims

No "done", "works", "fixed", "passes" or "should be fine" unless, in this same turn:

1. you named the check that proves it (test, command, request, page load, read-back),
2. ran it fresh and complete,
3. read the output (exit code, counts, the actual value),
4. and it confirms the claim. If not: report the real state, with the evidence.

- A subagent's or tool's "success" is a claim, not evidence: check the diff, file or state yourself.
- Partial checks prove partial things. Say which part you checked and which you did not.
- "Should", "probably", "seems to" before a check = stop and run the check.
- Can't verify (hardware, reboot, someone else's device)? Say exactly what is untested and how they can check it.

## 2. Bugs: red first, theories second

1. **Loop first.** Before reading code to form a theory, get one command that shows the
   user's exact symptom going red: a failing test, a curl, a CLI run with a fixture, a
   Playwright script, a replayed payload. Make it fast and deterministic. For flaky bugs,
   loop the trigger until the failure rate is debuggable. No loop possible: say so, list
   what you tried, ask for access or a captured artifact.
2. **Minimise.** Cut inputs/config/steps one at a time while it stays red.
3. **3–5 ranked hypotheses**, each falsifiable: "if X is the cause, changing Y makes it
   disappear". Share the list when the user may know something (recent deploys, ruled-out causes).
4. **Probe one variable at a time.** Tag every debug log with a unique prefix
   (`[DBG-a4f2]`) so cleanup is one grep. For slowness: measure a baseline, don't log.
5. **Fix at the root** (ponytail: one place, shared code), regression test at a real seam
   first if one exists, then re-run the original loop. Remove all tagged debug output.
6. **Three failed fixes = stop.** The design is likely wrong; discuss before fix #4.

Redact secrets in every command, log or output you show: `<REDACTED>`.

## 3. Bulk and irreversible actions

Anything touching many records, deleting, overwriting, migrating or writing to an external API:

1. **Know the write semantics.** Is it a merge/patch or a full replace? A full-replace call
   with one field wipes the rest. Prefer patch; read the API docs if unsure.
2. **Snapshot first**: export, backup or copy what you will change, somewhere you can restore from.
3. **One item first.** Do a single record, read it back completely, compare with the snapshot.
4. **Then the batch**, verifying every item (or a count plus spot checks), and report failures.
5. Irreversible and not explicitly approved? Ask first.

## Credits

Ideas adapted from obra/superpowers (verification-before-completion, systematic-debugging;
MIT, © 2025 Jesse Vincent) and mattpocock/skills (diagnosing-bugs; MIT, © 2026 Matt Pocock).
