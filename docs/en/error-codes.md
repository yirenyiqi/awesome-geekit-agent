# Error codes

Use the HTTP status, error body, and [usage logs](https://geekit.top/usage-logs) to diagnose Geekit API failures. The table lists common causes and remedies.

## HTTP status codes

| Status | Common cause | Remedy |
| --- | --- | --- |
| `400` Bad request | Invalid JSON, fields, model parameters, or protocol | Follow `error.message`, then check model IDs, paths, and parameter bounds |
| `401` Authentication failed | Missing, incorrect, expired, or disabled API key | Check the key on [API Keys](https://geekit.top/keys), update the authentication header, and restart the client |
| `402` Insufficient balance | An upstream API reports insufficient balance | Check the error and usage log, then contact support to investigate the upstream service |
| `403` Access restricted or insufficient quota | Model permissions, IP allowlists, groups, membership, or available quota prevent access | Check key restrictions, account and key quotas, and subscriptions, then top up in the [wallet](https://geekit.top/wallet) as needed |
| `404` Resource missing | Incorrect BaseURL, endpoint, or model ID, or a missing upstream resource | Check [First API call](getting-started.md) and copy the exact ID from the [model list](https://geekit.top/docs/en/api/models) |
| `408` Request timeout | Upload or request transmission takes too long | Check the connection and retry with a smaller file |
| `413` Request too large | A file, image, or request body exceeds a service limit | Compress files or split input and check the endpoint limits |
| `422` Invalid parameters | The upstream cannot process a parameter or field combination | Correct parameter types and model capability settings using the error message |
| `429` Rate limit reached | Request rate, concurrency, or upstream quota reaches its limit | Reduce concurrency, follow `Retry-After`, and apply exponential backoff |
| `500` Internal error | Geekit or an upstream encounters an internal failure | Check [service status](https://geekit.top/docs/en/support/status), retry later, and provide a request ID if it persists |
| `502` Gateway error | An upstream connection fails or returns an invalid response | Retry later and inspect the usage log for upstream details |
| `503` Temporarily unavailable | No available channel, upstream overload, or maintenance | Retry later, check service status, or select another available model |
| `504` Gateway timeout | An upstream exceeds the gateway response timeout | Reduce input or output length, use streaming, and check client timeouts |

Insufficient Geekit account or key quota usually returns `403`. Statuses including `402`, `408`, `413`, and `422` can also originate from upstream services or ingress proxies. Read the actual error body for details.

## Error response

An example error from an OpenAI-compatible endpoint follows.

```json
{
  "error": {
    "message": "Invalid request parameters",
    "type": "geekit_error",
    "param": "",
    "code": "invalid_request"
  }
}
```

`message` describes the failure, `type` identifies its type, and `code` is a machine-readable identifier. Anthropic and Gemini endpoints use their respective protocol error structures.

| Error identifier | What to check |
| --- | --- |
| `invalid_request`, `bad_request_body` | Request body, field types, and parameter bounds |
| `access_denied` | Model, key, IP, or group permissions |
| `insufficient_user_quota` | Account, subscription, and available quota |
| `pre_consume_token_quota_failed` | Key quota and estimated request consumption |
| `get_channel_failed`, `channel:no_available_key` | Channel availability for the selected model and group |
| `model_price_error` | Model billing configuration, with support assistance |
| `do_request_failed`, `bad_response_status_code` | Upstream connectivity and error responses |

## Retries and support

Use bounded exponential backoff for `429` and temporary `5xx` errors, honoring `Retry-After` when supplied. Correct authentication, permission, and parameter errors before retrying. After a stream interruption or task submission timeout, check usage logs or task status before sending another request.

Include the request time and timezone, model ID, endpoint, HTTP status, error identifier, and request ID when contacting support. Redact keys and private request data.

This page follows the organization of DeepSeek's error documentation and adds Geekit authentication, quota, and routing behavior. See [DeepSeek error codes](https://api-docs.deepseek.com/quick_start/error_codes/).
