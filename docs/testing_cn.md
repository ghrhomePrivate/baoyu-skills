# 测试策略

本仓库包含大量脚本，但它们不共享单一运行时或依赖图。风险最低的测试策略是从稳定的基于 Node 的库代码开始，然后向外扩展到 CLI 和技能级别的冒烟测试。

## 当前基线

- 根测试运行器：`node:test`
- 入口：`npm test`
- 覆盖率命令：`npm run test:coverage`
- CI 触发器：GitHub Actions，在 `push`、`pull_request` 和手动触发时运行

这避免了在一个已经混合了普通 Node 脚本、基于 Bun 的技能包、vendor 代码和浏览器自动化的仓库中引入 Jest/Vitest。

## 推进计划

### 阶段 1：稳定库覆盖

首先关注 `scripts/lib/` 下的纯函数。

- `scripts/lib/release-files.mjs`
- `scripts/lib/shared-skill-packages.mjs`

目标：

- 验证文件过滤和发布打包规则
- 捕获包 vendor 化和依赖重写中的回归
- 保持测试确定性，不依赖网络、Bun 或浏览器

### 阶段 2：根 CLI 集成测试

为已支持 dry-run 或纯本地流程的根 CLI 添加临时目录集成测试。

- `scripts/sync-shared-skill-packages.mjs`
- `scripts/publish-skill.mjs --dry-run`
- `scripts/sync-clawhub.mjs` 参数处理和本地技能发现

目标：

- 断言常见流程的退出码和标准输出
- 覆盖 CLI 参数解析，无需访问外部服务

### 阶段 3：技能脚本冒烟测试

为选定的 `skills/*/scripts/` 包添加可选的冒烟测试，优先选择：

- 接受本地输入文件的
- 有确定性输出的
- 不需要已认证浏览器会话的

示例：

- Markdown 转换
- 文件转换辅助工具
- 本地内容分析器

将浏览器自动化、登录流程和实时 API 发布脚本排除在默认 CI 路径之外，除非它们被显式 mock。

### 阶段 4：覆盖率门槛

在稳定的 Node 路径有足够广度后，为已测试的根模块在 CI 中添加覆盖率阈值。

推荐顺序：

1. 先仅做报告
2. 为 `scripts/lib/**` 添加行/函数阈值
3. 在技能级冒烟测试稳定后扩展包含模式

## 新测试约定

- 优先使用临时目录而非已提交的测试数据文件，除非测试数据被大量复用
- 先测试导出函数，再测试 CLI 封装
- 在默认 CI 中避免网络、浏览器和凭据依赖
- 保持测试隔离，确保可以用 `node --test` 直接运行
