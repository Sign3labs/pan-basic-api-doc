# pan-basic-api-doc
This document outlines how to integrate with the PAN Details API provided by Sign3. The API accepts a PAN number and returns various details related to the PAN.

---

## Endpoint

```
POST https://you.sign3.in/v1/pan_details
```

---

## Authorization

This API requires **Basic Auth** via the `Authorization` header.

> Replace the encoded string with your own API credentials.

```
Authorization: Basic <Base64(tenantId:tenantSecret)>
```

---

## Request

### Headers

| Header              | Value                                                                 |
|---------------------|-----------------------------------------------------------------------|
| `Content-Type`      | `application/json`                                                    |
| `Authorization`     | `Basic <your_api_token>`                                              |

### Body

| Key           | Value                                                                                                          | Required     |
|--------------|-----------------------------------------------------------------------------------------------------------------|--------------|
| `pan_number` | `PAN Number Length: 10 Pattern:^([a-zA-Z]{3})([abcfghljpteABCFGHLJPTE]{1})([a-zA-Z]{1})([0-9]{4})([a-zA-Z]{1})$`| true         |

```json
{
  "pan_number": "FQYPK****K"
}
```

---

## Example cURL Request

```bash
curl --location 'https://you.sign3.in/v1/pan_details' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic <your_api_token>' \
--data '{
    "pan_number": "FQYPK****K"
}'
```

---

## Successful Response cases (200)

### If provided PAN is valid and we are able to fetch details related to PAN.

```json
{
  "request_id": "93b9226e-171e-4835-9ea5-d44f00f0bfdc",
  "pan_data": {
    "user_exist": true,
    "pan_details": {
      "pan": "FQYPK****K",
      "pan_type": "Individual",
      "fullname": "MANISH KUMAR",
      "first_name": "MANISH",
      "middle_name": "",
      "last_name": "KUMAR",
      "gender": "male",
      "aadhaar_number": "XXXXXXXX7144",
      "aadhaar_linked": true,
      "dob": "01/02/1995",
      "address": {
        "building_name": "H.No- 67",
        "locality": "Khanda Kheri",
        "street_name": "VPO - Khanda Kheri",
        "pincode": "125038",
        "city": "HISAR",
        "state": "Haryana",
        "country": "India"
      },
      "mobile": "",
      "email": ""
    }
  }
}
```

### Response Field Details

| Field                                       | Type      | Description                                       |
|---------------------------------------------|---------- |---------------------------------------------------|
| `request_id`                                | string    | Unique identifier for the API request             |
| `pan_data.user_exist`                       | bool      | Boolean indicating if user details were found     |
| `pan_data.pan_details.pan`                  | string    | PAN of the Individual / Entity                    |
| `pan_data.pan_details.pan_type`             | string    | Type of PAN (Individual / Company / HUF / AOP)    |
| `pan_data.pan_details.fullname`             | string    | Full name of the Applicant / Entity as per government records, including first name, middle name & last name.   |
| `pan_data.pan_details.first_name`           | string    | First name of the applicant as per government records              |
| `pan_data.pan_details.middle_name`          | string    | Middle name of the applicant as per government records             |
| `pan_data.pan_details.last_name`            | string    | Last Name of the applicant as per government records               |
| `pan_data.pan_details.gender`               | string    | Individual’s gender as per PAN (for other PAN types this field would be empty string). Possible Values (male / female / transgender) |
| `pan_data.pan_details.aadhaar_number`       | string    | Aadhaar linked to PAN (masked form - first 8 digits masked and last 4 digits unmasked. Other than Individual it will be empty string.).   |
| `pan_data.pan_details.aadhaar_linked`       | bool      | Boolean indicating Aadhaar linkage                |
| `pan_data.pan_details.dob`                  | string    | Date of birth or Incorporation in the format DD/MM/YYYY                   |
| `pan_data.pan_details.mobile`               | string    | Mobile number associated with PAN from the government source                |
| `pan_data.pan_details.email`                | string    | Email ID registered for given PAN Number (usually provided during the PAN card registration or updation)                   |
| `pan_data.pan_details.address`              | dict      | Object containing the address available against PAN from the government source                 |
| `pan_data.pan_details.address.building_name`| string    | Address: Building or House number                 |
| `pan_data.pan_details.address.locality`     | string    | Address: Locality                                 |
| `pan_data.pan_details.address.street_name`  | string    | Address: Street                  |
| `pan_data.pan_details.address.pincode`      | string    | Address: Pincode                                  |
| `pan_data.pan_details.address.city`         | string    | Address: City                                     |
| `pan_data.pan_details.address.state`        | string    | Address: State                                    |
| `pan_data.pan_details.address.country`      | string    | Address: Country (For Indian Address, this field is generally given as empty string from the source and for other countries, this field contains the country name.)        |

---

## No PAN Details Found for give PAN input

```json
{
  "request_id": "39e3e6b0-6c26-4614-bdda-83b8fafd4d79",
  "pan_data": {
    "user_exist": false
  }
}
```

---

## Some error occured while getting PAN details

```json
{
  "request_id": "39e3e6b0-6c26-4614-bdda-83b8fafd4d79",
  "pan_data": {
    "error": true
  }
}
```

---

## Invalid PAN Number input

```json
{
  "request_id": "39e3e6b0-6c26-4614-bdda-83b8fafd4d79",
  "pan_data": {
    "error_msg": "Invalid Login id"
  }
}
```


## Other Possible non 200 response codes.

### 4XX
```json
{"error_msg": "Bad Request"}
```

### 5XX
```json
{"error_msg": "Sign3 Server Error" || <message related to error>}
```

---

## Notes

- Ensure that your PAN input is in valid format (10-character alphanumeric string).

---

## 🆕 Changelog

### April 21, 2025

- Initial release of Sign3 Pan details basic integration guide.
- Added `request curl`, `header` and `payload` details.
- Included pan details response structure and field explanations.
- Provided different error case examples.

---
