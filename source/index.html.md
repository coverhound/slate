---
title: Leads API

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:

includes:

search: false
---

# Introduction

Welcome to CoverHound's Commercial Leads API. Third-party partners can use our API to send individual leads to CoverHound digitally. Version 1.0 of the Leads API is limited to creating accounts in our proprietary agency tool and collecting further details on the business lead (e.g., payroll, revenue). If you have any questions regarding the documentation, please reach out to your partner contact or commercial-product@coverhound.com

# Authentication

> To authorize, use this code:

```shell
# Pass the correct headers with each request
curl "https://coverhound.com/business-insurance/partners/api/v1/leads"
  -H "Authorization: test_api_key"
  -H "Content-Type: application/json"
```

The Leads API uses API keys to allow access to the API. All authorized partners will be provided an API Key by CoverHound.

The Leads API expects the API key to be included in all API requests to the server in a header:

`Authorization: test_api_key`

<aside class="notice">
You must replace <code>test_api_key</code> with your personal API key.
</aside>

# Leads

## Send New Leads

> Request example:

```shell
POST https://coverhound.com/business-insurance/partners/api/v1/leads
Content-Type: application/json
Authorization: test_api_key
```

```json
{
  "shopper": {
    "cbt_code": "CBT_123",
    "state": "CA",
    "first_name": "Jane",
    "last_name": "Smith",
    "fein": "123456789",
    "telephone": "1234567890",
    "extension": "123",
    "email": "jane@api_test.com",
    "business_name": "Jane Smith Corporation",
    "product_codes": ["workers_compensation", "bop"]
  },
  "lead_data": {
    "CH_020": "1 California Street",
    "CH_251": "This is a better description of what our business does."
  }
}
```

This endpoint posts new lead data to our API. The API allows for one lead per request.

### HTTP Request

`POST https://coverhound.com/business-insurance/partners/api/v1/leads`

### Query Parameters

Parameter | Description
--------- | -----------
shopper | Stores all shopper related data.
cbt_code (optional) | Class of Business code. Should match a CoverHound CBT code.
state | State for lead. Accepts both state name and abbreviation, i.e. "California" or "CA".
first_name | First name of lead.
last_name | Last name of lead.
fein (optional) | 9 digit FEIN of lead.
telephone | Telephone of lead. Accepts set of 10 digits without parentheses or dashes.
extension | Extension of lead. Accepts a maximum of 10 digits.
email (optional) | Email of lead.
business_name | Business name of lead.
product_codes | Array of CoverHound product codes.
lead_data | Store all lead related data. Each key/value pair inside lead_data should match a CoverHound question code and corresponding answer.
