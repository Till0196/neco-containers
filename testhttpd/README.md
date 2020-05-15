testhttpd container
===============

This directory provides container image `testhttpd` and contains its source code.

testhttpd is a micro HTTP server that can run in Kubernetes cluster with limited privileges.
Specifically, it runs as a non-root user and does not write to the root filesystem.


Usage
-----

```console
$ kubectl run quay.io/cybozu/testhttpd
``` 

Docker images
-------------

Docker images are available on [Quay.io](https://quay.io/repository/cybozu/testhttpd)
