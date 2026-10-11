# Access, setup and recovery

Read the section matching the failure or setup request. This skill cannot bypass Monash SSO/MFA or make a Moodle session permanent.

## Installation and first access

The dependency is [bunizao/moodle-cli](https://github.com/bunizao/moodle-cli). Use its current installation instructions when installation is requested. The locally inspected baseline is 0.10.0; its npm build specifies Node >=22.13. Its standalone distribution bundles a runtime.

For a requested npm installation:

```bash
npm install -g moodle-cli
moodle --version
```

Configure root `https://learning.monash.edu`, not a `/my/` or course URL. Prefer a per-command environment setting to changing unrelated site configuration:

```bash
MOODLE_BASE_URL=https://learning.monash.edu moodle auth login
MOODLE_BASE_URL=https://learning.monash.edu moodle user --json
```

The user completes credentials and MFA in the login UI. Report access as verified only after an authenticated read succeeds. Do not echo the account record unnecessarily.

## Existing CLI access fails

1. Inspect the original error. A missing CLI executable, network failure, malformed command, permission denial and expired session need different remedies.
2. For authentication questions, use `moodle auth status --json`; for unclear runtime/browser/cache errors use `moodle doctor --json`. Avoid dumping diagnostic logs into the conversation or repository.
3. If access has expired, explain that user sign-in is needed. Within authorized login/setup work, use `moodle auth login` and let the user finish SSO. Retry the original read once after successful recovery.
4. If the same failure remains, report the concrete cause and next user action. Do not loop login, cookie extraction or `--no-cache` attempts.

Valid cached credentials can allow queries without a visible login each time. Cache presence alone does not prove server-side validity. Session expiry, SSO policy or revocation can still require sign-in.

## macOS Security prompts

Determine which component is requesting access and the non-secret dialog text. Use an authorized screenshot/UI inspection if available, or ask for the wording. Do not diagnose every Security window as a Full Disk Access problem.

- A Keychain prompt may relate to browser cookie decryption or encrypted CLI session storage. Identify the named requesting app/runtime and keychain item before explaining the choice. Do not automatically select “Always Allow”.
- A browser database permission error may involve Full Disk Access for the app running the command. Recommend that setting only when the error supports it; the app may need restarting.
- A browser/SSO or Cloudflare login page is a separate authorization flow, not a Keychain permission.

Stop reissuing the operation while a modal remains open. If this task launched a blocked process, terminate only that identified process when appropriate; do not kill all `security`, browser or terminal processes. Do not change Keychain ACLs, disable macOS protections or purge credentials to suppress a dialog.

If supported automatic session capture remains unavailable, the user may use the CLI's secure local `moodle auth login --paste` prompt themselves. Never ask them to paste the cookie or a “Copy as cURL” request into chat, an argument, a script or GitHub.

## Optional keepalive

When the user requests continued session renewal, inspect existing state before installing a duplicate job:

```bash
moodle auth keepalive status --json
moodle auth keepalive install
```

Explain the background job and verify its status afterwards. Renewal can reduce idle expiry but cannot guarantee Monash will never request authentication. A one-off due check does not authorize installing a background job.

## Existing private MCP or requested deployment

Use discovered tool schemas for reads. If remote access fails, `moodle mcp status --json` can distinguish deployment state from Moodle session state. Do not treat local CLI success as proof that the Worker's session is valid.

- `SESSION_EXPIRED`: during authorized recovery, `moodle mcp login` acquires and uploads a fresh session.
- `MOODLE_AUTH_REQUIRED` with `recovery.url`: relay the returned recovery link privately to the user, then retry after they finish. Do not fetch or consume one-time owner/pairing links yourself.
- Requested setup: inspect `moodle mcp deploy --help` and upstream setup guidance for the installed version before deploying. Cloudflare is required only for the managed remote path. Explain relevant external resources and renewal behavior; do not hardcode a personal Worker endpoint or assume automatic mobile-token renewal is available at Monash.
- If Wrangler reports no route or `workers.dev` subdomain exists, direct the user to the account's onboarding link from that error to register a subdomain, or use their chosen configured route. Retry deployment after that prerequisite is met. Do not create an unrelated Worker manually as a workaround.

Use `moodle mcp connect` or `moodle mcp pair` when requested client setup needs it. Pairing codes and setup URLs are temporary access material: deliver only to the owner in the setup conversation and never commit them. Token rotation, revocation, repair and removal are separate operations with their own consequences; do not invoke them for an ordinary content query.
