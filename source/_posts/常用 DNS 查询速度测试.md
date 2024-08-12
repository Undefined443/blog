### 测试工具

[DNS-Benchmark | GitHub](https://github.com/Undefined443/DNS-Benchmark)

### 总结

进过多轮测试，我认为：

- `223.5.5.5` 和 `223.6.6.6` 平均响应时间最短（`223.5.5.5` 在教育网内疑似无法连通）
- `114.114.114.114` 紧跟其后
- `119.29.29.29` 略逊一筹
- 在加密 DNS 系列中，阿里 DoH `https://dns.alidns.com/dns-query` 和阿里 DoT `tls://dns.alidns.com` 均表现不错，查询速度略低于其未加密 DNS
- 其余加密 DNS 平均响应时间都比较长
- `223.5.5.5` 和 `114.114.114.114` 会拒绝某些域名的查询请求（`REFUSED`），如 `api.segment.io`、`hm.baidu.com` 和 `www.google-analytics.com`