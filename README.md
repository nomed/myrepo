
FORMAT: 1A
HOST: https://payments.cancanmobile.com/api/v2/

# CANCAN

CANCAN is a Payment Gateway Integration Platform that integrates Point Of Sale
systems with multiple Mobile Payment Gateways including WeChat Pay, Alipay and
LINE Pay.

## Definitions

### Merchant

A Merchant is business entity that provides goods and services to Customers. A
Merchant has a business relationship with CANCAN.

CANCAN provides Merchants with simple unified integration with Mobile Payment
Service Providers.

### Customer

A Customer is a consumer who makes a purchases from a Merchant. They use their
preferred Mobile Payment Service Provider to pay for goods and services. CANCAN
facilitates this transaction for the Merchant.

## Authentication

Authenticate your account when using the API by including your secret API key in
the request in `Authorization` HTTP header. You can retrieve your API key from
account settings page.

Example:

```
curl --include \
     --request GET \
     --header "Content-Type: application/json" \
     --header "Authorization: Bearer d6fb50==" \
'https://payments.cancanmobile.com/api/v2/transactions/fb8cc042'
```

## HTTP Responses

### Example response

```
{
  "data": {
    "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
    "type": "sale",
    "attributes": {
      "state": "pending",
      "payment_gateway": "linepay_th_domestic",
      "amount": 10000,
      "currency": "THB",
      "description": "Item 246",
      "pos_id": "POS123",
      "mch_level_1": "Thailand",
      "mch_level_2": "Bangkok",
      "mch_level_3": "Bang Rak",
      "failure_code": null,
      "failure_detail": null,
      "created_at": "2017-01-18T09:53:52Z"
    }
  }
}
```

### Top-level

Each HTTP response to CANCAN API contains a JSON document. Each JSON document
has one of the following top-level members(but not both at the same time):

Name | Description
---------- | -------
data | the document's "primary data"
errors | an error object

### Primary data

Primary data is either:

* a single resource object, for requests that target single resources

* an array of resource objects, or an empty array (`[]`), for requests that target
resource collections

### Resource objects

A resource object has the following members:

Name | Description
---------- | -------
id | Unique identifier of an object, provided by POS system upon object creation.
type | the type of an object, in singular form
attributes | an attributes object, representing some of the resource’s data.

## Errors

CANCAN API uses conventional HTTP response codes to indicate the success or
failure of an API request. In general, codes in the `2xx` range indicate
success, codes in the `4xx` range indicate an error that failed given the
information provided (e.g., a required parameter was omitted etc.), and codes
in the `5xx` range indicate an error with CANCAN's servers.

If error occurs, an error object is returned as an array keyed by `errors`
in the top level of a JSON document.

Each element of an array contains an object with following attributes:

Name | Description
---------- | -------
status | the HTTP status code applicable to this problem
code | a short string describing the kind of error that occurred
detail | a human-readable explanation specific to this occurrence of the problem

Example:

```
{
  "errors": [
    {
      "status": "404"
      "code": "not_found_error",
      "detail": "Record Not Found"
    }
  ]
}
```

#### HTTP status codes summary

HTTP status code | Meaning
---------- | -------
200 | Everything worked as expected.
201 | Resource successfully created.
400 | The request was unacceptable, often due to missing a required parameter.
401 | No valid API key provided.
404 | The requested resource doesn't exist.
429 | Too many requests hit the API too quickly. We recommend an exponential backoff of your requests.
500, 502, 503, 504 | Something went wrong on CANCAN's end.

#### Error codes summary

Error code | Meaning
---------- | -------
authentication_error | Authentication Error
attribute_error | Attribute Error
access_forbidden_error | Access Forbidden
not_found_error | Record Not Found

### Transaction failures

If transaction has failed, the `state` attribute of the resource object
will contain value `failed`. The details of the failure will be present in
attributes `failure_code` and `failure_detail`.

#### Failure codes summary

