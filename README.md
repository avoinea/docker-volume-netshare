# Docker NFS, AWS EFS & Samba/CIFS Volume Plugin

[![Build Status](https://travis-ci.org/gondor/docker-volume-netshare.svg)](https://travis-ci.org/gondor/docker-volume-netshare)

Mount NFS v3/4, AWS EFS or CIFS inside your docker containers.  This is a docker plugin which enables these volume types to be directly mounted within a container.

## NFS Prerequisites on Linux

NFS needs to be installed on Linux systems in order to properly mount NFS mounts.  

- For Ubuntu/Debian: `sudo apt-get install -y nfs-common`
- For RHEL/CentOS: `sudo yum install -y nfs-utils`

It is recommend to try mounting an NFS volume to eliminate any configuration issues prior to running the plugin:
```
sudo mount -t nfs4 1.1.1.1:/mountpoint /target/mount
```

## Installation

#### From Source

```
$ go get github.com/gondor/docker-volume-netshare
$ go build
```

#### From Binaries

* Architecture i386 [ [linux](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_linux_386.tar.gz?direct) / [netbsd](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_netbsd_386.zip?direct) / [freebsd](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_freebsd_386.zip?direct) / [openbsd](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_openbsd_386.zip?direct) ]
* Architecture amd64 [ [linux](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_linux_amd64.tar.gz?direct) / [netbsd](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_netbsd_amd64.zip?direct) / [freebsd](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_freebsd_amd64.zip?direct) / [openbsd](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_openbsd_amd64.zip?direct) ]
* Debian Package [ [i386](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_i386.deb?direct) ] / [amd64](https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_amd64.deb?direct) ] ]

#### On Ubuntu / Debian

The method below will install the sysvinit and /etc/default options that can be overwritten during service start.

1. Install the Package

```
  $ wget https://dl.bintray.com//content/pacesys/docker/docker-volume-netshare_0.5_i386.deb
  $ sudo dpkg -i docker-volume-netshare_0.5_i386.deb
```

2. Modify the startup options in `/etc/default/docker-volume-netshare`
3. Start the service `service docker-volume-netshare start`


## Usage

#### Launching in NFS mode

**1. Run the plugin - can be added to systemd or run in the background**

```
  $ sudo docker-volume-netshare nfs
```

**2. Launch a container**

```
  $ docker run -i -t --volume-driver=nfs -v nfshost/path:/mount ubuntu /bin/bash
```

#### Launching in EFS mode

**1. Run the plugin - can be added to systemd or run in the background**

```
  // With File System ID resolution to AZ / Region URI
  $ sudo docker-volume-netshare efs
  // For VPCs without AWS DNS - using IP for Mount
  $ sudo docker-volume-netshare efs --noresolve
```

**2. Launch a container**

```
  // Launching a container using the EFS File System ID
  $ docker run -i -t --volume-driver=efs -v fs-2324532:/mount ubuntu /bin/bash
  // Launching a container using the IP Address of the EFS mount point (--noresolve flag in plugin)
  $ docker run -i -t --volume-driver=efs -v 10.2.3.1:/mount ubuntu /bin/bash
```

#### Launching in Samba/CIFS mode

**1. Run the plugin - can be added to systemd or run in the background**

```
  $ sudo docker-volume-netshare cifs --username user --password pass --domain domain
```

**2. Launch a container**

```
  // In CIFS the "//" is omitted and handled by netshare
  $ docker run -i -t --volume-driver=cifs -v cifshost/share:/mount ubuntu /bin/bash
```

## License

This software is licensed under the Apache 2 license, quoted below.

Copyright 2015 Jeremy Unruh

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
