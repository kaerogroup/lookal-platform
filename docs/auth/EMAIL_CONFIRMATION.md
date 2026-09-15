# LookaL Auth — Email Confirmation

Status: FOUNDATION

## Objective

Provide a first-party LookaL email-confirmation experience while Supabase remains the identity authority behind the product.

User-facing surfaces must use LookaL identity. Supabase must not become the visible product surface.

## Canonical flow

```text
/register
  -> Supabase signUp(email, password)
  -> /verify-email
  -> LookaL confirmation email
  -> /auth/confirm?token_hash=...&type=email
  -> server-side supabase.auth.verifyOtp(...)
  -> session cookie established
  -> /auth/verified
  -> onboarding or authenticated workspace
```

## Required application surfaces

- `/register`
- `/verify-email`
- `/auth/confirm`
- `/auth/verified`
- `/auth/error`

Runtime implementation must follow the application's existing framework and auth client once that source is present in this repository. Do not create a parallel auth stack.

## Supabase email template

Canonical source:

`supabase/templates/confirmation.html`

Supabase dashboard target:

`Authentication -> Email Templates -> Confirm signup`

The confirmation CTA must point to the first-party endpoint:

```text
{{ .SiteURL }}/auth/confirm?token_hash={{ .TokenHash }}&type=email
```

The application confirmation endpoint must verify the token server-side using `supabase.auth.verifyOtp()` before creating/accepting an authenticated session.

## URL configuration

In Supabase Authentication URL Configuration:

- `Site URL` must be the canonical LookaL production origin.
- Development/preview origins must be explicitly allow-listed where required.
- Redirects must never accept arbitrary external destinations from an untrusted query parameter.

## UX contract

### After registration

Show a first-party `/verify-email` state rather than treating an unconfirmed registration as a completed login.

Required information:

- clear title: `Semak email anda`
- masked or explicit account email where appropriate
- explanation that confirmation is required
- `Hantar semula email` action
- `Tukar alamat email` / back-to-register path

### After successful confirmation

Show `/auth/verified` briefly, then continue to canonical onboarding/workspace resolution.

Recommended copy:

- title: `Email berjaya disahkan`
- body: `Akaun LookaL anda kini aktif.`
- CTA: `Teruskan`

### Failure state

Expired, invalid, already-consumed, or malformed confirmation links must not expose raw Supabase errors.

Route to `/auth/error` with a stable LookaL-facing message and a safe resend path.

## Brand rules

- official `LookaL` wordmark/logo asset only; do not invent a logo path
- no Supabase branding in user-facing UI
- restrained corporate surface; no gradients or generic AI styling
- one primary action per auth state
- mobile-first typography and spacing
- Malay-first copy with English support following the product's language architecture

## Security rules

- Supabase remains authentication/session authority.
- Never expose `service_role` or secret keys to a browser bundle.
- Never trust `user_metadata` for authorization.
- Confirmation success must be based on verified token/session state, not query-string presence.
- Remove `token_hash` and auth token parameters from the browser URL after verification/redirect.
- Do not allow protected workspace access when the product policy requires confirmed email and `email_confirmed_at` is absent.
- Resend endpoints must be rate-limited and return non-enumerating responses where practical.

## Delivery boundary

This foundation intentionally does **not** invent a new application runtime. The current `lookal-platform` main branch contains product documentation/skeleton structure but no canonical Next.js/React Supabase auth implementation to patch.

Runtime slice begins only against the actual application source of truth. At that point implementation should include:

1. sign-up redirect/state handling
2. `/verify-email`
3. `/auth/confirm` server handler
4. `/auth/verified` and `/auth/error`
5. resend confirmation action
6. route protection for unconfirmed accounts
7. integration tests for valid, invalid, expired, and replayed links

## Acceptance

A production acceptance is only valid when all of the following are observed:

```text
register -> verify-email page -> receive LookaL email -> confirm -> verified state -> authenticated workspace
```

and:

- no Supabase-branded user-facing page is exposed
- invalid/expired links fail safely
- token parameters are not retained in final browser URL
- direct protected-route access by an unconfirmed account is blocked according to policy
- resend confirmation works without account-enumeration leakage
