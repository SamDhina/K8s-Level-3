# K8s-Level-3
Kubernetes Level 3 Test

-------------------------------------------------------------------------------------------------------------------

1 .Create a Kubernetes cluster on GCP (GCP gives free credits on signup so those should suffice for this exercise) on their virtual machines (do not use GKE) or use the cluster created in the level2 test. If possible share a script / code which can be used to create the cluster.
samdhina_x11@cloudshell:~ (smooth-loop-245005)$ kubectl get nodes
NAME                           STATUS                     ROLES    AGE   VERSION
kubernetes-master              Ready,SchedulingDisabled   <none>   18d   v1.15.0
kubernetes-minion-group-8t0k   Ready                      <none>   18d   v1.15.0
kubernetes-minion-group-w65t   Ready                      <none>   18d   v1.15.0
samdhina_x11@cloudshell:~ (smooth-loop-245005)$
-----------------------------------------------------------------------------------------------------------------------
2.	Setup CI Server (Jenkins or tool of your choice) inside the Kubernetes cluster and maintain the high availability of the jobs created.
  pods:
  samdhina_x11@cloudshell:~ (smooth-loop-245005)$ kubectl get pods -n jenkins
NAME                            READY   STATUS    RESTARTS   AGE
jenkins-prod-6dcd76f679-ptqbj   1/1     Running   0          22h
samdhina_x11@cloudshell:~ (smooth-loop-245005)$
SVC:
samdhina_x11@cloudshell:~ (smooth-loop-245005)$ kubectl get svc -n jenkins
NAME                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
jenkins-prod         LoadBalancer   10.0.245.175   <pending>     8080:30825/TCP   23h
jenkins-prod-agent   ClusterIP      10.0.52.89     <none>        50000/TCP        23h
  
  After Scaled :
  
  samdhina_x11@cloudshell:~/charts/stable/jenkins (smooth-loop-245005)$ kubectl get pods -n jenkins
NAME                            READY   STATUS    RESTARTS   AGE
jenkins-prod-6dcd76f679-478nd   1/1     Running   0          120m
jenkins-prod-6dcd76f679-ptqbj   1/1     Running   0          27h
samdhina_x11@cloudshell:~/charts/stable/jenkins (smooth-loop-245005)$
  
  
------------------------------------------------------------------------------------------------------------------------
3.	Create a namespace and deploy the mediawiki application (or any other application which you think is more suitable to showcase your ability, kindly justify why you have chosen a different application) on the cluster. 

samdhina_x11@cloudshell:~ (smooth-loop-245005)$ helm install --name my-release stable/mediawiki --namespace=app
NAME:   my-release
LAST DEPLOYED: Mon Jul 22 09:11:46 2019
NAMESPACE: app
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                      DATA  AGE
my-release-mariadb        1     1s
my-release-mariadb-tests  1     1s

==> v1/PersistentVolumeClaim
NAME                            STATUS   VOLUME    CAPACITY  ACCESS MODES  STORAGECLASS  AGE
my-release-mediawiki-mediawiki  Pending  standard  1s

==> v1/Pod(related)
NAME                                  READY  STATUS   RESTARTS  AGE
my-release-mariadb-0                  0/1    Pending  0         0s
my-release-mediawiki-8d786d8f4-xgqsc  0/1    Pending  0         1s

==> v1/Secret
NAME                  TYPE    DATA  AGE
my-release-mariadb    Opaque  2     1s
my-release-mediawiki  Opaque  1     1s

==> v1/Service
NAME                  TYPE          CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
my-release-mariadb    ClusterIP     10.0.229.41  <none>       3306/TCP                    1s
my-release-mediawiki  LoadBalancer  10.0.193.7   <pending>    80:30059/TCP,443:30261/TCP  1s

==> v1beta1/Deployment
NAME                  READY  UP-TO-DATE  AVAILABLE  AGE
my-release-mediawiki  0/1    1           0          1s

==> v1beta1/StatefulSet
NAME                READY  AGE
my-release-mariadb  0/1    1s

samdhina_x11@cloudshell:~ (smooth-loop-245005)$ kubectl get pods -n app
NAME                                   READY   STATUS    RESTARTS   AGE
my-release-mariadb-0                   1/1     Running   0          171m
my-release-mediawiki-8d786d8f4-xgqsc   1/1     Running   0          171m
samdhina_x11@cloudshell:~ (smooth-loop-245005)$ kubectl get svc -n app
NAME                   TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
my-release-mariadb     ClusterIP      10.0.229.41   <none>        3306/TCP                     171m
my-release-mediawiki   LoadBalancer   10.0.193.7    <pending>     80:30059/TCP,443:30261/TCP   171m
samdhina_x11@cloudshell:~ (smooth-loop-245005)$
--------------------------------------------------------------------------------------------------------------------------------------



