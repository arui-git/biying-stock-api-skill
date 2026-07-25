---
name: biying-stock-api
description: 必盈API — 沪深A股金融数据接口，覆盖实时行情、历史K线、技术指标、财务数据、基金、指数、北交所、科创板。
user-invocable: true
---

# 必盈API — 金融数据接口参考

必盈API（biyingapi.com）是国内专注沪深A股的轻量化商用金融数据接口，数据源对接正规券商，覆盖沪深A股、科创板、北交所、指数、基金。免费申请licence即可使用。

---

## 快速开始

### 认证方式

所有接口通过在URL路径中附加 **licence密钥** 进行认证。免费证书在 [必盈API官网](https://www.biyingapi.com/licencelt) 申请。

### 基础URL

| 类型 | 域名 |
|------|------|
| 标准接口 | `https://api.biyingapi.com` |
| 全市场/批量数据 | `https://all.biyingapi.com` |
| 企业版VIP | `https://专属子域名.biyingapi.com` |

### 通用参数说明

| 参数 | 说明 | 可选值 |
|------|------|--------|
| `{licence}` | 你的licence密钥 | 免费证书: 200次/分钟 |
| `{stock_code}` | 股票代码（6位数字） | 如 `000001`, `600519` |
| `{market}` | 交易所后缀 | `.SZ` 深证, `.SH` 上证, `.BJ` 北交所 |
| `{period}` | K线分时级别 | `1`, `5`, `15`, `30`, `60`, `d`(日), `w`(周), `m`(月), `y`(年) |
| `{adjust}` | 除权方式 | `n` 不复权, `q` 前复权, `h` 后复权 |
| `st` | 开始时间 | `YYYYMMDD` 格式，如 `20240601` |
| `et` | 结束时间 | `YYYYMMDD` 格式，如 `20250430` |
| `lt` | 最新条数 | 整数，如 `100` |

### 请求方式

所有接口均为 **GET** 请求，返回 **JSON**（也可返回CSV）。

### 请求频率限制

| 版本 | 频率 |
|------|------|
| 体验版/黄金版/包月版 | 1分钟300次 |
| 包年版 | 1分钟3000次 |
| 白金版 | 1分钟6000次 |

### 错误码

| 错误码 | 含义 |
|--------|------|
| `101` | 当日请求超限，需更换licence |
| `102` | licence无效 |

---

## 一、沪深A股接口

### 1.1 股票列表

#### 股票列表
```
GET https://api.biyingapi.com/hslt/list/{licence}
```
**返回字段**：`dm`(股票代码), `mc`(股票名称), `jys`(交易所: sh=上证, sz=深证)

#### 新股日历
```
GET https://api.biyingapi.com/hslt/new/{licence}
```
**返回字段**：`zqdm`(股票代码), `zqjc`(股票简称), `sgdm`(申购代码), `fxsl`(发行总数), `swfxsl`(网上发行), `sgsx`(申购上限), `dgsz`(顶格申购需配市值), `sgrq`(申购日期), `fxjg`(发行价格), `zxj`(最新价), `srspj`(首日收盘价), `zqgbrq`(中签号公布日), `zqjkrq`(中签缴款日), `ssrq`(上市日期), `syl`(发行市盈率), `hysyl`(行业市盈率), `wszql`(中签率%), `yzbsl`(连续一字板数量), `zf`(涨幅%), `yqhl`(每中一签获利), `zyyw`(主营业务)

#### 概念指数列表（券商数据）
```
GET https://api.biyingapi.com/hslt/sectorslist/{licence}
```
**返回字段**：`dm`(概念指数代码，如 101076.BKZS), `mc`(名称), `jys`(交易所)

#### 一级市场板块列表（券商数据）
```
GET https://api.biyingapi.com/hslt/primarylist/{licence}
```

#### 板块明细列表（券商数据）
```
GET https://api.biyingapi.com/hslt/sectors/{板块指数名称}/{licence}
```
示例：`/hslt/sectors/TFG板块趋势/{licence}`

### 1.2 指数、行业、概念

#### 指数/行业/概念树
```
GET https://api.biyingapi.com/hszg/list/{licence}
```

#### 根据指数/行业/概念找相关股票
```
GET https://api.biyingapi.com/hszg/gg/{指数行业概念代码}/{licence}
```
示例：`/hszg/gg/sw_sysh/{licence}`

#### 根据股票找相关指数/行业/概念
```
GET https://api.biyingapi.com/hszg/zg/{股票代码}/{licence}
```
示例：`/hszg/zg/000001/{licence}`

### 1.3 涨跌股池

| 接口 | URL |
|------|-----|
| 涨停股池 | `GET /hslt/ztgc/{日期}/{licence}` |
| 跌停股池 | `GET /hslt/dtgc/{日期}/{licence}` |
| 强势股池 | `GET /hslt/qsgc/{日期}/{licence}` |
| 次新股池 | `GET /hslt/cxgc/{日期}/{licence}` |
| 炸板股池 | `GET /hslt/zbgc/{日期}/{licence}` |

日期格式：`2020-01-15`

### 1.4 上市公司详情

| 接口 | URL |
|------|-----|
| 公司简介 | `GET /hscp/gsjj/{股票代码}/{licence}` |
| 所属指数 | `GET /hscp/sszs/{股票代码}/{licence}` |
| 历届高管成员 | `GET /hscp/ljgg/{股票代码}/{licence}` |
| 历届董事会成员 | `GET /hscp/ljds/{股票代码}/{licence}` |
| 历届监事会成员 | `GET /hscp/ljjj/{股票代码}/{licence}` |
| 近年分红 | `GET /hscp/jnfh/{股票代码}/{licence}` |
| 近年增发 | `GET /hscp/jnzf/{股票代码}/{licence}` |
| 解禁限售 | `GET /hscp/jjxs/{股票代码}/{licence}` |
| 季度利润 | `GET /hscp/jdlr/{股票代码}/{licence}` |
| 季度现金流 | `GET /hscp/jdxj/{股票代码}/{licence}` |
| 近年业绩预告 | `GET /hscp/yjyg/{股票代码}/{licence}` |
| 财务指标 | `GET /hscp/cwzb/{股票代码}/{licence}` |
| 十大股东 | `GET /hscp/sdgd/{股票代码}/{licence}` |
| 十大流通股东 | `GET /hscp/ltgd/{股票代码}/{licence}` |
| 股东变化趋势 | `GET /hscp/gdbh/{股票代码}/{licence}` |
| 基金持股 | `GET /hscp/jjcg/{股票代码}/{licence}` |

### 1.5 实时交易

#### 实时交易数据（公开数据源）
```
GET https://api.biyingapi.com/hsrl/ssjy/{股票代码}/{licence}
```

#### 实时交易数据（券商数据源）★核心接口
```
GET https://api.biyingapi.com/hsstock/real/time/{股票代码}/{licence}
```

**返回字段说明**：

| 字段 | 类型 | 说明 |
|------|------|------|
| `p` | number | 最新价 |
| `o` | number | 开盘价 |
| `h` | number | 最高价 |
| `l` | number | 最低价 |
| `yc` | number | 前收盘价 |
| `cje` | number | 成交总额 |
| `v` | number | 成交总量 |
| `pv` | number | 原始成交总量 |
| `t` | string | 更新时间 |
| `ud` | float | 涨跌额 |
| `pc` | float | 涨跌幅 |
| `zf` | float | 振幅 |
| `mc` | string | 股票名称 |
| `dm` | string | 股票代码 |

#### 买卖五档盘口
```
GET https://api.biyingapi.com/hsstock/real/five/{股票代码}/{licence}
```
返回买一~买五、卖一~卖五的价格和数量。

#### 当天逐笔交易
```
GET https://api.biyingapi.com/hsrl/zbjy/{股票代码}/{licence}
```

#### 实时交易数据（多股，最多20只）
```
GET https://api.biyingapi.com/hsrl/ssjy_more/{licence}?stock_codes=000001,000002,000004
```

#### 全市场实时（券商，需付费）
```
GET https://all.biyingapi.com/hsrl/ssjy/all/{licence}
```

#### 全市场实时（公开，需付费）
```
GET https://all.biyingapi.com/hsrl/real/all/{licence}
```

#### 资金流向数据
```
GET https://api.biyingapi.com/hsstock/history/transaction/{股票代码}/{licence}?st={开始时间}&et={结束时间}&lt={最新条数}
```

### 1.6 行情数据

#### 最新分时交易
```
GET https://api.biyingapi.com/hsstock/latest/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?lt={最新条数}
```
示例：`/hsstock/latest/000001.SZ/d/n/{licence}?lt=3`

#### 历史分时交易 ★核心接口
```
GET https://all.biyingapi.com/hsstock/history/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st={开始时间}&et={结束时间}&lt={最新条数}
```
示例：`/hsstock/history/000001.SZ/d/n/{licence}?st=20250101&et=20250430&lt=100`

#### 历史涨跌停价格
```
GET https://api.biyingapi.com/hsstock/stopprice/history/{股票代码}.{市场}/{licence}?st={开始时间}&et={结束时间}
```
示例：`/hsstock/stopprice/history/000001.SZ/{licence}?st=20240501&et=20240601`

#### 行情指标
```
GET https://api.biyingapi.com/hsstock/indicators/{股票代码}.{市场}/{licence}?st={开始时间}&et={结束时间}
```
返回内置技术指标数据。

#### 企业版历史数据【1m级别】
```
GET https://{专属子域名}.biyingapi.com/hsstock/vip/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st={开始时间}&et={结束时间}&lt={最新条数}
```
仅企业版用户可用，支持1分钟级别数据。

### 1.7 基础信息

#### 股票基础信息
```
GET https://api.biyingapi.com/hsstock/instrument/{股票代码}.{市场}/{licence}
```
示例：`/hsstock/instrument/000001.SZ/{licence}`
返回股票的合约基础信息（总股本、流通股本、上市日期等）。

### 1.8 公司财务

| 接口 | URL |
|------|-----|
| 资产负债表 | `GET /hsstock/financial/balance/{股票代码}.{市场}/{licence}?st=&et=` |
| 利润表 | `GET /hsstock/financial/income/{股票代码}.{市场}/{licence}?st=&et=` |
| 现金流量表 | `GET /hsstock/financial/cashflow/{股票代码}.{市场}/{licence}?st=&et=` |
| 财务主要指标 | `GET /hsstock/financial/pershareindex/{股票代码}.{市场}/{licence}?st=&et=` |
| 公司股本表 | `GET /hsstock/financial/capital/{股票代码}.{市场}/{licence}?st=&et=` |
| 十大股东 | `GET /hsstock/financial/topholder/{股票代码}.{市场}/{licence}?st=&et=` |
| 十大流通股东 | `GET /hsstock/financial/flowholder/{股票代码}.{市场}/{licence}?st=&et=` |
| 公司股东数 | `GET /hsstock/financial/hm/{股票代码}.{市场}/{licence}?st=&et=` |

示例：`/hsstock/financial/balance/600519.SH/{licence}?st=20230330&et=20230630`

### 1.9 技术指标（内置预计算）

| 指标 | URL |
|------|-----|
| MACD | `GET /hsstock/history/macd/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st=&et=&lt=` |
| MA(均线) | `GET /hsstock/history/ma/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st=&et=&lt=` |
| BOLL(布林) | `GET /hsstock/history/boll/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st=&et=&lt=` |
| KDJ | `GET /hsstock/history/kdj/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st=&et=&lt=` |

---

## 二、指数数据接口

### 2.1 指数列表
```
GET https://api.biyingapi.com/hsindex/list/{licence}
```

### 2.2 实时交易
```
GET https://api.biyingapi.com/hsindex/real/time/{指数代码}/{licence}
```
示例：`/hsindex/real/time/000001/{licence}`（上证指数代码为 000001.SH）

### 2.3 行情数据

#### 最新分时交易
```
GET https://api.biyingapi.com/hsindex/latest/{指数代码}.{市场}/{分时级别}/{licence}?lt={最新条数}
```

#### 历史分时交易
```
GET https://api.biyingapi.com/hsindex/history/{指数代码}.{市场}/{分时级别}/{licence}?st={开始时间}&et={结束时间}
```

### 2.4 技术指标

| 指标 | URL |
|------|-----|
| MACD | `GET /hsindex/history/macd/{指数代码}.{市场}/{分时级别}/{licence}?st=&et=&lt=` |
| MA | `GET /hsindex/history/ma/{指数代码}.{市场}/{分时级别}/{licence}?st=&et=&lt=` |
| BOLL | `GET /hsindex/history/boll/{指数代码}.{市场}/{分时级别}/{licence}?st=&et=&lt=` |
| KDJ | `GET /hsindex/history/kdj/{指数代码}.{市场}/{分时级别}/{licence}?st=&et=&lt=` |

---

## 三、基金数据接口

| 接口 | URL |
|------|-----|
| 沪深基金列表 | `GET /fd/list/all/{licence}` |
| ETF基金列表 | `GET /fd/list/etf/{licence}` |
| 基金实时数据 | `GET /fd/real/time/{基金代码}/{licence}` |

---

## 四、北京交易所接口

### 4.1 股票列表
```
GET https://api.biyingapi.com/bj/list/all/{licence}
GET https://api.biyingapi.com/bj/list/index/{licence}
```

### 4.2 实时交易
```
GET https://api.biyingapi.com/bj/stock/real/time/{股票代码}/{licence}
GET https://api.biyingapi.com/bj/stock/real/five/{股票代码}/{licence}
GET https://api.biyingapi.com/bj/index/real/time/{指数代码}/{licence}
```

### 4.3 行情数据
```
GET https://api.biyingapi.com/bj/history/{股票代码}.{市场}/{分时级别}/{除权方式}/{licence}?st=&et=&lt=
```

### 4.4 公司财务
与沪深A股财务接口结构一致，路径前缀为 `/bj/financial/`：

| 接口 | URL |
|------|-----|
| 资产负债表 | `GET /bj/financial/balance/{股票代码}.BJ/{licence}?st=&et=` |
| 利润表 | `GET /bj/financial/income/{股票代码}.BJ/{licence}?st=&et=` |
| 现金流量表 | `GET /bj/financial/cashflow/{股票代码}.BJ/{licence}?st=&et=` |
| 财务主要指标 | `GET /bj/financial/pershareindex/{股票代码}.BJ/{licence}?st=&et=` |
| 公司股本表 | `GET /bj/financial/capital/{股票代码}.BJ/{licence}?st=&et=` |
| 十大股东 | `GET /bj/financial/topholder/{股票代码}.BJ/{licence}?st=&et=` |
| 十大流通股东 | `GET /bj/financial/flowholder/{股票代码}.BJ/{licence}?st=&et=` |
| 公司股东数 | `GET /bj/financial/hm/{股票代码}.BJ/{licence}?st=&et=` |

---

## 五、科创板接口

| 接口 | URL |
|------|-----|
| 科创股票列表 | `GET /kc/list/all/{licence}` |
| 股票实时数据 | `GET /kc/real/time/{股票代码}/{licence}` |
| 买卖五档盘口 | `GET /kc/real/five/{股票代码}/{licence}` |

---

## 使用方式

### 调用示例（curl）

```bash
# 获取股票列表
curl "https://api.biyingapi.com/hslt/list/你的licence"

# 获取贵州茅台实时行情（券商数据）
curl "https://api.biyingapi.com/hsstock/real/time/600519/你的licence"

# 获取平安银行日K线历史数据
curl "https://all.biyingapi.com/hsstock/history/000001.SZ/d/n/你的licence?st=20250101&et=20250430&lt=100"

# 获取MACD技术指标
curl "https://api.biyingapi.com/hsstock/history/macd/000001.SZ/d/n/你的licence?st=20250101&et=20250430&lt=100"

# 多股实时数据（最多20只）
curl "https://api.biyingapi.com/hsrl/ssjy_more/你的licence?stock_codes=000001,600519,000858"

# 获取资产负债表
curl "https://api.biyingapi.com/hsstock/financial/balance/600519.SH/你的licence?st=20230330&et=20230630"

# 获取涨停股池
curl "https://api.biyingapi.com/hslt/ztgc/2025-01-15/你的licence"

# 获取股票基础信息
curl "https://api.biyingapi.com/hsstock/instrument/000001.SZ/你的licence"
```

### Python 调用示例

```python
import requests

LICENCE = "你的licence"

# 获取股票列表
resp = requests.get(f"https://api.biyingapi.com/hslt/list/{LICENCE}")
stocks = resp.json()
print(stocks)

# 获取实时行情
resp = requests.get(f"https://api.biyingapi.com/hsstock/real/time/000001/{LICENCE}")
quote = resp.json()
print(f"最新价: {quote.get('p')}, 涨跌幅: {quote.get('pc')}%")

# 获取历史K线
resp = requests.get(
    f"https://all.biyingapi.com/hsstock/history/000001.SZ/d/n/{LICENCE}",
    params={"st": "20250101", "et": "20250430", "lt": 100}
)
kline = resp.json()
print(kline)
```

### 重要提醒

1. **测试证书**：`biyinglicence` 是官方测试证书，只能返回 `000001` 的数据。正式使用时需替换为自己的licence。
2. **免费证书申请**：访问 https://www.biyingapi.com/licencelt 直接获取，无需注册。
3. **证书查询/升级**：https://www.biyingapi.com/licence-query
4. **官方文档**：https://biyingapi.com/doc_hs
