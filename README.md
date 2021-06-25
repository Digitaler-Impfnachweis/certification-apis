# Certification API Documentation

This repository contains documentation about certification endpoints and related flows for authentication and authorization to be integrated with information systems of physicians, pharmacies and vaccination centers.

The interface specifications are aligned with the DGC schema specification published in version [1.0.0](https://github.com/ehn-digital-green-development/ehn-dgc-schema/releases/tag/1.0.0).

## OpenAPI Specifications

- **Issuer API** ([OpenAPI Specification](dgc-certify-api.yaml))

  Provides endpoints for the web frontend, vaccination center software,
  patient management software to issue [DGC](https://ec.europa.eu/info/live-work-travel-eu/coronavirus-response/safe-covid-19-vaccines-europeans/covid-19-digital-green-certificates)
  compliant certificates.

- **[DSC TrustList Update API](dsc-update/README.md)** ([OpenAPI Specification](dsc-update/dsc-update-api.yaml))

  Provides an endpoint for verifiers to get the latest list of trusted Document Signing Certificates (DSC).

## Integration scenarios
The following table illustrates how the solution can be integrated into existing solutions:

| Scenario | Description | 
| --- | --- | 
| 1. Medical Practitioner office | Solution is integrated in the primary IT solution of the doctor. Solution is connected to the Telematik Infrastructure and leverages the SMC-B as primary authentication solution.|
| 2. Vaccination Center | Solution is integrated in the primary IT solution of the vaccination center. Solution is connected to the internet and leverages client-certificate authentication. Software solution is required to have a strong user authentication based on BSI req. like hardware-tokens and user credentials |  


## Authentication
In order to use the mentioned APIs different authentication options exist. 

- [Authentication with TI SMCB](SMCB-Authentication.md)
- Authentication with client-certificate (mTLS)

## Additional Information

For implementors the official documentation of the Digital Green Certificate
can be found on the [EU eHealth Network](https://ec.europa.eu/health/ehealth/key_documents_en) page.

## Copyright

```
Copyright (C) 2021 IBM Deutschland GmbH 
Copyright (C) 2021 ubirch GmbH

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
