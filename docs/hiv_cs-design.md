# HIV Case Surveillance System Design { #hiv-cs-design }

## Background and Purpose

The Case Surveillance component of the DHIS2 Toolkit for HIV is based on the latest WHO [Consolidated guidelines on person-centred HIV strategic information: strengthening routine data for impact](https://www.who.int/publications/i/item/9789240055315) (2022) and the Digital adaptation kit for HIV: operational requirements for implementing WHO recommendations and standards within digital systems, second edition.

This toolkit includes
- WHO-recommended dashboard analyses and core indicators for monitoring HIV care and treatment initiatives and using key metrics to adjust programming & drive impact 
- DHIS2 Tracker program aligned with the WHO Digital Adaptation Kit data standards for individual level data collection and operational person-centred monitoring at facility and community levels
- Aggregate data sets and data elements to model aggregated tracker data for performant, anonymized analytics 
- Data exchange mapping for aggregating DHIS2 tracker data for Case Surveillance indicators and integration of key indicators with national HMIS

The HIV toolkit is optimised for improving strategic information systems in countries to facilitate data-driven decision-making, as well as streamlining routine data management processes. The DHIS2 HIV Case Surveillance tracker is not designed to provide clinical decision-support for HIV care and treatment services, but rather serves as an operational tool and a source of individual level data for person-centred monitoring. 

The system design document explains the reference configuration in DHIS2 for the HIV Case Surveillance use case, including a detailed description of DHIS2 Tracker configuration, dashboard design, use of program indicators, mapping of tracker data to the aggregate data model, and data exchange mechanisms. This document also does not consider the resources and infrastructure needed to implement such a system, such as servers, power, internet connections, backups, training and user support, which can be found in the [DHIS2 Tracker Implementation Guide](https://docs.dhis2.org/en/implement/tracker-implementation/target-audience.html).

Reference metadata for this toolkit is available at: [dhis2.org/metadata-downloads](https://dhis2.org/metadata-downloads/).

### Acknowledgements

The HIV Case Surveillance toolkit has been developed in partnership with the WHO Global HIV Programme and the Pan American Health Organization (PAHO) with support from The Global Fund. We are grateful to WHO and PAHO for providing subject matter expertise in the design and development of these tools, as well as to the many countries who have shared their implementation experience with us

## System design overview

### Background

Background
Efforts to end the HIV epidemic by 2030 face persistent challenges, with 1.5 million new infections in 2020 alone. Key populations and their sexual partners constituted 70% of global new infections in 2021 (UNAIDS, 2022), highlighting barriers to equitable access. Paramount importance is to emphasise routine programmatic data for tracking service delivery, identifying at-risk individuals, and achieving universal access through a person-centred approach. Confidentiality and data privacy is crucial, especially for vulnerable populations that may be stigmatised, necessitating a cautious approach to individual-level data collection and anonymization for data use needs above the service delivery level.. 

A person-centred monitoring approach offers distinct advantages for longitudinal HIV data collection that can track and record HIV care and treatment services provided to an individual over time and space (e.g. different service delivery sites and touchpoints with HIV care and treament programmes). Scaling up individual level data collection allows for flexible disaggregation and enhanced analysis by various factors such as time, location, age, gender, clinical status, coinfections, treatment and more. This depth of information enhances our understanding of HIV epidemiology and facilitates ongoing monitoring of trends. Implementation of case-based electronic data is anticipated to enhance data quality by minimising data entry steps, integrating automatic calculations and validations, swiftly correcting or completing inconsistent or incomplete individual records, and facilitating de-duplication.

However, in most LMICs, the digitization of individual-level data systems managed by Ministries of Health often takes years to reach scale, and must be matched by substantial in-country resources to sustain the system over time. Furthermore, historical data and other programmatic data are often collected or represented through the aggregate data model – such as denominators based on Spectrum estimates for people living with HIV and aggregated reporting from paper-based service delivery sites with low/no connectivity, limited availability of devices and/or weak infrastructure. 

Therefore, the HIV Case Surveillance toolkit leverages all DHIS2 applications and data models to support practical, implementable approaches to collecting, analysing and using HIV Case Surveillance data effectively to manage and link newly identified HIV cases into treatment programs and ensure a proper monitoring of people living with HIV. These tools can be used in a modular way to meet the needs of any country along the maturity model for digitization of health data – including hybrid implementation of paper-based and digitised systems, as well as incorporating data from other electronic tools like hospital EMRs where these may be used.

### Use Case

The HIV Case Surveillance toolkit is designed to support routine data collection for programmes that support people living with HIV to enrol in a HIV program and receive HIV care and treatment services. The system is designed to bring data together to generate granular HIV Case Surveillance indicators and their disaggregations for routine data analysis, person-centred monitoring and strategic information through flexible dashboards and ad-hoc analysis with DHIS2 analytics tools. 

The data capture components of the system design allows for staff at service delivery sites, or higher levels depending on local contexts, to capture the core data elements during routine interactions with HIV  care and treatment programme participants that will allow programme staff to generate key performance indicators as detailed in WHO’s Consolidated guidelines for person-centred HIV strategic information (WHO, 2022). This component uses the DHIS2 Tracker data model. While DHIS2 HIV Case Surveillance tracker program is not optimised to support clinical case management or decision support, it serves as a useful electronic registry that supports decentralised electronic data capture of Case Surveillance data down to health facility or any other point of service from all types of actors. Individual level tracker data are aggregated using DHIS2 program indicators; which are mapped to aggregate data elements and data sets for populating core indicators and serving the analytical dashboards with anonymized data. 

The DHIS2 tracker program structure has been harmonised and follows the same logic as the HIV prevention Tracker, allowing for a person enrolled in a prevention program to enter the Case Surveillance program with a positive HIV diagnosis. In some contexts, it may be desirable to integrate prevention and Case Surveillance into a single tracker program. Harmonisation of data elements between the two trackers will allow for local adaptation and integration, described further in the ‘Implement’ section of this document. 

#### Intended users

The HIV Case Surveillance system design focuses on meeting the needs of end users at all levels of the health system, including those responsible for implementing HIV programmes in countries. These users may include:

- **HIV program managers & staff (national & sub-national)**: data users who are responsible for routine analysis of data, using data to improve operations and programme strategies, and providing data-driven feedback to programme staff, including implementing partners, facilities, and other service delivery points
- **HIV programme data managers**: users who are responsible for overseeing data collection, management, data quality, analysis and reporting functions for the national HIV programme
- **System admins/HMIS focal points**: MOH staff and/or core DHIS2 team responsible for maintaining and improving data systems for health programmes, integrating data streams into national platforms, providing technical support for system design, adaptation and end user support; and maintaining the DHIS2 system over time
- **Service delivery sites**: Staff at service delivery sites may use the DHIS2 Tracker program to record data for routine visits, as well as use DHIS2 tools like working lists and line-lists to follow up with clients and monitor individual-level uptake of services. 
- **Implementing partners**: organisations who provide technical assistance to the national HIV programme, collect and analyse data on behalf of the overall national programme strategy, and may be responsible for the operations of service delivery networks such as HIV counselling, educational outreach, community health workers.

### Design Structure

The DHIS2 HIV Case Surveillance configuration is structured in three major components:
- **Dashboards & Indicators**: the HIV Case Surveillance dashboards and indicators are all served by the aggregate data model in DHIS2. Case Surveillance dashboards are shared with the HIV HMIS module. 
- **Aggregate data sets**: aggregate datasets, data elements and associated category combinations are configured according to the WHO’s HIV analysis framework to populate the core indicators. Three datasets are designed to receive aggregated tracker data (e.g. via program indicators) and model this data in a performant way, leveraging the advantages of the aggregate data model in DHIS2 for analysis. Alternatively, these datasets can also be used for aggregate paper-based reporting; or to store data reported routinely from other individual-level data systems in use, such as sites using EMRs or other mobile applications. 
  - HIV Case Surveillance (monthly)
  - HIV Case Surveillance (yearly)
  - Population estimates 
- **Tracker program**: a DHIS2 tracker program has been configured for individual-level, longitudinal data collection. The tracker program can be used with the DHIS2 web or Android clients.

These modular components are designed based on the heterogeneous nature of HIV data systems in countries and support the typical architecture for implementing case-based data systems alongside integrated national HMIS infrastructure: 

![HIV illustrative architecture](resources/images/hiv_architecture_simple.png)

## Tracker

### Tracker Program Structure

The tracker program structure is as follows:

![Case Surveillance tracker structure](resources/images/case_surveillance_program_structure.png)

| **Stage**           | **Description**                                              |
| ------------------- | ------------------------------------------------------------ |
| Enrollment          | The enrollment stage collects the basic demographic data about a person, including unique identifiers, as Tracked Entity Attributes (TEAs). Several of these core TEAs such as Family Name and Given name are shared across DHIS2 Tracker programs. The Tracked Entity Type for the HIV Case Surveillance program is ‘Person’. **The stage is non-repeatable.** |
| Initial case report | The stage can be filled at the same time as the enrollment of the patient.This stage contains the information related to HIV diagnosis and key population groups. **The stage is non-repeatable.** |
| Visit               | Main stage that collects all the information necessary for the follow-up of the patient and the program. **The stage is repeatable** |
|Follow-up | This stage is not tied to any indicators and is purely designed as a tool for recording any contact made with patients who have missed treatment visits and will need to be contacted. **The stage is repeatable**|

### Tracked Entity Type

The DHIS2 HIV Case Surveillance tracker program allows for the enrollment of a tracked entity type [TET] ‘person’ into the HIV Case Surveillance program. The TET type is shared with the HIV Prevention tracker to allow enrollment of the same TEI in both programs, in the case that a person receiving prevention services receives a positive HIV diagnosis.

### Enrollment

It is assumed that the HIV Case Surveillance workflow begins by enrolling any person with a confirmed HIV positive status.

When a person is enrolled in the HIV Case Surveillance program as a Tracked Entity Instance (TEI), Tracked Entity Attributes (TEA) are recorded to form the case profile. All TEAs are aligned and harmonised with those included in the HIV Prevention tracker allowing for enrollment in both programs or for integration into a single tracker program if desired at the local implementation level. Note that several TEAs are shared across non-HIV DHIS2 Tracker programs; these are configured with a prefix ‘GEN’ (general) to indicate they are shared between tracker programs. Check out the [Common Metadata Library](https://docs.dhis2.org/en/topics/metadata/dhis2-who-digital-health-data-toolkit/common-metadata-library/design.html) for more information on shared tracker metadata.

The TEA **program UID** attribute refers to an HIV program specific identifier. It has been kept intentionally blank as coding processes are dependent on local context. Refer to the [DHIS2 User Documentation on configuring system-wide unique identifiers as DHIS2 TEAs](https://docs.dhis2.org/en/use/user-guides/dhis-core-version-238/configuring-the-system/programs.html#create_tracked_entity_attribute:~:text=Select%20Unique%20to,same%20organisation%20unit.). The use of anonymous UID codes allows individuals accessing HIV prevention services to be uniquely identified and followed longitudinally, without the collection of other types of personally identifying information (WHO, 2022).

The **date of birth and age** can be assigned in two different ways:

- Known date of birth: select the Date of birth
- Unknown date of birth: select the Date of birth - Unknown option and then enter the age either in years or months.

Once done, a 'probable' age of birth is assigned to the Date of birth based on the value entered either on the years or month TEA.

![Date of Birth](resources/images/date_of_birth_unknown.gif)

>**Note:**
>
>The date of birth is configured as mandatory; it must always have a value because it is used in the calculation of all program indicators involving age groups

### Stage 1: Initial case report (non-repeatable)

The data acquired at this stage pertains to HIV diagnosis related information and the identification of clients belonging to one or multiple key population groups. 

Program rules have been configured to populate the age of HIV diagnosis based on the difference between the age of birth and the date of HIV diagnosis.

Concerning the key population information we recommend implementations to refer to WHO’s 2022 *Consolidated guidelines on person-centred HIV strategic information*, and specifically sections 2.2.2 for guidance on collecting information on key populations for prevention interventions to allow for this disaggregation, further considerations in section 5.1.1, and section 6.4 on maintaining data privacy, security and confidentiality as it relates to key population data captured.

![Initial case report](resources/images/initial_case_report.png)

Details of the HIV diagnosis and key population data is collected as a program stage in order to further protect the sensitive nature of this data. Specifically, by including this data only in the program stage, it is not accessible for searching a TEI – where users may have broader access to search for clients. 

In some implementations, instead of having the different key population groups listed as selectable categories, a set of questions has been proposed and according to the answer a key population identification is assigned to the clients. *See the section Implementation Considerations & Local Adaptation*.

For an individual enrolled in an HIV Case Surveillance program, it is possible that their association with a key population group may change over time, as an individual’s risk of HIV acquisition, participation in high risk behaviours, and HIV prevention needs vary over time. The program stage is intentionally designed as non-repeatable to allow optimised analytics based on the key population group. However, a user may revisit this stage and modify the key population groups as needed.

### Stage 2: Visit [repeatable]

#### Configuration of Dates

This repeatable program stage captures all information relevant to visits/encounters for the person receiving HIV care and treatment services, regardless of which type of service they access. It allows for capturing multiple types of services provided in a visit. This is integral for a comprehensive, person-centred management and reporting system. 

In this program stage, the DHIS2 program ‘event date’ (or default ‘report date’ in the Maintenance app for program configuration) has been renamed to represent ‘Visit date.’ This means that all analytics based on the event date will evaluate based on the date of the person’s encounter (visit) with the HIV care and treatment service provider.

![DHIS2 Event Date](resources/images/event_date_visit_date.png)

In addition to the visit date, additional key dates have been configured as Tracker domain data elements related to these activities (e.g. ART initiation, STI test date, etc.) and these should be reported to enable additional time-based analytics.

#### Visit details

The initial Visit Details section allows the user to report the patient status at the visit and as well select which HIV care and treatment services will be recorded for this visit report. 

The patient status is a key information that needs to be reported at every visit and it’s used in most of the indicators present in the program.
The set of options assigned is:
- PLHIV: the client is alive and is part of the cohort
- Death (documented): the client is dead
- Lost to follow up: the client is lost to follow up
In case ‘death’ or ‘follow up’ is selected, the relatives date of dead and declaration of lost to follow-up are requested

>**Note:**
>
>The recommended threshold for designation of people living with HIV on ART as lost to followup is 28 days after the last missed appointment or last ARV refill, to account for DSD ART. See chapter 3 of WHO's 'Consolidated guidelines on person-centred HIV strategic information' for additional information.

A series of program rules will show/hide the desired sections depending on which type of HIV care details will be recorded. It is generally assumed that the user will create a new Event in the Visit stage for each time they interact with a service provider, regardless of how many HIV care services they access during the visit. 

The ‘Currently pregnant’, ‘Cervical cancer’ and ‘Vertical transmission’ will only show for PLHIV enrolled with TEA ‘Sex assigned at birth’ as ‘Female’ or ‘Other’, based on program rules. 

![Visit details section and selection of HIV care services](resources/images/visit_details_selections.gif)

>**Note:**
>
>Hiding a section does not erase the data that are recorded there and all the program indicators are NOT filtered by the selection or not of the section. In case there are any data that has been mistakenly entered, they will need to be removed manually one by one and not simply de-selecting the section

#### Treatment

The treatment section is always shown and doesn’t need to be selected from the ‘Visit details’ section as ART treatment is the central pillar on the management of PLHIV.

The main and mandatory information to be reported is the treatments status in which, according to the current status of the patient at the visit, a set of options has been assigned:
- **Initiation (new)**: PLHIV starting ART for the first time in their life
- **Initiation (after stopping)**: PLHIV restarting the ART after a period of interruption.
- **On ART**: PLHIV already on ART at the moment of the visit
- **Refused treatment**: PLHIV refuse the ART after counselling
- **Stopped treatment**: PLHIV reports of having stopped treatment

The viral load information will be shown only in case the difference between the visit date (DHIS2 event date) and the date of ART initiation is more than 180 days.

DSD ART models information need to be customised prior implementation according to country guidelines on provision of multi-monthly ART dispensing.

‘Last days with ART’ element is auto assigned adding the amount of days of ART provided to the visit date (DHIS2 event date) as considered the date in which the ART has been provided.

![Treatment section](resources/images/treatment_section.png)

#### TB/HIV

Within the HIV/TB section information related to TB screening, testing, treatment of active TB disease and TB preventive treatment are requested.

A serie of program rules are being setted to:
- Hide all the information related to ‘TB screening, test and TB preventive treatment’ related information in case the PLHIV has TB disease
- Report the date of ‘TB diagnosis’ in the following visits (DHIS2 event) in case has been reported and still with the disease
- Report the date of ‘TB treatment start date’ in the following visits (DHIS2 event) in case has been reported and still on treatment
- Report the date of ‘TB preventive treatment initiated’ in the following visits (DHIS2 event) in case has been reported and still on treatment

![HIV/TB section](resources/images/hiv_tb_section.png)

#### Vertical transmission

The information collected for the vertical transmission have been classified in the two (2) different (sub)section according to the timing of when data collection should occur:
- Information at birth
- Follow-up visit

Within the first section ‘Information at birth’ information related to delivery date and place of delivery, ANC first visit date and HIV status, ART and viral load status and HIV-exposed infant ART status are requested

![Information at birth section](resources/images/information_birth_section.png)

Within the second section ‘Follow-up visit’ information related to the age of the HIV-exposed infant at visit, breastfeeding status, ART status of the mother and HIV testing of the HIV-exposed infant are requested.

![Follow-up visit](resources/images/information_FU_section.png)

#### STI

In the Sexually Transmitted Infections (STI) section, information related to the STI syndrome diagnosed, STI test performed and treatment administered are collected among clients enrolled in the HIV Case surveillance program. 

![STI Data Collection](resources/images/STI.png]

#### Viral hepatitis

In the Viral hepatitis section, information related to HBsAG and HCV infection, tests performed and results and treatment are collected among clients enrolled in the HIV Case Surveillance program. 

In case the PLHIV is infected with HBV and/or HCV, all the information related to the test are going to be hidden through a serie of program rules

![Viral Hepatitis](resources/images/viral_hepatitis.png)

#### Cervical cancer

Within the cervical cancer section, information related to the number of HPV vaccination doses received, cervical cancer diagnosis and cervical cancer screening with relative outcome and treatment status are requested.

In case the WLHIV has been diagnosed with invasive cervical cancer, all the information related to the screening are hidden

![Cervical cancer](resources/images/cervical_cancer.png)

### Stage 3: Follow-up [repeteable]

This repeatable program stage is purely designed as a tool for recording any contact made with patients who have missed treatment visits, and will need to be contacted.
It is not currently linked to indicators and can safely be removed from the programme if not used in a clinical setting without affecting the other modules.

In this program stage, the DHIS2 program ‘event date’ (or default ‘report date’ in the Maintenance app for program configuration) has been renamed to represent the ‘Date of attempt to contact client’. 

It records the reason why this follow up is needed (Missed clinical care visit, missed medication pickup, missed non-clinical visit, did not initiate ART, inconclusive HIV status, test results received, Other follow up reason) follow-up method (Text message, phone call, home visit or other) and the result of the follow up (Returning to clinic, self-transferred out, hospitalised, refused to return, not located, reported dead, confirmed dead). If when following-up the patient has changed their treatment status (for example, decided to stop treatment), the program displays a prompt to complete a Visit stage option and record this.

![Follow-up stage](resources/images/follow_up.png)

### Tracker Data Elements

All data elements configured for the Tracker domain are also included in the Data Element Group ‘HIV case surveillance (tracker)’ [pGlRt4xB9rz]. This serves as a de facto DHIS2 data dictionary for the HIV case surveillance tracker use case. It allows for the data elements to be exported from DHIS2 and used independently of the Tracker program configuration, for example in the case that an implementation redesigns their Tracker from scratch for local workflows and still wants to maintain compliance with the core WHO-recommended data dictionary included in the Digital Adaptation Kit.

#### Cloned data elements for multiple option selection

Within the program stages for  ‘Initial Case report’ and ‘Visits’, there are a number of data elements that are cloned to allow the selection of multiple options for a given concept, sharing the same option set. This design is implemented as follows:
- Cloning of data elements eligible for multi-option choice
  - The number of clone of the data element must be the same as the number of options present in the related option set
  - Each cloned data element has its own UID, name and code
- Program Rules
  - Hide the consequent Data Elements if the previous have not been selected
  - Show error in case the same Option has been selected more than once in the same group of Data Elements

For example, to capture multiple probable route of transmission, there are a series of data elements that are cloned to represent each discrete diagnosis: 
- HIV - Probable route of transmission - 1 [ODG7SJQHCBv]
- HIV - Probable route of transmission - 2 [jsXCQZT5f9c]
- HIV - Probable route of transmission - 3 [HxPpUO6SuXD]
- HIV - Probable route of transmission - 4 [TOLlMMdudRi]

#### Hidden Data Elements & Assigned Values to Data Elements

All the Data Elements mentioned hereunder are “hidden” and therefore not visible during the data entry process; however they are required for the calculation of program indicators. 

Here we provide an overview of all the “hidden” data elements which are contained in the repeatable Visit stage, where program rules are used to assign a value. In many cases, the program rule simply assigns a value from a previous program stage (such as Initial Report) in order to allow the program indicators to calculate correctly. 

**Assigning key population and date of HIV diagnosis data element value to Visit Stage**

Program rules are used to copy and assign the value of the Client key population group and date of HIV diagnosis recorded in the initial case report to a hidden data element field on the repeatable ‘Visit’ program stage. This allows for all calculations and disaggregations based on key population groups and age of infection to be configured namely by using the autoassigned data element as a filter in the program indicator

These Data Elements are identified with the postfix “- VISIT”

| Metadata     | UID         | Name                                                                               |
|--------------|-------------|------------------------------------------------------------------------------------|
| Data Element | mNeVBYWladf | HIV - Positive Date - VISITS                                                       |
| Data Element | ySHCKMYGHD6 | HIV - Age at Diagnosis - VISITS                                                    |
| Data Element | kXutCUTOxcq | HIV - Key population - Men who have sex with men - VISITS                          |
| Data Element | GSSOfwzb0DQ | HIV - Key population - Person who inject drugs - VISITS                            |
| Data Element | iqXxZbRDhYG | HIV - Key population - People living in prisons and other closed settings - VISITS |
| Data Element | kICLXq7IEuP | HIV - Key population - Sex worker - VISITS                                         |
| Data Element | vZQyUQCKSc6 | HIV - Key population - Trans and gender-diverse people - VISITS                    |
| Data Element | kXutCUTOxcq | HIV - Key population - Men who have sex with men - VISITS                          |
| Data Element | GSSOfwzb0DQ | HIV - Key population - Person who inject drugs - VISITS                            |
| Data Element | iqXxZbRDhYG | HIV - Key population - People living in prisons and other closed settings - VISITS |
| Data Element | kICLXq7IEuP | HIV - Key population - Sex worker - VISITS                                         |
| Data Element | vZQyUQCKSc6 | HIV - Key population - Trans and gender-diverse people - VISITS                    |
| Program rule | Bj2Z3glSvHy | HIV - Assign value to Date of HIV positive diagnosis                               |
| Program rule | FNCVV9kG0oi | HIV - Assign value to Age when diagnosed with HIV                                  |
| Program rule | iLq9wgcYwAE | HIV - Assign value TRUE - Men who have sex with men                                |
| Program rule | yYGoGwZ612X | HIV - Assign value TRUE - Person who inject drugss                                 |
| Program rule | X51QRO65q4p | HIV - Assign value TRUE - people in prison or other closed settings                |
| Program rule | T3EGb8JMpRs | HIV - Assign value TRUE - Sex worker                                               |
| Program rule | zFTfjVAEY42 | HIV - Assign value TRUE - Trans and gender-diverse people                          |
| Program rule | mjrCXVhuYhe | HIV - Assign value FALSE - Men who have sex with men                               |
| Program rule | HxtlnaW2LHW | HIV - Assign value FALSE - Person who inject drugss                                |
| Program rule | TBzZvKdia6D | HIV - Assign value FALSE - people in prison or other closed settings               |
| Program rule | gs5hHXi3Q62 | HIV - Assign value FALSE - Sex worker                                              |
| Program rule | X4GW8qfrm0j | HIV - Assign value FALSE - Trans and gender-diverse people                         |

**HTS.1 People living with HIV who know their HIV status**

To be able to calculate this program indicator we need to use a CUSTOM period boundaries based on the ‘Date of HIV positive diagnosis’ as we want to count all the clients since the date of the diagnosis until either the current date, in case their still alive and in the cohort or the date of death / lost to follow-up.
To enable this counting the data element ‘HIV - Cohort date’ [ CrFaWOLSKiK] has been created and get assigned the value of the the current date in case the client is still alive / in the cohort (PLHIV) and the date of the death / lost to follow-up in case the client has dead or has been lost to follow-up.

| Metadata     | UID         | Name                                                       |
|--------------|-------------|------------------------------------------------------------|
| Data Element | CrFaWOLSKiK | HIV - Cohort date                                          |
| Program rule | ciDmWAbeFK4 | HIV - Assign today date if no change in the patient status |
| Program rule | f0dYm1jWn9B | HIV - Assign the death date if has value                   |
| Program rule | O3JX3rziDw8 | HIV - Assign the LTFU date if has value                    |

**ART.8 Appropriate second viral load test after adherence counselling**

In the numerator we need to calculate PLHIV on ART receiving a viral load test within three (3) months after a viral load test result of >=1000 copies/mL.
Data elements assigned with the date of the last viral load test conducted and the last result are used in the Program Indicator filter to compare the date of the last viral load test with the actual.

| Metadata     | UID         | Name                                     |
|--------------|-------------|------------------------------------------|
| Data Element | yCF5h2RdrOP | HIV - Treatment: Previous VL date        |
| Data Element | Cx6DHpEoRpb | HIV - Treatment: Previous VL value       |
| Program rule | dxPan4qsYWH | HIV - Assign previous VL date if present |
| Program rule | QxPGCFykSDT | HIV - HIV Previous viral load value      |

**DSD.2 Uptake of DSD ART models among people living with HIV**

In both numerator and denominator we need to calculate the number of people that are newly eligible for DSD ART models. Data element assigned with a TRUE value in case the client was not eligible in the previous visit but is eligible in the current one it’s used in the filter of the Program Indicator.

| Metadata     | UID         | Name                                                                                                                     |
|--------------|-------------|--------------------------------------------------------------------------------------------------------------------------|
| Data Element | w0s4IrXdJA1 | HIV - Treatment: Newly eligible for DSD ART                                                                              |
| Program rule | zCZ4RdA6QvL | HIV - Assign true to newly eligible for DSD ART if eligible in current visit and not eligible in the previous one        |
| Program rule | CKCZoiLk2MR | HIV - Assign false to newly eligible for DSD ART if eligible or not in current visit and/or eligible in the previous one |

Moreover in the numerator we need to count the ones newly enrolled in the DSD ART models.  Data element assigned with a TRUE value in case the client was not enrolled in the previous visit but is enrolled in the current one it’s used in the filter of the Program Indicator.

| Metadata     | UID         | Name                                                                                                                     |
|--------------|-------------|--------------------------------------------------------------------------------------------------------------------------|
| Data Element | Q7LmA2FMrzl | HIV - Treatment: Newly enrolled on DSD ART models                                                                        |
| Program rule | zNltrhsK3ZD | HIV - Assign true to newly enrolled for DSD ART if eligible in current visit and not enrolled in the previous one        |
| Program rule | ikzuYLNkxiV | HIV - Assign false to newly enrolled for DSD ART if eligible or not in current visit and/or enrolled in the previous one |

**DSD.5 Viral suppression among people living with HIV engaged in DSD ART models**

In both numerator and denominator of this indicator we need to count the PLHIV that are enrolled in DSD ART models and that have been tested for viral load (and are suppressed). Being a program indicator of type EVENT and having the information of viral load not directly linked with the enrollment or not in DSD ART models in the data model, a data element that is gonna be auto populated with the last available DSD ART enrollment status value (true or false) has been assigned to the program indicator

| Metadata     | UID         | Name                                           |
|--------------|-------------|------------------------------------------------|
| Data Element | PYFRaC4dJXg | HIV - Treatment: Last enrolled DSD ART status  |
| Program rule | zfcYNnMjDk6 | HIV - Assign DSD enrolled if present           |
| Program rule | ZgaUPJHLPvO | HIV - Assign last DSD enrolled if present      |
| Program rule | n0oipCCQK0x | HIV - Don't assign DSD enrolled if not present |

**DFT.3 TB testing among those symptom-screened positive**

The indicator need to count the number of PLHIV that, after being screened positive for TB, are tested for TB. We therefore need the date in which the last TB screening has been performed and we need to ensure that the screening has happened before the testing

| Metadata     | UID         | Name                                                           |
|--------------|-------------|----------------------------------------------------------------|
| Data Element | nWrIpSGAdVj | HIV - HIV/TB: Date screened positive for TB symptoms           |
| Program rule | jFviS9zNghE | HIV - Assign previous screening date if present                |
| Program rule | NqbC1zxf46M | HIV - Assign true if screened positive for TB in current event |

**TBH.3 TB diagnostic testing type**

In the numerator we need to count only the PLHIV that has received as first TB test a WHO approved rapid molecular TB test.
A data element reporting TRUE in case the first test has been with a mWRD (Molecular WHO Recommended rapid diagnostic) test is created and reported over the following events.

| Metadata     | UID         | Name                                                                                 |
|--------------|-------------|--------------------------------------------------------------------------------------|
| Data Element | dJJVB3fkwKd | HIV - HIV/TB: First TB test as mWRD                                                  |
| Program rule | WTONjptEnGe | HIV - Assign true to first test as mWRD and the date if has been the first test used |
| Program rule | WQaVtgqajwh | HIV - Assign false to first test as mWRD if has not been the first test used         |
| Program rule | LuBGQxFdoZ5 | HIV - Assign true value to first TB test as mWRD if previously tested as first       |

Moreover another data element reporting the date of the first TB test (if is a mWRD) is created and the value reported in the following events

| Metadata     | UID         | Name                                                                                 |
|--------------|-------------|--------------------------------------------------------------------------------------|
| Data Element | ay8M5cOVV6g | HIV - HIV/TB: First TB test as mWRD date                                             |
| Program rule | WTONjptEnGe | HIV - Assign true to first test as mWRD and the date if has been the first test used |
| Program rule | mTg0iaPC4BK | HIV - Assign previous value for First TB test as mWRD date if present                |

**STI.8	Repeat diagnosis of STI syndrome**

In the numerator we need to calculate the number of people attending HIV care and treatment services diagnosed with a particular STI syndrome two or more times. Data elements assigned with the last date in which a specific STI syndrome was reported are used in the PI filter to compare the date of the last diagnosis with the actual diagnosis; if the difference is lower than 12 months then our client will be counted. This is achieved with five (5) DEs assigned with “last diagnosis date” for each possible STI syndrome diagnosed.

| Metadata     | UID         | Name                                                                             |
|--------------|-------------|----------------------------------------------------------------------------------|
| Data Element | DI8O2sFAjIZ | HIV - STI: Urethral discharge syndrome last diagnosis date                       |
| Data Element | PMYnHPcLOrg | HIV - STI: Vaginal discharge syndrome last diagnosis date                        |
| Data Element | fGg6Qx8AzDw | HIV - STI: Lower abdominal pain last diagnosis date                              |
| Data Element | Nxjc5ElxwEj | HIV - STI: Genital ulcer disease syndrome last diagnosis date                    |
| Data Element | bEFauzOj4ov | HIV - STI: Anorectal discharge last diagnosis date                               |
| Program Rule | xgt6RIk6jJg | HIV - Assign the last date of urethral discharge syndrome if diagnosed           |
| Program Rule | RkeUbXxbAxf | HIV - Assign previous date of urethral discharge if no new diagnosis             |
| Program Rule | wKskErfCnRy | HIV - Assign the last date of vaginal discharge syndrome if diagnosed            |
| Program Rule | o9Je9CMIOOO | HIV - Assign previous date of vaginal discharge if no new diagnosis              |
| Program Rule | GMfpQZONK6N | HIV - Assign the last date of lower abdominal pain if diagnosed                  |
| Program Rule | vAtYaFpk9CI | HIV - Assign previous date of lower abdominal pain if no new diagnosis           |
| Program Rule | qJVXmzaqmCR | HIV - Assign the last date of genital ulcer disease syndrome if diagnosed        |
| Program Rule | vzE0J0fo7F4 | HIV - Assign previous date of genital ulcer disease syndrome if no new diagnosis |
| Program Rule | ES3S2qANfLw | HIV - Assign the last date of anorectal discharge if diagnosed                   |
| Program Rule | pW1lTs8KeDM | HIV - Assign previous date of anorectal discharge if no new diagnosis            |

**Vertical transmission indicators (VER.1 → VER.4)**

The vertical transmission indicators (form VER.1 to VER.4 included) need to reference the day of delivery. Being a program indicators of type EVENT a Data Element reporting the last delivery date has been created as the information needed for the calculation of the indicators VER.1 → VER.4 can be collected in different visit

| Metadata     | UID         | Name                                                      |
|--------------|-------------|-----------------------------------------------------------|
| Data Element | s2ITWwTvqC0 | HIV - PMTCT: Last delivery date                           |
| Program rule | z16QguGzYY6 | HIV - Assign the date of delivery if present              |
| Program rule | pgb8Pr2ldfM | HIV - Assign previous last delivery date if has any value |
| Program rule | aPpennvPlIL | HIV - Don't assign the date of delivery if not present    |

**HEP.7 HCV cured among people living with HIV**

In both numerator and denominator the program indicator used are EVENT type therefore we need to report the last HCV treatment status as the completion of the treatment and the test to confirm a sustained virological response can happen in different visits

| Metadata     | UID         | Name                                                   |
|--------------|-------------|--------------------------------------------------------|
| Data Element | GSK3oZtzrMS | HIV - Viral hepatitis: Last HCV treatment status       |
| Program rule | C6iTbpA4n46 | HIV - Assign treatment status if present               |
| Program rule | jC5pahYGKuz | HIV - Assign previous last treatment status if present |
| Program rule | o51CPtUmxsl | HIV - Don't assign treatment status if not present     |

**CCa.4 Cervical cancer survival**

In the numerator of this indicator we need to count the WLHIV with a diagnosis of cervical cancer that are still alive and a Data Element reporting the last diagnosis has been created.

| Metadata     | UID         | Name                                                        |
|--------------|-------------|-------------------------------------------------------------|
| Data Element | ntKnCH7bMGR | HIV - Cervical cancer: Cervical cancer last diagnosis       |
| Program rule | kSfwWuCTwOu | HIV - Assign cervical cancer diagnosis if present           |
| Program rule | juN7w5x3gKo | HIV - Assign last cervical cancer diagnosis if present      |
| Program rule | YmEXnBJNKFc | HIV - Don't assign cervical cancer diagnosis if not present |

### Program Indicators

**Program indicators** are used to aggregate individual-level data captured in the tracker data model; and map these to the aggregate data model for presentation, consumption and use in the DHIS2 analytics apps and dashboards. The use of the DHIS2 Data Exchange App for transferring program indicator values to aggregate data elements is described further in the section on Metadata Mapping & Data Exchange. 

Program indicators are organized into two program indicator groups: 
- *HIV Case surveillance - Data Exchange* [s7DBTTX68Lk] contains all PIs that are mapped to a corresponding target aggregate data elements for analysis. 
- *HIV Prevention - WHO standard list* [KKvBWmeIsvN] contains all PIs that are part of the WHO standard list of indicators (each PI typically represents a numerator or denominator from the SI guidelines standard list)

>**Warning**
>
>The majority of Program indicators are EVENT-based and use CUSTOM period boundaries requiring some adaptation. We have provided accurate representations of the program indicators based on the generic data model; however, It is the responsibility of the implementing organisation to decide what type of organisation unit dimension to assign to each program indicator, according to national guidelines and in consultation with end users. Please review the section on local adaptation and implementation considerations for more information on how to adjust program indicators and calculations during localization and country adaptation.

#### Key population filter in program indicators

As described in the section on *Hidden Data Elements & Assigned Values*, program rules are used to assign and copy the TEI’s key population group recorded in the initial case report to a hidden data element field on the repeatable Visit program stage. This allows for all program indicator calculations based on key population group to be configured, namely by using the auto-assigned data element as a filter in the program indicator. 

Note that in adjusting program indicators that filter based on Key Population group, you must use the set of data elements containing the postfix “- VISIT”:

- HIV - Key population - Men who have sex with men - VISITS [kXutCUTOxcq]
- HIV - Key population - Person who inject drugs - VISITS [GSSOfwzb0DQ]
- HIV - Key population - People living in prisons and other closed settings - VISITS [iqXxZbRDhYG]
- HIV - Key population - Sex worker - VISITS [kICLXq7IEuP]
- HIV - Key population - Trans and gender-diverse people - VISITS [vZQyUQCKSc6]

#### PLHIV currently on ART

Some program indicators are required to count the number of PLHIV currently on treatment either at the current date and/or for a specific reporting period.
To be able to allow this type of calculation we need to work on two different part of the program indicators formula:
- Period boundaries
- Filter

On the period boundaries section we need to use a combination of two different CUSTOM date as we want to count all the PLHIV from the moment in which they start the ART up to the last day with treatment. Therefore the date to use are:
- Before end of reporting period: Date of treatment initiation [Lv3c5VSA9t3]
- After the start of reporting period: Last day with ART [EGjmPoKhHpM]

On the filter side we need to be able to count the ones that still will have treatment after either the end of the reporting period or the current date as could be that the current date is in the period of analysis (eg. current date is 1st of November 2023 and we are analysing the ones still on ART in the 2023)
Hereunder a snapshot of the filter:

*(d2:daysBetween(V{analytics_period_end},#{Last day with ART})>=0
|| d2:daysBetween(V{current_date},#{Last day with ART})>=0)*

#### Notable Program Indicators configuration

**HTS.1 People living with HIV who know their HIV status**
To be able to calculate this program indicator of type ENROLLMENT we need to use a CUSTOM period boundaries based on the ‘Date of HIV positive diagnosis’ as we want to count all the clients since the date of the diagnosis until either the current date, in case their still alive and in the cohort or the date of death / lost to follow-up.
To enable this counting the data element ‘HIV - Cohort date’ [ CrFaWOLSKiK] has been created and get assigned the value of the the current date in case the client is still alive / in the cohort (PLHIV) and the date of the death / lost to follow-up in case the client has dead or has been lost to follow-up.
On this way the program indicator ‘HIV CS - HTS.1 People living with HIV who know their HIV status’ [HY5WGeOQPmC] has assigned the following period boundaries:
- Before end of reporting period: Date of HIV positive diagnosis
- After start of reporting period: Cohort date
The program indicator is of type ENROLLMENT as we want to know the information collected on the last visit (DHIS2 event)

**ART.8 Appropriate second viral load test after adherence counselling**
The numerator is a program indicator of type EVENT that needs to calculate the amount of PLHIV on ART receiving a viral load test within three (3) months after a viral load test result of >=1000 copies/mL.
Data elements assigned with the date of the last viral load test conducted and the last result are used in the Program Indicator filter to compare the date of the last viral load test with the actual; if the difference is lower than three (3) months and the previous viral load result is >=1000 copies/mL then our client will be counted.
In the period boundaries the CUSTOM period boundaries with the element of ‘Viral load test date’ [v4K5u8wftrq] need to be used as we want the value returned for the actual viral load conducted and not the one within three (3) months from the previous one

**DSD.2 Uptake of DSD ART models among people living with HIV**

To be able to calculate this indicator we need to count in both numerator and denominator (program indicators of type EVENT) the clients that are newly eligible for DSD ART models.
Data element assigned with a TRUE value in case the client was not eligible in the previous visit but is eligible in the current one it’s used in the filter of the Program Indicators.

Moreover in the numerator we need to count the ones newly enrolled in the DSD ART models.  Data element assigned with a TRUE value in case the client was not enrolled in the previous visit but is enrolled in the current one it’s used in the filter of the Program Indicator.

**DSD.5 Viral suppression among people living with HIV engaged in DSD ART models**

In both numerator and denominator of this indicator we need to count the PLHIV that are enrolled in DSD ART models and that have been tested for viral load (and are suppressed). Being a program indicator of type EVENT and having the information of viral load not directly linked with the enrollment or not in DSD ART models in the data model, a data element that is gonna be auto populated with the last available DSD ART enrollment status value (true or false) has been assigned to the program indicator

**TBH.3 TB diagnostic testing type**

In the numerator we need to count only the PLHIV that has received as first TB test a WHO approved rapid molecular TB test.
A data element reporting TRUE in case the first test has been with a mWRD (Molecular WHO Recommended rapid diagnostic) test is created and reported over the following events.
Moreover another data element reporting the date of the first TB test (if is a mWRD) is created and the value reported in the following events

**STI.8	Repeat diagnosis of STI syndrome**
The reporting period of the indicator has been set-up as the “last 12 months” therefore the output will be the proportion of clients diagnosed with a particular STI syndrome two or more times within the last 12 months.

In the numerator we need to calculate the number of people attending HIV prevention services diagnosed with a particular STI syndrome two or more times. This filter relies on the hidden data elements assigned with values for ‘last diagnosis date’:
- HIV - STI: Urethral discharge syndrome last diagnosis date [DI8O2sFAjIZ]
- HIV - STI: Vaginal discharge syndrome last diagnosis date [PMYnHPcLOrg]
- HIV - STI: Lower abdominal pain last diagnosis date [fGg6Qx8AzDw]
- HIV - STI: Genital ulcer disease syndrome last diagnosis date [Nxjc5ElxwEj]
- HIV - STI: Anorectal discharge last diagnosis date [bEFauzOj4ov]

In case there is the need to add/delete any STI syndrome according to the implementation needs, the data model needs to be replicated for additional STI diagnosis; or removed from PI calculations for STI diagnoses not relevant.

**Vertical transmission indicators (VER.1 → VER.4)**

The vertical transmission indicators (form VER.1 to VER.4 included) need to reference the day of delivery. Being a program indicators of type EVENT a Data Element reporting the last delivery date has been created as the information needed for the calculation of the indicators VER.1 → VER.4 can be collected in different visit.
In the period boundaries for all the program indicators we therefore need to use the following Data Element: *HIV - PMTCT: Last delivery date*
As well on the indicator *VER.2  Early infant diagnosis (EID) coverage* we have to use the Data Element *HIV - PMTCT: Last delivery date* as the information related to HIV-exposed infant test is collected in a following visit

**HEP.7 HCV cured among people living with HIV**

In both numerator and denominator the program indicator used are EVENT type therefore we need to report the last HCV treatment status as the completion of the treatment and the test to confirm a sustained virological response can happen in different visits.
In both numerator and denominator we need to filter the treatment status by the following Data Element:  *HIV - Viral hepatitis: Last HCV treatment status*

**CCa.4 Cervical cancer survival**

In the numerator of this indicator we need to count the WLHIV with a diagnosis of cervical cancer that are still alive and the Data Element *HIV - Cervical cancer: Cervical cancer last diagnosis* [ntKnCH7bMGR] reporting the last diagnosis has been created.
This Data Element *HIV - Cervical cancer: Cervical cancer last diagnosis* is used in the filter and has the same Option Sets as the Data Element *HIV - Cervical cancer: Cervical cancer diagnosis*

## Metadata mapping & data exchange

As described above, all the dashboards and indicators are supported by an aggregate data model. This allows for mixed methods of reporting from various electronic and paper-based sources, enabling all data to be brought together for the purpose of analysis and use such as in an integrated HMIS. 

Where target aggregate data elements can be populated by aggregating the underlying Tracker data from the HIV Case Surveillance tracker, we have pre-configured a set of program indicators and included the mapping of the target aggregate dimensions (data element UID and Category Option Combination UID). 

See [DHIS2 User documentation on how to use the Data Exchange App](https://docs.dhis2.org/en/use/user-guides/dhis-core-version-240/exchanging-data/data-exchange.html). 

See [DHIS2 Developer documentation on the DHIS2 APIs for aggregate data exchange](https://docs.dhis2.org/en/develop/using-the-api/dhis-core-version-240/data-exchange.html?h=aggregate+data+exchange+2.40#create-aggregate-data-exchange) for more information.

The metadata has been aligned to the data dictionaries and indicator references published in the WHO’s Digital adaptation kit (DAK) for HIV, second edition (see Web Annex A of the digital adaptation kit for the data dictionary). Note that the Tracker not designed to support all aspects of clinical care guidelines and case management, some of which is contained in the HIV DAK, nor to replace robust facility EMRs; however, data from EMRs can be consumed into the national HIV registry for analysis and use.

Here you can find the mapping between the DHIS2 metadata and the HIV DAK data dictionary.

## Implementation Considerations & Local Adaptation

This chapter describes some of the possibilities for adapting the configuration for local context and needs, as well as implementation considerations that are important for the HIV Case Surveillance use case.

### Data Privacy & Confidentiality

Collecting data on key populations poses certain challenges, particularly when information is linked or shared across service providers and programmes.  All individual-level health data, including those of key populations, must be classified as sensitive personal data, or personally identifiable information, that require a high standard of safety and security. All health information systems must have robust data security and confidentiality protocols in place to safeguard data, supported by laws and policies that protect health information. The processing of personal health data must address cybersecurity, trust building, accountability and governance, ethics, equity, capacity building and digital literacy. 

Where safety and the potential to discourage access to services are a concern, the routine collection of key population information is not advised; and should be removed from the digitized DHIS2 Case Surveillance Tracker Program. Similarly, before capturing personally identifiable data such as client name, birth date and other indirectly identifiable data, a risk assessment and security review of the electronic systems and SOPs for all users must be reviewed and gaps addressed. 

### Tracker structure adaptation

The HIV Case Surveillance tracker structure is mostly a flat structure with only one main repeatable stage to record activities for any type of prevention visit. This intentionally simplified structure allows for increased flexibility for local adaptation and customization. For example: 
- Sections can be easily translated in stages in case different user have to entered different information (as detailed previously the HIV Case Surveillance service are very transversal and can have several actors involved on the distribution of the services)
- Reduced amount of program rules and well identified by the section targeted
- If a specific activity (HIV care intervention, such as cervical cancer) is not relevant to your country’s package of HIV care and treatment services offered, you can simply remove an entire section without repercussions on the rest of the data model. 

#### Integration of HIV Case Surveillance and HIV Prevention tracker

Depending on local context, some implementations may wish to integrate prevention and case surveillance into a single DHIS2 Tracker Program. An integrated tracker design option was considered for the global design guide, but ultimately discarded in favor of two Tracker programs for the following reasons:
- Scale: the number of persons (TEIs) enrolled in a prevention program is likely to be much, much greater than the number of persons (TEIs) diagnosed as positive and enrolled in a Case Surveillance program. This may have an impact on performance and also requires adequate server infrastructure for large-scale Tracker deployment.
- Types of service providers: in many country contexts, a large scale and variety of service providers may be engaged in HIV prevention services, such as community health workers, NGOs/CSOs as implementing partners, etc. Often these prevention service providers are not responsible for following up the HIV case through lifelong treatment and should not have access to the client’s health data after being identified as an HIV positive client. Therefore, separating the Tracker programs suits the variety and helps to ensure protection & data confidentiality of people living with HIV. 
- Indicators included in the WHO SI guidelines and DAK do not necessitate ‘cross-program’ analysis; all indicators can be generated within DHIS2 using two separate programs.
- A typical workflow from prevention services to HIV case surveillance is well supported by closing the enrollment in a DHIS2 prevention tracker program and opening a new enrollment in the case surveillance program. The person (TEI) is linked to both Prevention and Case Surveillance programs in case ad hoc analysis will be required for advanced programmatic data analysis conducted outside of DHIS2. 

An integrated design can be implemented leveraging many of the existing DHIS2 metadata, such as the Tracker data element library. Both Tracker programs follow a similar structure with initial case report and integrated, repeatable ‘visit’ stage for all follow-up thereafter. 
Tracker metadata that are common between the HIV prevention and HIV case surveillance trackers are shared between the two programs (using the same UIDs) as follows:
- All Tracked Entity Attributes: all the TEAs are the same
- Initial case report: the Data Elements related to the Key population identification are the same
- Visit stage:
  - STI data elements are the same
  - Viral hepatitis: Data Elements related to the testing, date and result are the same 

![Structure for an integrated Prevention/Case Surveillance Tracker Program](/resources/images/integrated_prev_cs_structure.jpg)

### Analytics & Indicator Calculations

#### Ownership Analytics

From DHIS2 v40, new functionality is available that allows for program indicator calculations to be made based on the ‘ownership’ org unit of a given enrollment. For example, you can count the number of unique clients that are receiving HIV prevention services at a facility even if they were not originally enrolled in care at that facility or officially transferred.

Program indicators can define which **organisation unit dimension** to be used in analytics. Choices include the organisation unit for the event, enrollment, registration, organisation unit data element and tracked entity instance ownership at the start or end of the reporting period. Ownership analytics apply only to program indicators configured *based on enrollment*.

More detailed guidance about Ownership Analytics can be found in the [Tracker Design Guide](https://docs.dhis2.org/en/implement/database-design/tracker-system-design/tracker-analytics.html#common-challenge-3-transfers-and-ownership).

>**Note:**
>
>for implementations using DHIS2 software version 2.39 or lower, program indicators generated at any level below national will not reflect the current ‘ownership’ org unit. This means that if a TEI (person) who has been permanently transferred from one organisation unit (Health Facility A) to another (Health Facility B) for a given Tracker program enrollment *will still be counted as belonging to Health Facility A* for the purposes of program indicators. In practical terms, this means that when a patient has moved between sites, all longitudinal indicators for this patient are not re-assigned to the patient’s latest site, and remain assigned to the initial site where the patient first entered the program. This is a critical limitation for typical HIV use cases, where site level performance metrics such as lost-to-follow-up must be generated accurately at a subnational or facility level. If your implementation uses DHIS2 v2.39 or lower, please review your configuration carefully to see if you have accounted for the exclusion of persons ‘transferred out’ based on the org unit enrollment ownership.

#### Event date vs Custom date

As previously described, a set of dates are requested for all these activities that can be carried out beyond an HIV prevention service but which information is still relevant for a comprehensive, person-centred management and reporting system.

The large majority of Program Indicators use this date in both period boundaries (as CUSTOM date) and/or in the filter. In case the implementation decide to use the event date (‘visit date’) as the default date for any of the activities, this will need to be reflected as well on the Program indicators

#### Tracker Data Analysis

For authorised users with access to personally identifiable data captured through the HIV Case Surveillance tracker program, additional ad hoc analysis of tracker data can be achieved using the Line-list app and configuring working lists as part of the DHIS2 tracker program (note: only for users that have access to the Capture app for data collection). 

In general, it is recommended to serve routine analytics for program monitoring through the aggregate data model, using program indicators and data exchange services, as implemented in this design. In part, this is because the aggregate data model in DHIS2 provides optimised dimensionality for analysis (e.g. the ability to slice and filter by Categories & CatCombos in Data Visualizer app). This also allows for more performant analytics, as running queries on large databases containing years worth of individual data can create unnecessary stress on the server. 

Ad hoc analyses and individual level mining is typically restricted only to authorised public health users (such as HIV programme data managers) for advanced analyses; and users at the service delivery point (facility) for conducting operational follow up activities, contacting clients, or performing basic routine data quality checks & analyses on the individual level data they are responsible for collecting.

### Operationalizing variables for key populations

Some countries have experienced difficulties defining whether people seeking services belong to a given key population, considering that disclosure may put them at risk for discrimination, stigmatization and, in certain contexts, legal punity.

To facilitate the operationalization of key population definitions, the following table from the “[Framework for Monitoring HIV/STI Services for Key Populations in Latin America and the Caribbean](https://www.paho.org/en/documents/framework-monitoring-hivsti-services-key-populations-latin-america-and-caribbean-2019)” offers a sensitive and specific tool to standardise the collection of information through a minimum set of variables which allows for the categorization of persons into key population groups. This tool can be integrated into the DHIS2 Prevention Tracker by configuring an additional set of data elements that follow the structure of the questionnaire:

![PAHO Questionnaire for operationalizing key population variables](resources/images/PAHO_questions_key_pop.png)

According to the data collected from the questionnaire, the client can be identified in one or more groups due to the overlapping practices and vulnerabilities. A country may have other key population and definitions; therefore, it is expected that the tool should be adapted according to the national context. The assignment of a person to a given key population group can be achieved using program rules to assign a value to the core data elements for Key Population groups included in the DHIs2 prevention tracker metadata.

![PAHO interpreting responses for monitoring and data analysis](resources/images/PAHO_key_pop_answers.png)

## References

WHO (2022). Consolidated guidelines on person-centred HIV strategic information: strengthening routine data for impact [https://www.who.int/publications/i/item/9789240055315](https://www.who.int/publications/i/item/9789240055315)

UNAIDS (2022) IN DANGER: UNAIDS Global AIDS Update 2022. Geneva: Joint United Nations Programme on HIV/ AIDS [https://www.unaids.org/en/resources/documents/2022/in-danger-global-aids-update](https://www.unaids.org/en/resources/documents/2022/in-danger-global-aids-update)
