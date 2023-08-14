# Example Links Extension Specification

- **Title:** Example Links
- **Identifier:** <https://stac-extensions.github.io/example-links/v0.0.1/schema.json>
- **Field Name Prefix:** example
- **Scope:** Item, Catalog, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Example Links Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It allows to add example links in a well-specified manner so that e.g. code snippets can be shown with code highlighting.

- Examples:
  - [Item example](examples/item.json): Shows the usage in a generic STAC Item
  - [Collection example](examples/collection.json): Shows the usage in a STAC Collection from Maxar
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:
- [ ] Catalogs
- [ ] Collections
- [ ] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [x] Links (with `rel` set to `example`)

| Field Name        | Type    | Description |
| ----------------- | ------- | ----------- |
| example:container | boolean | Specifies whether the given URI points directly to the example code (`false`, default, e.g. a Python file) or to a container format (`true`, e.g. PDF, HTML, Jupyter Notebook, Markdown, ...) that contains/describes code snippets. |
| example:language  | string  | If the example is in a specific programming language, specify it here. Should be one of the [languages listed for Linguist](https://github.com/github-linguist/linguist/blob/master/lib/linguist/languages.yml). For example, `JavaScript` or `Python`. If a page contains multiple programming languages, you can use `Multiple`. |

Note on the use of `type` and `example:language`: For source code files (i.e. non-containers) it can be enough to just
specify the media type in `type` if there is a media type available that is specific enough (e.g. `text/x-python`).
Nevertheless, it is recommended to always specify `example:language` to improve interoperability.

**Recommended additional fields:**
- `descripton`: It is recommended to add a description as defined in
  [common metadata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#basics).
- `file:size`: If `example:container` is `false` (ot not set), clients may decide to load the code and highlight it in the UI.
  Thus, it is recommended to set the field [`file:size` from the file extension](https://github.com/stac-extensions/file/blob/main/README.md)
  so that clients may skip loading overly large examples.
- Additional [request related properties](https://github.com/radiantearth/stac-spec/issues/1198)
  such as `method`, `body` or `header` can be used.

## Relation types

The following types must be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type    | Description |
| ------- | ----------- |
| example | Link to an example. |

## Usage Examples

Examples can be provided directly as source code files or as a document with embedded code (e.g. web page, PDF document, Jupyter Notebook).

### Examples: Source Code Files

The Link Object for a Python source code file:
```json
{
  "rel": "example",
  "href": "https://stac.example/run.py",
  "title": "Example in Python",
  "type": "text/x-python",
  "description": "This describes the *Python* script...",
  "file:size": 123547,
  "example:language": "Python"
}
```

The Link Object for a JavaScript source code file:
```json
{
  "rel": "example",
  "href": "https://stac.example/run.js",
  "title": "Example in JavaScript",
  "type": "application/javascript",
  "description": "This describes the *JavaScript* code...",
  "file:size": 231,
  "example:language": "JavaScript"
}
```

### Examples: Embedded Code

The Link Object for a PDF document with C code:
```json
{
  "rel": "example",
  "href": "https://stac.example/py-example.pdf",
  "title": "Example PDF containing C",
  "description": "Describes how to do fancy stuff with the data in C",
  "type": "application/pdf",
  "example:container": true,
  "example:language": "C"
}
```

The Link Object for a Jupyter Notebook containing Python code:
```json
{
  "rel": "example",
  "href": "https://stac.example/py-notebook.ipynb",
  "title": "Example Notebook containing Python",
  "description": "Describes how to do fancy stuff with the data in Python",
  "type": "application/x-ipynb+json",
  "example:container": true,
  "example:language": "Python"
}
```

The Link Object for a HTML document that shows JavaScript code:
```json
{
  "rel": "example",
  "href": "https://stac.example/js-example.html",
  "title": "Example webpage containing JS",
  "description": "Describes how to do fancy stuff with the data in JavaScript",
  "type": "text/html",
  "example:container": true,
  "example:language": "JavaScript"
}
```

The Link Object for a RMarkdown document that shows R code:
```json
{
  "rel": "example",
  "href": "https://stac.example/js-example.rmd",
  "title": "Example RMarkdown document containing R",
  "description": "Describes how to do fancy stuff with the data in R",
  "type": "text/markdown",
  "example:container": true,
  "example:language": "R"
}
```

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