Failure code | Meaning
---------- | -------
bank_failure | System has failed while communicating with the bank
card_failure | System has failed while charging the card
customer_insufficient_balance_failure | Customer has insufficient balance
customer_not_allowed_to_pay_failure | Customer is not allowed to pay
payment_feature_disabled_failure | Payment feature is disabled by the customer
merchant_insufficient_balance_failure | Merchant has insufficient balance
merchant_not_allowed_to_sell_failure | Merchant is not allowed to sell
payment_auth_key_failure | Payment authorization key is incorrect or expired
payment_gateway_failure | Payment Gateway Failure
payment_password_failure | User is required to enter payment password
refund_exceeds_sale_amount_failure | Refund amount exceeds sale amount
refund_not_allowed_failure | Refund for this transaction is not allowed
sale_not_found_failure | Sale Not Found
transaction_id_failure | Transaction ID is in incorrect format or not unique
unknown_failure | Unknown Failure
void_not_allowed_failure | Void for this transaction is not allowed

## WeChat Specific Documentation

### Video tutorial of how to use WeChat Pay

A video showing how WeChat Pay is used is available [here](https://drive.google.com/file/d/0B2vhkwBgkDy5ZWFBMWkxSVZDVTQ/view?usp=sharing).

### Install WeChat app on mobile device

You will first need to install WeChat app on your mobile device.

- [Download iOS version here](https://itunes.apple.com/en/app/wechat/id414478124)
- [Download Android version here](https://play.google.com/store/apps/details?id=com.tencent.mm)

In order to test in the CANCAN sandbox environment, you need a WeChat test account.
We can provide one for you by email.

### WeChat Domestic vs. WeChat International test accounts

WeChat International test transactions should be denominated in USD.
WeChat Domestic transactions should be denominated in CNY.

### How to Log In to the WeChat International App

1. Download the WeChat app if you do not already have it on your mobile device:

2. If you already have WeChat and are logged in, click on the “Me” tab, then “Log Out”, then “More”
    ![WeChat_IntApp_Step2](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/WeChat_IntApp_Step2.png)
3. Click “Switch Account”, then “Change Login Mode” to enable “WeChat ID/Email/QQ ID” login.
    ![WeChat_IntApp_Step3](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/WeChat_IntApp_Step3.png)
4. Enter account # and password. Then click on “Me” tab, and then select “Wallet”
    ![WeChat_IntApp_Step4](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/WeChat_IntApp_Step4.png)
5. Click “Quick Pay”. Enter 6 digit payment password to enable Quick Pay barcode if requested.
    ![WeChat_IntApp_Step5](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/WeChat_IntApp_Step5.png)

## Alipay Specific Documentation

### Overview

The consumer using Alipay to pay for goods and services from a Merchant.

### Alipay Domestic vs. Alipay International test apps and accounts

Alipay Domestic transactions should be denominated in CNY.
- The Alipay Domestic test account uses a special test app which can
be downloaded [here](https://sandbox.alipaydev.com/user/downloadApp.htm)
- The Alipay International test account uses the regular Alipay app, and transactions
should be denominated in USD. The regular Alipay mobile app can be installed by downloading it from the following
locations:

- iOS - https://itunes.apple.com/us/app/zhi-fu-bao-zhifubao-kou-bei/id333206289
- Android - https://play.google.com/store/apps/details?id=com.eg.android.AlipayGphone

In order to test in a staging environment, you need the correct Alipay Domestic and/or International
test account. We can provide one or both for you by email.

**PLEASE NOTE THAT THE ALIPAY INTERNATIONAL TEST APP EXECUTES TRANSACTIONS WITH REAL MONEY.**
**PLEASE LIMIT ALIPAY INTERNATIONAL TRANSACTION AMOUNT TO 1 CENT.**

### Payment

Here is a link to a video demoing how to pay with Alipay at point-of-sale:

https://www.youtube.com/watch?v=fzI5TaA_eEg

1. Open Alipay app
    ![point out alipay icon](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/point_out_alipay_icon.png)
2. Login
    ![login](https://s3-ap-southeast-1.amazonaws.com/cancandocs/images/alipay_app_login.png)
3. Got to the mobile payment section (see video)
    ![push alipay app icon](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/point_out_alipay_pay_button.png)
4. Display QR Code
    ![push alipay app icon](https://cancandocs.s3-ap-southeast-1.amazonaws.com/images/alipay_barcode.png)
5. Scan with Barcode Scanner OR read number and use value for 'customer_key'
    parameter in API call.
6. Parse JSON response


## LINE Pay Specific Documentation

### Video tutorial of how to use LINE Pay

A video tutorial on how to use LINE Pay is available [here](https://www.youtube.com/watch?v=A-W3PYINNIc).

### Install LINE app on mobile device

You will first need to install LINE app on your mobile device.

- [Download iOS version here](https://itunes.apple.com/EN/app/line/id443904275)
- [Download Android version here](https://play.google.com/store/apps/details?id=jp.naver.line.android)

### LINE Pay simulator for sandbox environments

LINE Pay offers a LINE Pay web simulator at the following url:

https://sandbox-web-pay.line.me/web/sandbox/payment/otk





# Group Transactions

## Create a Transaction [/transactions]

### Create a Sale [POST]

Creates sale transaction, using payment amount, currency, and other
sale-related attributes.

+ Request (application/json)

    + Attributes

        + `data` (required, object)
          + `id`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (required, string)
            An identifier of transaction, unique to merchant.
          + `type`: `sale` (required, string)
            Transaction type.
          + `attributes` (required, object)
              + `payment_gateway`: `linepay_th_domestic`(required, string)
                The code of payment gateway to use for transaction
                processing. Must be enabled for merchant's account.

              + `payment_auth_key`: `081149423436` (required, string)
                Payment authorization key for current transaction. Usually
                is a result of scanning barcode/QR code from Customer's
                smartphone screen.

              + `amount`: `100` (required, number)
                A positive integer in the smallest currency unit (e.g., 100
                cents to charge $1.00 or 100 to charge ¥100, a 0-decimal
                currency) representing how much to charge.

              + `currency`: `THB` (required, string)
                Three-letter ISO currency code representing the currency in
                which the payment was made.

              + `description`: `Item 246` (required, string)
                A free-form description of the transaction.

              + `pos_id`: `POS123` (string)
                A unique identifier of POS system.

              + `mch_level_1`: `Thailand` (string)
                Arbitrary string.

              + `mch_level_2`: `Bangkok` (string)
                Arbitrary string.

              + `mch_level_3`: `Bang Rak` (string)
                Arbitrary string.

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "sale",
                "attributes": {
                  "payment_gateway": "linepay_th_domestic",
                  "payment_auth_key": "081149423436",
                  "amount": 10000,
                  "currency": "THB",
                  "description": "Item 246",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak"
                }
              }
            }

+ Response 201 (application/json)

    + Attributes(Sale)

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "sale",
                "attributes": {
                  "state": "pending",
                  "payment_gateway": "linepay_th_domestic",
                  "amount": 10000,
                  "currency": "THB",
                  "description": "Item 246",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak",
                  "failure_code": null,
                  "failure_detail": null,
                  "created_at": "2017-01-18T09:53:52Z"
                }
              }
            }

### Create a Void [POST]

Creates void transaction, that voids previously created sale transaction.
Merchant can void sale transaction while it's pending or within the short time
after it has been completed.

+ Request (application/json)

    + Attributes

        + `data` (required, object)
          + `id`: `ee96aaf7-9ec7-4baf-bf88-c0f77db672e0` (required, string)
            An identifier of transaction, unique to merchant.
          + `type`: `void` (required, string)
            Transaction type.
          + `attributes` (required, object)
              + `sale`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4`(required, string)
                The unique identifier of a sale transaction to void.

              + `description`: `Item 246` (string)
                A free-form description of the transaction.

              + `pos_id`: `POS123` (string)
                A unique identifier of POS system.

              + `mch_level_1`: `Thailand` (string)
                Arbitrary string.

              + `mch_level_2`: `Bangkok` (string)
                Arbitrary string.

              + `mch_level_3`: `Bang Rak` (string)
                Arbitrary string.

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "void",
                "attributes": {
                  "sale": "c2f5a10a-d7a7-4d69-83fa-b57c343d22d4",
                  "description": "customer has changed her mind",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak"
                }
              }
            }

+ Response 201 (application/json)

    + Attributes(Void)

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "void",
                "attributes": {
                  "state": "pending",
                  "sale": "c2f5a10a-d7a7-4d69-83fa-b57c343d22d4",
                  "description": "customer has changed her mind",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak",
                  "failure_code": null,
                  "failure_detail": null,
                  "created_at": "2017-01-18T09:53:52Z"
                }
              }
            }

### Create a Refund [POST]

Creates refund transaction, that refunds previously created sale transaction,
after sale has been completed.

Some payment gateways, but not all of them, allow partial refunds.

+ Request (application/json)

    + Attributes

        + `data` (required, object)
          + `id`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (required, string)
            An identifier of transaction, unique to merchant.
          + `type`: `refund` (required, string)
            Transaction type.
          + `attributes` (required, object)
              + `sale`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4`(required, string)
                The unique identifier of a sale transaction to refund.

              + `amount`: `100` (required, number)
                A positive integer in the smallest currency unit (e.g., 100
                cents to charge $1.00 or 100 to charge ¥100, a 0-decimal
                currency) representing how much to charge.

              + `description`: `Item 246` (string)
                A free-form description of the transaction.

              + `pos_id`: `POS123` (string)
                A unique identifier of POS system.

              + `mch_level_1`: `Thailand` (string)
                Arbitrary string.

              + `mch_level_2`: `Bangkok` (string)
                Arbitrary string.

              + `mch_level_3`: `Bang Rak` (string)
                Arbitrary string.

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "refund",
                "attributes": {
                  "sale": "c2f5a10a-d7a7-4d69-83fa-b57c343d22d4",
                  "amount": 10000,
                  "description": "factory defect",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak"
                }
              }
            }

+ Response 201 (application/json)

    + Attributes(Refund)

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "refund",
                "attributes": {
                  "state": "pending",
                  "sale": "c2f5a10a-d7a7-4d69-83fa-b57c343d22d4",
                  "description": "factory defect",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak",
                  "failure_code": null,
                  "failure_detail": null,
                  "created_at": "2017-01-18T09:53:52Z"
                }
              }
            }

## Retrieve a Transaction [/transactions/{id}]

Retrieves the details of a transaction that has previously been created. Supply
the unique identifier of a transaction, and CANCAN will return the corresponding
transaction information.

### Retrieve a Sale [GET]

Retrieves a sale transaction.

+ Parameters

    + id: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (string)
        The unique identifier of a transaction.

+ Response 200 (application/json)

    + Attributes (Sale)

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "sale",
                "attributes": {
                  "state": "pending",
                  "payment_gateway": "linepay_th_domestic",
                  "amount": 10000,
                  "currency": "THB",
                  "description": "Item 246",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak",
                  "failure_code": null,
                  "failure_detail": null,
                  "created_at": "2017-01-18T09:53:52Z"
                }
              }
            }

