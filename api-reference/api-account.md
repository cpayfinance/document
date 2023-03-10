# APIs of Account

## Query Addresses of Crypto Wallet

### Description
Retrieve all available deposit addresses of a user.

> Note:  
> Partner's system need to call this endpoint 
> before your user want to deposit directly from the external crypto wallet, 
> so cpay can generate new addresses for the user. 
> However, the user can not deposit.

### Endpoint
- REST API: *GET `http[s]://{domain}/openapi/v1/getWalletAddress`*
- Java SDK: *-*
- PHP SDK: *CpaySDK\Account::getWalletAddress*
- Go SDK: *-*

### Parameters

| Name | Type | Mandatory | Description |
| :---- | :---- | :---- | :---- |
| merchantId | long | yes | the unique ID generated by cpay for partner |
| userId | string(64) | yes | unique user ID generated by partner's system |
| sign | string(64) | yes | see [Signature](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md) |

### Response

| Name | Type | Description |
| :---- | :---- | :---- |
| code | int | see [Error Code](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#error-code) |
| data[i].address | string |  |
| data[i].currency | string |  |
| data[i].network | string |  |

### Example

```shell
curl 'https://api.cpay.ltd/openapi/v1/getWalletAddress?merchantId={merchantId}&userId={userId}&sign={sign}'
```

```json
{
    "code": 0,
    "msg": "success",
    "data": [
        {
            "address": "TRMYtLHzmTeMm1FW86LZh2LGnE2D7kJnnw",
            "currency": "USDT",
            "network": "TRON(TRC-20)"
        },
        {
            "address": "TRMYtLHzmTeMm1FW86LZh2LGnE2D7kJnnw",
            "currency": "TRX",
            "network": "TRON(TRC-20)"
        },
        {
            "address": "0xcb5e6bc3699d1a92f80d9e1de129e500a58b5221",
            "currency": "USDT",
            "network": "BSC(BEP-20)"
        },
        {
            "address": "0xcb5e6bc3699d1a92f80d9e1de129e500a58b5221",
            "currency": "BNB",
            "network": "BSC(BEP-20)"
        },
        {
            "address": "0x51777edc64e66e953a576d8b90310b2a80f18f67",
            "currency": "USDT",
            "network": "Ethereum(ERC-20)"
        },
        {
            "address": "0x51777edc64e66e953a576d8b90310b2a80f18f67",
            "currency": "ETH",
            "network": "Ethereum(ERC-20)"
        },
        {
            "address": "3Dj8spQNdFUXRP1T91egJx8ZhfDc6QECsa",
            "currency": "BTC",
            "network": "Bitcoin"
        }
    ],
    "traceid": "221117074719X9921729"
}
```
---

## Query Balance of Merchant

### Description
Retrieve balance of merchant's accounts.

### Endpoint
- REST API: *GET `http[s]://{domain}/openapi/v1/getMerchantBalance`*
- Java SDK: *-*
- PHP SDK: *-*
- Go SDK: *CpaySDK\Account::getMerchantBalance*

### Parameters

| Name | Type | Mandatory | Description |
| :---- | :---- | :---- | :---- |
| merchantId | long | yes | the unique ID generated by cpay for partner |
| sign | string(64) | yes | see [Signature](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md) |

### Response

| Name | Type | Description |
| :---- | :---- | :---- |
| code | int | see [Error Code](https://github.com/cpayfinance/document/blob/main/api-reference/api-enum.md#error-code) |
| data[i].availableBalance | string | balance of `checking account`, can be withdrawn and spent |
| data[i].freezeBalance | string | balance of `blocked account`, it will be transfer to `checking account` in time |
| data[i].currency | string |  |
| data[i].cryptoCurrency | string | ~~deprecated~~ |

### Example

```shell
curl 'https://api.cpay.ltd/openapi/v1/getMerchantBalance?merchantId={merchantId}&sign={sign}'
```

```json
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "availableBalance": "0",
      "freezeBalance": "0",
      "currency": "ETH",
      "cryptoCurrency": "ETH"
    },
    {
      "availableBalance": "0",
      "freezeBalance": "0",
      "currency": "TRX",
      "cryptoCurrency": "TRX"
    },
    {
      "availableBalance": "0",
      "freezeBalance": "0",
      "currency": "EUR",
      "cryptoCurrency": "EUR"
    },
    {
      "availableBalance": "0",
      "freezeBalance": "12.237",
      "currency": "USD",
      "cryptoCurrency": "USD"
    },
    {
      "availableBalance": "0",
      "freezeBalance": "0",
      "currency": "BTC",
      "cryptoCurrency": "BTC"
    },
    {
      "availableBalance": "11.008",
      "freezeBalance": "0",
      "currency": "USDT",
      "cryptoCurrency": "USDT"
    },
    {
      "availableBalance": "0",
      "freezeBalance": "0",
      "currency": "BNB",
      "cryptoCurrency": "BNB"
    }
  ],
  "traceid": "221117075237X7604753"
}
```
---