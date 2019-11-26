<img src="https://github.com/radiantearth/stac-site/raw/master/images/logo/stac-030-long.png" alt="stac-logo" width="700"/>

[![CircleCI](https://circleci.com/gh/radiantearth/stac-spec.svg?style=svg)](https://circleci.com/gh/radiantearth/stac-spec)

## About

The SpatioTemporal Asset Catalog (STAC) specification aims to standardize the way geospatial assets are exposed online and queried. 
A 'spatiotemporal asset' is any file that represents information about the earth captured in a certain space and 
time. The initial focus is primarily remotely-sensed imagery (from satellites, but also planes, drones, balloons, etc), but 
the core is designed to be extensible to SAR, full motion video, point clouds, hyperspectral, LiDAR and derived data like
NDVI, Digital Elevation Models, mosaics, etc. 

The goal is for all major providers of imagery and other earth observation data to expose their data as SpatioTemporal Asset 
Catalogs, so that new code doesn't need to be written whenever a new JSON-based REST API comes out that makes its data 
available in a slightly different way. This will enable standard library components in many languages. STAC can also be
implemented in a completely 'static' manner, enabling data publishers to expose their data by simply publishing linked JSON
files online.

## WARNING

The specification is currently still an early version, with the potential for some major things to change. The core is now
fleshed out, so implementors are encouraged to try it out and give feedback. But the goal is to actually be able to act
on that feedback, which will mean changes are quite possible. 

But efforts will be made to maintain the core fields established in the central [Item spec](item-spec/), 
[Catalog spec](catalog-spec/) and [Collection spec](collection-spec/).
The minimal amount is specified right now, but best practices should emerge with implementation and more will likely be specified.

## Current version and branches

The [master branch](https://github.com/radiantearth/stac-spec/tree/master) is the 'stable' version of the spec. It is currently version 
**0.8.1** of the specification. The 
[dev](https://github.com/radiantearth/stac-spec/tree/dev) branch is where active development takes place, and may have inconsistent examples. 
Whenever dev stabilizes a release is cut and we merge dev in to master. So master should be stable at any given time.
It is possible that there may be small releases in quick succession, especially if they are nice improvements that do 
not require lots of updating. More information on how the STAC development process works can be found in 
[process.md](process.md).

## Communication

For any questions feel free to jump on our [gitter channel](https://gitter.im/SpatioTemporal-Asset-Catalog/Lobby) or email 
our [google group](https://groups.google.com/forum/#!forum/stac-spec). The majority of communication about the evolution of 
the specification takes place in the [issue tracker](https://github.com/radiantearth/stac-spec/issues) and in 
[pull requests](https://github.com/radiantearth/stac-spec/pulls).

## In this Repository

This repository contains the core specifications plus examples and validation schemas and tools. Also included are a
few documents that provide more context and plans for the evolution of the specification. Each spec folder contains a
README explaining the layout of the folder, the main specification document, examples, validating schemas and OpenAPI
documents (if relevant). The four specifications detailed are meant to be used together, but are designed so each piece
is small, self-contained and reusable in other contexts.

**[item-spec/](item-spec/)** defines a STAC Item, which is a [GeoJSON](http://geojson.org) Feature
with additional fields for things like time, links to related entities and assets (including thumbnails). This is the 
atomic unit that describes the data to be discovered.

**[catalog-spec/](catalog-spec/)** specifies a structure to link various STAC Items together to be crawled or browsed. It is a
simple, flexible JSON file of links to Items, Catalogs or Collections that can be used in a variety of ways.

**[collection-spec/](collection-spec/)** provides additional information about a spatio-temporal collection of data.
In the context of STAC it is most likely a collection of STAC Items that is made available by a data provider.
It includes things like the spatial and temporal extent of the data, the license, keywords, etc.
It enables discovery at a higher level than individual items, providing a simple way to describe sets of data.

**[api-spec/](api-spec/)** extends the core publishing capabilities of [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) with  
STAC-related endpoints. They add catalog and collection browsing and an active STAC REST search endpoint that returns
just the Items a user requests in their query. The API is specified as a couple [OpenAPI](http://openapis.org) documents, one is just the
[minimal core API](api-spec/STAC.yaml) and the other one [integrates the API extensions](api-spec/STAC-extensions.yaml).

**Extensions:** The *[extensions/](extensions/)* folder is where extensions live. Extensions can extend the 
functionality of the core spec or add fields for specific domains.

**Additional documents** A complementary [how to help](how-to-help.md)
document, a [list of implementations](implementations.md), 
and a discussion of the collaboration [principles](principles.md) and specification approach.

## Design Overview

An important core principle of the STAC design is to embrace best practices of making data available on the web (like 
[HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) and [W3C Spatial Data on the Web](https://www.w3.org/TR/sdw-bp/)), and 
to leverage the reliability of flat files on object stores like [AWS S3](https://aws.amazon.com/s3/) and [Google Cloud Storage](https://cloud.google.com/storage/).
This lead to designing a static catalog at the core of the STAC spec.

### Catalog Types

STAC Catalogs generally fall into two different types: Static Catalogs and Dynamic Catalog APIs.

The two catalog types both implement the same fields and links, and can be treated as the same by clients.
For more details on the two types see the chapters below. Additionally, consider the [Static and Dynamic Catalogs](../best-practices.md#static-and-dynamic-catalogs)
section of the best practices document on how you might use them best.

#### Static Catalog

A static catalog is an implementation of the STAC specification that does not respond dynamically to requests. It is simply
a set of files on a web server that link to one another in a way that can be crawled, often stored in an cloud storage
service like [Amazon S3](https://aws.amazon.com/s3/) or [Google Cloud Storage](https://cloud.google.com/storage/).
The core JSON documents and link structures are encoded in the file, and work as long as things are structured properly.
A static catalog can only really be crawled by search engines and active catalogs; it can not respond to queries.
But it is incredibly reliable, as there are no moving parts, no clusters or databases to maintain.
The goal of STAC is to expose as much asset metadata online as possible, so the static catalog offers a very lower
barrier to entry for anyone with geospatial assets to make their data searchable.

#### Dynamic Catalog API

A dynamic catalog API is implemented in software as a RESTful API that responds to queries (like give me all imagery in Oahu gathered on January 15, 2017). 
Its structure and responses are usually generated dynamically and are designed to mirror the static catalog, so the same client and crawler tools can consume
it. It generally indexes data for efficient responses, and aims to be easy for existing APIs to implement as a more standard
interface for clients to consume. It is specified in OpenAPI 3.0. An active catalog will often be populated by a static catalog,
or at least may have a 'backup' of its fields stored as a cached static catalog.

### Core Metadata and Extensions

The Item specification defines the core fields that all assets must make available for searching in a catalog.
In addition there are some basic fields for describing collections of data.
Vendors can extend those core fields for the metadata they want to make available, and the community has started to define shared extensions.

### UML Diagram

A UML diagram of the [STAC model](STAC-UML.pdf) is provided to help with navigating the specification. 

## Contributing

Anyone building software that catalogs imagery or other geospatial assets is welcome to collaborate.
Beforehand, please review our [guidelines for contributions](CONTRIBUTING.md).





