# 运行 BundleMark

[English](OPERATIONS.en.md) · [首页](../../README.zh-CN.md)

## 独立复算

需要 Node.js 24、npm 11。执行 `npm ci --ignore-scripts --no-audit --no-fund`、`npm run arc:build`，再执行 `npm run arc:replay -- bundle.json`。复算无需网络或数据库；完整性/重算结果与主网真实性分别报告。

本地对账：`npm run arc:reconcile -- bundle.json accounting.sqlite namespace business_reference`。SQLite 是消费方自己的记账空间，不替代结算权威。仅 MATCHED 资金可分配；重复资金或冲突引用会被拒绝，不移动资金或自动发货。

## API 与网页

使用专用 PostgreSQL 16。旧库先备份并验证恢复。用 schema 所有者连接执行 `npm run arc:migrate`，目前增量迁移至 7；迁移连接不能作为运行时 API 连接。

通过 shell 或密钥管理器配置环境变量，应用不会自动加载 `.env`：

- `ARC_DATABASE_URL`：运行时只读角色，schema usage 和 SELECT。
- `ARC_REQUEST_DATABASE_URL`：受限请求/报告追加角色。权限应匹配 `verifier-storage.ts`、`verifier-session.ts` 及请求存储实际查询，不授予 schema owner 或超级用户权限；缺失时写请求不可用。
- `ARC_CURSOR_SECRET`：稳定秘密密钥，升级保持原值以延续分页；不能放进网页或原件。
- `ARC_PUBLIC_ORIGIN`：实际网页源，本地可为 `http://127.0.0.1:5178`；写请求受同源与 CSRF 约束。
- `ARC_API_PORT`：默认 8087；`ARC_API_HOST`：默认回环地址。
- 可选 `ARC_RPC_URL` / `ARC_PROVIDER_ALIAS`：使用已登记和版本化来源，不猜测接口或购买访问。

执行 `npm run arc:start`；另一个终端执行 `npm run dev -w @zerotrace/arc-task-ledger-web -- --host 127.0.0.1 --port 5178`。开发网页将 `/api` 代理至 8087，其他端口需设置 `ARC_API_PROXY`。真实交易采集有界、只读。旧任务采集是单独的 `npm run arc:worker` 进程，遵守显式扫描预算，离线复算无需启动它。

生产使用现有 Dockerfile 的 web target 与同源反代。继承的 Compose 模板引用旧导出目录，也没有配置新的请求写入角色，不能当作开箱即用的核验器部署。实际云配置以当前交付证据为准。保留卷、密钥与权限。

## 验证

独立检查包括 `arc:test:unit`、`arc:client:typecheck`、`lint`、`arc:build`、`license:check`。集成与浏览器检查需专用 `ARC_TEST_DATABASE_URL`，严禁指向生产。桌面/手机分别运行，遵守真实 API 配额；历史收据不是本轮检查。

私有权限依赖七天会话；公开原件版本不可撤回。会话丢失前导出，发布前检查完整预览。消费端保留未知和来源故障状态；详细边界见 [已知限制](../arc-task-ledger/settlement-verifier/KNOWN_LIMITATIONS.md)。
