---
title: Docker/Podman のセキュリティ機能比較
tags:
  - Linux
  - Podman
private: false
updated_at: '2023-11-19T03:55:10+09:00'
id: d8a77c7b67facf3c6526
organization_url_name: null
slide: false
ignorePublish: false
---
# Docker/Podman のセキュリティ機能比較

## 概要

Podman in Action
Podman イン・アクション

## 環境

- OS：Fedora 39 (VirtualBox)
- コンテナエンジン：Docker 24.0.7/Podman 4.7.2
- コンテナランタイム：runc
- Cgroupsバージョン：v2
- ストレージドライバ：fuse-overlayfs

`Fedora 39`のVMを2つ作成して、それぞれのVM上に`Docker/Podman`をインストールする。  
なるべく同じ環境になるように環境構築する。

<details><summary>docker info</summary><div>

```json
Client:
 Version:    24.0.7
 Context:    default
 Debug Mode: false

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 2
 Server Version: 24.0.7
 Storage Driver: fuse-overlayfs
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 091922f03c2762540fd057fba91260237ff86acb
 runc version: v1.1.9-0-gccaecfc
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  rootless
  cgroupns
 Kernel Version: 6.5.10-300.fc39.x86_64
 Operating System: Fedora Linux 39 (Server Edition)
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 7.741GiB
 Name: localhost.localdomain
 ID: 73e56d8c-7ad6-47b2-a029-570a6ffde314
 Docker Root Dir: /home/test/.local/share/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine

WARNING: No cpuset support
WARNING: No io.weight support
WARNING: No io.weight (per device) support
WARNING: No io.max (rbps) support
WARNING: No io.max (wbps) support
WARNING: No io.max (riops) support
WARNING: No io.max (wiops) support
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```
</div></details>

<details><summary>podman info</summary><div>

```json
host:
  arch: amd64
  buildahVersion: 1.32.0
  cgroupControllers:
  - cpu
  - io
  - memory
  - pids
  cgroupManager: systemd
  cgroupVersion: v2
  conmon:
    package: conmon-2.1.7-3.fc39.x86_64
    path: /usr/bin/conmon
    version: 'conmon version 2.1.7, commit: '
  cpuUtilization:
    idlePercent: 92.3
    systemPercent: 4.86
    userPercent: 2.84
  cpus: 4
  databaseBackend: boltdb
  distribution:
    distribution: fedora
    variant: workstation
    version: "39"
  eventLogger: journald
  freeLocks: 2048
  hostname: fedora-podman
  idMappings:
    gidmap:
    - container_id: 0
      host_id: 1000
      size: 1
    - container_id: 1
      host_id: 524288
      size: 65536
    uidmap:
    - container_id: 0
      host_id: 1000
      size: 1
    - container_id: 1
      host_id: 524288
      size: 65536
  kernel: 6.5.6-300.fc39.x86_64
  linkmode: dynamic
  logDriver: journald
  memFree: 5933297664
  memTotal: 8312250368
  networkBackend: netavark
  networkBackendInfo:
    backend: netavark
    dns:
      package: aardvark-dns-1.7.0-2.fc39.x86_64
      path: /usr/libexec/podman/aardvark-dns
      version: aardvark-dns 1.7.0
    package: netavark-1.7.0-2.fc39.x86_64
    path: /usr/libexec/podman/netavark
    version: netavark 1.7.0
  ociRuntime:
    name: runc
    package: runc-1.1.8-1.fc39.x86_64
    path: /usr/bin/runc
    version: |-
      runc version 1.1.8
      spec: 1.0.2-dev
      go: go1.21.0
      libseccomp: 2.5.3
  os: linux
  pasta:
    executable: /usr/bin/pasta
    package: passt-0^20230908.g05627dc-1.fc39.x86_64
    version: |
      pasta 0^20230908.g05627dc-1.fc39.x86_64-pasta
      Copyright Red Hat
      GNU General Public License, version 2 or later
        <https://www.gnu.org/licenses/old-licenses/gpl-2.0.html>
      This is free software: you are free to change and redistribute it.
      There is NO WARRANTY, to the extent permitted by law.
  remoteSocket:
    exists: false
    path: /run/user/1000/podman/podman.sock
  security:
    apparmorEnabled: false
    capabilities: CAP_CHOWN,CAP_DAC_OVERRIDE,CAP_FOWNER,CAP_FSETID,CAP_KILL,CAP_NET_BIND_SERVICE,CAP_SETFCAP,CAP_SETGID,CAP_SETPCAP,CAP_SETUID,CAP_SYS_CHROOT
    rootless: true
    seccompEnabled: true
    seccompProfilePath: /usr/share/containers/seccomp.json
    selinuxEnabled: true
  serviceIsRemote: false
  slirp4netns:
    executable: /usr/bin/slirp4netns
    package: slirp4netns-1.2.1-1.fc39.x86_64
    version: |-
      slirp4netns version 1.2.1
      commit: 09e31e92fa3d2a1d3ca261adaeb012c8d75a8194
      libslirp: 4.7.0
      SLIRP_CONFIG_VERSION_MAX: 4
      libseccomp: 2.5.3
  swapFree: 8312057856
  swapTotal: 8312057856
  uptime: 0h 2m 9.00s
plugins:
  authorization: null
  log:
  - k8s-file
  - none
  - passthrough
  - journald
  network:
  - bridge
  - macvlan
  - ipvlan
  volume:
  - local
registries:
  search:
  - registry.fedoraproject.org
  - registry.access.redhat.com
  - docker.io
  - quay.io
store:
  configFile: /home/test/.config/containers/storage.conf
  containerStore:
    number: 0
    paused: 0
    running: 0
    stopped: 0
  graphDriverName: overlay
  graphOptions:
    overlay.mount_program:
      Executable: /usr/bin/fuse-overlayfs
      Package: fuse-overlayfs-1.12-2.fc39.x86_64
      Version: |-
        fusermount3 version: 3.16.1
        fuse-overlayfs: version 1.12
        FUSE library version 3.16.1
        using FUSE kernel interface version 7.38
  graphRoot: /home/test/.local/share/containers/storage
  graphRootAllocated: 52610203648
  graphRootUsed: 3985653760
  graphStatus:
    Backing Filesystem: btrfs
    Native Overlay Diff: "false"
    Supports d_type: "true"
    Supports shifting: "true"
    Supports volatile: "true"
    Using metacopy: "false"
  imageCopyTmpDir: /var/tmp
  imageStore:
    number: 0
  runRoot: /run/user/1000/containers
  transientStore: false
  volumePath: /home/test/.local/share/containers/storage/volumes
version:
  APIVersion: 4.7.0
  Built: 1695838680
  BuiltTime: Thu Sep 28 03:18:00 2023
  GitCommit: ""
  GoVersion: go1.21.1
  Os: linux
  OsArch: linux/amd64
  Version: 4.7.0
```
</div></details>

