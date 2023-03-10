# APIs of Transaction


## Table of Content

- [Create Crypto Pay-in Order](#create-crypto-pay-in-order)
- [Create Credit Card Pay-in Order](#create-credit-card-pay-in-order)


## Webhooks


## Create Crypto Pay-in Order

### Description
Create a new order for cryptocurrency pay-in.

### Endpoint
**POST** `http[s]://{domain}/openapi/v1/createCryptoOrder`

### Parameters

| Name | Type | Mandatory | Description |
| :---- | :---- | :---- | :---- |
| merchantId | long | yes | unique id generated by cpay for partner |
| userId | string(64) | yes | unique user id generated by partner's system |
| merchantTradeNo | string(64) | yes | unique order id generated by partner's system |
| createTime | long | yes | timestamp(ms) when order created in partner's system |
| cryptoCurrency | string(16) | yes | e.g. `BTC`, `USDT`, `ETH` |
| amount | string(128) | yes | total amount of order. 8 precision supported, e.g. `66.12345678`  |
| callBackURL | string(256) | no | partner will receive notification by the endpoint when the order `COMPLETED` or `CLOSED`. |
| successURL | string(256) | no | the endpoint is used to navigate the users back to the partner's landing page when payment succeeded  |
| failURL | string(256) | no | the endpoint is used to navigate the users back to the partner's landing page when payment failed  |
| sign | string(64) | yes | see [Signature](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md) |

### Response

| Name | Type | Description |
| :---- | :---- | :---- |
| code | int | see [Error Code](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#error-code) |
| data.orderId | string | unique order id generated by cpay |
| data.merchantId | long | same as request parameter of `merchantId` |
| data.mid | long | same as request parameter of `merchantId` |
| data.cryptoCurrency | string | same as request parameter of `cryptoCurrency` |
| data.orderAmount | string | same as request parameter of `totalAmount` |
| data.receivedAmount | string | same as request parameter of `receivedAmount` |
| data.merchantTradeNo | string | same as request parameter of `merchantTradeNo` |
| data.cashierURL | string | the endpoint is a checkout page which users will be navigated by partner, and users will complete the checkout process on the page |
| data.returnURL | string | ~~deprecated~~ |
| data.successURL | string | same as request parameter of `successURL` |
| data.failURL | string | same as request parameter of `failURL` |
| data.remark | string | |

### Example

```shell
curl -X POST 'https://{domain}/openapi/v1/createCryptoOrder' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'merchantId={merchantId}&userId={userId}&merchantTradeNo={merchantTradeNo}&createTime={createTime}&cryptoCurrency={cryptoCurrency}&amount={amount}&callBackURL={callBackURL}&successURL={successURL}&failURL={failURL}&sign={sign}'
```

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "orderId": "2302280607488abc123",
    "mid": 20043900000,
    "merchantId": "20043900000",
    "cryptoCurrency": "ETH",
    "orderAmount": "0.009114",
    "receivedAmount": "0.009114",
    "merchantTradeNo": "Z6789961549",
    "cashierURL": "https://{domain}/payment?orderId=2302280607488abc123&sign=875ee0c50a97d06f1bxxx",
    "returnURL": "",
    "successURL": "",
    "failURL": "",
    "remark": ""
  },
  "traceid": "230228060805X5548324"
}
```

---

## Create Credit Card Pay-in Order 

### Description
Create a new order for credit card pay-in.

### Endpoint
- REST API: *POST `http[s]://{domain}/openapi/v1/createCreditCardOrder`*
- Java SDK: *-*
- PHP SDK: *-*
- Go SDK: *CpaySDK\Transaction::createCreditCardPayinOrder*

### Parameters

| Name | Type | Mandatory | Description |
| :---- | :---- | :---- | :---- |
| merchantId | long | yes | unique id generated by cpay for partner |
| userId | string(64) | yes | unique user id generated by partner's system |
| merchantTradeNo | string(64) | yes | unique order id generated by partner's system |
| createTime | long | yes | timestamp(ms) when order created in partner's system |
| currency | string(16) | yes | refers to `ISO-4217`, e.g. `USD`, `EUR` |
| amount | string(128) | yes | total amount of order. 8 precision supported, e.g. `66.12345678`  |
| products | json string(1024) | yes | order's summary info, e.g. [{"name":"pen","price":"0.5","num":"1","currency":"USD"}]  |
| country | string(16) | yes | country code, refers to `ISO3166-1`, e.g. `US` |
| email | string(128) | yes | partner will receive notification by the endpoint when the order `COMPLETED` or `CLOSED` |
| ip | string(16) | yes | partner will receive notification by the endpoint when the order `COMPLETED` or `CLOSED` |
| callBackURL | string(256) | no | partner will receive notification by the endpoint when the order `COMPLETED` or `CLOSED` |
| successURL | string(256) | no | user will be navigated to the endpoint when payment succeeded  |
| failURL | string(256) | no | endpoint is used to navigate the users back to the partner's landing page when payment failed  |
| sign | string(64) | yes | see [Signature](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md) |

### Response

| Name | Type | Description |
| :---- | :---- | :---- |
| code | int | see [Error Code](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#error-code) |
| data.orderId | string | unique order id generated by cpay |
| data.merchantId | long | same as request parameter of `merchantId` |
| data.orderStatus | string | see [Order Status](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#order-status) |
| data.orderCurrency | string | same as request parameter of `currency` |
| data.orderAmount | string | same as request parameter of `amount` |
| data.receivedAmount | string |  |
| data.pledgeAmount | string |  |
| data.fee | string |  |
| data.merchantTradeNo | string | same as request parameter of `merchantTradeNo` |
| data.cashierURL | string | endpoint of the checkout page which users will be navigated by partner, and users will complete payment on the page |
| data.successURL | string | same as request parameter of `successURL` |
| data.failURL | string | same as request parameter of `failURL` |
| data.sign | string |  |

### Example

```shell
curl -X POST 'https://{domain}/openapi/v1/createCreditCardOrder' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'merchantId={merchantId}&userId={userId}&merchantTradeNo={merchantTradeNo}&createTime={createTime}&currency={currency}&amount={amount}&products={products}&country={country}&email={email}&ip={ip}&callBackURL={callBackURL}&successURL={successURL}&failURL={failURL}&sign={sign}'
```

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "merchantId": 21416460000,
    "merchantTradeNo": "TD2023030922142_5843",
    "orderId": "230309081605509858014",
    "orderStatus": "0",
    "orderCurrency": "EUR",
    "orderAmount": "6.84",
    "receivedAmount": "0",
    "pledgeAmount": "0",
    "fee": "0",
    "cashierURL": "https://{checkout domain}/api/v6/paymentPage/AF6CA1EA730D0659D26E952EA84BEC82DC462C52",
    "successURL": "https://{partner domain}/checkouts/clientcallback/cpay?order_number=TD2023030922142&status=paid",
    "failURL": "https://{partner domain}/checkouts/clientcallback/cpay?order_number=TD2023030922142&status=fail",
    "sign": "09b280696a407a82055be69b882cfc71f097f2c6090fe8a7c5d06d15e4b97290"
  },
  "traceid": "230309081605X5086830"
}
```
---

## Create Crypto Pay-out Order

### Description
Transfer cryptocurrency to external crypto wallet.

### Endpoint
**POST** `http[s]://{domain}/openapi/v1/withdraw`
- REST API: *POST `http[s]://{domain}/openapi/v1/withdraw`*
- Java SDK: *-*
- PHP SDK: *-*
- Go SDK: *CpaySDK\Account::getMerchantBalance*

### Parameters

| Name | Type | Mandatory | Description |
| :---- | :---- | :---- | :---- |
| merchantId | long | yes | unique id generated by cpay for partner |
| userId | string(64) | yes | unique user id generated by partner's system |
| merchantTradeNo | string(64) | yes | unique order id generated by partner's system |
| createTime | long | yes | timestamp(ms) when order created in partner's system |
| cryptoCurrency | string(16) | yes | e.g. `BTC`, `USDT`, `ETH` |
| network | string(16) | yes | e.g. Bitcoin, `Ethereum(ERC-20)`, `TRON(TRC-20)`, `BSC(BEP-20)` |
| totalAmount | string(128) | yes | total amount of order. 8 precision supported, e.g. `66.12345678`  |
| receivedAmount | string(128) | yes | actual amount will be received  |
| toAddress | string(64) | yes | address of external crypto wallet  |
| sign | string(64) | yes | see [Signature](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md) |

### Response

| Name | Type | Description |
| :---- | :---- | :---- |
| code | int | see [Error Code](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#error-code) |
| data.cryptoCurrency | string | same as request parameter of `cryptoCurrency` |
| data.orderAmount | string | same as request parameter of `totalAmount` |
| data.receivedAmount | string | same as request parameter of `receivedAmount` |
| data.fee | string | amount of service fee collects by cpay |
| data.merchantTradeNo | string | same as request parameter of `merchantTradeNo` |
| data.remark | string | |
| data.status | string | see [Order Status](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#order-status) |
| data.orderId | string | unique order id generated by cpay |
| data.network | string | same as request parameter of `network` |
| data.merchantUserId | string | same as request parameter of `userId` |
| data.mid | long | same as request parameter of `merchantId` |
| data.createTime | long | timestamp(ms) when order created in cpay|

### Example

```shell
curl -X POST 'https://{domain}/openapi/v1/withdraw' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'merchantId={merchantId}&userId={userId}&merchantTradeNo={merchantTradeNo}&createTime={createTime}&cryptoCurrency={cryptoCurrency}&network={network}&totalAmount={totalAmount}&receivedAmount={receivedAmount}&toAddress={toAddress}&sign={sign}'
```

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "cryptoCurrency": "USDT",
    "orderAmount": "4.000000",
    "receivedAmount": "3.000000",
    "fee": "1",
    "merchantTradeNo": "F2302281318597644",
    "remark": "",
    "status": "11",
    "orderId": "230228060551113503003",
    "network": "TRON",
    "merchantUserId": "279768",
    "mid": 200012345,
    "createTime": 1677564351000
  },
  "traceid": "230228060551X1119543"
}
```
---


---

## Query Payment Order Info

### Description
Retrieve info of a payment order.

### Endpoint
**GET** `http[s]://{domain}/openapi/v1/getOrderDetail`

### Parameters

| Name | Type | Mandatory | Description |
| :---- | :---- | :---- | :---- |
| merchantId | long | yes | unique id generated by cpay for partner |
| merchantTradeNo | string(64) | no | unique order id generated by partner's system |
| cpayOrderId | string(64) | no | unique order id generated by cpay |
| hash | string(128) | no | transaction hash of blockchain  |
| sign | string(64) | yes | see [Signature](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md) |

> `merchantTradeNo`, `cpayOrderId` and `hash` can not be empty at the same time.
### Response

| Name | Type | Description |
| :---- | :---- | :---- |
| code | int | see [Error Code](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#error-code) |
| data.orderId | string | unique order id generated by cpay |
| data.merchantId | long | same as request parameter of `merchantId` |
| data.status | string | see [Order Status](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#order-status) |
| data.currency | string |  |
| data.receivedAmount | string | merchant account will receive  |
| data.actualAmount | string |  |
| data.pledgeAmount | string |  |
| data.fee | string | service fee charged by cpay  |
| data.merchantTradeNo | string | same as request parameter of `merchantTradeNo` |
| data.merchantUserId | string | unique user id generated by partner's system |
| data.hash | string | unique transaction id generated by the bank or blockchain |
| data.network | string | blockchain network |
| data.remark | string |  |
| data.createTime | long |  |

### Example

```shell
curl -X POST 'https://{domain}/openapi/v1/getOrderDetail?merchantId={merchantId}&merchantTradeNo={merchantTradeNo}&hash={hash}&cpayOrderId={cpayOrderId}&sign={sign}' \
-H 'Content-Type: application/x-www-form-urlencoded'
```

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "actualAmount": "10",
    "createTime": 1663155010000,
    "currency": "USDT",
    "fee": "0",
    "hash": "c4dbbf5737c52072817dc92fa09c41386f8024fee883270b92bfc1943616f957",
    "merchantId": 20003092,
    "merchantTradeNo": "7b785240e426694635bb09fb101ae241",
    "merchantUserId": "test002",
    "network": "TRON",
    "orderId": "2209141130105863014",
    "pledgeAmount": "0",
    "receivedAmount": "10",
    "remark": "some remark info",
    "status": "14"
  },
  "traceid": "221117082056X6478814"
}
```

