# HIV Case Surveillance Tracker Installation Guide { #hiv-cs-installation }

Package Version 2.0.0

System default language: English

## Installation

Installation of the module consists of several steps:

1. [Preparing](#preparing-the-metadata-file) the metadata file.
2. [Importing](#importing-metadata) the metadata file into DHIS2.
3. [Configuring](#configuration) the imported metadata.
4. [Adapting](#adapting-the-tracker-program) the program after being imported

It is recommended to first read through each section of the installation guide before starting the installation and configuration process in DHIS2. Identify applicable sections depending on the type of your import:

1. Import into a blank DHIS2 instance
2. Import into a DHIS2 instance with existing metadata (No other versions of HIV Case Surveillance tracker imported previously).
3. Update existing/older version of the HIV Case Surveillance tracker.

The steps outlined in this document should be tested in a test/staging DHIS2 instance and only then applied to a production environment.

## Requirements

In order to install the module, an administrator user account on DHIS2 is required. The procedure outlined in this document should be tested in a test/staging environment before being performed on a production instance of DHIS2.

Great care should be taken to ensure that the server itself and the DHIS2 application is well secured, to restrict access to the data being collected. Details on securing a DHIS2 system is outside the scope of this document, and we refer to the [DHIS2 documentation](http://dhis2.org/documentation).

## Preparing the metadata file

**NOTE:** If you are installing the package on a new instance of DHIS2, you can skip the “Preparing the metadata file” section and move immediately to the section [Importing a metadata file into DHIS2](#importing-metadata)

While not always necessary, it can often be advantageous to make certain modifications to the metadata file before importing it into DHIS2.

### Default data dimension (if applicable)

In early versions of DHIS2, the UID of the default data dimension was auto-generated. Thus, while all DHIS2 instances have a default category option, data element category, category combination and category option combination, the UIDs of these defaults can be different. Later versions of DHIS2 have hardcoded UIDs for the default dimension, and these UIDs are used in the configuration packages.

To avoid conflicts when importing the metadata, it is advisable to search and replace the entire .json file for all occurrences of these default objects, replacing UIDs of the .json file with the UIDs of the database in which the file will be imported. Table 1 shows the UIDs which should be replaced, as well as the API endpoints to identify the existing UIDs.

| Object                      | UID         | API endpoint                                              |
|:----------------------------|:------------|:----------------------------------------------------------|
| Category                    | GLevLNI9wkl | `../api/categories.json?filter=name:eq:default`           |
| Category option             | xYerKDKCefk | `../api/categoryOptions.json?filter=name:eq:default`      |
| Category combination        | bjDvmb4bfuf | `../api/categoryCombos.json?filter=name:eq:default`       |
| Category option combination | HllvX50cXC0 | `../api/categoryOptionCombos.json?filter=name:eq:default` |

For example, if importing a configuration package into <https://play.dhis2.org/demo>, the UID of the default category option combination could be identified through <https://play.dhis2.org/demo/api/categoryOptionCombos.json?filter=name:eq:default> as bRowv6yZOF2.

You could then search and replace all occurrences of HllvX50cXC0 with bRowv6yZOF2 in the .json file, as that is the ID of default in the system you are importing into. **_Note that this search and replace operation must be done with a plain text editor_**, not a word processor like Microsoft Word.

### Indicator types

Indicator type is another type of object that can create import conflict because certain names are used in different DHIS2 databases (.e.g "Percentage"). Since Indicator types are defined simply by their factor and whether or not they are simple numbers without a denominator, they are unambiguous and can be replaced through a search and replace of the UIDs. This avoids potential import conflicts, and avoids creating duplicate indicator types. Table 2 shows the UIDs which could be replaced, as well as the API endpoints to identify the existing UIDs

| Object     | UID         | API endpoint                                                             |
|:-----------|:------------|:-------------------------------------------------------------------------|
| Percentage | hmSnCXmLYwt | `../api/indicatorTypes.json?filter=number:eq:false&filter=factor:eq:100` |
| Rate (factor=1)| k4RGC3sMTzO | `../api/indicatorTypes.json?filter=number:eq:false&filter=factor:eq:1`|
| Per 10 000 | FWTvArgP0jt | `../api/indicatorTypes.json?filter=number:eq:false&filter=factor:eq:10000`|
| Numerator only (number) | kHy61PbChXr | `..api/indicatorTypes.json?filter=number:eq:true`|

### Tracked Entity Type

Like indicator types, you may have already existing tracked entity types in your DHIS2 database. The references to the tracked entity type should be changed to reflect what is in your system so you do not create duplicates. Table 3 shows the UIDs which could be replaced, as well as the API endpoints to identify the existing UIDs

| Object | UID         | API endpoint                                           |
|:-------|:------------|:-------------------------------------------------------|
| Person | MCPQUTHX1Ze | `../api/trackedEntityTypes.json?filter=name:eq:Person` |

### Visualizations using Root Organisation Unit UID

Visualizations, event reports, report tables and maps that are assigned to a specific organisation unit level or organisation unit group, have a reference to the root (level 1) organisation unit. Such objects, if present in the metadata file, contain a placeholder `<OU_ROOT_UID>`. Use the search function in the .json file editor to possibly identify this placeholder and replace it with the UID of the level 1 organisation unit in the target instance.

Some visualizations and maps may contain references to organisation unit levels. Maps that consist of several map views may contain various Organisation unit level references based on the configuration of the map layer. Adjust the organisation unit level references in the metadata json file to match the organisation unit structure in the target instance before importing the metadata file.

### Upgrading metadata package

The process of upgrading an existing package to a newer version in a working DHIS2 instance is a complex operation that has to be taken with precaution. Such process has to be run in a staging instance first, before upgrading the configuration on the production server. As metadata objects may have been removed, added or changed, it is important to ensure that:

   - the format of existing data can be mapped and adjusted to the new configuration;
   - the discontinued metadata objects are deleted from the instance; 
   - The existing objects are updated;
   - the new objects are created;
   - assignment of users to relevant user groups is reviewed.

## Importing metadata

The .json metadata file is imported through the [Import/Export](https://docs.dhis2.org/master/en/user/html/import_export.html) app of DHIS2. It is advisable to use the "dry run" feature to identify issues before attempting to do an actual import of the metadata. If "dry run" reports any issues or conflicts, see the [import conflicts](#handling-import-conflicts) section below.

If the "dry run"/"validate" import works without error, attempt to import the metadata. If the import succeeds without any errors, you can proceed to [configure](#configuration) the module. In some cases, import conflicts or issues are not shown during the "dry run", but appear when the actual import is attempted. In this case, the import summary will list any errors that need to be resolved.

### Handling import conflicts

> NOTE: If you are importing into a new DHIS2 instance, you will not have to worry about import conflicts, as there is nothing in the database you are importing to conflict with. Follow the instructions to import the metadata then please proceed to the “[Configuration](#configuration)” section.

There are a number of different conflicts that may occur, though the most common is that there are metadata objects in the configuration package with a name, shortname and/or code that already exists in the target database. There are a couple of alternative solutions to these problems, with different advantages and disadvantages. Which one is more appropriate will depend, for example, on the type of object for which a conflict occurs.

#### Alternative 1

Rename the existing object in your DHIS2 database for which there is a conflict. The advantage of this approach is that there is no need to modify the .json file, as changes are instead done through the user interface of DHIS2. This is likely to be less error prone. It also means that the configuration package is left as is, which can be an advantage for example when training material and documentation based on the configuration package will be used.

#### Alternative 2

Rename the object for which there is a conflict in the .json file. The advantage of this approach is that the existing DHIS2 metadata is left as-is. This can be a factor when there is training material or documentation such as SOPs of data dictionaries linked to the object in question, and it does not involve any risk of confusing users by modifying the metadata they are familiar with.

Note that for both alternative 1 and 2, the modification can be as simple as adding a small pre/post-fix to the name, to minimise the risk of confusion.

#### Alternative 3

A third and more complicated approach is to modify the .json file to re-use existing metadata. For example, in cases where an option set already exists for a certain concept (e.g. "sex"), that option set could be removed from the .json file and all references to its UID replaced with the corresponding option set already in the database. The big advantage of this (which is not limited to the cases where there is a direct import conflict) is to avoid creating duplicate metadata in the database. There are some key considerations to make when performing this type of modification:

* it requires expert knowledge of the detailed metadata structure of DHIS2
* the approach does not work for all types of objects. In particular, certain types of objects have dependencies which are complicated to solve in this way, for example related to disaggregations.
* future updates to the configuration package will be complicated.

## Configuration

Once all metadata has been successfully imported, there are a few steps that need to be taken before the module is functional.

### Sharing

First, you will have to use the _Sharing_ functionality of DHIS2 to configure which users (user groups) should see the metadata and data associated with the programme as well as who can register/enter data into the program. By default, sharing has been configured for the following:

* Tracked entity type
* Program
* Program stages
* Dashboards

A metadata package usually contains several user groups:

* HIV CS - Access
* HIV CS - Admin
* HIV CS - Data capture

By default, the following is assigned to these user groups

| Object              | User Groups                             |                                                  |                                                     |
|---------------------|-----------------------------------------|--------------------------------------------------|-----------------------------------------------------|
|                     | HIV CS - Access                         | HIV CS - Admin                                   | HIV CS - Data capture                               |
| Tracked entity type | Metadata : can view <br> Data: can view | Metadata : can edit and view <br> Data: can view | Metadata : can view <br> Data: can capture and view |
| Program             | Metadata : can view <br> Data: can view | Metadata : can edit and view <br> Data: can view | Metadata : can view <br> Data: can capture and view |
| Program Stages      | Metadata : can view <br> Data: can view | Metadata : can edit and view <br> Data: can view | Metadata : can view <br> Data: can capture and view |
| Dashboards          | Metadata : can view <br> Data: can view | Metadata : can edit and view <br> Data: can view | Metadata : can view <br> Data: can view             |

You will want to assign your users to the appropriate user group based on their role within the system. You may want to enable sharing for other objects in the package depending on your set up. Refer to the [DHIS2 Documentation](https://docs.dhis2.org/master/en/dhis2_user_manual_en/about-sharing-of-objects.html) for more information on configuring sharing.

### User Roles

Users will need user roles in order to engage with the various applications within DHIS2. The following minimum roles are recommended:

1. Tracker data analysis : Can see event analytics and access dashboards, event reports, event visualizer, data visualizer, pivot tables, reports and maps.
2. Tracker data capture : Can add data values, update tracked entities, search tracked entities across org units and access tracker capture

Refer to the [DHIS2 Documentation](http://dhis2.org/documentation) for more information on configuring user roles.

### Organisation Units

You must assign the program to organisation units within your own hierarchy in order to be able to see the program in tracker capture.

### Visualizations using Organisation Unit Group

There are visualizations, event reports, report tables and maps that have a reference to a specific organisation unit group called 'HMIS: All Health Facilities' with the UID 'zk3lBJfnL6b'. You must assign the organisation units that are health facilities within your own hierarchy into it in order to be able to see the correct visualizations.

### Duplicated metadata

> **NOTE**
>
> This section only applies if you are importing into a DHIS2 database in which there is already meta-data present. If you are working with a new DHIS2 instance, please skip this section and go to [Adapting the tracker program](#adapting-the-tracker-program).
> If you are using any third party applications that rely on the current metadata, please take into account that this update could break them”

Even when metadata has been successfully imported without any import conflicts, there can be duplicates in the metadata - data elements, tracked entity attributes or option sets that already exist. As was noted in the section above on resolving conflict, an important issue to keep in mind is that decisions on making changes to the metadata in DHIS2 also needs to take into account other documents and resources that are in different ways associated with both the existing metadata, and the metadata that has been imported through the configuration package. Resolving duplicates is thus not only a matter of "cleaning up the database", but also making sure that this is done without, for example, breaking potential integrating with other systems, the possibility to use training material, breaking SOPs etc. This will very much be context-dependent.

## Adapting the tracker program

Once the programme has been imported, you might want to make certain modifications to the programme. Examples of local adaptations that _could_ be made include:

* Adding additional variables to the form.
* Adapting data element/option names according to national conventions.
* Adding translations to variables and/or the data entry form.
* Modifying program indicators based on local case definitions.

However, it is strongly recommended to take great caution if you decide to change or remove any of the included form/metadata. There is a danger that modifications could break functionality, for example program rules and program indicators.
