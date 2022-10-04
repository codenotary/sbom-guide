---
weight: -999
title: "Main Page"
url: /
---

# Developer's Guide to SBOMs

Most modern applications are built atop a growing library of open-source and proprietary components; it would be tedious and time-consuming to manually identify each component, evaluate the components' dependencies, and determine if those components are then suitable for inclusion in the project.

Instead, automated software can assemble a Software Bill of Materials (SBOM) for each release artifact at some point in the build pipeline, when dependency and build information is readily available.

{{< hint info >}}
**Definition of *Software Bill of Materials***

A <strong><dfn title="software bill of materials">Software Bill of Materials (<abbr title="software bill of materials">SBOM</abbr>)</dfn></strong> is a manifest which uniquely identifies and enumerates the software dependencies contained within a codebase, build artifact, or runtime container.

{{< /hint >}}

## Why SBOMs?

The SBOM is a mechanism to share build-time component inventories between projects and developers, and it serves as a fundamental tool for managing software supply chain integrity.

**Without knowing what dependencies are included in a given third-party library, it's impossible to keep vulnerable components away from production.** Likewise, without an inventory of the software running on your infrastructure, it's impossible to determine which components are affected after new vulnerabilities are discovered.

## Why does this matter to me?

If you are a software developer or DevOps engineer, you should be aware of SBOMs and their applications. Even if you are not *yet* required to provide SBOMs for your projects, you can still benefit from the transparency and integrity they provide to the software supply chain.

## Getting Started

{{< columns >}}
If you would like to better understand the SBOM concept, you can begin reading with the introduction of our [Understanding SBOMs]({{< ref "/sboms/sbom-intro" >}} "Understanding SBOMs") guide.

<--->

If you would like to start using Codenotary Trustcenter to improve the security of your software supply chain, you can begin with...
{{< /columns >}}
