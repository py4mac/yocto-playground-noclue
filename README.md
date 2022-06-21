# Credits
* https://www.youtube.com/watch?v=aPsMuSU-Btw


# Pre-requisites

## Build docker image
A based docker image must be built previously. See https://github.com/py4mac/dotfiles

## Run docker env
```sh
docker-compose run virtual-env
```

## Warnings
In case of issue, increase watch file using command
```sh
sudo sysctl -n -w fs.inotify.max_user_watches=1048576
```

# Steps

# Collect dependencies repo
```sh
repo init -u https://github.com/py4mac/yocto-manifest.git -b main -m manifest.xml
repo sync
cd src
```

## Initial build
```sh
bitbake-layers add-layer ../meta-noclue
bitbake core-image-minimal
bitbake noclue
bitbake noclue-image
```

## Test new image
```sh
runqemu nographic slirp
```

login is `root`

The new binary file imported is then
```sh
noclue
```

## Exit
```sh
halt
```

# Tips

## Create a new layer before building (see initial build step)
```sh
bitbake-layers create-layer ../meta-noclue
bitbake-layers add-layer ../meta-noclue
bitbake core-image-minimal
```

## Add C/C++ image
Create folder at same level than poky, with C/C++ code + CMakeLists
then
`devtool add --no-same-dir ../noclue`

Copy `*.bb` files and `*.bbappend` files in `meta-noclue` folder (`meta-noclue/recipes-noclue/c`)

Create image in `meta-noclue/recipes-noclue/images/noclue-image.bb` folder with content:

```
require recipes-core/images/core-image-minimal.bb

IMAGE_INSTALL += "noclue"
```

Then
```sh
bitbake noclue
bitbake noclue-image
```