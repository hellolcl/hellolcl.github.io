---
title: 项目
date: 2026-11-08 10:00:00
comment: false
aside: false
---

<style>
.app-card {
  max-width: 720px;
  margin: 2rem auto;
  padding: 2rem;
  border-radius: 16px;
  background: var(--card-bg, #f7f9fc);
  box-shadow: 0 4px 16px rgba(0,0,0,.08);
  text-align: center;
}
.app-card h2 { margin-top: 0; }
.app-card .tech { color: #888; font-size: .9rem; margin-bottom: 1.5rem; }
.app-btn {
  display: inline-block;
  padding: .7rem 2.2rem;
  border-radius: 999px;
  background: #425aef;
  color: #fff !important;
  text-decoration: none;
  font-weight: 600;
  transition: all .3s;
}
.app-btn:hover { background: #3549cf; transform: translateY(-2px); }
[data-theme="dark"] .app-card { background: #1b1c22; }
</style>

<div class="app-card">

## ✈️ LLMentor · AI 差旅助手

多智能体协作的差旅助手:一句话完成行程规划、机票酒店、签证核查、天气资讯,支持企业差旅政策合规审批。

<div class="tech">Spring Boot 3 · AgentScope · React 19 · MCP · RAG · Docker</div>

<a class="app-btn" href="http://117.72.116.55:8090" target="_blank" rel="noopener">在线体验</a>

</div>

### 体验提示

1. 点击按钮进入后先**注册账号**(邮箱 + 密码)
2. 试试输入:`帮我安排下周三到周五从北京到深圳的出差,需要机票和酒店,顺便看下那边天气`
3. 演示环境 LLM 接口有额度限制,响应慢或降级属正常现象
