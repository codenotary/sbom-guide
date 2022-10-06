---
weight: 50
title: CycloneDX Documents
draft: false
---

# Communicating Known Vulnerabilities with SBOMs

We've already seen how an inventory of components is necessary for establishing the chain of custody of software components, but a manifest of the components used to build a piece of software cannot, by itself, prevent the use of vulnerable components. So far, we've primarily seen how SBOMs—which are snapshots of asset composition—can be used to ensure the *integrity* of known software components. But what about the *security* of those components?

As with SPDX, a CycloneDX SBOM describes the composition of a software asset. Beyond that, however, CycloneDX provides several ways to communicate relevant security information about a given asset and its dependencies. CycloneDX SBOMs can communicate known vulnerabilities in the dependency graph of a software project and, in the form of security advisories, disclose previously unknown vulnerabilities.

In this section of the guide, we'll look at how CycloneDX SBOMs can be used in analyzing software for vulnerabilities and how SBOMs can be used to communicate known vulnerabilities (and the remediation of those vulnerabilities) to downstream consumers of a software project.

## Vulnerability Analysis

One use case that is enabled by CycloneDX is the ability to communicate the exploitability of a known vulnerability in a component—in the context of a specific software project.

For instance, [one Vulnerability Exploitability Exchange (VEX) example from CycloneDX](https://github.com/CycloneDX/bom-examples/tree/master/VEX/Use-Cases/Case-1) demonstrates how the risk of vulnerabilities inhereted from upstream components can be communicated between projects. In this excerpt, the `vulnerabilities` property of the SBOM indicates that there is a known vulnerability `CVE-2020-25649` in a dependency of the project, but because the vulnerable code is not used by the current project, the vulnerability is not exploitable:

```json
{
  . . .
  "vulnerabilities": [
    {
      "id": "CVE-2020-25649",
      "ratings": [
        {
          "source": {
            "name": "NVD",
          },
          "score": 7.5,
          "severity": "high"
        },
        {
          "source": {
            "name": "Acme Inc"
          },
          "score": 0.0,
          "severity": "none",
        }
      ],
      "analysis": {
        "state": "not_affected",
        "justification": "code_not_reachable",
        "response": ["will_not_fix", "update"],
        "detail": "[...] review indicates that the vulnerable code is not reachable, either directly or indirectly."
      }
    }
  ]
}
```

## Analyzing the Chain of Custody

CycloneDX offers us the concept of "component [pedigree](https://cyclonedx.org/use-cases/#pedigree)", which documents the source of the components comprising a software project "and the commits, patches, and diffs which make it unique".

This example, adapted from CycloneDX's documentation, shows a CycloneDX SBOM with pedigree information for a component (in XML format, this time):

```xml
<bom xmlns="http://cyclonedx.org/schema/bom/1.4" serialNumber="urn:uuid:3e671687-395b-41f5-a30f-a58921a69b79" version="1">
  <components>
    <component type="library">
      <group>com.acme</group>
      <name>sample-library</name>
      <version>1.0.0</version>
      <pedigree>
        <ancestors>
          <!-- The original variant of the component -->
          <component type="library">
            <group>org.example</group>
            <name>sample-library</name>
            <version>1.0.0</version>
          </component>
        </ancestors>
        <commits><commit>
            <!-- The specific changes in this variant -->
            <uid>7638417db6d59f3c431d3e1f261cc637155684cd</uid>
            <url>https://location/to/7638417db6d59f3c431d3e1f261cc637155684cd</url>
            <author>
              <email>john.doe@example.com</email>
            </author>
            <message>Initial commit</message>
        </commit></commits>
        <patches>
          <patch type="unofficial">
            <diff>
              <url>uri/to/changes.diff</url>
            </diff>
            <resolves>
              <issue type="enhancement">
                <id>JIRA-17240</id>
                <description>Great new feature that does something</description>
                <source>
                  <url>https://issues.acme.org/17240</url>
                </source>
              </issue></resolves></patch></patches>
    </pedigree></component></components>
</bom>
```

The use case for this type of information is in analyzing the risk posed by changes to a project as it evolves over time.

---

### Example: Communicating the Risk of Legacy Dependencies

To illustrate, let's consider a hypothetical project, `libacme`, which depends on an unmaintained version of the `liblegacy` library.

Because the older version of `liblegacy` no longer receives security patches from the original maintainers, the `libacme` project maintains its own fork of the library, which regularly receives patches from the `libacme` security team as vulnerabilities are discovered.

When automated security tools analyze the SBOM for `libacme`, encountering a dependency such as `liblegacy`—with its major version that has long since reached end-of-life and its many known vulnerabilities—will surely raise a red flag.

However, if the SBOM for `libacme` also includes the pedigree of the `liblegacy` component, automated tools and security analysts alike can see that the `liblegacy` component has been forked and maintained by the `libacme` project.

{{< details title="Example of CycloneDX pedigree for security fixes" open=false >}}

*This example was adapted from the CycloneDX documentation and heavily modified to highlight the relevant concept.*

```json
{
  "bomFormat": "CycloneDX",
  "components": [
    {
      "type": "library",
      "group": "com.acme",
      "name": "liblegacy",
      "version": "1.5.3",
      "pedigree": {
        "ancestors": [
          {
            "type": "library",
            "group": "org.example",
            "name": "liblegacy",
            "version": "1.5.2"
          }
        ],
        "commits": [
          {
            "uid": "7638417db6d59f3c431d3e1f261cc637155684cd",
            "committer": {
              "timestamp": "2018-11-13T20:20:39+00:00",
              "name": "Acme Security Team",
              "email": "security@acme.exp"
            },
            "message": "enhanced version"
          }
        ],
        "patches": [
          {
            "type": "backport",
            "diff": {
              "text": {
                "contentType": "text/plain",
                "encoding": "base64",
                "content": "ZXhhbXBsZSBkaWZmIGhlcmU="
              },
              "url": "uri/to/changes.diff"
            },
            "resolves": [
              {
                "type": "security",
                "id": "CVE-2019-9997",
                "name": "CVE-2019-9997",
                "description": "Issue description here",
                "source": {
                  "name": "NVD",
                  "url": "https://nvd.nist.gov/vuln/detail/CVE-2019-9997"
                }
              }
            ]
          }
        ]
      }
    }
  ]
}
```

{{< /details >}}

Although the initial analysis of the SBOM suggests that `libacme` contains an insecure dependency, the pedigree information can allow automated tools to determine that the vulnerabilities known to exist in `liblegacy` have been remediated by the security team at the `libacme` project.

If the party responsible for the patch remediating the vulnerability is known and trusted (according to the organization's security policies), then the flags automatically raised by the scanning tool can be automatically dismissed.

---

Once again, the SBOM is acting as a *static* representation of a software asset, and the pedigree information documents how that asset has evolved to its current state.

When an organization relies on an open source component for a critical piece of their infrastructure, the pedigree information provided by a CycloneDX SBOM can help guide the security team in assessing the risks posed by updates to that component (according to whatever threat model is used in their particular context).
