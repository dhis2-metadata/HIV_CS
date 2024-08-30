# Release Note { #hiv-cs-release-note }

## 2.0.1

A new version of the HIV Case Surveillance module (v2.0.1) has been released with some minor changes.
- Revision of metadata naming convention for programRules, OptionSets
- Deletion of Category option (UID=gQ3Pe5EG9MJ, name=DELETE_Other)
- Deletion of Tracked Entity Attribute (UID=ntelZthDPpR, name=TB registration number)

## 2.0.0

This is a major new release of the HIV metadata and system design for integrating HIV data into national HMIS for analysis. 

**New Content**

New content for the HIV Case Surveillance module is based on the latest WHO [Consolidated guidelines on person-centred HIV strategic information: strengthening routine data for impact](https://www.who.int/publications/i/item/9789240055315) (2022) and all indicators are compliant with the HIV Digital Adaptation Kit, second version.

Components included as part of this package:

- New components for covering the core and optional indicators present on the last HIV SI guidelines such as STI, Cervical Cancer, Vertical transmission and Viral hepatitis
- Revision of all indicators and program indicators
- Revision of all metadata and allignement with last naming convention
- Metadata mapping with the HIV DAK version 2.0
- Revision of current HIV Case Surveillance dashboards and integration of new ones
- Datasets used for population of dashboards (all dashboards are aggregate based)
- Data exhchange mapping 

All the changes are highlighted on the [Diff file](resources/files/HIV_CS_DIFF_1.0_2.0.xlsx)

## 1.0.0

Initial release of the HIV Case Surveillance package

1. Denominators for indicators have been aligned to the corresponding aggregate HIV HMIS package, using 'HIV - Estimated people living with HIV' `ORYyhLEyzrA`
2. Program indicator definitions updated for retention indicators:
   HIV - PLHIV retained in the last 12 months (numerator): includes enrollments who started ART exactly 12 months before the start of the reporting period, excluding those with at least one visit marked LTFU, died, transfer out, or stopped treatment. You must have at least one visit RETAINED in order to be included in the numerator.
   HIV - PLHIV started HIV 12 months ago (denominator): includes all enrollments who started ART exactly 12 months before the start of the reporting period
3. Descriptions of indicators updated
4. Updated names of user groups for consistency with naming conventions.
