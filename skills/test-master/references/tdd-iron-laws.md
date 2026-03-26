# TDD Iron Laws

## The Fundamental Principle

> **NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.**

## The Three Iron Laws

### Iron Law 1
> "You shall not write any production code unless it is to make a failing test pass."

### Iron Law 2
> "If you didn't watch the test fail, you don't know if it tests the right thing."

### Iron Law 3
> "Production code exists → A test exists that failed first. Otherwise → It's not TDD."

## RED-GREEN-REFACTOR Cycle

```typescript
// RED: Write failing test
it('should return 0 for empty array', () => {
  expect(sum([])).toBe(0);
});

// GREEN: Simplest passing code
function sum(numbers: number[]): number {
  return 0;
}

// REFACTOR: Improve while keeping green
function sum(numbers: number[]): number {
  return numbers.reduce((acc, n) => acc + n, 0);
}
```

## Verification Checklist

- [ ] Every production function has corresponding tests
- [ ] Each test was written before its implementation
- [ ] Each test was observed to fail first
- [ ] Tests verify behavior, not implementation
- [ ] No production code exists without a test

*Adapted from obra/superpowers by Jesse Vincent (@obra), MIT License.*
