---
title: Working with Dependencies
weight: 150
draft: true
---

## Looking up known dependencies with `vcn`

To

```bash
vcn a --bom-what-includes (<scheme>://<name>@<version> | --hash <hash>)
```

### Container support with `vcn`

To

```bash
vcn <command> <scheme>://<image_or_container> [command options]
```

<!-- TODO:
> Supported schemes:
> - docker: Docker image ID or tag, requires running docker deamon container
> - container: Docker container ID or tag, requires running docker deamon image
> - image: container image in container registry. URL format: image://[<registry_server>/]image_tag. If <registry_server> is not specified, Docker Hub is used. By default vcn tries to connect to the registry using active Docker session, however users can always override it by using image-registry-user/image-registry-password CLI parameters. -->

### Cascade operations with `vcn`

To

```bash
vcn notarize|untrust|unsupport [command options ...] --bom-cascade [--bom-force]
```
