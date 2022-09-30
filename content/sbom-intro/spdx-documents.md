---
weight: 30
title: SPDX Documents
draft: false
---

# Documenting Software Artifacts with SBOMs

The Software Package Data Exchange (SPDX) Specification is an open standard for communicating software bill of materials (SBOM) information.

SPDX is an initiative of the Linux Foundation created to develop tools and formats for communicating the licensing information of software packages. Most developers have probably already encountered one specification from SPDX in the form of the SPDX License List, which is "a list of commonly found licenses and exceptions used for open source and other collaborative software." If you've defined a license in a Node project's `package.json` file or a Python project's `pyproject.toml` file, you've already interacted with a core component of an SPDX SBOM.

The [Software Package Data Exchange (SPDX) Specification](https://spdx.github.io/spdx-spec/v2.3/introduction/) defines a data exchange format for information about software packages and related content, "collected and shared in a common format with the goal of saving time and improving data accuracy."

{{< hint warning >}}
**SPDX Document Examples**

The examples in this section are taken from the [SPDX YAML Example, v2.2.2](https://github.com/spdx/spdx-spec/blob/development/v2.2.2/examples/SPDXYAMLExample-2.2.spdx.yaml). The content presented in some of the examples below has been modified or reformatted as necessary for the purposes of this guide. For full examples in each of the available formats for SPDX documents, have a look at [Producing/Consuming SPDX Documents](https://spdx.dev/resources/use/#documents) on the SPDX website.

Content omitted for formatting or clarity may be replaced with an ellipsis (`. . .`).

{{< /hint >}}

## SPDX for Licensing Compliance

Let's have a look at how SPDX enables licensing compliance, as a demonstration of the practical applications of SBOMs.

The only mandatory section of an SPDX document is the metadata for the document itself, where the document creator can [define the SPDX Specification Version](https://spdx.github.io/spdx-spec/v2.3/document-creation-information/#61-spdx-version-field), a [data license](https://spdx.github.io/spdx-spec/v2.3/document-creation-information/#62-data-license-field) for the information contained in the SBOM document itself (always `CC0-1.0`, to ensure SBOM metadata can be freely used), the [SPDX Identifier](https://spdx.github.io/spdx-spec/v2.3/document-creation-information/#63-spdx-identifier-field) that references the document itself, and several other attributes that explain how the SPDX document was created.

### Defining a Software Package

In the additional sections of an SPDX document, the licensing relationships between a software project and its dependencies are [defined in a structured way](https://spdx.github.io/spdx-spec/v2.3/package-information/). When an SPDX document is describing a software package, it first defines the metadata of the package that the document describes, including the package's name, version, supplier, the URI of the document's source project, and checksums for the package's distributed file.

```yaml
packages:
  - SPDXID: "SPDXRef-Package"
    name: "glibc"
    originator: "Organization: ExampleCodeInspect (contact@example.com)"
    packageFileName: "glibc-2.11.1.tar.gz"
    packageVerificationCode:
      packageVerificationCodeExcludedFiles:
        - "./package.spdx"
      packageVerificationCodeValue: "d6a770ba38583ed4bb4525bd96e50461655d2758"
    sourceInfo: "uses glibc-2_11-branch from git://sourceware.org/git/glibc.git."
    summary: "GNU C library."
    supplier: "Person: Jane Doe (jane.doe@example.com)"
    versionInfo: "2.11.1"
    checksums:
      - algorithm: "MD5"
        checksumValue: "624c1abb3664f4b35547e7c73864ad24"
      - algorithm: "SHA1"
        checksumValue: "85ed0817af83a24ad8da68c2b5094de69833983c"
      - algorithm: "SHA256"
        checksumValue: "11b6d3ee554eedf79299905a98f9b9a04e498210b59f15094c916c91d150efcd"
    downloadLocation: "http://ftp.gnu.org/gnu/glibc/glibc-ports-2.15.tar.gz"
```

### Defining a Package's License

For defining the license of a software package and its relationship with the package's dependencies, SPDX provides several different properties for communicating any licensing information surfaced by a license scanner application. At the highest level, where the package's actual licensing information is defined, an SPDX document may incorporate information extracted directly from the package, any copyright and licensing information found from scanning the source code, and any conclusions that the document creator (such as a license scanning tool) has made about how the licenses defined in the package apply:

```yaml
packages:
  - SPDXID: "SPDXRef-Package"
  . . .
    attributionTexts:
      - "The GNU C Library is free software.  See the file COPYING.LIB for copying conditions,\
        \ and LICENSES for notices about a few contributions that require these additional\
        \ notices to be distributed.  License copyright years may be listed using range\
        \ notation, e.g., 1996-2015, indicating that every year in the range, inclusive,\
        \ is a copyrightable year that would otherwise be listed individually."
    copyrightText: "Copyright 2008-2010 John Smith"
    filesAnalyzed: true
    licenseConcluded: "(LGPL-2.0-only OR LicenseRef-3)"
    licenseDeclared: "(LGPL-2.0-only AND LicenseRef-3)"
    licenseInfoFromFiles:
      - "GPL-2.0-only"
      - "LicenseRef-2"
      - "LicenseRef-1"
    licenseComments:
      "The license for this project changed with the release of version\
      \ x.y.  The version of the project included here post-dates the license change."
```

The metadata defining how an SPDX document's contents were generated allows for downstream consumers of an SBOM to determine how the included information meets their particular needs. This information is critical for ensuring that the SBOM is accurate and complete; for example, if a package scanner only returns the license declared for a package itself (without scanning the source code to determine if other licenses apply), it may not meet the needs of an organization that incorporates components from many different open source projects into its own products.

### Defining the Constituents of a Package

When a license scanner produces an SPDX document for a software project, if it detects licensing information for some code buried deep inside the project, it may surface this information in a dedicated "[other licensing information detected](https://spdx.github.io/spdx-spec/v2.3/other-licensing-information-detected/)" section.

```yaml
hasExtractedLicensingInfos:
  - licenseId: "LicenseRef-1"
    extractedText:
      "/*\n * (c) Copyright 2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007,\
      \ 2008, 2009 Hewlett-Packard Development Company, LP\n * All rights reserved.\n\
      \ *\n * Redistribution and use in source and binary forms, with or without\n *\
      \ modification, are permitted provided that the following conditions\n * are met:\n\
      \ * 1. Redistributions of source code must retain the above copyright\n *    notice,\
      . . .
      */"
  - licenseId: "LicenseRef-2"
    extractedText:
      "This package includes the GRDDL parser developed by Hewlett Packard\
      \ under the following license:\n� Copyright 2007 Hewlett-Packard Development Company,\
      \ LP\n\nRedistribution and use in source and binary forms, with or without modification,\
      \ are permitted provided that the following conditions are met: \n\nRedistributions\
      . . .
      "
  - licenseId: "LicenseRef-4"
    extractedText:
      "/*\n * (c) Copyright 2009 University of Bristol\n * All rights reserved.\n\
      \ *\n * Redistribution and use in source and binary forms, with or without\n *\
      . . .
      */"
  - licenseId: "LicenseRef-Beerware-4.2"
    comment: "The beerware license has a couple of other standard variants."
    extractedText:
      "\"THE BEER-WARE LICENSE\" (Revision 42):\nphk@FreeBSD.ORG wrote\
      \ this file. As long as you retain this notice you\ncan do whatever you want with\
      \ this stuff. If we meet some day, and you think this stuff is worth it, you can\
      \ buy me a beer in return Poul-Henning Kamp"
    name: "Beer-Ware License (Version 42)"
    seeAlsos:
      - "http://people.freebsd.org/~phk/"
  - licenseId: "LicenseRef-3"
    comment: "This is tye CyperNeko License"
    extractedText:
      "The CyberNeko Software License, Version 1.0\n\n \n(C) Copyright\
      \ 2002-2005, Andy Clark.  All rights reserved.\n \nRedistribution and use in source\
      \ and binary forms, with or without\nmodification, are permitted provided that\
      . . .
      "
    name: "CyberNeko License"
    seeAlsos:
      - "http://people.apache.org/~andyc/neko/LICENSE"
      - "http://justasample.url.com"
```

The licensing information from this SBOM comprises three different proprietary licenses and a common open source licens with several known variants. The license scanner has found licenses attached to specific pieces of source code within the project, and in this particular example, compliance with those licenses is achieved by retaining the original copyright and license information in each of source files.

When the licenses found within a project impose restrictions on using the code, the "other licensing information detected" section of the SBOM provides downstream consumers with an understanding of what those restrictions entail. For example, if a project incorporates and modifies large pieces of code from projects that are licensed as `GPL-2.0-only` and `GPL-2.0-or-later`, licensing those modifications as `GPL-3.0-or-later` would not be compliant with the original license of some of the code.

### Why does this matter?

As this example demonstrates, fully understanding the licensing of a software artifact and its dependencies is necessary to have any idea if you are complying with those terms.

Likewise, if you want to ensure dependencies with known vulnerabilities are not included in your projects, you need to have a full picture of the components comprising your software. This brings us to the security applications of SBOMs...
