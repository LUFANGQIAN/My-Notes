# TermRepo 集词库 - 项目完整文档

------

# 📚 TermRepo 项目说明文档

## 目录

1. [项目概述](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#项目概述)
2. [产品架构](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#产品架构)
3. [功能特性](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#功能特性)
4. [商业模式](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#商业模式)
5. [许可证说明](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#许可证说明)
6. [快速开始](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#快速开始)
7. [技术栈](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#技术栈)
8. [项目结构](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#项目结构)
9. [开发指南](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#开发指南)
10. [部署指南](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#部署指南)
11. [FAQ](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#faq)
12. [贡献指南](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#贡献指南)
13. [联系方式](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#联系方式)

------

## 项目概述

### 🎯 项目愿景

**TermRepo 集词库** 是专为开发者设计的术语管理工具，帮助你在编码过程中高效收集、整理和复习专业术语，打破技术学习中的语言壁垒。

### 💡 核心理念

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│    选中单词 → 一键收藏 → 多端同步 → 智能复习 → 掌握术语          │
│                                                                 │
│    让每个开发者都能建立自己的技术术语知识库                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 📊 目标用户

| 用户群体             | 痛点             | TermRepo 解决方案        |
| -------------------- | ---------------- | ------------------------ |
| **非英语母语开发者** | 技术文档阅读困难 | 快速收藏生词，积累术语库 |
| **技术学习者**       | 术语分散难整理   | 统一管理，多端同步       |
| **团队开发者**       | 术语理解不一致   | 共享词库，统一认知       |
| **技术写作者**       | 术语翻译不统一   | 建立个人/团队术语标准    |

------

## 产品架构

### 整体架构图

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         TermRepo 生态系统                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐              │
│  │  VS Code     │    │  JetBrains   │    │  移动端      │              │
│  │  插件        │    │  插件        │    │  PWA         │              │
│  │              │    │              │    │              │              │
│  │  • 选词收藏  │    │  • 选词收藏  │    │  • 随时复习  │              │
│  │  • 右键菜单  │    │  • 右键菜单  │    │  • 离线访问  │              │
│  │  • 侧边栏    │    │  • 侧边栏    │    │  • 推送提醒  │              │
│  └──────┬───────┘    └──────┬───────┘    └──────┬───────┘              │
│         │                   │                   │                       │
│         └───────────────────┼───────────────────┘                       │
│                             │                                           │
│                             ▼                                           │
│              ┌──────────────────────────────┐                          │
│              │       同步服务层              │                          │
│              │  ┌────────────────────────┐  │                          │
│              │  │   用户认证 & 授权       │  │                          │
│              │  └────────────────────────┘  │                          │
│              │  ┌────────────────────────┐  │                          │
│              │  │   数据同步 API          │  │                          │
│              │  └────────────────────────┘  │                          │
│              │  ┌────────────────────────┐  │                          │
│              │  │   冲突解决服务          │  │                          │
│              │  └────────────────────────┘  │                          │
│              └──────────────┬───────────────┘                          │
│                             │                                           │
│                             ▼                                           │
│              ┌──────────────────────────────┐                          │
│              │       数据存储层              │                          │
│              │  ┌────────────┐ ┌──────────┐ │                          │
│              │  │ PostgreSQL │ │  Redis   │ │                          │
│              │  │  主数据库  │ │  缓存    │ │                          │
│              │  └────────────┘ └──────────┘ │                          │
│              │  ┌────────────┐ ┌──────────┐ │                          │
│              │  │  对象存储  │ │  搜索    │ │                          │
│              │  │  文件备份  │ │  Elastic │ │                          │
│              │  └────────────┘ └──────────┘ │                          │
│              └──────────────────────────────┘                          │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 组件说明

| 组件               | 仓库                 | 许可证       | 说明                   |
| ------------------ | -------------------- | ------------ | ---------------------- |
| **VS Code 插件**   | termrepo-vscode      | MIT          | 核心入口，代码编辑集成 |
| **JetBrains 插件** | termrepo-jetbrains   | MIT          | IDEA/PyCharm 等支持    |
| **移动端 PWA**     | termrepo-mobile      | AGPL+Commons | 随时复习，推送提醒     |
| **同步服务**       | termrepo-server      | AGPL+Commons | 数据同步，用户管理     |
| **术语市场**       | termrepo-marketplace | 商业闭源     | 社区术语分享平台       |

------

## 功能特性

### 📝 核心功能

#### 1. 一键收藏

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  在代码中选中单词 → 右键 → "收藏到 TermRepo"                     │
│                                                                 │
│  自动捕获：                                                      │
│  • 单词/术语本身                                                │
│  • 所在文件路径                                                 │
│  • 代码行号                                                     │
│  • 上下文代码片段                                               │
│  • 时间戳                                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 2. 智能整理

| 功能         | 说明                                       |
| ------------ | ------------------------------------------ |
| **自动分类** | 根据文件类型自动标记（前端/后端/数据库等） |
| **标签系统** | 自定义标签，多维分类                       |
| **翻译管理** | 手动/自动添加翻译，支持多语言              |
| **词频统计** | 显示术语出现频率，优先复习高频词           |

#### 3. 多端同步

```
┌─────────────────────────────────────────────────────────────────┐
│                    同步策略                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  本地存储 ←→ 云端同步 ←→ 多设备                                  │
│                                                                 │
│  • 实时同步：收藏后立即同步                                      │
│  • 离线可用：无网络时本地存储，恢复后自动同步                     │
│  • 冲突解决：最新修改优先，保留历史版本                          │
│  • 增量同步：仅同步变化数据，节省流量                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 4. 复习系统

| 模式           | 说明                          |
| -------------- | ----------------------------- |
| **闪卡模式**   | 正面术语，背面翻译，类似 Anki |
| **上下文复习** | 显示原始代码场景，加深理解    |
| **智能间隔**   | 根据记忆曲线安排复习时间      |
| **测试模式**   | 选择题/填空题，检验掌握程度   |

### 🚀 高级功能（官方云）

| 功能     | 开源版 | 官方云 |
| -------- | ------ | ------ |
| 基础同步 | ✅      | ✅      |
| 本地存储 | ✅      | ✅      |
| 多端同步 | ✅      | ✅      |
| 术语市场 | ❌      | ✅      |
| 自动备份 | ❌      | ✅      |
| 团队词库 | ❌      | ✅      |
| 协作编辑 | ❌      | ✅      |
| SLA 保障 | ❌      | ✅      |
| 优先支持 | ❌      | ✅      |

------

## 商业模式

### 双轨模式

```
┌─────────────────────────────────────────────────────────────────┐
│                    TermRepo 商业模式                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────┐   ┌─────────────────────────┐     │
│  │     开源版本 (免费)      │   │    官方云服务 (付费)     │     │
│  ├─────────────────────────┤   ├─────────────────────────┤     │
│  │                         │   │                         │     │
│  │  • VS Code 插件         │   │  • 所有开源功能          │     │
│  │  • 自建同步服务         │   │  • 术语市场访问          │     │
│  │  • 本地存储             │   │  • 自动备份 (每日)       │     │
│  │  • 移动端 PWA           │   │  • 团队管理 (5 人+)       │     │
│  │  • 基础同步功能         │   │  • 协作编辑              │     │
│  │  • 社区支持             │   │  • SLA 99.9%             │     │
│  │                         │   │  • 优先技术支持          │     │
│  │  完全免费               │   │  ¥19/月 或 ¥199/年       │     │
│  │                         │   │                         │     │
│  └─────────────────────────┘   └─────────────────────────┘     │
│                                                                 │
│  核心壁垒：服务质量和生态，而非代码本身                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 收入来源

| 来源         | 说明                  | 占比目标 |
| ------------ | --------------------- | -------- |
| **个人订阅** | ¥19/月，完整云功能    | 50%      |
| **团队订阅** | ¥99/月/5 人，协作功能 | 30%      |
| **企业授权** | 定制部署，私有化      | 15%      |
| **术语市场** | 优质内容分成          | 5%       |

------

## 许可证说明

### 双许可证策略

| 组件             | 许可证                   | 允许                 | 禁止                |
| ---------------- | ------------------------ | -------------------- | ------------------- |
| **VS Code 插件** | MIT                      | 商业使用、修改、分发 | 无                  |
| **同步服务**     | AGPL v3 + Commons Clause | 个人自用、内部使用   | 商业 SaaS、竞品服务 |
| **移动端 PWA**   | AGPL v3 + Commons Clause | 个人自用、内部使用   | 商业 SaaS、竞品服务 |
| **术语市场**     | 商业闭源                 | 官方运营             | 任何第三方使用      |

### 许可证原文

详细许可证文本请参见各仓库的 `LICENSE` 文件：

- [termrepo-vscode/LICENSE](https://github.com/termrepo/termrepo-vscode/blob/main/LICENSE) (MIT)
- [termrepo-server/LICENSE](https://github.com/termrepo/termrepo-server/blob/main/LICENSE) (AGPL+Commons)

### 商业授权

如需商业使用受限组件，请联系：**[license@termrepo.io](mailto:license@termrepo.io)**

------

## 快速开始

### 方式一：使用官方云服务（推荐新手）

```bash
# 1. 在 VS Code 中安装插件
# 扩展商店搜索 "TermRepo" 或访问：
# https://marketplace.visualstudio.com/items?itemName=termrepo.termrepo

# 2. 安装后打开命令面板 (Ctrl+Shift+P)
# 输入 "TermRepo: 配置订阅链接"

# 3. 访问 termrepo.io 注册账号获取订阅链接

# 4. 开始收藏单词！
```

### 方式二：自建服务（推荐高级用户）

```bash
# 1. 克隆仓库
git clone https://github.com/termrepo/termrepo-server.git
cd termrepo-server

# 2. 安装依赖
npm install

# 3. 配置环境变量
cp .env.example .env
# 编辑 .env 文件，配置数据库等

# 4. 启动服务
npm run build
npm start

# 5. 安装 VS Code 插件并配置本地服务地址
```

详细部署指南见 [部署指南](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#部署指南)

------

## 技术栈

### 前端（VS Code 插件）

| 技术        | 版本  | 用途         |
| ----------- | ----- | ------------ |
| TypeScript  | 5.0+  | 主要开发语言 |
| VS Code API | 1.85+ | 插件框架     |
| React       | 18+   | Webview UI   |
| SQLite      | -     | 本地存储     |

### 后端服务

| 技术           | 版本    | 用途      |
| -------------- | ------- | --------- |
| Node.js        | 18+ LTS | 运行时    |
| TypeScript     | 5.0+    | 开发语言  |
| Express/NestJS | 最新    | Web 框架  |
| PostgreSQL     | 15+     | 主数据库  |
| Redis          | 7+      | 缓存/会话 |
| Docker         | 最新    | 容器化    |

### 移动端

| 技术     | 版本 | 用途       |
| -------- | ---- | ---------- |
| PWA      | -    | 跨平台应用 |
| React    | 18+  | UI 框架    |
| Workbox  | -    | 离线缓存   |
| Push API | -    | 推送通知   |

### 基础设施

| 服务           | 用途       |
| -------------- | ---------- |
| GitHub Actions | CI/CD      |
| Docker Hub     | 镜像存储   |
| AWS/Aliyun     | 云服务部署 |
| Sentry         | 错误监控   |

------

## 项目结构

### 主仓库组织

```
termrepo/
├── termrepo-vscode/          # VS Code 插件
│   ├── src/
│   │   ├── extension.ts      # 入口文件
│   │   ├── commands/         # 命令处理
│   │   ├── providers/        # 视图提供者
│   │   ├── storage/          # 本地存储
│   │   └── sync/             # 同步服务
│   ├── package.json
│   ├── LICENSE               # MIT
│   └── README.md
│
├── termrepo-server/          # 同步服务后端
│   ├── src/
│   │   ├── api/              # API 路由
│   │   ├── services/         # 业务逻辑
│   │   ├── models/           # 数据模型
│   │   └── middleware/       # 中间件
│   ├── docker/
│   ├── package.json
│   ├── LICENSE               # AGPL+Commons
│   └── README.md
│
├── termrepo-mobile/          # 移动端 PWA
│   ├── src/
│   ├── public/
│   ├── package.json
│   ├── LICENSE               # AGPL+Commons
│   └── README.md
│
├── docs/                     # 项目文档
│   ├── user-guide/           # 用户指南
│   ├── developer-guide/      # 开发指南
│   └── api/                  # API 文档
│
└── README.md                 # 项目总览
```

------

## 开发指南

### 环境要求

| 工具       | 版本    | 必选   |
| ---------- | ------- | ------ |
| Node.js    | 18+ LTS | ✅      |
| npm/yarn   | 最新    | ✅      |
| Git        | 最新    | ✅      |
| VS Code    | 最新    | ✅      |
| Docker     | 最新    | ⚠️ 可选 |
| PostgreSQL | 15+     | ⚠️ 可选 |

### VS Code 插件开发

```bash
# 1. 克隆仓库
git clone https://github.com/termrepo/termrepo-vscode.git
cd termrepo-vscode

# 2. 安装依赖
npm install

# 3. 启动调试
npm run watch
# 在 VS Code 中按 F5 启动扩展开发主机

# 4. 运行测试
npm test

# 5. 打包
npm run package
# 输出：termrepo-0.1.0.vsix
```

### 后端服务开发

```bash
# 1. 克隆仓库
git clone https://github.com/termrepo/termrepo-server.git
cd termrepo-server

# 2. 安装依赖
npm install

# 3. 配置环境
cp .env.example .env
# 编辑 .env 配置数据库连接等

# 4. 启动开发服务器
npm run dev

# 5. 使用 Docker 启动完整环境
docker-compose up -d
```

### 代码规范

```typescript
// 遵循以下规范
- TypeScript 严格模式
- ESLint + Prettier 代码格式化
- 提交信息遵循 Conventional Commits
- 单元测试覆盖率 > 80%
```

------

## 部署指南

### 自建服务部署

#### 方式一：Docker Compose（推荐）

```yaml
# docker-compose.yml
version: '3.8'

services:
  termrepo-server:
    image: termrepo/server:latest
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/termrepo
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=your-secret-key
    depends_on:
      - db
      - redis

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=termrepo
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

#### 方式二：源码部署

```bash
# 1. 准备环境
# - Node.js 18+
# - PostgreSQL 15+
# - Redis 7+

# 2. 克隆代码
git clone https://github.com/termrepo/termrepo-server.git
cd termrepo-server

# 3. 安装依赖
npm install --production

# 4. 配置环境变量
cp .env.example .env
# 编辑 .env 文件

# 5. 数据库迁移
npm run db:migrate

# 6. 启动服务
npm start

# 7. 使用 PM2 管理（生产环境）
npm install -g pm2
pm2 start npm --name "termrepo-server" -- start
pm2 save
```

### 生产环境建议

| 项目           | 建议                         |
| -------------- | ---------------------------- |
| **HTTPS**      | 必须启用，使用 Let's Encrypt |
| **反向代理**   | Nginx/Caddy                  |
| **数据库备份** | 每日自动备份                 |
| **监控**       | Prometheus + Grafana         |
| **日志**       | 集中式日志管理               |
| **防火墙**     | 仅开放必要端口               |

------

## FAQ

### 常见问题

#### Q1: 自建服务和官方云有什么区别？

| 项目     | 自建服务 | 官方云   |
| -------- | -------- | -------- |
| 费用     | 免费     | ¥19/月起 |
| 数据控制 | 完全自主 | 托管     |
| 维护成本 | 自行维护 | 无需维护 |
| 高级功能 | ❌        | ✅        |
| 技术支持 | 社区     | 优先支持 |

#### Q2: 数据安全如何保障？

- 所有传输使用 HTTPS 加密
- 密码使用 bcrypt 哈希存储
- 支持端到端加密（开发中）
- 定期安全审计

#### Q3: 可以导出我的数据吗？

可以！支持导出为：

- JSON 格式
- CSV 格式
- Anki 卡片格式

#### Q4: 支持哪些编辑器？

| 编辑器         | 状态     |
| -------------- | -------- |
| VS Code        | ✅ 已发布 |
| JetBrains 系列 | 🚧 开发中 |
| Vim/Neovim     | 📋 计划中 |
| Sublime Text   | 📋 计划中 |

#### Q5: 如何贡献代码？

详见 [贡献指南](https://www.qianwen.com/chat/8cc92dd94aad47eebcedc3d448decd2b?ch=tongyi_redirect#贡献指南)

------

## 贡献指南

### 贡献方式

| 方式           | 说明                  |
| -------------- | --------------------- |
| **提交 Issue** | 报告 Bug 或提出新功能 |
| **提交 PR**    | 修复问题或添加功能    |
| **文档改进**   | 完善文档内容          |
| **翻译**       | 帮助国际化            |
| **社区帮助**   | 回答其他用户问题      |

### PR 流程

```
1. Fork 仓库
2. 创建分支 (feature/xxx 或 fix/xxx)
3. 提交代码 (遵循 Conventional Commits)
4. 运行测试 (npm test)
5. 提交 Pull Request
6. 等待 Code Review
7. 合并入主分支
```

### 提交信息规范

```bash
# 格式
<type>(<scope>): <subject>

# 示例
feat(sync): 添加增量同步功能
fix(plugin): 修复收藏单词崩溃问题
docs(readme): 更新快速开始指南
```

### 贡献者协议

所有贡献者需签署 CLA（Contributor License Agreement），确保项目对贡献代码有完整使用权。

------

## 联系方式

| 渠道         | 链接/地址                                          |
| ------------ | -------------------------------------------------- |
| **官网**     | [https://termrepo.io](https://termrepo.io/)        |
| **GitHub**   | https://github.com/termrepo                        |
| **问题反馈** | https://github.com/termrepo/termrepo-vscode/issues |
| **邮件**     | [support@termrepo.io](mailto:support@termrepo.io)  |
| **商业授权** | [license@termrepo.io](mailto:license@termrepo.io)  |
| **Twitter**  | @termrepo_io                                       |
| **Discord**  | https://discord.gg/termrepo                        |

------

## 更新日志

### v0.1.0 (2025-Q1)

- ✅ VS Code 插件发布
- ✅ 基础收藏功能
- ✅ 本地存储
- ✅ 自建服务部署

### v0.2.0 (2025-Q2) 计划

- 📋 云端同步服务
- 📋 移动端 PWA
- 📋 复习系统

### v1.0.0 (2025-Q3) 计划

- 📋 术语市场
- 📋 团队协作
- 📋 JetBrains 插件

------

## 致谢

感谢所有贡献者和用户的支持！

------

**© 2025 TermRepo. All rights reserved.**

------