### Retrieve a Void [GET]

Retrieves a void transaction.

+ Parameters

    + id: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (string)
        The unique identifier of a transaction.

+ Response 200 (application/json)

    + Attributes (Void)

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "void",
                "attributes": {
                  "state": "pending",
                  "sale": "c2f5a10a-d7a7-4d69-83fa-b57c343d22d4",
                  "description": "customer has changed her mind",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak",
                  "failure_code": null,
                  "failure_detail": null,
                  "created_at": "2017-01-18T09:53:52Z"
                }
              }
            }

### Retrieve a Refund [GET]

Retrieves a refund transaction.

+ Parameters

    + id: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (string)
        The unique identifier of a transaction.

+ Request (application/json)

+ Response 200 (application/json)

    + Attributes (Refund)

    + Body

            {
              "data": {
                "id": "ee96aaf7-9ec7-4baf-bf88-c0f77db672e0",
                "type": "refund",
                "attributes": {
                  "state": "pending",
                  "sale": "c2f5a10a-d7a7-4d69-83fa-b57c343d22d4",
                  "description": "factory defect",
                  "pos_id": "POS123",
                  "mch_level_1": "Thailand",
                  "mch_level_2": "Bangkok",
                  "mch_level_3": "Bang Rak",
                  "failure_code": null,
                  "failure_detail": null,
                  "created_at": "2017-01-18T09:53:52Z"
                }
              }
            }

