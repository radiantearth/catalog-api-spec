# STAC Scientific Profile Spec

This document explains the fields of the STAC Scientific (sci) Profile to a STAC `Catalog`. Scientific metadata is considered to be data that indicate from which publication a dataset originates and how the dataset itself should be cited or referenced. Overall, it helps to increase reproducibility and citability.

* [Example](example-merraclim.json)
* [JSON Schema](schema.json)

## Scientific Profile Description

These are fields that extend the `Catalog` object:

| Element          | Type                 | Name                      | Description                                                  |
| ---------------- | -------------------- | ------------------------- | ------------------------------------------------------------ |
| sci:doi          | string               | Dataset DOI               | [DOI](https://www.doi.org/) of the dataset.                  |
| sci:citation     | string               | Proposed Dataset Citation | The proposed citation for the dataset.                       |
| sci:publications | [Publication Object] | Publications              | List of relevant publications referencing and describing this dataset. |

**sci:doi**: The [DOI](https://www.doi.org/) name of the dataset, e.g. `10.1000/xyz123`. This MUST NOT be a DOIs link. See the DOI section for more information about DOI links.

**sci:citation**: The recommended human-readable reference (citation) to be used by publications citing this dataset. No specific citation style is suggested, but the citation should contain all information required to find the publication distinctively.

**sci:publications**: List of relevant publications referencing and describing this dataset. Can contain a DOI and a citation, see chapter *Publication Object*.

### Publication Object

| Element  | Type   | Name                 | Description                                                  |
| -------- | ------ | -------------------- | ------------------------------------------------------------ |
| doi      | string | Publication DOI      | [DOI](https://www.doi.org/) of the publication referencing this dataset. |
| citation | string | Publication Citation | Citation of the publication referencing this dataset.        |

**doi**: The [DOI](https://www.doi.org/) name of a publication which describes and references the dataset. The publications should include more information about the dataset and how it was processed. This MUST NOT be a DOI link. See the DOIs section for more information about DOI links.

**citation**: Human-readable reference (citation) of a publication which describes and references the dataset. The publications should include more information about the dataset and how it was processed. No specific citation style is suggested, but a citation should contain all information required to find the publication distinctively.

## DOIs

DOIs are persistent digital interoperable identifier that uniquely identify for digital publications. They can be registered at registration agencies affiliated with the International DOI Foundation.

For all DOI names respective DOI links SHOULD be added to the links section of the catalog with the `rel` type `cite-as` (see the [IETF draft](https://tools.ietf.org/id/draft-vandesompel-citeas-03.html)).