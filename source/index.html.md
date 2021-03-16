---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the OttoStamp API! You can use our API to access OttoStamp API endpoints.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

**Institution ID**: `OS0001`   
**API Key**: `4e64e11b-f132-4670-bf34-6f609346fb98`  

**Development**: `https://apidev.ottopoint.id/ottostamp`  
**Staging**: ``  
# Signature

### How To Generate Signature

> Example Generate Signature:

```go
func SignReplaceAll(str string) string {
	regx := regexp.MustCompile("[^a-zA-Z0-9{}:.,]")
	output := regx.ReplaceAllLiteralString(str, "")
	output = strings.ToLower(output)
	return output
}

func HashSha512(secret, data string) string {
	hash := hmac.New(sha512.New, []byte(secret))
	hash.Write([]byte(data))
	return fmt.Sprintf("%x", hash.Sum(nil))
}

ApiKey := "669b6fdc-175d-4fcc-9e26-5f3551c5331f"
InsitutionID := "OS001"
AppsID := "appsId"
DeviceID := "869552045462447"
ChannelID := "H2H"
Geolocation := "-6.231340755705505, 106.84526008386358"
Timestamp := "1579666534"

// ex Request Body
type Request struct {
  MerchantPhone string `json:"merchantPhone"`
}

data := Request{
  MerchantPhone: "0813469989"
}

marshal, _ := json.Marshal(data)
stringify := string(marshal)

jsonReqString := SignReplaceAll(stringify)

plainText := fmt.Sprint(
  jsonReqString +
   "&" + DeviceID + 
   "&" + InstitutionID + 
   "&" + Geolocation + 
   "&" + ChannelID + 
   "&" + AppsID + 
   "&" + Timestamp + 
   "&" ApiKey,
)

signature := HashSha512(plainText)

// ex Signature 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91
```
# Merchant

## Register Merchant

### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | false | string |

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
name | true | string | 100 | ex. Soursally, nama merchant
phoneNumber | true | string | 20 | ex. 081299465052, nomor telepon merchant
email | false | string | 100 | ex. merchant@soursally.com, email utama merchant
address | false | string | 200 | ex. jl. H Gemin, alamat merchant
businessName | false | string | 
businessType | false | integer |

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/register" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
```

> Request body:

``` json
{
    "name": "Toko Taufik #1",
    "phoneNumber": "0813469989",
    "email": "toko@gmail.com",
    "address": "jl. toko no 3",
    "businessName": "Toko Gw",
    "businessType": 1
}
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "id": "e34dc465-fb11-4f16-855a-f69ead5e3c46",
        "uid": "M53516896",
        "partnerId": "c10ae1b6-f4fe-4615-b2cb-de59441f60ac",
        "name": "Toko Taufik #1",
        "phoneNumber": "0813469989",
        "email": "toko@gmail.com",
        "businessNamne": "Toko Gw",
        "businessType": 1,
        "address": "jl. toko no 3",
        "status": "Active",
        "createdAt": "2021-03-10T20:03:20.396952Z",
        "createdBy": "System",
        "updatedAt": "2021-03-10T20:03:20.396952Z",
        "updatedBy": "System"
    }
}
```

### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Merchant berhasil terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`13` | Failed | Nomor Telepon Merchant Sudah Terdaftar |
`99` | General Error | |

## Generate Token

### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | false | string |

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | ----------- |
merchantPhone | true | string | 20 | nomor telepon merchant, ex format `081299465055`

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/token" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
```

> Request body:

```json
{
    "merchantPhone": "0813469989"
}
```

> Example response:

