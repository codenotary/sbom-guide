---
weight: 410
title: Intro to CAS
draft: true
---

## About Codenotary's Community Attestation Service

TODO.

## Using CAS to Notarize and Authenticate

TODO: Intro.

### `cas` CLI Quickstart

```bash
# Get API key from: https://cas.codenotary.com

# Get CAS:
https://github.com/codenotary/cas/releases

# Login
export CAS_API_KEY=YOUR_API_KEY; cas login

## Create and sign a Software Bill of Materials
cas notarize --bom docker://ubuntu

## Verify and authenticate assets with signing key
cas authenticate --bom docker://ubuntu
```


### Summary of Functionality

TODO: Brief listing of all functionality.

### Basic Notarization and Authentication

```bash
# Notarize image
cas notarize docker://container_image

# Authenticate image
cas authenticate docker://container -SignerID.

# Inspect image
cas inspect docker://container_image

# Verify image
cas authenticate docker://container_image && docker run container_image
```

### Authenticating Trusted Dependencies

If we attempt to authenticate a Docker image which has not yet been notarized with out signing key, we receive a warning that the asset hasn't yet been notarized, as well as a listing of the asset's dependencies, their hashes, and their authentication status (`Unknown`, in this case).

```bash
❯ cas authenticate --bom docker://python:3.9-alpine

Resolving dependencies...

Authenticating dependencies...
 100% |██████████████████████████████████████████████████████| (36/36, 59 it/s)

.python-rundeps@20220907.231849   b9bddeccfab9c3d7731f6b39360dcf3cfdeb1b7f Unknown
alpine-baselayout@3.2.0-r22       3c6c70ccb77b490fd2663506ae7727a638eda4a6 Unknown
alpine-baselayout-data@3.2.0-r22  d6554033bbe7f571edc82954fd97e59aa4c7f045 Unknown
alpine-keys@2.4-r1                cffd2a49107574ba448f4b23b4bfc597676b9054 Unknown
. . .

Warning: c9b90024bc4d49b1fa0ea4673b6eb1db1058cd1cba4b840d336bedf803a0afcf was not notarized
```

We can then notarize this image, which resolves all dependencies, authenticates them against CAS, and signs the `Unknown` dependencies, changing the status to `Trusted`.

```bash
❯ cas notarize --bom docker://python:3.9-alpine

Resolving dependencies...

Authenticating dependencies...
 100% |██████████████████████████████████████████████████████| (36/36, 58 it/s)

Notarizing 36 dependencies ...
 100% |██████████████████████████████████████████████████████| (36/36, 39 it/s)

.python-rundeps@20220907.231849   b9bddeccfab9c3d7731f6b39360dcf3cfdeb1b7f Trusted
alpine-baselayout@3.2.0-r22       3c6c70ccb77b490fd2663506ae7727a638eda4a6 Trusted
alpine-baselayout-data@3.2.0-r22  d6554033bbe7f571edc82954fd97e59aa4c7f045 Trusted
. . .

Your assets will not be uploaded. They will be processed locally.

Kind:   docker
Name:   docker://python:3.9-alpine
Hash:   c9b90024bc4d49b1fa0ea4673b6eb1db1058cd1cba4b840d336bedf803a0afcf
Metadata: architecture="arm64"
    docker={
        "Architecture": "arm64",
        "Created": "2022-09-07T23:19:03.452996827Z",
        "DockerVersion": "20.10.12",
        "Id": "sha256:c9b90024bc4d49b1fa0ea4673b6eb1db1058cd1cba4b840d336bedf803a0afcf",
        . . .
    }
    platform="linux"
    version="3.9-alpine"

SignerID: bmlja0Babcdefgh12345==
Apikey revoked: no
Status:   TRUSTED
Dependencies:
    .python-rundeps@20220907.231849   b9bddeccfab9c3d7731f6b39360dcf3cfdeb1b7f
    alpine-baselayout@3.2.0-r22       3c6c70ccb77b490fd2663506ae7727a638eda4a6
    alpine-baselayout-data@3.2.0-r22  d6554033bbe7f571edc82954fd97e59aa4c7f045
    alpine-keys@2.4-r1                cffd2a49107574ba448f4b23b4bfc597676b9054
    apk-tools@2.12.9-r3               bd9d72a8be3f3e5f046759c4e82086b6b7195622
    busybox@1.35.0-r17                31ea3e2c718f4a2dee63d808a2e1156fdcfc15ba
    . . .
```

Now, if we attempt to authenticate the next version of the image (`python:3.10-alpine` instead of `python:3.9-alpine`, we are again warned that the asset has not been notarized. Unlike in the previous `cas authenticate` example, however, all but one of the resolved dependences is already marked as `Trusted`.

```bash
❯ cas authenticate --bom docker://python:3.10-alpine

Resolving dependencies...

Authenticating dependencies...
 100% |██████████████████████████████████████████████████████| (36/36, 17 it/s)

.python-rundeps@20220907.223701   f3105e48f2a5caae5d0d2b6cbba5468a06a111c2 Unknown
alpine-baselayout@3.2.0-r22       3c6c70ccb77b490fd2663506ae7727a638eda4a6 Trusted
alpine-baselayout-data@3.2.0-r22  d6554033bbe7f571edc82954fd97e59aa4c7f045 Trusted
alpine-keys@2.4-r1                cffd2a49107574ba448f4b23b4bfc597676b9054 Trusted
apk-tools@2.12.9-r3               bd9d72a8be3f3e5f046759c4e82086b6b7195622 Trusted
busybox@1.35.0-r17                31ea3e2c718f4a2dee63d808a2e1156fdcfc15ba Trusted
. . .

Warning: 85e5afa95f3f0b68ffc12ae1f57557dc2115bb822c9e9ff0278 was not notarized
```

After `cas authenticate` resolves the container's dependencies (using Alpine's package manager to enumerate installed packages), it attempts to authenticate those dependencies using the current signing key. Because the underlying image containing the operating system is the same for both `python:3.9-alpine` and `python:3.10-alpine`, the only detected difference is the Python installation.

As you can see, not only does CAS help keep `Untrusted` software out of your production environment, but it can also reduce the burden of assessing dependency changes by indicating which have already been trusted.

### Notarizing container images

TODO.

### Notarizing source code

TODO.

{{< tabs "notarize-source-code" >}}

  {{< tab "Python" >}}

   ```Python
   python examples here
   ```

  {{< /tab >}}

  {{< tab "Go" >}}

   ```Go
   go example here
   ```

  {{< /tab >}}

  {{< tab "Java" >}}

   ```Java
   Java example here
   ```

  {{< /tab >}}

  {{< tab "Node" >}}

   ```JavaScript
   Node example here
   ```

  {{< /tab >}}

  {{< tab ".net" >}}

   ```csharp
   .net example here
   ```

  {{< /tab >}}

  {{< tab "PHP" >}}

   ```PHP
   PHP example here
   ```

  {{< /tab >}}

  {{< tab "Rust" >}}

   ```Rust
   Rust example here
   ```

  {{< /tab >}}

{{< /tabs >}}

### Notarizing packages

TODO.



