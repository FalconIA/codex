# HTTP error response bodies

By default, Codex does not include a provider response body when an unrecognized
HTTP `429 Too Many Requests` response falls back to the retry-limit error. The
error retains its status and request ID and displays the usual short summary.

Set `CODEX_HTTP_429_FULL_BODY` to `1` or `true` before starting Codex to retain
and display the entire raw response body for that generic `429` fallback. Any
other value, including an unset variable, leaves the default behavior unchanged.

## Enable or disable

PowerShell for the current session:

```powershell
$env:CODEX_HTTP_429_FULL_BODY = "1"
codex

# Disable again.
Remove-Item Env:CODEX_HTTP_429_FULL_BODY -ErrorAction SilentlyContinue
```

POSIX-compatible shells for the current session:

```sh
export CODEX_HTTP_429_FULL_BODY=true
codex

# Disable again.
unset CODEX_HTTP_429_FULL_BODY
```

The one-shot form also works:

```sh
CODEX_HTTP_429_FULL_BODY=1 codex
```

## Scope and safety

- The switch applies only when an HTTP `429` response is not recognized as a
  typed quota, usage, capacity, policy, or other specifically handled error and
  therefore becomes a retry-limit error.
- Recognized typed `429` mappings remain unchanged, and status handling outside
  generic `429`, including HTTP `402`, is unaffected.
- The retry-limit error variant, retry classification, status, request ID, and
  `Retry-After` metadata are preserved.
- An empty or whitespace-only body falls back to the usual retry-limit summary.
- The switch does not make additional HTTP requests or add request headers or
  credentials to the output. Provider response bodies can themselves contain
  sensitive diagnostic, account, or request information, so enable full-body
  display only when that exposure is acceptable.
