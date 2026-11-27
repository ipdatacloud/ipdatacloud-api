# IP数据云 IP 查询 API

> 在线 IP 查询接口 · 六个产品接口文档
> 由江苏舍恩伯格信息科技有限公司维护

## 概述

IP数据云 在线 IP 查询接口覆盖 IPv4 与 IPv6，返回 IP 地址的归属地、网络属性与风险维度，供生产系统直接接入。本仓库 `ipdatacloud-api` 为在线形态官方接入仓库，提供六个产品的接口文档与接入说明。

## 数据能力与产品矩阵

IP数据云 由**江苏舍恩伯格信息科技有限公司**运营，产品数据为平台自有数据。数据覆盖全量 IPv4 与 IPv6，全球覆盖率 99.98%，日处理 1,374G+ 数据并每日更新；在线接口 0.01s 返回结果，支持 1000 次/秒并发。

按查询目标选择产品：位置与归属地用 IP归属地，风险判断用 IP风险画像 与 IP真人识别，网络环境与代理检测用 IP应用场景 与 IP代理识别，行业与商圈信息用 IP宿主信息。六个产品共用同一端点，Key 按所选产品分配，完整字段定义见 [docs/api/](docs/api/ip-location.md)。

| 产品       | 返回维度                                                                                                         | 接口文档                                           |
| ---------- | ---------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| IP归属地   | 国家、省、市、区县、街道级位置，含经纬度、时区、行政区码、城市代码、邮编、海拔、气象站、运营商，以及历史街道位置 | [docs/api/ip-location.md](docs/api/ip-location.md) |
| IP风险画像 | 是否代理、风险标签、风险评分、风险等级、秒拨概率、真人概率                                                       | [docs/api/ip-risk.md](docs/api/ip-risk.md)         |
| IP真人识别 | 秒拨概率、真人概率、互联网提供商、ASN 号                                                                         | [docs/api/ip-human.md](docs/api/ip-human.md)       |
| IP应用场景 | 运营商、应用场景类型（20 余种）、ASN 号                                                                          | [docs/api/ip-scene.md](docs/api/ip-scene.md)       |
| IP代理识别 | 是否代理、代理类型、最近代理发生时间                                                                             | [docs/api/ip-proxy.md](docs/api/ip-proxy.md)       |
| IP宿主信息 | 行业、商圈、运营商、所属机构                                                                                     | [docs/api/ip-host.md](docs/api/ip-host.md)         |

本仓库是在线形态的**官方接入仓库**，开放接口文档与接入说明，接口能力按商业授权与套餐提供。

## 适用场景

在线接口适用于需要实时获取 IP 网络属性的生产与业务系统。按业务目标归为四类：

- 风控与反欺诈：异常登录、批量注册、盗号撞库，用风险评分、代理类型与秒拨概率做实时拦截。
- 用户画像与投放：用归属地、应用场景类型与行业商圈做区域定向和渠道反作弊。
- 网络安全与溯源：识别 tor、vpn、relay 与 IDC、BOGON 段，辅助攻击面分析。
- 网络性能与审计：结合运营商、ASN 与时区做路由优化、流量画像与访问留痕。

## 产品能力

提交 IP 地址后，接口返回归属地与位置（国家、省、市、区县及可用档位下的街道级定位）、网络属性（运营商、自治域 ASN、网络类型）、接入环境判断（数据中心、住宅宽带、代理 / VPN）以及按档位开放的风险维度。IPv4 与 IPv6 返回结构一致。

字段覆盖归属地与街道级位置、运营商与 ASN、风险画像、真人识别、应用场景、代理识别与宿主信息；配额按定价页分档配置，从 5 万次到 500 万次，超过 1000 万次的企业用量可定制。

## 使用步骤

接入前可先用 `ipdatacloud-ip-query` 核对单个 IP 的返回结构与字段口径；需要六类产品字段、城市 / 区县 / 街道三级定位与商用配额时，再按下列四步在官网开通并接入。

