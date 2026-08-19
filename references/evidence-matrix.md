# Evidence Matrix

Use this reference after the initial baseline when several symptoms appear at
once. The entries are diagnostic interpretations, not commands to execute.

| Evidence | What it supports | What it does not prove |
| --- | --- | --- |
| `request timed out` followed by `reconnecting 2/5` | The current request or stream exceeded its timeout and the client retried | That the request used WebSocket rather than HTTP, or that the proxy is the cause |
| `Responses WebSocket timed out` plus HTTP reachability OK | Ordinary HTTPS is reachable while the WebSocket path is not completing | That all network paths or all providers are broken |
| Task settings show the built-in provider while config names a custom HTTP provider | The task was created or persisted with the old provider | That the new config is malformed or ineffective for new tasks |
| A new blank task shows the intended provider and completes | The new provider/config path works for that test | That an old task will be migrated automatically |
| Every pinned node fails WebSocket but HTTP remains reachable | The proxy service, upstream route, or provider transport is likely incompatible with WebSocket | That changing another unrelated local setting will fix it |
| One pinned node passes and another fails | Node or route quality/capability differs | That auto-select will consistently choose the passing node |
| Doctor reports degraded/HTTP fallback but the request eventually succeeds | The client has a usable fallback path | That the WebSocket retry delay has been eliminated |

## Minimal timeline

Use real local timestamps in private notes, but redact them from public issue
reports when they can identify the user or task:

```text
T1 config/provider changed
T2 failing task created
T3 failing task restored
T4 request timed out and retries began
T5 fresh task created after T1
T6 fresh task applied provider and completed
```

The key comparison is `T2` versus `T1`, followed by the applied provider at
`T5`. If `T2 < T1` and the old provider appears at `T3`, restarting did not
test the new configuration. If the intended provider appears at `T5` and the
request completes, task persistence was the root cause for the old task.

## Fresh-task acceptance check

Record these fields for the fresh task:

```text
created after provider/config change: yes
applied provider: intended provider
transport: expected HTTP or WebSocket path
request result: completed or failed
retry loop: absent or present
```

Do not publish raw event payloads if they contain authentication material or
identifying local paths.

