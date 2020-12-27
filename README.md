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

Next to the current poky master branch, the manifest is also available for the poky zeus(yocto 3.0) and dunfell (yocto 3.1 - Long Term Support (LTS)) branches.
It can be used with **repo init** with the **-b <branchname>**, for instance for zeus:

    $ repo init -u git@github.com:chlongv/yocto-repo.git -b zeus
