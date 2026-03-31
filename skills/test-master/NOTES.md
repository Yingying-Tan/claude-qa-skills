# Test Master - Customization Notes

## Customization: MCP Tool Integration
Add the following to `SKILL.md` in two places:

### 1. Add "Available Tools" section to SKILL.md
```markdown
## Available Tools
- **Jira MCP** — read story details, acceptance criteria, priority, and linked tickets
- **GitHub MCP** — read PR changes, codebase files, and understand code implementation logic
```

### 2. Add MCP usage to each Workflow step
```markdown
0. **Load context**
   - Search `references/domain-knowledge/` by keyword
   - IF Jira ticket provided → use Jira MCP to read story description, AC, and priority
   - IF GitHub repo/PR provided → use GitHub MCP to read PR diff and relevant code files to understand implementation logic

1. **Define scope**
   - Use Jira MCP story details to identify what components are changed
   - Use GitHub MCP to read codebase and understand how the feature is implemented

2. **Create strategy**
   - Map Jira AC items directly to test cases
   - Use GitHub MCP code reading to identify edge cases from actual implementation

3. **Write tests**
   - Base test cases on Jira AC + actual code logic from GitHub MCP
```

### Why both are needed:
- Jira MCP → provides WHAT needs to be tested (requirements, AC, business intent)
- GitHub MCP → provides HOW it is implemented (code logic, edge cases, dependencies)
- Together → enables precise, implementation-aware test cases instead of generic ones

---

## Rule: Domain Knowledge lookup priority
When performing any test analysis, Claude MUST follow this workflow:
1. **First** — search `references/domain-knowledge/` by keyword matching the feature/topic being tested
2. **If found** — read and understand the domain knowledge first, then apply `SKILL.md` workflow and all relevant references to complete the testing work
3. **If not found** — proceed directly with `SKILL.md` workflow using QA methodology and common sense
- Domain knowledge provides business context, NOT a replacement for QA methodology
- The full test analysis is always driven by `SKILL.md` + references; domain knowledge enriches it with business-specific rules
- Suggested fix: Add this lookup rule explicitly to `SKILL.md` Core Workflow Step 0

---

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
