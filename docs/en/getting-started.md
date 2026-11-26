# Make your first API call

Create a token in [API Keys](https://geekit.top/keys) and choose a model that supports Chat Completions in the [model marketplace](https://geekit.top/pricing/models). The example below sends it a single message.

## Set persistent environment variables

Add these lines to your personal device's `~/.zshrc` or `~/.bashrc`, substitute your token and model ID, and open a new terminal.

```bash
export GEEKIT_BASE_URL="https://geekit.top"
export GEEKIT_API_KEY="<your Geekit API key>"
export GEEKIT_MODEL="<model ID>"
```

In PowerShell, save the variables for your user account and open a new terminal.

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", "https://geekit.top", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", "<your Geekit API key>", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", "<model ID>", "User")
```

## Send a message

```bash
curl "$GEEKIT_BASE_URL/v1/chat/completions" \
  -H "Authorization: Bearer $GEEKIT_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"model\":\"$GEEKIT_MODEL\",\"messages\":[{\"role\":\"user\",\"content\":\"Hello!\"}]}"
```

In PowerShell, run:

```powershell
$body = @{ model = $env:GEEKIT_MODEL; messages = @(@{ role = "user"; content = "Hello!" }) } | ConvertTo-Json -Depth 4
Invoke-RestMethod "$env:GEEKIT_BASE_URL/v1/chat/completions" -Method Post -ContentType "application/json" -Headers @{ Authorization = "Bearer $env:GEEKIT_API_KEY" } -Body $body
```

Read the reply from `choices[0].message.content` and token counts from `usage`. If the model is unavailable, use the same token to [list available models](https://geekit.top/docs/en/api/models). For incremental output, see [Streaming and errors](https://geekit.top/docs/en/guides/streaming-and-errors).
