<img src="https://github.com/radiantearth/stac-site/raw/master/images/logo/stac-030-long.png" alt="stac-logo" width="700"/>

[![CircleCI](https://circleci.com/gh/radiantearth/stac-spec.svg?style=svg)](https://circleci.com/gh/radiantearth/stac-spec)

**Mar 2, 2020** - The API portion of STAC has been split off into a [new repository - stac-api-spec](https://github.com/radiantearth/stac-api-spec) and will start being versioned and released separately than the core STAC spec. This is a work in progress, to advance us to 1.0-beta releases. Links and references between STAC and the STAC API may be incorrect, and the testing requirement to merge is temporarily disabled so we can collaboratively get it working.

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

The specification is reaching maturity, and the next release will be 1.0-beta1. At this point we don't anticipate any 
major changes, but reserve the right to make them if we get feedback that something just doesn't work.

## Current version and branches

The [master branch](https://github.com/radiantearth/stac-spec/tree/master) is the 'stable' version of the spec. It is currently version 
**0.9.0** of the specification. The 
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
documents (if relevant). And there is one more specification in the STAC 'family', which is
the [STAC API specification](https://github.com/radiantearth/stac-api-spec/), which now lives in its own repository. It
provides API endpoints, based on the [OGC API - Features](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) standard,
that enable clients to search for `item`s that match their filtering criteria. The four specifications are meant to be used 
together, but are designed so each piece is small, self-contained and reusable in other contexts.

**[item-spec/](item-spec/)** defines a STAC Item, which is a [GeoJSON](http://geojson.org) Feature
with additional fields for things like time, links to related entities and assets (including thumbnails). This is the 
atomic unit that describes the data to be discovered.

**[catalog-spec/](catalog-spec/)** specifies a structure to link various STAC Items together to be crawled or browsed. It is a
simple, flexible JSON file of links to Items, Catalogs or Collections that can be used in a variety of ways.

**[collection-spec/](collection-spec/)** provides additional information about a spatio-temporal collection of data.
In the context of STAC it is most likely a collection of STAC Items that is made available by a data provider.
It includes things like the spatial and temporal extent of the data, the license, keywords, etc.
It enables discovery at a higher level than individual items, providing a simple way to describe sets of data.

**Extensions:** The *[extensions/](extensions/)* folder is where extensions live. Extensions can extend the 
functionality of the core spec or add fields for specific domains.

**Additional documents** A complementary [how to help](how-to-help.md)
document, a [list of implementations](implementations.md), [best practices](best-practices.md)
and a discussion of the collaboration [principles](principles.md) and specification approach.

### UML Diagram

A UML diagram of the [STAC model](STAC-UML.pdf) is provided to help with navigating the specification. 

## Contributing

Anyone building software that catalogs imagery or other geospatial assets is welcome to collaborate.
Beforehand, please review our [guidelines for contributions](CONTRIBUTING.md).
