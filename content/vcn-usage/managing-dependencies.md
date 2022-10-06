---
title: Working with Dependencies
weight: 150
draft: false
---

# Aritfacts and Dependencies

## Looking up known dependencies with `vcn`

To find all assets which include a particular component as a dependency, the `--bom-what-includes` flag can be passed to `vcn authenticate`. The command will then return a list of your assets which include the specified component in their SBOM.

```bash
vcn authenicate --bom-what-includes (<scheme>://<name>@<version> | --hash <hash>)
```

With this command, the asset to search for can be specified in the same way as any other variation of the `vcn authenticate` command, but the deprecated `docker://<imageId>` scheme is not supported.

## Container support with `vcn`

When container images are a part of your organization's build and deployment workflow, it's not only critical to have attestation of the images themselves, but also to identify the software components contained within the filesystems of those images. vcn can help you scan any image or container to identify the components contained within:

```bash
vcn [authenticate|notarize|...] <scheme>://<image_or_container> [options]
```

where `<scheme>` is one of `container`, `image`, or `docker`.

### Authenticating Docker images and containers

When the `container` or `docker` schemes are used, vcn will attempt to scan the specified container or image via the Docker daemon running on that machine. The `docker://` scheme takes a Docker image ID as its `<image_or_container>` value, while the `container://` scheme takes a Docker container ID.

### Connecting to a container registry

The `image://` scheme is structured differently:

```console
image://[<registry_server>/]<image_tag>
```

In the `image://` scheme, the `<registry_server>` is optional, and if not specified, vcn will attempt to use Docker Hub via the active Docker session. The `<image_tag>` is required, and must be a valid Docker image tag.

To override the credentials used to connect to the container registry, you can specify the `--image-registry-user` and `--image-registry-password` flags.

### Handling image archives

If a TAR file asset containing an exported container image is passed to a vcn command, vcn will attempt to process the file as a container image, for example:

```bash
vcn authenticate --bom debian-buster.tar
```

## Cascade operations with `vcn`

The hierarchical structure of SBOMs—and of the software dependencies they represent—can be leveraged to ensure that the status of a component is up-to-date across all of its dependents. This is known as a "cascade" operation, and vcn can perform this operation with any of the following commands:

```bash
vcn notarize  [options] --bom-cascade [--bom-force] <artifact>
vcn untrust   [options] --bom-cascade [--bom-force] <artifact>
vcn unsupport [options] --bom-cascade [--bom-force] <artifact>
```

When the `--bom-cascade` flag is passed to any of these commands, vcn will propogate the command to all other assets which include the specified component in their SBOM.

This means, for example, that if you run a `vcn untrust` command with the `--bom-cascade` flag, vcn will mark the specified artifact as untrusted, as well as any other assets which depend on that component.
