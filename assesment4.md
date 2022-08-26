# Results
1. Deleted voting app - `kubectl delete pod vote-94849dc97-hq5p6`
    * since this is created in deployment, and deployment uses ReplicaSet a new pod created with different hash `vote-94849dc97-n7fh4` old one is `pod/vote-94849dc97-hq5p6`
2. Delete worker app - `kubectl delete pod worker-dd46d7584-98d9v`
    * nothing changed except pod hash
    * old hash `worker-dd46d7584-98d9v` new hash `worker-dd46d7584-z24gr`
3. Delete DB - `kubectl delete pod db-b54cd94f4-r8pkn`
    * voting is working but results are not getting updated
    * worker got restarted `worker-dd46d7584-z24gr    1/1     Running   2          8m29s`. It is showing 2 because we deleted twice
    * after `kubectl delete pod/result-5d57b59f4b-n8hxc` results are started to show up
    * results got reset to 50:50 and 0 votes because its a new database
## Reasons for results page not working
* connection between db and results pod stopped, because of that results are not getting updated
```
[root@ip-172-31-10-26 k8s-specifications]# kubectl logs pod/result-5d57b59f4b-vnfzn
Fri, 26 Aug 2022 13:20:15 GMT body-parser deprecated bodyParser: use individual json/urlencoded middlewares at server.js:67:9
Fri, 26 Aug 2022 13:20:15 GMT body-parser deprecated undefined extended: provide extended option at ../node_modules/body-parser/index.js:105:29
App running on port 80
Connected to db
Error performing query: Error: This socket has been ended by the other party
```

* after deleting the results pod it restarted it and connection was eshtablished again

# Things learned
* docker creating containers and images
* disadvantages of docker and rise of micro services and kubernetes
* API server and worker nodes, scheduler, etcd, pod, namespaces
* Controllers
    * Replication Manager
    * replicationSet
    * DaemonSet
    * BatchJob and Cronjob
* Service
    * ClusterIp
    * NodePort
    * LoadBalancer
* Deployment

