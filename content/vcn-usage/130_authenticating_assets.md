---
title: Authenticating Assets
weight: 130
draft: false
---

# Authenticating Assets with Trustcenter

After an asset has been notarized with Codenotary Trustcenter, it can be authenticated to verify the integrity and signature of the asset before deployment to a production environment.

When an asset is authenticated, `vcn` will verify the integrity of the asset by comparing the hash of an asset with the signed hash stored by Trustcenter. If the hash of the asset matches the signed hash stored by Trustcenter and that asset is marked as Trusted, `vcn` will return a success message.

## Using `vcn` to authenticate an asset

To authenticate an asset, which verifies the integrity of the asset by comparing it with the signed hash stored immutably by Trustcenter, use the `vcn authenticate` command:

```bash
vcn authenticate <asset>
```

`vcn` will query your immutable ledger in Trustcenter for a notarized record of the asset. If the record is found and the asset has been marked as Trusted, the asset has been authenticated and can be released to production.

## Authenticate with a specific signerID with `vcn`

Using signerID:

```bash
vcn authenticate --signerID <signerID> <asset>
```

## Authenticate multiple assets with `vcn`

To authenticate assets in batches, you can use [`xargs`](https://man7.org/linux/man-pages/man1/xargs.1.html) to call `vcn authenticate` on each asset in a directory:

```bash
ls | xargs vcn authenticate
```
