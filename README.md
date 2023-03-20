# CPay Payment Gateway Integration Tutorial

## Table of Content

- [Integration Flow](#integration-flow)
  - [Step 1: Get Credentials](#step-1-get-credentials)
  - [Step 2: Set IP whitelist](#step-2-set-ip-whitelist)
  - [Step 3: Authentication](#step-3-authentication)
  - [Step 4: Start to Integrate with CPay APIs](#step-4-start-to-integrate-with-cpay-apis)
    - [Using H2H API](#using-h2h-api)
    - [Using WordPress Plugin](#using-wordpress-plugin)
- [How to work](#how-to-work)
    - [Mode 1](#mode-1)
    - [Mode 2](#mode-2)
    - [Mode 3](#mode-3)
    - [Mode 4](#mode-4)
- [Q&A](#qa)


## Integration Flow

### Step 1: Get Credentials
Partners will be provided with the following credentials:
- `MerchantID`
- `SecurityKey`
- `API Host`, see [here](https://github.com/cpayfinance/document/blob/main/api-reference/api-host.md)

### Step 2: Set IP whitelist
Partners should set the IP whitelist in `CPay Merchant System` first, otherwise the request will get invalid response, such as `illegal IP`.

### Step 3: Authentication
The request message will be hashed with the API `SecurityKey` using `SHA256` algorithm.

Examples:
```shell
curl https://domain/openapi/v1/getSth?xx=1001&yy=&aa=hello&sign=Vs23424SHW

curl -X POST 'https://domain/openapi/v1/updateSth' -d 'xx=1001&yy=&aa=hello&sign=Vs23424SHW'
```

1. All parameters will be sorted by parameter name and stitched into a string, e.g. `aa=hello&xx=1001`. 
   > Parameter `yy` is ignored here due to its empty value.

2. Append `SecurityKey` to the end of string `aa=hello&xx=1001`, and you will get a new string, e.g. `aa=hello&xx=1001&key=aaaaaaaaaaxxxxxx`

3. Encrypt the string generated in previous step by `SHA256` algorithm.

4. Convert the ciphertext into lower case.

> See a demo [here](https://github.com/cpayfinance/document/blob/main/api-reference/api-signature.md)


### Step 4: Start to Integrate with CPay APIs

#### Using H2H API
> recommend: using SDK for integration

- Transaction API
  - [Create Crypto Pay-in Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-crypto-pay-in-order)
  - [Create Credit Card Pay-in Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-credit-card-pay-in-order)
  - [Create Crypto Pay-out Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-crypto-pay-out-order)
  - [Create Fiat Pay-out Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-fiat-pay-out-order)
  - [Query Order Info](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#query-payment-order-info)

- Account API
  - [Query Addresses of Crypto Wallet](https://github.com/cpayfinance/document/blob/main/api-reference/api-account.md#query-addresses-of-crypto-wallet)
  - [Query Balance of Merchant](https://github.com/cpayfinance/document/blob/main/api-reference/api-account.md#query-balance-of-merchant)


#### Using WordPress Plugin
If the website of partner built by WordPress, integration using the plugin may be more graceful.
- [Integration Tutorial for WordPress](https://github.com/cpayfinance/document/blob/main/wordpress-plugin-reference/wordpress-plugin.md)


## How `CPay Payment Gateway` Work

### Mode 1
In this mode, the checkout page of bank will be used for payment using credit card.

![integrate-bank-checkout-cc](https://static.cpay.ltd/images/docs/integrate-bank-checkout-cc.png)

> - API [Create Credit Card Pay-in Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-credit-card-pay-in-order)
    will be called with same parameters in `step 1.2`


### Mode 2
In this mode, the checkout page of cpay will be used for payment using credit card.

![integrate-cpay-checkout-cc](https://static.cpay.ltd/images/docs/integrate-cpay-checkout-cc.png)

> - API [Create Credit Card Pay-in Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-credit-card-pay-in-order)
    will be called with same parameters in `step 1.2`


### Mode 3
In this mode, the checkout page of cpay will be used for payment using cryptocurrency.

![integrate-cpay-checkout-crypto1](https://static.cpay.ltd/images/docs/integrate-cpay-checkout-crypto-1.png)

> - API [Create Crypto Pay-in Order](https://github.com/cpayfinance/document/blob/main/api-reference/api-transaction.md#create-crypto-pay-in-order)
    will be called with same parameters in `step 1.2`


### Mode 4
In this mode, payment will be completed by cryptocurrency without a checkout page.

![integrate-cpay-checkout-crypto](https://static.cpay.ltd/images/docs/integrate-cpay-checkout-crypto.png)


## Q&A
Q1: What are the similarities and differences between `Mode 1` and `Mode 2` ?
> 
>> Similarities:
>> - The same API `Create Credit Card Pay-in Order` will be called with same parameters in `step 1.2`.
>> - CPay will notify partners in `step 5`, and retry in ten times until receiving `success`.
>
>> Differences:
>> - The page of checkout url was released by the bank in `Mode 1`, however CPay released the page in `Mode 2`


Q2: What are the similarities and differences between `Mode 3` and `Mode 4` ?
> 
>> Similarities:
>> - They are both used for pay-in with cryptocurrency.
>
>> Differences:
>> - In `Mode 3`, an order of business should be created in partner's system before pay-in. (see figure in section `How to Work#Mode 3`)
>> - In `Mode 4`, users will transfer to cpay's address directly without creating an order of business. After cpay notifying the partner, the order of business may be created. (see figure in section `How to Work#Mode 4`)


Q3: How to configure notify url ? 
> 
> The url can be passed by parameter `callBackURL` or configured in `CPay Merchant System`.  
> If the url was both configured at the same time, the value passed by parameter `callBackURL` will be used first.

