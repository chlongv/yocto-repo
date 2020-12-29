# yocto-repo

A simple repo (https://gerrit.googlesource.com/git-repo/) manifest to bring together several open-embedded meta layers that I use to hack with the Yocto project/OpenEmbedded.

## Usage

Since **repo** is a script, it can easily be installed so (taken from https://gerrit.googlesource.com/git-repo/+/refs/heads/master/README.md):

    $ mkdir -p ~/.bin
    $ PATH="${HOME}/.bin:${PATH}"
    $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
    $ chmod a+rx ~/.bin/repo

The next step is to initialize repo manifest in the directory where you want to checkout the git repos (in this case in the created yocto dir), with the **repo init** command:

    $ mkdir yocto
    $ cd yocto
    $ repo init -u git@github.com:chlongv/yocto-repo.git

Finally, to checkout all the git repos (here poky + the various meta layers), the **repo sync** command shall be used:

    $ repo sync

## Included meta layers

* poky (of course): git://git.yoctoproject.org/poky
* meta-openembedded: git://git.openembedded.org/meta-openembedded
* meta-clang: git://github.com/kraj/meta-clang
* meta-raspberrypi: git://git.yoctoproject.org/meta-raspberrypi
* meta-security: git://git.yoctoproject.org/meta-security
* meta-webkit: git://github.com/Igalia/meta-webkit

## Branches

In addition to the current master branch, the manifest is also available for the zeus (yocto 3.0), dunfell (yocto 3.1 - Long Term Support (LTS)), gatesgarth (yocto 3.2) branches.
It can be used with **repo init** with the **-b <branchname>**, for instance for zeus:

    $ repo init -u git@github.com:chlongv/yocto-repo.git -b zeus

For some meta layers, the branch may stay on master as long as the corresponding branch is not available upstream.

## Build in a container

It is interesting to be independent from the host OS when building with yocto. This is especially relevant when one wants to build something based on an old yocto release, that maybe is not supported anymore on a current, up-to-date, Linux distribution. A container build image is perfect for this. The reference for this is crops/poky-container: https://github.com/crops/poky-container

The below commands assume a Linux host and docker is used as container engine. In addition to the standard documented crops/poky image usage, a cache that can be shared between the different builds, with the help of [site.conf](site.conf) that targets the /cache volue, is added (site.conf has to be copied to build/conf/site.conf).

    $ docker run --rm -it -v /home/val/yocto:/work -v /home/val/yocto/cache:/cache crops/poky --workdir=work

From there, a yocto build can be done as usual. To use another image than the default one (ubuntu-18.04 at the time of writing), it has to be appended to the crops/poky container image:

    $ docker run --rm -it -v /home/val/yocto:/work -v /home/val/yocto/cache:/cache crops/poky:debian-9 --workdir=work

Small remark about runqemu in the docker container: The options *nographic* (no display avaialble) and *slirp* (user networking, no root priviledge required) have to be added for qemu to be able to run the built image.
