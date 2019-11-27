# cvmfs-containerd-snapshot

Here I will gather all the information and set up to work on a cvmfs-containerd-snapshotter.
Code name of the project will be `MagPie` for no particular reason. Hopefully we will find a better name.

The goal of this specific document is to distill knowledge and develop-operation experience for myself and other people interested in understading `MagPie`, contribute to it, or write something similar.

## Goal
The overall goal of the project is to have docker/k8s/(anything that uses containers under the hood) be able to run **unmodified** docker images, but picking the unpacked layers from cvmfs.
Of course, it will be necessary to pre-ingest the docker image in cvmfs, and we are ok with that.
This should allow to run big containers in the GRID (or wherever /cvmfs can be mounted) with ease.

## Why now
This work will possible after https://github.com/containerd/containerd/pull/3846 get merged in `containerd` that should happen pretty soon at the time of writing.

## Snapshot
The reference of the snapshotter is here: https://github.com/containerd/containerd/blob/master/snapshots/snapshotter.go#L124

The goal of the snapshotter is to create the filesystem that a container use to run.
The filesystem is created in layer (the docker layer) that are stack one on top of each other to provide the final union filesystem.
The operation of stacking the layer one on top of the other is usually done by overlayfs.

