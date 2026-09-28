# Skill 开发与提交流程

本文面向在 `rd-skills` 仓库中开发、维护和提交 Skill 的研发人员。

目标是让 Skill 从创建、校验、评审到合入后被 Team Skill Hub 自动同步，形成稳定、可追踪、可回归的开发流程。

## 1. 开发流程总览

```text
Developer
  ↓
基于模板创建 Skill
  ↓
编写 SKILL.md
  ↓
补充 references / examples
  ↓
本地校验
  ↓
补充 Evaluation
  ↓
提交分支
  ↓
Merge Request
  ↓
CODEOWNERS Review + CI
  ↓
Merge 到 main
  ↓
Team Skill Hub 自动同步
  ↓
Skill 可被 MCP search / resolve / get
```

不要直接修改生产中的 Hub 数据。Git 仓库是 Skill 的唯一事实来源。

## 2. 仓库目录约定

推荐一个业务域一个一级目录，一个 Skill 一个子目录：

```text
rd-skills/
├── ota/
│   ├── EVALUATION.yaml
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   │   └── review-checklist.md
│   │   └── examples/
│   │       └── example.md
│   └── deployment/
│       └── SKILL.md
├── templates/
├── schemas/
└── CODEOWNERS
```

约定：

- Skill 的入口文件必须命名为 `SKILL.md`。
- Skill 名称应在仓库内唯一。
- 复杂背景资料放入 `references/`，不要全部塞进 `SKILL.md`。
- 可复用输入/输出示例放入 `examples/`。
- 一个 Skill 应解决一个相对明确的问题，不要做成“万能 Skill”。

## 3. 创建一个新 Skill

优先从模板复制：

```bash
mkdir -p ota/my-skill
cp templates/SKILL.template.md ota/my-skill/SKILL.md
```

然后修改 Front Matter 和正文。

最小示例：

```yaml
---
schema_version: 1
name: ota-my-skill
version: 0.1.0
description: >
  Describe when this skill should be selected and what problem it solves.

metadata:
  audience:
    - developer

  domain:
    - ota

  category:
    - development

  keywords:
    - ota
    - keyword

  visibility:
    - internal

  maturity: experimental
  owner: ota-team
  priority: 50
  depends_on: []

  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---
```

正文建议至少包含：

```markdown
# Purpose

这个 Skill 解决什么问题，什么时候应该使用。

# Instructions

1. 具体执行步骤。
2. 判断条件。
3. 必须遵守的约束。
4. 失败时如何处理。
5. 输出中必须包含什么。

# Resources

需要时再加载 references/ 下的资料。
```

## 4. Metadata 字段规范

### schema_version

当前必须为：

```yaml
schema_version: 1
```

### name

要求：

- 仓库内唯一；
- 使用稳定、可搜索的名称；
- 推荐 kebab-case；
- 不要轻易修改已发布 Skill 的名称。

例如：

```yaml
name: ota-code-review
```

### version

推荐使用 SemVer：

```text
0.1.0   experimental
0.2.0   增加能力
1.0.0   稳定发布
1.1.0   向后兼容功能增强
2.0.0   有明显不兼容变化
```

### description

Description 不是标题，而是搜索和路由的重要输入。

应写清：

- 什么时候使用；
- 解决什么问题；
- 与相近 Skill 的主要区别。

避免：

```text
OTA skill
Useful development helper
```

推荐：

```text
Review an OTA implementation with focus on unnecessary APIs,
excessive call chains, redundant abstractions, dead code,
maintainability and deletion risk.
```

### audience

示例：

```yaml
audience:
  - developer
```

不要把仅供内部研发使用的 Skill 标成 customer。

### domain

用于业务域过滤：

```yaml
domain:
  - ota
  - embedded
```

### category

表示能力类型，例如：

```yaml
category:
  - code-review
  - debugging
  - deployment
```

### keywords

Keywords 直接影响 Skill 搜索与路由。

建议同时覆盖：

- 中文关键词；
- 英文关键词；
- 常见同义词；
- 用户实际会输入的表达。

例如：

```yaml
keywords:
  - ota
  - api
  - code review
  - call chain
  - 接口
  - 调用链
  - 代码审查
```

不要堆大量无关关键词来提高命中率。

### visibility

内部 Skill：

```yaml
visibility:
  - internal
```

客户可见 Skill 应放入对应客户仓库，并按仓库权限策略控制，不要仅依赖 Skill 自身的 metadata 隔离敏感内容。

### maturity

支持：

```text
experimental
beta
stable
deprecated
```

新 Skill 默认建议：

```yaml
maturity: experimental
```

经过实际使用和 Evaluation 后再升级到 `beta` / `stable`。

