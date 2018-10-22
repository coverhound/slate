---
title: TrustedPlace™ Quoting API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL

search: true
---

# Onboarding

If you'd like to partner with TrustedPlace™, please contact the team at <mailto:support@trustedplace.com>.

```shell
% export TRUSTED_PLACE_API_KEY=th1s-1s-y0ur-publ1c-k3y
% export TRUSTED_PLACE_API_SECRET=1nt3rn4ls3cr3t
```

Your TrustedPlace™ contact will generate your public API key and private API secret. The public key can be shared amongst team members, but the secret should be concealed through your application's configuration management.

Should you need to invalidate your API secret, please contact TrustedPlace™ support.

<aside class="notice">
You will need a separate key/secret pair for developing with a testing environment.
</aside>

# Tokens

## Create a token

> Create a token with:

```shell
% curl -i https://trustedplace.com/api/v1/tokens \
       -X POST \
       -H "X-API-Key: $TRUSTED_PLACE_API_KEY" \
       -H "X-API-Secret: $TRUSTED_PLACE_API_SECRET"
```

> A successful request will return token information in JSON format, like:

```json
{
  "token": {
    "value": "t0k3nv4lu3",
    "type": "Bearer"
  }
}
```

> This information should be stored in some way:

```shell
% export TRUSTED_PLACE_TOKEN=t0k3nv4lu3
% export TRUSTED_PLACE_TOKEN_TYPE=Bearer
```

`POST /api/v1/tokens`

After receiving a public API key (`<public key>`) and private API secret (`<private secret>`),
the client may create an access token by issuing a `POST` request to
the `/api/v1/tokens` endpoint.

The client may create and maintain multiple access tokens at the same time.

### Request

HTTP&nbsp;Header | Required | Values
----------------------------- | -------- | ------
X-API-Key | Always | `<public key>`
X-API-Secret | Always | `<private secret>`

### Response

Status&nbsp;Code | Notes
---------------- | -----
201 Created | Success
401 Unauthorized | Failure: See `X-Error-Detail`

HTTP&nbsp;Header | Notes
------------------------------ | ------
X-Error-Detail | Use this information to understand why the request failed.
X-Request-Id | Provide this unique identifier for your request when contacting support.

## Invalidate a token
`DELETE /api/v1/tokens`

The client may invalidate a single token per request at anytime. Information
about requests associated with an invalidated token will remain for record-keeping and support purposes.

> Invalidate a token with:

```shell
% curl -i "https://trustedplace.com/api/v1/tokens/$TRUSTED_PLACE_TOKEN" \
       -X DELETE \
       -H "X-API-Key: $TRUSTED_PLACE_API_KEY" \
       -H "X-API-Secret: $TRUSTED_PLACE_API_SECRET"
```

### Request

HTTP&nbsp;Header | Required | Values
---------------- | -------- | ------
X-API-Key | Always | `<public key>`
X-API-Secret | Always | `<private secret>`

### Response

Status&nbsp;Code | Notes
---------------- | -----
200 OK | Success
401 Unauthorized | Failure: See `X-Error-Detail`

HTTP&nbsp;Header | Notes
------------------------------ | ------
X-Error-Detail | Use this information to understand why the request failed.
X-Request-Id | Provide this unique identifier for your request when contacting support.

# Quotes

## Create quotes

> Build your JSON request body (stored as `data.json` in this example):

```json
{
  "property": {
    "residenceType": "homeowner",
    "occupancy": "primary",
    "propertyType": "single_family",
    "squareFootageCategory": "large"
  },
  "address": {
  	"addressLine1": "One State St.",
    "addressLine2": "",
    "city": "Hartford",
    "stateStr": "CT",
    "zipCode": "06102",
    "country": "US"
  },
  "billingAddress": {
  	"addressLine1": "One California St.",
    "addressLine2": "Suite 1100",
    "city": "San Francisco",
    "stateStr": "CA",
    "zipCode": "94111",
    "country": "US"
  },
  "personalInformation" : {
    "firstName": "John",
    "middleName": "",
    "lastName": "Smith",
    "email": "john.smith@example.com",
    "phone": "4158675309",
    "isCellPhone": true
  },
  "marketingInformation" : {
    "campaign" : "TestCampaign",
    "pub" : "AgentSmith123",
    "treaty" : "a40496b7-8353-44b2-936d-10c770c38ad7"
  }
}
```

> `POST` your request to the quoting endpoint:

