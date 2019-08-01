---
title: CyberPolicy Quote API Documentation v1.0

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:

includes:

search: true
---

# Introduction
Welcome to the CyberPolicy Quote API. Third-party partners can use this API to get Cyber insurance quotes from CyberPolicy. If you have any questions regarding the documentation, please reach out to your partner contact or product@coverhound.com

<!-- # Quote Eligibility

This API will only provide quotes for businesses:

 - Under $5M in annual revenue
 - Less than 20 employees
 - All states except KY and FL
 - No prior claims -->

# Authentication

> To authorize, pass the correct headers with each request:

```shell
curl "https://cyberpolicy.com/api/v1/quote_request"
  -H "Authorization: test_api_key"
  -H "Content-Type: application/json"
```

> Make sure to replace `test_api_key` with your API key.

CyberPolicy uses API keys to allow access to the Quote API. Please contact us for an API key.

CyberPolicy expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: test_api_key`

# Request

The Quote API endpoint posts an insurance application to our Quote API. The request body must contain a valid insurance application, as detailed below.

**HTTP Request**

`POST https://cyberpolicy.com/api/v1/quote_request`

## Request Parameters

> Request Example:

```json
{
  "cyber_insurance_request": {
    "partner": {
      "partner_name": "Sample Business",
      "partner_phone": "8888888888",
      "partner_email": "sample@email.com",
      "partner_url": "sample.com"
    },
    "shopper": {
      "shopper_name": "Ari Smit",
      "business_name": "Ari Safari, Inc",
      "email": "ari@safari.com",
      "phone": "888888888",
      "street1": "1 California St",
      "street2": "Suie 1100",
      "city": "San Francisco",
      "state": "CA",
      "zip_code": "94111"
    },
    "underwriting_questions" {
      "business_type_id": "425",
      "projected_revenue": "0-400k",
      "num_employees": "14",
      "compliances": {"PCI/DCI","HIPAA/HITECH"},
      "chip_pos": true,
      "hipaa_hitech": true,
      "encrypts_sensitive_data_on": {"computers","mobile devices","networks"},
      "has_backups": true,
      "backup_kinds": {"On-site","Offsite-pysical","cloud"},
      "written_info_sec_plan": true,
      "has_antivirus":  true,
      "has_firewalls": true,
      "num_sensitive_customer_records": "2000",
      "in_house_it": true,
      "third_party_scans": true,
      "action_brought_against": false,
      "cyber_attack_experiences":  false
    }
  }
```


Parameter | Description
----- | ----- | ------
shopper_name | Contact Name
business_name | Business Name
email | Email
phone | Phone
street1 | Street Address
street2 | Suite/Floor
city | City
state | State
zip_code | Zip Code
business_type_id | Type of Business
projected_revenue | Projected Revenue (Next 12 Months)
num_employees | Number of Employees
compliances | Are you subject to: <ul><li>PCI/DCI Compliance</li> <li>HIPAA/HITECH Compliance</li><li>None</li></ul>
chip_pos | Do you use only chip-enabled card readers or are you certified as PCI/DSS compliant?
hipaa_hitech | Are you compliant with applicable HIPAA and HITECH Act requirements?
encrypts_sensitive_data_on | Do you encrypt sensitive data on: <ul><li>Office Computers</li><li>Mobile Devices (laptops, cell phones, flash drives, tablets, etc.)</li><li>Networks</li><li>None</li></ul>
has_backups | Do you backup critical business systems, data, and Personally Identifiable Information (PII) at least once a week?
backup_kinds | Where do you back it up? (select all that apply) <ul><li>On-site</li><li>Offsite (physical storage)</li><li>Offsite (the Cloud)</li></ul>
written_info_sec_plan | Do you have a Written Information Security Plan that includes procedures on how to handle and correct sensitive data?
has_antivirus | Do you have antivirus in place? (updated at least monthly)
has_firewalls | Do you have firewalls in place? (updated at least monthly)
num_sensitive_customer_records | For how many customers, employees, and vendors do you (or a vendor on your behalf) store, transmit or have access to at least one of the following pieces of sensitive information? <ul><li>Personal Information (Name, DOB, Address, etc.)</li> <li>Credit/Debit Cards</li><li>Financial/Banking</li><li>Medical (PHI)</li><li>Social Security or National Id</li><li>Other Sensitive Data</li></ul>
in_house_it | Do you have an in-house IT person that implements and manages a computer security program for the company network?
third_party_scans | Do you have 3rd party perform vulnerability scans or penetration testing done on your computer network at least once a year?
action_brought_against | Has any regulatory, governmental or administrative action been brought against you due to your handling of sensitive data?
cyber_attack_experiences | Within the last 5 years, have you experienced or have reason to suspect any of the following (or something similar to them): <ul><li>System intrusions</li> <li>Tampering</li><li>Virus or malicious code attacks</li><li>Loss of data</li><li>Loss of portable media</li><li>Hacking incident</li><li>Extortion attempt</li><li>Data theft</li><li>Copyright or trademark dispute</li></ul>

