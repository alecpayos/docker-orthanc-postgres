# Dockerized NOOP

This project aims to narrow down the unsupported DICOM modalities \
and SOP Class UIDs when these studies are uploaded into Orthanc.

This project uses the following docker images:
  - N - nginx:latest
  - O - orthancteam/orthanc
  - O - ohif/app:latest
  - P - postgres:14

PS: Currently this should only be installed locally be the user.

## Installation

1. Clone the repo
2. Run docker-compose up

Once the steps above are done, you should now be able to immediately \
use the apps. Follow the usage section below.

## Usage

These 3 are the main endpoints you can use to view, upload, and search \
for dicom data. None of the apps can be developed with this repo as we \
have used their corresponding Docker Images for this project.

Immediately after running the installation you should be able to click \
on the links below. You should be able to see them working.

**OHIF Viewer** \
*http://localhost:3000/*

**Orthanc Explorer V2** \
*http://localhost:8042/ui/app/settings#/*

**DICOM STOW-RS** \
*http://localhost:4242*

## Supported DICOM Modalities and Media Storage SOP Class UIDs

There is currently 1 study proven to be supported by this project.

For a complete list of the UIDs, refer to: \
https://dicom.nema.org/dicom/2013/output/chtml/part04/sect_i.4.html

For the more in-depth look at the Orthanc-emf2sf support, see: \
[Support and unsupported modalities and SOP Class UIDs](./orthanc-emf2sf-support.md)

## Tables In PostgreSQL

These are the following tables created by Orthanc. \
These store all values related to the study's: Metadata, Study size, etc...
 - attachedfiles
 - changes
 - dicomidentifiers
 - exportedresources
 - globalintegers
 - globalintegerschanges
 - globalproperties
 - labels
 - maindicomtags
 - metadata
 - patientrecyclingorder
 - resources
 - serverproperties
 - storagearea

## Local OrthancStorage Configuration

Should you wish to change the OrthancStorage from postgres to local sqlite \
Add these to the `plugins.json` file just below the name prop (Line 2).

```
"StorageDirectory": "/var/lib/orthanc/storage/",
"IndexDirectory": "/var/lib/orthanc/index/",
"TemporaryDirectory": "/tmp/Orthanc/",
```

And set the `EnableStorage` of the `PostgreSQL` plugin to false (Line 94).

```
"EnableStorage" : true,
```

## OHIF Datasource Configuration

Add this to the datasource configuration of the OHIF Viewer. \
The `default.js` file can be found in `platform/app/public/config`. \
Append the code below into the ***dataSources*** array (Line 35).

```
{
  friendlyName: 'Orthanc Server',
  namespace: '@ohif/extension-default.dataSourcesModule.dicomweb',
  sourceName: 'dicomweb',
  configuration: {
    name: 'Orthanc',
    wadoUriRoot: 'http://localhost/wado',
    qidoRoot: 'http://localhost/dicom-web',
    wadoRoot: 'http://localhost/dicom-web',
    qidoSupportsIncludeField: false,
    imageRendering: 'wadors',
    thumbnailRendering: 'wadors',
  },
}
```

## Disclaimer

This is a personal project of mine. Any updates or breakages are not directly accountable to me. \
Visit the links below to know more about support regarding the docker images.

**Docker**
 - https://hub.docker.com/support/contact/

**Nginx**
 - https://www.nginx.com/support/
 - https://docs.nginx.com/nginx-management-suite/support/contact-support/

**Orthanc**
 - https://orthanc.uclouvain.be/book/users/support.html

**OHIF Viewer**
 - https://github.com/OHIF/Viewers/issues/
 - https://groups.google.com/g/cornerstone-platform

**PostgreSQL**
 - https://www.postgresql.org/docs/current/bug-reporting.html
 - https://www.postgresql.org/account/login/?next=/account/submitbug/