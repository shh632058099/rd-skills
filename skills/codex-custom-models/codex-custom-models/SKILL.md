---
schema_version: 1
name: codex-custom-models
version: 0.1.0
description: >
  Configure Codex and Claude Code custom model catalogs, model providers, and model-picker settings;
  use when adding, changing, or troubleshooting custom model configuration rather than general model selection advice.
metadata:
  short-description: Configure custom model settings
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
    - Codex
    - Claude Code
    - model_catalog_json
    - custom model
    - model provider
    - model picker
    - 自定义模型
    - 模型目录
    - 模型 provider
    - 模型选择器
    - 配置
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

帮助维护 Codex 与 Claude Code 的自定义模型配置，覆盖 `model_catalog_json`、自定义模型、模型 provider 和 model picker 相关设置。

# When to use

使用本 Skill 的场景包括：

- 添加、删除或修改自定义模型；
- 配置模型 provider 或 provider 与模型的关联；
- 调整 model picker 中可显示的模型；
- 排查上述配置未生效、无法选择或格式错误的问题。

不要将本 Skill 用于模型能力比较、采购选型或与配置无关的通用 LLM 使用问题。

# Instructions

1. 先确认目标客户端（Codex、Claude Code 或两者）、配置文件位置、目标 provider、模型标识，以及期望的 model-picker 展示结果。
2. 读取现有配置和适用的当前 schema/产品文档；保留无关配置、现有排序和注释约定，不凭空猜测字段名称或字段含义。
3. 将需求拆分为三类检查：
   - `model_catalog_json` 或模型目录条目是否包含目标模型；
   - provider 配置、认证引用和模型映射是否一致；
   - model picker 是否引用了正确的模型标识并满足客户端支持范围。
4. 以最小改动更新配置。涉及 JSON 时保持严格 JSON 语法；涉及其他配置格式时遵循该文件已有的格式和命名风格。
5. 检查模型 ID、provider ID、引用关系和大小写是否前后一致，并确认没有把密钥、token 或其他凭据写入配置文件。
6. 使用客户端提供的配置校验、启动检查或等价的离线验证确认结果；若无法运行验证，明确说明未验证的部分，不宣称配置已经生效。

# Constraints

- 不修改与自定义模型配置无关的设置。
- 不提交真实凭据、访问令牌、私有端点密码或其他敏感信息；使用已有的安全引用机制。
- 不把模型名称、provider 能力或 schema 字段当作事实臆测；缺少依据时先标记假设并请求确认。
- 发现客户端版本差异时，分别说明各客户端的配置和验证结果。

# Output expectations

输出应至少包含：

- 修改或建议修改的文件和字段；
- 目标客户端、provider 和模型 ID；
- model picker 的预期变化；
- 已执行的验证及其结果；
- 仍需用户确认的版本差异、假设或未验证项。
