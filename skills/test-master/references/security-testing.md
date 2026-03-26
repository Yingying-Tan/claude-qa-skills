# Security Testing

## Authentication Tests

```typescript
describe('Authentication Security', () => {
  it('rejects invalid credentials', async () => {
    await request(app)
      .post('/api/login')
      .send({ email: 'user@test.com', password: 'wrong' })
      .expect(401);
  });

  it('rejects expired tokens', async () => {
    const expiredToken = createExpiredToken();
    await request(app)
      .get('/api/protected')
      .set('Authorization', `Bearer ${expiredToken}`)
      .expect(401);
  });

  it('enforces rate limiting on login', async () => {
    for (let i = 0; i < 6; i++) {
      await request(app)
        .post('/api/login')
        .send({ email: 'user@test.com', password: 'wrong' });
    }
    await request(app)
      .post('/api/login')
      .expect(429);
  });
});
```

## Security Test Checklist

| Category | Tests |
|----------|---------|
| **Auth** | Invalid creds, token expiry, tampering |
| **Input** | SQL injection, XSS, command injection |
| **Access** | IDOR, privilege escalation |
| **Rate Limit** | Brute force, API abuse |
| **Headers** | CSP, HSTS, X-Frame-Options |
| **Data** | PII exposure, error messages |

## Quick Reference

| Vulnerability | Test Approach |
|---------------|--------------|
| SQL Injection | `'; DROP TABLE--` in inputs |
| XSS | `<script>alert(1)</script>` |
| IDOR | Access other user's resources |
| CSRF | Missing/invalid tokens |
| Auth Bypass | Missing auth, expired tokens |
