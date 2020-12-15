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

Download heketi server and client

```sh
$  curl -s https://api.github.com/repos/heketi/heketi/releases/latest \
  | grep browser_download_url \
  | grep linux.amd64 \
  | cut -d '"' -f 4 \
  | wget -qi -
```

Untar file and copy files

```sh
$  sudo cp heketi/{heketi,heketi-cli} /usr/local/bin/
$
```

Copy config file

```sh
$  sudo mkdir /var/lib/heketi /etc/heketi /var/log/heketi
$  sudo cp heketi/heketi.json /etc/heketi/
```

Add heketi user and group

```sh
$  sudo groupadd --system heketi
$  sudo useradd -s /sbin/nologin --system -g heketi heketi
```

Create ssh keys for passwordless access to gluster nodes

```sh
{
    ssh-keygen -f /etc/heketi/heketi_key -t rsa =N ''
    for x in {1..2}; do
      ssh-copy-id -i /etc/heketi/heketi_key.pub root@10.10.10.20$x
    done
}
```

Reference:
[https://archive.fosdem.org/2020/schedule/event/sds_gluster_thin_arbiter/attachments/slides/4110/export/events/attachments/sds_gluster_thin_arbiter/slides/4110/gluster_thin_arbiter_fosdem_2020.pdf](https://archive.fosdem.org/2020/schedule/event/sds_gluster_thin_arbiter/attachments/slides/4110/export/events/attachments/sds_gluster_thin_arbiter/slides/4110/gluster_thin_arbiter_fosdem_2020.pdf)
[Setup GlusterFS Storage With Heketi on CentOS 8 / CentOS 7](https://computingforgeeks.com/setup-glusterfs-storage-with-heketi-on-centos-server/)
