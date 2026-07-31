# omp agent config (chezmoi-managed)

Synced (this dir):
- `config.yml` — settings / model roles / theme
- `models.yml` — custom providers (apiKey is OS-templated)
- `mcp.json` — MCP servers

Not synced (stay on each machine):
- `agent.db*` — OAuth / stored credentials
- `*.db*` — caches / history
- `sessions/`, `terminal-sessions/`
- `logs/`, `run/`, `puppeteer/`
- any local secret files

## Secrets

| OS | How |
|---|---|
| macOS | Keychain: `security add-generic-password -a "$USER" -s omp.longcat.api-key -w 'KEY' -U` |
| Windows | User env `LONGCAT_API_KEY` (System Properties → Environment Variables), one-time |
| Linux | User env `LONGCAT_API_KEY`, or swap the template branch to `secret-tool` / pass |

## Workflow

```bash
# after editing live files under ~/.omp/agent
chezmoi re-add ~/.omp/agent/config.yml
chezmoi re-add ~/.omp/agent/mcp.json
# models.yml is a template — edit the source:
chezmoi edit ~/.omp/agent/models.yml

chezmoi apply
git -C "$(chezmoi source-path)" add -A && git -C "$(chezmoi source-path)" commit -m "omp: …" && git -C "$(chezmoi source-path)" push
```
