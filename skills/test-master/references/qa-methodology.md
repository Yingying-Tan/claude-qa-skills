# QA Methodology

## Manual Testing Types

### Exploratory Testing
```markdown
**Charter**: Explore {feature} with focus on {aspect}
**Duration**: 60-90 min
**Mission**: Find defects in {specific functionality}

Test Ideas:
- Boundary conditions & edge cases
- Error handling & recovery
- User workflow variations
- Integration points

Findings:
1. [HIGH] {Issue + impact}
2. [MED] {Issue + impact}

Coverage: {Areas explored} | Risks: {Identified risks}
```

### Usability Testing
```markdown
**Task**: Can users complete {action} intuitively?
**Metrics**: Time to complete, errors made, satisfaction (1-5)
**Success**: 80% complete without help in <5 min
```

### Accessibility Testing (WCAG 2.1 AA)
```typescript
test('accessibility compliance', async ({ page }) => {
  await page.keyboard.press('Tab');
  expect(['A', 'BUTTON', 'INPUT']).toContain(
    await page.evaluate(() => document.activeElement.tagName)
  );
  const violations = await page.evaluate(async () => {
    const axe = await import('axe-core');
    return (await axe.run()).violations;
  });
  expect(violations).toHaveLength(0);
});
```

## Test Design Techniques

### Risk-Based Testing
```markdown
| Risk | Probability | Impact | Priority | Test Effort |
|------|-------------|--------|----------|-------------|
| Critical | High | High | P0 | Exhaustive |
| High | Med-High | High | P1 | Comprehensive |
| Medium | Low-Med | Med | P2 | Standard |
| Low | Low | Low | P3 | Smoke only |
```

## Defect Management

### Root Cause Analysis (5 Whys)
```markdown
1. Why did defect occur? {User input not validated}
2. Why wasn't it validated? {Validation logic missing}
3. Why was it missing? {Requirement unclear}
4. Why was requirement unclear? {Acceptance criteria incomplete}
5. Why incomplete? {No QA review in planning}

**Root Cause**: QA not involved in requirements phase
**Prevention**: Add QA to all planning meetings
```

### Defect Report Template
```markdown
## [CRITICAL] {Defect Title}

**Steps to Reproduce**:
1. {Step 1}
2. {Step 2}

**Expected**: {Should happen}
**Actual**: {Actually happens}
**Impact**: {Business/user impact}
**Root Cause**: {Why it happened}
**Fix**: {Recommended solution}
```

## Quality Metrics

### Quality Dashboard
```markdown
| Metric | Target | Actual | Trend | Status |
|--------|--------|--------|-------|--------|
| Coverage | >80% | 87% | ↑ | ✓ |
| Defect Leakage | <5% | 3% | ↓ | ✓ |
| Automation | >70% | 68% | ↑ | ⚠ |
| Critical Defects | 0 | 0 | → | ✓ |
| MTTR | <48h | 36h | ↓ | ✓ |
```

## Continuous Testing & Shift-Left

### Feedback Cycle Targets
```typescript
const feedbackCycle = {
  unitTests: '< 5 min',
  integration: '< 15 min',
  e2e: '< 30 min',
  regression: '< 2 hours',
};
```

## Quality Gates
```markdown
**Must Pass (Blockers)**:
- [ ] Zero critical defects
- [ ] Coverage >80%
- [ ] All P0/P1 tests passing
- [ ] Performance SLA met
- [ ] Security scan clean
- [ ] Accessibility WCAG AA

**Decision**: GO | NO-GO | GO with exceptions
```

## Test Planning

### Test Plan Template
```markdown
## Test Plan: {Feature}

**Scope**: {What to test}
**Types**: Unit, Integration, E2E, Perf, Security
**Resources**: {Team allocation}
**Dependencies**: {Prerequisites}
**Schedule**: {Timeline}
**Entry Criteria**: {Start conditions}
**Exit Criteria**: {Completion conditions}
**Risks**: {Identified risks + mitigation}
```

## Quick Reference

| Metric | Excellent | Good | Needs Work |
|--------|-----------|------|------------|
| Coverage | >90% | 70-90% | <70% |
| Leakage | <2% | 2-5% | >5% |
| Automation | >80% | 60-80% | <60% |
| MTTR | <24h | 24-48h | >48h |
