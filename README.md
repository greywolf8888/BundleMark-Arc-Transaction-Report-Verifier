# Arc USDC 结算核验器

独立运行源码包，兼容既有ArcBounty任务。参见 docs/arc-task-ledger/settlement-verifier/USER_GUIDE.zh.md、CURRENT_RELEASE.md 与 VALIDATION.json。执行 npm ci、npm run arc:build，再配置既有PostgreSQL；本机原件复算 npm run arc:replay -- bundle.json，对账 npm run arc:reconcile -- bundle.json local.sqlite namespace business_reference。导出本身不证明上线、主网真实性或用户采用。
