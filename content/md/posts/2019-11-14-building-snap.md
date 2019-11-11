{:title "Building SNAP"
:layout :post
:tags ["practice" "snap"]
}

In my spare I develop open source tools for various topics. Mainly the tools which support the project I am working on. Depends on the type of the project there are different distribution strategies.

One of the distribution option is to build and publish the snap.

Documentation is relatively good, but not complete, and not always up to date, so below is the up to date (at the moment of writing) snap declaration and building.

### snapcraft.yaml

Here is the functional [snapcraft.yaml](https://github.com/zshamrock/dynocsv/blob/master/snap/snapcraft.yaml) for building local Go application. 

```
name: dynocsv # you probably want to 'snapcraft register <name>'
version: '1.0.0' # just for humans, typically '1.2+git' or '1.3.2'
summary: Exports DynamoDB table into CSV # 79 char long summary
description: |
  Exports DynamoDB table into CSV, additionally can filter out specific columns and limit the number of items to be exported.

  The tool relies on reading AWS profile config credentials (`$HOME/.aws`), so when installed you would need explicitly allow this access by running:

  `snap connect dynocsv:aws-config-credentials`

confinement: strict # use 'strict' once you have the right plugs and slots
base: core18
#grade: stable # must be 'stable' to release into candidate/stable channels
grade: stable
license: MIT

parts:
  dynocsv:
    plugin: go
    source-type: local
    source: .
    # alternatively it is also possible to build using "git" source
    #    source-type: git
    #    source: https://github.com/zshamrock/dynocsv
    go-importpath: github.com/zshamrock/dynocsv
    build-packages:
      - gcc
      - git

plugs:
  aws-config-credentials:
    interface: personal-files
    read:
      - $HOME/.aws/config
      - $HOME/.aws/credentials

apps:
  dynocsv:
    command: dynocsv
    plugs:
      - network
      - removable-media
      - home
      - aws-config-credentials

```

### On Debian

```
$ snap debug confinement
partial

$ snap debug sandbox-features 
apparmor:             kernel:caps kernel:domain kernel:file kernel:mount kernel:namespaces kernel:network_v8 kernel:policy kernel:ptrace kernel:query kernel:rlimit kernel:signal parser:unsafe policy:downgraded support-level:partial
confinement-options:  classic devmode
dbus:                 mediated-bus-access
kmod:                 mediated-modprobe
mount:                freezer-cgroup-v1 layouts mount-namespace per-snap-persistency per-snap-profiles per-snap-updates per-snap-user-profiles stale-base-invalidation
seccomp:              bpf-actlog bpf-argument-filtering kernel:allow kernel:errno kernel:kill_process kernel:kill_thread kernel:log kernel:trace kernel:trap
udev:                 device-cgroup-v1 tagging

```

From the above output, even the `strict` confinement is declared in the `snapcraft.yaml` Debian 10 (at the moment of writing) only supports `partial` implementation of the confinement (`support-level: partial`), and only 2 `confinement-options: classic devmode`.

So when you install the snap locally `snap install --dangerous dynocsv_1.0.0_amd64.snap`, and run it, it will work fine without any issues, which give the false sense of correctness of your `snapcraft.yaml`. So you always need to test on the system which has full confinement support.

### On Ubuntu

Below commands are running on AWS EC2 Ubuntu 18.04.2 LTS instance (ami-09ca8a080b6699260).

```
$ snap debug confinement 
strict

$ snap debug sandbox-features 
apparmor:             kernel:caps kernel:dbus kernel:domain kernel:file kernel:mount kernel:namespaces kernel:network kernel:policy kernel:ptrace kernel:query kernel:rlimit kernel:signal parser:unsafe policy:default support-level:full
confinement-options:  classic devmode strict
dbus:                 mediated-bus-access
kmod:                 mediated-modprobe
mount:                freezer-cgroup-v1 layouts mount-namespace per-snap-persistency per-snap-profiles per-snap-updates per-snap-user-profiles stale-base-invalidation
seccomp:              bpf-actlog bpf-argument-filtering kernel:allow kernel:errno kernel:kill_process kernel:kill_thread kernel:log kernel:trace kernel:trap
udev:                 device-cgroup-v1 tagging
```

And here the full confinement support is enabled (`support-level: full`), and all 3 `confinement-options: classic devmode strict` are enabled. So if install the above snap on this instance, it will fail, as expected. 

### Manually connect the plug


```
$ snap connections
Interface  Plug             Slot      Notes
home       dynocsv:home     :home     -
network    dynocsv:network  :network  -
```

```
$ snap connections dynocsv
Interface        Plug                            Slot      Notes
home             dynocsv:home                    :home     -
network          dynocsv:network                 :network  -
personal-files   dynocsv:aws-config-credentials  -         -
removable-media  dynocsv:removable-media         -         -
```

`home` and `network` connections (other names are plugs, interfaces) are connected automatically by default. Which is not the case for the `personal-files` connection, which has the corresponding alias/plug: `aws-config-credentials` as per [personal-files interface documentation](https://snapcraft.io/docs/personal-files-interface), and has to be connected manually:

```
$ sudo snap connect dynocsv:aws-config-credentials
```

And now:

```
$ snap connections dynocsv
Interface        Plug                            Slot             Notes
home             dynocsv:home                    :home            -
network          dynocsv:network                 :network         -
personal-files   dynocsv:aws-config-credentials  :personal-files  manual
removable-media  dynocsv:removable-media         -                -
```

See "Notes: manual" for that `personal-files` interface.

Now if you run the app everything will work as expected.
