# Automation Frameworks

## Advanced Framework Patterns

### Screenplay Pattern
```typescript
export class Actor {
  constructor(private page: Page) {}
  attemptsTo(...tasks: Task[]) {
    return Promise.all(tasks.map(t => t.performAs(this)));
  }
}

class Login implements Task {
  constructor(private email: string, private password: string) {}
  async performAs(actor: Actor) {
    await actor.page.getByLabel('Email').fill(this.email);
    await actor.page.getByLabel('Password').fill(this.password);
    await actor.page.getByRole('button', { name: 'Login' }).click();
  }
}
```

## CI/CD Integration

```yaml
name: E2E Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test
```

## Quick Reference

| Pattern | Best For |
|---------|----------|
| Page Object | Reusable components |
| Screenplay | Complex workflows |
| Keyword-Driven | Non-tech testers |
| Model-Based | State machines |
