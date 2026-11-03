---
name: secure-defaults
description: >
  Checklist for missing hardening in small self-hosted apps, services, scripts and public
  repos: things a diff-based security review does not flag because they are absent.
  Use when building a new service/app/script, before publishing a repo, before a release,
  and when asked whether a project is "security-first".
---

# Secure defaults

A vulnerability review finds bad code. This finds **missing** protection. Walk every
group; for each item that applies, either build it or accept it consciously and write
down why (README "Security" section or a code comment). Unchecked is not an option.

## 1. Closed by default
- Auth is on out of the box. Required secrets (session key, tokens) are mandatory: refuse
  to start without them, or generate one and print it once. No "optional" token that
  silently leaves a guessable URL open.
- First-account setup only with a one-time code from the logs, never "whoever comes first".
- Every route needs auth unless deliberately public: API, downloads, exports, feeds,
  websockets. Test it: request each route anonymously.
- Allowlists, not denylists: senders, commands, paths, hosts, file types.
- Fail closed: on an error in an auth or permission check, refuse. Fail-open only on
  purpose and documented.

## 2. Login and sessions
- Rate-limit logins per IP and per account; compare secrets in constant time.
- Cookies: Secure (on by default, explicit opt-out for plain-HTTP LAN), HttpOnly, SameSite.
- Sessions: an absolute and an idle expiry, a new session ID at login, logout that really
  revokes.
- State-changing requests: CSRF token or a strict Origin check; never change state on GET;
  CORS off unless needed.
- SSO/OIDC: accept exactly one account, pinned on issuer + subject (not a username or an
  unverified email); validate signature, audience, expiry, nonce; use PKCE.
- Signed links and approvals: single use, with a nonce or short expiry (no replay).

## 3. Behind a proxy
- The app port is reachable only through the proxy/tunnel (bind to localhost or an
  internal network), so SSO and IP limits can't be bypassed.
- Trust X-Forwarded-For / identity headers only from that proxy. Behind container NAT the
  client IP may be the bridge gateway: check before relying on it.
- Security headers: CSP without 'unsafe-inline' (move inline JS/CSS to files),
  nosniff, frame-ancestors, Referrer-Policy, HSTS where HTTPS.

## 4. Input and output
- Lookups keyed by outside input: `Object.hasOwn`/`Map` (JS) or a dict with explicit
  membership, never a plain object index (prototype keys like `constructor`).
- Limits: request body, upload size, processing time, concurrent jobs, memory.
- No debug pages or stack traces in production; error messages without internals.

## 5. Secrets
- Never in URLs (webhook `sig=` links count as secrets), argv, logs, error output or git.
- Read from env/secret files with 0600 permissions. A secret pasted into a chat or log is
  leaked: rotate it.

## 6. Files and processes
- Temp files: `mktemp` in a private directory, 0600, write-then-rename (atomic); no fixed
  `/tmp` paths (symlink and race attacks).
- Shell out with an argument list, never a string with user input.
- State and cache files: in the user's own dir, not world-writable, not followed through symlinks.

## 7. Containers
- Rootless, a dedicated user per service, non-root inside the image.
- Own network per service towards the proxy, never one shared network for everything.
- `cap_drop: [ALL]`, `no-new-privileges`, read-only filesystem where the app tolerates it.
- No Docker/Podman socket, no privileged mode, no broad host mounts.
- Outbound traffic only where needed. Pin base images; scan images in CI (e.g. Trivy).
- Host-specific rules (users, uids, firewall, proxy layout) belong in your own deployment
  checklist; follow it if you have one.

## 8. Be honest about it
- README "Security" section: what is protected, what isn't, what the operator must do
  (HTTPS, proxy, secrets). Accepted risks listed there.

Sources: own review findings; additions inspired by OWASP cheat sheets (session management,
CSRF, OAuth2, REST, Docker).
