# isric instance

- forked from https://github.com/soilwise-he/link-liveliness-assessment
- add gitlab ci script

# OGC API - Records; link liveliness assessment

A component which evaluates for a set of metadata records (describing either data or knowledge sources), if:

- the links to external sources are valid
- the links within the repository are valid
- link metadata represents accurately the resource

The component either returns a http status: 200 (ok), 403 (non autorized), 404 (not found), 500 (error), ...
Status 302 is forwarded to new location and the test is repeated.

The component runs an evaluation for a single resource at request, or runs tests at intervals providing a history of availability 

A link either points to:

- another metadata record
- a downloadable instance (pdf/zip/sqlite) of the resource
- an API

If endpoint is API, some sanity checks can be performed on the API:

- Identify if the API adopted any API-standard
- IF an API standard is adopted, does the API support basic operations of that API

The benefit of latter is that it provides more information then a simple ping to the index page of the API, typical examples of standardised API's are SOAP, GraphQL, SPARQL, OpenAPI, WMS, WFS

## OGC API - records

OGC is in the process of adopting the [OGC API - Records](https://github.com/opengeospatial/ogcapi-records) specification. A standardised API to interact with Catalogues. The specification includes a datamodel for metadata. This tool assesses the linkage section of any record in an OGC API - Records.


## Source Code Brief Desrciption

Running the linkchecker.py will utilize the requests library from python to get the relevant EJP Soil Catalogue source.
Run the command below
* python linkchecker.py
The urls selected from the requests will passed to linkchecker using the proper options.
The output generated will be written to a PostgreSQL database.
A .env is required to define the database connection parameters.
More specifically the following parameters must be specified

```
    POSTGRES_HOST=
    POSTGRES_PORT=
    POSTGRES_DB=
    POSTGRES_USER=
    POSTGRES_PASSWORD=
```

## API
The api.py file creates a FastAPI in order to retrieve links statuses. 
Run the command below
* python -m uvicorn api:app --reload --host 0.0.0.0 --port 8000 
To view the service of the FastAPI on [http://127.0.0.1:8000/docs]

# Docker
A Docker instance must be running for the linkchecker command to work.

## CI/CD
A workflow is provided in order to run it as a cronological job once per week every Sunday Midnight
(However currently it is commemended to save running minutes since it takes about 80 minutes to complete)
It is necessary to use the **secrets context in gitlab in order to be connected to database

## Roadmap

### GeoHealthCheck integration

[GeoHealthCheck](https://GeoHealthCheck.org) is a component to monitor livelyhood of typical OGC services (WMS, WFS, WCS, CSW). It is based on the [owslib](https://owslib.readthedocs.io/en/latest/) library, which provides a python implementation of various OGC services clients.

## Soilwise-he project

This work has been initiated as part of the [Soilwise-he project](https://soilwise-he.eu/).
The project receives funding from the European Union’s HORIZON Innovation Actions 2022 under grant agreement No. 101112838.

