---
title: "Notarizing Assets with Trustcenter"
weight: 120
draft: false
---

## Notarize an asset with `vcn`

To notarize an asset:

```bash
vcn notarize <artifact>
```

Passing the `--bom` flag to the `vcn notarize` command will notarize the asset's dependencies and immutably associate them with the asset in Trustcenter:

```bash
vcn notarize --bom <artifact>
```

If you need to notarize assets in bulk, you can supply a CSV file that enumerates the hash, name, and labels of each asset to the `vcn notarize` command:

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

## Managing assets with labels

Codenotary Trustcenter can associate labels with your notarized artifacts to provide an additional level of metadata for authentication. Labels can be used to indicate the intended use of an artifact, such as `production`, `staging`, or `development`. Labels can also be used to indicate the type of artifact, such as `library`, `binary`, or `container`.

Labels can be appended to, deleted from, or overwritten on an artifact when the `vcn notarize` command is run by passing the `--labels-add`, `--labels-del`, or `--labels-set` flags, as illustrated below.

### vcn Labeling Example

We can begin by notarizing an artifact with the `--labels-add` flag:

```bash
❯ vcn n image://example --labels-add 'label1,label2,label3'

Name:        example
Hash:        f34ed96bfd9f329f89ce3977373cef37ce5d0a4ba8c5ed4aebca785d649b9082
. . .
SignerID:    demo-signer
Apikey revoked:    no
Status:        TRUSTED
Labels:        label1 (added), label2 (added), label3 (added)
```

The asset can then be notarized again with the `--labels-del` flag to remove one of the labels from our signature for the artifact:

```bash
❯ vcn notarize image://example --labels-del 'label3'

Name:        example
Hash:        f34ed96bfd9f329f89ce3977373cef37ce5d0a4ba8c5ed4aebca785d649b9082
. . .
Status:        TRUSTED
Labels:        label3 (removed)

❯ vcn inspect image://example --labels

Name:        example
Hash:        f34ed96bfd9f329f89ce3977373cef37ce5d0a4ba8c5ed4aebca785d649b9082
. . .
Status:        TRUSTED
Labels:        label1, label2
```

Then, after using the `--labels-set` flag to overwrite the existing labels on the artifact, we can use the `--labels-add` flag to append a final label to the artifact:

```bash
❯ vcn notarize image://example --labels-set 'label4,label5'

Name:        example
Hash:        f34ed96bfd9f329f89ce3977373cef37ce5d0a4ba8c5ed4aebca785d649b9082
. . .
Status:        TRUSTED
Labels:        label4, label5
```

```bash
❯ vcn notarize image://example --labels-add 'label6'

Name:        example
Hash:        f34ed96bfd9f329f89ce3977373cef37ce5d0a4ba8c5ed4aebca785d649b9082
. . .
Status:        TRUSTED
Labels:        label6 (added)
```

```bash
❯ vcn inspect image://example --labels

Name:        example
Hash:        f34ed96bfd9f329f89ce3977373cef37ce5d0a4ba8c5ed4aebca785d649b9082
. . .
Status:        TRUSTED
Labels:        label4, label5, label6
```

These labels provide an additional layer of metadata, and can be used to alter the results of the `vcn authenticate` command. For example, if we run the `vcn authenticate` command with the `--label` flag, the result will vary depending on the state of the asset's labels:

```bash
vcn inspect <artifact> --label 'demo'
```

- If the artifact was marked **Trusted**, and
    - the label `demo` is **assigned**, then the status is `TRUSTED`.
    - the label `demo` is **missing**, then the status is `UNKNOWN`.
- If the artifact was marked **Untrusted**/**Unsupported**, and
    - the label `demo` is **assigned**, then the status is `UNTRUSTED`/`UNSUPPORTED` (respectively).
    - the label `demo` is **missing**, then the status is `UNKNOWN`.