# Open WebUI + QuickRouter: real text-client validation

Tested 2026-11-02, Open WebUI 0.12.5-slim. Exact image digest is in `openwebui-text-receipt.json`. Two native-UI text tests passed. This is a client configuration example, not an official preset or marketplace approval.

## Existing instance
1. Sign in to your own Open WebUI as an administrator.
2. Open **Admin Panel → Settings → Connections** (Chinese UI: 管理员面板 → 设置 → 外部连接).
3. Enable OpenAI API, add a connection with URL `https://api.quickrouter.ai/v1`, and enter your private QuickRouter API key. Save. Never share the key in screenshots or configuration downloads.
4. Select `gpt-4o-mini`. If the list is empty, check the connection/key and model access. A manually configured model ID must match the gateway ID exactly.
5. Disable automatic title, follow-up, tag and autocomplete generation for a bounded smoke test. The supplied `openwebui.env.example` contains equivalent environment names; persistent settings on existing installations may override environment changes.
6. Send `这是 QuickRouter 文本接入测试。请只回复：QR_OK`. Actual response: `QR_OK`.
7. Send `把工单“发票金额有误，请重新开票”分类。只返回 JSON，字段 category，值 billing、technical、other 三选一。`. Actual result contained `{"category":"billing"}` (wrapped in a JSON code block).

## Evidence
- [Connection URL screenshot](https://quickrouter.ai/images/ecosystem-20261102/openwebui-connection.jpg)
- [Native chat result](https://quickrouter.ai/images/ecosystem-20261102/openwebui-text-result.jpg)
- [Receipt](./openwebui-text-receipt.json), independently checked against the local client history. Provider token usage was not fully saved by this client; no billed-token total is claimed.

## Boundaries
Only text chat and Chinese classification were tested. This slim setup does not establish RAG, vision, tools, voice, video, or all-model compatibility. The template/configuration is free; QuickRouter text requests may be billed. Model output can vary; check semantics and JSON fields rather than exact whitespace.

401: check key and URL; 404: check `/v1` and model ID; 429: check quota and routing before a bounded retry; DNS/timeout: test connectivity from the container as well as the host.

Use the [official installation guide](https://docs.openwebui.com/getting-started/quick-start/) for fresh installations. The `.env.example` is an optional merge fragment, not a complete production deployment or security configuration.
