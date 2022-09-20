---
title: "Authenticating Assets with Trustcenter"
weight: 130
draft: false
---

## Authenticate asset with `vcn`

To authenticate an asset with Trustcenter, which verifies the integrity and signature of the asset, use the `vcn authenticate` command:

```bash
Usage:
  vcn authenticate [flags] ARG(s)

Aliases:
  authenticate, a, verify, v

Examples:
  vcn authenticate /bin/vcn

Flags:
      --attach string                    authenticate an artifact on a specific attachment label. With this it's be possible to retrieve the specific attachment with:
                                         vcn a binary1 --attach=vscanner.result:jobid123 --output=attachments
                                         or to get all attachments for a label:
                                         vcn a binary1 --attach=jobid123 --output=attachments
      --bom                              link asset to its dependencies from BOM
      --bom-batch-size uint              by default BOM dependencies are authenticated/notarized in batches of up to 100 dependencies each. Use this flag to set a different batch size. A value of 0 will disable batching (all dependencies will be authenticated/notarized at once). (default 100)
      --bom-cdx-json string              name of the file to output BOM in CycloneDX JSON format
      --bom-cdx-xml string               name of the file to output BOM in CycloneDX XML format
      --bom-deps-only                    authenticate only the dependencies, not the asset
      --bom-diff-base string             hash of the artifact to diff the BOM against
      --bom-max-unsupported float        max number (in %) of unsupported dependencies
      --bom-spdx string                  same as bom-spdx-tv (for backward compatibility)
      --bom-spdx-json string             name of the file to output BOM in SPDX JSON format
      --bom-spdx-tv string               name of the file to output BOM in SPDX tag-value format
      --bom-trust-level string           min trust level: untrusted (unt) / unsupported (uns) / unknown (unk) / trusted (t) (default "trusted")
      --bom-what-includes                output all assets that use the specified asset
      --enforce-signature-verify         if this flag is provided vcn will disable signature auto trusting when connecting to a new Codenotary Cloud server
      --exit-code int                    override default exit codes in case of success
      --experimental                     allow experimental features
      --force                            if provided when downloading attachments files are silently overwritten
      --github-token string              Github OAuth token for querying BOM Github package details. Either authenticated or not, requests are subject to Github limits
      --hash string                      specify a hash to authenticate, if set no ARG(s) can be used
  -h, --help                             help for authenticate
      --image-key-file string            [EXPERIMENTAL] Public key for validating container image signature
      --image-registry-password string   container image registry password (i.e. docker.hub password)
      --image-registry-user string       container image registry user name (i.e. docker.hub user)
      --label string                     label to authenticate against
      --lc-api-key string                Codenotary Cloud server api key
      --lc-cert string                   local or absolute path to a certificate file needed to set up tls connection to a Codenotary Cloud server
      --lc-host string                   if set with host, action will be route to a Codenotary Cloud server
      --lc-ledger string                 Codenotary Cloud ledger. Required when a multi-ledger API key is used.
      --lc-no-tls                        allow insecure connections when connecting to a Codenotary Cloud server
      --lc-port string                   port to connect to Codenotary Cloud. Defaults to 80 when --lc-no-tls is provided, 443 otherwise
      --lc-skip-tls-verify               disables tls certificate verification when connecting to a Codenotary Cloud server
      --lc-uid string                    authenticate on a specific artifact uid
      --name string                      asset name to look up
  -s, --signerID strings                 accept only authentications matching the passed SignerID
      --signing-pub-key string           specify a public key to verify signature in messages when connected to a Codenotary Cloud server. It's required a valid ECDSA key content without header and footer. Ex: --signing-pub-key="MFkwE...y5i4w=="
      --signing-pub-key-file string      specify a public key file path to verify signature in messages when connected to a Codenotary Cloud server. If no public key file is specified but server is signig messages is possible an interactive confirmation of the fingerprint. When confirmed the public key is stored in ~/.vcn-trusted-signing-pub-key file.
      --version string                   asset version to look up

Global Flags:
  -o, --output string                output format, one of: --output=json|--output=yaml|--output=''|--output=no-progress. In Codenotary Cloud authenticate command is possible to specify also --output=attachments. It permits to download all items attached to an artifact.
      --retry-attempts uint          a number of retries to be made; setting this to zero will disable retries
      --retry-initial-backoff uint   initial delay for exponential backoff, in milliseconds (default 1500)
  -S, --silent                       silent mode, don't show progress spinner, but it will still output the result
      --vcnpath string               config files (default is user home directory)
      --verbose                      if true, print additional information

```

Using signerID:

```bash
vcn authenticate --signerID <signerID> <asset>
```