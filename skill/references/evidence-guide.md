# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives:**
- The plan-context block's scope section (what files, what boundary, what is excluded)
- The candidate PR's diff (unified format; the file paths, line ranges, and changed code)
- The candidate PR's description (any claims about scope or fidelity)

**What good looks like:**
- Every changed file in the diff falls inside the plan's stated boundary (e.g., "only src/parser/tokenizer.c and tests/")
- The diff makes exactly what the plan describes; no silent additions like new config options, new files outside scope, or rewritten unrelated functions
- If the description claims "implements the posted plan exactly" or "no changes beyond the plan", the diff must match: no hidden hunks, no undisclosed extras
- Deviation notes in the plan explicitly permit any deviation from the posted scope, and the diff honors those notes
- If the description claims less than the plan (e.g., "deferred X"), the plan's notes must have approved the deferral before the PR was opened

**What silent drift looks like:**
- The diff changes files the plan never mentioned (e.g., a config rewrite hiding in the second hunk)
- The description claims "exactly as planned" while the diff includes unscoped work (new flags, restructuring)
- The plan notes a 100-line limit; the diff spans 300 lines with no note explaining why
- A description saying "no API changes" while the diff adds a new optional parameter

## Test evidence (harness category: not-tested)

**Where it lives:**
- The test-evidence section of the candidate PR (before/after, with the specific repro run)
- The plan-context block's test plan (what the student committed to testing)

**What good looks like:**
- The plan names a specific repro (e.g., "re-run the issue's script, expect len == 100000")
- The test-evidence section shows that exact repro run before and after the fix
- The before output shows the original failure (error, wrong value, or missing behavior)
- The after output shows the expected result (no error, correct value, or expected behavior)
- Both runs are explicitly named or labeled so the change is observable (not just "tests pass")
- For additional test scenarios the plan names: either shown as before/after output, or verified by passing unit/integration tests in the diff
- For unchanged cases ("verify the control still works"): the repo's stated test suite passing verifies this (no separate before/after evidence needed)
- If the plan requires repo checks or suite pass, those are shown run and passing

**What not-tested looks like:**
- "Verified working on my machine" with no concrete repro shown
- "Tests pass" with no indication which tests, no before/after
- The plan requires the issue's repro; only unrelated tests are shown
- The plan names two failure modes; only the control (unchanged path) is tested
- The evidence runs a different repro than the plan stated (e.g., tests a single-file case when the plan required two files)
- The plan says "verify the fix", but no new test is added and the existing suite is not shown passing

## Diff quality (harness category: unreviewable)

**Where it lives:**
- The unified diff (changed code, line ranges, hunks)
- The commit list and commit messages

**What good looks like:**
- The fix is visible: changed code directly addresses the stated problem (e.g., "added capacity check before buffer copy")
- No unrelated hunks (the diff does not include rewrites of unrelated functions, configuration churn, or file moves)
- No debug output (print statements, eprintln, console.log left behind)
- No dead code (functions marked allow(dead_code), commented-out blocks, or old implementations)
- No pure formatting churn (mechanical re-indents, newline-only changes, import reordering unrelated to the fix)
- Commit messages are clear (e.g., "parser: reserve stream space for pending copy" not "wip" or "fix")

**What unreviewable looks like:**
- Commented-out debug lines in the fix itself
- The actual fix is buried in a hunk that also re-indents an entire file
- A commit named "fmt + cleanup" that includes both the fix and unrelated refactoring
- A function left behind marked with allow(dead_code) with no comment
- Multiple "first attempts" visible in comments

## Standards and comms (harness category: standards-wall)

**Where it lives:**
- The repo-facts block (PR template sections and stated contribution policy or AI-use disclosure requirement)
- The candidate PR's title, description, and commits (whether they answer the template asks)
- The stated AI-use policy (whether the PR honors it)

**What good looks like:**
- The repo's template asks for "Closes #xxxx" and "Tests added"; both appear in the description or commit message
- The template asks for a CHANGELOG entry; the diff includes it (repo-facts note which file)
- The template asks for a type-of-change label ("bug fix", "feature", etc.); the description explicitly states it
- If the repo states "all AI usage must be disclosed", the description contains a disclosure (e.g., "This fix was developed with AI assistance")
- If the policy says "AI use requires maintainer approval", the thread engagement shows that discussion happened (unlikely in eval but the frame must allow it)
- All required template sections are filled with substantive content, not boilerplate

**What standards-wall looks like:**
- The repo requires "Closes #xxxx"; the description omits it
- The repo template has a required checklist; it is not checked or is empty
- The repo states "all AI usage must be disclosed"; the description does not mention AI
- The description copy-pastes template boilerplate without filling it in
- The PR template asks "screenshots if appropriate"; a visual change arrives with none

