# 万得金融数据查询（wind-mcp-skill）

通过本地 CLI 调用万得 Wind 金融数据服务，按数据域取数：股票、基金 / ETF、指数 / 板块、债券、公告与新闻、宏观与行业指标、跨标的聚合计算。

## 定位

让智能体在回答金融数据问题时优先取用结构化、可溯源的权威数据，而不是依赖模型记忆或通用网页信息。所有回答只基于接口返回值，不补常识、不补点评；成功取数后附数据来源声明。

## 特性

- **七大领域路由**：`stock_data`、`fund_data`、`index_data`、`bond_data`、`financial_docs`、`economic_data`、`analytics_data`，每个领域一份契约文档，参数名与字段值一律以契约为准。
- **标准化回执**：成功返回数据对象，失败返回带 `code` 的结构化错误信封（`AUTH_ERROR`、`PARAM_VALIDATION_ERROR`、`RATE_LIMIT_ERROR` 等），便于按错误类型精准修正。
- **批量与并发约束**：价格指标类工具单次最多 50 个代码；并发默认串行，显式要求时上限 10，遇限流立即降级。
- **可诊断**：内置 `diagnose`、`list-tools`、`open-portal`、`setup-key` 等子命令，密钥缺失时给出明确指引。
- **零构建依赖**：纯 Node.js ESM 脚本，无需安装第三方包。

## 快速开始

1. 环境：Node.js 18+（推荐 20 及以上）。

2. 配置 API Key（三选一，优先级从高到低）：

```bash
# 方式一：交互式写入配置
node scripts/cli.mjs setup-key YOUR_WIND_KEY --scope global

# 方式二：环境变量
export WIND_API_KEY=YOUR_WIND_KEY      # Windows PowerShell: $env:WIND_API_KEY="YOUR_WIND_KEY"

# 方式三：用户全局配置文件（默认路径 ~/.wind-aifinmarket/config）
```

3. 调用示例：

```bash
node scripts/cli.mjs call stock_data get_stock_price_indicators '{"windcode":"600519.SH"}'
node scripts/cli.mjs call index_data get_index_quote '{"windcode":"000300.SH"}'
node scripts/cli.mjs list-tools stock_data
```

4. 非 POSIX 环境（PowerShell / cmd）建议把 UTF-8 JSON 参数写入 `scripts/request-<后缀>.json`，以 `@scripts/request-<后缀>.json` 传入，调用后删除。

## 目录结构

```
wind-mcp-skill/
├── SKILL.md                    # 技能入口：定路由 → 发命令 → 读回执 → 收口
├── references/
│   ├── stock.md                # 股票契约
│   ├── fund.md                 # 基金契约
│   ├── index.md                # 指数契约
│   ├── bond.md                 # 债券契约
│   ├── financial-docs.md       # 公告与新闻契约
│   ├── economic.md             # 宏观 / 行业 / 汇率指标契约
│   ├── analytics.md            # 跨标的聚合契约
│   ├── stock-indicators.md     # 股票常用指标字段
│   ├── fund-indicators.md      # 基金常用指标字段
│   └── index-indicators.md     # 指数常用指标字段
├── scripts/
│   ├── cli.mjs                 # 主入口（call / list-tools / setup-key / diagnose）
│   ├── update-check.mjs        # 更新检查
│   ├── tool-manifest.json      # 工具清单
│   └── call-rules.json         # 调用规则
├── README.md
├── README_EN.md
├── LICENSE
└── .gitignore
```

> 运行时状态文件（如 `update-state.json`）与本地密钥配置不随仓库分发，已在 `.gitignore` 中忽略。

## License

MIT License，详见 [LICENSE](LICENSE)。

## 免责声明

本技能仅提供数据访问通道，不构成任何投资建议。数据版权归数据服务提供方所有，使用时请遵守其服务条款与授权范围；行情与财务数据可能存在延迟或口径差异，投资决策请自行判断并承担风险。请勿将 API Key 提交到版本库或公开分享。
---

## 打赏支持

如果这个技能帮你省下了时间，欢迎请我喝杯咖啡 ☕，支持持续维护更新：

<p align="center">
  <img src="donate-wechat.jpg" alt="微信收款码" width="200"/>
  <img src="donate-alipay.jpg" alt="支付宝收款码" width="200"/>
  <br/>
  <sub>微信 / 支付宝收款码（赞赏支持，请注明你的昵称）</sub>
</p>