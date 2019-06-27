---
title: "Rancher2 Install Base Aws Ubuntu"
date: 2018-06-06T17:08:28+08:00
draft: false
tags: ["rancher", "install", "aws", "ubuntu"]
categories: ["rancher"]
subtitle: "aws中ubuntu安装rancher2.0"
descriptions: ""
bigimg:
---


aws中ubuntu安装rancher2.0

## Env

#### Env-a

- cloud: aws
- os: ubuntu 16.04
- docker: 17.03.2-ce

#### Env-b

- cloud: aws
- os: Amazon Linux AMI 2018.03
- docker: 17.12.1-ce

这里的env-b可以不看, 因为效果与env-a相同

**Rancher versions:**

rancher/server or rancher/rancher: rancher/server 2.0.2

**Docker version: 17.03.2-ce

**Operating system and kernel: 

- ubuntu 16.04
- Linux ip-172-31-12-229 4.4.0-1060-aws #69-Ubuntu SMP Sun May 20 13:42:07 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux

**Type/provider of hosts: AWS

**Setup details: single node rancher

**Environment Template: Kubernetes

**Steps to Reproduce:**

```
sudo docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
docker logs e95 -f
```
**Results:**


#### env-a报错情况
```
root@ip-172-31-12-229:/home/ubuntu# docker logs e95 -f
2018/06/06 11:45:16 [INFO] Listening on /tmp/log.sock
2018/06/06 11:45:16 [INFO] [certificates] Generating CA kubernetes certificates
2018/06/06 11:45:17 [INFO] [certificates] Generating Kubernetes API server certificates
***
***
***
2018/06/06 11:45:32 [INFO] uploading digitaloceanConfig to node schema
2018/06/06 11:45:32 [INFO] uploading vmwarevsphereConfig to node schema
2018/06/06 11:45:32 [INFO] uploading vmwarevsphereConfig to node schema
E0606 11:45:38.982324       1 core.go:70] Failed to start service controller: WARNING: no cloud provider provided, services of type LoadBalancer will fail.
E0606 11:45:39.156222       1 certificates.go:48] Failed to start certificate controller: error reading CA cert file "/etc/kubernetes/ca/ca.pem": open /etc/kubernetes/ca/ca.pem: no such file or directory
2018/06/06 11:45:54 [INFO] traversing files and generating templates
2018/06/06 11:45:54 [INFO] Updating catalog library
2018/06/06 11:45:54 [INFO] Creating template library-prometheus
2018/06/06 11:45:55 [INFO] Creating template library-redis
2018/06/06 11:45:55 [INFO] Creating template library-longhorn
2018/06/06 11:45:55 [INFO] Creating template library-memcached
2018/06/06 11:45:55 [INFO] Creating template library-mongodb-replicaset
2018/06/06 11:45:55 [INFO] Creating template library-mysql
2018/06/06 11:45:55 [INFO] Creating template library-mariadb
2018/06/06 11:45:55 [INFO] Creating template library-mongodb
2018/06/06 11:45:55 [INFO] Creating template library-wordpress
2018/06/06 11:45:55 [INFO] Creating template library-nfs-provisioner
2018/06/06 11:45:55 [INFO] Creating template library-etcd-operator
2018/06/06 11:45:55 [INFO] Creating template library-grafana
2018/06/06 11:45:55 [INFO] Creating template library-magento
2018-06-06 11:55:19.631773 I | mvcc: store.index: compact 1198
2018-06-06 11:55:19.633173 I | mvcc: finished scheduled compaction at 1198 (took 920.507µs)
***
***
2018/06/06 23:45:31 [INFO] Running cluster events cleanup
2018/06/06 23:45:31 [INFO] Done running cluster events cleanup
2018-06-06 23:50:20.099819 I | mvcc: store.index: compact 65618
2018-06-06 23:50:20.100893 I | mvcc: finished scheduled compaction at 65618 (took 633.146µs)
2018-06-06 23:55:20.103352 I | mvcc: store.index: compact 66068
2018-06-06 23:55:20.104455 I | mvcc: finished scheduled compaction at 66068 (took 698.482µs)
2018-06-07 00:00:20.107535 I | mvcc: store.index: compact 66519
2018-06-07 00:00:20.108586 I | mvcc: finished scheduled compaction at 66519 (took 648.45µs)
2018-06-07 00:05:20.112088 I | mvcc: store.index: compact 66970
2018-06-07 00:05:20.113204 I | mvcc: finished scheduled compaction at 66970 (took 721.359µs)
2018-06-07 00:10:20.115164 I | mvcc: store.index: compact 67419
2018-06-07 00:10:20.116207 I | mvcc: finished scheduled compaction at 67419 (took 697.953µs)
2018-06-07 00:15:20.118671 I | mvcc: store.index: compact 67870
2018-06-07 00:15:20.119770 I | mvcc: finished scheduled compaction at 67870 (took 696.612µs)
2018-06-07 00:20:20.121399 I | mvcc: store.index: compact 68321
2018-06-07 00:20:20.122572 I | mvcc: finished scheduled compaction at 68321 (took 694.312µs)
2018-06-07 00:25:20.125420 I | mvcc: store.index: compact 68771
2018-06-07 00:25:20.127133 I | mvcc: finished scheduled compaction at 68771 (took 1.167633ms)
2018-06-07 00:30:20.129448 I | mvcc: store.index: compact 69220
2018-06-07 00:30:20.130575 I | mvcc: finished scheduled compaction at 69220 (took 676.794µs)
2018-06-07 00:35:20.132825 I | mvcc: store.index: compact 69671
2018-06-07 00:35:20.133879 I | mvcc: finished scheduled compaction at 69671 (took 642.16µs)
2018-06-07 00:40:20.136230 I | mvcc: store.index: compact 70122
2018-06-07 00:40:20.137384 I | mvcc: finished scheduled compaction at 70122 (took 740.249µs)
2018-06-07 00:45:20.139217 I | mvcc: store.index: compact 70573
2018-06-07 00:45:20.140354 I | mvcc: finished scheduled compaction at 70573 (took 732.96µs)
2018/06/07 00:45:31 [INFO] Running cluster events cleanup
2018/06/07 00:45:31 [INFO] Done running cluster events cleanup
```


