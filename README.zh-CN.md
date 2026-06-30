# OKKI Claw

OKKI Claw 是一个本地 sidecar，用于把 OpenClaw 以及本地 Agent（Claude Code、Opencode、Codex、Oh My PI 等）工作流连接到 OKKI Claw agent relay，让你无需手动配置内网穿透，也能在其他设备上继续使用本地 Agent。

语言：[English](README.md) | 简体中文

<p align="left">
  <a href="https://agent-relay.okki.com">中文官网</a>
  ·
  <a href="https://agent-relay.okki.ai">海外官网</a>
  ·
  <a href="https://github.com/okki-claw/okki-claw/releases/latest">下载最新版本</a>
  ·
  <a href="https://github.com/okki-claw/okki-claw/issues">反馈问题</a>
</p>

## 它解决什么问题

OKKI Claw 运行在 OpenClaw gateway 所在的本机上。它会主动连接到 OKKI Claw agent relay，并把经过授权的 Agent 流量通过该 relay 转发出去。

- 在另一台电脑、平板或手机浏览器上访问本地 OpenClaw 会话。
- 避免手动配置 ngrok、Tailscale、反向代理或路由器端口转发。
- OpenClaw gateway 凭证和设备身份保留在本地设备上。
- 通过公开 release 包分发，不在本公开仓库暴露 OKKI Claw 源码。

## 官网

- 中文：<https://agent-relay.okki.com>
- 海外：<https://agent-relay.okki.ai>

## 仓库范围

这个公开仓库是分发仓库，只包含可公开的信息：

- 安装和使用文档。
- 发布说明和下载链接。
- `okki-claw-*.tgz` 等 GitHub Release 产物。
- 安全问题反馈说明。

源码、构建流水线、私有配置、签名和发布凭证保存在独立私有仓库中。

## 工作方式

```text
远程浏览器 / OKKI Claw App
        |
        | HTTPS / WebSocket
        v
OKKI Claw cloud relay
        |
        | 已认证的出站 tunnel
        v
本机 OKKI Claw sidecar
        |
        | 本地 gateway 连接
        v
OpenClaw gateway 和本地 Agent 工作流
```

云端 relay 不应该接收你的本地 OpenClaw 凭证。OKKI Claw 会在本机发现或读取 gateway 配置，并只在本地使用这些凭证。

## 系统要求

- Node.js 22 或更高版本。
- 本机已运行 OpenClaw gateway 或兼容的本地 Agent gateway。
- OKKI Claw 账号，以及从网页端获取的 pairing token。
- 本机可以访问 OKKI Claw relay endpoint。

## 安装方式

你可以选择以下任一安装路径：

- **Agent 自动安装**：如果你已经在使用 Claude Code、Opencode、Codex、Oh My PI 或其他本地 coding agent，推荐让 agent 帮你下载安装最新 OKKI Claw release。
- **手动安装**：如果你希望自己执行每一步命令，推荐使用这种方式。

两种方式安装的是同一个 release 包，后续配对和启动流程也一致。

## 快速开始：Agent 自动安装

如果你希望本地 coding agent 帮你完成安装，可以使用这个流程。

### 1. 在 OKKI Grow 后台生成安装 prompt

打开 OKKI Grow 后台，创建或选择要连接的设备，然后生成 OKKI Claw 的 agent 安装 prompt。

后台生成的 prompt 会包含适用于当前环境的安装指令。将它复制到你要安装 OKKI Claw 的机器上的本地 agent 中执行。

按照 agent 中的生成 prompt 执行，直到它确认 `okki-claw --help` 可以正常运行。如果 OKKI Grow 生成的 prompt 中包含 pairing token，请不要公开分享这段 prompt。

### 2. 配对当前设备

从 OKKI Claw 获取 pairing token 后，你可以自己执行命令，也可以让 agent 在本机执行：

```bash
okki-claw pair --pairing-token <token> --device-name "my-openclaw"
```

