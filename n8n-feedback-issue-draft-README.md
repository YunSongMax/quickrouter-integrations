# n8n 用户反馈生成 GitHub Issue 中文草稿

把合成反馈整理成可复制的中文 Issue 草稿。工作流没有 GitHub 凭据、GitHub 节点或发布请求。

## 使用方法

1. 在 n8n 导入 n8n-feedback-issue-draft-workflow.json。
2. 双击“QuickRouter 文字请求”，创建或选择 Header Auth 凭据：`Authorization` / `Bearer YOUR_QUICKROUTER_KEY`。不要把 Key 写进节点、截图或导出文件。
3. 保留合成样例，先手动执行一次。每次仅调用一次节点内指定的低成本文字模型，最多输出 600 tokens，不自动重试。
4. 检查“校验字段与结果”，再下载“导出待确认 CSV”。输出必须人工确认。
5. 更换资料前检查脱敏、业务字段、记录上限和校验规则。不要把样例中的固定断言直接用于其他业务输入。

工作流不包含密码、API Key、凭据 ID 或固定执行结果。实际消费和模型可用性以自己的控制台为准。CSV 对公式前缀做转义，示例不包含自动对外发布步骤。

原生实测完成后，结果与版本会单独记录在 n8n-feedback-issue-draft-receipt.json 和 n8n-feedback-issue-draft-actual-result.json。
