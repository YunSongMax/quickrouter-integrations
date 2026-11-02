# AnythingLLM 接入 QuickRouter：中文聊天配置与实测

实测：2026-11-02，AnythingLLM 1.19.0，gpt-4o-mini，max_tokens=256，stream=false。模板不含密钥；API 按实际用量计费。

1. 在已有 AnythingLLM 中打开设置 → 人工智能提供商 → 大语言模型（LLM），选择 Generic OpenAI。Docker 服务端必须能访问 QuickRouter。

2. Base URL 填 https://api.quickrouter.ai/v1；API Key 填自己的 QuickRouter Key；聊天模型填 gpt-4o-mini；上下文窗口填 4096，最大输出 tokens 填 256，禁用流式。保存。下载的 .env 是合并样例，不要覆盖已有密钥与部署配置；改环境变量后需重启自己的实例。

3. 创建或打开空文档工作区，点工作区齿轮 → 聊天设置，模式选择“聊天”。本版本默认“代理/自动”，不能把普通聊天测试等同于 Agent 测试。保存后新建聊天。

4. 先发送“接入测试：只回复 QR_OK，不要添加其他内容。”，收到 QR_OK 后，再发送下方中文合成工单；核对 category 为 billing，summary 保留原意。

## 本次第二个输入

把以下合成工单分为 billing、technical、account 或 other。只输出 JSON 对象，字段为 category 和 summary，summary 用中文，不要 Markdown，不调用工具。工单：测试订单发生重复扣款，请核对账单。

## 实际输出

```json
{
  "category": "billing",
  "summary": "测试订单发生重复扣款，请核对账单。"
}
```

## 验证范围与限制

本次在空文档工作区、普通聊天模式完成两个提示词，不上传资料、不创建向量、不调用工具。不据此宣称 RAG、Agent 或所有模型均兼容。

初次“代理/自动”模式尝试没有完成，已停止，没有计为成功。最终两次普通聊天通过；下载回执保留验证范围。

原生客户端记录两次成功对话共 274 tokens（93 + 181）；这是客户端报告值，未独立核对账单。4096 是该测试配置的上下文预算，256 是输出上限，两者不能相互替代。

界面可能把 JSON 引号渲染为排版引号。机器处理应使用复制/导出原始文本；下载的 actual-result.json 保留原生存储内容，不从截图识别。

[图文教程与自己的接入入口](https://quickrouter.ai/integrations/visit/anythingllm-quickrouter/github)
