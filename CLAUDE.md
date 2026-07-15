# CLAUDE.md

## What this repo is

A fork of [untrunc](https://github.com/anthwlock/untrunc) for building a static `untrunc` binary targeting **AWS Lambda** (x86_64, Amazon Linux 2023 runtime). Untrunc repairs damaged/truncated MP4, M4V, MOV, 3GP videos given a similar healthy reference file from the same camera.

## Build for AWS Lambda

Requires Docker. Produces `lambda-build/untrunc`:

```shell
./build-lambda.sh
```

This builds inside `Dockerfile.lambda` (base `public.ecr.aws/lambda/python:3.13`) using `make FF_VER=6.0`, then extracts the stripped binary. Deploy as a Lambda layer at `/opt/bin/untrunc`.

## Build notes

- FFmpeg 6.0 is compiled from source with a minimal config (decoders + demuxers only).
- The Lambda image ships `binutils 2.41`, which breaks FFmpeg 6.0's x86 inline asm. `ffmpeg-mathops.patch` (the upstream FFmpeg fix `effadce6c756`) is applied before configure. Do not delete this file.
- `.gitignore` has `!ffmpeg-mathops.patch` so the patch stays tracked despite the `ffmpeg-*` ignore rule.

## Other builds (not Lambda)

`make FF_VER=6.0` works on Linux/macOS with system or locally-built FFmpeg. See the upstream README for non-Lambda usage.
