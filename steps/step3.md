## Les Services

Positive
: Voici la documentation utile pour cette étape

- [Blabla](blabla)

kubectl get pods

kubectl get services

kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080

 kubectl get services

 kubectl describe services/kubernetes-bootcamp

(voir le node port (localhost est l’adresse par defaut))

curl localhost:port


NAMESPACE :

kubectl describe deployment

(Voir le selector)

kubectl get pods -l run=kubernetes-bootcamp

kubectl label pod kubernetes-bootcamp-5c69669756-x7jrp app=v1

kubectl describe pods kubernetes-bootcamp-5c69669756-x7jrp

(on peux voir les labels et co : lo, container ID etc)

kubectl get pods -l app=v1

Description des labels (avec la bonne slide) : Front/Bac/ différences des env (prod/pprod/dev)

DELETE SERVICE :

kubectl delete service -l run=kubernetes-bootcamp

kubectl get services

pour vérifier on fait le curl :

curl localhost:Node port (ca ne marche pas)

SERVICE : SCALE 

kubectl get deployments 

The DESIRED state is showing the configured number of replicas
The CURRENT state show how many replicas are running now
The UP-TO-DATE is the number of replicas that were updated to match the desired (configured) state
The AVAILABLE state shows how many replicas are actually AVAILABLE to the users


kubectl scale deployments/kubernetes-bootcamp --replicas=4

kubectl get deployments

kubectl get pods -o wide

(describe service)

kubectl describe services/kubernetes-bootcamp

On peux faire le curl avec le node port il y a du load balancing (pas le meme container)

kubectl scale deployments/kubernetes-bootcamp --replicas=2

kubectl get deployments

kubectl get pods -o wide




UPDATE (rolling update)

kubectl get pods

kubectl describe pods

kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2

kubectl get pods


On refaot un curl 

confirmer :

kubectl rollout status deployments/kubernetes-bootcamp

kubectl describe pods


kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10

kubectl get deployments

Ca ne amrche pas : ROLLBACK

kubectl rollout undo deployments/kubernetes-bootcamp

kubectl describe pods