```shell
% curl -i https://trustedplace.com/api/v1/quotes \
       -X POST \
       -d @data.json \
       -H "Content-Type: application/json" \
       -H "X-API-Key: $TRUSTED_PLACE_API_KEY" \
       -H "Authorization: $TRUSTED_PLACE_TOKEN_TYPE $TRUSTED_PLACE_TOKEN"
```

`POST /api/v1/quotes`

This endpoint creates quotes any API client can surface to its users. TrustedPlace™ also stores and surfaces shopper information (in the form of a session cookie) that an API client can use to update shopper information and re-quote.

### Request Headers

HTTP&nbsp;Header | Required | Values
---------------- | -------- | ------
X-API-Key | Always | `<public key>`
Authorization | Always | `Bearer <token value>`
Content-Type | Always | `application/json`

### Request

Key | Possible Values | Required | Notes
----| --------------- | -------- | -----
*property* | | | *Top-level*
residenceType | `"homeowner"`<br>`"renter"` | Always |
occupancy | `"primary"`<br>`"investment"` | If&nbsp;`"homeowner"` | `"primary"` encompasses secondary residences as well
propertyType | `"single_family"`<br>`"condo"`<br>`"apartment"`<br>`"townhouse"`<br>`"multi_family"`<br>`"mobile_home"`<br>`"recreational_vehicle"`<br>`"other"` | If&nbsp;`"homeowner"` |
squareFootageCategory | `"small"`<br>`"medium"`<br>`"large"`<br>`"extra_large"` | If&nbsp;`"homeowner"` | small: 0ft²- 2,000ft²<br>medium: 2,001ft² - 4,000ft²<br>large: 4,001ft² - 10,000ft²<br>extra_large: 10,000ft²+
*address (billingAddress)* | | | *Top-level*
addressLine1 | string | Always |
addressLine2 | string | Optional |
city | string | Always |
stateStr | string | Always |
zipCode | string | Always |
country | string | Always |
*personalInformation* | | | *Top-level*
firstName | string | Always |
middleName | string | Optional |
lastName | string | Always |
email | string | Always | Used for both contact and documentation
phone | string | Always | Only 10-character strings of digits
isCellPhone | `true`<br>`false` | Always |
*marketingInformation* | | *Optional* | *Top-level*
campaign | string | Optional | Marketing campaign identifier
pub | string | Optional | External agent identifier
treaty | string | Optional | Alternate treaty identifier (overrides established treaty)

<aside class="warning">
If <code>billingAddress</code> is not provided in an initial request, the API defaults <code>billingAddress</code> values to those provided via <code>address</code>. Any subsequent requests for the same shopper will override both addresses.
</aside>

<aside class="notice">
This endpoint accepts JSON with either camelCased or snake_cased keys.
</aside>

### Response

> Example success response:

```json
{
  "data": {
    "apiVersion": "1.0.0",
    "shopper": {
      "uuid": "ad89b4f8-c905-4f2e-b60c-8f02b7814033",
      "eligible": true,
      "ineligibilityReasons": [],
      "partner": {
         "uuid": "3a46da69-c60b-485b-8dbc-d1a65384e0b8",
         "displayName": "Test Partner",
         "discountDetails": "This is a test description of the promotion",
         "promoMessage": "Get 100% off!"
      }
    },
    "quotes": [
      {
        "deductible": "500.0",
        "expirationDate": "2019-01-15",
        "limit": "25000.0",
        "resumeUrl": "https://trustedplace.com/resume/91869baf-538d-4ad5-b04d-a568f46c050f",
        "coverageOption": {
          "kind": "bundle",
          "residenceType": "homeowner",
          "coveragePlan": "Bundled Coverage"
         },
         "paymentOptions": [
           {
             "kind": "annual",
             "grossPremium": "180.37",
             "applicableTax": "11.45",
             "purchasePrice": "191.82"
           },
           {
             "kind": "monthly",
             "grossPremium": "196.79",
             "applicableTax": "12.5",
             "purchasePrice": "209.29"
           }
         ]
      },
      {
        "deductible": "500.0",
        "expirationDate": "2019-01-15",
        "limit": "25000.0",
        "resumeUrl": "https://trustedplace.com/resume/9ab15bcb-5f8a-4440-9f98-854d1482de6f",
        "coverageOption": {
          "kind": "dwelling",
          "residenceType": "homeowner",
          "coveragePlan": "Home Systems Coverage"
        },
        "paymentOptions": [
          {
            "kind": "annual",
            "grossPremium": "137.35",
            "applicableTax": "8.72",
            "purchasePrice": "146.07"
          },
          {
            "kind": "monthly",
            "grossPremium": "149.85",
            "applicableTax": "9.52",
            "purchasePrice": "159.37"
          }
        ]
      },
      {
        "deductible": "500.0",
        "expirationDate": "2019-01-15",
        "limit": "25000.0",
        "resumeUrl": "https://trustedplace.com/resume/a5d6aa01-6b06-423b-8174-858196ee9586",
        "coverageOption": {
          "kind": "hocontents",
          "residenceType": "homeowner",
          "coveragePlan": "Appliances and Electronics Coverage"
        },
        "paymentOptions": [
          {
            "kind": "annual",
            "grossPremium": "62.78",
            "applicableTax": "3.99",
            "purchasePrice": "66.77"
          },
          {
            "kind": "monthly",
            "grossPremium": "68.49",
            "applicableTax": "4.35",
            "purchasePrice": "72.84"
          }
        ]
      }
    ]
  }
}
```

