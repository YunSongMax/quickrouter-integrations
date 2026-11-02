# Flowise 接入 QuickRouter：现有部署的文字链配置与实测

实测：2026-11-02，Flowise 3.1.4，gpt-4o-mini，max_tokens=256，stream=false。模板不含密钥；API 按实际用量计费。

1. 在现有 Flowise 的 Chatflows 中新建画布，Settings → Load Chatflow，选择本页下载的 flowise-text-chatflow.json。应出现“QuickRouter 文字模型”“中文输入提示”“输出文字结果”三个节点和中文便签。

2. 进入 Credentials → Add Credential → OpenAI API。Credential Name 自定，OpenAI Api Key 填自己的 QuickRouter Key 原文，不加 Bearer 前缀。返回画布，刷新后在模型的 Connect Credential 中选中该凭据。

3. Model Name 填 gpt-4o-mini，Temperature=0。打开 Additional Parameters：BasePath=https://api.quickrouter.ai/v1、Max Tokens=256、Timeout=30000、Streaming 关闭；BaseOptions 留空。保存画布。

4. 点击右上角 chat，先发送“接入测试：只回复 QR_OK，不要添加其他内容。”，收到 QR_OK 后，再发送下方合成工单。不要连续重试或连接其他工具。

## 本次第二个输入

只输出一个 JSON 对象，包含 category 和 summary。category 只能是 billing、bug、feature；summary 用中文，不超过 30 字。合成工单：测试订单发生重复扣款，请核对账单。

## 实际输出

```json
{
  "category": "billing",
  "summary": "测试订单重复扣款，请核对账单。"
}
```

## 验证范围与限制

官方已宣布停止维护。官网 sunset 页面说明 2026-07-29 冻结代码、2026-08-31 结束维护。本页保留给现有部署做兼容核对，不作为新生产系统的选型建议。

本次官方 Docker 3.1.4 默认 SQLite 启动遇到 this.db.exec is not a function；改用官方支持的 DATABASE_TYPE=postgres，在隔离 PostgreSQL 17 数据库完成验证，没有修改厂商登录或会话代码。新实例管理员密码由使用者自行设置。

公开工作流不含凭据、Key、聊天记录或固定回答。导入后需要自行创建凭据并选中。测试只覆盖 LLM Chain 文字输入输出；没有验证 Agent、RAG、工具或迁移后的生产稳定性。

原生聊天记录没有提供 token 统计，本页不按字符数估算账单。256 是每次输出上限，不是实际消费量；实际费用以 QuickRouter 账户日志为准。

[图文教程与自己的接入入口](https://quickrouter.ai/integrations/visit/flowise-quickrouter/github)
