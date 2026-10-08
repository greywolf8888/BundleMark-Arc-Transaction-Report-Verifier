<div align="center">
  <img src="apps/arc-task-ledger-web/public/brand/bundlemark-icon.png" width="112" alt="BundleMark 项目图标" />
  <h1>BundleMark</h1>
  <p><strong>Arc 交易报告核验器</strong></p>
  <p>让交易报告可以核对、导出和独立复算。</p>
  <p><a href="README.md">English</a> · 简体中文 · <a href="https://web--atl-web--xtd599t97njk.code.run/">在线使用</a> · <a href="docs/arc-task-ledger/settlement-verifier/openapi.json">OpenAPI</a></p>
  <img src="https://img.shields.io/badge/链访问-只读-b7ff00" alt="链访问只读" />
  <img src="https://img.shields.io/badge/界面-English%20%2F%20简体中文-24311b" alt="双语界面" />
  <img src="https://img.shields.io/badge/许可证-Apache--2.0-blue" alt="Apache 2.0" />
</div>

## 从交易到可复核报告

交易截图难以独立核对。BundleMark 接收 Arc 主网交易哈希或登记的浏览器链接，实际读取链数据，识别 USDC movement（资金转移），与用户填写的收款条件逐项比较。报告保留原始回执、证据位置、覆盖、来源、时间与规则版本。金额、资金边和证据可以联动检查。

1. 输入尚未保存的交易，点击 **Read transaction / 读取交易**。
2. 选择具体资金转移，填写预期收款人、金额或范围、可选付款人及 UTC 时间条件。
3. 检查匹配、不匹配、未知及每项依据；没有收款条件时只是观察报告。
4. 保存固定报告并下载原件；预览公开内容后，可明确发布不可撤回的公开版本。
5. 在另一个进程中复算原件或用于本地对账；在线重查是另一个明确操作。

**不需要连接钱包。** 不签名、不授权、不广播交易、不移动资金。

## 提供的能力

- **通用核验**：按指定交易查询，与既有 ArcBounty 任务入口并存。
- **准确金额**：整数字符串归一化至 18 位原子精度；native/ERC-20 镜像不重复计数；Gas、转移金额、收款人净变化分别展示。
- **逐项解释**：每项输出预期、实际值与证据；未知、来源不可用、过时与冲突不会变成 0。
- **报告与权限**：PostgreSQL 追加持久化、报告内容不可覆盖；默认私有，公开读取不授予管理权限。
- **本地复算**：浏览器本地文件或独立 Node.js 进程重新解析回执、核对摘要并复算；离线复算不认证主网真实性。
- **独立消费**：轻量 TypeScript 客户端、OpenAPI、SQLite 对账示例；同业务引用幂等，同资金转移不能重复分配。
- **双语与设备**：默认英文，显式切换简体中文并记住选择；状态、标题、日期随界面切换，原件协议标识保持原样。
- **实时观察**：有界刷新链状态；实时区块不等于任务实时到账，未归档观察不作为正式取证依据。

## 快速运行

需要 Node.js 24、npm 11；完整服务还需要专用 PostgreSQL 16。独立复算无需数据库或网络。

```sh
git clone git@github.com:greywolf8888/BundleMark-Arc-Transaction-Report-Verifier.git
cd BundleMark-Arc-Transaction-Report-Verifier
npm ci --ignore-scripts --no-audit --no-fund
npm run arc:build
npm run arc:replay -- /path/to/bundle.json
npm run arc:reconcile -- /path/to/bundle.json ./accounting.sqlite my_namespace invoice_001
```

从自己可访问的报告页面下载原件。旧主网原件描述历史观察，不代表当前链状态。完整服务请阅读 [运行说明](docs/project/OPERATIONS.zh-CN.md)，先配置角色、稳定密钥、同源网页，再增量迁移；不要删除旧数据库。

## 文档导航

- 使用：[中文用户指南](docs/arc-task-ledger/settlement-verifier/USER_GUIDE.zh.md) · [English guide](docs/arc-task-ledger/settlement-verifier/USER_GUIDE.en.md)
- 接口：[API 与错误/配额](docs/arc-task-ledger/settlement-verifier/API.md) · [OpenAPI](docs/arc-task-ledger/settlement-verifier/openapi.json) · [客户端](examples/arc-task-ledger/verifier-client.ts)
- 消费：[示例说明](examples/arc-task-ledger/README.zh-CN.md) · [复算](examples/arc-task-ledger/replay-bundle.ts) · [对账](examples/arc-task-ledger/reconcile.ts)
- 边界：[已知限制](docs/arc-task-ledger/settlement-verifier/KNOWN_LIMITATIONS.md) · [当前版本](docs/arc-task-ledger/settlement-verifier/CURRENT_RELEASE.md)
- 来源：[18 个原始独立提交](docs/project/HISTORY.json) · [图标原件摘要](docs/project/ORIGINAL_ASSETS.json) · [本轮交付](docs/project/DELIVERY.zh-CN.md)
- 协作：[贡献说明](CONTRIBUTING.md) · [安全报告](SECURITY.md) · [许可证](LICENSE) · [来源声明](NOTICE)

## 如何理解结果

`MATCHED` 表示所选资金转移满足冻结条件；`MISMATCHED` 表示有可判断的条件不满足；`INCONCLUSIVE` 表示证据不足；`UNSUPPORTED` 表示超出支持范围。HTTP 200、构建和测试不能替代匹配。匹配不证明订单交付、实体归属或完整取证结论。

私有权限依赖原浏览器七天会话。清除 Cookie、会话过期或遗失会失去管理权限，目前没有账号恢复。请及时导出。公开前预览完整内容：地址、金额、时间条件、原始链数据都会公开；移除私有 `contextRef` 不等于匿名化链上事实。公开固定版本不可撤回。

## 版本与来源

项目来自 ZeroTrace 独立 Arc 部署源码闭包，18 个原始提交按顺序保留原 SHA；没有导入 ZeroTrace 整个开发历史，Arc 构建依赖的共享 schema/证据包仍保留。新品牌只调整界面、元信息、源码链接和文档；资金规则、旧报告、固定快照和分页密钥沿用既有权威。

历史验证带日期和源码版本，不能算作新版本全部验收通过。本轮检查单独记录。不表示 Arc 官方认证、资助获批、真人审批或上游采用。默认数据采购预算为零，本仓库不代替用户提交资助申请。
