## Les PODS

Positive
: Voici la documentation utile pour cette étape

- [Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/)
- [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

Comme nous venons d'installer Kubernetes, vous ne devriez pas avoir de nodes en cours d'exécution dans votre cluster. Vous pouvez le vérifier via la commande suivante:

```shell
kubectl get nodes
```

- Nous allons lancer un premier pod via la commande suivante:

```shell
kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080
```

- Voilà, vous avez lancer votre premier pod. Mais est-ce qu'il est fonctionnel ? Executez la commande suivante afin de lister l'état de tous nos pods.

```shell
kubectl get pods
```

Le pod que nous venons de lancer est à l'état `PENDING` car Kubernetes n'a pas réussi à le déployer. En effet, Notre cluster ne possède aucun noeud, sur lequel nous pouvons déployer nos ressources. Il ne sait donc pas ou le mettre.

- Nous allons installer **Minikube** à présent. Vous pouvez suivre les instructions sur le repository du projet https://github.com/kubernetes/minikube/releases.

- Une fois **Minikube** installé, vous pouvez le lancer via la commande `minikube start`

- Si vous executez de nouveau la commande `kubectl get nodes`, vous devriez voir le noeud **Minikube**

- Si vous déployez de nouveau le pod `kubernetes-bootcamp`, il devrait avoir l'état `Running` à présent.

- Notre application est à présent accessible dans le réseau du cluster, mais pas à l'exterieur. Elle le sera lorsque nous aurons aborder les `service`. Mais pour l'instant, nous pouvons la rendre accessible, via la commande `proxy` de Kubernetes. Dans un nouveau terminal, exécutez la commande:

```shell
kubectl proxy
```

- Si vous executez la commande suivante, vous devriez récuperer les informations générales du cluster

```shell
curl http://localhost:8001/version
```

- Une fois le proxy activé, vous pouvez à présent accéder à chaque pod.
- Récupérez le nom de votre pod via la commande `kubectl get pods`
- Accédez à votre pod en exéucant la commande:

```shell
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```

- Voici d'autres commandes que vous pouvez exécuter pour intéragir avec votre pod

```shell
kubectl logs $POD_NAME // pour visualiser les logs
kubectl exec $POD_NAME cat server.js // pour exécuter une commande dans le container
kubectl describe pods $POD_NAME // pour avoir des informations sur le pod
```

- Pour l'instant, nous avons lancer qu'une seule instance de notre pod. Vous pouvez le vérifier en executant la commande suivante

```shell
kubectl get deployment
```

- Grace à une autre ressource, les `ReplicaSet`, il y aura toujours une instance de votre pod fonctionnelle. Pour s'en assurer, veuiller executer les commandes suivantes

```shell
kubectl delete pods $POD_NAME
kubectl get pods
```

- Pour supprimer votre pod, ainsi que les ressources associées, vous pouvez exécuter la commande suivante:

```shell
kubectl delete deployment $DEPLOYMENT_NAME
```

- Nous allons à présent créer notre premier fichier de description **Kubernetes**. Avant cela, vous pouvez lancer la commande suivante en mode `watch`

```shell
kubectl get pods -w
```

- Veuillez créer un fichier `config.yml` avec le contenu suivant

```yml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: nightclazz
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nightclazz
  template:
    metadata:
      labels:
        app: nightclazz
    spec:
      containers:
        - name: nightclazz
          image: gcr.io/google-samples/kubernetes-bootcamp:v1
          imagePullPolicy: Always

          ports:
            - containerPort: 8080
      restartPolicy: Always
```

- Pour appliquer cette configuration, exécutez la commande suivante

```shell
kubectl apply -f config.yml
```

Cette fois-ci, nous allons avoir trois instances de notre pod (Cf `replicas`)

- Vous pouvez à présent exécuter les mêmes commandes que précédemment, afin d'avoir le meme résultat. Attention le nom du pod a peut-être changé.

- Le fait d'ajouter des labels à nos ressources, va nous permettre de filtrer le résultat de nos commandes. Par exemple, si je souhaite avoir les infos des pods ayant le label `app` égal à `nightclazz`, je peux utiliser la commande suivante :

```shell
kubectl get pods -l app=nightclazz --show-labels
```
