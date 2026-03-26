# Testing Anti-Patterns

> **"Test what the code does, not what the mocks do."**

## The Five Anti-Patterns

### 1. Testing Mock Behavior
```typescript
// BAD
expect(mockApi).toHaveBeenCalledWith(1);

// GOOD
const user = await service.getUser(1);
expect(user.name).toBe('Alice');
```

### 2. Test-Only Methods in Production
```typescript
// BAD: _resetForTesting() in production class
// GOOD: Use fresh instances per test
function createFreshCache(): UserCache { return new UserCache(); }
```

### 3. Mocking Without Understanding
```typescript
// BAD: Mock everything
// GOOD: Mock only external services, use real deps where possible
```

### 4. Incomplete Mocks
```typescript
// BAD: { id: 1, name: 'Test' } missing email, permissions...
// GOOD: Use factories that fill all required fields
const mockUser = createMockUser({ name: 'Test User' });
```

### 5. Tests as Afterthought
```typescript
// BAD: Ship feature, add tests later
// GOOD: TDD - tests ship with the feature
```

## Detection Checklist

| Warning Sign | Anti-Pattern |
|-------------|-------------|
| Only mock assertions, no output test | Testing mock behavior |
| `_reset()` in production code | Test-only methods |
| 10+ mocks per test | Over-mocking |
| Mocks return `{ success: true }` only | Incomplete mocks |
| Test files added weeks after feature | Tests as afterthought |

*Adapted from obra/superpowers by Jesse Vincent (@obra), MIT License.*
