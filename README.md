<div align="center">

# 智能 AI 面试官平台

一个面向求职场景的 AI 面试练习与知识库问答项目。

[![Java](https://img.shields.io/badge/Java-21-orange?logo=openjdk)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.0-green?logo=springboot)](https://spring.io/projects/spring-boot)
[![Spring AI](https://img.shields.io/badge/Spring%20AI-2.0-blue)](https://spring.io/projects/spring-ai)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-336791?logo=postgresql)](https://www.postgresql.org/)
[![React](https://img.shields.io/badge/React-18.3-blue?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.6-blue?logo=typescript)](https://www.typescriptlang.org/)

</div>

## 项目简介

InterviewGuide 主要提供三类能力：简历解析、模拟面试、RAG 知识库问答。
项目结合 LLM、向量检索和流式交互，实现更轻量的 AI 面试辅助体验。

## 核心功能

- **简历解析**：支持 PDF / DOCX / DOC / TXT，配合去重与异步处理。
- **模拟面试**：基于简历内容出题，支持 Follow-up 追问与结果汇总。
- **知识库问答**：使用 PostgreSQL + pgvector 实现文档检索与 SSE 流式回答。
- **工程化能力**：支持结构化输出重试、Docker 部署、PDF 导出等。

## 技术栈

- **后端**：Java 21、Spring Boot 4.0、Spring AI 2.0、PostgreSQL + pgvector、Redis（Redisson）、Apache Tika、MapStruct、iText 8
- **前端**：React 18.3、TypeScript 5.6、Vite、Tailwind CSS、React Router、Framer Motion、Recharts

## 快速导航

- [快速开始](#快速开始)
- [Docker 快速部署](#docker-快速部署)
- [效果展示](#效果展示)
- [项目结构](#项目结构)
- [常见问题](#常见问题)

## 系统架构

**提示**：架构图采用 draw.io 绘制，导出为 svg 格式，在 Github Dark 模式下的显示效果会有问题。

![](https://oss.javaguide.cn/xingqiu/pratical-project/interview-guide/interview-guide-architecture-diagram.svg)

**异步处理流程**：

简历分析、知识库向量化和面试报告生成采用 Redis Stream 异步处理，这里以简历分析和知识库向量化为例介绍整体流程：

```
上传请求 → 保存文件 → 发送消息到 Stream → 立即返回
                              ↓
                      Consumer 消费消息
                              ↓
                    执行分析/向量化任务
                              ↓
                      更新数据库状态
                              ↓
                   前端轮询获取最新状态
```

状态流转： `PENDING` → `PROCESSING` → `COMPLETED` / `FAILED`。

## 功能特性

### 简历管理模块

- **多格式解析**：支持 PDF、DOCX、DOC、TXT 等简历格式。
- **异步处理流**：基于 Redis Stream 实现异步分析，支持实时查看处理进度。
- **稳定性保障**：内置结构化输出重试机制与基于内容哈希的重复检测。
- **分析报告导出**：支持将 AI 分析结果导出为 PDF 报告。

### 模拟面试模块

- **个性化出题**：基于简历内容生成针对性的面试题目。
- **智能追问流**：支持多轮 Follow-up，构建线性问答链路。
- **分批评估机制**：通过分批处理降低长文本评估的 Token 风险。
- **报告一键导出**：支持异步生成并导出评估报告。

### 知识库管理模块

- **文档智能处理**：支持 PDF、DOCX、Markdown 等文档的上传、分块与向量化。
- **RAG 检索增强**：结合向量库提升 AI 问答准确性与专业度。
- **流式响应交互**：基于 SSE 实现打字机式输出体验。
- **智能问答对话**：支持知识库问答与统计信息查看。

### TODO

- [x] 问答助手的 Markdown 展示优化
- [x] 知识库管理页面的知识库下载
- [x] 异步生成模拟面试评估报告
- [x] Docker 快速部署
- [x] 添加 API 限流保护
- [x] 前端性能优化（RAG 聊天 - 虚拟列表）
- [x] 模拟面试增加追问功能
- [ ] 打通模拟面试和知识库

## 效果展示

### 效果展示

> 下方截图仅用于说明功能形态，部分图片为项目展示图引用；如果你要用于个人仓库，建议后续替换为自己的界面截图。

- 简历分析、模拟面试、知识库问答、项目结构等页面均已实现。
- 如果你计划二次开发，建议替换为自己的业务截图与实际部署环境截图。


## 项目结构

```
interview-guide/
├── app/                              # 后端应用
│   ├── src/main/java/interview/guide/
│   │   ├── App.java                  # 主启动类
│   │   ├── common/                   # 通用模块
│   │   │   ├── config/               # 配置类
│   │   │   ├── exception/            # 异常处理
│   │   │   └── result/               # 统一响应
│   │   ├── infrastructure/           # 基础设施
│   │   │   ├── export/               # PDF 导出
│   │   │   ├── file/                 # 文件处理
│   │   │   ├── redis/                # Redis 服务
│   │   │   └── storage/              # 对象存储
│   │   └── modules/                  # 业务模块
│   │       ├── interview/            # 面试模块
│   │       ├── knowledgebase/        # 知识库模块
│   │       └── resume/               # 简历模块
│   └── src/main/resources/
│       ├── application.yml           # 应用配置
│       └── prompts/                  # AI 提示词模板
│
├── frontend/                         # 前端应用
│   ├── src/
│   │   ├── api/                      # API 接口
│   │   ├── components/               # 公共组件
│   │   ├── pages/                    # 页面组件
│   │   ├── types/                    # 类型定义
│   │   └── utils/                    # 工具函数
│   ├── package.json
│   └── vite.config.ts
│
└── README.md
```

## 快速开始

环境要求：

| 依赖          | 版本 | 必需 |
| ------------- | ---- | ---- |
| JDK           | 21+  | 是   |
| Node.js       | 18+  | 是   |
| PostgreSQL    | 14+  | 是   |
| pgvector 扩展 | -    | 是   |
| Redis         | 6+   | 是   |
| S3 兼容存储   | -    | 是   |

### 1. 克隆项目

```bash
git clone git@github.com:Ran-jyqj/interviewAgent.git
cd interviewAgent
```

### 2. 配置数据库

```sql
-- 创建数据库
CREATE DATABASE interview_guide;

-- 连接数据库并启用 pgvector 扩展（可选，启动后端SpringAI框架底层会自动创建）
CREATE EXTENSION vector;
```

### 3. 配置环境变量

```bash
# AI API 密钥（阿里云 DashScope）
export AI_BAILIAN_API_KEY=your_api_key

# 可选：指定模型，默认 qwen-plus
export AI_MODEL=qwen-plus
```

### 4. 修改应用配置

编辑 `app/src/main/resources/application.yml`：

```yaml
spring:
  # PostgreSQL 数据库配置
  datasource:
    url: jdbc:postgresql://${POSTGRES_HOST:localhost}:${POSTGRES_PORT:5432}/${POSTGRES_DB:interview_guide}
    username: ${POSTGRES_USER:postgres}
    password: ${POSTGRES_PASSWORD:123456}
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: create #首次启动用 create，表创建成功后，改回 update

    # Redisson 配置（使用 spring.redis.redisson）
  redis:
    redisson:
      config: |
        singleServerConfig:
          address: "redis://${REDIS_HOST:localhost}:${REDIS_PORT:6379}"
          database: 0
          connectionMinimumIdleSize: 10
          connectionPoolSize: 64
          subscriptionConnectionMinimumIdleSize: 1
          subscriptionConnectionPoolSize: 50

# RustFS（S3 兼容）存储配置
app:
  # 面试配置
  interview:
    follow-up-count: ${APP_INTERVIEW_FOLLOW_UP_COUNT:1}    # 每个主问题生成追问数量
    evaluation:
      batch-size: ${APP_INTERVIEW_EVALUATION_BATCH_SIZE:8} # 回答评估分批大小
  storage:
    endpoint: ${APP_STORAGE_ENDPOINT:http://localhost:9000}
    access-key: ${APP_STORAGE_ACCESS_KEY:wr45VXJZhCxc6FAWz0YR}
    secret-key: ${APP_STORAGE_SECRET_KEY:GtKxV57WJkpw4CvASPBzTy2DYElLnRqh8dIXQa0m}
    bucket: ${APP_STORAGE_BUCKET:interview-guide}
    region: ${APP_STORAGE_REGION:us-east-1}



```

⚠️**注意**：

1. JPA 的 `ddl-auto` 首次启动用 `create`，表创建成功后，改回 `update`。
2. 如果本地有 Minio 的话，可以用其替换 RusfFS。

### 5. 启动服务

**方式 A：本地启动**

后端：

```bash
./gradlew bootRun
```

后端服务启动于 `http://localhost:8080`

前端：

```bash
cd frontend
pnpm install
pnpm dev
```

前端服务启动于 `http://localhost:5173`

**方式 B：Docker 一键启动**

如果你希望快速体验完整环境，建议直接使用 Docker Compose：

```bash
cp .env.example .env
docker-compose up -d --build
```

启动后可访问：前端 `http://localhost`，后端 `http://localhost:8080`


## Docker 快速部署

本项目提供了完整的 Docker 支持，可以一键启动所有服务（前后端、数据库、中间件）。

### 1. 前置准备
- 安装 [Docker](https://www.docker.com/products/docker-desktop/) 和 Docker Compose
- 申请阿里云百炼 API Key（用于 AI 对话功能）

### 2. 快速启动
在项目根目录下执行：

```bash
# 1. 复制环境变量配置文件
cp .env.example .env

# 2. 编辑 .env 文件，填入 AI 配置
# vim .env
# 必填：AI_BAILIAN_API_KEY=your_key_here
# 可选：AI_MODEL=qwen-plus        # 默认值为 qwen-plus
#        # 也可以改为 qwen-max、qwen-long 等其他可用模型
#
# 面试参数配置（可选）：
# APP_INTERVIEW_FOLLOW_UP_COUNT=1         # 每个主问题生成追问数量（默认 1）
# APP_INTERVIEW_EVALUATION_BATCH_SIZE=8   # 回答评估分批大小（默认 8）

# 3. 构建并启动所有服务
docker-compose up -d --build
```

### 3. 服务访问
启动完成后，您可以通过以下地址访问各个服务：

| 服务             | 地址                                           | 默认账号     | 默认密码     | 说明                   |
| ---------------- | ---------------------------------------------- | ------------ | ------------ | ---------------------- |
| **前端应用**     | [http://localhost](http://localhost)           | -            | -            | 用户访问入口           |
| **后端 API**     | [http://localhost:8080](http://localhost:8080) | -            | -            | Swagger/接口文档       |
| **MinIO 控制台** | [http://localhost:9001](http://localhost:9001) | `minioadmin` | `minioadmin` | 对象存储管理           |
| **MinIO API**    | `localhost:9000`                               | -            | -            | S3 兼容接口            |
| **PostgreSQL**   | `localhost:5432`                               | `postgres`   | `password`   | 数据库 (包含 pgvector) |
| **Redis**        | `localhost:6379`                               | -            | -            | 缓存与消息队列         |

### 4. 常用运维命令

```bash
# 查看服务状态
docker-compose ps

# 查看后端日志
docker-compose logs -f app

# 停止并移除所有服务
docker-compose down

# 清理无用镜像（构建产生的中间层）
docker image prune -f
```

## 使用场景

| 用户角色        | 使用场景                               |
| --------------- | -------------------------------------- |
| **求职者**      | 上传简历获取分析建议，进行模拟面试练习 |
| **HR/招聘人员** | 批量分析简历，评估候选人能力           |
| **培训机构**    | 提供面试培训服务，管理知识库资源       |

## 常见问题

### Q: 数据库表创建失败/数据丢失

这大概率是 JPA 的 `ddl-auto` 配置不对的原因。`ddl-auto` 模式对比：

| 模式     | 行为                            | 适用场景      |
| -------- | ------------------------------- | ------------- |
| create   | 无条件删除并重建所有表          | 开发/测试环境 |
| update   | 对比现有 schema，只执行增量更新 | 开发环境      |
| validate | 只验证，不修改                  | 生产环境      |
| none     | 什么都不做                      | 生产环境      |

对于新数据库，推荐：

```yaml
# 首次启动用 create
jpa:
  hibernate:
    ddl-auto: create

# 表创建成功后，改回 update
jpa:
  hibernate:
    ddl-auto: update
```

记得改回 **update**，否则每次重启都会删除所有数据！

### Q: 知识库向量化失败

当 `initialize-schema: false` 时，Spring AI **不会自动创建** `vector_store` 表。

```java
spring:
  ai:
    vectorstore:
      pgvector:
        initialize-schema: true 

```

建议开发环境设置为 true，方便快速启动。生产环境设置为 false，手动管理数据库 schema，避免意外变更。

### Q: 简历分析失败

检查一下阿里云 DashScope API KEY 是否配置正确（申请地址：<https://bailian.console.aliyun.com/>）。

### Q: 简历分析一直显示"分析中"？

检查 Redis 连接和 Stream Consumer 是否正常运行。查看后端日志确认是否有错误。

### Q: PDF 导出失败或中文显示异常？

项目已内置中文字体（珠圆玉润仿宋），支持跨平台导出。如遇到问题，请检查：
- 字体文件是否存在：`app/src/main/resources/fonts/ZhuqueFangsong-Regular.ttf`
- 检查日志中的字体加载信息
- 确认 iText 依赖是否正确

## 贡献

欢迎提交 Issue 和 Pull Request！

## 许可证

AGPL-3.0 License（只要通过网络提供服务，就必须向用户公开修改后的源码）
