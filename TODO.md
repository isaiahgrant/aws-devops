# TODO

1. When going to deploy the next unique image (Alpine3.12-Node12.20-Python3.8.5), I ran into some issues with compatibility of Alpine packages. For starters I had to downgrade Alpine version just to access those packages. Additionally, the Dockerfile was no longer standardized as dependencies changed from using r0 to r1. Rather than making these arguments as well, I moved to keeping a historical record of build files. This strategy needs further review.

1. Reduction of the image size. The one drawback here is that we _need_ these utilities (node, python, aws*) available at runtime, so they cannot simply be removed after building like in multi-stage builds. The original 186MB compressed and ~600MB uncompressed was _abysmal_. Making apk dependencies virtual and removing them at the end of the build brought the image down ~245MB. Removing cache dir in pip installs brought it down ~30MB. Removing npm cache brought it down another ~20MB.

1. Automation. It would be great if releases of this image could be automated based on releases of it's dependencies, but this may be challenging due to compatibility. Some options would be to "try run" builds and include automated testing on the build before releasing. Another could be to maintain a mapping, but this still has a prominent manual element as I am not sure if these can be obtained programmatically.
