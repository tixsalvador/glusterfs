# Provision vagrant glusterfs

```sh
$  vagrant up
```

Install glusterfs

```sh
$  yum install -y centos-release-gluster
$  yum install -y glusterfs gluster-cli glusterfs-libs glusterfs-server
```

Add peers run only on 1 node

```sh
$  gluster peer probe 10.10.10.202
$  gluster peer status
```

Reference:
[https://archive.fosdem.org/2020/schedule/event/sds_gluster_thin_arbiter/attachments/slides/4110/export/events/attachments/sds_gluster_thin_arbiter/slides/4110/gluster_thin_arbiter_fosdem_2020.pdf](https://archive.fosdem.org/2020/schedule/event/sds_gluster_thin_arbiter/attachments/slides/4110/export/events/attachments/sds_gluster_thin_arbiter/slides/4110/gluster_thin_arbiter_fosdem_2020.pdf)
