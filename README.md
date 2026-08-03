# Abuild util-linux archive

This repository preserves the historical util-linux source revisions formerly
used by Abuild under the old `util-linux-ng` submodule name. The authoritative
project is maintained in the
[kernel.org util-linux repository](https://git.kernel.org/pub/scm/utils/util-linux/util-linux.git/);
its [GitHub repository](https://github.com/util-linux/util-linux) is the
official backup and collaboration mirror. The project itself was named
util-linux-ng between 2006 and 2010.

The long-lived branches have deliberately separate roles:

- `master` follows the official upstream `master` branch;
- `abuild` is the final source revision selected by Abuild;
- `archive/abuild-pin-e35f5287` preserves Abuild's earlier source revision;
  and
- `abuild-gh` adds only this maintenance README and files below `.github/` to
  `abuild`.

The `abuild` branch is exactly the official `v2.36.1` tag at `35c07c82`.
Abuild initially selected the official `v2.17.2` tag at `e35f5287`; the
dedicated archive branch keeps that older Gitlink target directly named.

Abuild used only the static `libuuid` library from this submodule, as a QEMU
build dependency. It later removed the vendored util-linux source and now uses
distribution tooling and libraries. These branches are retained for provenance
and reproducibility, not as recommended util-linux versions for new systems.

## Continuous integration

Run the same Trixie check locally with Docker:

```sh
.github/ci/run .github/ci/check
```

The check regenerates the historical Autotools files, configures only
`libuuid`, builds and stages the static library, runs its two applicable
non-privileged upstream tests, and checks the installed package metadata. Each
test has an independent timeout. The check executes as a non-root user in a
read-only container without network access or Linux capabilities.

The upstream UUID tests are the relevant non-privileged subset for Abuild's
former use. This archive does not claim coverage of util-linux tools that
Abuild never consumed or of tests requiring root, mounts, loop devices, or
other host resources.

The weekly upstream monitor checks whether `master` and the mirrored final
release tags still match the authoritative kernel.org repository. It reports
drift but never updates branches automatically.