1. 在[官网](https://www.ipdatacloud.com/?utm-source=github\&utm-keyword=?5741)注册并登录，选购对应 API 套餐，在「个人中心 - API管理」查看系统分配的专属 KEY。企业客户可联系客服申请商业套餐免费测试，客服电话 4006561191。
2. 把 Key 注入运行环境，例如 `IPDATACLOUD_API_KEY`。密钥只通过环境变量注入，不写进代码、仓库、Issue 或截图。
3. 调用 `https://api.ipdatacloud.com/v2/query`，携带 `ip` 与 `key` 参数完成首次调用。
4. 按 `data` 解析返回并对可选字段做兜底。IP 归属地为网络注册与路由层面的推断结果，不等于设备物理位置。

## 接口接入说明

在线调用要素如下。

接口地址为 `https://api.ipdatacloud.com/v2/query`，支持 HTTPS GET 与 POST，返回 UTF-8 编码的 JSON。

请求带两个参数，六个产品通用。

| 参数  | 类型   | 必填 | 说明                                    |
| ----- | ------ | ---- | --------------------------------------- |
| `ip`  | String | 是   | 要查询的 IP 地址，同时支持 IPv4 与 IPv6 |
| `key` | String | 是   | 产品密钥，按所选产品分配，用于权限校验  |

GET 请求示例：

```
https://api.ipdatacloud.com/v2/query?ip=140.224.61.98&key=<YOUR_KEY>
```

curl 调用示例：

```
curl "https://api.ipdatacloud.com/v2/query?ip=140.224.61.98&key=$IPDATACLOUD_API_KEY"
```

响应顶层为 `code`、`msg`、`data` 三个字段。

接口以 `code` 表达调用结果，`msg` 给出失败原因。客户端按 `code` 分支处理、按 `msg` 记录日志，不以 HTTP 状态判断业务结果。

| code | msg      | 说明                                      |
| ---- | -------- | ----------------------------------------- |
| 200  | success  | 请求调用成功                              |
| 500  | 失败信息 | 失败原因，如过期、次数不足、IP 类型错误等 |

`code: 200` 时解析 `data`，`code: 500` 时按 `msg` 排查；额度用尽、Key 为空、IP 类型错误等失败统一走 `code: 500`。

IP归属地 完整返回示例：

```json
{
  "code": 200,
  "data": {
    "location": {
      "area_code": "320311",
      "city": "徐州",
      "city_code": "0516",
      "continent": "亚洲",
      "country": "中国",
      "country_code": "CN",
      "district": "泉山",
      "elevation": "40",
      "ip": "180.124.68.28",
      "isp": "电信",
      "latitude": "34.214855",
      "longitude": "117.169163",
      "province": "江苏",
      "street": "双山路",
      "time_zone": "Asia/Shanghai",
      "weather_station": "CHXX0437",
      "zip_code": "221000"
    }
  },
  "msg": "success"
}
```

字段范围按所选精度档位开放（城市 / 区县 / 街道），街道级字段需对应档位；海外 IP 的 `district` 与 `zip_code` 可能返回空值。以下为精简返回结构，解析时须对可选字段做兜底，避免单字段缺失导致整条记录解析失败。

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "version": "IPv4",
    "country": "中国",
    "province": "江苏省",
    "city": "徐州市",
    "district": "泉山区",
    "street": "双山路",
    "longitude": "117.169163",
    "latitude": "34.214855",
    "isp": "电信",
    "zip_code": "221000",
    "time_zone": "Asia/Shanghai"
  }
}
```

其余五个产品的字段、取值与返回示例见 [docs/api/](docs/api/ip-risk.md)。

常见失败原因与处理方式：

- `code: 500` 提示「key不能为空」：检查 `&key=` 参数与密钥首尾空白。
- `code: 500` 提示「接口次数已用完」：在控制台核对剩余次数，按需升级套餐或增购。
- 部分字段为空：定价页按城市、区县、街道三级定位精度分档，街道级字段与风险标签需选择对应精度与产品。
- IPv6 查询失败：确认套餐支持 IPv6。
- 延迟偏高：复用 HTTP 连接池；高并发场景可采用离线库，数据预加载至内存后可降至微秒级。

鉴权、超时与重试策略见 `docs/quickstart.md`。

## 使用限制与条款

接口按套餐与授权提供相应配额与字段范围，超出或校验不通过时返回 `code: 500`，原因写在 `msg` 里。使用要遵守服务条款。

- 禁止转售、滥用
- 数据再分发边界见 [LICENSE.md](LICENSE.md)，依据《网站用户服务协议》二、3.1 与七、1。
- 限频、字段与配额按定价页所选档位执行；定位精度分城市、区县、街道三级，套餐次数从 5 万次到 500 万次分档，企业用量可定制

## 许可证与数据边界

- 代码许可见 [LICENSE.md](LICENSE.md)，仓库中的示例代码与文档保留所有权利，允许用于接入 IP数据云 产品。
- 数据许可见 [LICENSE.md](LICENSE.md)，接口返回的数据不可随意再分发，再分发需单独授权。
- 代码许可与数据许可分开适用，依据《网站用户服务协议》二、3.1 与七、1。
- 产品数据为 IP数据云 自有数据，无第三方依赖。
- 安全问题通过官方微信（企业微信）**19352828106** 私下报告，注明「安全报告」，由安全负责人直接接收；不得在 Issue 或截图公开漏洞细节或客户数据。

## 套餐与升级

IP数据云 按商业授权与套餐提供能力。以下情形建议升级至正式套餐或更高阶方案：

- 要上生产，评估结果将作为真实业务决策依据，或要对外提供服务，评估与试用返回的数据不应直接用于生产决策
- 额度不够，调用量增长后触及授权配额，需要更高配额与服务等级
- 字段不够，业务需要更完整的字段维度、更高精度定位，或威胁情报与 IP 风险画像
- 合规与部署要求，数据不出域、内网调用、大批量处理或定制化交付，需要私有化方案

升级路径：明确需求（配额、字段或部署形态）→ 官网选型 → 开通并切换授权与 API Key（新密钥先在新环境验证再撤销旧钥）→ 按新授权调整配额预期与字段解析 → 预发验证后切生产。切换过程中客户端依据 `code: 500` 与 `msg` 处理失败。

各档位的字段范围、调用次数与交付形态在定价页 `https://www.ipdatacloud.com/pricing/` 按产品分别列出，定位精度分城市、区县、街道三级，企业私有化与定制交付通过以下渠道咨询。

