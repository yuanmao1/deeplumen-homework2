# Homework: Multi-Tenant Static Site Auto Deployment

## Background

我们希望构建一个能够 **自动部署静态站点并按子域名隔离租户** 的系统。

假设主域名为：

```
deeplumen.com
```

系统需要支持自动部署如下形式的站点：

```
user1.deeplumen.com
user2.deeplumen.com
user3.deeplumen.com
```

每个子域名对应一个 **独立的静态网站实例**。

候选人需要实现一个程序，能够 **自动接收一个静态网站包并完成部署**。

---

# Task Description

请使用 **Golang 或 Python** 实现一个系统，实现以下能力：

### 1. 自动部署静态站点

给定：

```
tenant_name = user1
site_package = geo-html/
```

系统应能够自动部署为：

```
https://user1.deeplumen.com
```

并能够访问该静态站点。

---

### 2. 多租户隔离

不同用户的站点应相互隔离，例如：

```
user1.deeplumen.com
user2.deeplumen.com
```

对应：

```
/sites/user1/
/sites/user2/
```

或其他你设计的隔离方案。

---

### 3. 子域名绑定

系统需要自动将子域名绑定到对应站点。

例如：

```
user1.deeplumen.com → /sites/user1
user2.deeplumen.com → /sites/user2
```

你可以使用：

* Nginx
* Caddy
* Traefik
* 或其它方式实现。（推荐cloudflare)

---

# Static Site Package Structure

部署的静态站点结构类似如下：

```
geo-html/
├── index.html
├── jobs/
│   └── index.html
├── accounts/
│   └── login/
│       └── index.html
├── index.php/
│   └── press-releases/
│       └── index.html
├── ai/
│   ├── index.html
│   ├── ai-agent/
│   │   ├── index.html
│   │   └── en/
│   │       └── index.html
│   ├── moseeker-ats/
│   │   ├── index.html
│   │   └── en/
│   │       └── index.html
│   └── ai-interview/
│       ├── index.html
│       └── en/
│           └── index.html
```

系统只需要 **完整部署该目录即可**。

---

# Functional Requirements

系统至少需要实现：

### 1. 创建站点

例如：

```
POST /deploy
```

请求：

```
tenant: user1
site_package: geo-html.zip
```

行为：

* 解压站点
* 创建站点目录
* 生成对应子域名配置
* 启用站点

---

### 2. 站点访问

部署完成后可以访问：

```
https://user1.deeplumen.com
```

并正确返回静态页面。

---

### 3. 多租户支持

系统应支持多个租户同时存在：

```
user1.deeplumen.com
user2.deeplumen.com
user3.deeplumen.com
```

---

# Technical Requirements

语言要求：

```
Golang 或 Python
```

可以使用任何你认为合理的技术方案，例如：

### Web Framework

Golang

* Gin
* Fiber
* Echo

Python

* FastAPI
* Flask
* Django

---

### 部署方式（任选）

例如：

* Nginx 自动生成配置
* Caddy 自动 TLS
* Docker 容器部署
* 本地文件系统部署
* 云服务器 API

---

# Expected Deliverables

请提交一个 **GitHub 仓库**，包含：

### 1. 源代码

完整实现系统。

---

### 2. README

需要包含：

* 系统架构说明
* 部署方式
* 如何运行

例如：

```
go run main.go
```

或

```
docker compose up
```

---

### 3. Demo Instructions

例如：

```
curl -X POST /deploy ...
```

或简单的 CLI：

```
deploy user1 ./geo-html
```

---

# Bonus (Optional)

以下为加分项：

### 自动 HTTPS

例如：

```
Let's Encrypt
```

---

### Docker 化部署

例如：

```
docker compose up
```

---

### 自动 DNS 管理

例如：

* Cloudflare API
* Route53

---

### UI 控制台

例如：

```
/dashboard
```

---

### 站点删除

例如：

```
DELETE /site/user1
```

---

# Evaluation Criteria

我们主要关注：

### 1. 系统设计能力

例如：

* 多租户隔离
* 子域名路由设计

---

### 2. 工程能力

例如：

* 代码结构
* 可维护性
* 文档清晰度

---

### 3. 自动化程度

例如：

* 是否可以一键部署
* 是否需要手动改配置

---

### 4. AI 使用能力

欢迎使用：

* ChatGPT
* Copilot
* Claude

但需要 **对 AI 生成的代码负责**。

---

# Submission

完成后请将 **GitHub 仓库链接发送给 HR**。

---


我们更看重：

* 设计思路
* 工程结构

而不是功能复杂度。
