# LibreChat + QuickRouter: real text-client validation

Tested 2026-11-02, LibreChat v0.8.9-rc1, a release candidate. Exact tested container digest is recorded in `librechat-text-receipt.json`; verify your own version before production use. Two native-UI text tests passed.

## Existing instance
1. Keep your existing LibreChat installation and configuration. Back up `librechat.yaml` and `.env`.
2. Merge the single `endpoints.custom` item from the downloadable `librechat.yaml`. If `endpoints` or `custom` already exists, add the item to the existing mapping/list; do not create duplicate keys or overwrite unrelated settings.
3. In your private `.env`, set `QUICKROUTER_API_KEY=YOUR_KEY`. The downloaded YAML refers to this environment variable, never a real embedded key.
4. Restart LibreChat using your existing deployment method.
5. Open model selection, choose **QuickRouter → gpt-4o-mini**. The endpoint uses `https://api.quickrouter.ai/v1`, disables automatic titles and limits output to 128 tokens. No third-party model-vendor login is needed.
6. Send `这是 QuickRouter 文本接入测试。请只回复：QR_OK`. Actual response: `QR_OK`.
7. Send `把工单“发票金额有误，请重新开票”分类。只返回 JSON，字段 category，值 billing、technical、other 三选一。`. Actual response contained `{"category":"billing"}` in a JSON code block.

## Evidence
- [Model selection](https://quickrouter.ai/images/ecosystem-20261102/librechat-model-selection.jpg)
- [Native client result](https://quickrouter.ai/images/ecosystem-20261102/librechat-text-result.jpg)
- [Receipt](./librechat-text-receipt.json), independently checked against the local client database. Client-estimated token counts are not a verified gateway billing receipt.

## Limits and errors
Only text chat and Chinese classification were tested. Image, RAG, tool use, voice and video are untested. APIs may be billed. Output can vary; validate the classification.

Missing endpoint: check YAML indentation, startup validation and restart. 401: check the environment variable/key. 404: check Base URL and exact model ID. 429: check quota/channel availability before retry. DNS: check the container network. A 128-token cap can truncate longer business answers; increase deliberately after the smoke test.

[Official custom-endpoint configuration reference](https://www.librechat.ai/docs/configuration/librechat_yaml/object_structure/custom_endpoints).

## Separate upstream contribution
[LibreChat docs PR #904](https://github.com/LibreChat-AI/docs/pull/904) fixes a pre-existing Azure YAML example. It does not add this QuickRouter configuration and is not a provider endorsement. It is open, pending maintainer review/CI authorization.
