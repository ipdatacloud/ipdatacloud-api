# IP 归属地接口

按 IP 地址查询地理位置与网络属性，覆盖 IPv4 与 IPv6。返回国家、省、市、区县、街道，以及经纬度、行政区码、运营商、时区、邮编、海拔与气象站编码。

## 一、接口说明

IP归属地数据查询接口，输入单个 IP 地址，返回该地址的地理归属与网络属性。

## 二、接口方式

| 项目 | 规格 |
| --- | --- |
| 接口地址 | `https://api.ipdatacloud.com/v2/query` |
| 请求方式 | HTTPS GET / POST |
| 输出格式 | JSON |
| 字符集 | UTF-8 |

## 三、请求参数

| 字段名称 | 字段类型 | 是否必填 | 字段描述 | 备注 |
| --- | --- | --- | --- | --- |
| `ip` | String | 是 | IP 地址 | 同时支持 IPv4 和 IPv6 |
| `key` | String | 是 | 产品密钥 | 根据所选产品分配，权限校验用 |

请求示例：

```
https://api.ipdatacloud.com/v2/query?ip=需要查询的ip&key=您申请的key
```

## 四、输出说明

| 字段名称 | 字段类型 | 字段描述 | 备注 |
| --- | --- | --- | --- |
| `code` | String | 状态码 | 参考状态码一节 |
| `msg` | String | 状态信息 | 请求成功时内容为 `success` |
| `data` | JsonObject | 输出信息 | 请求失败时内容为空，字段见下 |

### 4.1 IPv4 归属地返回字段

| 字段名称 | 字段类型 | 字段描述 |
| --- | --- | --- |
| `continent` | String | 所属七大洲 |
| `country` | String | 国家 |
| `country_code` | String | 国家英文简写 |
| `province` | String | 省份 |
| `city` | String | 城市 |
| `district` | String | 区县 |
| `street` | String | 街道 |
| `multi_street` | JsonObject | 历史街道位置详情，见 4.1.1 |
| `longitude` | String | 经度 |
| `latitude` | String | 纬度 |
| `area_code` | String | 行政区码 |
| `isp` | String | 运营商 |
| `time_zone` | String | 时区 |
| `elevation` | String | 海拔 |
| `weather_station` | String | 气象站 |
| `zip_code` | String | 邮编 |
| `city_code` | String | 城市代码 |

### 4.1.1 multi_street 返回字段

| 字段名称 | 字段描述 | 备注 |
| --- | --- | --- |
| `lng` | 经度 | |
| `lat` | 纬度 | |
| `province` | 省份 | |
| `city` | 城市 | |
| `district` | 区县 | |
| `street` | 街道 | |
| `zip_code` | 邮编 | |
| `radius` | 范围半径 | |

### 4.2 IPv6 归属地返回字段

| 字段名称 | 字段类型 | 字段描述 |
| --- | --- | --- |
| `continent` | String | 所属七大洲 |
| `country` | String | 国家 |
| `country_code` | String | 国家英文简写 |
| `province` | String | 省份 |
| `city` | String | 城市 |
| `district` | String | 区县 |
| `longitude` | String | 经度 |
| `latitude` | String | 纬度 |
| `area_code` | String | 行政区码 |
| `isp` | String | 运营商 |
| `time_zone` | String | 时区 |
| `elevation` | String | 海拔 |
| `weather_station` | String | 气象站 |
| `zip_code` | String | 邮编 |
| `city_code` | String | 城市代码 |

IPv6 不返回 `street` 与 `multi_street`，其余字段与 IPv4 一致。

## 五、状态码

| code | msg | 备注 |
| --- | --- | --- |
| 200 | success | 请求调用成功 |
| 500 | 失败信息 | 请求失败原因（过期、次数不足、IP 类型错误等） |

## 六、返回值示例

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
      "multi_street": [
        {
          "lng": "117.169163",
          "lat": "34.214855",
          "province": "江苏",
          "city": "徐州",
          "district": "泉山",
          "street": "双山路",
          "radius": "2.27",
          "zip_code": "221000"
        },
        {
          "lng": "117.191078",
          "lat": "34.224231",
          "province": "江苏",
          "city": "徐州",
          "district": "泉山",
          "street": "解放南路387号",
          "radius": "1.15",
          "zip_code": "221000"
        },
        {
          "lng": "117.180535",
          "lat": "34.218589",
          "province": "江苏",
          "city": "徐州",
          "district": "泉山",
          "street": "文华路",
          "radius": "2.73",
          "zip_code": "221000"
        }
      ],
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

## 七、接入说明

- 定位精度分城市、区县、街道三级，按套餐档位开放；街道级字段需对应档位。
- IPv6 查询需套餐支持，未开通时返回 `code: 500`。
- 解析时对可选字段做兜底，单个字段缺失不应导致整条记录解析失败。
- IP 归属地为网络注册与路由层面的推断结果，不等于设备物理位置。

## 八、相关文档

- 开通与首次调用见 [quickstart.md](../quickstart.md)
- 其余五个产品的接口文档见 [ip-risk.md](ip-risk.md)、[ip-scene.md](ip-scene.md)、[ip-human.md](ip-human.md)、[ip-proxy.md](ip-proxy.md)、[ip-host.md](ip-host.md)
- 常见问题见 [faq.md](../faq.md)
