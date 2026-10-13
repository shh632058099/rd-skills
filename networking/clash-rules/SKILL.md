---
schema_version: 1
name: clash-rules
description: "维护和集成 Clash Verge Rev 的广告拦截、代理分流与规则集配置；适用于编辑当前模块的 YAML、更新规则集引用或排查规则匹配顺序。"
metadata:
  short-description: "维护 Clash Verge Rev 分流规则"
  audience:
    - developer
  domain:
    - networking
    - clash
  category:
    - configuration
    - ruleset
  keywords:
    - Clash Verge Rev
    - Clash
    - rule-providers
    - rule-set
    - Shadowrocket
    - ad blocking
    - proxy routing
    - 分流
    - 广告拦截
  visibility:
    - internal
  maturity: experimental
  owner: networking
  priority: 50
  depends_on: []
  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---

# Clash Rules

## Purpose

Use this skill when working on the Clash Verge Rev rule module represented by:

- `clash-verge-rev-JP.yaml`: the integration snippet containing file-based rule providers and rule order;
- `ruleset/sr_top500_banlist_ad_reject.list`: domains rejected as advertisements or telemetry;
- `ruleset/sr_top500_banlist_ad_proxy.list`: domains and CIDRs sent through the `JP` proxy group.

The lists originate from Shadowrocket-ADBlock-Rules-Forever and use Clash classical rules. Treat them as data sources, not as application code.

## Instructions

1. Preserve the provider names and their mapping unless the user explicitly requests a rename:
   - `sr-top500-proxy` -> `ruleset/sr_top500_banlist_ad_proxy.list`
   - `sr-top500-reject` -> `ruleset/sr_top500_banlist_ad_reject.list`
2. Keep both providers as `type: file` and `behavior: classical`; paths are relative to the consuming Clash configuration.
3. Preserve rule precedence. Reject rules must be evaluated first, proxy rules second, then `GEOIP,CN,DIRECT`, with `MATCH,DIRECT` as the final fallback.
4. When adding a rule, place it in the appropriate list and preserve the existing rule syntax (`DOMAIN-SUFFIX`, `IP-CIDR`, or another syntax already supported by the target Clash implementation). Do not silently convert a domain rule into a broader CIDR or suffix.
5. Before changing routing behavior, check whether a target overlaps an existing reject or proxy entry. Explain precedence and the resulting route in the response.
6. Keep credentials, node definitions, and proxy-group secrets out of this module. The `JP` group is supplied by the user's main Clash configuration.
7. For integration output, provide the YAML snippet and state the required relative paths. Do not inline the large rule lists unless the user specifically asks for a standalone expanded configuration.

## Validation

After edits, verify that:

- the YAML parses and contains both `rule-providers` entries;
- each provider path points to an existing file;
- every `RULE-SET` name matches a provider name;
- reject precedes proxy, and `MATCH,DIRECT` remains last;
- list entries are non-empty, use supported classical-rule syntax, and do not contain credentials or accidental full configuration fragments.

If a target Clash client has different support for `IP-CIDR`, IPv6 CIDRs, or file providers, call out that compatibility issue instead of changing semantics implicitly.
