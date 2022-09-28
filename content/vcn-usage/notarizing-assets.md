---
title: Notarizing Assets
weight: 120
draft: false
---

# Notarizing Assets

When using `vcn` with Codenotary Trustcenter, the notarization process creates a cryptographic signature of the asset and stores it in a cryptographically-verifiable immutable database (immudb). That signature can then be used to authenticate the asset and verify its integrity.

## Notarize an asset with `vcn`

The most basic way to notarize an asset is to pass the asset to the `vcn notarize` command:

```bash
vcn notarize <artifact>
```

This command will sign the combination of the asset's name, version, and hash to unique identify it. `vcn` will then send the signature to Trustcenter, which will store an immutable record of the signature.

## Notarizing assets with dependencies

Passing the `--bom` flag to the `vcn notarize` command will notarize the asset itself, in combination with notarization the asset's dependencies. This process will immutably associate the artifact with its dependencies in Trustcenter:

```bash
vcn notarize --bom <artifact>
```

## Notarizing assets in bulk

If you need to notarize assets in bulk, you can supply a CSV file that enumerates the hash, name, and [labels]({{< relref "/vcn-usage/labeling-assets" >}}) of each asset to the `vcn notarize` command:

```bash
vcn notarize --import-file <csv_file>
```

The contents of your CSV file should follow the format

```bash
hash,name,labels
```

where `hash` is the hash of the asset, `name` is the name of the asset, and `labels` is an optional list of semicolon-separated labels. For example, your CSV file will look something like this:

```csv
addf340d683e7dc9be1859f4e9a85f5143d4b21c,libcrypto1.1@1.1.1q-r0,label1;label2
722a653f03c02836b5f6391bc588e28aff86e44b,libssl1.1@1.1.1q-r0,label2
2962576b068d3e220d1df7730a0fc5ac49a201a5,ssl_client@1.35.0-r17,label2;label3
124baa9bfd023f2c0308a11b13086c3c2c3ecfd1,zlib@1.2.12-r3,label1;label3
```

## Add attachments to notarization transactions

To attach files containing user-defined supporting documentation (e.g., build pipeline metadata or deployment information) to a notarization transaction in Trustcenter, use the `--attach` flag to specify the path to the file and a label to identify it:

```bash
vcn notarize <artifact> --attach=<ATTACHMENT_PATH>[:<ATTACHMENT_LABEL>]
```

{{< details title="`vcn notarize` Flags Documentation" open=false >}}

- `--attach`
  - Add user defined file attachments. This flag can be repeated to include multiple attachments.

    It's possible to specify a label for each entry, by appending the label to the file path after a colon, for example: `--attach=metadata.json:jobid123`. When authenticating an asset with `vcn authenticate`, the same path and label can be specifed with the `--attach` flag to retrieve that attachment. The label alone can be specified with the `--attach` flag to retrieve all attachments, e.g. `vcn a <artifact> --attach=jobid123`.

{{< /details >}}