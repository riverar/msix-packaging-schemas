# MSIX Packaging Schemas / Diagrams

XML Schemas (XSD) extracted from the Windows `AppxPackaging.dll` resource section.

These are the package/manifest/bundle/packaging schemas that Microsoft ships
embedded inside `AppxPackaging.dll`. Only some of them are documented publicly;
the ones that are live under
[learn.microsoft.com/uwp/schemas/](https://learn.microsoft.com/uwp/schemas/).
The rest are internal/platform schemas not exposed in the docs.

## Source

- **File:** `AppxPackaging.dll` (PE32+, x86-64)
- **DLL version:** `10.0.29648.1000`
- **Extraction point:** PE resource directory, **resource type 500**, language `0x0409`
- **Files:** 118 XSDs, one per resource

## File naming

Where the namespace of a schema is known to Visual Studio's schema catalog
(`SchemaCatalog`, the `<SchemaCatalog xmlns="http://schemas.microsoft.com/xsd/catalog">`
file shipped with the .NET/MSIX tooling), the canonical catalog filename is used,
e.g.:

- `UapManifestSchema_v11.xsd` — UAP manifest schema, version 11
- `DesktopManifestSchema_11.xsd` — desktop bridge manifest schema, v11
- `AppxManifestTypes.xsd` — shared AppX manifest types
- `AppxContentGroupMapSchema.xsd` / `SourceContentGroupMapSchema.xsd` — content group maps

The remaining schemas are not in that catalog, so they are named after each
schema's `targetNamespace` + root element, e.g.:

- `AppxManifestSchema2010_v4.xsd` — the 2010 app package manifest
- `AppInstallerManifest2021.xsd` — App Installer manifest
- `AppxBundleSchema2016.xsd` — app bundle schema

Where Microsoft ships more than one resource with the same namespace (duplicate
revisions of the same schema), the resource ID from the DLL is appended as
`_r<id>` (e.g. `UapManifestSchema_v3_r133.xsd` vs `UapManifestSchema_v3_r212.xsd`)
so every extracted resource is preserved and unambiguous.

> Note: the schemas cross-reference each other only via namespace-only
> `xs:import` (no `schemaLocation`), so imports cannot be resolved against the
> filenames in this repo. Treat the set as a reference snapshot of the embedded
> schemas, not as a build-ready package.
