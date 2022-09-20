---
title: "Authenticating SBOMs with Trustcenter"
weight: 140
draft: false
---

## Add attachmentments to notarization transaction with `vcn`

To add attachments containing different types of supporting documentation to a notarization transaction:

```bash
vcn n artifact --attach attachment1:label1 --attach attachment2:label1 --attach attachment3:label2
```

TODO.

### Resolving dependencies with `vcn`

To

```bash
vcn bom <asset> [bom options] [bom output options]
```

### Authenticating an asset's BOM with `vcn`

To

```bash
vcn a --bom <asset> [bom options] [bom output options]
```

### Notarizing an asset's BOM with `vcn`

To

```bash
vcn n --bom <asset> [bom options] [bom output options]
```

### Alternative output formats with `vcn`

To work with individual dependencies:

```bash
vcn a|n|ut|us <scheme>://<name>@<version> | --hash <hash>

# Alternative flags:
# --bom-spdx
# --bom-cdx-json
# --bom-cdx-xml
```