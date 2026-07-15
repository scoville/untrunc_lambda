Untrunc
=======

Restore a damaged (truncated) mp4, m4v, mov, 3gp video. Provided you have a similar not broken video. And some luck.

You need:

* Another video file which isn't broken
* ~~Basic ability to use a command line~~ ([GUI](#GUI) exists)

## AWS Lambda Compatible Build

This is a fork specifically configured for AWS Lambda deployment. For building a binary compatible with AWS Lambda:

```shell
# Build for Lambda (requires Docker)
./build-lambda.sh
```

This creates a stripped x86_64 ELF binary at `lambda-build/untrunc`, built inside the AWS Lambda Python 3.13 runtime image. It is dynamically linked against the Lambda runtime's own glibc/libstdc++/libm/libgcc_s (all resolve to `/lib64/` in the Lambda environment), so it runs as a Lambda layer without bundling any libraries. The build applies `ffmpeg-mathops.patch` (upstream FFmpeg fix `effadce6c75`) so FFmpeg 6.0 compiles with binutils >= 2.41 shipped by the Lambda image.
