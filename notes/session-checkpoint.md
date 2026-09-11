# Current Goal
Help user set up `pi` as a local coding agent backed by a self-hosted vLLM (OpenAI-compatible) server; produce concrete configuration steps and any helper scripts (analysis-only; no source code changes).

# Current State
- Repo: `/home/poop/code/vendor/pi-mono` (git repo).
- Checkpoint file created at `notes/session-checkpoint.md` and was template-empty (now filled).
- Constraints (user): planning/analysis only; may run commands and create small helper scripts; do not modify source code.
- Read `README.md` + `AGENTS.md` (repo rules + package list).
- Read `packages/coding-agent/README.md` and `packages/ai/README.md` (vLLM is supported as “any OpenAI-compatible API” via baseUrl/compat settings).
- Note: repo is currently in OSS weekend mode (Apr 2, 2026 through Apr 13, 2026 per `README.md`).
- User has vLLM running at `http://127.0.0.1:8002` and `GET /v1/models` returns model id `omnicoder-9b`.
- Installed global pi: `npm install -g @mariozechner/pi-coding-agent` → `pi --version` reports `0.64.0`.
- Configured pi for vLLM:
  - `~/.pi/agent/models.json`: provider `vllm` → `baseUrl` `http://127.0.0.1:8002/v1`, `api` `openai-completions`, `apiKey` `dummy`, `authHeader` true, compat hardened (`supportsDeveloperRole` false, `supportsReasoningEffort` false, `supportsStrictMode` false, `supportsUsageInStreaming` false, `maxTokensField` `max_tokens`), model `omnicoder-9b` with `contextWindow` `262144`.
  - `~/.pi/agent/settings.json`: defaults to `vllm/omnicoder-9b`, `defaultThinkingLevel` `off`, compaction + retry tuned, enabledModels `vllm/*`.
  - `~/.pi/agent/AGENTS.md`: global context describing vLLM backend + safety notes.
- Added convenience launcher: `/home/poop/bin/omnicoder` runs `pi --model vllm/omnicoder-9b --tools read,grep,find,ls,bash,edit,write ...`.
- Added prompt templates: `~/.pi/agent/prompts/{review,context,fix}.md`.
- Added community skills repo (pi-skills): cloned to `~/.pi/agent/git/github.com/badlogic/pi-skills` and symlinked as `~/.pi/agent/skills/pi-skills` for discovery.

# Decisions
- Use checkpointing skill + keep this file current for session continuity.
- Follow repo rules: avoid dynamic imports, avoid `npm run dev/build/test`; run `npm run check` only if code changes happen (not expected for analysis-only).

# Open Problems
- If pi shows “Connection error”, verify vLLM is reachable from the same network namespace as pi (localhost vs container/WSL) and that vLLM is running with OpenAI tool-calling support if you want tool use.
- Optional: decide which third-party pi packages (extensions/themes) to install; none installed yet.

# Resume Instructions
- Verify model list: `pi --list-models vllm`.
- Start agent: `omnicoder` (or `pi` with defaults from `~/.pi/agent/settings.json`).
- If you want web browsing/search: set up `~/.pi/agent/skills/pi-skills/brave-search` (requires `BRAVE_API_KEY` + `npm install` in that directory).
