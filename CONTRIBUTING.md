# Contributing

[English](CONTRIBUTING.md) | [简体中文](CONTRIBUTING.zh-CN.md)

Contribute a new Geekit integration, update client configuration, or report a reproducible compatibility issue.

## Add or update a guide

1. Fork the repository and create a branch.
2. Add matching Markdown files under `docs/en/agents/` and `docs/zh/agents/`, or update the existing pair.
3. Include the client version, platform, date checked, official installation link, and prerequisites.
4. Explain the protocol, BaseURL, API key storage, and exact model ID selection. Use placeholders for credentials.
5. Add first-run steps, expected endpoint, streaming and tool-call checks, and troubleshooting. Record which steps you verified and the result.
6. Link official sources and update both README tool directories and both Agent index pages.
7. Check relative links and configuration syntax, then open a pull request describing the change and verification.

## Report compatibility issues

Open an issue with the client version, operating system, model ID, protocol, HTTP status, redacted error body, and reproduction steps. Include a request ID when available. Redact credentials and private project data.

## Review and license

Keep instructions aligned with official client behavior and current Geekit endpoints. Document any BaseURL limitations. Contributions follow the repository's AGPL-3.0 license.
