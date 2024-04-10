# Main cluster

Cluster of 3 nodes and control server.

```
192.168.0.50 control
192.168.0.10 node0
192.168.0.11 node1
192.168.0.12 node2
```

## Dependencies

To build an iso for cloud-init you will need to install **genisoimage** or **mkisofs**

> **mkisofs** is a drop in replacement for **genisoimage** 
```console
brew install cdrtools
```

## Vagrant

**Debian 12** with **cloud-init** and **Guest Additions**

[nulluuid/debian12](https://app.vagrantup.com/nulluuid/boxes/debian12)

```console
# Deploy entire cluster
vagrant up

# Deploy only control server and one node
vagrant up control node0

# Show status
vagrant status

# SSH into control
vagrant ssh control

# Destroy cluster
vagrant destroy
```

## Cloud-init

```console
cloud-init query --list-keys
cloud-init query --all
```

## Saltstack keys

```console
# Show all keys
salt-key --print-all

# Accept keys from node0
salt-key -a node0 --yes

# Delete keys from node0
salt-key -d node0 --yes
```
