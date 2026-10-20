# WorldFirst（万里汇）独立远程分流规则

将匹配到的 WorldFirst 自有域名交给客户端中**已有的英国节点或策略**。优先适配 Loon，兼容 Clash Verge Rev（Mihomo）。两个规则文件只定义匹配范围，不指定出口策略。

本项目不提供代理节点、代理订阅或完整客户端配置，不涉及服务器修改。使用英国出口不保证通过 WorldFirst 的地区审核、身份验证或风控。

## 远程规则地址

- 仓库：[worldfirst-rules](https://github.com/daixianglong199-dotcom/worldfirst-rules)
- Loon：[WorldFirst.list](https://raw.githubusercontent.com/daixianglong199-dotcom/worldfirst-rules/main/rules/WorldFirst.list)
- Mihomo：[WorldFirst.yaml](https://raw.githubusercontent.com/daixianglong199-dotcom/worldfirst-rules/main/rules/WorldFirst.yaml)

`main` 地址随仓库更新。发布和可用性检查状态见 [VALIDATION.md](VALIDATION.md)。

## 规则范围

共 **3 条**规则，两种格式的范围完全一致。

| 类型 | 域名 | 用途和边界 |
| --- | --- | --- |
| DOMAIN-SUFFIX | worldfirst.com | 匹配主域名及其所有层级子域，包括英国官网、portal.worldfirst.com、docs.worldfirst.com 等；不重复列出已覆盖子域。 |
| DOMAIN | portal.worldfirst.com.cn | 仅精确匹配中国站账户门户；不匹配其下级子域。 |
| DOMAIN | www.worldfirst.com.cn | 仅精确匹配中国站官网；不匹配其下级子域。 |

`worldfirst.com.cn` 根域名和其他未列出的 `.com.cn` 子域不匹配。没有关键字规则，也没有添加 Alibaba、AliExpress、Taobao、其他金融服务或共享第三方域名。来源及候选域名见 [SOURCES.md](SOURCES.md)。

## Loon 导入

以下为接入说明，需由使用者自行操作。先备份客户端配置。

1. 在 Loon 的规则订阅/远程规则管理中新增上述 `.list` Raw 地址。
2. 将策略选为已有的英国节点，或一个已选中英国节点的现有策略组；启用该订阅并更新规则。
3. 将它排在其他可能命中 WorldFirst 的**远程域名规则集**之前。
4. 保持规则分流模式，检查请求日志中的命中规则和实际出口。

如手动接入配置，在现有 `[Remote Rule]` 段新增一行，不重复创建整个段：

```ini
[Remote Rule]
https://raw.githubusercontent.com/daixianglong199-dotcom/worldfirst-rules/main/rules/WorldFirst.list, policy=英国策略占位符, tag=WorldFirst, enabled=true
```

把 `英国策略占位符` 替换为客户端中真实存在的英国节点/策略名称。本项目不定义该策略，也不更改代理订阅。

### Loon 优先级与直连冲突

根据 [Loon 官方规则文档](https://nsloon.app/docs/Rule/)，3.0.3 及以上版本对域名目标先匹配域名规则，无匹配时才解析 DNS 并匹配 IP 规则；来源优先级为**本地规则 > 插件规则 > 订阅规则**。同一来源内按规则顺序决定优先级。

因此，仅把本订阅放在 `[Remote Rule]` 顶部，不能覆盖本地或插件中已命中的 WorldFirst 直连域名规则。若日志显示 DIRECT：

1. 先确认本订阅已启用、下载成功、包含三条规则，且所选策略实际使用英国节点。
2. 在规则查询/请求日志中检查 `www.worldfirst.com`、`portal.worldfirst.com`、`portal.worldfirst.com.cn` 和 `www.worldfirst.com.cn` 的命中来源。
3. 查找已有本地、插件和远程规则中的 `worldfirst.com`、`worldfirst.com.cn`，以及可能覆盖它们的更宽泛域名规则。若冲突来自其他远程规则集，优先调整本订阅在远程规则中的顺序。
4. 若冲突来自本地或插件，记录具体条目后再由使用者决定如何处理其针对 WorldFirst 的冲突；本任务不自动改动它们。不要删除整套中国直连规则或改变默认 DIRECT、GEOIP、FINAL。
5. 更新规则后重新建立测试连接，避免把已有连接/缓存的旧路径当作新规则结果。

## Clash Verge Rev / Mihomo 导入

`.yaml` 是 **Rule Provider**，不能作为完整代理订阅导入。`behavior` 必须是 `classical`，`format` 必须是 `yaml`。

以下是配置片段，供使用者合并到现有配置；不是可替换整个配置的文件：

```yaml
rule-providers:
  worldfirst:
    type: http
    behavior: classical
    format: yaml
    url: https://raw.githubusercontent.com/daixianglong199-dotcom/worldfirst-rules/main/rules/WorldFirst.yaml
    path: ./ruleset/WorldFirst.yaml
    interval: 86400

rules:
  - RULE-SET,worldfirst,英国策略占位符
  # 其后保留现有 rules 的全部条目及原有顺序。
```

将 `英国策略占位符` 替换为现有英国节点或策略组的准确名称。若 `worldfirst` 这个 provider 名称已被占用，应换一个独立名称，并同步修改 `RULE-SET` 引用。

在 Clash Verge Rev 中，建议仅对需要接入的订阅使用以下操作，保留订阅原文：

1. 先备份配置，右键目标订阅，打开“编辑扩展配置”（不同版本菜单名称可能略有不同）。将上述 **`rule-providers` 部分**加入扩展配置，保留已有 provider。
2. 右键该订阅打开“编辑规则”，在 **prepend/前置规则**中加入 `RULE-SET,worldfirst,英国策略占位符`。不要再添加这三条域名的散列副本。
3. 保存、重新应用配置，在最终生效配置中确认 provider 存在、URL 正确、规则位于所有可能提前命中的直连规则集、GEOSITE、GEOIP、MATCH 之前。保留其余规则内容和相对顺序。
4. 手动更新 provider，检查连接日志，确认命中 `worldfirst` 且最终出口是已有英国节点。

**不要将上面的整个 `rules:` 示例直接粘进扩展配置。** Clash Verge Rev 的扩展配置会整体替换列表，可能覆盖现有规则；使用“编辑规则”的前置功能来插入该条规则。若其他扩展脚本稍后又改写 `rules` 或 `rule-providers`，需在最终配置中确认本条仍生效。

Mihomo 按规则从上到下匹配；仅把文件添加到 provider 中不会自动启用分流，还必须有相应的 `RULE-SET`。本项目不改动默认 DIRECT、GEOIP、MATCH，也不创建代理组。

## 验证与限制

见 [VALIDATION.md](VALIDATION.md) 中的语法、匹配边界、重复项、一致性及 Raw 检查结果。

这是域名规则，只有客户端能够获得目标域名时才有对应匹配依据；纯 IP 连接、未记录的新域名、共享资源域名不在保证范围内。第三方验证码、CDN、分析服务不因被官网引用而自动纳入。未进行用户设备上的 Loon / Clash Verge Rev 实际运行、账户登录或交易测试。

## 参考文档

- [Loon 订阅规则](https://nsloon.app/docs/Rule/sub_rule/) 与 [官方示例配置](https://github.com/Loon0x00/LoonExampleConfig/blob/master/example.conf)
- [Mihomo Rule Provider 配置](https://wiki.metacubex.one/config/rule-providers/)、[集合内容](https://wiki.metacubex.one/config/rule-providers/content/) 与 [路由规则](https://wiki.metacubex.one/config/rules/)
- [Clash Verge Rev 扩展配置](https://www.clashverge.dev/guide/extend.html) 与 [自定义规则](https://www.clashverge.dev/guide/rules.html)