# Data Structures

## Sale

+ `data` (object)
  + `id`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (string)
    An identifier of transaction, unique to merchant.
  + `type`: `sale` (string)
    Transaction type.
  + `attributes` (object)
      + `state`: `pending` (enum)
          + pending
          + succeeded
          + failed

      + `payment_gateway`: `linepay_th_domestic`(string)
        The code of payment gateway to use for transaction
        processing. Must be enabled for merchant's account.

      + `amount`: `100` (number)
        A positive integer in the smallest currency unit (e.g., 100
        cents to charge $1.00 or 100 to charge ¥100, a 0-decimal
        currency) representing how much to charge.

      + `currency`: `THB` (string)
        Three-letter ISO currency code representing the currency in
        which the payment was made.

      + `description`: `Item 246` (string)
        A free-form description of the transaction.

      + `pos_id`: `POS123` (string)
        A unique identifier of POS system.

      + `mch_level_1`: `Thailand` (string)
        Arbitrary string.

      + `mch_level_2`: `Bangkok` (string)
        Arbitrary string.

      + `mch_level_3`: `Bang Rak` (string)
        Arbitrary string.

      + `failure_code`: `transaction_denied_failure` (string)
          a short string describing the kind of failure that occurred.
          `null` if no failure occurred.

      + `failure_detail`: `Transaction has been denied by customer` (string)
          Human-readable failure message. `null` if no failure occurred.

      + `created_at`: `2017-01-18T09:53:52Z` (string)
          Transaction creation time in UTC time zone (ISO 8601).

