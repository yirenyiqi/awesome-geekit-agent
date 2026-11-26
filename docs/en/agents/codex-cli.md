# Codex CLI

Codex CLI is one of Geekit's recommended clients. Its custom provider uses OpenAI Responses for code understanding, editing, command execution, and long-running tasks.

Verification date: 2026-07-29

**Desktop management interface**

If you use the Geekit client and want a graphical interface for managing providers, sessions, context windows, and enhancements, see the instructions bundled with that client. Codex CLI users can continue with this page.

## 1. Configure persistent environment variables

Generate a key on the [API Keys](https://geekit.top/keys) page. Keep the environment variable name `GEEKIT_API_KEY` and replace its value with your real Geekit key. Prefer the latest model currently available in the [model catalog](https://geekit.top/pricing); this tutorial uses `gpt-6-astra`.

Add the following to `~/.zshrc` (Zsh) or `~/.bashrc` (Bash):

```bash
export GEEKIT_BASE_URL="https://geekit.top"
export GEEKIT_API_KEY="<your real Geekit API key>"
export GEEKIT_MODEL="gpt-6-astra"
```

Reopen your terminal or run `source ~/.zshrc` / `source ~/.bashrc`. In PowerShell, set persistent user environment variables:

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", "https://geekit.top", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", "<your real Geekit API key>", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", "gpt-6-astra", "User")
```

Close and reopen PowerShell afterward. `GEEKIT_BASE_URL` is the Geekit root address without `/v1`. If `gpt-6-astra` is unavailable to your account, update both the persistent `GEEKIT_MODEL` variable and the `model` setting below to a current ID from the catalog.

## 2. Install Codex CLI

If OpenAI's website is reachable, you can install or update using the [official Codex CLI instructions](https://developers.openai.com/codex/cli/).

**Install from Geekit's mirror**

If the official release source is unreachable, use Geekit's protected software download site. Download requests must include the `GEEKIT_API_KEY` configured in step 1. The installer selects the verified release for your platform and verifies signatures and SHA256 checksums.

macOS / Linux:

```bash
curl -fsSL \
  -H "Authorization: Bearer $GEEKIT_API_KEY" \
  https://s3.geekit.top/install/codex/install.sh | sh
```

Windows PowerShell:

```powershell
$installer = Join-Path ([IO.Path]::GetTempPath()) ("geekit-codex-" + [Guid]::NewGuid() + ".ps1")
try {
  Invoke-RestMethod `
    -Headers @{ Authorization = "Bearer $env:GEEKIT_API_KEY" } `
    -Uri "https://s3.geekit.top/install/codex/install.ps1" `
    -OutFile $installer `
    -MaximumRedirection 0
  & $installer
}
finally {
  Remove-Item -LiteralPath $installer -ErrorAction SilentlyContinue
}
```

This supports both installation and updates. Scripts and binaries are downloaded from `s3.geekit.top` without redirecting to overseas download sites. If a candidate has not passed all verification, the previous working release remains available.

Run `codex --version` after installation. Then configure the Geekit provider. An OpenAI login or OpenAI API key is not required when using Geekit.

Before configuring, also confirm:

- You have the Geekit root address, user API key, and a model ID that supports Responses.
- The model supports streaming events, tool calls, and enough context for your tasks.

Verify Responses:

```bash
curl "$GEEKIT_BASE_URL/v1/responses" \
  -H "Authorization: Bearer $GEEKIT_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"model\": \"$GEEKIT_MODEL\",
    \"input\": \"Reply only: Connection successful\"
  }"
```

## 3. Configure the provider

Edit the user-level `~/.codex/config.toml`:

```toml
model_provider = "geekit"
model = "gpt-6-astra"

[model_providers.geekit]
name = "Geekit"
base_url = "https://geekit.top/v1"
env_key = "GEEKIT_API_KEY"
wire_api = "responses"
```

Store only the environment variable name in this configuration, not the real key. `geekit` is a custom provider ID; do not overwrite a built-in Codex provider.

## 4. Start and choose a model

Start with the configured default model:

```bash
codex
```

Override the model for one session:

```bash
codex -m "$GEEKIT_MODEL"
```

Begin with a minimal read-only task:

```text
Read the README in the current directory and summarize the project's purpose. Do not modify any files.
```

## 5. Verify the connection

Check all of the following:

1. Codex receives complete or streamed responses.
2. It can read the workspace and invoke tools after authorization.
3. Geekit logs show requests to `/v1/responses`.
4. The logged model, tokens, and status match expectations.
5. If this client version performs server-side compaction, it does not encounter a `/v1/responses/compact` routing error. The absence of that request is not a failure.

If text succeeds but tool calls fail, first try a model explicitly supporting Responses tool calls.

## Troubleshooting

### Requests go to Chat Completions

Check `wire_api = "responses"`. Current Codex custom providers should use Responses; do not configure the removed Chat wire API.

### `401` responses

Ensure the variable name exactly matches `env_key`:

```bash
test -n "$GEEKIT_API_KEY" && echo "GEEKIT_API_KEY is set"
```

Do not print the variable's value.

### `404` responses

Set `base_url` to `https://geekit.top/v1`. Codex appends `/responses`; do not put the complete `/v1/responses` endpoint in `base_url`.

### Tool or streaming errors

Check that the model natively supports Responses. A Chat Completions-only model may return text without supporting Codex's events, reasoning, and tool workflow.

## WebSocket and server-side compaction

Custom providers use HTTP/SSE by default. Do not enable Responses WebSocket before testing compatibility with Geekit and the model. Current Codex custom providers may not call the remote `/v1/responses/compact` endpoint. Its presence is therefore not a condition for a successful connection; it serves clients explicitly implementing server-side compaction.

## Restore the official provider

Exit Codex and remove or comment out the top-level `model_provider = "geekit"` and custom default model selection. You may keep `[model_providers.geekit]` for later use. Then select the default provider using OpenAI's official login flow.

For Zsh / Bash, remove the `GEEKIT_BASE_URL`, `GEEKIT_API_KEY`, and `GEEKIT_MODEL` lines from your shell configuration and reopen the terminal. To clear them in the current terminal as well:

```bash
unset GEEKIT_BASE_URL
unset GEEKIT_API_KEY
unset GEEKIT_MODEL
```

For PowerShell, remove the persistent user variables:

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", $null, "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", $null, "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", $null, "User")
```

Reopen the terminal afterward.

## Official sources

- [Codex CLI installation and usage](https://developers.openai.com/codex/cli/)
- [Advanced Codex configuration](https://developers.openai.com/codex/config-file/config-advanced)
- [Basic Codex configuration](https://developers.openai.com/codex/config-file/config-basic)
- [Codex provider protocol implementation](https://github.com/openai/codex/blob/main/codex-rs/model-provider-info/src/lib.rs)
- [Codex configuration schema](https://github.com/openai/codex/blob/main/codex-rs/core/config.schema.json)
