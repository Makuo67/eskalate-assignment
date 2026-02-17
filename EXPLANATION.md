# Assessment Explanation

- What was the bug?

The Client.request() method incorrectly handled dictionary-based OAuth2 tokens. The logic only refreshed tokens when self.oauth2_token was falsy or when it was an OAuth2Token instance whose expired property returned True. This meant that if the token was a dictionary (e.g., {"expires_at": 0}), it was treated as a valid token even though it could not generate an Authorization header. This resulted in API requests missing authentication.

- Why did it happen?

The refresh condition used this check:

```
if not self.oauth2_token or (
    isinstance(self.oauth2_token, OAuth2Token) and self.oauth2_token.expired
):
```

A dictionary is truthy and is not an OAuth2Token, so the expiration branch is skipped. As a result, invalid dict tokens were silently accepted. Later, the code attempted to set the Authorization header only if the token was an OAuth2Token, which left the request unauthenticated.

- Why does the fix solve it?

The fix replaces the condition with:

```
if not isinstance(self.oauth2_token, OAuth2Token) or self.oauth2_token.expired:
```

This ensures only real OAuth2Token instances are treated as valid. Any dict, None, or malformed token now correctly triggers a refresh. This is the smallest and most targeted fix.

- One edge case not covered by tests

Tests do not cover the scenario where an OAuth2Token exists but contains a corrupted or non-integer expires_at value. This could cause unexpected comparison behavior at runtime.
