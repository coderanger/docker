# Porting to FreeBSD
Major milestones for porting docker on FreeBSD are:

* make it compile (DONE)
* make it start as a daemon (DONE)
* load an image and create the container (aka working graphdriver) (DONE)
* run the container 
* working top\start\stop\kill (aka working execdriver)
* working networking aka NAT
* working port forward (aka working networkdriver)
* working volumes and links
* major code cleanup and steps to push code to docker project

# Running
We dont have working docker image on freebsd, and cross-compile doesn't work wery well, so now we need to compile on FreeBSD directly

First we get the sources

    export GOPATH=`pwd`
    go get github.com/docker/docker
    cd src/docker/docker
    git remote set-url origin https://github.com/kvasdopil/docker.git
    git pull
    cd ../../..

Now build the docker

    sh hack/make/.go-autogen
    cp -rp src/github.com/docker/docker/vendor/* .
    go build -tags daemon github.com/docker/docker/docker/docker

This should build the docker executable in current directory. You can run docker with command:
    
    ./docker -d -b none -e jail -s zfs