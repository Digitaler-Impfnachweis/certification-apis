# Schema for creating certificates

Please read the following instructions carefully in order to create commonly compatible DCCs using the `/api/v2/issue` API.  
The following information adheres to the [Guidelines on Technical Specifications for EU Digital COVID Certificates JSON Schema Specification Schema version 1.3.0 2021-06-09](https://ec.europa.eu/health/sites/default/files/ehealth/docs/covid-certificate_json_specification_en.pdf), but also includes further information, e.g. for **booster vaccinations**. Please bear in mind, that specifications for the Digitales Impfquotenmonitoring by the RKI might be different from the specifications for the certificates.

The APIs can be used to issue both vaccination and recovery certificates. Vaccination certificates document a single vaccination dose, like for example first dose, second dose or a booster dose. Recovery certificates document a recovery from COVID-19 detected by a positive PCR test.  
Any certificate should only contain a single record: a single vaccination or a single recovery statement.

## Vocabulary EN/DE

*   vaccination certificate = Impfzertifikat
*   recovery certificate = Genesenenzertifikat
*   recovery vaccination (A vaccination, after a person has recovered from COVID-19) = Genesenenimpfung (Impfung, nachdem eine Person eine COVID-19 Erkrankung durchlaufen hat)
*   cross vaccination = Kreuzimpfung
*   booster vaccination = Auffrischungsimpfung


## Information for all types of certificates

All certificates must be provided with information on the receiver of the certificate:

*   `nam/fn`: Surname(s), such as family name(s), of the holder. Exactly 1 (one) non-empty field MUST be provided, with all surnames included in it. In case of multiple surnames, these MUST be separated by a space. Combination names including hyphens or similar characters must however stay the same. ≤ 50 characters.
*   `nam/gn`: Forename(s), such as given name(s), of the holder. If the holder has no forenames, the field MUST be skipped. In all other cases, exactly 1 (one) non-empty field MUST be provided, with all forenames included in it. In case of multiple forenames, these MUST be separated by a space. ≤ 50 characters.
*   `dob`: Date of birth of the DCC holder. Complete or partial date without time restricted to the range from 1900-01-01 to 2099-12-31\. Exactly 1 (one) non-empty field MUST be provided if the complete or partial date of birth is known. If the date of birth is not known even partially, the field MUST be set to an empty string "". This should match the information as provided on travel documents. One of the following ISO 8601 formats MUST be used if information on date of birth is available. Other options are not supported. `YYYY-MM-DD`, `YYYY-MM`, `YYYY`  
    Example:

```json
"nam": {
	"fn": "Musterfrau",
	"gn": "Max"
}, 
"dob": "1991-02-03"
```

## Vaccination Certificates

Vaccination records are marked with `v`, for example:

```json
{
	"nam": {
		"fn": "Musterfrau",
		"gn": "Max"
	}, 
	"dob": "1991-02-03",
	"v": [{
			"id": "IZ28215B",
            "tg": "840539006",
            "vp": "1119305005",
            "mp": "EU/1/20/1528",
            "ma": "ORG-100001699",
            "dn": 1,
            "sd": 2,
            "dt": "2021-04-14"
	}]
}
```
The fields within the vaccination certificate request must be as follows:

*   `id`: Identifier of the administering location (i.e. vaccination center ID / DIM-ID, BSNR or similar identifier). It will be used in the construction of the DGCI (digital green certificate identifier). Due to the specification of the DGCI only the use of uppercase letters and numbers 0-9 are allowed. `^[0-9A-Z]+$`
*   `tg`: The disease agent targeted as defined by SNOMED CT. Currently for COVID-19, only `"840539006"` is to be used.
*   `vp`: The vaccine prophylaxis as defined by SNOMED CT. Can be
    *   `1119349007` for a SARS-CoV-2 mRNA vaccine
    *   `1119305005` for a SARS-CoV-2 antigen vaccine
*   `mp`: The medicinal product used for this specific dose of vaccination. Can be
    *   `EU/1/20/1528` for Comirnaty by BioNTech/Pfizer
    *   `EU/1/20/1507`for Spikevax by Moderna
    *   `EU/1/21/1529` for Vaxzevria by AstraZeneca
    *   `EU/1/20/1525` for COVID-19 Vaccine Janssen by Janssen-Cilag/Johnson and Johnson
*   `ma`: Marketing authorisation holder or manufacturer, if no marketing authorization holder is present.
    *   `ORG-100030215` for Biontech Manufacturing GmbH
    *   `ORG-100031184` for Moderna Biotech Spain S.L.
    *   `ORG-100001699`for AstraZeneca AB
    *   `ORG-100001417` for Janssen-Cilag International
*   `dn`: Sequence number (positive integer) of the dose given during this vaccination event, please see the information below.
*   `sd`: Total number of doses (positive integer) in a complete vaccination series according to the used vaccination protocol, please see the information below.
*   `dt`: The date when the described dose was received, in the format `YYYY-MM-DD`.

### Values for `dn` and `sd` for base vaccinations with the same vaccine

Base vaccinations are regular vaccinations with the same vaccine.  
Valid values for regular vaccine are limited to

*   for the vaccines by Biontech, Moderna, AstraZeneca:
    *   `sd` must always be `2`
    *   `dn` can either be `1` for the first dose or `2` for the second dose
*   for the vaccine by Johnson and Johnson:
    *   `sd` must always be `1`
    *   `dn` must always be `1`

### Values for `dn` and `sd` for base vaccinations with the different vaccines (cross vaccinations)

Cross vaccinations are second dose vaccinations, where the first dose has been a different vaccine than the second dose.

Valid values for cross vaccines are limited to

*   for the vaccines by Biontech, Moderna, AstraZeneca:
    *   `sd` must always be `2`
    *   `dn` must always be `2`

### Values for `dn` and `sd` for recovery vaccinations

Recovery vaccinations are second dose vaccinations, where the patient has recovered from COVID-19 before.

Valid values for recovery vaccines are limited to

*   for the vaccines by Biontech, Moderna, AstraZeneca:
    *   `sd` must always be `1`
    *   `dn` must always be `1`
*   for the vaccines by Johnson and Johnson
    *   `sd` must always be `1`
    *   `dn` must always be `1`

### Values for `dn` and `sd` for booster vaccinations

Booster vaccinations are vaccinations after the patient has received her full vaccination series by base or cross vaccinations. Booster vaccinations can be with the same vaccine or with different vaccines as before.

Valid values for booster vaccines are limited to  
*   After a full vaccinations series has been received  
*   `sd` must always be equal to `dn`, where the value indicates the number of the total vaccinations received.

This implies, that

*   for a full vaccination series by the Biontech, Moderna, AstraZeneca vaccine (or combinations) the values will be `sd=3` and `dn=3` for the first booster vaccination, `sd=4` and `dn=4` for the second booster vaccination, etc.
*   for a full vaccination series by the Johnson and Johnson vaccine the values will be `sd=2`and `dn=2` for the first booster vaccination, `sd=3` and `dn=3` for the second booster vaccination, etc.

## Recovery Certificates

Vaccination records are marked with `r`, for example:

```json	
{
	"nam": {
		"fn": "Musterfrau",
		"gn": "Max"
	}, 
	"dob": "1991-02-03",
	"r": [{
			"id": "BSNR12345",
            "tg": "840539006",
            "fr": "2021-04-21",
            "df": "2021-05-01",
            "du": "2021-10-21"
        }]
}
```
The fields within the recovery certificate request must be as follows:

*   `id`: Identifier of the administering location (i.e. vaccination center ID / DIM-ID, BSNR or similar identifier). It will be used in the construction of the DGCI (digital green certificate identifier). Due to the specification of the DGCI only the use of uppercase letters and numbers 0-9 are allowed. `^[0-9A-Z]+$`
*   `tg`: The disease agent targeted as defined by SNOMED CT. Currently for COVID-19, only `"840539006"` is to be used.
*   `fr`: The date when a sample for the NAAT/PCR test producing a positive result was collected, in the format `YYYY-MM-DD`.
*   `df`: The first date on which the certificate is considered to be valid. The first date valid begins on the 28th day after the date of the sample collection, so `df` must be `fr + 28 days`
*   `du`: The last date on which the certificate is considered to be valid. A recovery certificate must not be issued for NAAT/PCR tests older than 180 days. The date for `du` must be
    *   after or equal `fr`
    *   before or equal `fr + 180 days`.

## Further resources

*   [https://ec.europa.eu/health/ehealth/covid-19_en](https://ec.europa.eu/health/ehealth/covid-19_en)
*   [https://ec.europa.eu/health/sites/default/files/ehealth/docs/covid-certificate_json_specification_en.pdf](https://ec.europa.eu/health/sites/default/files/ehealth/docs/covid-certificate_json_specification_en.pdf)
*   [https://github.com/ehn-dcc-development/ehn-dcc-valuesets](https://github.com/ehn-dcc-development/ehn-dcc-valuesets)
