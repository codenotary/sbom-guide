---
title: Introduction
type: docs
---

# Developer's Guide to SBOMs

A **Software Bill of Materials (SBOM)** is a manifest which uniquely identifies and enumerates the software dependencies contained within a codebase, build artifact, or runtime container. A dynamic SBOM can further integrate information from vulnerability scanners to help ensure the integrity of your software supply chain.

## Why SBOMs?

Most modern applications are built atop a growing library of open-source and proprietary components; it would be tedious and time-consuming to manually identify each component, evaluate the components' dependencies, and determine if those components are then suitable for inclusion in the project.

Instead, automated software assembles the SBOM for each release artifact at some point in the build pipeline, when dependency and build information is readily available.

The SBOM is a mechanism to share build-time component inventories between projects and developers, and it serves as a fundamental tool for managing software supply chain integrity. Without knowing what dependencies are included in a given third-party library, it's impossible to keep vulnerable components away from production. Likewise, without an inventory of the software running on your infrastructure, it's impossible to determine which components are affected after new vulnerabilities are discovered.

Federal agencies (and the White House) are now issuing guidelines intended to increase software supply chain security—through increased software safety and integrity.