```json
{
  "responseCode": "00",
  "responseDesc": "Success",
  "data": {
    "token": "MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
  }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Token Berhasil di buat |
`03` | Failed | Nomor Telepon Merchant Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`99` | General Error | |

## History Earning / Redemption

### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorization | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Query Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | ----------- |
recordPerPage | true | string | | The number of search results to return per page
currentPage | true | string | | Ex 1

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/transaction/v1/merchant/history/list" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "histories": [
            {
                "id": "197d825f-892d-4daf-b975-4528da7ea1d6",
                "transactionId": "a0d89fa9-475c-4373-8696-2d4b6556bca3",
                "comment": "Promo Deleted",
                "value": "10",
                "referenceId": "f69717a4-f784-4902-8b04-7d5b4e46fbb4",
                "code": "RM01",
                "type": "Promo Deleted",
                "transactionTime": "2021-03-12T04:23:40.18845Z",
                "customerName": "Taufik",
                "customerPhone": "081345679912"
            },
            {
                "id": "695421b0-04fd-486b-b906-114b4ea53720",
                "transactionId": "db624b33-23b2-4111-b87d-f3656e0e8ade",
                "comment": "Promo Deleted",
                "value": "0",
                "referenceId": "f69717a4-f784-4902-8b04-7d5b4e46fbb4",
                "code": "RM01",
                "type": "Promo Deleted",
                "transactionTime": "2021-03-12T04:23:39.98357Z",
                "customerName": "Taufik",
                "customerPhone": "081345679922"
            },
            {
                "id": "fec8c5ff-f0c0-4a05-bf89-bcd443e7481f",
                "transactionId": "150dcf64-c9e3-4739-9a61-0fb17e009308",
                "comment": "Earning Stamp",
                "value": "10",
                "referenceId": "0b80d9a6-0b9d-4294-8f8a-8fb5af21efdb",
                "code": "AD01",
                "type": "Adding",
                "transactionTime": "2021-03-10T18:55:00Z",
                "customerName": "Taufik",
                "customerPhone": "081345679912"
            },
            {
                "id": "ed2c85ce-3f7f-452e-af1b-63e6f9c68965",
                "transactionId": "a3c75923-26ee-4a90-8eda-592e79b327d0",
                "comment": "Spending Stamp",
                "value": "5",
                "referenceId": "ca0f73b6-4ff9-462a-855b-facc1fa349e0",
                "code": "SP01",
                "type": "Spending",
                "transactionTime": "2021-03-10T15:24:00Z",
                "customerName": "Taufik",
                "customerPhone": "081345679912"
            },
            {
                "id": "cfa9d8b8-c9fa-4dc1-ae19-86cc8e6c6a74",
                "transactionId": "a031c18c-2a5d-45fa-b1fe-21dcdbcac22f",
                "comment": "Spending Stamp",
                "value": "5",
                "referenceId": "cf8b6598-fae0-41f8-b3a0-b3ddfc85d285",
                "code": "SP01",
                "type": "Spending",
                "transactionTime": "2021-03-10T15:12:00Z",
                "customerName": "Taufik",
                "customerPhone": "081345679912"
            }
        ],
        "totalRecord": 6,
        "recordPerPage": 5,
        "currentPage": 1,
        "totalPage": 2
    }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Token Berhasil di buat |
`03` | Failed | Nomor Telepon Merchant Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`99` | General Error | |

## Reporting

### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorization | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Query Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | ----------- |
startDate | true | string | | Ex `2021-01-01`, Format `YYYY-MM-DD`
endDate | true | string | | Ex `2021-01-30`, Format `YYYY-MM-DD`

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/transaction/history/report" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "totalEarned": 20,
        "totalRedeemed": 10,
        "totalDeleted": 10,
        "available": 10
    }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Token Berhasil di buat |
`03` | Failed | Nomor Telepon Merchant Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`99` | General Error | |

# Customer
## Add Customer
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
customerPhone | true | string | 20 | nomor telepon customer, ex format `081299465055`
customerName | true | string | 100 | name lengkap customer
cutomerEmail | true | string | 40 | email customer

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/customer/register" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Request body:

```json
{
    "customerPhone": "081345679912",
    "customerName": "Taufik",
    "customerEmail": "taufik@mail.com"
}
```

> Example response:

```json
{
  "responseCode": "00",
  "responseDesc": "Success",
  "data": {
    "id": "cfc4dfac-a843-41c4-961e-304b530f3a16",
    "uid": "C99430886",
    "name": "Taufik",
    "phone": "081345679912",
    "email": "taufik@mail.com",
    "status": "Active",
    "createdAt": "2021-03-10T07:46:09.484887Z",
    "createdBy": "System",
    "updatedAt": "2021-03-10T07:46:09.484887Z",
    "updatedBy": "System"
  }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Customer berhasil di tambahkan pada merchant / Nomor Customer sudah terdaftar pada sistem dan di link pada merchant |
`03` | Failed | Nomor Telepon Customer Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`99` | General Error | |

## Check Balance
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Query Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
customerPhone | true | string | 20 | nomor telepon merchant, ex format `081299465055`

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/transaction/v1/customer/balance?customerPhone=081299465055" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "balance": "0"
    }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | |
