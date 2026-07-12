# Contribution 1: Burr UI Header Section
# Contribution 2: CI: release CI follow-ups: tigher coverage

**Contribution Number:** 2 
**Student:** Timothy Lee  
**Issues:** 
- [Issue 1 (Completed)](https://github.com/apache/burr/issues/411) 
- [Issue 2 (Pending Approval)](https://github.com/apache/burr/issues/747)

**Status:** Phase IV Complete (PR #2)

---

## Why I Chose This Issue

This issue interests me because it is an intersection of many things that interest me. Apache Burr is essentially a lightweight in-process Python framework that standardizes the expression and execution of state machines as action-driven graphs. It is particularly suited for AI agent workflows and the primary languages it works with are Python and TypeScript, which are the two main languages I am strongest at, but also want to improve at. Understanding AI agent workflows is also something that has been intriguing to me as I have been diving deeper into AI in general and I hope to be able to contribute and gain more insight on this project as a whole.

---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

---

## Reproduction Process

### Environment Setup

Used the project's "Quick Start" instructions for setting up the project. Finished the setup in around 5 minutes. The repository is well-maintained and are clear on instructions so there were no issues setting up.

Working branch: https://github.com/t1mato/burr/tree/fix-issue-burr-ui-header

### Steps to Reproduce

Prerequsites: Install doc dependencies

1. From the repo root:
```
pip install -e ".[dev]"
pip install sphinx furo myst-nb sphinx-sitemap sphinx-toolbox
```
2. Build and view docs:
```
cd docs
make html
open _build/html/index.html
```
3. **Expected:** On the left sidebar, a Burr UI should be there to help the users find UI-related documentation. 
4. **Actual:** There is no "Burr UI" section at the top level, and users have to dig depeer into documentation. 

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** The Burr docs have no top-level "Burr UI" sidebar section, making the UI undiscoverable. UI content exists but is scattered across multiple files.

**Match:** Every top-level nav section in this project follows the same pattern: a directory with an `index.rst` landing page registered in `docs/index.srt` via `toctree`. The Hamilton UI screenshot demonstrates the target structure that was suggested: a top-level entry with sub-pages for Overview, Local Mode, and Production Mode.

**Plan:** 
1. Create `docs/burr_ui/index.rst` - section landing page with a toctree listing sub-pages and a one-paragraph intro to the UI.
2. Create `docs/burr_ui/overview.rst` - what the UI shows (projects, applications, steps), drawing from `concepts/tracking.rst` lines 36-44.
3. Create `docs/burr_ui/local-mode.rst` - running the UI locally, pulling content from `concepts/tracking.rst` lines 100-155 (terminal command, notebook launch, FastAPI mount).
4. Create `docs/burr_ui/production-mode.rst` - deploying in production, pulling content from `examples/deployment/monitoring.rst`.
5. Modify `docs/index.rst` - add `burr_ui/index` to the main `toctree` after `getting_started/index`.
6. Update `docs/concepts/tracking.rst` - add a `..seealso::` pointing to the new section so existing liinks don't become dead ends. 

**Implement:** [Link to your branch/commits as you work]

**Review:** All new `.rst` files need the Apache License header, no Sphinx build warnings, and content should be reorganized rather than duplicated.

**Evaluate:** Run `cd docs && make html`, open `_build/html/index.html`, and confirm "Burr UI" appears as a top-levels diebar section with working sub-pages and no build warnings. 

---

## Testing Strategy

### Unit Tests

- [X] Sphinx builds without warnings: Run `make html` in `docs/` and confirm no section title underline warnings or missing reference errors.
- [X] toctree entries resolve: Verify `docs/index.rst` → `ui/index` and `ui/index` → `getting-started`, `notebook`, `deployment` all render without broken links
- [X] Cross-reference integrity: Confirm the `.. _ui: label` on `ui/index.rst` resolves correctly for any existing `:ref:\ui` links elsewhere in the doc
- [X] Image path resolves: Verify png in `ui/index.rst` renders correctly

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week 3 Progress

Added a dedicated Burr UI section to the documentation and restructured the content
  from a single monolithic docs/ui.rst into a docs/ui/ directory with focused pages:

  - ui/index.rst — Overview, data model (Projects / Applications / Steps), and toctree
  - ui/getting-started.rst — Installation, connecting an app with with_tracker, and
  reloading prior state
  - ui/notebook.rst — Launching the UI from Jupyter and Google Colab via %burr_ui
  - ui/deployment.rst — Embedding in FastAPI and production backend options (local
  filesystem vs. S3)

  Updated docs/index.rst to point to the new ui/index toctree entry. PR #815 is open
  against apache/burr main.

### Code Changes

- **Files modified:**
- docs/index.rst
- docs/ui/index.rst
- docs/ui/getting-started.rst
- docs/ui/notebook.rst
- docs/ui/deployment.rst
  
- **Key commits:**
- [Commit 1](https://github.com/apache/burr/pull/815/changes/a7b475357e302cb90741712a33327832b3f8ba5f)
- [Commit 2](https://github.com/apache/burr/pull/815/changes/81ca828b411729ddfc98de023253abf0b895067b)
  
- **Approach decisions:** 
- Split into a `docs/ui/` sections rather than expanding a single page. A dedicated directory with an `index.rst` toctree follows the same pattern used by other sections in the repo and makes the UI discoverable as a leading header in the sidebar
- Separated content by use case. Each serve distinct audiences, so splitting them avoids a long scrolling page and lets users navigate directly to what they need
- Kept the `.. _ui:` label on `ui/index.rst`, preserving any existing cross-references in the docs that point to the `:ref:\ui` target

### Week 5 Progress

Finished a new PR and just opened it up. Waiting for review and approval.

Implemented all remaining sub-items from #747, covering four areas of CI/release hardening: file/script fixes, smoke test
reliability, CI coverage gaps, and license-header hygiene.

  - Files & Scripts — Renamed examples/deep-researcher/utils.py → deep_researcher_utils.py to avoid an Apache RAT basename
  collision with four other ASF-owned utils.py files; threaded --skip-signing through cmd_verify so CI can verify artifacts
  without GPG keys; extended RAT scanning to .whl artifacts alongside source/sdist tarballs.
  - Smoke Test — Replaced the hardcoded time.sleep(2) with a polling loop against /api/v0/projects that fails fast if the
  server exits; launched the server in its own process group and sent SIGTERM to the whole group on teardown to stop
  orphaned uvicorn processes; added a GET / check for the UI; added a --cleanup/--no-cleanup flag (auto-disabled under
  GITHUB_ACTIONS so workspaces survive for artifact upload).
  - CI Coverage Gaps — Added a bare-install job that installs the wheel without optional extras and imports core symbols,
  catching leakage of optional deps into core; added an sdist-wheel-equivalence job that rebuilds the wheel from the sdist
  and compares content hashes against the CI-built wheel; pinned the Apache RAT JAR download with a SHA256 checksum.
  - Hygiene — Added scripts/check_asf_headers.py (checks Python/YAML/shell files for the ASF header, reading .rat-excludes
  at runtime to stay in sync automatically) and wired it as a pre-commit hook; added a weekly Monday 09:00 UTC cron to the
  release validation workflow to catch dependency drift between releases.

  ### Code Changes

  - **Files modified:**
    - .github/workflows/release-validation.yml
    - .pre-commit-config.yaml
    - .rat-excludes
    - examples/deep-researcher/application.py, examples/deep-researcher/deep_researcher_utils.py (renamed)
    - scripts/apache_release.py, scripts/verify_apache_artifacts.py, scripts/ci_smoke_server.py
    - scripts/check_asf_headers.py (new)
    - tests/test_apache_release.py, tests/test_verify_apache_artifacts.py, tests/test_ci_smoke_server.py (new),
  tests/test_check_asf_headers.py (new)

  - **Key commits:**
    - [ci: add files/scripts](https://github.com/apache/burr/pull/832/changes/49ba90090f67bacec1ebdd0bdd81db9ed94dce22)
    - [ci: add smoke test improvements](https://github.com/apache/burr/pull/832/commits/41c866cfbea1dd9b4423fcb6ca6c0775978dbaba)
    - [ci: add CI coverage gaps](https://github.com/apache/burr/pull/832/commits/e6bf7d74643c4dfde36c654438009dfd7fc48bbf)
    - [ci: add hygiene improvements](https://github.com/apache/burr/pull/832/commits/0115dd093a8bc9651e72e8a7d54be386c9adeb84)
    - 
  - **Approach decisions:**
  - Compared wheels by file content hashes rather than binary equality in sdist-wheel-equivalence, since zip timestamps
  make byte-for-byte wheel comparison unreliable.
  - Ran the smoke-test server in its own process group (start_new_session=True) so teardown can SIGTERM the whole group,
  preventing orphaned uvicorn children — a plain proc.terminate() wouldn't reach subprocess-spawned workers.
  - Made check_asf_headers.py read .rat-excludes at runtime instead of duplicating an exclusion list, so the two license
  checks (RAT and the new pre-commit hook) can't drift out of sync.
  - Kept --cleanup/--no-cleanup defaulting to cleanup locally but auto-disabling under GITHUB_ACTIONS, so CI failures
  leave the workspace intact for artifact upload/debugging without requiring a manual flag in the workflow.

### Week 6 Progress
PR was approved and merged! A maintainer just left some comments and they suggested that I run some local tests to double-check some changes. Will do that and then go ahead and look for a third PR to work on.

---

## Pull Request

**PR Link 1:** https://github.com/apache/burr/pull/815

**PR Description:** Add a dedicated docs/ui/ section covering the Burr UI from installation through notebook usage and production deployment, with focused pages for getting-started, notebook/Colab, and deployment. Update docs/index.rst to point to the new ui/index toctree entry and remove the previous single-page ui.rst.

**Maintainer Feedback:**
- 6/22/2026: "This looks good, thanks!"

**Status:** Approved & Merged

**PR Link 2:** https://github.com/apache/burr/pull/832

**PR Description:** CI issue. Add more test coverage for various implementations, including files/scripts, smoke tests, coverage gaps, and hygiene. 

**Maintainer Feedback:**
- 7/3/2026 - "Will look shortly"
- 7/10/2026 - Asking for a few local tests to double-check, almost all changes look approved and it was merged!

**Status:** Approved & Merged

---

## Learnings & Reflections

**Week 3**

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome
- Pretty straightforward, I think just remembering to reference image paths correctly, especially when there's a lot of subdirectories in the repository. 

### What I'd Do Differently Next Time

**Week 5**

### Technical Skills Gained
- Wheel/sdist internals: Learned that .whl files are just zips with a RECORD manifest listing hashes of every other file
- Comparing wheels built at different times requires hashing each member's content and explicitly excluding RECORD
- Process-group signal handling in Python requires setting `start_new_session=True` on `subprocess.Popen` + `os.killpg(os.getpgid(pid), SIGTERM)` to reap an entire process tree, instead of using `proc.terminate()`, which only kills the direct child and orphans uvicorn workers
- Supply-chain pinning practices: pinning the Apache RAT JAR download by SHA256 rather than trusting the URL at fetch time

### Challenges Overcome
- Smoke test's original `time.sleep(2)` was both slow and flaky. Replacing it with the polling solved both.
- Understanding how wheels worked and diagnosing why two wheels built from idential sources still failed a binary diff

### What I'd Do Differently Next Time
- Add `--skip-signing` checks earlier in the release-tooling rather than as a follow-up pass
- Write the wheel comparison test cases before the implementation
- Possibly write smaller commits, utilizing test suites for each unique issue.

**Week 6**
- Just waiting for PR to approve merges, need to take a look over some comments and local tests that were suggested
- Once I finish the last few changes, I will get started on my third PR!

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
