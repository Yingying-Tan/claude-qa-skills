# Test Master - Customization Notes

## Customization: Add Domain Knowledge as mandatory Step 0
- Create `references/domain-knowledge.md` with fixed business rules (password policy, roles, lockout rules, etc.)
- Add it to `SKILL.md` Core Workflow as Step 0 — ALWAYS load before anything else:
  ```markdown
  0. **Load context** — ALWAYS load `references/domain-knowledge.md` first
  ```
- Also add to Reference Guide table with "ALWAYS" trigger:
  ```markdown
  | Domain Knowledge | `references/domain-knowledge.md` | ALWAYS — load before every task |
  ```
- Dynamic inputs (Jira ticket, GitHub repo) go in the Prompt — NOT in the skill
- Only fixed, rarely-changing business rules belong in `references/domain-knowledge.md`

---

## Rule: qa-methodology.md is MANDATORY for all testing work
- ALL test strategies MUST go through `references/qa-methodology.md` first
- ALL test cases MUST be designed based on `references/qa-methodology.md`
- Customization needed: Add `qa-methodology.md` as a required load in SKILL.md, NOT optional
- Suggested fix: Update SKILL.md Core Workflow to explicitly state:
  - Step 1 (Define Scope): ALWAYS load `references/qa-methodology.md` — use Test Plan Template
  - Step 2 (Create Strategy): ALWAYS validate strategy against `references/qa-methodology.md` — use Risk-Based Testing matrix
  - Step 3 (Write Tests): ALWAYS check test cases against `references/qa-methodology.md` — use Test Design Techniques

---

## Gap 1: "How to Identify Components" not covered
- `SKILL.md` Step 1 only says: "Identify what to test and which testing types apply"
- No guidance on *how* to identify components
- Suggested fix: Add a section to `references/qa-methodology.md` or a new `references/scope-definition.md`

## Gap 3: `related-skills` has no instructions for Claude — see SKILLS_GUIDE.md

**Reference:** https://github.com/Jeffallan/claude-skills/blob/main/SKILLS_GUIDE.md

### Where test-master fits in workflows (from SKILLS_GUIDE.md):

**New Feature Development:**
1. Feature Forge → define requirements
2. Architecture Designer → design system
3. Fullstack Guardian + Framework Skills → implement
4. **Test Master + Playwright Expert** → test comprehensively ← here
5. Code Reviewer → review code
6. Security Reviewer → security review
7. DevOps Engineer → deploy
8. Monitoring Expert → add observability

**Bug Fixing:**
1. Debugging Wizard → identify root cause
2. Fullstack Guardian + Framework Skills → fix
3. **Test Master** → add regression tests ← here
4. Code Reviewer → review fix
5. DevOps Engineer → deploy

### What each related skill adds to test-master:
- `playwright-expert` — E2E browser testing (used together in feature dev)
- `devops-engineer` — CI/CD pipeline for running tests on deploy
- `fullstack-guardian` — implements the feature that test-master then tests
- `debugging-wizard` — identifies root cause before test-master adds regression tests
- `code-reviewer` — reviews code after test-master validates it
- `feature-forge` — defines requirements before test-master plans test strategy

### Gap:
- This relationship context lives only in `SKILLS_GUIDE.md` on GitHub — NOT loaded by Claude
- Suggested fix: Add a "When to combine" section to `SKILL.md` explaining the above

- `SKILL.md` lists: `related-skills: fullstack-guardian, playwright-expert, devops-engineer, debugging-wizard, code-reviewer, feature-forge`
- But gives Claude no instructions on: when to suggest them, how to combine them, or what each one adds
- The explanation lives in `CONTRIBUTING.md` and `README.md` in the GitHub repo — neither is loaded by Claude
- Suggested fix: Add a "Related Skills" section to `SKILL.md` explaining when and how to use each related skill, e.g.:
  - `playwright-expert` — use when E2E browser tests are needed
  - `devops-engineer` — use when setting up CI/CD pipelines for tests
  - `fullstack-guardian` — use when combining testing with code review

## Gap 2: No mapping between workflow steps and reference files
- The Reference Guide table in `SKILL.md` only has "Load When" based on tools/topics
- No explicit link between the 5 workflow steps and which reference file to load
- Suggested fix: Add a "Workflow Step" column to the Reference Guide table, e.g.:
  | Topic | Reference | Load When | Workflow Step |
  |-------|-----------|-----------|---------------|
  | QA Methodology | qa-methodology.md | Manual testing, shift-left | Step 1: Define Scope |
  | Unit Testing | unit-testing.md | Jest, Vitest, pytest | Step 3: Write Tests |
