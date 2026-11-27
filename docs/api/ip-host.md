# IP 宿主信息接口

按 IP 地址返回所在行业、商圈、运营商与所属机构，用于识别 IP 所属的行业类型，例如金融、教育、医疗。

## 一、接口说明

IP宿主信息数据查询的服务接口，返回 IP 的所在商圈、所在行业、互联网提供商与所属机构。

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
| `data` | JsonObject | 输出信息 | 字段见 4.1 |

### 4.1 data 输出参数

| 字段名称 | 字段类型 | 字段描述 |
| --- | --- | --- |
| `business` | String | 所在商圈 |
| `industry` | String | 所在行业 |
| `isp` | String | 互联网提供商 |
| `owner` | String | 所属机构 |

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
    "host_information": {
      "business": "徐州软件园",
      "industry": "房产小区",
      "isp": "电信",
      "owner": "电信"
    }
  },
  "msg": "success"
}
```

## 七、接入说明

- 商圈与行业按 IP 所属的机构或小区标注，非商业地段可能为空。
- `owner` 为所属机构名称，与 `isp` 在部分场景下取值相同，业务侧按需取用。
- 行业标签需与业务分类体系做映射，不建议直接作为风控结论使用。

## 八、相关文档

- 开通与首次调用见 [quickstart.md](../quickstart.md)
- 应用场景类型见 [ip-scene.md](ip-scene.md)
- 归属地与位置字段见 [ip-location.md](ip-location.md)
- 常见问题见 [faq.md](../faq.md)
