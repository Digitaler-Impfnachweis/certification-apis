# Certification API Documentation

This repository contains documentation for certification endpoints for the
vaccinationn issuer.

- [ubirch-certify-api.yaml](ubirch-certify-api.yaml) - certification endpoints

# Building

Install redoc-cli and create a single html documentation:

```bash
npm install -g redoc-cli
redoc-cli bundle -o ubirch-certify-api.html ubirch-certify-api.yaml
```

## Copyright

```
Copyright (c) 2021 ubirch GmbH

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