`12` | Failed | Token or Session Expired | 
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`13` | Failed | Nomor Telepon Customer Belum di daftarkan oleh merchant |
`99` | General Error | |

## Earning Stamp / Coupon
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
customerPhone | true | string | 20 | nomor telepon customer, ex format `081299465055`
transactionTime | true | string | | format `YYYY-MM-DD HH:mm:ss`
referenceId | true | string | 40 | nomor referensi redemption
amount | true | string | 20 | Ex `20000`

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/transaction/v1/customer/earn" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Request body:

```json
{
    "transactionTime": "2021-03-15 15:27:00",
    "customerPhone": "081345679912",
    "referenceId": "0356f2b2-261b-4d98-9191-1c63bd9da9dd",
    "amount": "100000"
}
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": null
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | |
`12` | Failed | Token or Session Expired | 
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`13` | Failed | Nomor Telepon Customer Belum di daftarkan oleh merchant |
`99` | General Error | |

## Check Eligible Redemption
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Query Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
customerPhone | true | string | 20 | nomor telepon merchant, ex format `081299465055`

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/transaction/v1/customer/redemption/eligible?customerPhone=081299465055" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "balance": "0"
    }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | |
`12` | Failed | Token or Session Expired | 
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`13` | Failed | Nomor Telepon Customer Belum di daftarkan oleh merchant |
`99` | General Error | |
## Redemption
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
inquiryId | true | string | 40 | di dapat pada response `Check Eligible`
transactionTime | true | string | | format `YYYY-MM-DD HH:mm:ss`
customerPhone | true | string | 20 | nomor telepon merchant, ex format `081299465055`
referenceId | true | string | 40 | nomor referensi redemption

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/transaction/v1/customer/redemption" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Request body:

```json
{
    "inquiryId": "d57b725f-1963-44b5-8c75-b880c0338ffb",
    "transactionTime": "2021-03-10 15:24:00",
    "customerPhone": "081345679912",
    "referenceId": "ca0f73b6-4ff9-462a-855b-facc1fa349e0",
}
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "balance": "0"
    }
}
```
### Response

**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | |
`12` | Failed | Token or Session Expired | 
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`13` | Failed | Nomor Telepon Customer Belum di daftarkan oleh merchant |
`99` | General Error | |
## History
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Query Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
customerPhone | true | string | 20 | Ex `081299465055`
currentPage | true | string | |
recordPerPage | true | string | |

> Example Request:

```shell
curl "hhttps://apidev.ottopoint.id/ottostamp/transaction/v1/merchant/history/list" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "histories": [
            {
                "id": "197d825f-892d-4daf-b975-4528da7ea1d6",
                "transactionId": "a0d89fa9-475c-4373-8696-2d4b6556bca3",
                "comment": "Promo Deleted",
                "value": "10",
                "referenceId": "f69717a4-f784-4902-8b04-7d5b4e46fbb4",
                "code": "RM01",
                "type": "Promo Deleted",
                "transactionTime": "2021-03-12T04:23:40.18845Z"
            },
            {
                "id": "fec8c5ff-f0c0-4a05-bf89-bcd443e7481f",
                "transactionId": "150dcf64-c9e3-4739-9a61-0fb17e009308",
                "comment": "Earning Stamp",
                "value": "10",
                "referenceId": "0b80d9a6-0b9d-4294-8f8a-8fb5af21efdb",
                "code": "AD01",
                "type": "Adding",
                "transactionTime": "2021-03-10T18:55:00Z"
            },
            {
                "id": "ed2c85ce-3f7f-452e-af1b-63e6f9c68965",
                "transactionId": "a3c75923-26ee-4a90-8eda-592e79b327d0",
                "comment": "Spending Stamp",
                "value": "5",
                "referenceId": "ca0f73b6-4ff9-462a-855b-facc1fa349e0",
                "code": "SP01",
                "type": "Spending",
                "transactionTime": "2021-03-10T15:24:00Z"
            },
            {
                "id": "cfa9d8b8-c9fa-4dc1-ae19-86cc8e6c6a74",
                "transactionId": "a031c18c-2a5d-45fa-b1fe-21dcdbcac22f",
                "comment": "Spending Stamp",
                "value": "5",
                "referenceId": "cf8b6598-fae0-41f8-b3a0-b3ddfc85d285",
                "code": "SP01",
                "type": "Spending",
                "transactionTime": "2021-03-10T15:12:00Z"
            },
            {
                "id": "bf644ca4-60d2-4141-a759-47e285b342f9",
                "transactionId": "d61a0574-f9e6-440c-a36c-d0e7fd2103a6",
                "comment": "Earning Stamp",
                "value": "10",
                "referenceId": "2cf880a6-4e21-4619-a54f-20cdd1eb033a",
                "code": "AD01",
                "type": "Adding",
                "transactionTime": "2021-03-10T15:01:00Z"
            }
        ],
        "totalRecord": 5,
        "recordPerPage": 5,
        "currentPage": 1,
        "totalPage": 1
    }
}
```
### Response

**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | |
`12` | Failed | Token or Session Expired | 
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`13` | Failed | Nomor Telepon Customer Belum di daftarkan oleh merchant |
`99` | General Error | |

# Promo
## Add Promo
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
name | true | string | 100 | nama promo
minTrx | true | int | |
earnStartDate | true | string | | tanggal mulai earning, format `YYYY-MM-DD`
earnEndDate | true | string | | tanggal berakhir earning, format `YYYY-MM-DD`
earnStamp | true | numeric | | stamp yang akan di dapatkan customer jika transaksi >= minTrx
promoStartDate | true | string | | tanggal mulai promo, format `YYYY-MM-DD`
promoEndDate | true | string | | tanggal berakhir promo, format `YYYY-MM-DD`
promoType | true | string | | jenis promo, Ex `NT` (untuk saat ini, hanya ada jenis promo `nominal transaksi`)
promoNominal | true | numeric | | potongan harga yang di dapatkan customer ketika ingin melakukan redemption stamp mereka, ex.`4`
redeemStamp | true | numeric | | stamp yang di butuhkan untuk mendapatkan `promoNominal`


> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/promo/add" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Request body:

```json
{
    "name": "Promo Hari Ini",
    "minTrx": 10000,
    "earnStartDate": "2021-03-10",
    "earnEndDate": "2021-03-30",
    "earnStamp": 10,
    "promoStartDate": "2021-03-10",
    "promoEndDate": "2021-03-30",
    "promoType": "NT",
    "redeemStamp": 5,
    "promoNominal": 20000
}
```

> Example response:

```json
{
  "responseCode": "00",
  "responseDesc": "Success",
  "data": null
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Customer berhasil di tambahkan pada merchant / Nomor Customer sudah terdaftar pada sistem dan di link pada merchant |
`03` | Failed | Nomor Telepon Customer Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`22` | Bad Request | Invalid promo start date or end date
`99` | General Error | |

## Detail Promo
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | false | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/promo/detail" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Example response:

```json
{
    "responseCode": "00",
    "responseDesc": "Success",
    "data": {
        "id": "a2bb4a8d-d04e-4d87-be03-f80f972d9738",
        "name": "Promo Hari Ini",
        "promoType": "NT",
        "minTrx": 10000,
        "earnStamp": 10,
        "earnStartDate": "2021-03-14T00:00:00Z",
        "earnEndDate": "2021-03-30T00:00:00Z",
        "promoStartDate": "2021-03-14T00:00:00Z",
        "promoEndDate": "2021-03-30T00:00:00Z",
        "redeemStamp": 5,
        "promoNominal": 20000,
        "status": "Active",
        "createdAt": "2021-03-13T22:07:39.183317Z",
        "createdBy": "System",
        "updatedAt": "2021-03-13T22:07:39.183317Z",
        "updatedBy": "System"
    }
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Customer berhasil di tambahkan pada merchant / Nomor Customer sudah terdaftar pada sistem dan di link pada merchant |
`03` | Failed | Nomor Telepon Customer Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`22` | Bad Request | Invalid promo start date or end date
`99` | General Error | |

## Update Promo
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
earnStartDate | true | string | tanggal earning, format `YYYY-MM-DD`
earnEndDate | true | string | tanggal earning, format `YYYY-MM-DD`
promoStartDate | true | string | tanggal earning, format `YYYY-MM-DD`
promoEndDate | true | string | tanggal earning, format `YYYY-MM-DD`


> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/promo/edit" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Request body:

```json
{
    "earnStartDate": "2021-03-10",
    "earnEndDate": "2021-03-30",
    "promoStartDate": "2021-03-10",
    "promoEndDate": "2021-03-30",
}
```

> Example response:

```json
{
  "responseCode": "00",
  "responseDesc": "Success",
  "data": null
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Customer berhasil di tambahkan pada merchant / Nomor Customer sudah terdaftar pada sistem dan di link pada merchant |
`03` | Failed | Nomor Telepon Customer Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`22` | Bad Request | Invalid promo start date or end date
`99` | General Error | |

## Delete Promo
### HTTP Headers

**Property** | **Required** | **Type** | **Description**
--------- | -------- | --------- | -----------
InstitutionId | true | string | institutionId / insitution code yang di berikan
AppsId | true | string | ex. `appsId`
DeviceId | true | string | ex. `869552045462447`
ChannelId | true | string | ex. `H2H`
Geolocation | true | string | ex. `-6.231340755705505, 106.84526008386358`
Timestamp | true | string | ex. `1579666534`
Signature | true | string | ex. `60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91`
Authorizatoin | true | string | ex. `MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY=`

### Body Parameter

**Property** | **Required** | **Type** | **Size** | **Description**
--------- | -------- | --------- | ----------- | -----------
id | true | string | | id promo yang di dapat response `Detail Promo`


> Example Request:

```shell
curl "https://apidev.ottopoint.id/ottostamp/merchant/v1/promo/delete" \
  -H "Content-Type: application/json" \
  -H "InstitutionId: OS001" \
  -H "AppsId: appsId" \
  -H "DeviceId: 869552045462447" \
  -H "ChannelId: H2H" \
  -H "Geolocation: -6.231340755705505, 106.84526008386358" \
  -H "Timestamp: 1579666534" \
  -H "Signature: 60874cef2259f961b84faced046bd968f5cdd583ed1fbe41cdb7306d1386a473ac98ef167f0f19e7e7b57937fb1a54e3b161873628ef483bb9d75d50f625ae91" \
  -H "Authorization: MDgxMzQ2OTk4OXVKRUR6Z2RtOVpsUXEyNDY="
```

> Request body:

```json
{
    "earnStartDate": "2021-03-10",
    "earnEndDate": "2021-03-30",
    "promoStartDate": "2021-03-10",
    "promoEndDate": "2021-03-30",
}
```

> Example response:

```json
{
  "responseCode": "00",
  "responseDesc": "Success",
  "data": null
}
```
### Response
**Response Code** | **Meaning** | **Description** | **Definition**
--------- | -------- | --------- | -----------
`00` | Success | Customer berhasil di tambahkan pada merchant / Nomor Customer sudah terdaftar pada sistem dan di link pada merchant |
`03` | Failed | Nomor Telepon Customer Belum Terdaftar |
`06` | Bad Request | Signature tidak tepat |
`08` | Bad Request | Terdapat kesalahan pada data yang di kirim | 
`12` | Failed | Token or Session Expired | 
`22` | Bad Request | Invalid promo start date or end date
`99` | General Error | |
