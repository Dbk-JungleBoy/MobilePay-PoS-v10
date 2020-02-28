# <a name="release_notes"></a>PoS API Release Notes

The Point of Sale API V10 will be in production **Q2 2020**. It will be released in SandProd **Q1 2020**.

## Changelog

### 2020-02-27

- Added HTTP StatusCodes and ErrorCodes pr. endpoint in the API. Response codes have also slightly changed in the API on the basis of this documentation so be aware of minor changes to the API as well. See more at [API Errors](endpoint_errors).

- Added header to all endpoints in the API called X-MobilePay-MerchantVatNumber. For more information see [Input Formats](input_formats#HTTP_Headers)

- Fixed wrong header name from X-IBM-ClientId to X-IBM-Client-Id. See [Input Formats](input_formats#HTTP_Headers)

---
### 2020-02-07

- Added documentation on payment restrictions under [Input Formats](input_formats#payment_restrictions) and expanded on best practices regarding payment restrictions [Best Practices](best_practices)

---
### 2020-02-04

- Specified expiration length of Refund reservations to 24 hours. [Refunds](payment_flows#refunds)

---
### 2020-01-31

- Added a description of Self-Certification to the documention under [Self Certification](self_certification)

---

### 2019-12-23

- Changed the month that news will come out regarding self-certification from December 2019 to January 2020

---

### 2019-12-10

- Added `BrandName` to the subpage **Input and Output Formats** in the Brand section

---

### 2019-12-10

- Renamed subpage **Input Formats** to **Input and Output Formats**.
- Adjusted content of subpage **Input and Output Formats**
  - **HTTP Headers**
    - Renamed from `X-MP-\*` to `X-MobilePay-\*`.
    - Removed `X-MP-IntegratorId`.
  - **Payments**
    - Added `PlannedCaptureDelay`, `CustomerToken` and `CustomerReceiptToken`.
