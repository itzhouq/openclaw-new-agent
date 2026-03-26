# openclaw-new-agent

> 在 OpenClaw 上丝滑创建多个独立的飞书机器人 Agent

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blue)](https://clawhub.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

仓库地址：https://github.com/itzhouq/openclaw-new-agent

## 痛点：官方配置太复杂，容易改错

在 OpenClaw 上新增一个独立的飞书机器人，官方流程需要：

1. **手动编辑 `openclaw.json`** — 复杂的嵌套 JSON，改错一点就崩
2. **多账号模式配置繁琐** — 需要理解 `channels.feishu.accounts` 的结构
3. **allowFrom 白名单容易遗漏** — 配错了消息就收不到
4. **备份恢复麻烦** — 改坏了不知道怎么回滚
5. **Gateway 重启时机不确定** — 不知道什么时候配置生效

**核心痛点**：配置改错一次，可能需要花半小时排查问题。

---

## 解决方案

`openclaw-new-agent` Skill 核心能力：

- ✅ **自动备份** — 每次变更前自动备份，坏了随时回滚
- ✅ **分步骤引导** — 每一步做什么清清楚楚，不会迷路
- ✅ **allowFrom 自动获取** — 不需要用户记 open_id，发条消息自动识别
- ✅ **配置 patch 化** — 只改需要改的部分，不碰其他配置
- ✅ **验证三步曲** — 创建完自动确认是否成功

---

## 快速开始

### 一键创建飞书机器人（如需新建）

👉 https://open.feishu.cn/page/openclaw?form=multiAgent

直接生成包含 WebSocket 事件订阅的模板应用，无需手动配置权限。

---

## 丝滑创建流程

### Step 0：确认基本信息

告诉 AI 助手：

> "创建一个新机器人：[名称]，[用途]"

示例：
> "创建一个新机器人：码字精，用于写作辅助"

![image-20260326180008946](https://github.com/itzhouq/openclaw-new-agent/blob/master/assets/image-20260326180008946.png)

AI 会自动询问：
- App ID & App Secret（没有？点击上面链接创建）
- 工作区名称（默认建议格式）

![image-20260326175903868](https://github.com/itzhouq/openclaw-new-agent/blob/master/assets/image-20260326175903868.png)

---

![image-20260326180114565](https://github.com/itzhouq/openclaw-new-agent/blob/master/assets/image-20260326180114565.png)

### Step 1：自动备份

AI 自动执行：

```bash
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.before{agentId}bak
```

**每次配置变更前都会备份**，坏了随时回滚。

---

### Step 2：创建工作区

AI 自动创建与主 workspace **平级**的目录：

```
~/.openclaw/
├── workspace/                    # 主工作区
└── workspace-{name}/           # 新工作区
    ├── SOUL.md                 # Agent 角色定义
    ├── USER.md                 # 用户信息
    ├── AGENTS.md               # 工作区说明
    └── memory/                 # 每日日志
```

---

### Step 3：添加配置

AI 使用 `gateway config.patch` 局部更新，**不会覆盖你的其他配置**。

---



![image-20260326180234755](https://github.com/itzhouq/openclaw-new-agent/blob/master/assets/image-20260326180234755.png)

![image-20260326180300244](https://github.com/itzhouq/openclaw-new-agent/blob/master/assets/image-20260326180300244.png)

![image-20260326180334922](https://github.com/itzhouq/openclaw-new-agent/blob/master/assets/image-20260326180334922.png)



### Step 4：自动获取 allowFrom

配置后**不需要你记 open_id**。

你只需要给新机器人发一条消息，AI 自动从日志中识别发送者 ID 并补充到白名单。

---

### Step 5：验证成功

AI 自动检查：

```bash
openclaw doctor --non-interactive
```

确认输出包含新 Agent。

---

## 回滚方案（如果改坏了）

一句话告诉 AI：

> "回滚刚才的配置"

AI 自动执行回滚。

---

## 文件说明

| 文件 | 说明 |
|------|------|
| `~/.openclaw/openclaw.json` | 主配置文件 |
| `~/.openclaw/openclaw.json.before{agentId}bak` | 变更前自动备份 |
| `~/.openclaw/workspace-{name}/` | Agent 工作区 |
| `~/.openclaw/logs/gateway.log` | 日志文件，排查问题用 |

---

## 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| 消息被拒绝 "not in DM allowlist" | 发送者 ID 不在白名单 | AI 自动从日志获取并补充 |
| 配置后没反应 | Gateway 未重启 | 等待 2-3 秒 |
| 消息发到了别的机器人 | 账号混淆 | 确认用正确的机器人对话 |
| 飞书机器人没响应 | 未开启事件订阅 | 用一键创建链接 |

---

## 一句话总结

> **告诉 AI "创建新机器人 + 名称和用途"，剩下的全部自动完成。**

---

## 安装

### Github方式

```
git clone https://github.com/itzhouq/openclaw-new-agent.git
```

### clawhub 方式

```
npx clawhub@latest install openclaw-new-agent
```

或手动复制到 `~/.openclaw/skills/` 目录。

---

## 发布日志

### v1.0.0 (2026-03-26)
- 初始版本
- 支持多飞书机器人创建
- 自动备份和白名单获取
- 完整流程引导

---

## 许可证

MIT License

---

## 相关链接

- [OpenClaw 官方文档](https://docs.openclaw.ai)
- [ClawHub Skill 市场](https://clawhub.com)
- [飞书开放平台](https://open.feishu.cn/)
