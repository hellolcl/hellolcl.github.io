---
title: 🚀 项目体验 | LLMentor AI 差旅助手
date: 2026-11-08 10:00:00
updated: 2026-11-08 10:00:00
top: 1
tags:
  - 项目
categories:
  - 项目
---

> 🎉 我的多智能体 AI 差旅助手上线了,一句话完成行程规划、机票酒店、签证核查、天气资讯,点击下方按钮立即体验!

<a href="http://117.72.116.55:8090" target="_blank" rel="noopener" style="display:inline-block;padding:.7rem 2.2rem;border-radius:999px;background:#425aef;color:#fff;text-decoration:none;font-weight:600;">🛫 立即在线体验</a>

## 它能做什么

对着它说一句人话,比如:

> 帮我安排下周三到周五从北京到深圳的出差,需要机票和酒店,顺便看下那边天气

Agent 会自动拆解任务、调用工具(签证/天气/资讯 MCP)、流式输出完整行程方案,并支持按企业差旅政策发起合规审批。

## 技术亮点

| 能力 | 实现 |
|---|---|
| 多 Agent 协作 | AgentScope Java 框架(规划/查询/预订/审批多智能体) |
| 工具生态 | MCP 协议接入签证、天气、资讯等外部工具 |
| 企业知识 | RAG 检索差旅政策文档,审批自动比对合规性 |
| 高可用 | 工具熔断降级 + 会话持久化 + 流式输出 |
| 部署 | Docker Compose 五容器,单机一键拉起 |

更详细的项目页面:[项目板块](/apps/)
