# The routine

What re checks when it is woken on a schedule rather than by a person. It is
a list so that a person can read it and change it by pull request.

## When

Twice a day by default. A run is skipped when nothing has happened since
the last one: no new commit on any deployed branch, no deploy, no edit in
the Cryptograss namespace. A run is never skipped for more than three days
in a row, because "nothing changed" is itself a claim worth checking.

Both numbers are settings, not constants.

## What, in order

1. **What changed.** New commits on each repository's deployed branch;
   deploys; edits in the Cryptograss namespace. Everything below is aimed at
   what changed.
2. **Docs.** Each page is stamped with the commit it was checked against.
   For every page whose stamp is no longer the deployed commit, re-check
   the claims the change could have touched. Correct the page or restamp it.
3. **Tests.** For each repository: can its runner find the tests, did they
   run for the commit that is deployed, and what happened. Tests that exist
   but that no runner collects are a finding.
4. **Jenkins.** The last build of each job. Failures, and jobs that haven't
   run in longer than they are supposed to take to come round. A job that
   is quietly not running is worse than one that fails.
5. **What is actually running.** The commit each server is serving, beside
   the commit its branch says it ought to be.

## Where the results go

- The tables (test results, builds, deployed commits) are written by a dumb
  tool to pages under `Cryptograss:Status`, stamped with the block. They are
  written every run.
- re speaks in the docs-tests-qa Mood only when something is wrong, or has
  changed in a way someone would act on. Otherwise the run leaves a dot.
