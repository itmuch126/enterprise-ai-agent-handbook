# Spring Boot Mock API 契约

版本：企业AI平台与Agent应用落地手册 v1.0  
阶段：v1.1 样板深化阶段  
更新时间：2026-06-29  

## 1. 说明

本文档为企业 AI 平台与业务 Agent 演示准备 Mock API 契约。所有接口路径、字段、错误码均为示例，需结合企业实际系统确认，不代表真实生产接口。

所有接口建议携带 `traceId`、`requestId`，并在服务端完成权限校验、规则校验、审计日志和幂等控制。

## 2. API 清单

| 接口名称 | 接口路径 | 请求方法 | 接口说明 | 请求参数示例 | 响应字段示例 | 权限要求 | 错误码示例 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 用户查询 API | `/mock/users/{userId}` | GET | 查询用户或客户摘要 | `userId=mock_user_001` | `userId,status,maskedMobile,riskLevel` | 用户查看权限 | `USER_NOT_FOUND`,`PERMISSION_DENIED` | 示例 |
| 订单查询 API | `/mock/orders/{orderId}` | GET | 查询订单基础信息 | `orderId=MOCK_ORDER_001` | `orderId,orderStatus,orderAmount` | 订单查看权限 | `ORDER_NOT_FOUND` | 示例 |
| 订单详情 API | `/mock/orders/{orderId}/detail` | GET | 查询订单详情和明细 | `orderId=MOCK_ORDER_001` | `items,contractId,paymentStatus` | 订单详情权限 | `PERMISSION_DENIED` | 示例 |
| 订单状态 API | `/mock/orders/{orderId}/status` | GET | 查询订单状态和履约状态 | `orderId=MOCK_ORDER_001` | `orderStatus,fulfillmentStatus,timeline` | 订单状态权限 | `DOWNSTREAM_TIMEOUT` | 示例 |
| 结算单查询 API | `/mock/settlements/{settlementId}` | GET | 查询结算单摘要 | `settlementId=MOCK_SETTLE_001` | `settlementNo,amount,status` | 结算查看权限 | `SETTLEMENT_NOT_FOUND` | 示例 |
| 结算规则查询 API | `/mock/settlement-rules/{ruleId}` | GET | 查询结算规则版本和说明 | `ruleId=SETTLE-RULE-001` | `ruleId,ruleVersion,description` | 规则查看权限 | `RULE_NOT_FOUND` | 示例 |
| 财务应收查询 API | `/mock/finance/receivables` | GET | 查询应收摘要 | `customerId=mock_customer_001&period=2026-06` | `receivableAmount,agingDays,status` | 财务应收权限 | `PERMISSION_DENIED` | 示例 |
| 财务应付查询 API | `/mock/finance/payables` | GET | 查询应付摘要 | `supplierId=mock_supplier_001&period=2026-06` | `payableAmount,paymentStatus` | 财务应付权限 | `PERMISSION_DENIED` | 示例 |
| 发票查询 API | `/mock/invoices/{invoiceId}` | GET | 查询发票状态 | `invoiceId=MOCK_INVOICE_001` | `invoiceNo,invoiceStatus,taxAmount` | 发票查看权限 | `INVOICE_NOT_FOUND` | 示例 |
| 凭证查询 API | `/mock/vouchers/{voucherId}` | GET | 查询凭证状态 | `voucherId=MOCK_VOUCHER_001` | `voucherNo,postingStatus` | 凭证查看权限 | `VOUCHER_NOT_FOUND` | 示例 |
| 对账差异查询 API | `/mock/reconciliation/differences` | GET | 查询对账差异结果 | `settlementId=MOCK_SETTLE_001` | `differenceAmount,differenceReason` | 对账权限 | `RECONCILE_NOT_READY` | 示例 |
| 审批发起 API | `/mock/workflow/approval` | POST | 发起高风险动作审批 | `actionType=SETTLEMENT_ADJUST` | `approvalId,approvalStatus` | 审批发起权限 | `APPROVAL_REQUIRED`,`PERMISSION_DENIED` | 示例 |
| 审计日志写入 API | `/mock/audit/logs` | POST | 写入操作审计日志 | `traceId,actionType,resourceId` | `auditId,writeStatus` | 系统审计权限 | `AUDIT_WRITE_FAILED` | 示例 |

## 3. 通用请求头

| Header | 说明 | 示例 |
| --- | --- | --- |
| `X-Trace-Id` | 链路追踪 ID | `TRACE_MOCK_001` |
| `X-Request-Id` | 请求 ID | `REQ_MOCK_001` |
| `X-User-Id` | 当前用户 | `mock_user_001` |
| `X-Agent-Code` | Agent 编码 | `order_agent` |

## 4. 通用响应结构

```json
{
  "traceId": "TRACE_MOCK_001",
  "requestId": "REQ_MOCK_001",
  "status": "success",
  "data": {}
}
```

错误响应见 [error-response.sample.json](sample-json/error-response.sample.json)。
