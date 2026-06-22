# Contribution 1: Burr UI Header Section

**Contribution Number:** 1 
**Student:** Timothy Lee  
**Issue:** [GitHub issue link](https://github.com/apache/burr/issues/411)
**Status:** Phase III Complete

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

### Week [Y] Progress

[Continue documenting as you work]

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
  
- **Approach decisions:** [Why you chose certain approaches]
- Split into a `docs/ui/` sections rather than expanding a single page. A dedicated directory with an `index.rst` toctree follows the same pattern used by other sections in the repo and makes the UI discoverable as a leading header in the sidebar
- Separated content by use case. Each serve distinct audiences, so splitting them avoids a long scrolling page and lets users navigate directly to what they need
- Kept the `.. _ui:` label on `ui/index.rst`, preserving any existing cross-references in the docs that point to the `:ref:\ui` target

---

## Pull Request

**PR Link:** https://github.com/apache/burr/pull/815

**PR Description:** Add a dedicated docs/ui/ section covering the Burr UI from installation through notebook usage and production deployment, with focused pages for getting-started, notebook/Colab, and deployment. Update docs/index.rst to point to the new ui/index toctree entry and remove the previous single-page ui.rst.

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

**Week 3**
- Pretty straightforward, I think just remembering to reference image paths correctly, especially when there's a lot of subdirectories in the repository. 

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
