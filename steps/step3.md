## Les Services

Positive
: Voici la documentation utile pour cette étape

- [Service](https://kubernetes.io/docs/concepts/services-networking/service/)


Verifions que nous avons nos `PODS` de disponible

```shell
kubectl get pods
```

Afin de voir nos différents services et avoir les informations de base, il suffit de faire la commande suivante : 

```shell
kubectl get services
```
Nous pouvons voir quelques informations importantes : 
* l'ID du service qui sert à receuillir les différentes informations de celui-ci // A COMPLETER
* Le type du service 
  * `Cluster IP` : On ne peut acceder au service que dans le cluster
  * `NodePort` : 
  * `LoadBalancer` :
* Le cluster IP : IP local
* External IP : IP externe
* PORT : les différents ports exposés
* AGE : L'uptime de votre service

Afin de créer un service avec notre application de type `NodePort` sur le port 8080, il suffit de faire la commande suivante : 

```shell
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

Vous pouvez observer que notre service a bien été créer avec la commande suivante :

```shell
 kubectl get services
```

Afin d'avoir les informations de votre service, vous devez utiliser la commande `describe`.

```shell
λ  kubectl describe services/kubernetes-bootcamp
Name:                     kubernetes-bootcamp
Namespace:                default
Labels:                   run=kubernetes-bootcamp
Annotations:              <none>
Selector:                 run=kubernetes-bootcamp
Type:                     NodePort
IP:                       10.105.234.192
LoadBalancer Ingress:     localhost
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  32181/TCP
Endpoints:                10.1.0.5:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

```

Pour vérifier que le service à bien été lancé, nous pouvons appeler notre service avec un GET sur le `NodePort` : ici 31281.

```shell
curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-5c69669756-x7jrp | v=1
```

Nous savons maintenant créer un service de type NodePort sur notre applicaton Bootcamp.

Vous trouverez un descriptif des différents types de service kubernetes ici : https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0


## LABEL :

Les `labels` sont une partie importante de Kubernetes. Ils permettent d'organiser nos PODS suivant notre politique de déploiement.

Une bonne pratique est de nommer les différents environnements dans des labels, ou encore le type d'application (back,front, etc ...)

Afin de voir ces differents informations, nous pouvons "décrire" nos deployments : 

```shell
kubectl describe deployment
Name:                   kubernetes-bootcamp
Namespace:              default
CreationTimestamp:      Mon, 24 Dec 2018 10:18:42 +0100
Labels:                 run=kubernetes-bootcamp
Annotations:            deployment.kubernetes.io/revision=1
Selector:               run=kubernetes-bootcamp
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  1 max unavailable, 1 max surge
Pod Template:
  Labels:  run=kubernetes-bootcamp
  Containers:
   kubernetes-bootcamp:
    Image:        gcr.io/google-samples/kubernetes-bootcamp:v1
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   kubernetes-bootcamp-5c69669756 (1/1 replicas created)
Events:          <none>
```

 Ici le pod est dans le namespace "default", et il dispose d'un `label` "run=kubernetes-bootcamp"

Pour selectionner les pods d'un label, nous pouvons executer la commande suivante : 

```shell
kubectl get pods -l run=kubernetes-bootcamp
NAME                                   READY     STATUS    RESTARTS   AGE
kubernetes-bootcamp-5c69669756-x7jrp   1/1       Running   0          4d
```

Nous pouvons rajouter différents `label`, par exemple la version de l'application, le jour de la mise en prod, etc ... Votre imagination est la seule limite : 

```shell
kubectl label pod kubernetes-bootcamp-5c69669756-x7jrp env=pprod
```

Nous pouvons voir le label en décrivant le pod : 

```shell
 kubectl describe pods kubernetes-bootcamp-5c69669756-x7jrp
Name:           kubernetes-bootcamp-5c69669756-x7jrp
Namespace:      default
Node:           docker-for-desktop/192.168.65.3
Start Time:     Mon, 24 Dec 2018 10:18:42 +0100
Labels:         app=v1
                env=pprod
                pod-template-hash=1725225312
                run=kubernetes-bootcamp
Annotations:    <none>
Status:         Running
IP:             10.1.0.5
Controlled By:  ReplicaSet/kubernetes-bootcamp-5c69669756
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://b71c4560997a4d477ded57a6da751ce3188b662835e9c333b54cc169e694df98
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://gcr.io/google-samples/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 24 Dec 2018 10:19:43 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-q6z9r (ro)
Conditions:
  Type           Status
  Initialized    True
  Ready          True
  PodScheduled   True
Volumes:
  default-token-q6z9r:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-q6z9r
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>
```

Pour lister les pods d'un label : 

```shell
kubectl get pods -l env=pprod
```

## SCALE :

Le scalling d'application est une notion importante de Kubernetes. Il permet d'avoir plusieurs `réplicats` de notre POD Kubernetes suivant la charge ou la politique de déploiement d'une application. 

Nous pouvons voir les `réplicats` avec la commande suivante : 

```shell
 kubectl get deployments
NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp     1         1         1            1           4d
```

Pour information :

* DESIRED : Montre le nombre de replicats configuré 
* CURRENT : Montre le nombre de replicats disponible à l'execution de la commande
* UP-TO-DATE : Montre les réplicats à jours
* AVAILABLE : Montre les réplicats disponible pour les utilisateurs

Afin d'augmenter le nombre de réplicats, rien de plus simple avec la commande `scale` : 

```shell
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

Nous pouvons voir la bonne execution de celle-ci : 

```shell
kubectl get deployments
NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp     4         4         4            4           4d
```

Ainsi que la liste de nos PODS : 

```shell
kubectl get pods -o wide
NAME                                     READY     STATUS    RESTARTS   AGE       IP         NODE
kubernetes-bootcamp-1-684564d696-mqlvd   1/1       Running   0          4d        10.1.0.6   docker-for-desktop
kubernetes-bootcamp-5c69669756-ldkqf     1/1       Running   0          49s       10.1.0.8   docker-for-desktop
kubernetes-bootcamp-5c69669756-lrptf     1/1       Running   0          49s       10.1.0.9   docker-for-desktop
kubernetes-bootcamp-5c69669756-qx86r     1/1       Running   0          49s       10.1.0.7   docker-for-desktop
```

Nous avons bien 4 replicats de l'application bootcamp.

De base Kubernetes gére les réplicats avec un LoadBalancing, avec Istio et d'autres composants nous pouvons régler cette logique avec du canary release par exemple.

```shell
curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-5c69669756-lrptf | v=1

C:\tools\Cmder
λ curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-5c69669756-ldkqf | v=1

C:\tools\Cmder
λ curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-5c69669756-ldkqf | v=1

C:\tools\Cmder
λ curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-5c69669756-ldkqf | v=1

C:\tools\Cmder
λ curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-5c69669756-x7jrp | v=1
```

Pour la suite du TP nous allons revenir à un replicas afin de garder des resources : 

```shell
kubectl scale deployments/kubernetes-bootcamp --replicas=2
```

Vérifions que cela fonctionne bien : 

```shell
kubectl get pods -o wide
NAME                                     READY     STATUS    RESTARTS   AGE       IP         NODE
kubernetes-bootcamp-1-684564d696-mqlvd   1/1       Running   0          4d        10.1.0.6   docker-for-desktop
```


## UPDATE :

Une des grosse force de Kubernetes est de pouvoir faire du `rolling update` afin d'avoir le moins possible de `Downtime`.
Attention, des vérifications doivent être effectué avant, avec des tests d'intégrations/unitaires ...

```shell
kubectl describe pods
Name:           kubernetes-bootcamp-1-684564d696-mqlvd
Namespace:      default
Node:           docker-for-desktop/192.168.65.3
Start Time:     Mon, 24 Dec 2018 11:31:48 +0100
Labels:         pod-template-hash=2401208252
                run=kubernetes-bootcamp-1
Annotations:    <none>
Status:         Running
IP:             10.1.0.6
Controlled By:  ReplicaSet/kubernetes-bootcamp-1-684564d696
Containers:
  kubernetes-bootcamp-1:
    Container ID:   docker://ed9f835069ddcdb139a12ea742c243a30af359f94ed7e4ee741cae00685195fa
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://gcr.io/google-samples/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 24 Dec 2018 11:31:50 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-q6z9r (ro)
Conditions:
  Type           Status
  Initialized    True
  Ready          True
  PodScheduled   True
Volumes:
  default-token-q6z9r:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-q6z9r
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>
```

Nous pouvons voir que l'imge ici est en version 1, afin de passer en version 2:

```shell
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```

Vous pouvez voir que le nouveau POD se met à jour avant d'eteindre celui en version 1 afin d'être toujours disponible. C'est ce qu'on appelle le `rolling update` :


```shell
kubectl get pods
```

Nous pouvons vérifier la bonne mise à jour avec un appel sur le endpoint :

```shell
λ curl localhost:32181
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-7799cbcb86-7g8q9 | v=2
```

Afin de vérifier nous pouvons faire aussi un `rollout` :

```shell
kubectl rollout status deployments/kubernetes-bootcamp
deployment "kubernetes-bootcamp" successfully rolled out
```

Si la mise à jour ne marche pas, pas de panique. Nous pouvons simplement faire un `rollback` : 

```shell
kubectl rollout undo deployments/kubernetes-bootcamp
```

L'information du pod permet de voir que l'application est en v1.

```shell
kubectl describe pods
```

## DELETE SERVICE :

Afin de `delete` un service, il suffit de faire la commande suivante : 

```shell
kubectl delete service -l run=kubernetes-bootcamp
```
`Attention` nous supprimons tous les services avec le label "kubernetes-bootcamp" ici.
