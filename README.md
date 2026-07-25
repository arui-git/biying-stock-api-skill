# 必盈API Skill — 沪深A股金融数据接口

必盈API（[biyingapi.com](https://biyingapi.com)）是国内专注沪深A股的轻量化商用金融数据接口，数据源对接正规券商。本仓库是该API的 Claude Code Skill，方便在 Claude Code 中快速调用所有接口。

## 覆盖市场

- 沪深A股
- 科创板
- 北京交易所
- 指数
- 基金

## 接口能力

| 分类 | 说明 |
|------|------|
| 股票列表 | 股票代码/名称、新股日历、概念指数、板块列表 |
| 实时交易 | 实时行情（券商/公开）、买卖五档、逐笔交易、多股批量查询 |
| 行情数据 | 最新分时、历史K线（1m/5m/15m/30m/60m/日/周/月/年）、涨跌停价格 |
| 技术指标 | MACD、MA、BOLL、KDJ（内置预计算） |
| 公司财务 | 资产负债表、利润表、现金流量表、主要指标、股本、股东数据 |
| 上市公司 | 公司简介、高管、分红、增发、解禁、业绩预告、基金持股 |
| 涨跌股池 | 涨停、跌停、强势、次新、炸板股池 |
| 指数/行业/概念 | 分类树、关联查询 |

## 快速开始

### 1. 获取免费 Licence

访问 [必盈API官网](https://www.biyingapi.com/licencelt) 免费申请，无需注册。

### 2. 调用示例

```bash
# 获取股票列表
curl "https://api.biyingapi.com/hslt/list/你的licence"

# 获取贵州茅台实时行情
curl "https://api.biyingapi.com/hsstock/real/time/600519/你的licence"

# 获取平安银行日K线历史数据
curl "https://all.biyingapi.com/hsstock/history/000001.SZ/d/n/你的licence?st=20250101&et=20250430&lt=100"
```

```python
import requests

LICENCE = "你的licence"

# 实时行情
resp = requests.get(f"https://api.biyingapi.com/hsstock/real/time/000001/{LICENCE}")
quote = resp.json()
print(f"最新价: {quote.get('p')}, 涨跌幅: {quote.get('pc')}%")
```

## 通用参数

| 参数 | 说明 | 可选值 |
|------|------|--------|
| `{period}` | K线级别 | `1` `5` `15` `30` `60` `d` `w` `m` `y` |
| `{adjust}` | 除权方式 | `n` 不复权, `q` 前复权, `h` 后复权 |
| `{market}` | 市场后缀 | `.SZ` 深证, `.SH` 上证, `.BJ` 北交所 |
| `st` / `et` | 起止时间 | `YYYYMMDD` 格式 |
| `lt` | 返回条数 | 整数 |

## 请求频率

| 版本 | 频率 |
|------|------|
| 体验版/黄金版/包月版 | 300次/分钟 |
| 包年版 | 3000次/分钟 |
| 白金版 | 6000次/分钟 |

## 参考

- 官方文档：https://biyingapi.com/doc_hs
- 证书申请：https://www.biyingapi.com/licencelt
