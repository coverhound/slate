---
title: CyberPolicy Quote API Documentation v1.0

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
   - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction


<aside class="warning">Can we get rid of the options to view "shell", "python", and "javascript"? ---> </aside>

Welcome to the CyberPolicy Quote API!

This documentation outlines how to request a cyber insurance quote.

Please don't hesitate to reach out if you have any further questions.

Ari Vared

ari@cyberpolicy.com

# Quote Eligibility

This API will only provide quotes for businesses:

 - Under $5M in annual revenue
 - Less than 20 employees
 - All states except KY and FL
 - No prior claims 

<aside class="warning">Is this information helpful for the documentation?</aside>

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

CyberPolicy uses API keys to allow access to the API. Please contact us for a new API key.

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="warning">I have NO IDEA what this should be. Please help :-)</aside>

# Quote Request

What does this request do

**Request**

`https://insert_request_url.com`

**Requirements**

 - request type: POST
 - request header with a developer key
 - request parameter with the name of the applicaiton form
 - request body must contain a valid set of answers representing the questions our insurance application asks

 <aside class="warning">I made this up/copied from somewhere else. Please review and change.</aside>

## Sample Request

> Here is a sample request:

```ruby

{
  "cyber_insurance_request": {
    "partner": {
      "partner_name": "Sample Business"
      "partner_phone": "8888888888"
      "partner_email": "sample@email.com"
      "partner_url": "sample.com"
    }
    "shopper": {
      "shopper_name": "Ari Vared"
      "business_name": "Ari Safari, Inc"
      "email": "ari@safari.com"
      "phone": "888888888"
      "street1": "1 California St"
      "street2": "Suie 1100"
      "city": "San Francisco"
      "state": "CA"
      "zip_code": "94111"
    }
    "underwriting_questions" {
      "business_type_id": "425"
      "projected_revenue": "0-400k"
      "num_employees": "14"
      "compliances": {"PCI/DCI","HIPAA/HITECH"}
      "chip_pos": TRUE
      "hipaa_hitech": TRUE
      "encrypts_sensitive_data_on": {"computers","mobile devices","networks"}
      "has_backups": TRUE
      "backup_kinds": {"On-site","Offsite-pysical","cloud"}
      "written_info_sec_plan": TRUE
      "has_antivirus":  TRUE
      "has_firewalls": TRUE
      "num_sensitive_customer_records": "2000"
      "in_house_it": TRUE
      "third_party_scans": TRUE
      "action_brought_against": FALSE
      "cyber_attack_experiences":  FALSE
    }
    }
  }

```


parameter name | internal code | question text | notes
----- | ----- | ------ | ------- | ------
shopper_name | cp_020 | Contact Name | We need to add the "shopper" descriptor to this
business_name | cp_024 | Business Name | We need to add the "bussiness" descriptor to this
email | cp_023 | Email
phone | cp_022 | Phone
street1 | cp_025 | Street Address
street2 | cp_027 | Suite/Floor
city | cp_026 | City
state | cp_034 | State
zip_code | cp_003 | Zip Code
business_type_id | cp_001 | Type of Business | We will ask for them to send the `business_types_id`
projected_revenue | cp_021 | Projected Revenue (Next 12 Months)
num_employees | cp_002 | Number of Employees 
compliances | cp_037 | Are you subject to: <ul><li>PCI/DCI Compliance</li> <li>HIPAA/HITECH Compliance</li><li>None</li></lu>
chip_pos | cp_032 | Do you use only chip-enabled card readers or are you certified as PCI/DSS compliant?
hipaa_hitech | cp_060 | Are you compliant with applicable HIPAA and HITECH Act requirements?
encrypts_sensitive_data_on | cp_048 | Do you encrypt sensitive data on: <ul><li>Office Computers</li><li>Mobile Devices (laptops, cell phones, flash drives, tablets, etc.)</li><li>Networks</li><li>None</li></lu>
has_backups | cp_049 | Do you backup critical business systems, data, and Personally Identifiable Information (PII) at least once a week?
backup_kinds | cp_050 | Where do you back it up? (select all that apply) <ul><li>On-site</li><li>Offsite (physical storage)</li><li>Offsite (the Cloud)</li></lu> 
written_info_sec_plan | cp_051 | Do you have a Written Information Security Plan that includes procedures on how to handle and correct sensitive data?
has_antivirus | cp_040 | Do you have antivirus in place? (updated at least monthly)
has_firewalls | cp_042 | Do you have firewalls in place? (updated at least monthly)
num_sensitive_customer_records | cp_006 | For how many customers, employees, and vendors do you (or a vendor on your behalf) store, transmit or have access to at least one of the following pieces of sensitive information? <ul><li>Personal Information (Name, DOB, Address, etc.)</li> <li>Credit/Debit Cards</li><li>Financial/Banking</li><li>Medical (PHI)</li><li>Social Security or National Id</li><li>Other Sensitive Data</li></ul>
in_house_it | cp_036 | Do you have an in-house IT person that implements and manages a computer security program for the company network?
third_party_scans | cp_043 | Do you have 3rd party perform vulnerability scans or penetration testing done on your computer network at least once a year?
action_brought_against | cp_054 | Has any regulatory, governmental or administrative action been brought against you due to your handling of sensitive data?
cyber_attack_experiences | cp_055 | Within the last 5 years, have you experienced or have reason to suspect any of the following (or something similar to them): <ul><li>System intrusions</li> <li>Tampering</li><li>Virus or malicious code attacks</li><li>Loss of data</li><li>Loss of portable media</li><li>Hacking incident</li><li>Extortion attempt</li><li>Data theft</li><li>Copyright or trademark dispute</li></ul>

