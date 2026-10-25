# ansible-gomplate

Installs gomplate on Red Hat-family Linux hosts with amd64 or arm64 CPUs and
makes it available as `/usr/bin/gomplate`. The role manages no services.

## Variables

- `gomplate_version` defaults to `latest`: checks GitHub for the latest stable
  release on each run and upgrades only when its SemVer is newer. Set an exact
  SemVer string without a leading `v`, such as `"5.2.0"`, to enforce that version,
  including downgrades. Empty and null values are invalid.
- `gomplate_checksum` defaults to `""`. Set a 64-character hexadecimal SHA-256
  checksum to override checksum discovery. Pair an override with an exact version
  to avoid applying it to a future release.
- `gomplate_checksums` supplies embedded SHA-256 pins, keyed by exact version and
  then `amd64` or `arm64`. It defaults to pins for `5.2.0`. Unlisted versions use
  the exact upstream asset digest, or the release checksum manifest when that
  digest is absent.

Run with gathered facts and root privileges. The managed host needs HTTPS access
to GitHub for release discovery and downloads. Shared transfer helpers provide
bounded retries. Release discovery uses GitHub's unauthenticated API and honors
rate-limit waits within its retry budget; requests still fail when recovery
exceeds that budget. An already-current pinned version needs neither discovery
nor downloads. Downloads always require a verified checksum. Check mode performs
read-only version and metadata checks but does not download the binary.