## セキュリティ

以下の項目について、設定の可否・行いやすさなどの観点で評価を行う。  
基本的にはデフォルト設定を基準とする。

- Rootless対応
- アーキテクチャ
- mountされるパス
- Capability
- SELinux
- Seccomp

### Rootless対応

`Podman/Docker`ともにRootless対応している。  
Podmanはパッケージをインストールするだけでよいが、Dockerは、dockerdをRootlessで実行するために、  
`dockerd-rootless.sh`などを追加でインストールする必要がある。  
また、dockerdを起動するため、`systemctl`の操作が必要になる（初回のみ）。

#### Docker

以下に示す公式ドキュメントを参照
https://docs.docker.com/engine/security/rootless/#install

```console
test@fedora-docker:~$ curl -fsSL https://get.docker.com/rootless | sh
★rootless用のdockerインストール

test@fedora-docker:~$ cat << EOF >> ~/.bashrc
export PATH=/home/$(whoami)/bin:\$PATH
export DOCKER_HOST=unix:///run/user/$(id -u)/docker.sock
EOF
test@fedora-docker:~$ source ~/.bashrc
★環境変数を設定

test@fedora-docker:~$ systemctl --user enable docker
★dockerdの起動

test@fedora-docker:~$ systemctl --user enable docker
test@fedora-docker:~$ sudo loginctl enable-linger $(whoami)
★システム起動時にdockerdが起動するように設定

test@fedora-docker:~$ docker run --rm hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:c79d06dfdfd3d3eb04cafd0dc2bacab0992ebc243e083cabe208bac4dd7759e0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
... snip ...
★コンテナが正常に起動できることを確認
```

#### Podman

