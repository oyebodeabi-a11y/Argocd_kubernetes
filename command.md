# make sure you have a kubernetes tool ready to work with 
# for this project i choose minkube a single node cluster but for minkube to work there must be docker deskstop running on ur system


# or if the above does not installed

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version
====================
# create a prpject folder 
 argocd-deployment-kubenrnetes
 
# start your minkube by running the command below
minikube start

# the command above get ur single node cluster ready coipled with the kubernetes cli interface call kubectl

# now run the command below

minikube status

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

# to install argocd on kubernetes cluster to monitor and control your kuberntes worklow/workload


kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


# to check if argocd pods are running on the argo cd namespace
kubectl get pods -n argocd

argocd-application-controller-0                    1/1     Running   4 (4m17s ago)    9d
argocd-applicationset-controller-6b6c76c96-754c2   1/1     Running   4 (4m18s ago)    9d
argocd-dex-server-8bc648f66-rck5k                  1/1     Running   4 (4m19s ago)    9d
argocd-notifications-controller-7fb7c99cdb-tgjsk   1/1     Running   19 (4m19s ago)   9d
argocd-redis-65c8584965-j4bwn                      1/1     Running   4 (4m19s ago)    9d
argocd-repo-server-554fd64cbb-lz7bp                1/1     Running   15 (4m19s ago)   9d
argocd-server-6dcbfcb5d5-twlr5 

# to check for the argo cd service

k get services -n argocd

NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.97.69.80      <none>        7000/TCP,8080/TCP            9d
argocd-dex-server                         ClusterIP   10.97.166.79     <none>        5556/TCP,5557/TCP,5558/TCP   9d
argocd-metrics                            ClusterIP   10.101.25.241    <none>        8082/TCP                     9d
argocd-notifications-controller-metrics   ClusterIP   10.96.156.23     <none>        9001/TCP                     9d
argocd-redis                              ClusterIP   10.108.150.55    <none>        6379/TCP                     9d
argocd-repo-server                        ClusterIP   10.110.210.139   <none>        8081/TCP,8084/TCP            9d
argocd-server                             ClusterIP   10.99.238.98     <none>        80/TCP,443/TCP               9d
argocd-server-metrics                     ClusterIP   10.102.196.231   <none>        8083/TCP 

# now select the service argocd-server and port forward on it in the namespace to get the argocd gui as shown below
# run it on your terminal

kubectl port-forward service/argocd-server -n argocd 8080:443
Forwarding from 127.0.0.1:8080 -> 8080


# use below to get the argocd Gui locally on the browser
127.0.0.1:8080

# click  advanced button and click the Proceed to 127.0.0.1 (unsafe) and view the GUI



# To get Credentials to login
# The username is admin but use the below to get the passowrd

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

In argocd cli

<img width="932" height="431" alt="Image" src="https://github.com/user-attachments/assets/a8290e42-1b40-4cba-a2d3-e2a934591ccd" />


## or another method

kubectl -n argocd get secret argocd-initial-admin-secret -o yaml

apiVersion: v1
data:
  password: SklsN0llczB4SUJXYlVKag==
kind: Secret
metadata:
  creationTimestamp: "2024-10-17T10:06:09Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "1330639"
  uid: 0fa80383-d1f3-4048-973c-9cf833a6af38
type: Opaque


# Now decode the password by running this command below

echo SklsN0llczB4SUJXYlVKag== | base64 -d 

JIl7Ies0xIBWbUJj%