<aside class="warning">We told them that we would accept revenue buckets instead of a flat amount. The buckets would be {0-400k, 400k-1m, 1m-2m, 2m-5m, 5m+}</aside>

<aside class="warning">How should we handle encryption check boxes?</aside>

## Request Schema

### Requester Information
```ruby
"partner": {
      "partner_name": "Sample Business"
      "partner_phone": "8888888888"
      "partner_email": "sample@email.com"
      "partner_url": "sample.com"
    }
```

Parameter | Type | Required | Description
--------- | ------- | ----------- | ---------- 
partner_name | STRING | Required
partner_phone | STRING | Required
partner_email | STRING | Required
partner_url | STRING | Required



### Shopper Profile

```ruby
"shopper": {
      "shopper_name": "Ari Vared"
      "business_name": "Ari Safari, Inc"
      "email": "ari@safari.com"
      "phone": "888888888"
      "street1": "1 California St"
      "street2": "Suie 1100"
      "city": "San Francisco"
      "state": "CA"
      "zip_code": "94111"
    }
```

Parameter | Type | Required | Description
--------- | ------- | ----------- | ---------- 
shopper_name | STRING | Required
business_name | STRING | Required
email | STRING | Required
phone | STRING | Required
street1 | STRING | Required
street2 | STRING | Required
city | STRING | Required
state | STATE | Required
zip_code | STRING | Required


### Underwriting Questions

```ruby
"underwriting_questions" {
      "business_type_id": "425"
      "projected_revenue": "0-400k"
      "num_employees": "14"
      "compliances": {"PCI/DCI","HIPAA/HITECH"}
      "chip_pos": TRUE
      "hipaa_hitech": TRUE
      "encrypts_sensitive_data_on": {"computers","mobile devices","networks"}
      "has_backups": TRUE
      "backup_kinds": {"On-site","Offsite-pysical","cloud"}
      "written_info_sec_plan": TRUE
      "has_antivirus":  TRUE
      "has_firewalls": TRUE
      "num_sensitive_customer_records": "2000"
      "in_house_it": TRUE
      "third_party_scans": TRUE
      "action_brought_against": FALSE
      "cyber_attack_experiences":  FALSE
```

Parameter | Type | Required | Description
--------- | ------- | ----------- | --------
business_type_id | STRING | Required | Contact CyberPolicy for business_types and their associated codes
projected_revenue | CURRENCY | Required 
num_employees | INTEGER |  Required | Total full and part-time employee count
compliances | ARRAY | Required | Possible answers are "HIPAA/HITECH" and/or "PCI/DCI" or "None"
chip_pos | BOOLEAN | Optional, required if `compliances`="PCI/DCI"
hipaa_hitech | BOOLEAN | Optional, required if `compliances`="HIPAA/HITECH"
encrypts_sensitive_data_on | ARRAY | Required | Possible answers are "computers" and/or "mobile devices" and/or "networks" or "None"
has_backups | BOOLEAN | Required
backup_kinds | ARRAY | Optional, required if `has_backups`=TRUE | Possible answers are "On-site" and/or "Offsite-physical" or "cloud"
written_info_sec_plan | BOOLEAN | Required
has_antivirus | BOOLEAN | Required
has_firewalls | BOOLEAN | Required
num_sensitive_customer_records | INTEGER | Required
in_house_it | BOOLEAN | Required
third_party_scans | BOOLEAN | Required
action_brought_against | BOOLEAN | Required
cyber_attack_experiences | BOOLEAN | Required

<aside class="warning">Need to determine how to handle the compliances question.</aside>


# Sample Responses



## Success

>A successful quote will return the following:

```ruby
{
  "quote": {
    "uuid": "348a0dcb-3fca-4031-bb8d-c8d06f3a1e24",
    "success": true,
    "amount": 1000,
    "carrier_quote_reference": null,
    "outcome": "quote",
    "outcome_details": null
  }
}
```


Parameter | Description
--------- | -----------
uuid | The UID of the quote
success | 
amount | This is the price in dollars, including all taxes and fees
outcome |
outcome_details |

<aside class="warning">Do we need `carrier_qupte_reference`?</aside>

<aside class="notice">
Outcome Details If the outcome is "decline" or "referral" it will be a string with the reasons, but if the outcome is "quote" it will be null.
</aside>

## Decline

>A declined quote will respond the following:

```ruby
  "quote": {
    "uuid": "348a0dcb-3fca-4031-bb8d-c8d06f3a1e24",
    "success": false,
    "amount": null,
    "carrier_quote_reference": null,
    "outcome": "decline",
    "outcome_details": "A horse insulted my insurance agency."
  }
}
```


## Bad Request



## Internal System Error




# Sample Markup

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete