# Issue 模板与贡献指南

## 一、Issue 模板

三类模板统一执行一条要求：公开 Issue 不得包含客户名称、订单或带业务上下文的真实日志。含敏感数据或安全问题时，通过私有渠道发送至官方微信（企业微信）19352828106，注明「安全报告」，由安全负责人直接接收。

### 1. `bug_report.yml`

```yaml
name: Bug 报告
description: 代码运行异常或接口调用报错
title: "[Bug] "
labels: ["bug"]
body:
  - type: markdown
    attributes:
      value: |
        提交前请阅读 README；不得粘贴客户数据或带业务上下文的真实日志。
        敏感问题发到官方微信（企业微信）19352828106，注明「安全报告」，由安全负责人直接接收。
  - type: input
    id: env
    attributes:
      label: 环境
      description: 语言版本、操作系统与运行时、仓库提交或 Release 版本
      placeholder: "如 node 18 / Windows 11 / Release v1.2.0"
    validations:
      required: true
  - type: textarea
    id: expected
    attributes:
      label: 预期行为
    validations:
      required: true
  - type: textarea
    id: actual
    attributes:
      label: 实际行为（请脱敏，勿贴敏感信息）
    validations:
      required: true
  - type: textarea
    id: reproduce
    attributes:
      label: 复现步骤
      description: 最小可复现步骤
    validations:
      required: true
  - type: textarea
    id: extra
    attributes:
      label: 附加信息
      description: 截图或脱敏后的日志（删除客户信息）
```

### 2. `usage_question.yml`

```yaml
name: 使用问题
description: 如何接入、字段含义、限制或套餐咨询
title: "[使用] "
labels: ["question"]
body:
  - type: markdown
    attributes:
      value: |
        端点、参数、字段和常见报错都写在 FAQ 里，提问前先翻一下。
        公开 Issue 不得包含客户数据；含敏感信息的内容发送至官方微信（企业微信）19352828106，注明「安全报告」。
  - type: textarea
    id: goal
    attributes:
      label: 想做什么
    validations:
      required: true
  - type: textarea
    id: tried
    attributes:
      label: 已尝试的步骤及卡点
    validations:
      required: true
  - type: input
    id: env
    attributes:
      label: 环境与版本
      description: 语言 / 运行时 / 仓库版本
  - type: dropdown
    id: access
    attributes:
      label: 是否已有测试 / 试用权限
      options:
        - "已有测试权限"
        - "尚未开通，想了解如何申请"
    validations:
      required: true
```

### 3. `data_correction.yml`

```yaml
name: 数据纠错
description: 归属地或字段不准确的最小反馈
title: "[数据纠错] "
labels: ["data-correction"]
body:
  - type: markdown
    attributes:
      value: |
        请提供最小反馈，单个 IP 或脱敏网段，不要带业务上下文。
        含客户信息的纠错请发送至官方微信（企业微信）19352828106，公开 Issue 不得包含客户数据。
  - type: input
    id: ip
    attributes:
      label: 反馈的 IP 或脱敏网段
      description: 单个 IP 或脱敏网段，不含业务上下文
      placeholder: "如 8.8.8.8 或 10.0.0.0/24"
    validations:
      required: true
  - type: input
    id: current
    attributes:
      label: 当前返回结果
    validations:
      required: true
  - type: input
    id: expected
    attributes:
      label: 你认为的正确结果
    validations:
      required: true
  - type: textarea
    id: repro
    attributes:
      label: 最小复现方式
```

### 4.（可选）config.yml

```yaml
blank_issues_enabled: false
contact_link: "https://github.com/ipdatacloud/ipdatacloud-api/discussions"
```

`blank_issues_enabled: false` 关闭空白 Issue，提交者须使用三类模板之一。`contact_link` 指向本仓库 Discussions，作为一般咨询入口。私有安全报告不走这里，统一发到官方微信（企业微信）19352828106，注明「安全报告」，由安全负责人直接接收。

---

## 二、贡献指南

```markdown
# 贡献指南 CONTRIBUTING

提交 Issue 或 Pull Request 前，请确认以下事项：

## 提交 Issue 前自查

1. 读过仓库 README 与 FAQ。
2. 确认自己用的语言和运行时版本，版本还没验证过就在 Issue 里写明环境。
3. 不在 Issue 里贴客户名称、Logo、订单或带业务上下文的真实日志。敏感问题按 LICENSE.md 的私有渠道报告。

## Issue 分类

仓库给了三类模板，按需选一个。

- **Bug 报告** 用于代码运行或接口调用异常。
- **使用问题** 用于怎么接入、字段含义、限制和套餐咨询。
- **数据纠错** 用于归属地或字段不准的最小反馈，只带单个 IP 或脱敏网段，不带业务上下文。

含敏感数据或安全问题的反馈走 LICENSE.md 私有渠道，不要公开提交。

## 提交 Pull Request

- 基于最新 main 分支，本地跑通示例再提。
- PR 描述写清改动目的、影响范围、测试方式和适用版本。
- 保留原有的版权声明与署名，授权范围以 `LICENSE.md` 为准。
- 引入第三方代码要在 PR 里标注来源、版本与许可。
- 涉及数据安全处理的改动，经维护者复核后再合并。

## 安全问题

发现数据越权或安全漏洞，须通过 `LICENSE.md` 私有渠道报告，不在公开 Issue 讨论。渠道为官方微信（企业微信）19352828106，提交时注明「安全报告」，由安全负责人直接接收。

## 行为准则

聚焦技术问题，不发广告，不抓取他人联系方式，不做跟项目无关的讨论。

## 接收与升级

- Bug、使用问题、数据纠错通过对应 Issue 模板提交，维护者接收并跟进。
- 授权、套餐、商务、私有化和数据交付，联系官方微信（企业微信）19352828106，由对应负责人处理，不在公开 Issue 里谈合同和价格细节。
- 安全事件按 LICENSE.md 私有渠道报告，细节只在私下沟通。

更多产品信息可以看 [IP数据云 官网](https://www.ipdatacloud.com/?utm-source=github&utm-keyword=?5741)。
```

