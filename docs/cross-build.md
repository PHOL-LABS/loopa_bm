# Raspberry Pi Zero cross-compilation CI

This repository now includes a GitHub Actions workflow that cross-compiles the project for Raspberry Pi Zero/Zero W by leveraging Docker Buildx, following the approach described in [Roland's Dev Blog](https://rolandjitsu.com/posts/cross-compile-for-raspberry-pi-with-docker/).

## What the workflow does
- Uses `ci/Dockerfile.rpi-zero-cross` to provision an ARMv6 toolchain (`arm-linux-gnueabihf`) on Debian.
- Installs the expected Raspberry Pi system libraries: **RtAudio**, **RtMidi**, **wiringPi**, **libsamplerate**, and the legacy GPU headers/libs from the Raspberry Pi userland tree (installed under `/usr/arm-linux-gnueabihf/opt/vc`).
- Builds the project with CMake using the cross toolchain and publishes the resulting `loopa` binary as a build artifact.

## Running locally
If you have Docker 19.03+ and Buildx available (matching the tooling from the blog post), you can reproduce the CI build locally:

```bash
docker buildx build \
  -f ci/Dockerfile.rpi-zero-cross \
  --target artifact \
  --output type=local,dest=./artifacts \
  .
```

The binary will be placed at `./artifacts/out/loopa`.
