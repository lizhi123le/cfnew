
## 2026-07-26: Fix custom_proxy_group bracket syntax for non-select types

### Problem
custom_proxy_group with []GroupName bracket syntax in url-test/load-balance/fallback groups treated bracket references as regex patterns, producing ALL nodes instead of only referenced group members.

### Fix
1. 解析IniTemplate文本 (line 2938): Added hasBrackets: hasBracketsConsumed to parsed proxy group object.
2. 生成代理组配置 (lines 3020-3031): Added pg.hasBrackets check - true=push proxies as-is (literal group refs), false=keep regex filtering.

### Key Insight
- select type already handled bracket syntax via per-proxy 是正则表达式(p) check
- Non-select types batch-processed through 编译正则() without distinguishing syntax
- hasBrackets flag set at parse time is the cleanest discriminator
- Self-reference filter and .* expansion still apply correctly after
