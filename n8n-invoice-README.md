# PDF invoice to validated CSV with QuickRouter

Tested 2026-11-02 with n8n 2.16.0. Creator submission #22204 is awaiting human review; this is not an approved n8n listing.

## Setup
1. Import `n8n-pdf-invoice-to-csv.workflow.json` into your n8n instance. Self-hosted Community Edition does not require a Cloud trial.
2. In n8n Credentials, create HTTP Header Auth. Header name: `Authorization`. Header value: `Bearer YOUR_QUICKROUTER_KEY` with one space after Bearer. Never put a real key into exported workflow JSON.
3. Select that credential in **QuickRouter text request**. Confirm `gemini-2.5-flash-lite` is available to your account and you have API credit. The template is free; API calls may be billed.
4. Keep the included synthetic invoice for the first run. Execute **Extract PDF text** alone to inspect the input without calling any model.
5. Execute the full workflow. It sends one bounded text request with no automatic retry, validates JSON, checks amounts with integer cents, and exports a formula-safe CSV.
6. Inspect **Validate result**, then **Export CSV → Download**. Expect PEN-BLUE: 3 × 2.50 = 7.50 and BOOK-A5: 2 × 4.00 = 8.00. Grand total: 15.50 USD. Actual text can vary; values must pass validation.

## Evidence
- [Native extraction screenshot](https://quickrouter.ai/images/ecosystem-20261102/n8n-pdf-extraction.jpg)
- [Imported native workflow](https://quickrouter.ai/images/ecosystem-20261102/n8n-workflow.jpg)
- [Real API execution receipt](./n8n-pdf-invoice-receipt.json): HTTP 200, 249 tokens, two rows.
- [Actual JSON](./n8n-pdf-invoice-result.json) and [native CSV](./n8n-pdf-invoice-result.csv), retrieved from the original complete API execution.

The final JSON hash in the receipt identifies the tested and submitted file. Local runtime credential binding and result-retention settings are private and absent from the download.

## Limits and errors
Selectable-text PDFs only. Scan/OCR, taxes, discounts and complex layouts require additional processing and verification. No accounting-system updates or payments are made. Review every real invoice.

- 401: check the Header Auth credential, including Bearer spacing.
- 429: check balance, model access and channel capacity; do not keep resending blindly.
- DNS/EAI_AGAIN: check the host/container DNS and connectivity. This happened in the local test environment; the complete API execution later succeeded.
- Invalid JSON or inconsistent totals: the workflow stops. Inspect the model result before retrying; another API request can cost tokens.

No video, image or audio model is used. See the [official n8n HTTP Request documentation](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/).
