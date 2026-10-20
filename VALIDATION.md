# WorldFirst 验证记录

检查日期：2026-10-20（Europe/London）。

## 本地检查

结果：**通过**。使用 Node.js 运行独立校验脚本，使用官方 nodeca/js-yaml 4.1.0 的 JSON_SCHEMA 解析 YAML。解析器及临时检查脚本位于工作区独立检查目录，不作为订阅内容发布。

| 检查项 | 结果 |
| --- | --- |
| Loon 规则静态语法 | 三条均为支持的 DOMAIN / DOMAIN-SUFFIX 两字段规则，无策略字段；通过。非 Loon 客户端解析实测。 |
| Mihomo YAML | 可由实际 YAML 解析器读取；顶层只有 payload，内容为三个规则字符串；通过。 |
| 重复和冗余规则 | 两文件均为 0；没有重复列出已覆盖的 portal.worldfirst.com。 |
| 两文件范围及顺序 | 完全一致，均为三条；通过。 |
| 正例 | 每种格式 8 例，全部通过：主域、www、portal、docs、多层子域、www.global-voyage 及两个精确 .com.cn 主机。 |
| 反例 | 每种格式 24 例，全部通过：未列入的 .com.cn 主机、精确主机的下级子域、伪装前后缀、Alibaba / AliExpress / Taobao、PayPal、Kraken、共享 CDN 及无关域名。 |
| README 配置片段 | 实际 YAML 解析通过；provider 为 classical/yaml，RULE-SET 和 provider 同名；Loon 的 policy/tag/enabled 参数齐全。 |
| 文件编码 | UTF-8、无 BOM、末尾换行；通过。 |

共 32 个域名案例 × 2 种格式 = **64 次匹配断言**。匹配检查依照官方 DOMAIN / DOMAIN-SUFFIX 语义模拟，不能替代客户端集成测试。

规则文件 SHA-256：

```text
698d761d92a8df81b85215fdccf9465db8aec75c613c500c5ad3bed72bf7a2b5  rules/WorldFirst.list
b9df9ad7cc579496c505e96d638a2dd389ef7488a0889643e634671d4e5addd0  rules/WorldFirst.yaml
```

## 发布与客户端

- [GitHub 仓库](https://github.com/daixianglong199-dotcom/worldfirst-rules) 已创建。GitHub API 确认 `private: false`、默认分支为 `main`。
- 2026-10-20 11:17 BST（10:17 UTC）使用未登录的 HTTP GET 检查两个实际订阅地址：均为 **HTTP 200**，`Content-Type: text/plain; charset=utf-8`，与本地规则**逐字节一致**，SHA-256 与上表一致。

| 文件 | Raw 地址 | 大小 |
| --- | --- | --- |
| Loon | [WorldFirst.list](https://raw.githubusercontent.com/daixianglong199-dotcom/worldfirst-rules/main/rules/WorldFirst.list) | 142 字节 |
| Mihomo | [WorldFirst.yaml](https://raw.githubusercontent.com/daixianglong199-dotcom/worldfirst-rules/main/rules/WorldFirst.yaml) | 111 字节 |

- Loon / Clash Verge Rev 客户端实际运行：未实测。
- 英国节点连通性、账户登录、地区审核、交易与风控：未测试，不作保证。

不会为验证而更改现有客户端、Kraken 仓库、Oracle Cloud、3X-UI 或其他服务器。
