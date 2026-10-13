# Example: code to Skill

Developer request:

> 分析当前 OTA 更新实现，把重复使用的部署和故障排查流程提炼成 skill。

Expected approach:

1. Inspect the actual OTA implementation, scripts, and docs.
2. Separate deployment and troubleshooting if they are independent workflows.
3. Produce one focused `SKILL.md` per reusable task.
4. Preserve concrete commands only when they are portable; parameterize local
   paths, hosts, tokens, project names, and versions.
5. Add validation steps derived from tests, logs, or runtime behavior.
6. Classify broadly reusable procedures as `rd-skills`; otherwise use
   `custom-skills`.
7. Wait for an explicit `提交 skill` instruction before calling Team Skill Hub.