### Shopper Information

Key | Values | Description
--- | ------ | -----------
uuid | string | Share this unique identifier for the shopper when contacting support.
eligible | boolean | Whether the shopper is eligible to receive quotes
ineligibilityReasons | array of strings | Provides explanations for shopper ineligibility. Empty when shopper is eligible

### Partner Information

Key | Values | Description
--- | ------ | -----------
uuid | string | Unique identifier
displayName | string | Public partnership name
discountDetails | string or `null` | Explanation for ratings discount based on associated partner and promotion
promoMessage | string or `null` | Headline marketing message explaining promotion

### Quote(s) Information

TrustedPlace™ surfaces an array of JSON objects, each representing a single quote. Ineligible requests will surface an empty array, and details about ineligibility can be found in shopper information. Property information determines the number of quotes TrustedPlace™ generates for a shopper.

Key | Values | Description
--- | ------ | -----------
deductible | string | Parses into $USD
expirationDate | string | Date on which quoted premiums expire. Must re-quote for this shopper on and after date
limit | string | Parses into $USD
resumeUrl | string (URL) | TrustedPlace™ landing page on which shoppers can continue to purchase a contract from a given quote

### Coverage Option Informatin

Key | Values | Description
--- | ------ | -----------
kind | `"bundle"`<br>`"dwelling"`<br>`"contents"`<br>`"hocontents"` | Identifies different coverage plans
residenceType | `"homeowner"`<br>`"renter"` | Corresponds with request's `property.residenceType`
coveragePlan | string | Concise description of coverage


### Payment Option Information

TrustedPlace™ provides two billing cycle options: annual and monthly payment plans. Annual plans require a single upfront payment for a year's worth of coverage, and the total charge is often discounted. Monthly plans comprise twelve installments over the lifecycle of the contract. For accounting purposes, the charge for the first installment is often slightly greater than for the remaining eleven.

The developer can decide how to market the price differences surfaced through the API.

Key | Values | Description
--- | ------ | -----------
kind | `"annual"`<br>`"monthly"` | Billing cycle for given payment option
grossPremium | string | Parses into $USD (cost before taxes)
applicableTax | string | Parses into $USD
purchasePrice | string | Parses into $USD (total charge to the shopper over billing cycle)

## Invalid Request Examples

> Invalid `"property.residenceType"` value:

```js
{
  // ...
  "property": {
    "residenceType": "homeowne",
  // ...
  }
}
```
> Response with error detail:

```json
{
  "data": {
    "messages": {
      "structure": {
        "residenceType": [
          "is not included in the list"
        ]
      }
    },
    "status": 422,
    "shopper": {
      "uuid": "ad89b4f8-c905-4f2e-b60c-8f02b7814033",
    }
  }
}
```

> Missing `"address"` information

```js
{
  // ...
  "address": {}
  // ...
}
```

> Response with error detail:

```json
{
  "data": {
    "messages": {
      "structure": {
        "zipCode": [
          "can't be blank"
        ]
      }
    },
    "status": 422,
    "shopper": {
      "uuid": "58b68567-c71e-4544-82ab-a91de9401ad2"
    }
  }
}
```

> Invalid `"phone"` and `"email"` formats:

```js
{
  // ...
    "email" : "john.smith@example",
    "phone" : "123",
  // ...
}
```

> Response with error detail:

```json
{
  "data": {
    "messages": {
      "personalInformation": {
        "phone": [
          "Phone number is invalid: Enter a valid phone number"
        ],
        "email": [
          "address is not properly formatted: Enter a valid email address"
        ]
      }
    },
    "status": 422,
    "shopper": {
      "uuid": "58b68567-c71e-4544-82ab-a91de9401ad2"
    }
  }
}
```

TrustedPlace™ provides failure explanations for syntactically valid JSON requests that fail validation. See code examples for schema and more detail.
