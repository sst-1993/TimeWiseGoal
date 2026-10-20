# TimeWiseGoal

**TimeWiseGoal** 是一个面向个人与团队的目标规划和执行平台，提供 Web、桌面端及 Flutter 客户端。项目围绕目标拆解、任务跟进与 AI 辅助创作，帮助用户把计划转化为可执行、可追踪的日常行动。

> 本仓库包含多个客户端和服务端配置。不同客户端的功能覆盖可能不同；完整运行需要配置自己的 Supabase 项目及所需的第三方服务。

## 功能概览

- **目标与任务管理**：目标树拆解、任务创建与跟进、进度查看和提醒。
- **AI 工作室**：AI 助手、内容生成及多模态创作能力；具体可用模型取决于服务端函数和 API 配置。
- **可视化工作流**：通过节点组织 AI 和多媒体处理步骤，并查看执行状态与结果。
- **社区功能**：论坛、社区活动、好友等互动模块。
- **个人工作台**：个人资料、通知、主题和多语言设置等。
- **多端支持**：React Web 应用、Tauri 桌面应用，以及 Flutter 移动应用；Flutter 项目也包含 Web 支持相关配置。

部分扩展模块依赖 Supabase Edge Functions、数据库迁移及第三方服务，克隆代码后不会自动获得生产环境能力。

## 技术栈

| 模块 | 技术 |
| --- | --- |
| Web | React 18、TypeScript、Vite、Tailwind CSS |
| 桌面端 | Tauri 2、Rust |
| 移动端 | Flutter、Dart、Provider、Material Design |
| 后端集成 | Supabase Auth、PostgreSQL、Realtime、Storage、Edge Functions |

## 仓库结构

```text
.
├── src/                 # React Web 应用
├── src-tauri/           # Tauri 桌面端壳与 Rust 代码
├── flutter_app/         # Flutter 客户端（Android、iOS、Web 等）
├── supabase/            # Edge Functions、数据库迁移及相关配置
├── lib_flutter/         # 独立的 Flutter 移动端分析 SDK
└── package.json         # Web / Tauri 项目脚本与依赖
```

## 快速开始：Web

### 环境要求

- Node.js 20 LTS 或更高版本
- npm
- 一个可用的 Supabase 项目

### 安装与启动

```bash
npm install
```

复制环境变量模板为本地配置文件：

```powershell
Copy-Item .env.example .env.local
```

在 `.env.local` 中至少填写：

```dotenv
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

然后启动开发服务器：

```bash
npm run dev
```

常用命令：

```bash
npm run build     # 构建 Web 应用
npm run preview   # 本地预览构建产物
npm run lint      # ESLint 检查
```

## Flutter 客户端

环境要求：Flutter SDK 与 Dart SDK（项目要求 Dart `>=3.0.0 <4.0.0`），以及目标平台对应的工具链。iOS 构建需要 macOS 和 Xcode。

```bash
cd flutter_app
flutter pub get
flutter run
```

运行特定平台：

```bash
flutter run -d chrome     # Web
flutter run -d android    # Android
flutter run -d ios        # iOS（macOS）
```

构建示例：

```bash
flutter build web --release
flutter build apk --release
flutter build appbundle --release
flutter build ios --release   # macOS + Xcode
```

Flutter 的 Supabase URL 和匿名公钥目前由 [supabase_config.dart](flutter_app/lib/config/supabase_config.dart) 提供。接入自己的 Supabase 项目时，请更新该配置；不要把 Supabase `service_role` 密钥或其他私密密钥放入客户端代码。

## 后端与第三方服务

Web 和 Flutter 客户端都依赖 Supabase。要在本地完整运行，除 Supabase URL 与 anon key 外，还需要按功能启用对应的数据库表、RLS 策略、Storage bucket 和 Edge Functions。相关迁移和函数位于 `supabase/`；请先在自己的 Supabase 项目中审阅并按项目部署流程应用，不要直接将生产数据库用于测试。

AI 生成、支付、对象存储、地图或其他集成可能需要额外的第三方账号和凭据。具体配置参考 `.env.example` 及 `supabase/functions/` 中各功能的说明。涉及服务器权限的密钥应仅保存为服务端或 Supabase Edge Function secrets；任何以 `VITE_` 开头的值都会进入前端构建，不能视为秘密。

## 桌面端开发

桌面端基于 Tauri 2。除 Node.js/npm 外，还需安装 Rust 工具链及目标系统的 Tauri 依赖。可使用项目脚本启动或构建：

```bash
npm run tauri:dev
npm run tauri:build
```

桌面端脚本当前使用类 Unix shell 语法；Windows 上请按 Tauri 官方文档配置 Rust、WebView2 和构建环境，并根据实际情况调整脚本执行方式。

## 贡献

欢迎通过 Issue 讨论问题或提交改进。建议贡献前先确认改动范围，并在 PR 中说明变更内容、验证方式以及涉及的数据库迁移或外部服务配置。涉及数据库的改动应包含相应迁移，并检查 RLS 与权限边界。

## 安全提示

- 不要提交 `.env.local`、访问令牌、服务角色密钥或生产环境凭据。
- 若发现密钥曾提交到版本库，即使文件已删除，也应在对应服务商处立即撤销并轮换。
- 对外部署前，请审核数据库 RLS、Edge Functions、CORS、支付回调与对象存储权限。

## 许可证

当前仓库未包含根目录 `LICENSE` 文件，因此尚未明确本项目的开源授权。请在发布或接受外部代码贡献前，由项目维护者选择并添加适用的许可证；仅有 README 或源代码公开不代表已授予复制、修改或再分发的权利。
