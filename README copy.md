好的，这是一个根据您提供的资料和需求，并参考了主流开源项目风格设计的 `README.md` 文件。它结构清晰、信息全面，旨在为您的项目 `Proxify` 打造一个专业且吸引人的第一印象。

---

直接复制以下 Markdown 内容并保存为 `README.md` 即可。

```markdown
<div align="center">
  <a href="https://github.com/poixeai/proxify">
    <img src="https://proxify.poixe.com/logo.svg" alt="Proxify Logo" width="120" height="120">
  </a>
  <h1>Proxify</h1>
  <p>一个开源、轻量、高性能的 AI 接口反向代理网关</p>
  <p>
    <strong>支持 OpenAI、Anthropic (Claude)、Google (Gemini)、DeepSeek 等几乎所有主流 AI 模型厂商</strong>
  </p>

  <p>
    <a href="https://github.com/poixeai/proxify/blob/main/LICENSE">
      <img alt="License" src="https://img.shields.io/github/license/poixeai/proxify?style=for-the-badge&color=blue">
    </a>
    <a href="https://github.com/poixeai/proxify">
      <img alt="Go Version" src="https://img.shields.io/github/go-mod/go-version/poixeai/proxify?style=for-the-badge">
    </a>
    <a href="https://github.com/poixeai/proxify/stargazers">
      <img alt="Stars" src="https://img.shields.io/github/stars/poixeai/proxify?style=for-the-badge&logo=github">
    </a>
    <a href="https://github.com/poixeai/proxify/issues">
      <img alt="Issues" src="https://img.shields.io/github/issues/poixeai/proxify?style=for-the-badge&logo=github">
    </a>
  </p>
  <h4>
    <a href="https://proxify.poixe.com">官方网站</a>
    <span> · </span>
    <a href="https://github.com/poixeai/proxify">查看源码</a>
    <span> · </span>
    <a href="https://github.com/poixeai/proxify/issues/new">报告 Bug</a>
  </h4>
</div>

---

**Proxify** 是一个专为 AI 服务设计的开源、轻量级、自托管反向代理解决方案。它允许开发者通过统一的入口访问各类大模型 API，解决了地区限制、多服务配置复杂等问题。Proxify 对 LLM 的流式响应进行了深度优化，确保了最佳的性能和用户体验。

## ✨ 功能特性

*   💎 **强大扩展能力**：不仅是 AI 接口网关，Proxify 也是一个通用的反向代理服务器。我们对 LLM API 做了专项优化，包括流式传输、心跳保活、尾部冲刺等。
*   🚀 **统一 API 入口**：通过一级路径即可路由到不同上游，例如 `/openai` → `api.openai.com`，`/gemini` → `generativelanguage.googleapis.com`。所有路由规则一处配置，简单高效。
*   ⚡ **轻量与高性能**：后端采用 Golang 构建，原生支持高并发，资源占用极低。在 0.5 GB 内存的服务器上也能轻松流畅运行。
*   🚄 **极致流式优化**：
    *   **平滑输出**：内置流控器，将大模型快速生成的文本块平滑地以“打字机”效果流式传输给客户端。
    *   **心跳维持**：在 SSE (Server-Sent Events) 流中自动插入心跳消息，有效防止因网络空闲导致的连接意外中断。
    *   **尾部冲刺**：在保障丝滑输出的同时，通过尾部冲刺技术将最坏延迟控制在可接受范围，优化最终响应时间。
*   🛡️ **安全与隐私**：自托管部署，所有请求数据完全在您自己的掌控之下，不经过任何第三方服务器，彻底杜绝隐私泄露风险。
*   🌐 **广泛兼容性**：已预置 OpenAI、Azure、Claude、Gemini、DeepSeek 等主流 AI 服务商的路由，同时支持通过配置文件便捷地横向扩展到任意 API。
*   🔧 **极致易用**：从现有服务切换到 Proxify，通常只需修改一行 `baseURL` 配置，无需改动任何业务代码或请求参数。
*   👨‍💻 **开源与专业**：项目由 AI 领域资深团队设计与维护，代码完全开源、透明可审计，杜绝供应商锁定，欢迎社区贡献（PRs / Issues）。

## 🛠️ 技术栈

*   **后端网关**: Golang + Gin
*   **前端面板**: React + Vite + TypeScript + Tailwind CSS

## 🚀 快速开始

将您现有的服务对接到 Proxify 只需三步。

#### 1. 确定目标服务

浏览我们支持的 [API 列表](#-广泛兼容的-api-端点)，找到您需要的服务及其对应的代理路径前缀（Path）。

#### 2. 替换基础 URL

在您的代码中，将原始 API 的基础 URL 替换为您的 Proxify 部署地址，并附加上一步中确定的路径前缀。

- **原始地址**: `https://api.openai.com/v1/chat/completions`
- **替换为**: `http://<您的-Proxify-地址>/openai/v1/chat/completions`

#### 3. 发送请求

