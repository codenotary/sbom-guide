---
title: Authenticating Dependencies
weight: 160
draft: false
---

# Authenticating Known and Trusted Dependencies

If we attempt to authenticate a Docker image which has not yet been notarized with our signing key, we receive a warning that the asset hasn't yet been notarized:

```bash
❯ vcn authenticate --bom docker://python:3.9-alpine

Warning: c9b90024bc4d49b1fa0ea4673b6eb1db1058cd1cba4b840d336bedf803a0afcf was not notarized
```

When we then notarize the image, all of its dependencies are resolved, authenticated with Trustcenter, signed, and marked as `Trusted`:

```bash
❯ vcn notarize --bom image://python:3.9-alpine

Your assets will not be uploaded. They will be processed locally.

Notarization in progress...
artifact notarized

Kind:        image
Name:        python:3.9-alpine
Hash:        dca341b7a3fdbe1aa117f97f55321e60fe54a177d6f58ab3373ece796aca52ef
Metadata:    hashtype="SHA256"
        image={
            "Architecture": "amd64",
            "Created": "2022-09-07 23:29:27.440739972 +0000 UTC",
            "DockerVersion": "20.10.12",
            "Id": "0721d3b351f08b8a337ace23d9e1f99cc9cab25b3459b95359b85054c631a1af",
            . . .
        }
        platform="linux"
        architecture="amd64"
SignerID:    demo-signer
Apikey revoked:    no
Status:        TRUSTED
Dependencies:
        busybox@1.35.0-r17                     899f82d8925d0659b628ab403a44a433bcd97a06 TRUSTED
        musl@1.2.3-r0                          682bb42e6503a00152397e3db87be4602d566ac4 TRUSTED
        alpine-keys@2.4-r1                     1417c88edb049afbaaa0d5e94a15c3726fe68f31 TRUSTED
        alpine-baselayout@3.2.0-r22            97afe73342be73255da8d7e0929d7f73a625ce4d TRUSTED
        . . .
        keyutils-libs@1.6.3-r1                 19eb523e1d62d8b90431763aa3073d30e3283fb2 TRUSTED
        libuuid@2.38-r1                        68bd5e9b8fe99566387e2ad7b7a44c8cf0936673 TRUSTED
        readline@8.1.2-r0                      f676007339535e21de79acffbe7ae743a1f7168c TRUSTED
        .python-rundeps@20220907.232918        2d6f839e7b5f86c10811f4574f044b3b3ad53417 TRUSTED
```

If we then attempt to authenticate the image we just notarized, we can confirm that it is now marked as `Trusted`:

```bash
vcn authenticate --bom image://python:3.9-alpine

Kind:        image
Name:        python:3.9-alpine
Hash:        dca341b7a3fdbe1aa117f97f55321e60fe54a177d6f58ab3373ece796aca52ef
Metadata:    architecture="amd64"
        hashtype="SHA256"
        image={
            "Architecture": "amd64",
            "Created": "2022-09-07 23:29:27.440739972 +0000 UTC",
            "DockerVersion": "20.10.12",
            "Id": "0721d3b351f08b8a337ace23d9e1f99cc9cab25b3459b95359b85054c631a1af",
            . . .
        }
        platform="linux"
SignerID:    demo-signer
Apikey revoked:    no
Status:        TRUSTED
Dependencies:
        alpine-baselayout-data@3.2.0-r22       bf84212e37b7916942f03263f997c94e39494525 TRUSTED
        musl@1.2.3-r0                          682bb42e6503a00152397e3db87be4602d566ac4 TRUSTED
        busybox@1.35.0-r17                     899f82d8925d0659b628ab403a44a433bcd97a06 TRUSTED
        alpine-baselayout@3.2.0-r22            97afe73342be73255da8d7e0929d7f73a625ce4d TRUSTED
        . . .
        keyutils-libs@1.6.3-r1                 19eb523e1d62d8b90431763aa3073d30e3283fb2 TRUSTED
        libuuid@2.38-r1                        68bd5e9b8fe99566387e2ad7b7a44c8cf0936673 TRUSTED
        readline@8.1.2-r0                      f676007339535e21de79acffbe7ae743a1f7168c TRUSTED
        .python-rundeps@20220907.232918        2d6f839e7b5f86c10811f4574f044b3b3ad53417 TRUSTED
```

Let's say there is a hypothetical vulnerability discovered in the package `keyutils-libs@1.6.3-r1`, and we want to ensure we don't run any containers based on images containing this dependency.

We can take the untrusted dependency's hash and mark it as untrusted:

```bash
❯ vcn untrust --hash 19eb523e1d62d8b90431763aa3073d30e3283fb2

Your assets will not be uploaded. They will be processed locally.

Notarization in progress...
artifact notarized

Name:        keyutils-libs
Hash:        19eb523e1d62d8b90431763aa3073d30e3283fb2
Metadata:    hashtype="SHA1"
        license="GPL-2.0-or-later LGPL-2.0-or-later"
        version="1.6.3-r1"
SignerID:    demo-signer
Apikey revoked:    no
Status:        UNTRUSTED
```

Now, when we attempt to notarize any assets that contain our known-vulnerable dependency, we will see that the notarization fails:

```bash
❯ vcn notarize --bom image://python:3.10-alpine

Dependency keyutils-libs@1.6.3-r1 trust level is UNTRUSTED

Error: some dependencies have insufficient trust level so artifact cannot be notarized. You can override it with --bom-force option
```

Because the `keyutils-libs@1.6.3-r1` dependency has already been marked as untrusted with our current signing key, any future attempts to notarize an asset containing that dependency will fail.
