---
title: Introduction
type: docs
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

## Why does this matter?

Federal agencies (and the White House) are now issuing guidelines intended to increase software supply chain security—through increased software safety and integrity.

To quote President Biden's [Executive Order 14028](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/) on *Improving the Nation’s Cybersecurity*:

> [T]he trust we place in our digital infrastructure should be proportional to how trustworthy and transparent that infrastructure is, and to the consequences we will incur if that trust is misplaced.

This order calls for transparency in the software supply chain, and SBOMs are a key part of that transparency. Notably, the policies laid out in this order will require vendors to provide SBOMs for all software they sell to the federal government, which will result in more vendors requesting SBOMs from their upstream suppliers.

The tooling and standards for SBOMs are continuing to mature, and as it becomes increasingly important for developers of both proprietary and open source software to generate SBOMs for their projects, the integration of SBOMs into software development workflows will become more widespread. A [pilot program created by the above order](https://www.whitehouse.gov/briefing-room/statements-releases/2021/05/12/fact-sheet-president-signs-executive-order-charting-new-course-to-improve-the-nations-cybersecurity-and-protect-federal-government-networks/) shows the potential for how SBOMs can be used to communicate the security posture of software products (emphasis added):

> [The executive order] creates a pilot program to create an “energy star” type of label so the government – and the public at large – can quickly determine whether software was developed securely. Too much of our software, including critical software, is shipped with significant vulnerabilities that our adversaries exploit. This is a long-standing, well-known problem, but for too long we have kicked the can down the road. **We need to use the purchasing power of the Federal Government to drive the market to build security into all software from the ground up.**

If you are a software developer or DevOps engineer, you should be aware of SBOMs and their applications. Even if you are not *yet* required to provide SBOMs for your projects, you can still benefit from the transparency and integrity they provide to the software supply chain.
