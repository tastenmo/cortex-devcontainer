Place the SEGGER J-Link Linux installer here before rebuilding the devcontainer.

Accepted filenames:
- JLink_Linux_*.deb
- JLink_Linux_*.tgz

The Dockerfile installs the newest matching file it finds in this directory.

Reason: SEGGER's download flow is license-gated and not stable enough to hardcode into an unattended container build.