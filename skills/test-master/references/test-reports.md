# Test Reports

## Test Report Template

```markdown
# Test Report: {Feature Name}

**Date**: YYYY-MM-DD  
**Tester**: {Name}  
**Version**: {App Version}

## Summary

| Metric | Value |
|--------|-------|
| Total Tests | X |
| Passed | X |
| Failed | X |
| Coverage | X% |

## Findings

### [CRITICAL] {Issue Title}
- **Location**: src/api/users.ts:45
- **Expected**: 401 Unauthorized
- **Actual**: 201 Created
- **Fix**: Add auth middleware

### [HIGH] / [MEDIUM] / [LOW] {Issue Title}
- **Details**: ...

## Coverage Analysis

| Module | Lines | Branches | Functions |
|--------|-------|----------|-----------|
| api/ | 85% | 78% | 90% |

## Sign-off
- [ ] All critical issues addressed
- [ ] Coverage meets threshold (80%)
- [ ] Performance meets SLA
```

## Severity Definitions

| Severity | Criteria |
|----------|---------|
| **CRITICAL** | Security vulnerability, data loss, system crash |
| **HIGH** | Major functionality broken, severe performance |
| **MEDIUM** | Feature partially working, workaround exists |
| **LOW** | Minor issue, cosmetic, edge case |