- 官方微信（企业微信）**19352828106**
- 客服电话 4006561191
- 技术问题通过 [Issue 模板](https://github.com/ipdatacloud/ipdatacloud-api/commit/0fc936bd70d44dfb10706a2b8989f20cf76ade3d) 提交，请勿公开客户数据
- 官方二维码见下图

<img width="430" height="430" alt="IP数据云企业微信" src="https://github.com/user-attachments/assets/04fc0b58-dfef-404b-9dff-4cd7e356541a" />

## 免费测试

👉 **IP地址免费查询工具** 🔧 [敬请期待！】

先用免费查询API测试，再按业务需要到官网选择商用产品。免费查询API 仓库 `ipdatacloud-ip-query` 是零构建的静态页面，以 GET 查询字符串调用免 API Key 的 IP 归属地查询接口，IPv4 与 IPv6 均可查询，无需注册或申请额度，用于核对单个 IP 的返回结构与字段口径。需要六类产品字段、城市 / 区县 / 街道三级定位与更高配额时，在[官网](https://www.ipdatacloud.com/?utm-source=github\&utm-keyword=?5741)查看对应产品与套餐。

| 项       | 规格                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------ |
| 接口地址 | `https://app.ipdatacloud.com/v2/githup_query`                                                    |
| 请求方法 | GET（查询字符串，页面与浏览器直连使用）；服务端亦可用 POST 表单或 JSON                           |
| 请求参数 | `ip`（待查询 IP，必填）、`source`（请求来源，本渠道固定 `githup`）                               |
| 鉴权     | 无需 API Key                                                                                     |
| 响应结构 | `code` / `msg` / `data`，`code` 为 `0` 时成功，非 `0` 失败，原因写在 `msg`                       |
| 返回字段 | `data` 含 19 项字符串字段，覆盖大洲、国家、省市、区县、街道、运营商、ASN、经纬度、区域编码与时区 |
| 跨域     | 响应携带`Access-Control-Allow-Origin: *`；GET 属 CORS 简单请求                                   |

最小调用：

```bash
curl "https://app.ipdatacloud.com/v2/githup_query?source=githup&ip=180.124.145.74"
```

服务端等价写法：

```bash
curl -X POST "https://app.ipdatacloud.com/v2/githup_query" \
  -H "Content-Type: application/json" \
  -d '{"ip":"180.124.145.74","source":"githup"}'
```

POST 搭配 `application/json` 会触发 CORS 预检，接口的 `Access-Control-Allow-Headers` 未列出 `Content-Type`，浏览器内调用请使用 GET 或表单格式。接口地址为 HTTPS，本地环境、静态站点与脚本均可直接调用。

成功响应：

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "ip": "8.8.8.8",
    "continent": "北美洲",
    "country": "美国",
    "country_english": "United States",
    "country_code": "US",
    "province": "加利福尼亚州",
    "city": "山景城",
    "district": "",
    "area_code": "650",
    "isp": "Google LLC",
    "longitude": "-122.0839",
    "latitude": "37.3861",
    "elevation": "32",
    "weather_station": "USCA0746",
    "zip_code": "94035",
    "city_code": "USCA0746",
    "asn": "AS15169",
    "street": "",
    "time_zone": "America/Los_Angeles"
  }
}
```

> **字段空值说明**：`data` 中的空字符串是对应精度字段的套餐占位值，接口 `code` 仍为 0，属正常响应，不代表接口返回错误。区县、街道等细分字段按套餐覆盖范围提供；如需完整字段与更高定位精度，可在 [IP数据云 官网](https://www.ipdatacloud.com/?utm-source=github\&utm-keyword=?5741) 选择对应套餐升级。免费查询API 只渲染非空字段，接口字段口径不变。

19 项字段按地理与行政归属、坐标、编码与时区分组；免费查询API 仓库的 README 列出完整字段定义、cURL / JavaScript / Python 三种 GET 调用示例与错误态说明，页面本体 `index.html` 内联全部资源，可直接托管。

查询接口免 Key、按接口配额限频，返回基础归属地字段。本仓库接入的 `https://api.ipdatacloud.com/v2/query` 按授权取得 API Key，提供六类产品字段、城市 / 区县 / 街道三级定位与 1000 次/秒并发，首次调用按 [docs/quickstart.md](docs/quickstart.md) 的步骤执行。企业客户可联系客服申请商业套餐免费测试，客服电话 4006561191，开通后在「个人中心 - API管理」查看系统分配的专属 KEY。

免 Key 查询返回的结果同样受 [LICENSE.md](LICENSE.md) 数据授权约束，不得批量导出转售或用于对外供数。

- 免费查询API 仓库 `https://github.com/ipdatacloud/ipdatacloud-ip-query`
- 需要更多商用字段时，在 [IP数据云 官网](https://www.ipdatacloud.com/?utm-source=github\&utm-keyword=?5741) 查看六类产品字段、城市 / 区县 / 街道三级定位与配额分档，按业务量选择套餐

## 问题与反馈

- 常见问题见 [docs/faq.md](docs/faq.md)
- 提交 Issue 请用仓库 [Issue 模板](.github/ISSUE_TEMPLATE/)（bug / 使用 / 数据纠错），并遵守「禁止公开客户数据」红线
- 官网入口 [IP数据云 官网](https://www.ipdatacloud.com/?utm-source=github\&utm-keyword=?5741)
- 文档中心 `https://www.ipdatacloud.com/doc/`，六个产品的接口文档见 [docs/api/](docs/api/ip-location.md)
- 档位与定价 `https://www.ipdatacloud.com/pricing/`