## Request Schema

### Requester Information
```json
"partner": {
      "partner_name": "Sample Business",
      "partner_phone": "8888888888",
      "partner_email": "sample@email.com",
      "partner_url": "sample.com"
    }
```

Parameter | Type | Required
--------- | ------- | -----------
partner_name | STRING | Required
partner_phone | STRING | Required
partner_email | STRING | Required
partner_url | STRING | Required



### Shopper Profile

```json
"shopper": {
      "shopper_name": "Ari Smith",
      "business_name": "Ari Safari, Inc",
      "email": "ari@safari.com",
      "phone": "888888888",
      "street1": "1 California St",
      "street2": "Suite 1100",
      "city": "San Francisco",
      "state": "CA",
      "zip_code": "94111"
    }
```

Parameter | Type | Required
--------- | ------- | -----------
shopper_name | STRING | Required
business_name | STRING | Required
email | STRING | Required
phone | STRING | Required
street1 | STRING | Required
street2 | STRING | Required
city | STRING | Required
state | STRING | Required
zip_code | STRING | Required


### Underwriting Questions

```json
"underwriting_questions" {
      "business_type_id": "425",
      "projected_revenue": "0-400k",
      "num_employees": "14",
      "compliances": {"PCI/DCI","HIPAA/HITECH"},
      "chip_pos": true,
      "hipaa_hitech": true,
      "encrypts_sensitive_data_on": {"computers","mobile devices","networks"},
      "has_backups": true,
      "backup_kinds": {"On-site","Offsite-pysical","cloud"},
      "written_info_sec_plan": true,
      "has_antivirus":  true,
      "has_firewalls": true,
      "num_sensitive_customer_records": "2000",
      "in_house_it": true,
      "third_party_scans": true,
      "action_brought_against": false,
      "cyber_attack_experiences":  false
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

<!-- Note from Ari: <aside class="warning">Need to determine how to handle the compliance question.</aside> -->


# Response

>A successful quote will return the following:

```json
{
  "quote": {
    "uuid": "348a0dcb-3fca-4031-bb8d-c8d06f3a1e24",
    "success": true,
    "amount": 1000,
    "outcome": "quote",
    "outcome_details": null
  }
}
```

>A declined quote will return the following:

```json
  "quote": {
    "uuid": "348a0dcb-3fca-4031-bb8d-c8d06f3a1e24",
    "success": false,
    "amount": null,
    "outcome": "decline",
    "outcome_details": "The applicant is above the revenue threshold."
  }
}
```


Parameter | Description
--------- | -----------
uuid | The unique identifier of the quote
success | Whether or not we were able to return a quote
amount | The price in dollars, including all taxes and fees
outcome | possible values: `quote`, `decline`, `referral`
outcome_details | the reason for a `decline` or `referral` outcome

