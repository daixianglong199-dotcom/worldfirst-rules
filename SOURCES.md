# WorldFirst 域名来源与验证记录

研究日期：2026-10-20（Europe/London）。验证仅针对公开来源、域名匹配边界，不代表已验证英国账户的全部网络依赖或域名的法律所有权。

## 正式规则的证据

| 正式规则 | 公开来源 | 验证状态 |
| --- | --- | --- |
| DOMAIN-SUFFIX,worldfirst.com | 用户初始规则；下列 Beginner-tl 源文件中的 `+.worldfirst.com`；[WorldFirst 英国官网](https://www.worldfirst.com/uk/) 的登录/注册链接指向 `portal.worldfirst.com` | 公开官网和上游交叉确认；域名族按用户要求纳入；英国客户端依赖未实测。 |
| DOMAIN,portal.worldfirst.com.cn | 用户初始规则；Beginner-tl 源文件；[WorldFirst 中国官网英文页](https://www.worldfirst.com.cn/static/en-cn/index.html) 的注册入口指向该主机；[公开门户](https://portal.worldfirst.com.cn/) 跳转到同主机 `/login` | 官网链接和公开门户交叉确认；仅精确匹配，不推定所有 `.com.cn` 子域均有必要。 |
| DOMAIN,www.worldfirst.com.cn | 用户初始规则；Beginner-tl 源文件；上述中国官网页面在该主机下提供 WorldFirst 内容并链接全球/英国官网 | 官网和上游交叉确认；仅精确匹配。 |

## 指定 GitHub 项目研究

固定到本次读取的提交，便于复核；只摘取域名事实，不导入上游的路由策略或整套规则。

1. **Beginner-tl/Mihomo-rules**：[源文件](https://github.com/Beginner-tl/Mihomo-rules/blob/2ce703d37eaf248c44dfebb98b6afbdf346650e2/scripts/custom/crossborder-ecommerce.list)，提交 `2ce703d37eaf248c44dfebb98b6afbdf346650e2`。WorldFirst 小节列出 `portal.worldfirst.com`、`portal.worldfirst.com.cn`、`www.worldfirst.com.cn`、`+.worldfirst.com`。前一条已包含在后缀规则中，去重后即本项目三条。该项目原来的跨境电商直连用途不沿用。
2. **fortressme/ownmagicrule**：[人工规则](https://github.com/fortressme/ownmagicrule/blob/e24e190fda5b938449caf5b59d32f16a874b73a0/rules/MyDirect.MANUAL.list)，提交 `e24e190fda5b938449caf5b59d32f16a874b73a0`。发现 `DOMAIN-SUFFIX,worldfirst.com` 和 `DOMAIN-SUFFIX,worldfirst.com.cn`。其整个 `.com.cn` 后缀覆盖不证明英国账户需要如此扩大，本项目不采用该扩大范围，也不沿用直连策略。
3. **Xiaobei09/Rule-Provider**：[Loon/HK.list](https://github.com/Xiaobei09/Rule-Provider/blob/5a39c5994669932f5a518282f135d8627e657711/ruleset/site/Loon/HK.list) 在提交 `5a39c5994669932f5a518282f135d8627e657711` 中列出 `DOMAIN-SUFFIX,worldfirst.com.cn`。同一提交的 [Loon/SG.list](https://github.com/Xiaobei09/Rule-Provider/blob/5a39c5994669932f5a518282f135d8627e657711/ruleset/site/Loon/SG.list) 等搜索命中 `worldfirst.com`。这些是大型地区分类集；文件中的地区标签不作为英国出口或业务依赖的证据。

搜索和定点读取发现的 WorldFirst 域名没有提供足够依据新增独立注册域。未宣称对三个仓库所有历史版本或账户登录后请求做了穷尽审计。

## 额外发现：已覆盖

英国官网公开引用的 `portal.worldfirst.com`、`www.global-voyage.worldfirst.com`，以及 [公开文档](https://docs.worldfirst.com/wfdocs/products/global_spend_onboarding_guide) 所在的 `docs.worldfirst.com` 都已由 `.com` 后缀规则覆盖，不增列重复规则。

## 候选域名：不进入正式规则

| 域名或范围 | 已知证据 | 尚需验证 |
| --- | --- | --- |
| worldfirst.com.cn 根域名及整个后缀 | 两个上游使用宽泛后缀规则 | 没有证明英国账户需要覆盖全部主机；保持不匹配。 |
| developers.worldfirst.com.cn | 英国官网的 API References 链接指向此主机；[文档中心](https://developers.worldfirst.com.cn/docs/alipay-worldfirst/overview_zh/home) 有 WorldFirst 内容 | 官方关联明确，但未确认用户英国账户的实际访问/业务需要；暂不添加。 |
| ics-global.worldfirst.com.cn | [公开加载页](https://ics-global.worldfirst.com.cn/mada/home?scene=c7a80a519db99eb1) 可检索到 | 具体用途、英国账户依赖待验证；暂不添加。 |
| page.worldfirst.com.cn | [公开 WorldFirst 操作说明 PDF](https://page.worldfirst.com.cn/storage/2021/9/2/cbaba049-3a50-48a8-9c15-c6c66f1fada9.pdf) | 历史资料托管用途不等于当前英国账户依赖；暂不添加。 |
| mdn.marmot-cloud.com、cdn.marmot-cloud.com、cdn-worldfirst.marmot-cloud.com | 英国/中国官网或公开资料引用其图片、PDF | 引用不能证明主机由 WorldFirst 独占；不得扩为共享 `marmot-cloud.com` 后缀，本次全部排除。 |

未来只在公开官方资料与实际请求记录足以确认用途时评估**精确域名**；不因品牌关键词、CNAME、共享 CDN 或上游地区分类而扩大范围。不收集或发布账户 Cookie、令牌、交易数据。
