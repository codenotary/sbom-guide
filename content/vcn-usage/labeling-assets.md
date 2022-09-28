---
title: Labeling Assets
weight: 135
draft: false
---

# Managing Assets with Labels

Codenotary Trustcenter can associate labels with your notarized artifacts to provide an additional level of metadata for authentication. Labels can be used to indicate the intended use of an artifact, such as `production`, `staging`, or `development`. Labels can also be used to indicate the type of artifact, such as `library`, `binary`, or `container`.

Labels can be appended to, deleted from, or overwritten on an artifact when the `vcn notarize` command is run by passing the `--labels-add`, `--labels-del`, or `--labels-set` flags, as illustrated below.

## vcn Labeling Example

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

### How labels are used in authentication

- If the artifact was marked **Trusted**, and
    - the label `demo` is **assigned**, then the status is `TRUSTED`.
    - the label `demo` is **missing**, then the status is `UNKNOWN`.
- If the artifact was marked **Untrusted**/**Unsupported**, and
    - the label `demo` is **assigned**, then the status is `UNTRUSTED`/`UNSUPPORTED` (respectively).
    - the label `demo` is **missing**, then the status is `UNKNOWN`.