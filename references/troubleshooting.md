# Troubleshooting

Read this when a tool errors, an env does not answer, or a result looks stale. Status words, causes and the Waive rule are in `SKILL.md`. Do not restate them.

Each entry is a diagnostic: symptom, check, what it means. None of them is a verdict. A point you drop or downgrade because of an entry still goes under `Known decisions applied`, with its source and the SHA you re-checked it on (`SKILL.md`, section Prior knowledge).

Hosts come from `.claude/rules/environments.md` or from the user. Never from this file.

## Env does not answer

**CDN error page "origin down" (Cloudflare 521, 522, or the equivalent) on a non-production env.**
Check the time and the project's notes on environments. Many teams shut integration and preprod envs down at night and on weekends.
Meaning: inside a documented shutdown window, the CUJs on that env are `BLOCKED`, cause `env down`: ask when the env is back, or for another env. Outside such a window, or right after this PR was deployed, record the error as observed, with the time and the deployed SHA if readable, and do not assume a shutdown.

**503 during working hours, every page.**
Check, if the user has cluster access, that the env's deployments are not scaled to zero (a sleep scheduler such as kube-green whose wake-up failed, a cost-saving job).
Meaning: env down. Scaling it back up mutates the env: see the env-change rule in `SKILL.md` (section Forbidden). An approval given in an earlier session does not count. Without that reply, the CUJs stay `BLOCKED`, cause `env down`.

**NXDOMAIN on an env host.**
Check with a public resolver (`nslookup <host> 8.8.8.8`), then `curl --resolve <host>:443:<ip>`.
Meaning: an on-demand env with nothing deployed may have no DNS record. Right after a deploy, a corporate or VPN resolver can keep the NXDOMAIN in cache for minutes while public resolvers already answer. Not `env down` until the second check fails. If it fails too, the CUJs are `BLOCKED`, cause `env down`, and you ask for the URL or another env.

**404 on a host the project used to have.**
Meaning: the host may have moved. Take it from the environments file or the user, never from memory or a pattern.

## Result looks stale

**A front-end change is not visible, though the env SHA contains the PR HEAD.**
Check the asset in the network tab: `cf-cache-status` (or the CDN's equivalent) and `age` on the URL the page loads, then `fetch(url + '?qa=' + Date.now(), {cache: 'reload'})`. A miss on the second and a hit with an old body on the first means the CDN serves a cached copy.
Meaning: the page tests the cache, not the PR. Record both observations. A static JS or CSS file of the diff loaded without a version or hash in its URL is a fact to report, with the file.

**Translation keys show raw on screen (`cart_load_error_title` instead of a sentence).**
Check what the server returns for the translation resource, with `{cache: 'no-store'}`.
- The server has the key: the browser cached the old file. Refetch with `{cache: 'reload'}`, reload the page, run the CUJ again.
- The server serves old texts: a server-side translation cache was not purged at deploy. Purging it changes the env: env-change rule in `SKILL.md` (section Forbidden).
Whether a stale cache is a release risk is not decided here: it goes through `Known decisions applied`, with its source, or into the report as observed.

**Images or downloadable files 404 on preprod or another non-production env.**
Check whether the env's storage is meant to carry production assets (environments file, the user).
Meaning: if it is not, a journey that needs them cannot be proven there: `BLOCKED`, cause `no data`, and propose another env. Dropping the point needs `Known decisions applied`.

## Tools

**chrome-devtools `get_css_styles` fails with `longhandValues.intersection is not a function`.**
Seen 2026-10-07 with chrome-devtools-mcp pinned in a project `.mcp.json`. Read the rules with `evaluate_script` instead (`getComputedStyle`, `document.styleSheets`). The tool is not `MISSING`: the CUJ runs.

**chrome-devtools tools are absent.**
See `SKILL.md`, section Preflight, "How to connect". A `chrome-devtools` server set at local scope wins over the project one in `.mcp.json`.

**Login fails on a test account that the user says is valid.**
Check whether the app delegates login to an SSO / OAuth provider. Accounts managed by the provider often have no local password, so a legacy email + password form rejects them. Use the SSO entry point.
