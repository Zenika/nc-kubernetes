## Les PODS

Positive
: Voici la documentation utile pour cette étape

- [Blabla](blabla)

$ kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080
>création d’un pod avec l’image kubernetes bootcamp (a changer)

$ kubectl deployments 
> expliquer le deployments 

BROUILLON : 

Commandes kubernetes : 

I) WebUI

Activer kubernetes sur Windows/Mac : https://github.com/kubernetes/dashboard/wiki/Access-control#kubeconfig

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/


kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
kubectl proxy
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

(https://github.com/kubernetes/dashboard/wiki/Access-control#kubeconfig)

Check existing secrets in kube-system namespace
$ kubectl -n kube-system get secret

Let's get token from 'replicaset-controller-token-kzpmc'. 
# It should have permissions to see Replica Sets in the cluster.
$ kubectl -n kube-system describe secret replicaset-controller-token-kzpmc



II ) Deploying an APP :

kubectl get nodes 

kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080

kubectl get deployments

http://localhost:8001/api/v1/namespaces/default/pods/kubernetes-bootcamp-5c69669756-x7jrp/proxy/

kubectl logs kubernetes-bootcamp-5c69669756-x7jrp ⇒ voir les logs

Executer des commandes : 

kubectl exec kubernetes-bootcamp-5c69669756-x7jrp env

kubectl exec kubernetes-bootcamp-5c69669756-x7jrp cat server.js

