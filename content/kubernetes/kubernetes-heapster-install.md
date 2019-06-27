---
title: "Kubernetes Heapster Install"
date: 2018-03-28T10:46:26+08:00
draft: false
tags: ["kubernetes", "heapster"]
categories: ["kubernetes"]
subtitle: ""
descriptions: ""
bigimg:
---

## env

192.168.31.120 master

## step


```
root@km:~/heapster# git remote -v
origin	https://github.com/kubernetes/heapster.git (fetch)
origin	https://github.com/kubernetes/heapster.git (push)
root@km:~/heapster# git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   deploy/kube-config/influxdb/grafana.yaml
	modified:   deploy/kube-config/influxdb/heapster.yaml
	modified:   deploy/kube-config/influxdb/influxdb.yaml

no changes added to commit (use "git add" and/or "git commit -a")
root@km:~/heapster# git diff deploy/kube-config/influxdb/grafana.yaml
diff --git a/deploy/kube-config/influxdb/grafana.yaml b/deploy/kube-config/influxdb/grafana.yaml
index de98b65..d06c665 100644
--- a/deploy/kube-config/influxdb/grafana.yaml
+++ b/deploy/kube-config/influxdb/grafana.yaml
@@ -65,8 +65,10 @@ spec:
   # type: LoadBalancer
   # You could also use NodePort to expose the service at a randomly-generated port
   # type: NodePort
+  type: NodePort
   ports:
   - port: 80
     targetPort: 3000
+    nodePort: 30002
   selector:
     k8s-app: grafana
root@km:~/heapster# git diff deploy/kube-config/influxdb/heapster.yaml 
diff --git a/deploy/kube-config/influxdb/heapster.yaml b/deploy/kube-config/influxdb/heapster.yaml
index 9d45dea..0e92361 100644
--- a/deploy/kube-config/influxdb/heapster.yaml
+++ b/deploy/kube-config/influxdb/heapster.yaml
@@ -25,7 +25,7 @@ spec:
         command:
         - /heapster
         - --source=kubernetes:https://kubernetes.default
-        - --sink=influxdb:http://monitoring-influxdb.kube-system.svc:8086
+        - --sink=influxdb:http://monitoring-influxdb.kube-system.svc.cluster.local:8086
 ---
 apiVersion: v1
 kind: Service
root@km:~/heapster# git diff deploy/kube-config/influxdb/influxdb.yaml 
diff --git a/deploy/kube-config/influxdb/influxdb.yaml b/deploy/kube-config/influxdb/influxdb.yaml
index 03e717b..87062f7 100644
--- a/deploy/kube-config/influxdb/influxdb.yaml
+++ b/deploy/kube-config/influxdb/influxdb.yaml
@@ -33,8 +33,10 @@ metadata:
   name: monitoring-influxdb
   namespace: kube-system
 spec:
+  type: NodePort
   ports:
   - port: 8086
     targetPort: 8086
+    nodePort: 30003
   selector:
     k8s-app: influxdb
root@km:~/heapster# 
```

