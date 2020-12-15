# Provision vagrant glusterfs

```sh
$  vagrant up
```

### Option 1 using glusterfs

Install glusterfs

```sh
$  yum install -y centos-release-gluster
$  yum install -y glusterfs gluster-cli glusterfs-libs glusterfs-server
```

Add peers run only on 1 node

```sh
$  gluster peer probe 10.10.10.202
$  gluster peer probe 10.10.10.203
$  gluster peer status
$  gluster pool list
```

Create gluster volume

```sh
$  gluster volume create mysqldata replica 3 transport tcp 10.10.10.201:/mnt/glusterfs/mysqldata/ 10.10.10.202:/mnt/glusterfs/mysqldata 10.10.10.203:/mnt/glusterfs/mysqldata/
$   gluster volume start mysqldata
```

Check volume

```sh
$  gluster volume info all
```

### Option 2 Using Heketi

Setup passwordless ssh login using ssh keys from master to gluster nodes

Reference:
[https://archive.fosdem.org/2020/schedule/event/sds_gluster_thin_arbiter/attachments/slides/4110/export/events/attachments/sds_gluster_thin_arbiter/slides/4110/gluster_thin_arbiter_fosdem_2020.pdf](https://archive.fosdem.org/2020/schedule/event/sds_gluster_thin_arbiter/attachments/slides/4110/export/events/attachments/sds_gluster_thin_arbiter/slides/4110/gluster_thin_arbiter_fosdem_2020.pdf)
