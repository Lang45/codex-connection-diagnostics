# Codex Connection Diagnostics

This repository contains one Codex skill for investigating repeated
`reconnecting` messages, request timeouts, and slow task startup on Windows.

The central diagnostic rule is to verify the provider attached to the failing
task. A provider change in `config.toml` can affect new tasks without migrating
an existing task. Restarting or resuming an old task is therefore not a valid
test of the new provider.

The skill also separates WebSocket reachability from ordinary HTTPS
reachability, explains HTTP fallback, and provides a controlled fresh-task and
fixed-node verification workflow.

For users in mainland China, a lawful and stable VPN or proxy route can make
Codex materially more convenient when the endpoints are not reliably reachable
directly. The skill treats that route as a testable transport path: it checks
the actual provider and distinguishes ordinary HTTPS reachability from
WebSocket support instead of assuming that a working web page proves Codex is
healthy.

## Install

Copy this repository directory into the Codex skills directory so that
`SKILL.md` is at the root of the installed skill folder. The exact directory
varies by installation; for a user-local install on Windows, use the configured
Codex skills directory and keep the folder name `codex-connection-diagnostics`.

The skill is designed to be implicitly discoverable and can also be invoked as
`$codex-connection-diagnostics`.

## Privacy

Do not publish logs containing API keys, bearer tokens, cookies, proxy
credentials, private endpoints, or identifying local paths. The repository
contains no user credentials or machine-specific connection details.

## Scope

This skill is a diagnostic guide. It does not automatically change proxy
settings, rewrite `config.toml`, or migrate existing Codex tasks.

