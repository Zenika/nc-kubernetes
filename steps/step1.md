author: Zenika
summary: Initiation à Kubernetes
id: kube-codelab

# Kubernetes

Prérequis:

- Docker
- Kubernetes

Pour installer Docker et Kubernets, vous pouvez utiliser par exemple

- Docker for Mac: https://hub.docker.com/editions/community/docker-ce-desktop-mac
- Docker for Windows: https://hub.docker.com/editions/community/docker-ce-desktop-windows
- Docker for Linux: 
https://docs.docker.com/install/linux/docker-ce/ubuntu/
https://kubernetes.io/docs/getting-started-guides/ubuntu/installation/

## Installation

Positive
: Voici la documentation utile pour cette étape

- [Kubernetes Intro](https://kubernetes.io/docs/tutorials/kubernetes-basics/)


Une fois Kubernetes installé, exécutez la commande suivante pour s'assurer que `kubectl` est bien accessible depuis votre terminal. 

```shell
kubectl --help
```
Vous devriez avoir toutes les commandes disponibles. Si vous souhaitez de l'information sur une commande en particulier, vous pouvez éxecuter la commande `kubtctl get notes --help` par exemple. 

Afin de nous aider et de visualiser les travaux que nous allons faire, nous allons déployer l'interface graphique de Kubernetes. Nous rentrerons dans les détails par la suite (notamment du fichier de dployment)

Pour créer une resource à partir d'un fichier nous allons simplement utiliser la commande :

```shell
kubectl create -f file
```

Kubernetes à mis en place un fichier de deploiement pour le dashboard à l'adresse suivante : https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml

Nous allons simplement faire la commande suivante afin de créer la ressource : 

```shell
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

Afin d'acceder en ligne à notre service Kubernetes et notamment utiliser l'API Kubernetes, nous allons utiliser la notion du `proxy` Kubernetes. Pour cela rien de plus simple : 

```shell
kubectl proxy
```

Notez que vous pouvez specifier un `port` précis avec l'argument suivant :

```shell
kubectl proxy --port=8080
```

Afin de vérifier le bon fonctionnement de notre proxy nous pouvons effectuer un appel à celui-ci : 

```shell
curl http://localhost:8080/api/            
{
  "kind": "APIVersions",
  "versions": [
    "v1"
  ],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "192.168.65.3:6443"
    }
  ]
}
```

Avec cette API nous pouvons acceder à toutes nos resources Kubernetes, par exemple nous pouvons accéder aux pod avec le endpoint suivant : 

```shell
curl http://localhost:8080/api/v1/namespaces/default/pods
```

Nous pouvons acceder au Dashboard avec l'url suivante : 
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/ 


Attention une authentification est requise avec le dashboard, pour cela nous allons voir la notion des `secrets`. Nous ne rentrerons pas dans les détails durant se tp.

Pour avoir accés aux secrets Kubernetes, il suffit de faire la commande 

```shell
 kubectl get secret
```

Cependant le token du dashboard est disponible dans la resource "kubernetes-dashboard-token-xxx" qui se situe dans le namespace "kube-system".

Pour cela il faut faire :

```shell
kubectl -n kube-system get secret
```

Afin d'acceder aux détails de notre objet "secret", il faut copier l'ID de celui ci et de détailler les differentes informations avec la commande suivante : 

```shell
kubectl -n kube-system describe secret replicaset-controller-token-xxxx
```

(Attention xxx est un ID unique)
