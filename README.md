Docker in Docker Save (dind-save)
=================================

This is a build repository for a docker image built on top of `dind` with a patched dockerd binary that hacks `docker save` to only save the last layer of an image, yet retains the ability to `docker load` the output archive.

The patched binary is built from https://github.com/brthor/engine

## Usage
Usage typically works by bind mounting `/var/lib/docker` into the `dind-save` container. It's best to understand the risks while doing this. YMMV.

Example:
`docker run --privileged -v /var/lib/docker/image:/var/lib/docker/image -v /var/lib/docker/overlay2:/var/lib/docker/overlay2 -p 127.0.0.1:7777:2375 brthornbury/dind-save:18.09`

Then you can perform a docker save of the last layer of your image like this:
`docker -H 127.0.0.1:7777 save your_image_tag -o ./your_archive_name.tar.gz`