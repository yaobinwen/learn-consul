# Learn Consul

## Overview

This repository contains the versions of [Consul](https://github.com/hashicorp/consul) source files that I'm interested in and the notes I made for learning purpose. Therefore, the code in this repository belongs to the original authors of Consul and should be used under the same license. See their [license](https://github.com/hashicorp/consul/blob/main/LICENSE).

Each version is in its own sub-folder. My notes are all marked with `NOTE(ywen)`. The log messages I added are prefixed with `[ywen]`.

## Consul versions

- `1.16.1` was created from the tag [`v1.16.1`](https://github.com/hashicorp/consul/releases/tag/v1.16.1).

## How to build

### Build Issues

I ran into the following build issues when I built Consul from source the first time:

1). `go get .` couldn't find `net/netip` (and another package that I don't remember) in `GOROOT`. This should be because I was using an older version of Go which didn't have `net/netip` as a built-in package. I fixed it by upgrading to the latest version of Go.

2). This line `CGO_ENABLED=0 go install -ldflags "$(GOLDFLAGS)" -tags "$(GOTAGS)"` in `GNUmakefile` reported error that suggested I was using invalid link options. The root cause was the variable `$(GOLDFLAGS)` was expanded to the following block:

```
gpg: Signature made Sat 09 Sep 2023 04:53:58 PM UTC
gpg:                using RSA key FD606F0CE398E74E35CD69D40FC0A64AA26EEEF4
gpg: Can't check signature: No public key
2023-09-09T16:53:28Z
```

This was because the `git_date` shell function in `build-support/functions/10-util.sh` shows the signature but I don't have the Consul developers' SSH public keys. I modified the `git show` command that `git_date` calls and passed `--no-show-signature` to fix this issue.
