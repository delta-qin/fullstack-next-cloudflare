![Banner](banner.svg)

# ⚡ 全栈 Next.js + Cloudflare 模板

一个生产就绪的模板，用于构建基于 Next.js 15 和 Cloudflare 强大边缘基础设施的全栈应用程序。非常适合 MVP 项目，拥有慷慨的免费额度，并可无缝扩展到企业级应用。

**灵感来自 [Cloudflare SaaS Stack](https://github.com/supermemoryai/cloudflare-saas-stack)** - 与驱动 [Supermemory.ai](https://git.new/memory) 的技术栈相同，该应用以每月仅 5 美元的成本服务超过 2 万用户。本模板使用 Cloudflare Workers（而非 Pages）对该方法进行了现代化改造，包含全面的 D1 和 R2 示例，并提供完整的开发工作流。

你可以在 [Deepwiki](https://deepwiki.com/ifindev/fullstack-next-cloudflare) 上阅读 Devin AI 对本模板的详细说明和代码架构。

如果觉得有帮助，别忘了点个星 ⭐️


## 🌟 为什么选择 Cloudflare + Next.js？

**Cloudflare 边缘网络**提供无与伦比的性能和可靠性：
- ⚡ **超低延迟** - 部署到全球 300 多个位置
- 💰 **慷慨的免费额度** - 非常适合 MVP 和个人项目
- 📈 **轻松扩展** - 从零到数百万用户自动扩展
- 🔒 **内置安全** - DDoS 防护、WAF 等
- 🌍 **默认全球化** - 你的应用在每个用户附近运行

结合 **Next.js 15**，你可以获得现代 React 特性、服务器组件和服务器操作，以实现最佳性能和开发体验。

## 🛠️ 技术栈

### 🎯 **前端**
- ⚛️ **Next.js 15** - App Router 与 React 服务器组件（RSC）
- 🎨 **TailwindCSS 4** - 实用优先的 CSS 框架
- 📘 **TypeScript** - 全栈类型安全
- 🧩 **Shadcn UI** - 无样式、可访问的组件
- 📋 **React Hook Form + Zod** - 类型安全的表单处理

### ☁️ **后端和基础设施**
- 🌐 **Cloudflare Workers** - 无服务器边缘计算平台
- 🗃️ **Cloudflare D1** - 边缘分布式 SQLite 数据库
- 📦 **Cloudflare R2** - S3 兼容的对象存储
- 🤖 **Cloudflare Workers AI** - 边缘 AI 推理与开源模型
- 🔑 **Better Auth** - 现代身份验证与 Google OAuth
- 🛠️ **Drizzle ORM** - TypeScript 优先的数据库工具包

### 🚀 **DevOps 和部署**
- ⚙️ **GitHub Actions** - 自动化 CI/CD 流水线
- 🔧 **Wrangler** - Cloudflare 的 CLI 工具
- 👁️ **预览部署** - 在生产前测试更改
- 🔄 **数据库迁移** - 版本控制的模式变更
- 💾 **自动备份** - 生产数据库安全

### 📊 **数据流架构**
- **数据获取**：服务器操作 + React 服务器组件以实现最佳性能
- **数据变更**：带自动重新验证的服务器操作
- **AI 处理**：使用 Cloudflare Workers AI 的边缘 AI 推理
- **类型安全**：从数据库到 UI 的端到端 TypeScript
- **缓存**：内置 Next.js 缓存与 Cloudflare 边缘缓存

## 🏗️ 项目结构

本模板使用**基于特性/模块切片架构**以获得更好的可维护性和可扩展性：

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # 身份验证相关页面
│   ├── api/               # API 路由（用于外部访问）
│   │   └── summarize/     # AI 摘要端点
│   ├── dashboard/         # 仪表板页面
│   └── globals.css        # 全局样式
├── components/            # 共享 UI 组件
├── constants/             # 应用常量
├── db/                    # 数据库配置
│   ├── index.ts          # 数据库连接
│   └── schema.ts         # 数据库模式
├── lib/                   # 共享工具
├── modules/               # 功能模块
│   ├── auth/             # 身份验证模块
│   │   ├── actions/      # 身份验证服务器操作
│   │   ├── components/   # 身份验证组件
│   │   ├── hooks/        # 身份验证钩子
│   │   ├── models/       # 身份验证模型
│   │   ├── schemas/      # 身份验证模式
│   │   └── utils/        # 身份验证工具
│   ├── dashboard/        # 仪表板模块
│   └── todos/            # 待办事项模块
│       ├── actions/      # 待办事项服务器操作
│       ├── components/   # 待办事项组件
│       ├── models/       # 待办事项模型
│       └── schemas/      # 待办事项模式
├── services/              # 业务逻辑服务
│   └── summarizer.service.ts  # AI 摘要服务
└── drizzle/              # 数据库迁移
```

**主要架构优势：**
- **功能隔离** - 每个模块包含自己的操作、组件和逻辑
- **服务器操作** - 带自动重新验证的现代数据变更
- **React 服务器组件** - 通过服务器端渲染实现最佳性能
- **类型安全** - 从数据库到 UI 的端到端 TypeScript
- **可测试** - 清晰的关注点分离使测试更容易

## 🚀 快速开始

### 1. 前置要求

- **Cloudflare 账户** - [免费注册](https://dash.cloudflare.com/sign-up)
- **Node.js 20+** 和 **pnpm** 已安装
- **Google OAuth 应用** - 用于身份验证设置

### 2. 创建 Cloudflare API 令牌

创建用于 Wrangler 身份验证的 API 令牌：

1. 在 Cloudflare 仪表板中，转到 **账户 API 令牌**页面
2. 选择**创建令牌** > 找到**编辑 Cloudflare Workers** > 选择**使用模板**
3. 自定义你的令牌名称（例如，"Next.js Cloudflare 模板"）
4. 将令牌范围限定到你的账户和区域（如果使用自定义域名）
5. **添加额外权限**以访问 D1 数据库和 AI：
   - 账户 - D1:编辑
   - 账户 - D1:读取
   - 账户 - Cloudflare Workers AI:读取

**最终令牌权限：**
- "编辑 Cloudflare Workers" 模板的所有权限
- 账户 - D1:编辑（用于数据库操作）
- 账户 - D1:读取（用于数据库查询）
- 账户 - Cloudflare Workers AI:读取（用于 AI 推理）

### 3. 克隆和设置

```bash
# 克隆仓库
git clone https://github.com/ifindev/fullstack-next-cloudflare.git
cd fullstack-next-cloudflare

# 安装依赖
pnpm install
```

### 4. 环境配置

创建你的环境文件：

```bash
# 复制示例环境文件
cp .dev.vars.example .dev.vars
```

使用你的凭证编辑 `.dev.vars`：

```bash
# Cloudflare 配置
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_D1_TOKEN=your-api-token

# 身份验证密钥
BETTER_AUTH_SECRET=your-random-secret-here
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# 存储
CLOUDFLARE_R2_URL=your-r2-bucket-url
```

### 5. 身份验证设置

**Better Auth 密钥：**
```bash
# 生成随机密钥
openssl rand -base64 32
# 添加到 .dev.vars 中的 BETTER_AUTH_SECRET
```

**Google OAuth 设置：**
按照 [Better Auth Google 文档](https://www.better-auth.com/docs/authentication/google)：
1. 创建 Google OAuth 2.0 应用程序
2. 获取你的客户端 ID 和客户端密钥
3. 添加授权重定向 URI

### 6. 存储设置和 R2 URL 配置

**理解 R2 URL：**
Cloudflare R2 在开发和生产环境中有不同的 URL 行为：

- **开发环境**：R2 提供立即可用的公共 URL
- **生产环境**：R2 公共 URL 不适用于生产 - 你应该设置自定义域名

**步骤 1：创建 R2 存储桶**
```bash
# 为开发环境创建 R2 存储桶
wrangler r2 bucket create your-app-bucket-dev

# 为生产环境创建单独的存储桶
wrangler r2 bucket create your-app-bucket-prod
```

**步骤 2：获取开发环境的 R2 URL**

**启用公共开发 URL：**
1. 转到 Cloudflare 仪表板
2. 在侧边栏点击 "R2 对象存储"
3. 从列表中选择你的存储桶
4. 转到 "设置" 选项卡
5. 找到 "公共开发 URL" 部分
6. 点击 "启用公共 URL"
7. 复制显示的 URL（格式为：`https://pub-xxxxx.r2.dev`）

**URL 格式示例：**
```
https://pub-a1b2c3d4e5f6g7h8i9j0.r2.dev
```

**重要说明：**
- URL 由 Cloudflare 自动生成
- 不需要账户 ID - 所有信息都在提供的 URL 中
- 此 URL 仅用于开发（不适用于生产）

**步骤 3：将 R2 URL 添加到环境变量**
```bash
# 添加到你的 .dev.vars 文件（使用从仪表板复制的 URL）
CLOUDFLARE_R2_URL=https://pub-a1b2c3d4e5f6g7h8i9j0.r2.dev
```

**注意：**将示例 URL 替换为你从 R2 存储桶设置中复制的实际 URL。

**生产环境 - 自定义域名设置（必需）：**

⚠️ **重要**：默认的 R2 公共 URL 不应在生产环境中使用，因为它未针对性能进行优化，可能存在限制。

**为 R2 设置自定义域名：**
```bash
# 1. 转到 Cloudflare 仪表板 → R2 存储 → 你的存储桶 → 自定义域名
# 2. 点击 "连接域名" 并输入你想要的域名（例如，files.yourdomain.com）
# 3. 按照 Cloudflare 的指示更新你的 DNS 记录
# 4. 等待 SSL 证书颁发（通常需要几分钟）

# 你的生产 R2 URL 将是：
# https://files.yourdomain.com

# 将此添加到你的生产密钥：
echo "https://files.yourdomain.com" | wrangler secret put CLOUDFLARE_R2_URL
```

**R2 URL 总结：**
- **开发环境**：使用 R2 存储桶 "公共开发 URL" 设置中的 URL
- **生产环境**：必须使用自定义域名以获得更好的性能和可靠性

**R2 URL 工作原理：**
- **基础 URL**：从 R2 存储桶设置获取（格式：`https://pub-xxxxx.r2.dev`）
- **文件 URL**：`https://pub-xxxxx.r2.dev/{folder}/{file-name}.{extension}`
- **环境变量**：只有基础 URL 放入 `CLOUDFLARE_R2_URL`
- **代码**：完整的文件路径使用基础 URL 以编程方式构建

## 🛠️ 手动设置（详细）

如果你更喜欢手动设置所有内容或想要详细了解每个步骤，请遵循这个综合指南。

### 步骤 1：创建 Cloudflare 资源

**创建 D1 数据库：**
```bash
# 在边缘创建新的 SQLite 数据库
wrangler d1 create your-app-name

# 输出将显示：
# database_name = "your-app-name"
# database_id = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

**创建 R2 存储桶：**
```bash
# 创建对象存储桶
wrangler r2 bucket create your-app-bucket

# 列出存储桶以确认
wrangler r2 bucket list
```

### 步骤 2：配置 Wrangler

使用你的资源 ID 更新 `wrangler.jsonc`：

```jsonc
{
    "name": "your-app-name",
    "d1_databases": [
        {
            "binding": "DB",
            "database_name": "your-app-name",
            "database_id": "your-database-id-from-step-1",
            "migrations_dir": "./src/drizzle"
        }
    ],
    "r2_buckets": [
        {
            "bucket_name": "your-app-bucket",
            "binding": "FILES"
        }
    ],
    "ai": {
        "binding": "AI"
    }
}
```

### 步骤 3：设置身份验证

**生成 Better Auth 密钥：**
```bash
# macOS/Linux
openssl rand -base64 32

# Windows（PowerShell）
[System.Convert]::ToBase64String([System.Security.Cryptography.RandomNumberGenerator]::GetBytes(32))

# 或使用在线生成器：https://generate-secret.vercel.app/32
```

**配置 Google OAuth：**
1. 转到 [Google Cloud Console](https://console.cloud.google.com/)
2. 创建新项目或选择现有项目
3. 启用 Google+ API
4. 创建 OAuth 2.0 凭证
5. 添加授权重定向 URI：
   - `http://localhost:3000/api/auth/callback/google`（开发环境）
   - `https://your-app.your-subdomain.workers.dev/api/auth/callback/google`（生产环境）

### 步骤 4：环境配置

**创建本地环境文件：**
```bash
# .dev.vars 用于本地开发
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_D1_TOKEN=your-api-token
BETTER_AUTH_SECRET=your-generated-secret
GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-google-client-secret
# 从 R2 存储桶设置获取：R2 对象存储 → 你的存储桶 → 设置 → 公共开发 URL
CLOUDFLARE_R2_URL=https://pub-a1b2c3d4e5f6g7h8i9j0.r2.dev
```

**设置生产密钥：**
```bash
# 将每个密钥添加到 Cloudflare Workers
echo "your-secret-here" | wrangler secret put BETTER_AUTH_SECRET
echo "your-client-id" | wrangler secret put GOOGLE_CLIENT_ID
echo "your-client-secret" | wrangler secret put GOOGLE_CLIENT_SECRET
echo "your-r2-url" | wrangler secret put CLOUDFLARE_R2_URL
```

### 步骤 5：数据库设置

**生成 TypeScript 类型：**
```bash
# 为 TypeScript 生成 Cloudflare 绑定
pnpm run cf-typegen
```

**初始化数据库：**
```bash
# 从模式生成初始迁移
pnpm run db:generate

# 将迁移应用到本地数据库
pnpm run db:migrate:local

# 验证数据库结构
pnpm run db:inspect:local
```

**可选：填充示例数据**
```bash
# 创建并运行种子脚本
wrangler d1 execute next-cloudflare --local --command="
INSERT INTO todos (id, title, description, completed, created_at, updated_at) VALUES
('1', '欢迎使用你的应用', '这是一个示例待办事项', false, datetime('now'), datetime('now')),
('2', '设置身份验证', '配置 Google OAuth', true, datetime('now'), datetime('now'));
"
```

### 步骤 6：测试你的设置

**启动开发服务器：**
```bash
# 终端 1：启动 Wrangler（提供 D1 访问）
pnpm run wrangler:dev

# 终端 2：启动 Next.js（提供 HMR）
pnpm run dev

# 替代方案：单个命令（无 HMR）
pnpm run dev:cf
```

**验证一切正常：**
1. 打开 `http://localhost:3000`
2. 测试身份验证流程
3. 创建待办事项
4. 检查数据库：`pnpm run db:studio:local`

### 步骤 7：设置 GitHub Actions（可选）

**添加仓库密钥：**
转到你的 GitHub 仓库 → 设置 → 密钥并添加：

- `CLOUDFLARE_API_TOKEN` - 步骤 2 中的 API 令牌
- `CLOUDFLARE_ACCOUNT_ID` - 你的账户 ID
- `BETTER_AUTH_SECRET` - 你的身份验证密钥
- `GOOGLE_CLIENT_ID` - 你的 Google 客户端 ID
- `GOOGLE_CLIENT_SECRET` - 你的 Google 客户端密钥
- `CLOUDFLARE_R2_URL` - 你的 R2 存储桶 URL

**部署生产数据库：**
```bash
# 将迁移应用到生产环境
pnpm run db:migrate:prod

# 验证生产数据库
pnpm run db:inspect:prod
```

## 🔧 高级手动配置

### 自定义域名设置

**添加自定义域名：**
1. 转到 Cloudflare 仪表板 → Workers & Pages
2. 选择你的 worker → 设置 → 触发器
3. 点击 "添加自定义域名"
4. 输入你的域名（必须在你的 Cloudflare 账户中）

**更新 OAuth 重定向 URL：**
将你的自定义域名添加到 Google OAuth 设置：
- `https://yourdomain.com/api/auth/callback/google`

### 数据库优化

**添加索引以提高性能：**
```sql
-- 创建索引以提高查询性能
CREATE INDEX IF NOT EXISTS idx_todos_user_id ON todos(user_id);
CREATE INDEX IF NOT EXISTS idx_todos_created_at ON todos(created_at);
CREATE INDEX IF NOT EXISTS idx_todos_completed ON todos(completed);
```

**监控数据库性能：**
```bash
# 查看数据库洞察
wrangler d1 insights your-app-name --since 1h

# 导出数据以进行分析
wrangler d1 export your-app-name --output backup.sql
```

### R2 存储配置

**配置 CORS 以进行直接上传：**
```bash
# 创建 CORS 策略文件
echo '[
  {
    "AllowedOrigins": ["https://yourdomain.com", "http://localhost:3000"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
    "AllowedHeaders": ["*"],
    "ExposeHeaders": [],
    "MaxAgeSeconds": 3000
  }
]' > cors.json

# 应用 CORS 策略
wrangler r2 bucket cors put your-app-bucket --file cors.json
```

## 🏃‍♂️ 开发工作流

### 初始设置
```bash
# 1. 生成 Cloudflare 类型（在任何 wrangler.jsonc 更改后运行）
pnpm run cf-typegen

# 2. 应用数据库迁移
pnpm run db:migrate:local

# 3. 为 Cloudflare 构建应用程序
pnpm run build:cf
```

### 日常开发
```bash
# 终端 1：启动 Wrangler 以访问 D1 数据库
pnpm run wrangler:dev

# 终端 2：启动带 HMR 的 Next.js 开发服务器
pnpm run dev
```

**开发 URL：**
- 🌐 **带 HMR 的 Next.js**：`http://localhost:3000`（推荐）
- ⚙️ **Wrangler 开发服务器**：`http://localhost:8787`

### 替代开发选项
```bash
# 单个命令 - Cloudflare 运行时（无 HMR）
pnpm run dev:cf

# 使用远程 Cloudflare 资源测试
pnpm run dev:remote
```

## 📜 可用脚本

### **核心开发**
| 脚本 | 描述 |
|--------|-------------|
| `pnpm dev` | 启动带 HMR 的 Next.js |
| `pnpm run build:cf` | 为 Cloudflare Workers 构建 |
| `pnpm run wrangler:dev` | 启动 Wrangler 以访问本地 D1 |
| `pnpm run dev:cf` | 组合构建 + Cloudflare 开发服务器 |

### **数据库操作**
| 脚本 | 描述 |
|--------|-------------|
| `pnpm run db:generate` | 生成新迁移 |
| `pnpm run db:generate:named "migration_name"` | 生成命名迁移 |
| `pnpm run db:migrate:local` | 将迁移应用到本地 D1 |
| `pnpm run db:migrate:preview` | 将迁移应用到预览环境 |
| `pnpm run db:migrate:prod` | 将迁移应用到生产环境 |
| `pnpm run db:studio:local` | 为本地数据库打开 Drizzle Studio |
| `pnpm run db:inspect:local` | 列出本地数据库表 |
| `pnpm run db:reset:local` | 重置本地数据库 |

### **部署和生产**
| 脚本 | 描述 |
|--------|-------------|
| `pnpm run deploy` | 部署到生产环境 |
| `pnpm run deploy:preview` | 部署到预览环境 |
| `pnpm run cf-typegen` | 生成 Cloudflare TypeScript 类型 |
| `pnpm run cf:secret` | 向 Cloudflare Workers 添加密钥 |

### **开发顺序**

**首次设置：**
1. `pnpm run cf-typegen` - 生成类型
2. `pnpm run db:migrate:local` - 设置数据库
3. `pnpm run build:cf` - 构建应用程序

**日常开发：**
1. `pnpm run wrangler:dev` - 启动 D1 访问（终端 1）
2. `pnpm run dev` - 启动带 HMR 的 Next.js（终端 2）

**模式更改后：**
1. `pnpm run db:generate` - 生成迁移
2. `pnpm run db:migrate:local` - 应用到本地数据库

**wrangler.jsonc 更改后：**
1. `pnpm run cf-typegen` - 重新生成类型

## 🤖 AI 开发和测试

### 测试 AI API

**⚠️ 需要身份验证**：首先登录你的应用，然后测试 API。

**浏览器控制台（最简单）：**
1. 在 `http://localhost:3000` 登录
2. 打开 DevTools 控制台（F12）
3. 运行：
```javascript
fetch('/api/summarize', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  credentials: 'include',
  body: JSON.stringify({
    text: "要摘要的文本...",
    config: { maxLength: 100, style: "concise" }
  })
}).then(r => r.json()).then(console.log);
```

**cURL（带会话 cookie）：**
1. 首先在浏览器中登录
2. DevTools → 应用 → Cookies → 复制 `better-auth.session_token`
3. 在 cURL 中使用 cookie：
```bash
curl -X POST http://localhost:3000/api/summarize \
  -H "Content-Type: application/json" \
  -H "Cookie: better-auth.session_token=your-token-here" \
  -d '{"text": "你的文本在这里...", "config": {"maxLength": 100}}'
```

**Postman：**
1. 在浏览器中登录，从 DevTools 复制会话 cookie
2. 添加标头：`Cookie: better-auth.session_token=your-token-here`

**未经身份验证的请求响应：**
```json
{
  "success": false,
  "error": "需要身份验证",
  "data": null
}
```


### AI 服务架构

AI 集成遵循干净的基于服务的架构：

1. **API 路由**（`/api/summarize`）- 处理 HTTP 请求、身份验证和验证
2. **身份验证层** - 在处理请求之前验证用户会话
3. **SummarizerService** - 封装 AI 业务逻辑
4. **错误处理** - 带正确状态码的全面错误响应
5. **类型安全** - 完整的 TypeScript 支持和 Zod 验证

### AI 模型选项

Cloudflare Workers AI 支持各种模型：
- **@cf/meta/llama-3.2-1b-instruct** - 文本生成（当前）
- **@cf/meta/llama-3.2-3b-instruct** - 更强大的文本生成
- **@cf/meta/m2m100-1.2b** - 翻译
- **@cf/baai/bge-base-en-v1.5** - 文本嵌入
- **@cf/microsoft/resnet-50** - 图像分类

## 🔧 高级配置

### 数据库模式更改
```bash
# 1. 修改 src/db/schemas/ 中的模式文件
# 2. 生成迁移
pnpm run db:generate:named "add_user_table"
# 3. 应用到本地数据库
pnpm run db:migrate:local
# 4. 测试你的更改
# 5. 提交并部署（迁移自动运行）
```

### 添加新的 Cloudflare 资源
```bash
# 1. 使用新资源更新 wrangler.jsonc
# 2. 重新生成类型
pnpm run cf-typegen
# 3. 更新你的代码以使用新绑定
```

### 生产密钥管理
```bash
# 将密钥添加到生产环境
pnpm run cf:secret BETTER_AUTH_SECRET
pnpm run cf:secret GOOGLE_CLIENT_ID
pnpm run cf:secret GOOGLE_CLIENT_SECRET
```

## 📊 性能和监控

**内置可观察性：**
- ✅ Cloudflare Analytics（默认启用）
- ✅ 真实用户监控（RUM）
- ✅ 错误跟踪和日志记录
- ✅ 性能指标

**数据库监控：**
```bash
# 监控数据库性能
wrangler d1 insights next-cf-app

# 在 Cloudflare 仪表板中查看数据库指标
# 导航到 Workers & Pages → D1 → next-cf-app → 指标
```

## 🚀 部署

### 自动部署（推荐）

推送到 `main` 分支会通过 GitHub Actions 触发自动部署：

```bash
git add .
git commit -m "feat: 添加新功能"
git push origin main
```

**部署流程：**
1. ✅ 安装依赖
2. ✅ 构建应用程序
3. ✅ 运行数据库迁移
4. ✅ 部署到 Cloudflare Workers

### 手动部署

```bash
# 部署到生产环境
pnpm run deploy

# 部署到预览环境
pnpm run deploy:preview
```

## ✍️ 待办事项

### 🤖 AI 功能
- [ ] 使用 `@cf/meta/m2m100-1.2b` 添加文本翻译服务
- [ ] 使用 `@cf/baai/bge-base-en-v1.5` 实现文本嵌入以进行语义搜索
- [ ] 使用 `@cf/microsoft/resnet-50` 添加图像分类 API
- [ ] 创建带对话记忆的聊天/对话 API
- [ ] 使用 AI 分类添加内容审核
- [ ] 为用户反馈实现情感分析

### 💳 支付和通信
- [ ] 使用 [Resend](https://resend.com/) 和 [Cloudflare Email Routing](https://www.cloudflare.com/developer-platform/products/email-routing/) 实现电子邮件发送
- [ ] 使用 [Polar.sh](https://polar.sh/) 实现国际支付网关
- [ ] 使用 [Xendit](https://www.xendit.co/en-id/)、[Midtrans](https://midtrans.com/en) 或 [Duitku](https://www.duitku.com/) 实现印尼支付网关

### 📊 分析和性能
- [ ] 添加 Cloudflare Analytics 集成
- [ ] 实现自定义指标跟踪
- [ ] 添加性能监控仪表板
- [ ] 创建 AI 使用分析和成本跟踪



## 🤝 贡献

欢迎贡献！请随时提交问题和拉取请求。

## 📝 许可证

本项目根据 MIT 许可证授权 - 有关详细信息，请参阅 [LICENSE](LICENSE) 文件。

---

© 2025 Muhammad Arifin. 保留所有权利。


