# DCC Print Templates (Germany)

This directory contains the German print templates as SVG. The templates have
placeholders in the form `$xx` where `xx` corresponds to the field names of the DCC schema.

- [DCC Certificate of Recovery](RecoveryCertificateTemplate_v3.svg)
- [DCC Test Certificate](TestCertificateTemplate_v3.svg)
- [DCC Vaccination Certificate](VaccinationCertificateTemplate_v3.svg)

> The templates contain DEMO overlays that need to be removed for production cases.

## Fonts

The SVG templates require the free [Open Sans](https://fonts.google.com/specimen/Open+Sans) font.

## QR Code Replacement

Within the SVG the QR Code is added by replacing `$qr` by a base64 encoded png image of the
actual QR Code.
