<div align="center">

# 智能 AI 面试官平台

一个面向求职场景的 AI 面试练习与知识库问答项目。

[![Java](https://img.shields.io/badge/Java-21-orange?logo=openjdk)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.0-green?logo=springboot)](https://spring.io/projects/spring-boot)
[![Spring AI](https://img.shields.io/badge/Spring%20AI-2.0-blue)](https://spring.io/projects/spring-ai)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-336791?logo=postgresql)](https://www.postgresql.org/)

</div>

## 简介

InterviewGuide 提供简历解析、模拟面试和知识库问答三类能力，偏向“作品集 + 练手项目”定位。

## 亮点

- 简历上传后自动解析与去重
- 基于简历内容生成主问题与 Follow-up
- 使用 PostgreSQL + pgvector 做知识库检索
- SSE 流式输出，保留打字机体验
- 结构化输出重试、PDF 导出、Docker 部署

## 技术栈

- **后端**：Java 21、Spring Boot 4.0、Spring AI 2.0、PostgreSQL + pgvector、Redis（Redisson）、Apache Tika、MapStruct、iText 8
- **前端**：React 18.3、TypeScript 5.6、Vite、Tailwind CSS、React Router

## 快速导航

- [快速开始](#快速开始)
- [项目结构](#项目结构)
- [常见问题](#常见问题)




## 项目结构

- `app/`：后端 Spring Boot 服务
- `frontend/`：前端 React 应用
- `docker/`：数据库初始化脚本
- `docker-compose.yml`：本地一键启动编排

## 快速开始

1. 复制配置模板：`cp .env.example .env`
2. 填写必要变量：`AI_BAILIAN_API_KEY`、数据库账号、对象存储配置
3. 启动服务：`docker-compose up -d --build`
4. 本地开发可分别执行后端 `./gradlew bootRun` 和前端 `pnpm dev`

## 配置说明

- 环境变量优先看 `.env.example`
- 应用配置优先看 `app/src/main/resources/application.yml`
- README 不再维护完整配置块，避免与实际环境脱节



## 许可证

AGPL-3.0 License
