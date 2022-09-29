---
title: Authenticating SBOMs
weight: 140
draft: false
---

# Notarizing and Authenticating Software Bills of Materials (SBOMs) with Trustcenter

When a Software Bill of Materials (SBOM) is notarized by `vcn` for a container image, codebase, or other build artifact, that asset's entire manifest of dependencies is also notarized and they are immutably associated with one another.

This process creates a chain of custody for the asset and its dependencies, allowing you to check the integrity of the asset, track which artifacts are deployed on your infrastructure, and ensure untrusted dependencies never reach production.

## Resolving dependencies with `vcn`

You can use the `vcn bom` command to collect and display an artifact's manifest of dependencies:

```bash
vcn bom <artifact>
```

{{< details title="Example: `vcn bom`" open=false >}}
Example command:

```bash
vcn bom image://python:3.10-alpine
```

Output:

```console
:	alpine-baselayout-data@3.2.0-r22
:	musl@1.2.3-r0
:	busybox@1.35.0-r17
:	alpine-baselayout@3.2.0-r22
:	alpine-keys@2.4-r1
:	ca-certificates-bundle@20220614-r0
:	libcrypto1.1@1.1.1q-r0
:	libssl1.1@1.1.1q-r0
:	ssl_client@1.35.0-r17
:	zlib@1.2.12-r3
:	apk-tools@2.12.9-r3
:	scanelf@1.3.4-r0
:	musl-utils@1.2.3-r0
:	libc-utils@0.7.2-r3
:	ca-certificates@20220614-r0
:	tzdata@2022a-r0
:	ncurses-terminfo-base@6.3_p20220521-r0
:	ncurses-libs@6.3_p20220521-r0
:	libbz2@1.0.8-r1
:	sqlite-libs@3.38.5-r0
:	libffi@3.4.2-r1
:	gdbm@1.23-r0
:	xz-libs@5.2.5-r1
:	expat@2.4.8-r0
:	libintl@0.21-r2
:	libtirpc-conf@1.3.2-r1
:	krb5-conf@1.0-r2
:	libcom_err@1.46.5-r0
:	keyutils-libs@1.6.3-r1
:	libverto@0.3.2-r0
:	krb5-libs@1.19.3-r0
:	libtirpc@1.3.2-r1
:	libnsl@2.0.0-r0
:	libuuid@2.38-r1
:	readline@8.1.2-r0
:	.python-rundeps@20220907.224335
```

{{< /details >}}

## Notarizing an artifact's bill of materials

You can make use of a bill of materials by notarizing it together with the artifact, using the `vcn notarize` command and the `--bom` flag:

```bash
vcn notarize --bom <artifact>

# Short form of command:
vcn n --bom <artifact>
```

## Authenticating an artifact's bill of materials

After an artifact has been notarized with its bill of materials, you can authenticate the artifact and its dependencies, using the `vcn authenticate` command and the `--bom` flag:

```bash
vcn authenticate --bom <artifact>

# Short form of command:
vcn a --bom <artifact>
```

<!-- ## Alternative output formats with `vcn`

To work with individual dependencies:

```bash
vcn a|n|ut|us <scheme>://<name>@<version> | --hash <hash>

# Alternative flags:
# --bom-spdx
# --bom-cdx-json
# --bom-cdx-xml
``` -->
