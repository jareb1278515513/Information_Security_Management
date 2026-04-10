# DVWA SQL 注入简易扫描工具说明
刘佳仑——2023211559
## 1. 工具定位
这是一个简单的 SQL 注入扫描脚本，目标是：
- 快速检测 DVWA 页面参数是否存在 SQL 注入可疑迹象
- 输出可疑参数、payload 和触发原因

## 2. 文件说明
- `dvwa_sqli_scanner.py`：扫描脚本（Python 3）

## 3. 扫描逻辑
脚本会先发一个“基线请求”，再对每个 GET 参数拼接 payload 做对比，核心规则有 3 条：

1) SQL 报错特征匹配
- 检查响应中是否出现典型数据库报错文本
- 比如 `You have an error in your SQL syntax`、`PDOException` 等

2) 响应长度异常变化
- 如果测试响应和基线响应长度差异很大，就标记可疑
- 这是一个粗粒度规则，适合快速筛查

3) 延时注入迹象
- 对 `SLEEP` 类 payload 做简单耗时判断
- 如果明显慢于基线，则提示疑似时间盲注

## 4. 使用前准备（DVWA）
1. 启动 DVWA
2. 登录 DVWA
3. 将 Security Level 设置为 `Low`
4. 复制浏览器中的 Cookie（至少包含 `PHPSESSID` 和 `security`）

Cookie 示例：
PHPSESSID=abc123; security=low

## 5. 运行示例
在当前目录执行：

python3 dvwa_sqli_scanner.py \
  --url "http://127.0.0.1:8080/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie "PHPSESSID=abc123; security=low"

可选参数：
- `--timeout`：请求超时，默认 8 秒

## 6. 输出结果说明

- 目标页面：`/vulnerabilities/sqli/`
- 被测参数：`id`
- 命中 payload：例如 `' OR '1'='1`
- 工具输出原因：例如“命中 SQL 报错特征”或“响应长度变化较大”
- 人工复核：在浏览器手工构造请求，观察页面行为是否一致

## 7. 局限性
- 只做 GET 参数扫描
- 未处理 POST、JSON、复杂认证流程
- 规则是启发式的，存在误报/漏报
