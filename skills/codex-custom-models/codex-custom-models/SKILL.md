---
schema_version: 1
name: codex-custom-models
version: 0.1.1
description: >
  Configure and troubleshoot Codex CLI and Claude Code custom model catalogs, providers,
  defaults, and model-picker entries when connecting to an OpenAI-compatible gateway.
metadata:
  short-description: Configure custom model catalogs
  audience:
    - developer
  domain:
    - codex
    - claude-code
    - configuration
  category:
    - configuration
    - developer-tools
  keywords:
    - Codex CLI
    - Claude Code
    - BST AI Gateway
    - OpenAI-compatible gateway
    - model_catalog_json
    - gpt-model-catalog.json
    - custom model
    - model provider
    - model picker
    - codex debug models
    - codex exec -m
    - 自定义模型
    - 模型目录
    - 模型选择器
    - 网关
  visibility:
    - internal
  maturity: experimental
  owner: ai-platform-team
  priority: 50
  depends_on: []
  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---

# Purpose

帮助开发者配置并排查 Codex CLI、Claude Code 通过 OpenAI 兼容网关使用自定义模型时的模型目录、provider、默认模型和 model picker 设置。

# When to use

在以下场景使用：

- 自定义模型未出现在 Codex `/model` 选择器中；
- 需要设置或切换 Codex 默认模型；
- 需要为 Codex 或 Claude Code 配置 provider、网关和模型映射；
- 需要验证模型目录展示与实际调用是否都生效。

不要把本 Skill 用于一般的模型能力比较、采购选型或与配置无关的 LLM 问题。

# Instructions

1. 先确认客户端、CLI 版本、配置文件位置、网关 provider、模型 ID，以及期望的 picker 展示结果。
2. 对 Codex 检查 `~/.codex/config.toml`：
   - 将默认 `model`、`model_provider` 和 `model_catalog_json` 放在顶层；
   - 将网关地址、协议和认证引用放在对应的 `[model_providers.<name>]` 段；
   - 不要把顶层模型目录或默认模型误放进 provider 段。
3. 检查 `~/.codex/gpt-model-catalog.json` 中的每个目标模型：
   - `slug` 是发送给网关的实际模型 ID；
   - `display_name` 只控制展示名称；
   - `visibility: "list"` 才能进入 `/model` 列表；
   - 推理档位和排序字段符合当前 CLI 版本的 schema。
4. 对 Claude Code 使用当前版本支持的用户级 settings 配置网关和模型映射。需要固定显示的条目使用当前版本支持的 `modelPicker` 选项；不要把 Codex 的目录格式直接复制到 Claude Code，也不要使用已弃用的 `ANTHROPIC_SMALL_FAST_MODEL`。
5. 以最小改动更新配置，保留无关设置、现有格式和排序。JSON 必须保持严格语法；凭据仅使用环境变量或本地密钥管理，不写入配置、Skill 或知识库。
6. 依次验证：
   - 运行 `codex debug models`，确认实际加载的模型目录和 `/model` 数据源；
   - 使用 `codex exec -m <model-id>` 对目标模型做一次实际请求；
   - 将“模型可展示”和“网关可调用”分别记录，前者不能证明后者。
7. 如果需要回滚，移除顶层 `model_catalog_json` 或恢复已知可用的 catalog 备份，并再次运行目录检查。
8. 输出修改文件、客户端/provider、模型 ID、picker 预期变化、验证结果以及仍未验证的版本差异或假设。

# Constraints

- Codex 与 Claude Code 的模型 ID、目录格式和 picker 配置不可假设为可直接互换。
- `model_catalog_json` 负责模型列表展示和推理档位定义，不负责改变网关路由；模型能否调用仍取决于网关支持。
- CLI 配置键和版本行为可能变化；缺少当前版本依据时不得臆测字段或结论，升级后重新运行模型目录检查。
- 不提交 Bearer token、API key、客户数据、私有密码或未脱敏日志。
- 不修改与自定义模型配置无关的设置。