```console
test@fedora-podman:~$ sudo dnf install -y 
★podmanインストール

test@fedora-podman:~$ podman run --rm hello-world
Resolved "hello-world" as an alias (/etc/containers/registries.conf.d/000-shortnames.conf)
Trying to pull quay.io/podman/hello:latest...
Getting image source signatures
Copying blob d08b40be6878 done   | 
Copying config e2b3db5d4f done   | 
Writing manifest to image destination
!... Hello Podman World ...!

         .--"--.           
       / -     - \         
      / (O)   (O) \        
   ~~~| -=(,Y,)=- |         
    .---. /`  \   |~~      
 ~/  o  o \~~~~.----. ~~   
  | =(X)= |~  / (O (O) \   
   ~~~~~~~  ~| =(Y_)=-  |   
  ~~~~    ~~~|   U      |~~ 

Project:   https://github.com/containers/podman
Website:   https://podman.io
Documents: https://docs.podman.io
Twitter:   @Podman_io
★コンテナが正常に起動できることを確認
```

### アーキテクチャ

Dockerは、すべてのコンテナについてdockerdが監視しているため、dockerdが落ちるとすべてのコンテナも落ちる。  
一方でpodmanは、conmonというプロセスがコンテナごとに生成されて、そのプロセスがコンテナを監視する役目を担う。  
仮にconmonが落ちても、そのconmonが監視していたコンテナのみが落ちる。  
そのため、可用性の面でpodmanの方が優れていると言える。

#### Docker

Rootlessであっても、dockerdがデーモンとして稼働している構図は変わらない。  
コンテナを起動すると、コンテナランタイムのcontainerd、runcが起動して残る。

```console
test@fedora-docker:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES

test@fedora-docker:~$ ps aux | grep dockerd
test        3598  0.0  0.1 717128  8576 ?        Ssl  00:44   0:00 rootlesskit ... snip ... /home/test/bin/dockerd-rootless.sh
test        3609  0.0  0.0 717384  7552 ?        Sl   00:44   0:00 /proc/self/exe ... snip ... /home/test/bin/dockerd-rootless.sh
test        3638  0.0  0.8 1579540 65664 ?       Sl   00:44   0:00 dockerd
... snip ...
★dockerdに加え、rootlesskitも実行しているように見える

test@fedora-docker:~$ docker run -d --name test nginx
e1473cf4bc0906f5f6e76a4e09f7fc3883c2eb42aa5ff91976fd9036f8d04930
★コンテナを実行

test@fedora-docker:~$ ps aux | grep docker
test        3598  0.0  0.1 717128  8576 ?        Ssl  00:44   0:00 rootlesskit ... snip ... /home/test/bin/dockerd-rootless.sh
test        3609  0.0  0.0 717384  7552 ?        Sl   00:44   0:00 /proc/self/exe ... snip ... /home/test/bin/dockerd-rootless.sh
test        3638  0.4  0.8 1800736 68524 ?       Sl   00:44   0:31 dockerd
test        3661  0.0  0.4 751288 38128 ?        Ssl  00:44   0:05 containerd --config /run/user/1000/docker/containerd/containerd.toml
test        5728  0.2  0.0   5880  2052 ?        Ss   02:33   0:00 fuse-overlayfs ... snip ...
test        5737  0.2  0.1 722576 13568 ?        Sl   02:33   0:00 /home/test/bin/containerd-shim-runc-v2 -namespace moby -id e1473cf4bc0906f5f6e76a4e09f7fc3883c2eb42aa5ff91976fd9036f8d04930 -address /run/user/1000/docker/containerd/containerd.sock
... snip ...
★containerd、runcが起動したまま残っている、fuse-overlayfsも起動している
```

#### Podman

コンテナを起動していない場合、Podmanに関するプロセスは実行していない（podmanサービスを起動していない場合）。  
コンテナを起動すると、コンテナランタイムが起動するが、すぐに終了する。  
conmonプロセスが残り、コンテナの監視を行う。

```console
test@fedora-podman:~$ podman run -d --name test nginx
50c1cc7acdd72d9b92ce3429586f22427d7f87587060c381215a309d36fe1200
★コンテナを実行

test@fedora-podman:~$ ps aux | grep podman
test        3255  0.0  0.0 225924  2568 ?        Ss   02:30   0:00 /usr/bin/conmon --api-version 1 ... snip ... --exit-command-arg container --exit-command-arg cleanup --exit-command-arg 50c1cc7acdd72d9b92ce3429586f22427d7f87587060c381215a309d36fe1200
... snip ...
★conmonが起動したまま残っている

test@fedora-podman:~$ ps aux | grep overlay
test        3244  0.3  0.0   5880  2180 ?        Ss   02:30   0:00 /usr/bin/fuse-overlayfs -o lowerdir=/home/test/.local/share/containers/storage/overlay/l/WSD5MRAWCTSVQFAFTVKRIFRKE5:/home/test/.local/share/containers/storage/overlay/l/5UNYV6NXGAAVBX5FIAV5RITCPD:/home/test/.local/share/containers/storage/overlay/l/VI2YLACTBIH2GJX7QDKZL4GGTM:/home/test/.local/share/containers/storage/overlay/l/WJMLYE5KMMHPVF3JINZ25K7WX3:/home/test/.local/share/containers/storage/overlay/l/WAHZWLVGHO5GSUAOIFTKUAUXCF:/home/test/.local/share/containers/storage/overlay/l/5QGITVN2ULDRQDW6D7IVC45Q7C:/home/test/.local/share/containers/storage/overlay/l/MHEYYI5VF5PHF42BYMONLNJCA7,upperdir=/home/test/.local/share/containers/storage/overlay/67623ad081cdd5e7c9c8164dd73db3906f304262764faf55a907a80697bb6051/diff,workdir=/home/test/.local/share/containers/storage/overlay/67623ad081cdd5e7c9c8164dd73db3906f304262764faf55a907a80697bb6051/work,,context="system_u:object_r:container_file_t:s0:c325,c374" /home/test/.local/share/containers/storage/overlay/67623ad081cdd5e7c9c8164dd73db3906f304262764faf55a907a80697bb6051/merged
★fuse-overlayfsも起動している
... snip ...

```

### マウントされるパス

`ubi9`イメージで作成したコンテナ上で、`mount`コマンドを実行した結果を比較する。  
全体的には、`SELinux`のラベルが設定される`Podman`の方が優れているように見える。

<details><summary>dockerの場合</summary><div>

```console
test@fedora-docker:~$ docker run --rm registry.access.redhat.com/ubi9 mount
Unable to find image 'registry.access.redhat.com/ubi9:latest' locally
latest: Pulling from ubi9
b824f4b30c46: Pull complete 
Digest: sha256:6b95efc134c2af3d45472c0a2f88e6085433df058cc210abb2bb061ac4d74359
Status: Downloaded newer image for registry.access.redhat.com/ubi9:latest
fuse-overlayfs on / type fuse.fuse-overlayfs (rw,nodev,noatime,user_id=0,group_id=0,default_permissions,allow_other)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev type tmpfs (rw,nosuid,seclabel,size=65536k,mode=755,uid=1000,gid=1000,inode64)
sysfs on /sys type sysfs (ro,nosuid,nodev,noexec,relatime,seclabel)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,seclabel,gid=524292,mode=620,ptmxmode=666)
cgroup on /sys/fs/cgroup type cgroup2 (ro,nosuid,nodev,noexec,relatime,seclabel,nsdelegate,memory_recursiveprot)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime,seclabel)
shm on /dev/shm type tmpfs (rw,nosuid,nodev,noexec,relatime,seclabel,size=65536k,uid=1000,gid=1000,inode64)
/dev/sda3 on /etc/resolv.conf type btrfs (rw,relatime,seclabel,compress=zstd:1,space_cache=v2,subvolid=256,subvol=/home)
/dev/sda3 on /etc/hostname type btrfs (rw,relatime,seclabel,compress=zstd:1,space_cache=v2,subvolid=256,subvol=/home)
/dev/sda3 on /etc/hosts type btrfs (rw,relatime,seclabel,compress=zstd:1,space_cache=v2,subvolid=256,subvol=/home)
devtmpfs on /dev/null type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/random type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/full type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/tty type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/zero type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/urandom type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
proc on /proc/bus type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/fs type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/irq type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sys type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sysrq-trigger type proc (ro,nosuid,nodev,noexec,relatime)
tmpfs on /proc/asound type tmpfs (ro,relatime,seclabel,uid=1000,gid=1000,inode64)
tmpfs on /proc/acpi type tmpfs (ro,relatime,seclabel,uid=1000,gid=1000,inode64)
devtmpfs on /proc/kcore type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /proc/keys type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /proc/latency_stats type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /proc/timer_list type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
tmpfs on /proc/scsi type tmpfs (ro,relatime,seclabel,uid=1000,gid=1000,inode64)
tmpfs on /sys/firmware type tmpfs (ro,relatime,seclabel,uid=1000,gid=1000,inode64)
tmpfs on /sys/devices/virtual/powercap type tmpfs (ro,relatime,seclabel,uid=1000,gid=1000,inode64)
```
</div></details>

<details><summary>podmanの場合</summary><div>

```console
test@fedora-podman:~$ podman run --rm registry.access.redhat.com/ubi9 mount
Trying to pull registry.access.redhat.com/ubi9:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob b824f4b30c46 done   | 
Copying config 2a2c2b7af8 done   | 
Writing manifest to image destination
Storing signatures
fuse-overlayfs on / type fuse.fuse-overlayfs (rw,nodev,noatime,user_id=0,group_id=0,default_permissions,allow_other)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev type tmpfs (rw,nosuid,context="system_u:object_r:container_file_t:s0:c349,c888",size=65536k,mode=755,uid=1000,gid=1000,inode64)
sysfs on /sys type sysfs (ro,nosuid,nodev,noexec,relatime,seclabel)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",gid=524292,mode=620,ptmxmode=666)
cgroup on /sys/fs/cgroup type cgroup2 (ro,nosuid,nodev,noexec,relatime,seclabel,nsdelegate,memory_recursiveprot)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime,seclabel)
shm on /dev/shm type tmpfs (rw,nosuid,nodev,noexec,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",size=64000k,uid=1000,gid=1000,inode64)
tmpfs on /etc/resolv.conf type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=811740k,nr_inodes=202935,mode=700,uid=1000,gid=1000,inode64)
tmpfs on /etc/hosts type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=811740k,nr_inodes=202935,mode=700,uid=1000,gid=1000,inode64)
tmpfs on /etc/hostname type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=811740k,nr_inodes=202935,mode=700,uid=1000,gid=1000,inode64)
devtmpfs on /dev/null type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/random type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/full type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/tty type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/zero type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /dev/urandom type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
proc on /proc/bus type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/fs type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/irq type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sys type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/sysrq-trigger type proc (ro,nosuid,nodev,noexec,relatime)
proc on /proc/asound type proc (ro,nosuid,nodev,noexec,relatime)
tmpfs on /proc/acpi type tmpfs (ro,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",uid=1000,gid=1000,inode64)
devtmpfs on /proc/kcore type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /proc/keys type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /proc/latency_stats type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
devtmpfs on /proc/timer_list type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=1009299,mode=755,inode64)
tmpfs on /proc/scsi type tmpfs (ro,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",uid=1000,gid=1000,inode64)
tmpfs on /sys/firmware type tmpfs (ro,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",uid=1000,gid=1000,inode64)
tmpfs on /sys/fs/selinux type tmpfs (ro,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",uid=1000,gid=1000,inode64)
tmpfs on /sys/dev/block type tmpfs (ro,relatime,context="system_u:object_r:container_file_t:s0:c349,c888",uid=1000,gid=1000,inode64)
tmpfs on /run/secrets type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=811740k,nr_inodes=202935,mode=700,uid=1000,gid=1000,inode64)
tmpfs on /run/.containerenv type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=811740k,nr_inodes=202935,mode=700,uid=1000,gid=1000,inode64)
```
</div></details>

#### dockerとpodmanの差異

`mount`の結果の差異についてまとめる。

- Podmanは、`/dev`などを筆頭にマウントオプション`context="system_u:object_r:container_file_t:s0:cxxx,cyyy"`がついている。
    - SELinuxをEnforcingにしている場合、Podman以外のプロセスのアクセスを遮断できるため、セキュリティの向上につながる。
- `/etc/{resolv.conf,hostname,hosts}`のマウントタイプが異なる。
    - Dockerの`libnetwork`とPodmanの`Netavark`の、名前解決方法が異なるのが原因？
- `/proc/asound`のマウントタイプが異なる。
    - Dockerは`tmpfs`、Podmanは`procfs`でマウントされており、セキュリティの面ではDockerの方が優れていると言える。
- Podmanに`/sys/devices/virtual/powercap`のtmpfsマウントがない。
    - 4.7.2でマスクされるようになった。参考：https://github.com/containers/podman/pull/20501
- Podmanでは以下のパスが`tmpfs`でマウントされている。
    - /sys/fs/selinux
    - /sys/dev/block
    - /run/secrets
    - /run/.containerenv


### Capability

### SELinux

### Seccomp