# Void

+ `data` (object)
  + `id`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (string)
    An identifier of transaction, unique to merchant.
  + `type`: `void` (string)
    Transaction type.
  + `attributes` (object)
      + `state`: `pending` (enum)
          + pending
          + succeeded
          + failed

      + `sale`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4`(string)
        The unique identifier of a sale transaction to void.

      + `description`: `Item 246` (string)
        A free-form description of the transaction.

      + `pos_id`: `POS123` (string)
        A unique identifier of POS system.

      + `mch_level_1`: `Thailand` (string)
        Arbitrary string.

      + `mch_level_2`: `Bangkok` (string)
        Arbitrary string.

      + `mch_level_3`: `Bang Rak` (string)
        Arbitrary string.

      + `failure_code`: `transaction_denied_failure` (string)
          a short string describing the kind of failure that occurred.
          `null` if no failure occurred.

      + `failure_detail`: `Transaction has been denied by customer` (string)
          Human-readable failure message. `null` if no failure occurred.

      + `created_at`: `2017-01-18T09:53:52Z` (string)
          Transaction creation time in UTC time zone (ISO 8601).

## Refund

+ `data` (object)
  + `id`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4` (string)
    An identifier of transaction, unique to merchant.
  + `type`: `refund` (string)
    Transaction type.
  + `attributes` (object)
      + `state`: `pending` (enum)
          + pending
          + succeeded
          + failed

      + `sale`: `c2f5a10a-d7a7-4d69-83fa-b57c343d22d4`(string)
        The unique identifier of a sale transaction to void.

      + `amount`: `100` (number)
          A positive integer in the smallest currency unit (e.g., 100
          cents to charge $1.00 or 100 to charge ¥100, a 0-decimal
          currency) representing how much to charge.

      + `description`: `Item 246` (string)
        A free-form description of the transaction.

      + `pos_id`: `POS123` (string)
        A unique identifier of POS system.

      + `mch_level_1`: `Thailand` (string)
        Arbitrary string.

      + `mch_level_2`: `Bangkok` (string)
        Arbitrary string.

      + `mch_level_3`: `Bang Rak` (string)
        Arbitrary string.

      + `failure_code`: `transaction_denied_failure` (string)
          a short string describing the kind of failure that occurred.
          `null` if no failure occurred.

      + `failure_detail`: `Transaction has been denied by customer` (string)
          Human-readable failure message. `null` if no failure occurred.

      + `created_at`: `2017-01-18T09:53:52Z` (string)
          Transaction creation time in UTC time zone (ISO 8601).











