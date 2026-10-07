# cloudflare-cost-aware

写 Cloudflare Workers / D1 / KV / Queues / Durable Objects 代码时，让 AI agent 同时权衡**费用**和**性能**，给出量化的结论。

## 这是什么

一个 Claude Code / Cursor / Codex 通用的 skill。改动涉及 D1 表结构和索引、写入路径、cron 预计算、每请求读 KV、Queue 消息、DO 调用，或者讨论账单、用量告警、降本时，agent 会：

1. 按单价和真实频率算出每个方案的月费（D1 写比读贵约 1000 倍，是最常见的成本来源）；
2. 同时列出性能、数据新鲜度、主业务和安全规则时效的影响；
3. 按"主业务正确性不换钱、辅助功能先问使用频率、小钱不引入复杂度"给出结论；
4. 用 `rows_read` / `rows_written`、`wrangler d1 insights`、GraphQL Analytics 实测验证。

规则来自 api-gateway 的一次实际降本：本项目月费从约 $363 降到预计 $23～35，主功能不受影响。

## 安装

```bash
npx skills add pw-OpenCapsule/cloudflare-cost-aware -y -g
```

## 内容

- 单价与估算公式（按量计费部分，以官方定价和账单为准）
- 写代码时的核对项：D1 写、D1 读、cron 与预计算、KV、Queue / DO
- 权衡方式与结论原则
- 测量方法
- 评审清单