配对会把 sidecar 凭证保存到本地 OKKI Claw 配置中。请不要分享该配置文件。

### 3. 运行 sidecar

```bash
okki-claw daemon
```

sidecar 连接成功后，你可以在另一台设备打开 OKKI Claw 网页端，并选择这个已配对设备。

## 快速开始：手动安装

如果你希望自己完成安装，可以使用这个流程。

### 1. 下载 release 包

打开 [GitHub Releases](https://github.com/okki-claw/okki-claw/releases/latest)，下载最新的 `okki-claw-*.tgz` 包。

### 2. 全局安装

进入下载包所在目录，运行：

```bash
npm install -g ./okki-claw-<version>.tgz
okki-claw --help
```

如果你不想全局安装，也可以下载 release 产物后，使用你偏好的 Node.js 包工具运行。

### 3. 配对当前设备

从 OKKI Claw 获取 pairing token 后，配对当前本机：

```bash
okki-claw pair --pairing-token <token> --device-name "my-openclaw"
```

配对会把 sidecar 凭证保存到本地 OKKI Claw 配置中。请不要分享该配置文件。

### 4. 运行 sidecar

```bash
okki-claw daemon
```

sidecar 连接成功后，你可以在另一台设备打开 OKKI Claw 网页端，并选择这个已配对设备。

## 配置

常见环境变量：

| 变量 | 用途 |
| --- | --- |
| `AGENT_RELAY_CONFIG_PATH` | 自定义 OKKI Claw 本地配置路径。 |
| `RELAY_URL` | OKKI Claw tunnel endpoint。 |
| `OPENCLAW_GATEWAY_URL` | 自动发现不可用时指定本地 OpenClaw gateway URL。 |
| `OPENCLAW_GATEWAY_TOKEN` | 本地 OpenClaw gateway token，仅在本机使用。 |
| `SIDECAR_AUTO_UPDATE` | 启用或关闭自动更新检查。 |

这些环境变量只应该设置在运行 OKKI Claw 的本机上。请不要把它们粘贴到公开 issue 中。

## 更新

检查并应用更新：

```bash
okki-claw check-update
okki-claw update
```

如果启用了 daemon 自动更新，OKKI Claw 可以定期检查新版本，并在安装成功后重启到新版本。

## 安全模型

OKKI Claw 的安全边界很简单：本地凭证留在本地。

- OpenClaw gateway 凭证不应该上传到 relay。
- 设备配对凭证保存在你的本机。
- relay 只转发经过授权的流量，不应该变成通用 TCP 隧道。
- release 产物由私有仓库构建，然后发布为可下载包。
- 公开 issue 中不要包含 `.env` 文件、token、私钥或本地 identity 文件。

安全问题反馈请参考 [SECURITY.md](SECURITY.md)。

## 常见问题排查

### 找不到 `okki-claw` 命令

确认全局 npm bin 目录已经加入 `PATH`，然后重新安装下载的 release 包。

### 配对失败

检查 pairing token 是否仍然有效、本机时间是否正确，以及本机是否能访问 relay endpoint。

### sidecar 无法连接 OpenClaw

确认 OpenClaw 已在本机运行。如果自动发现失败，可以在本机设置 `OPENCLAW_GATEWAY_URL`，必要时再设置 `OPENCLAW_GATEWAY_TOKEN`。

### 远程设备看不到这台机器

确认 `okki-claw daemon` 仍在运行，并且本机可以访问配置的 relay endpoint。

## Release 产物

每个 GitHub Release 应包含：

- `okki-claw-*.tgz` 安装包。
- 面向用户的变更说明。
- 可用时提供 SHA256 等完整性校验信息。

请只安装本仓库官方 releases 发布的产物。

## 支持

- 中文官网：<https://agent-relay.okki.com>
- 海外官网：<https://agent-relay.okki.ai>
- Issues：<https://github.com/okki-claw/okki-claw/issues>
