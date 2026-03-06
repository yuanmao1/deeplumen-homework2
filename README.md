# Homework 2 — 多租户静态站点自动部署

## 背景

DeepLumen 需要一套能为不同客户自动部署独立静态站点的系统。每位客户拥有独立的子域名（如 `user1.deeplumen.com`、`user2.deeplumen.com`），彼此完全隔离，互不干扰。

你的任务是用 **Go 或 Python** 实现一个程序，实现上述自动化部署流程。

---

## 需求说明

### 1. 核心功能

- 接收一个静态文件包（目录或压缩包），将其自动部署为一个可公开访问的独立站点。
- 每次部署对应一个租户标识（如用户名或 ID），部署结果绑定到对应的子域名：
  ```
  <tenant>.deeplumen.com
  ```
- 多个租户的站点之间需要完全隔离，不能互相影响。

### 2. 静态文件结构示例

每次部署的输入为一个如下结构的静态文件目录：

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

部署后，以下 URL 应全部可正常访问（以租户 `user1` 为例）：

| URL | 对应文件 |
|-----|----------|
| `http://user1.deeplumen.com/` | `geo-html/index.html` |
| `http://user1.deeplumen.com/jobs/` | `geo-html/jobs/index.html` |
| `http://user1.deeplumen.com/accounts/login/` | `geo-html/accounts/login/index.html` |
| `http://user1.deeplumen.com/index.php/press-releases/` | `geo-html/index.php/press-releases/index.html` |
| `http://user1.deeplumen.com/ai/` | `geo-html/ai/index.html` |
| `http://user1.deeplumen.com/ai/ai-agent/` | `geo-html/ai/ai-agent/index.html` |
| `http://user1.deeplumen.com/ai/ai-agent/en/` | `geo-html/ai/ai-agent/en/index.html` |

### 3. 技术要求

- 实现语言：**Go** 或 **Python**（二选一）。
- 可以使用任意云服务商或自有服务器完成部署，例如：
  - 云对象存储 + CDN（如 AWS S3 + CloudFront、阿里云 OSS + CDN、腾讯云 COS + CDN 等）
  - 云函数 / Serverless 平台（如 Vercel、Netlify、Cloudflare Pages 等）
  - 自有服务器 + 反向代理（如 Nginx、Caddy 等）
- 子域名的 DNS 配置方式请在文档中说明（可以是手动配置通配符解析 `*.deeplumen.com`，也可以通过 DNS API 动态创建）。
- 程序需要有清晰的命令行接口（CLI）或 API 接口，说明如何触发一次部署。

### 4. 多租户隔离要求

- 不同租户的文件存储路径或命名空间必须相互隔离。
- 一个租户的部署/更新/删除操作不应影响其他租户的站点。
- 子域名与租户之间必须有明确的映射关系。

---

## 交付要求

请在本仓库中提交以下内容：

1. **源代码**
   - 完整、可运行的 Go 或 Python 实现。
   - 代码结构清晰，有必要的注释。

2. **文档**（可以是本 `README.md` 的扩展，也可以是单独的 `DESIGN.md`）
   - 架构设计说明：选择了哪种部署方式，为什么。
   - 部署流程说明：如何从静态文件包到可访问的子域名站点。
   - DNS 配置说明：如何配置 `*.deeplumen.com` 通配符解析或动态 DNS。
   - 使用说明：如何运行程序，支持哪些命令行参数或 API。

3. **示例/演示**（加分项）
   - 提供一个可访问的演示链接，或截图说明部署效果。
   - 提供自动化测试脚本或单元测试。

---

## 评分维度

| 维度 | 说明 |
|------|------|
| **功能完整性** | 是否能够将静态文件正确部署到对应子域名，所有路由均可访问 |
| **多租户隔离** | 不同租户之间是否真正隔离，不会互相影响 |
| **代码质量** | 代码结构、可读性、错误处理 |
| **文档清晰度** | 架构设计和使用说明是否清晰 |
| **扩展性** | 系统能否方便地扩展租户数量或支持更多功能（更新、删除等） |
| **演示效果（加分）** | 是否有实际可访问的演示 |

---

## 注意事项

- 不要在代码或文档中提交任何真实的密钥、Token 或账号信息，请使用环境变量或配置文件占位符。
- 如果使用云服务，不要求使用 `deeplumen.com` 真实域名，可以使用自己的测试域名或本地 `hosts` 文件模拟子域名。
- 如有疑问，欢迎在 Issue 中提问。

---

## 示例调用接口（仅供参考，不强制）

以下是一个参考性的 CLI 接口设计，你也可以设计成 HTTP API 或其他形式：

```bash
# 部署一个新租户的站点
deploy --tenant user1 --source ./geo-html/

# 更新已有租户的站点
deploy --tenant user1 --source ./geo-html-v2/ --update

# 删除一个租户的站点
deploy --tenant user1 --delete
```

---

祝编码顺利！ 🚀