### owner

必须填写明确负责团队：

```yaml
owner: ota-team
```

owner 应与 CODEOWNERS 责任关系一致。

### priority

范围：

```text
0 - 100
```

priority 只用于同等相关性下的排序，不应通过设置 100 来掩盖 description / keywords 不准确的问题。

### depends_on

没有依赖：

```yaml
depends_on: []
```

存在依赖时填写 Skill 名称：

```yaml
depends_on:
  - ota-base-diagnostics
```

不要制造不必要的依赖链。

### compatibility

明确哪些客户端适用：

```yaml
compatibility:
  codex: true
  claude_code: true
  customer_agent: false
```

## 5. SKILL.md 正文怎么写

Skill 不是知识文章，而是给 Agent 使用的执行说明。

优先写：

- 触发条件；
- 分析步骤；
- 决策规则；
- 风险约束；
- 输出格式；
- 什么时候读取额外资源。

避免只写：

- 大量背景知识；
- 泛泛的“请仔细分析”；
- 没有判断标准的原则；
- 与 Skill 任务无关的长篇资料。

推荐结构：

```markdown
# Purpose

# When to use

# Instructions

# Constraints

# Output expectations

# Resources
```

Instruction 应尽量可执行，例如：

```text
差：
Review the code carefully.

好：
For every removable API candidate:
1. identify all known callers;
2. identify compatibility impact;
3. estimate deletion risk as Low / Medium / High;
4. state required validation before deletion.
```

## 6. references 和 examples

### references/

用于存放只在需要时加载的深度资料，例如：

- 代码审查 checklist；
- 协议规范；
- OTA 状态机说明；
- Yocto CVE 处理规则；
- 项目约束；
- FAQ。

Skill 正文中应明确什么时候读取：

```markdown
Load `references/review-checklist.md` when a detailed review checklist is needed.
```

### examples/

建议包含真实但脱敏的示例：

- 用户问题；
- 典型输入；
- 期望输出结构；
- 正例；
- 必要时的反例。

不要提交：

- 密钥；
- Token；
- 客户隐私数据；
- 未脱敏日志；
- 公司禁止进入 Git 的内容。

## 7. 为 Skill 增加 Evaluation

一个 Skill 只要承担稳定业务能力，就应该有回归用例。

Evaluation 通常放在业务域目录：

```text
ota/EVALUATION.yaml
```

例如：

```yaml
schema_version: 1
id: ota-core-regression
description: OTA core Skill regression suite.

cases:
  - id: resolve-my-skill
    description: A typical request should resolve to the new Skill.
    target: skill
    operation: resolve
    query: review OTA update code for redundant APIs
    client: codex
    expect:
      selected: ota-my-skill
```

至少建议覆盖：

- 一个典型 resolve；
- 一个常见同义表达；
- 对稳定 Skill 的关键行为约束；
- 如果新增 Skill 与旧 Skill 很接近，增加防误匹配用例。

Evaluation 的目的不是证明“能搜到”，而是防止未来改 metadata、排序或新增 Skill 后出现路由回归。

## 8. 本地校验

Team Skill Hub 提供统一的 validator 和 evaluator。

推荐工作区结构：

```text
workspace/
├── team-skill-hub/
└── rd-skills/
```

首次准备：

```bash
cd team-skill-hub
npm ci
```

校验整个 Skill 仓库：

```bash
npm run validate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --revision working-tree
```

运行 Evaluation：

```bash
npm run evaluate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --revision working-tree
```

只运行某个 Suite：

```bash
npm run evaluate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --suite ota-core-regression
```

提交前要求：

```text
validate:repo 通过
+
相关 Evaluation 通过
```

如果只是修改文案，也要确认不会改变 Skill 的搜索和 resolve 行为。

## 9. 开发分支与提交

不要直接在 `main` 上开发。

推荐：

```bash
git checkout main
git pull

git checkout -b skill/ota-my-skill
```

完成开发后：

```bash
git status
git diff

git add ota/my-skill ota/EVALUATION.yaml
git commit -m "feat: add OTA my skill"
git push -u origin skill/ota-my-skill
```

推荐提交类型：

```text
feat:     新增 Skill / 新能力
fix:      修复错误的 Skill 行为或 metadata
docs:     仅文档说明
refactor: 不改变预期行为的结构调整
test:     Evaluation 变更
chore:    模板、Schema 或维护性改动
```

一个 MR 尽量只解决一个明确主题。

## 10. Merge Request 要写什么

MR 描述建议包含：

