# 项目架构指南

> 📖 **全栈 Next.js + Cloudflare 应用架构完全指南**  
> 本文档详细介绍项目的架构设计、代码组织、开发规范和部署流程，帮助新成员快速上手项目。

---

## 📑 目录

- [1. 项目概述](#1-项目概述)
- [2. 技术栈详解](#2-技术栈详解)
- [3. 项目结构](#3-项目结构)
- [4. 前后端调用链路](#4-前后端调用链路)
- [5. 模块化架构设计](#5-模块化架构设计)
- [6. 核心功能实现](#6-核心功能实现)
- [7. 开发工作流](#7-开发工作流)
- [8. 构建与部署](#8-构建与部署)
- [9. 添加新功能规范](#9-添加新功能规范)
- [10. 最佳实践](#10-最佳实践)

---

## 1. 项目概述

### 1.1 项目定位

这是一个基于 **Next.js 15** 和 **Cloudflare Workers** 的现代化全栈应用模板，采用边缘计算架构，具有以下特点：

- ⚡ **边缘优先**：应用运行在全球 300+ 个 Cloudflare 边缘节点
- 🚀 **高性能**：React Server Components (RSC) + Server Actions
- 💰 **低成本**：利用 Cloudflare 慷慨的免费额度
- 🔒 **类型安全**：端到端 TypeScript，从数据库到 UI
- 📦 **现代化**：采用最新的 Web 开发技术和最佳实践

### 1.2 核心特性

- **身份认证**：基于 Better Auth 的现代认证系统，支持邮箱密码和 OAuth（Google、GitHub）
- **数据持久化**：使用 Cloudflare D1（边缘 SQLite）+ Drizzle ORM
- **文件存储**：Cloudflare R2 对象存储（S3 兼容）
- **AI 能力**：Cloudflare Workers AI 边缘推理
- **自动部署**：GitHub Actions CI/CD 流水线

---

## 2. 技术栈详解

### 2.1 前端技术栈

| 技术 | 版本 | 用途 | 说明 |
|-----|------|------|------|
| **Next.js** | 15.4.6 | 全栈框架 | App Router + RSC + Server Actions |
| **React** | 19.1.0 | UI 库 | 最新的并发特性和 Server Components |
| **TypeScript** | 5.x | 类型系统 | 完整的类型安全保障 |
| **Tailwind CSS** | 4.x | 样式框架 | 现代化的实用优先 CSS |
| **Shadcn UI** | - | 组件库 | 基于 Radix UI 的可访问组件 |
| **React Hook Form** | 7.62.0 | 表单管理 | 高性能表单处理 |
| **Zod** | 4.1.8 | 数据验证 | 运行时类型验证和模式定义 |

### 2.2 后端与基础设施

| 技术 | 用途 | 说明 |
|-----|------|------|
| **Cloudflare Workers** | 边缘计算平台 | 无服务器函数，全球分布 |
| **Cloudflare D1** | 边缘数据库 | 分布式 SQLite 数据库 |
| **Cloudflare R2** | 对象存储 | S3 兼容的文件存储 |
| **Cloudflare Workers AI** | AI 推理 | 边缘 AI 模型推理 |
| **Drizzle ORM** | 数据库 ORM | TypeScript 优先的 ORM |
| **Better Auth** | 身份认证 | 现代化的认证解决方案 |

### 2.3 开发工具

| 工具 | 用途 |
|-----|------|
| **pnpm** | 包管理器 |
| **Wrangler** | Cloudflare CLI |
| **Drizzle Kit** | 数据库迁移工具 |
| **Biome** | 代码格式化和 Lint |
| **GitHub Actions** | CI/CD 自动化 |

---

## 3. 项目结构

### 3.1 整体目录结构

```
fullstack-next-cloudflare/
├── .github/                    # GitHub Actions 配置
│   └── workflows/
│       └── deploy.yml         # CI/CD 部署流水线
├── public/                     # 静态资源
├── src/                        # 源代码目录
│   ├── app/                   # Next.js App Router
│   │   ├── (auth)/           # 认证路由组（共享布局）
│   │   │   ├── layout.tsx    # 认证页面布局
│   │   │   ├── login/        # 登录页面
│   │   │   └── signup/       # 注册页面
│   │   ├── api/              # API 路由
│   │   │   ├── auth/         # Better Auth API 端点
│   │   │   └── summarize/    # AI 摘要 API
│   │   ├── dashboard/        # 仪表板路由
│   │   │   ├── layout.tsx    # 仪表板布局
│   │   │   ├── page.tsx      # 仪表板首页
│   │   │   └── todos/        # Todos 功能页面
│   │   ├── layout.tsx        # 根布局
│   │   ├── page.tsx          # 首页
│   │   └── globals.css       # 全局样式
│   ├── components/            # 共享 UI 组件
│   │   ├── ui/               # 基础 UI 组件（shadcn）
│   │   └── navigation.tsx    # 导航组件
│   ├── constants/             # 常量定义
│   │   └── validation.constant.ts
│   ├── db/                    # 数据库配置
│   │   ├── index.ts          # 数据库连接
│   │   └── schema.ts         # 数据库模式导出
│   ├── drizzle/              # 数据库迁移文件
│   │   ├── *.sql             # SQL 迁移文件
│   │   └── meta/             # 迁移元数据
│   ├── lib/                   # 通用工具库
│   │   ├── api-error.ts      # API 错误处理
│   │   ├── r2.ts             # R2 存储工具
│   │   └── utils.ts          # 通用工具函数
│   ├── modules/               # 功能模块（核心架构）
│   │   ├── auth/             # 认证模块
│   │   │   ├── actions/      # 服务器操作
│   │   │   ├── components/   # 认证相关组件
│   │   │   ├── hooks/        # 认证钩子
│   │   │   ├── models/       # 类型定义
│   │   │   ├── schemas/      # 数据库模式 + Zod 验证
│   │   │   ├── utils/        # 认证工具函数
│   │   │   ├── auth.route.ts # 路由定义
│   │   │   └── *.page.tsx    # 页面组件
│   │   ├── dashboard/        # 仪表板模块
│   │   └── todos/            # Todos 模块
│   │       ├── actions/      # 服务器操作
│   │       ├── components/   # Todos 组件
│   │       ├── models/       # 类型定义和枚举
│   │       ├── schemas/      # 数据库模式 + 验证
│   │       ├── todos.route.ts # 路由定义
│   │       └── *.page.tsx    # 页面组件
│   └── services/              # 业务逻辑服务层
│       └── summarizer.service.ts
├── .dev.vars                  # 本地环境变量（不提交）
├── biome.json                 # Biome 配置
├── drizzle.config.ts          # Drizzle 配置（远程）
├── drizzle.local.config.ts    # Drizzle 配置（本地）
├── middleware.ts              # Next.js 中间件（路由保护）
├── next.config.ts             # Next.js 配置
├── open-next.config.ts        # OpenNext Cloudflare 配置
├── package.json               # 项目依赖和脚本
├── tsconfig.json              # TypeScript 配置
├── wrangler.jsonc             # Cloudflare Workers 配置
└── worker-configuration.d.ts  # Worker 类型定义

```

### 3.2 关键目录说明

#### 3.2.1 `src/app/` - Next.js App Router

采用 Next.js 15 的 App Router 约定：

- **路由组** `(auth)`：用括号包裹，不影响 URL 路径，用于共享布局
- **动态路由** `[id]`：方括号表示动态参数
- **布局文件** `layout.tsx`：定义路由段的共享 UI
- **页面文件** `page.tsx`：路由的唯一 UI
- **API 路由** `api/*/route.ts`：API 端点处理器

#### 3.2.2 `src/modules/` - 功能模块（核心架构）

这是项目的核心架构，采用**模块化/功能切片**设计模式：

每个模块是一个独立的功能单元，包含该功能的所有相关代码：

```
module/
├── actions/        # Server Actions（数据获取和变更）
├── components/     # 模块专用组件
├── hooks/          # 模块专用 React Hooks
├── models/         # TypeScript 类型定义和枚举
├── schemas/        # 数据库模式（Drizzle）+ 验证模式（Zod）
├── utils/          # 模块工具函数
├── *.route.ts      # 路由定义（路径常量）
└── *.page.tsx      # 页面组件（与 app/ 中的页面对应）
```

**优势**：
- ✅ **高内聚低耦合**：相关代码集中在一起
- ✅ **易于维护**：修改功能只需关注一个模块
- ✅ **可扩展性强**：添加新功能就是添加新模块
- ✅ **代码复用**：模块间通过明确的接口通信

#### 3.2.3 `src/services/` - 业务逻辑层

独立的业务逻辑服务，不依赖于特定的 UI 框架：

- 封装复杂的业务逻辑
- 与外部服务交互（如 AI、第三方 API）
- 可被多个模块复用
- 便于单元测试

#### 3.2.4 `src/db/` - 数据库层

数据库相关的核心代码：

- `index.ts`：数据库连接工厂函数
- `schema.ts`：统一导出所有数据库模式

#### 3.2.5 `src/lib/` - 通用工具库

框架无关的通用工具函数：

- `api-error.ts`：统一的 API 错误处理
- `r2.ts`：R2 对象存储操作
- `utils.ts`：通用辅助函数（如 `cn` 用于 className 合并）

---

## 4. 前后端调用链路

### 4.1 数据流架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                         客户端（浏览器）                           │
├─────────────────────────────────────────────────────────────────┤
│  React Server Components (RSC)                                  │
│  └─ 直接调用 Server Actions 和 async 函数                        │
└────────────────────────┬────────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────────┐
│                   Cloudflare Edge Runtime                       │
├─────────────────────────────────────────────────────────────────┤
│  1. Middleware (middleware.ts)                                  │
│     └─ 路由保护、会话验证                                         │
│                                                                 │
│  2. Server Actions (modules/*/actions/*.action.ts)              │
│     └─ "use server" 标记的异步函数                               │
│     └─ 数据获取和变更逻辑                                         │
│                                                                 │
│  3. API Routes (app/api/*/route.ts)                            │
│     └─ RESTful API 端点（用于外部调用）                           │
│                                                                 │
│  4. Services (services/*.service.ts)                            │
│     └─ 业务逻辑封装                                              │
│                                                                 │
│  5. Auth Layer (modules/auth/utils/auth-utils.ts)              │
│     └─ 认证和授权检查                                            │
│                                                                 │
│  6. Database Layer (db/index.ts)                               │
│     └─ Drizzle ORM 数据库操作                                   │
└────────────────────────┬────────────────────────────────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
┌────────▼────────┐ ┌───▼─────┐ ┌──────▼────────┐
│  Cloudflare D1  │ │ CF R2   │ │ CF Workers AI │
│  (SQLite 数据库) │ │(对象存储)│ │  (AI 推理)     │
└─────────────────┘ └─────────┘ └───────────────┘
```

### 4.2 典型调用链路示例

#### 4.2.1 页面渲染流程（RSC）

**场景**：用户访问 `/dashboard/todos` 查看待办事项列表

```typescript
// 1. 用户访问 URL
GET /dashboard/todos

// 2. Middleware 拦截请求
// middleware.ts
middleware() 
  → 验证会话
  → 会话有效 → 继续
  → 会话无效 → 重定向到 /login

// 3. Next.js 渲染页面组件
// app/dashboard/todos/page.tsx (路由文件)
export default function Page() {
  return <TodoListPage />  // 调用模块页面组件
}

// 4. 页面组件执行 Server Action
// modules/todos/todo-list.page.tsx
export default async function TodoListPage() {
  const todos = await getAllTodos()  // 直接调用 Server Action
  return <div>{todos.map(...)}</div>
}

// 5. Server Action 执行数据库查询
// modules/todos/actions/get-todos.action.ts
"use server"
export default async function getAllTodos() {
  const user = await requireAuth()        // 验证用户身份
  const db = await getDb()               // 获取数据库连接
  const data = await db
    .select({...})
    .from(todos)
    .where(eq(todos.userId, user.id))   // 查询用户的 todos
  return data
}

// 6. 数据库连接
// db/index.ts
export async function getDb() {
  const { env } = await getCloudflareContext()
  return drizzle(env.next_cf_app, { schema })  // 返回 Drizzle 实例
}

// 7. 返回数据并渲染
HTML 响应发送给客户端
```

**关键点**：
- ✅ **零客户端 JavaScript**：数据获取在服务器端完成
- ✅ **自动序列化**：Server Action 返回的数据自动序列化
- ✅ **流式渲染**：支持 Suspense 和流式传输
- ✅ **类型安全**：端到端 TypeScript 类型推导

#### 4.2.2 数据变更流程（Server Actions）

**场景**：用户创建新的待办事项

```typescript
// 1. 用户在客户端填写表单
// modules/todos/components/todo-form.tsx
<form onSubmit={handleSubmit(onSubmit)}>
  <input {...register("title")} />
  <button type="submit">创建</button>
</form>

// 2. 表单提交处理
const onSubmit = async (data: TodoFormData) => {
  const result = await createTodo(data)  // 调用 Server Action
  
  if (result.success) {
    toast.success(t("todos.createSuccess"))
    router.push(todosRoutes.list)
  } else {
    toast.error(result.error)
  }
}

// 3. Server Action 执行
// modules/todos/actions/create-todo.action.ts
"use server"
export async function createTodo(data: TodoFormData) {
  // 3.1 验证用户
  const user = await requireAuth()
  
  // 3.2 验证数据
  const validatedData = insertTodoSchema.parse({
    ...data,
    userId: user.id,
  })
  
  // 3.3 数据库操作
  const db = await getDb()
  const [newTodo] = await db
    .insert(todos)
    .values(validatedData)
    .returning()
  
  // 3.4 重新验证缓存
  revalidatePath(todosRoutes.list)
  
  // 3.5 返回结果
  return { success: true, data: newTodo }
}
```

**关键点**：
- ✅ **安全性**：所有验证在服务器端进行
- ✅ **自动重新验证**：`revalidatePath` 刷新相关页面缓存
- ✅ **错误处理**：统一的错误处理和用户友好的错误消息
- ✅ **类型安全**：Zod 验证 + TypeScript 类型

#### 4.2.3 API 路由调用流程（外部访问）

**场景**：调用 AI 摘要 API

```typescript
// 1. 客户端发起请求
fetch('/api/summarize', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  credentials: 'include',  // 包含认证 cookie
  body: JSON.stringify({
    text: "要摘要的文本...",
    config: { maxLength: 100, style: "concise" }
  })
})

// 2. API Route Handler 处理请求
// app/api/summarize/route.ts
export async function POST(request: Request) {
  // 2.1 验证身份
  const auth = await getAuthInstance()
  const session = await auth.api.getSession({ headers: await headers() })
  if (!session?.user) {
    return new Response(JSON.stringify({ error: "Authentication required" }), 
      { status: 401 })
  }
  
  // 2.2 获取 Cloudflare 环境
  const { env } = await getCloudflareContext()
  
  // 2.3 验证请求数据
  const body = await request.json()
  const validated = summarizeRequestSchema.parse(body)
  
  // 2.4 调用服务层
  const summarizerService = new SummarizerService(env.AI)
  const result = await summarizerService.summarize(
    validated.text,
    validated.config
  )
  
  // 2.5 返回响应
  return new Response(
    JSON.stringify({ success: true, data: result }),
    { status: 200, headers: { 'Content-Type': 'application/json' } }
  )
}

// 3. 服务层执行业务逻辑
// services/summarizer.service.ts
export class SummarizerService {
  async summarize(text: string, config?: SummarizerConfig) {
    // 3.1 构建提示词
    const systemPrompt = this.buildSystemPrompt(...)
    
    // 3.2 调用 Cloudflare Workers AI
    const response = await this.ai.run("@cf/meta/llama-3.2-1b-instruct", {
      messages: [
        { role: "system", content: systemPrompt },
        { role: "user", content: `Please summarize: ${text}` }
      ]
    })
    
    // 3.3 返回结果
    return {
      summary: response.response,
      originalLength: text.length,
      summaryLength: response.response.length,
      tokensUsed: { input: ..., output: ... }
    }
  }
}
```

**关键点**：
- ✅ **RESTful 设计**：标准的 HTTP 方法和状态码
- ✅ **认证保护**：所有 API 都需要身份验证
- ✅ **请求验证**：Zod schema 验证请求体
- ✅ **服务层抽象**：业务逻辑封装在服务类中

### 4.3 认证流程

```typescript
// 1. 用户登录
// modules/auth/components/login-form.tsx
const onSubmit = async (data: SignInFormData) => {
  const result = await signIn(data)  // Server Action
  if (result.success) {
    router.push(dashboardRoutes.home)
  }
}

// 2. Server Action 处理登录
// modules/auth/actions/auth.action.ts
"use server"
export async function signIn({ email, password }) {
  const auth = await getAuthInstance()
  await auth.api.signInEmail({
    body: { email, password }
  })
  return { success: true }
}

// 3. Better Auth 验证凭据并创建会话
// modules/auth/utils/auth-utils.ts
async function getAuth() {
  const { env } = await getCloudflareContext()
  const db = await getDb()
  
  return betterAuth({
    secret: env.BETTER_AUTH_SECRET,
    database: drizzleAdapter(db, { provider: "sqlite" }),
    emailAndPassword: { enabled: true },
    socialProviders: {
      google: { ... },
      github: { ... }
    }
  })
}

// 4. 会话存储在 Cookie 中
// Better Auth 自动管理会话 cookie

// 5. 后续请求验证会话
// middleware.ts
export async function middleware(request: NextRequest) {
  const auth = await getAuth()
  const session = await auth.api.getSession({
    headers: request.headers
  })
  
  if (!session) {
    return NextResponse.redirect(new URL("/login", request.url))
  }
  
  return NextResponse.next()
}
```

---

## 5. 模块化架构设计

### 5.1 模块结构规范

每个功能模块遵循统一的目录结构：

```
modules/[feature-name]/
├── actions/                    # Server Actions（必需）
│   ├── create-*.action.ts     # 创建操作
│   ├── get-*.action.ts        # 查询操作
│   ├── update-*.action.ts     # 更新操作
│   └── delete-*.action.ts     # 删除操作
├── components/                 # 模块组件（必需）
│   ├── *-form.tsx             # 表单组件
│   ├── *-card.tsx             # 卡片组件
│   └── *-list.tsx             # 列表组件
├── hooks/                      # React Hooks（可选）
│   └── use-*.ts               # 自定义 hooks
├── models/                     # 类型定义（必需）
│   ├── *.model.ts             # 业务模型类型
│   └── *.enum.ts              # 枚举定义
├── schemas/                    # 数据模式（必需）
│   ├── *.schema.ts            # Drizzle 数据库模式 + Zod 验证
│   └── *.validation.ts        # 额外的验证规则
├── utils/                      # 工具函数（可选）
│   └── *-utils.ts             # 模块特定工具
├── [feature].route.ts          # 路由定义（必需）
├── [feature].layout.tsx        # 布局组件（可选）
└── *.page.tsx                  # 页面组件（必需）
```

### 5.2 模块示例：Todos 模块

#### 5.2.1 文件结构

```
modules/todos/
├── actions/
│   ├── create-todo.action.ts
│   ├── get-todos.action.ts
│   ├── get-todo-by-id.action.ts
│   ├── update-todo.action.ts
│   ├── delete-todo.action.ts
│   ├── create-category.action.ts
│   └── get-categories.action.ts
├── components/
│   ├── todo-form.tsx           # 表单组件
│   ├── todo-card.tsx           # 单个 todo 卡片
│   ├── delete-todo.tsx         # 删除确认对话框
│   ├── toggle-complete.tsx     # 完成状态切换
│   └── add-category.tsx        # 添加分类
├── models/
│   └── todo.enum.ts            # TodoStatus, TodoPriority 枚举
├── schemas/
│   ├── todo.schema.ts          # todos 表定义 + Zod 验证
│   └── category.schema.ts      # categories 表定义
├── todos.route.ts              # 路由常量
├── todo-list.page.tsx          # 列表页面
├── new-todo.page.tsx           # 创建页面
└── edit-todo.page.tsx          # 编辑页面
```

#### 5.2.2 核心代码示例

**1. 数据库模式定义**

```typescript
// modules/todos/schemas/todo.schema.ts
import { sqliteTable, text, integer } from "drizzle-orm/sqlite-core"
import { createInsertSchema, createSelectSchema } from "drizzle-zod"
import { z } from "zod"

// Drizzle 表定义
export const todos = sqliteTable("todos", {
  id: integer("id").primaryKey({ autoIncrement: true }),
  title: text("title").notNull(),
  description: text("description"),
  userId: text("user_id").notNull().references(() => user.id),
  status: text("status").$type<TodoStatusType>().notNull(),
  completed: integer("completed", { mode: "boolean" }).default(false),
  createdAt: text("created_at").notNull().$defaultFn(() => new Date().toISOString()),
})

// Zod 验证模式（自动生成 + 自定义验证）
export const insertTodoSchema = createInsertSchema(todos, {
  title: z.string().min(3, "标题至少 3 个字符").max(255, "标题最多 255 个字符"),
  description: z.string().max(1000, "描述最多 1000 个字符").optional(),
})

export const updateTodoSchema = insertTodoSchema.partial().omit({
  id: true,
  userId: true,
})

// TypeScript 类型（从模式推导）
export type Todo = typeof todos.$inferSelect
export type NewTodo = typeof todos.$inferInsert
```

**关键点**：
- ✅ **单一真相源**：数据库模式即文档
- ✅ **自动类型生成**：从 Drizzle 模式推导 TS 类型
- ✅ **运行时验证**：Zod schema 用于验证输入数据
- ✅ **开发体验**：修改模式自动更新类型和验证

**2. Server Actions**

```typescript
// modules/todos/actions/create-todo.action.ts
"use server"

import { revalidatePath } from "next/cache"
import { getDb, todos } from "@/db"
import { requireAuth } from "@/modules/auth/utils/auth-utils"
import { insertTodoSchema } from "../schemas/todo.schema"
import todosRoutes from "../todos.route"

export async function createTodo(data: unknown) {
  try {
    // 1. 验证用户身份
    const user = await requireAuth()
    
    // 2. 验证输入数据
    const validatedData = insertTodoSchema.parse({
      ...data,
      userId: user.id,
    })
    
    // 3. 数据库操作
    const db = await getDb()
    const [newTodo] = await db
      .insert(todos)
      .values(validatedData)
      .returning()
    
    // 4. 重新验证相关路径
    revalidatePath(todosRoutes.list)
    
    // 5. 返回结果
    return {
      success: true,
      data: newTodo,
      error: null,
    }
  } catch (error) {
    return {
      success: false,
      data: null,
      error: error instanceof Error ? error.message : "创建失败",
    }
  }
}
```

**关键点**：
- ✅ **"use server"** 指令：标记为服务器操作
- ✅ **安全第一**：始终验证用户身份和输入数据
- ✅ **自动重新验证**：使用 `revalidatePath` 更新缓存
- ✅ **统一错误处理**：返回标准的响应格式

**3. 页面组件（RSC）**

```typescript
// modules/todos/todo-list.page.tsx
import Link from "next/link"
import { Button } from "@/components/ui/button"
import getAllTodos from "./actions/get-todos.action"
import { TodoCard } from "./components/todo-card"
import todosRoutes from "./todos.route"

// React Server Component
export default async function TodoListPage() {
  // 直接在组件中调用 Server Action
  const todos = await getAllTodos()
  
  return (
    <>
      <div className="flex justify-between items-center mb-8">
        <h1 className="text-3xl font-bold">待办事项</h1>
        <Link href={todosRoutes.new}>
          <Button>添加待办</Button>
        </Link>
      </div>
      
      {todos.length === 0 ? (
        <div className="text-center py-12">
          <p>还没有待办事项</p>
          <Link href={todosRoutes.new}>
            <Button>创建第一个待办</Button>
          </Link>
        </div>
      ) : (
        <div className="grid gap-4">
          {todos.map((todo) => (
            <TodoCard key={todo.id} todo={todo} />
          ))}
        </div>
      )}
    </>
  )
}
```

**关键点**：
- ✅ **异步组件**：RSC 可以是 async 函数
- ✅ **直接数据获取**：无需 useEffect 或状态管理
- ✅ **零客户端 JS**：数据获取在服务器完成
- ✅ **自动缓存**：Next.js 自动缓存数据请求

**4. 客户端组件（表单）**

```typescript
// modules/todos/components/todo-form.tsx
"use client"

import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { useRouter } from "next/navigation"
import { toast } from "react-hot-toast"
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { createTodo } from "../actions/create-todo.action"
import { insertTodoSchema } from "../schemas/todo.schema"
import todosRoutes from "../todos.route"

export function TodoForm() {
  const router = useRouter()
  const form = useForm({
    resolver: zodResolver(insertTodoSchema.omit({ userId: true })),
    defaultValues: {
      title: "",
      description: "",
    },
  })
  
  const onSubmit = async (data) => {
    const result = await createTodo(data)
    
    if (result.success) {
      toast.success("创建成功")
      router.push(todosRoutes.list)
    } else {
      toast.error(result.error)
    }
  }
  
  return (
    <form onSubmit={form.handleSubmit(onSubmit)}>
      <Input {...form.register("title")} placeholder="标题" />
      <Input {...form.register("description")} placeholder="描述" />
      <Button type="submit" disabled={form.formState.isSubmitting}>
        {form.formState.isSubmitting ? "创建中..." : "创建"}
      </Button>
    </form>
  )
}
```

**关键点**：
- ✅ **"use client"** 指令：标记为客户端组件
- ✅ **表单验证**：使用 Zod resolver
- ✅ **用户反馈**：Loading 状态和 Toast 通知
- ✅ **导航**：操作成功后自动导航

**5. 路由定义**

```typescript
// modules/todos/todos.route.ts
const todosRoutes = {
  list: "/dashboard/todos",
  new: "/dashboard/todos/new",
  edit: (id: string | number) => `/dashboard/todos/${id}/edit`,
} as const

export default todosRoutes
```

**关键点**：
- ✅ **集中管理**：所有路由定义在一个文件
- ✅ **类型安全**：`as const` 提供字面量类型
- ✅ **动态路由**：函数生成动态 URL
- ✅ **易于重构**：修改路由只需改一个地方

### 5.3 模块间通信

模块间通过明确的接口通信，避免直接依赖：

```typescript
// ❌ 不推荐：直接导入其他模块的内部实现
import { TodoCard } from "@/modules/todos/components/todo-card"

// ✅ 推荐：通过公共 API 通信
import { getTodoById } from "@/modules/todos/actions/get-todo-by-id.action"
```

**原则**：
- ✅ **公共 API**：模块的 `actions/` 是对外接口
- ✅ **避免循环依赖**：模块间不应相互依赖
- ✅ **共享代码**：放在 `lib/` 或 `components/` 中

---

## 6. 核心功能实现

### 6.1 身份认证系统

#### 6.1.1 Better Auth 配置

```typescript
// modules/auth/utils/auth-utils.ts
async function getAuth() {
  const { env } = await getCloudflareContext()
  const db = await getDb()
  
  return betterAuth({
    // 密钥配置
    secret: env.BETTER_AUTH_SECRET,
    baseURL: env.BETTER_AUTH_URL,
    
    // 数据库适配器
    database: drizzleAdapter(db, { provider: "sqlite" }),
    
    // 邮箱密码认证
    emailAndPassword: {
      enabled: true,
    },
    
    // 社交登录
    socialProviders: {
      google: {
        enabled: true,
        clientId: env.GOOGLE_CLIENT_ID,
        clientSecret: env.GOOGLE_CLIENT_SECRET,
      },
      github: {
        enabled: true,
        clientId: env.AUTH_GITHUB_ID,
        clientSecret: env.AUTH_GITHUB_SECRET,
      },
    },
    
    // Next.js 集成
    plugins: [nextCookies()],
  })
}
```

#### 6.1.2 认证工具函数

```typescript
// 获取当前用户
export async function getCurrentUser(): Promise<AuthUser | null> {
  const auth = await getAuth()
  const session = await auth.api.getSession({
    headers: await headers()
  })
  return session?.user ? { 
    id: session.user.id,
    name: session.user.name,
    email: session.user.email,
  } : null
}

// 要求认证（用于需要登录的功能）
export async function requireAuth(): Promise<AuthUser> {
  const user = await getCurrentUser()
  if (!user) {
    throw new Error("Authentication required")
  }
  return user
}

// 检查是否已登录
export async function isAuthenticated(): Promise<boolean> {
  const user = await getCurrentUser()
  return user !== null
}
```

#### 6.1.3 路由保护（Middleware）

```typescript
// middleware.ts
import { NextRequest, NextResponse } from "next/server"
import { getAuthInstance } from "@/modules/auth/utils/auth-utils"

export async function middleware(request: NextRequest) {
  try {
    const auth = await getAuthInstance()
    const session = await auth.api.getSession({
      headers: request.headers
    })
    
    if (!session) {
      return NextResponse.redirect(new URL("/login", request.url))
    }
    
    return NextResponse.next()
  } catch (error) {
    return NextResponse.redirect(new URL("/login", request.url))
  }
}

export const config = {
  matcher: [
    "/dashboard/:path*",  // 保护所有 dashboard 路由
  ]
}
```

### 6.2 数据库操作（Drizzle ORM）

#### 6.2.1 数据库连接

```typescript
// db/index.ts
import { getCloudflareContext } from "@opennextjs/cloudflare"
import { drizzle } from "drizzle-orm/d1"
import * as schema from "./schema"

export async function getDb() {
  const { env } = await getCloudflareContext()
  // 使用 wrangler.jsonc 中定义的绑定名称
  return drizzle(env.next_cf_app, { schema })
}
```

#### 6.2.2 查询示例

```typescript
// 简单查询
const todos = await db.select().from(todos).where(eq(todos.userId, userId))

// 关联查询
const todosWithCategories = await db
  .select({
    id: todos.id,
    title: todos.title,
    categoryName: categories.name,
  })
  .from(todos)
  .leftJoin(categories, eq(todos.categoryId, categories.id))
  .where(eq(todos.userId, userId))

// 插入数据
const [newTodo] = await db
  .insert(todos)
  .values({ title: "新待办", userId })
  .returning()

// 更新数据
await db
  .update(todos)
  .set({ completed: true })
  .where(eq(todos.id, todoId))

// 删除数据
await db.delete(todos).where(eq(todos.id, todoId))
```

#### 6.2.3 数据库迁移

```bash
# 1. 修改 src/db/schemas/ 中的模式文件

# 2. 生成迁移文件
pnpm run db:generate:named "add_priority_field"

# 3. 应用到本地数据库
pnpm run db:migrate:local

# 4. 应用到生产数据库
pnpm run db:migrate:prod
```

### 6.3 文件存储（R2）

#### 6.3.1 上传文件

```typescript
// lib/r2.ts
export async function uploadToR2(
  file: File,
  folder: string = "uploads"
): Promise<UploadResult> {
  const { env } = await getCloudflareContext()
  
  // 生成唯一文件名
  const timestamp = Date.now()
  const randomId = Math.random().toString(36).substring(2, 15)
  const extension = file.name.split(".").pop() || "bin"
  const key = `${folder}/${timestamp}_${randomId}.${extension}`
  
  // 转换为 ArrayBuffer
  const arrayBuffer = await file.arrayBuffer()
  
  // 上传到 R2
  await env.next_cf_app_bucket.put(key, arrayBuffer, {
    httpMetadata: {
      contentType: file.type,
      cacheControl: "public, max-age=31536000",
    },
    customMetadata: {
      originalName: file.name,
      uploadedAt: new Date().toISOString(),
    },
  })
  
  // 返回公共 URL
  const publicUrl = `${env.CLOUDFLARE_R2_URL}/${key}`
  return { success: true, url: publicUrl, key }
}
```

#### 6.3.2 在 Server Action 中使用

```typescript
// modules/todos/actions/upload-image.action.ts
"use server"

import { uploadToR2 } from "@/lib/r2"

export async function uploadTodoImage(formData: FormData) {
  const file = formData.get("image") as File
  
  if (!file) {
    return { success: false, error: "未选择文件" }
  }
  
  const result = await uploadToR2(file, "todos")
  return result
}
```

### 6.4 AI 集成（Workers AI）

#### 6.4.1 服务层封装

```typescript
// services/summarizer.service.ts
export class SummarizerService {
  constructor(private readonly ai: Ai) {}
  
  async summarize(
    text: string,
    config?: SummarizerConfig
  ): Promise<SummaryResult> {
    const { maxLength, style, language } = config || {}
    
    // 构建系统提示词
    const systemPrompt = this.buildSystemPrompt(maxLength, style, language)
    
    // 调用 AI 模型
    const response = await this.ai.run("@cf/meta/llama-3.2-1b-instruct", {
      messages: [
        { role: "system", content: systemPrompt },
        { role: "user", content: `Please summarize: ${text}` }
      ]
    })
    
    return {
      summary: response.response?.trim() ?? "",
      originalLength: text.length,
      summaryLength: response.response.length,
    }
  }
  
  private buildSystemPrompt(maxLength, style, language): string {
    return `You are a professional text summarizer...`
  }
}
```

#### 6.4.2 API 路由使用

```typescript
// app/api/summarize/route.ts
export async function POST(request: Request) {
  // 验证认证
  const session = await auth.api.getSession({ headers: await headers() })
  if (!session?.user) {
    return new Response(
      JSON.stringify({ error: "Authentication required" }),
      { status: 401 }
    )
  }
  
  // 获取 AI 绑定
  const { env } = await getCloudflareContext()
  
  // 验证请求
  const body = await request.json()
  const validated = summarizeRequestSchema.parse(body)
  
  // 调用服务
  const service = new SummarizerService(env.AI)
  const result = await service.summarize(validated.text, validated.config)
  
  return new Response(
    JSON.stringify({ success: true, data: result }),
    { status: 200 }
  )
}
```

---

## 7. 开发工作流

### 7.1 初始设置

```bash
# 1. 克隆项目
git clone <repository-url>
cd fullstack-next-cloudflare

# 2. 安装依赖
pnpm install

# 3. 配置环境变量
cp .dev.vars.example .dev.vars
# 编辑 .dev.vars 填入你的配置

# 4. 生成 Cloudflare 类型
pnpm run cf-typegen

# 5. 运行数据库迁移
pnpm run db:migrate:local

# 6. （可选）查看数据库
pnpm run db:studio:local
```

### 7.2 日常开发

#### 7.2.1 推荐方式（两个终端）

```bash
# 首次构建
pnpm run build:cf
# 为 Cloudflare Workers 构建 |
```

```bash
# 有环境变量改动的话，需要执行
pnpm run cf-typegen
```

```bash
# 终端 1：启动 Wrangler（提供 D1 数据库访问）
pnpm run wrangler:dev

# 终端 2：启动 Next.js（提供热更新）
pnpm run dev
```

访问 `http://localhost:3000`（推荐，有热更新）

#### 7.2.2 备选方式（单终端）

```bash
# 单命令启动（无热更新，使用 Cloudflare 运行时）
pnpm run dev:cf
```

访问 `http://localhost:8787`

### 7.3 数据库操作

```bash
# 修改数据库模式后：

# 1. 生成迁移文件
pnpm run db:generate:named "your_migration_name"

# 2. 应用到本地数据库
pnpm run db:migrate:local

# 3. 查看本地数据库
pnpm run db:studio:local

# 4. 检查数据库表
pnpm run db:inspect:local

# 5. 重置本地数据库（谨慎使用）
pnpm run db:reset:local
```

### 7.4 代码格式化

```bash
# 使用 Biome 格式化代码
pnpm run lint
```

### 7.5 构建测试

```bash
# 构建用于 Cloudflare 的版本
pnpm run build:cf

# 本地测试构建结果
pnpm run dev:cf
```

---

## 8. 构建与部署

### 8.1 部署架构

```
┌─────────────────────────────────────────────────────────────┐
│                      GitHub Repository                       │
└────────────────────┬────────────────────────────────────────┘
                     │
                     │ Push to main / PR
                     │
┌────────────────────▼────────────────────────────────────────┐
│                    GitHub Actions                            │
├─────────────────────────────────────────────────────────────┤
│  1. Checkout code                                            │
│  2. Install dependencies (pnpm)                              │
│  3. Build application (build:cf)                             │
│  4. Run database migrations                                  │
│  5. Deploy to Cloudflare Workers                             │
└────────────────────┬────────────────────────────────────────┘
                     │
        ┌────────────┴───────────┐
        │                        │
┌───────▼─────────┐     ┌───────▼──────────┐
│  Preview        │     │  Production      │
│  (PR branches)  │     │  (main branch)   │
└─────────────────┘     └──────────────────┘
        │                        │
        ▼                        ▼
Cloudflare Workers          Cloudflare Workers
(preview environment)       (production environment)
```

### 8.2 自动部署流程

#### 8.2.1 预览部署（Pull Request）

当创建 Pull Request 时：

```yaml
# .github/workflows/deploy.yml
deploy-preview:
  if: github.event_name == 'pull_request'
  steps:
    - Checkout code
    - Setup pnpm & Node.js
    - Install dependencies
    - Build application (pnpm run build:cf)
    - Run database migrations (preview environment)
    - Deploy to Cloudflare Workers (preview)
    - Comment PR with preview URL
```

**特点**：
- ✅ **自动触发**：PR 创建时自动部署
- ✅ **独立环境**：预览环境独立于生产
- ✅ **自动评论**：部署完成后在 PR 中评论预览 URL
- ✅ **安全测试**：在生产前验证更改

#### 8.2.2 生产部署（Main 分支）

当合并到 `main` 分支时：

```yaml
# .github/workflows/deploy.yml
deploy-production:
  if: github.ref == 'refs/heads/main'
  steps:
    - Checkout code
    - Setup pnpm & Node.js
    - Install dependencies
    - Build application (pnpm run build:cf)
    - Backup production database
    - Run database migrations (production)
    - Deploy to Cloudflare Workers (production)
    - Post-deployment verification
```

**特点**：
- ✅ **自动备份**：部署前备份生产数据库
- ✅ **数据库迁移**：自动运行迁移
- ✅ **零停机**：Cloudflare Workers 支持无缝部署
- ✅ **验证步骤**：部署后验证

### 8.3 手动部署

#### 8.3.1 部署到生产

```bash
# 方式 1：使用 npm script
pnpm run deploy

# 方式 2：使用 wrangler（需要先构建）
pnpm run build:cf
wrangler deploy
```

#### 8.3.2 部署到预览环境

```bash
pnpm run deploy:preview
```

#### 8.3.3 数据库迁移

```bash
# 预览环境
pnpm run db:migrate:preview

# 生产环境
pnpm run db:migrate:prod
```

### 8.4 环境管理

#### 8.4.1 本地开发环境

```bash
# .dev.vars（不提交到 Git）
CLOUDFLARE_ACCOUNT_ID=your-account-id
CLOUDFLARE_D1_TOKEN=your-api-token
BETTER_AUTH_SECRET=your-secret
GOOGLE_CLIENT_ID=your-client-id
GOOGLE_CLIENT_SECRET=your-client-secret
CLOUDFLARE_R2_URL=https://pub-xxxxx.r2.dev
```

#### 8.4.2 GitHub Secrets（CI/CD）

在 GitHub 仓库设置中添加以下 Secrets：

```
CLOUDFLARE_API_TOKEN
CLOUDFLARE_ACCOUNT_ID
BETTER_AUTH_SECRET
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
CLOUDFLARE_R2_URL
```

#### 8.4.3 Cloudflare Worker Secrets（生产）

```bash
# 添加生产环境密钥
echo "your-secret" | wrangler secret put BETTER_AUTH_SECRET
echo "your-client-id" | wrangler secret put GOOGLE_CLIENT_ID
echo "your-client-secret" | wrangler secret put GOOGLE_CLIENT_SECRET
echo "your-r2-url" | wrangler secret put CLOUDFLARE_R2_URL

# 查看已设置的密钥（不显示值）
wrangler secret list
```

### 8.5 Wrangler 配置

```jsonc
// wrangler.jsonc
{
  "name": "next-cf-app",
  "main": ".open-next/worker.js",
  "compatibility_date": "2025-03-01",
  "compatibility_flags": ["nodejs_compat", "global_fetch_strictly_public"],
  
  // 静态资源
  "assets": {
    "binding": "ASSETS",
    "directory": ".open-next/assets"
  },
  
  // D1 数据库绑定
  "d1_databases": [
    {
      "binding": "next_cf_app",               // 代码中使用的名称
      "database_name": "next-cloudflare",     // Cloudflare 中的数据库名
      "database_id": "your-database-id",      // 数据库 ID
      "migrations_dir": "./src/drizzle"       // 迁移文件目录
    }
  ],
  
  // R2 存储桶绑定
  "r2_buckets": [
    {
      "bucket_name": "next-cloudflare",
      "binding": "next_cf_app_bucket"
    }
  ],
  
  // Workers AI 绑定
  "ai": {
    "binding": "AI"
  },
  
  // 预览环境配置
  "env": {
    "preview": {
      "name": "next-cf-app-preview",
      // ... 相同的绑定配置
    }
  }
}
```

### 8.6 部署检查清单

**部署前**：
- [ ] 代码已通过本地测试
- [ ] 数据库迁移已在本地验证
- [ ] 所有环境变量已配置
- [ ] 构建成功（`pnpm run build:cf`）

**部署中**：
- [ ] GitHub Actions 工作流运行成功
- [ ] 数据库迁移成功应用
- [ ] 密钥正确注入

**部署后**：
- [ ] 应用可以正常访问
- [ ] 认证功能正常
- [ ] 数据库操作正常
- [ ] R2 文件上传正常
- [ ] AI 功能正常（如果使用）

---

## 9. 添加新功能规范

### 9.1 添加新模块的完整流程

假设我们要添加一个"文章"（Articles）功能。

#### 9.1.1 第一步：规划模块结构

```
modules/articles/
├── actions/
│   ├── create-article.action.ts
│   ├── get-articles.action.ts
│   ├── get-article-by-id.action.ts
│   ├── update-article.action.ts
│   └── delete-article.action.ts
├── components/
│   ├── article-form.tsx
│   ├── article-card.tsx
│   └── article-list.tsx
├── models/
│   └── article.enum.ts
├── schemas/
│   └── article.schema.ts
├── articles.route.ts
├── article-list.page.tsx
├── article-detail.page.tsx
├── new-article.page.tsx
└── edit-article.page.tsx
```

#### 9.1.2 第二步：定义数据库模式

```typescript
// modules/articles/schemas/article.schema.ts
import { sqliteTable, text, integer } from "drizzle-orm/sqlite-core"
import { createInsertSchema, createSelectSchema } from "drizzle-zod"
import { z } from "zod"
import { user } from "@/modules/auth/schemas/auth.schema"

// 1. 定义 Drizzle 表模式
export const articles = sqliteTable("articles", {
  id: integer("id").primaryKey({ autoIncrement: true }),
  title: text("title").notNull(),
  content: text("content").notNull(),
  excerpt: text("excerpt"),
  coverImage: text("cover_image"),
  authorId: text("author_id")
    .notNull()
    .references(() => user.id, { onDelete: "cascade" }),
  published: integer("published", { mode: "boolean" }).default(false),
  publishedAt: text("published_at"),
  createdAt: text("created_at")
    .notNull()
    .$defaultFn(() => new Date().toISOString()),
  updatedAt: text("updated_at")
    .notNull()
    .$defaultFn(() => new Date().toISOString()),
})

// 2. 生成 Zod 验证模式
export const insertArticleSchema = createInsertSchema(articles, {
  title: z.string().min(5, "标题至少 5 个字符").max(200, "标题最多 200 个字符"),
  content: z.string().min(50, "内容至少 50 个字符"),
  excerpt: z.string().max(500, "摘要最多 500 个字符").optional(),
})

export const updateArticleSchema = insertArticleSchema.partial().omit({
  id: true,
  authorId: true,
  createdAt: true,
})

// 3. 导出 TypeScript 类型
export type Article = typeof articles.$inferSelect
export type NewArticle = typeof articles.$inferInsert
```

#### 9.1.3 第三步：更新数据库模式导出

```typescript
// db/schema.ts
export { articles } from "@/modules/articles/schemas/article.schema"
export { categories } from "@/modules/todos/schemas/category.schema"
export { todos } from "@/modules/todos/schemas/todo.schema"
export {
  account,
  session,
  user,
  verification,
} from "@/modules/auth/schemas/auth.schema"
```

#### 9.1.4 第四步：生成并应用数据库迁移

```bash
# 1. 生成迁移
pnpm run db:generate:named "create_articles_table"

# 2. 应用到本地数据库
pnpm run db:migrate:local

# 3. 验证数据库
pnpm run db:studio:local
```

#### 9.1.5 第五步：创建 Server Actions

```typescript
// modules/articles/actions/create-article.action.ts
"use server"

import { revalidatePath } from "next/cache"
import { getDb, articles } from "@/db"
import { requireAuth } from "@/modules/auth/utils/auth-utils"
import { insertArticleSchema } from "../schemas/article.schema"
import articlesRoutes from "../articles.route"

export async function createArticle(data: unknown) {
  try {
    // 验证用户
    const user = await requireAuth()
    
    // 验证数据
    const validatedData = insertArticleSchema.parse({
      ...data,
      authorId: user.id,
    })
    
    // 数据库操作
    const db = await getDb()
    const [newArticle] = await db
      .insert(articles)
      .values(validatedData)
      .returning()
    
    // 重新验证
    revalidatePath(articlesRoutes.list)
    
    return {
      success: true,
      data: newArticle,
      error: null,
    }
  } catch (error) {
    return {
      success: false,
      data: null,
      error: error instanceof Error ? error.message : "创建失败",
    }
  }
}
```

```typescript
// modules/articles/actions/get-articles.action.ts
"use server"

import { eq } from "drizzle-orm"
import { getDb, articles } from "@/db"
import { requireAuth } from "@/modules/auth/utils/auth-utils"

export async function getArticles() {
  try {
    const user = await requireAuth()
    const db = await getDb()
    
    const data = await db
      .select()
      .from(articles)
      .where(eq(articles.authorId, user.id))
      .orderBy(articles.createdAt)
    
    return data
  } catch (error) {
    console.error("获取文章失败:", error)
    return []
  }
}
```

#### 9.1.6 第六步：定义路由常量

```typescript
// modules/articles/articles.route.ts
const articlesRoutes = {
  list: "/dashboard/articles",
  detail: (id: string | number) => `/dashboard/articles/${id}`,
  new: "/dashboard/articles/new",
  edit: (id: string | number) => `/dashboard/articles/${id}/edit`,
} as const

export default articlesRoutes
```

#### 9.1.7 第七步：创建页面组件

```typescript
// modules/articles/article-list.page.tsx
import Link from "next/link"
import { Button } from "@/components/ui/button"
import { getArticles } from "./actions/get-articles.action"
import { ArticleCard } from "./components/article-card"
import articlesRoutes from "./articles.route"

export default async function ArticleListPage() {
  const articles = await getArticles()
  
  return (
    <>
      <div className="flex justify-between items-center mb-8">
        <h1 className="text-3xl font-bold">文章</h1>
        <Link href={articlesRoutes.new}>
          <Button>创建文章</Button>
        </Link>
      </div>
      
      <div className="grid gap-6">
        {articles.map((article) => (
          <ArticleCard key={article.id} article={article} />
        ))}
      </div>
    </>
  )
}
```

#### 9.1.8 第八步：在 App Router 中创建路由

```typescript
// app/dashboard/articles/page.tsx
import ArticleListPage from "@/modules/articles/article-list.page"

export default function Page() {
  return <ArticleListPage />
}
```

```typescript
// app/dashboard/articles/new/page.tsx
import NewArticlePage from "@/modules/articles/new-article.page"

export default function Page() {
  return <NewArticlePage />
}
```

```typescript
// app/dashboard/articles/[id]/page.tsx
import ArticleDetailPage from "@/modules/articles/article-detail.page"

export default function Page({ params }: { params: { id: string } }) {
  return <ArticleDetailPage id={params.id} />
}
```

#### 9.1.9 第九步：更新导航

```typescript
// components/navigation.tsx
const navLinks = [
  { href: dashboardRoutes.home, label: "仪表板" },
  { href: todosRoutes.list, label: "待办事项" },
  { href: articlesRoutes.list, label: "文章" },  // 新增
]
```

### 9.2 添加新的 API 端点

假设我们要添加一个文章摘要 API。

#### 9.2.1 创建服务层

```typescript
// services/article-summarizer.service.ts
export class ArticleSummarizerService {
  constructor(private readonly ai: Ai) {}
  
  async summarizeArticle(content: string): Promise<string> {
    const response = await this.ai.run("@cf/meta/llama-3.2-1b-instruct", {
      messages: [
        {
          role: "system",
          content: "你是一个专业的文章摘要助手。请用 2-3 句话总结文章要点。"
        },
        {
          role: "user",
          content: `请总结以下文章：\n\n${content}`
        }
      ]
    })
    
    return response.response?.trim() ?? ""
  }
}
```

#### 9.2.2 创建 API 路由

```typescript
// app/api/articles/summarize/route.ts
import { getCloudflareContext } from "@opennextjs/cloudflare"
import { headers } from "next/headers"
import { z } from "zod"
import handleApiError from "@/lib/api-error"
import { getAuthInstance } from "@/modules/auth/utils/auth-utils"
import { ArticleSummarizerService } from "@/services/article-summarizer.service"

const requestSchema = z.object({
  content: z.string().min(100, "内容太短").max(50000, "内容太长"),
})

export async function POST(request: Request) {
  try {
    // 验证认证
    const auth = await getAuthInstance()
    const session = await auth.api.getSession({
      headers: await headers()
    })
    
    if (!session?.user) {
      return new Response(
        JSON.stringify({ error: "需要认证" }),
        { status: 401 }
      )
    }
    
    // 验证请求
    const body = await request.json()
    const validated = requestSchema.parse(body)
    
    // 调用服务
    const { env } = await getCloudflareContext()
    const service = new ArticleSummarizerService(env.AI)
    const summary = await service.summarizeArticle(validated.content)
    
    return new Response(
      JSON.stringify({ success: true, data: { summary } }),
      { status: 200 }
    )
  } catch (error) {
    return handleApiError(error)
  }
}
```

### 9.3 代码组织规范总结

#### 9.3.1 文件命名规范

| 类型 | 命名规范 | 示例 |
|-----|---------|------|
| **页面组件** | `*.page.tsx` | `article-list.page.tsx` |
| **Server Actions** | `*-*.action.ts` | `create-article.action.ts` |
| **数据库模式** | `*.schema.ts` | `article.schema.ts` |
| **路由定义** | `*.route.ts` | `articles.route.ts` |
| **服务类** | `*.service.ts` | `article-summarizer.service.ts` |
| **工具函数** | `*-utils.ts` 或 `utils.ts` | `article-utils.ts` |
| **类型定义** | `*.model.ts` | `article.model.ts` |
| **枚举** | `*.enum.ts` | `article.enum.ts` |
| **组件** | `kebab-case.tsx` | `article-card.tsx` |

#### 9.3.2 代码放置位置

| 代码类型 | 放置位置 | 说明 |
|---------|---------|------|
| **功能模块** | `src/modules/[feature]/` | 完整的功能单元 |
| **共享组件** | `src/components/` | 跨模块复用的 UI 组件 |
| **共享工具** | `src/lib/` | 通用的辅助函数 |
| **业务服务** | `src/services/` | 独立的业务逻辑 |
| **数据库配置** | `src/db/` | 数据库连接和模式导出 |
| **常量定义** | `src/constants/` | 全局常量 |
| **API 路由** | `src/app/api/` | RESTful API 端点 |
| **页面路由** | `src/app/` | Next.js 页面路由 |

#### 9.3.3 导入规范

```typescript
// ✅ 推荐：使用路径别名
import { Button } from "@/components/ui/button"
import { getDb, articles } from "@/db"
import { requireAuth } from "@/modules/auth/utils/auth-utils"

// ❌ 不推荐：相对路径（难以重构）
import { Button } from "../../../../components/ui/button"
```

**路径别名配置**（已在 `tsconfig.json` 中配置）：
- `@/*` → `src/*`

---

## 10. 最佳实践

### 10.1 代码质量

#### 10.1.1 类型安全

```typescript
// ✅ 推荐：充分利用 TypeScript 类型推导
const todos = await getAllTodos()  // todos 类型自动推导为 Todo[]

// ✅ 推荐：使用 Zod 进行运行时验证
const validated = insertTodoSchema.parse(data)

// ❌ 不推荐：使用 any 类型
function processData(data: any) { ... }

// ✅ 推荐：明确的类型定义
function processData(data: TodoFormData) { ... }
```

#### 10.1.2 错误处理

```typescript
// ✅ 推荐：统一的错误处理格式
export async function createTodo(data: unknown) {
  try {
    // 业务逻辑
    return {
      success: true,
      data: result,
      error: null,
    }
  } catch (error) {
    return {
      success: false,
      data: null,
      error: error instanceof Error ? error.message : "操作失败",
    }
  }
}

// ✅ 推荐：在客户端显示用户友好的错误消息
if (!result.success) {
  toast.error(result.error || "操作失败，请重试")
}
```

#### 10.1.3 安全性

```typescript
// ✅ 推荐：始终验证用户身份
export async function createTodo(data: unknown) {
  const user = await requireAuth()  // 如果未登录会抛出错误
  // ... 业务逻辑
}

// ✅ 推荐：验证输入数据
const validatedData = insertTodoSchema.parse(data)

// ✅ 推荐：防止越权操作
const todo = await db
  .select()
  .from(todos)
  .where(and(
    eq(todos.id, todoId),
    eq(todos.userId, user.id)  // 确保用户只能访问自己的数据
  ))
```

### 10.2 性能优化

#### 10.2.1 数据获取

```typescript
// ✅ 推荐：使用 React Server Components 直接获取数据
export default async function TodoListPage() {
  const todos = await getAllTodos()  // 在服务器端执行
  return <div>{todos.map(...)}</div>
}

// ✅ 推荐：并行获取多个数据源
const [todos, categories] = await Promise.all([
  getAllTodos(),
  getCategories(),
])

// ❌ 不推荐：串行获取（慢）
const todos = await getAllTodos()
const categories = await getCategories()
```

#### 10.2.2 缓存策略

```typescript
// ✅ 推荐：使用 Next.js 缓存控制
export const revalidate = 3600  // 1 小时重新验证

// ✅ 推荐：在数据变更后重新验证缓存
await createTodo(data)
revalidatePath(todosRoutes.list)  // 清除相关页面缓存

// ✅ 推荐：使用 unstable_cache 缓存昂贵的计算
import { unstable_cache } from "next/cache"

const getExpensiveData = unstable_cache(
  async () => {
    // 昂贵的计算
    return result
  },
  ["cache-key"],
  { revalidate: 3600 }
)
```

#### 10.2.3 组件优化

```typescript
// ✅ 推荐：客户端组件只在需要交互时使用
"use client"  // 只在需要 useState、useEffect 等时添加

// ✅ 推荐：将客户端组件拆分得更小
// 大部分组件保持为服务器组件，只把需要交互的部分提取为客户端组件
export default async function TodoPage() {
  const todos = await getAllTodos()  // 服务器组件
  
  return (
    <div>
      <TodoList todos={todos} />  {/* 服务器组件 */}
      <CreateTodoButton />  {/* 客户端组件 */}
    </div>
  )
}
```

### 10.3 国际化

```typescript
// ✅ 推荐：使用国际化工具，不要硬编码文本
// modules/todos/todo-list.page.tsx
import { useTranslation } from "next-i18next"

export default function TodoListPage() {
  const { t } = useTranslation("todos")
  
  return (
    <h1>{t("title")}</h1>  // ✅ 从翻译文件加载
    // ❌ 不要这样：<h1>待办事项</h1>
  )
}

// ✅ 推荐：翻译文件结构
// locales/zh-CN/todos.json
{
  "title": "待办事项",
  "createSuccess": "创建成功",
  "deleteConfirm": "确定要删除吗？"
}

// locales/en/todos.json
{
  "title": "Todos",
  "createSuccess": "Created successfully",
  "deleteConfirm": "Are you sure you want to delete?"
}
```

### 10.4 测试建议

```typescript
// ✅ 推荐：为 Server Actions 编写单元测试
import { describe, it, expect } from "vitest"
import { createTodo } from "./create-todo.action"

describe("createTodo", () => {
  it("should create a new todo", async () => {
    const result = await createTodo({
      title: "测试待办",
      description: "测试描述",
    })
    
    expect(result.success).toBe(true)
    expect(result.data).toHaveProperty("id")
  })
  
  it("should reject invalid data", async () => {
    const result = await createTodo({
      title: "短",  // 太短，应该失败
    })
    
    expect(result.success).toBe(false)
    expect(result.error).toBeTruthy()
  })
})
```

### 10.5 注释规范

```typescript
// ✅ 推荐：为复杂的业务逻辑添加注释
/**
 * 创建新的待办事项
 * 
 * @param data - 待办事项数据（未验证）
 * @returns 包含成功状态、数据和错误信息的响应对象
 * 
 * @example
 * ```typescript
 * const result = await createTodo({
 *   title: "新待办",
 *   description: "描述"
 * })
 * ```
 */
export async function createTodo(data: unknown) {
  // 实现
}

// ✅ 推荐：为关键配置添加说明
export const todos = sqliteTable("todos", {
  // 自增主键
  id: integer("id").primaryKey({ autoIncrement: true }),
  
  // 待办标题，必填
  title: text("title").notNull(),
  
  // 关联用户，级联删除
  userId: text("user_id")
    .notNull()
    .references(() => user.id, { onDelete: "cascade" }),
})

// ❌ 不推荐：过度注释显而易见的代码
const title = data.title  // 获取标题
```

### 10.6 Git 工作流

```bash
# ✅ 推荐：遵循约定式提交（Conventional Commits）

# 新功能
git commit -m "feat: 添加文章模块"

# 修复 bug
git commit -m "fix: 修复待办事项删除失败的问题"

# 文档更新
git commit -m "docs: 更新架构文档"

# 代码重构
git commit -m "refactor: 重构认证工具函数"

# 性能优化
git commit -m "perf: 优化数据库查询性能"

# 测试
git commit -m "test: 添加待办事项单元测试"

# 构建配置
git commit -m "chore: 更新依赖版本"
```

---

## 附录

### A. 常用命令速查

```bash
# 开发
pnpm run dev                      # Next.js 开发服务器
pnpm run wrangler:dev             # Wrangler 开发服务器
pnpm run dev:cf                   # Cloudflare 运行时（单命令）

# 构建
pnpm run build:cf                 # 构建 Cloudflare 版本
pnpm run cf-typegen              # 生成 Cloudflare 类型

# 数据库
pnpm run db:generate             # 生成迁移
pnpm run db:generate:named "name" # 生成命名迁移
pnpm run db:migrate:local        # 本地迁移
pnpm run db:migrate:prod         # 生产迁移
pnpm run db:studio:local         # 打开数据库 Studio
pnpm run db:inspect:local        # 检查本地数据库
pnpm run db:reset:local          # 重置本地数据库

# 部署
pnpm run deploy                  # 部署到生产
pnpm run deploy:preview          # 部署到预览

# 密钥管理
pnpm run cf:secret SECRET_NAME   # 添加生产密钥

# 代码质量
pnpm run lint                    # 格式化代码
```

### B. 环境变量列表

| 变量名 | 环境 | 必需 | 说明 |
|-------|------|------|------|
| `CLOUDFLARE_ACCOUNT_ID` | 本地 | ✅ | Cloudflare 账户 ID |
| `CLOUDFLARE_D1_TOKEN` | 本地 | ✅ | D1 访问令牌 |
| `BETTER_AUTH_SECRET` | 全部 | ✅ | 认证密钥（32字节随机字符串） |
| `BETTER_AUTH_URL` | 生产 | ✅ | 应用基础 URL |
| `GOOGLE_CLIENT_ID` | 全部 | ✅ | Google OAuth 客户端 ID |
| `GOOGLE_CLIENT_SECRET` | 全部 | ✅ | Google OAuth 客户端密钥 |
| `AUTH_GITHUB_ID` | 全部 | ⭕ | GitHub OAuth 应用 ID |
| `AUTH_GITHUB_SECRET` | 全部 | ⭕ | GitHub OAuth 应用密钥 |
| `CLOUDFLARE_R2_URL` | 全部 | ✅ | R2 存储桶公共 URL |

### C. 常见问题排查

#### C.1 数据库连接失败

```bash
# 问题：无法连接到 D1 数据库
# 解决：
1. 检查 wrangler.jsonc 中的数据库绑定名称
2. 确保 Wrangler 开发服务器正在运行
3. 重新生成类型：pnpm run cf-typegen
```

#### C.2 认证失败

```bash
# 问题：登录后立即跳转到登录页
# 解决：
1. 检查 BETTER_AUTH_SECRET 是否配置
2. 清除浏览器 Cookie
3. 检查 middleware.ts 中的路由配置
```

#### C.3 构建失败

```bash
# 问题：pnpm run build:cf 失败
# 解决：
1. 清除缓存：rm -rf .next .open-next
2. 重新安装依赖：rm -rf node_modules && pnpm install
3. 检查 TypeScript 错误
```

### D. 学习资源

- **Next.js 官方文档**: https://nextjs.org/docs
- **Cloudflare Workers 文档**: https://developers.cloudflare.com/workers/
- **Drizzle ORM 文档**: https://orm.drizzle.team/docs/overview
- **Better Auth 文档**: https://www.better-auth.com/docs
- **React Server Components**: https://react.dev/reference/rsc/server-components

---

## 贡献

欢迎对本文档提出改进建议！如有疑问或发现错误，请提交 Issue 或 Pull Request。

---

**文档版本**: 1.0.0  
**最后更新**: 2025-01-25  
**维护者**: 项目团队