一切就绪！使用您原有的 API 密钥和请求参数，像往常一样发起请求。您的 API 密钥、请求头（Header）和请求体（Body）等所有其他部分都保持不变。

#### 代码示例 (Node.js - OpenAI SDK)

```javascript
// Node.js example using /openai proxy endpoint
import OpenAI from "openai";

const openai = new OpenAI({ 
    // highlight-start
    apiKey: "sk-...", // 您的 OpenAI API Key
    baseURL: "http://127.0.0.1:8080/openai/v1", // 指向您的 Proxify 服务
    // highlight-end
});

async function main() {
    const stream = await openai.chat.completions.create({
        model: "gpt-4",
        messages: [{ role: "user", content: "你好，请介绍一下自己" }],
        stream: true,
    });

    for await (const chunk of stream) {
        process.stdout.write(chunk.choices[0]?.delta?.content || "");
    }
}

main();
```

## 🖥️ 部署教程

### 方式一：使用 Docker (推荐)

我们强烈推荐使用 Docker 进行部署，这是最简单快捷的方式。

1.  **准备配置文件**
    在您的服务器上创建一个 `routes.json` 文件，用于定义路由规则。这是一个示例：
    ```json
    {
      "/openai": {
        "target": "https://api.openai.com"
      },
      "/deepseek": {
        "target": "https://api.deepseek.com"
      },
      "/claude": {
        "target": "https://api.anthropic.com"
      }
    }
    ```

2.  **运行 Docker 容器**
    将宿主机的 `routes.json` 文件挂载到容器中，并暴露端口。

    ```bash
    docker run -d \
      --name proxify \
      -p 8080:8080 \
      -v /path/to/your/routes.json:/app/routes.json \
      --restart=always \
      poixeai/proxify:latest
    ```
    *   请将 `/path/to/your/routes.json` 替换为您配置文件的实际路径。
    *   服务将在 `http://<您的服务器IP>:8080` 上运行。

### 方式二：手动编译部署

如果您希望从源码构建，请按以下步骤操作。

**环境要求:**
*   Go (版本 1.20+)
*   Node.js (版本 18+)
*   pnpm (或 npm/yarn)

**步骤:**

1.  **克隆仓库**
    ```bash
    git clone https://github.com/poixeai/proxify.git
    cd proxify
    ```

2.  **编译前端**
    ```bash
    cd web
    pnpm install
    pnpm build
    cd ..
    ```

3.  **编译后端**
    ```bash
    go build -o proxify .
    ```

4.  **准备配置文件**
    在可执行文件 `proxify` 同级目录下创建 `routes.json` 文件（内容同上）。

5.  **运行**
    ```bash
    ./proxify
    ```
    默认情况下，服务将在 `8080` 端口启动。

## ⚙️ 配置说明

Proxify 主要通过环境变量进行配置。

| 环境变量           | 类型    | 默认值          | 描述                                         |
|--------------------|---------|-----------------|----------------------------------------------|
| `PROXIFY_PORT`     | `Int`   | `8080`          | 服务监听的端口                               |
| `PROXIFY_ROUTES`   | `String`| `routes.json`   | 路由配置文件的路径                           |
| `PROXIFY_TIMEOUT`  | `Int`   | `300`           | 上游请求的超时时间（秒）                       |
| `PROXIFY_LOG_LEVEL`| `String`| `info`          | 日志级别，可选值: `debug`, `info`, `warn`, `error` |

## 🗺️ 广泛兼容的 API 端点

Proxify 支持代理任何 HTTP 服务。以下是一些预设的、经过优化的常用 AI 服务路由示例。

| 服务商      | 建议路径 (Path) | 目标地址 (URL)                           |
|-------------|-----------------|------------------------------------------|
| **OpenAI**  | `/openai`       | `https://api.openai.com`                 |
| **Azure**   | `/azure`        | `https://<your-res>.openai.azure.com`    |
| **DeepSeek**| `/deepseek`     | `https://api.deepseek.com`               |
| **Claude**  | `/claude`       | `https://api.anthropic.com`              |
| **Gemini**  | `/gemini`       | `https://generativelanguage.googleapis.com` |
| **Grok**    | `/grok`         | `https://api.x.ai`                       |
| **阿里云**   | `/aliyun`       | `https://dashscope.aliyuncs.com`         |
| **火山引擎** | `/volcengine`   | `https://ark.cn-beijing.volces.com`      |

*注意：实际可用路径取决于您的 `routes.json` 配置文件。*

## 🤝 贡献

我们欢迎并感谢所有形式的贡献！无论是提交 Issue、发起 Pull Request，还是改进文档，都是对社区的巨大支持。

请在贡献前阅读我们的《贡献指南》（`CONTRIBUTING.md` - 待创建）。

## 📄 开源协议

本项目采用 [MIT License](./LICENSE) 开源协议。

---

<div align="center">
  <p>Powered by <a href="https://poixe.com">Poixe AI</a></p>
</div>
```