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
- Docker for Linux: TODO

## Installation

Positive
: Voici la documentation utile pour cette étape

- [Blabla](blabla)


- Une fois Kubernetes installé, exécutez la commande suivante pour s'assurer que `kubectl` est bien accessible depuis votre terminal. 

```shell
kubectl --help
```
Vous devriez avoir toutes les commandes disponibles. Si vous souhaitez de l'information sur une commande en particulier, vous pouvez éxecuter la commande `kubtctl get notes --help` par exemple. 

//////////// NOTES à supprimer
$ kubectl help 
> l’aide Kubernetes extrêmement bien faite !

$ kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
> Création d’une resource à partir d’un fichier, nous reviendrons plus tard sur la description de celui-ci

$ kubectl proxy
> Permet d'accéder à l’API de Kubernetes à partir d’une adresse IP

Accéder à Kuberneters WEB-UI : http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/ 

/!\ Depuis kubernetes (a remplir j’ens suis pas sur), une authentification est requise. Pour des besoins de simplicités nous utiliserons le Token

Vérifier les secrets
$ kubectl -n kube-system get secret

(A completer ici)
$ kubectl -n kube-system describe secret replicaset-controller-token-kzpmc

