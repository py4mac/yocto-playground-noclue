# Credits
* https://www.youtube.com/watch?v=aPsMuSU-Btw


# Pre-requisites

* Build docker image
A based docker image must be built previously. See https://github.com/py4mac/dotfiles

* Run docker env

```sh
docker-compose run virtual-env
```

> **_NOTE:_**  In case of issue, increase watch file using command ```sudo sysctl -n -w fs.inotify.max_user_watches=1048576```

# Steps

* Collect dependencies repo
```sh
repo init -u https://github.com/py4mac/yocto-manifest.git -b main -m manifest.xml
repo sync
cd src
source poky/oe-init-build-env
```

* Add noclue layer
```sh
bitbake-layers add-layer ../meta-noclue
```

* Build image
```sh
bitbake noclue-image
```

* Test image
```sh
runqemu nographic slirp
```

login is `root`

The new binary file imported is then
```sh
noclue
```

* Exit
```sh
halt
```

# Tips

## Create a new layer before building (see initial build step)
* Create noclue layer
```sh
bitbake-layers create-layer ../meta-noclue
```

* Add noclue layer
```sh
bitbake-layers add-layer ../meta-noclue
```

* Build core image
```sh
bitbake core-image-minimal
```

## Add C/C++ image
At the same level than poky, create **noclue** folder with C/C++ code + CMakeLists and type
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
