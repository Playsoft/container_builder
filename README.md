container_builder
=========
Simple shell script for creating minimal http://docker.io container images. It copy files from given list with all dependency they have into base filesystem tar. Then you can import this tar by `docker import` command. So you first need to install required software on machine you plan to use for building image and create a list of files to copy. Idea was to have very small docker images to be used in Dockerfiles as base for dev code. Fully working nginx server is a 12M image as reported by `docker images`. Created and tested on Ubuntu 13.4

-----------

Usage:
-----------

    ./container_builder -f path [-h?vR] [-B path] [-o path] [-t path]
    
    Arguments:
    -h -?       - Show this help. And exit.
    -v          - Be more verbose.
    -B <path>   - Base image tarball location. Defaults to <script_dir>/rootfs.tar
    -f <path>   - Input file path. Imput file contain files and/or dirs to add into base image.
    -o <path>   - Output image tarball file name. Defaults to <scriptdir>/build-image.tar
    -t <path>   - Temporary working dir. Defaults to /tmp/container_builder/
    -R          - Do not remove temporary working dir after finish (speedups next build).

Example: 

    ./container_builder -f nginx-files -B rootfs.tar -o nginx_base.tar

    nginx-files content:
    /usr/sbin/nginx
    /etc/nginx/
    /lib/x86_64-linux-gnu/libnss_compat.so.2
    /lib/x86_64-linux-gnu/libnsl.so.1
    /lib/x86_64-linux-gnu/libnss_files.so.2
    /lib/x86_64-linux-gnu/libnss_nis.so.2
    /var/log/nginx

-----------

Requiremnets
-----------
* base linux image for x86_64 arch (I use http://buildroot.uclibc.org/ to create my base and it's 1.4MB in size)
* rsync
* tar, find, stat, awk, ldd (all common on many distributions)