#### env-b报错情况

```
[root@ip-172-31-32-161 ec2-user]# docker logs 626 -f
E0606 07:39:56.323409       1 garbagecollector.go:113] failed to sync all monitors: [couldn't look up resource {"management.cattle.io" "v3" "nodes"}: no matches for {management.cattle.io v3 nodes}, couldn't look up resource {"management.cattle.io" "v3" "globalroles"}: no matches for {management.cattle.io v3 globalroles}, couldn't look up resource {"management.cattle.io" "v3" "groupmembers"}: no matches for {management.cattle.io v3 groupmembers}, couldn't look up resource {"management.cattle.io" "v3" "listenconfigs"}: no matches for {management.cattle.io v3 listenconfigs}, couldn't look up resource {"management.cattle.io" "v3" "groups"}: no matches for {management.cattle.io v3 groups}]
E0606 07:40:01.279595       1 streamwatcher.go:109] Unable to decode an event from the watch stream: json: cannot unmarshal string into Go struct field dynamicEvent.Object of type v3.ProjectStatus
E0606 07:40:01.280050       1 streamwatcher.go:109] Unable to decode an event from the watch stream: json: cannot unmarshal string into Go struct field dynamicEvent.Object of type v3.NodeDriverStatus
E0606 07:40:01.281077       1 streamwatcher.go:109] Unable to decode an event from the watch stream: json: cannot unmarshal string into Go struct field dynamicEvent.Object of type v3.DynamicSchemaStatus
E0606 07:40:01.283896       1 streamwatcher.go:109] Unable to decode an event from the watch stream: json: cannot unmarshal string into Go struct field dynamicEvent.Object of type v3.SourceCodeCredentialStatus
E0606 07:48:06.259413       1 streamwatcher.go:109] Unable to decode an event from the watch stream: json: cannot unmarshal string into Go struct field dynamicEvent.Object of type v3.ClusterPipelineStatus
2018-06-06 07:49:49.076333 I | mvcc: store.index: compact 1198
2018-06-06 07:49:49.077697 I | mvcc: finished scheduled compaction at 1198 (took 892.715µs)
2018-06-06 07:54:49.079381 I | mvcc: store.index: compact 1650
2018-06-06 07:54:49.080456 I | mvcc: finished scheduled compaction at 1650 (took 622.642µs)
2018-06-06 07:57:51.244337 N | pkg/osutil: received terminated signal, shutting down...
2018/06/06 07:57:51 [INFO] Shutting down ProjectRoleTemplateBindingController controller
2018/06/06 07:57:51 [INFO] Shutting down ClusterRoleTemplateBindingController controller
2018/06/06 07:57:51 [INFO] Shutting down UserController controller
2018/06/06 07:57:51 [INFO] Shutting down NodeController controller
2018/06/06 07:57:51 [INFO] Shutting down NodeController controller
2018/06/06 07:57:51 [INFO] Shutting down ClusterController controller
2018/06/06 07:57:51 [INFO] Shutting down SecretController controller
2018/06/06 07:57:51 [INFO] Shutting down ClusterRegistrationTokenController controller
2018/06/06 07:57:51 [FATAL] context canceled
2018/06/06 07:57:51 [ERROR] server on [::]:443 returned err: accept tcp [::]:443: use of closed network connection
tail: cannot open ‘25’ for reading: No such file or directory
```

## Step

这里的输出，目前好像是不影响rancher server的使用。

但是，请在 aws中打开 443端口（只打开80是不够的，最后还是会转向443）。


当时，我就是没有把443端口打开，导致，一直怀疑是下面几个方面：

- 配置了vpc
- docker版本过高

事后看，与这些怀疑点无关。


## Ref

- YouTube的[Creating a Rancher 2.0-alpha Server in AWS EC2](https://www.youtube.com/watch?v=w3WusK76cEk)
- https://rancher.com/docs/rancher/v2.x/en/quick-start-guide/