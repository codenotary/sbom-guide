---
title: "Introduction to VCN"
weight: 110
draft: false
---

## Specifying Assets in `vcn` Commands

Throughout this documentation, you'll see the placeholder `<artifact>` used to refer to an asset that you want to authenticate or notarize. For example:

```bash
vcn authenticate <artifact>
```

The asset referred to by the `<artifact>` placeholder can be a file, directory, image, or git repository. The following are examples of how to specify an asset, where `COMMAND` is a placeholder for any `vcn` command that accepts an asset as an argument:

```bash
vcn COMMAND <file>
vcn COMMAND dir://<directory>
vcn COMMAND image://<imageId>
vcn COMMAND docker://<imageId> // deprecated, please use image
vcn COMMAND podman://<imageId>
vcn COMMAND git://<path_to_git_repo>
vcn COMMAND --hash <hash>
```

These docs will only use the `<artifact>` placeholder in examples, but the actual commands you run should specify the appropriate asset type based on one of the templates defined above.

## Using the `vcn` CLI

To begin using the `vcn` CLI, you must first log in with your credentials for Codenotary Trustcenter. After you generate an API key in Trustcenter, you can log in with the `vcn login` command:

```bash
vcn login --lc-host example.com
```

If you are using `vcn` in a script, you can set the API key in the `VCN_LC_API_KEY` environment variable, and then run the `vcn login` command without the `--lc-host` flag:

```bash
export VCN_LC_API_KEY=<API_KEY>
export VCN_LC_HOST=<TRUSTCENTER_DOMAIN>
export VCN_LC_PORT=443

vcn login
```

You can also specify the API key in an environment variable prefixed to the `vcn login` command.

```bash
VCN_LC_API_KEY=<API_KEY> vcn login --lc-host <TRUSTCENTER_DOMAIN>
```

However, by logging in without your API key present in the appropriate environment variable, the `--signerID` flag becomes mandatory.