```markdown
## What

新增/修改了什么 Skill。

## Why

解决什么真实研发问题。

## Routing

哪些用户表达应该命中这个 Skill。
哪些表达不应该命中。

## Risk

是否会与已有 Skill 竞争。
是否会改变已有 resolve 结果。
是否包含权限或客户可见性变化。

## Validation

- [ ] validate:repo passed
- [ ] evaluation passed
- [ ] added/updated regression cases
- [ ] no secrets or sensitive customer data
```

如果修改：

- `name`
- `visibility`
- `owner`
- `depends_on`
- `compatibility`
- 高 priority Skill

需要在 MR 中特别说明影响。

## 11. Review 与 CODEOWNERS

当前仓库按目录配置 CODEOWNERS，例如：

```text
/ota/** @ota-team
/templates/** @ai-platform-team
/schemas/** @ai-platform-team
```

因此：

- OTA Skill 由 OTA owner review；
- Schema / Template 变更由 AI Platform owner review；
- 不要为了绕过 Review 把文件放到错误目录。

Reviewer 重点检查：

1. Skill 是否真的需要新增；
2. 是否与现有 Skill 重复；
3. description / keywords 是否足够准确；
4. Instructions 是否可执行；
5. visibility 是否正确；
6. 是否泄露内部/客户敏感内容；
7. Evaluation 是否覆盖关键路由；
8. 是否引入不必要依赖；
9. 是否会影响已有 Skill resolve。

## 12. Merge 后发生什么

Merge 到 GitLab 的 `main` 后：

```text
GitLab Push
  ↓
Webhook
  ↓
Team Skill Hub fetch/reset
  ↓
扫描 Skill / Prompt / Agent
  ↓
Validation
  ↓
Evaluation/CI 负责合入前回归
  ↓
创建 validated snapshot
  ↓
原子激活新 revision
```

如果新 revision 校验失败，Hub 不应激活错误 revision，而是继续使用 Last Known Good。

开发者不需要手工登录 Hub 修改 Skill。

## 13. 如何验证已经上线

开发者可以通过 MCP 验证：

```text
list_skills
search_skills
resolve_skill
get_skill
```

例如搜索：

```text
search_skills("review OTA unnecessary APIs")
```

或者：

```text
resolve_skill("帮我检查 OTA 代码中哪些接口可以删除")
```

重点验证：

- 新 Skill 能命中正确请求；
- 不相关请求不会错误命中；
- client compatibility 生效；
- visibility / role 隔离符合预期。

如果刚 Merge 后暂时看不到更新，优先检查 GitLab CI / Webhook / Hub sync 状态，而不是重复提交 Skill。

## 14. 更新已有 Skill

更新已有 Skill 时，应优先保持 `name` 不变。

常见更新：

- 调整 Instructions；
- 增加 references；
- 增加 keywords；
- 修正 description；
- 增加 Evaluation；
- 提升 maturity；
- 修改 compatibility。

如果行为发生明显变化，应同步升级 `version`。

如果 Skill 已被其他 Skill / Agent 依赖，不要直接删除或改名，应先检查 `depends_on` 和 Agent binding。

## 15. Deprecated 与删除

不建议直接删除已经稳定使用的 Skill。

推荐先：

```yaml
maturity: deprecated
```

并在正文说明替代 Skill。

确认：

- 没有 Agent 依赖；
- 没有其他 Skill depends_on；
- Evaluation 已迁移；
- 使用方已切换；

之后再删除。

## 16. 提交前 Checklist

```text
[ ] SKILL.md 文件名正确
[ ] schema_version = 1
[ ] name 唯一且稳定
[ ] description 能准确说明触发场景
[ ] keywords 包含真实用户表达
[ ] visibility 正确
[ ] owner 正确
[ ] maturity 合理
[ ] priority 没有滥用
[ ] compatibility 正确
[ ] references/examples 无敏感信息
[ ] validate:repo 通过
[ ] Evaluation 通过
[ ] 新能力已增加回归用例
[ ] MR 描述了 Routing 和 Risk
[ ] CODEOWNERS Reviewer 已参与
```

## 17. 一个完整示例

新建：

```text
ota/deployment-troubleshooting/
├── SKILL.md
├── references/
│   └── troubleshooting-checklist.md
└── examples/
    └── failed-upgrade.md
```

然后：

```bash
cd ../team-skill-hub

npm run validate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --revision working-tree

npm run evaluate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --suite ota-core-regression
```

通过后：

```bash
cd ../rd-skills

git checkout -b skill/ota-deployment-troubleshooting
git add ota/deployment-troubleshooting ota/EVALUATION.yaml
git commit -m "feat: add OTA deployment troubleshooting skill"
git push -u origin skill/ota-deployment-troubleshooting
```

创建 MR，Review/CI 通过后合入 `main`，由 Team Skill Hub 自动同步。
