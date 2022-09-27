---
title: "Authenticating Assets with Trustcenter"
weight: 130
draft: false
---

# Authenticating Assets with Trustcenter

## Using `vcn` to authenticate an asset

To authenticate an asset with Trustcenter, which verifies the integrity and signature of the asset, use the `vcn authenticate` command:

```bash
vcn authenticate <asset>
```

## Authenticate with a specific signerID with `vcn`

Using signerID:

```bash
vcn authenticate --signerID <signerID> <asset>
```

## Authenticate multiple assets with `vcn`

To

```bash
ls | xargs vcn authenticate
```