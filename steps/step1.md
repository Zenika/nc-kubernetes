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

## Introduction

Positive
: Voici la documentation utile pour cette étape

- [Kubernetes Intro](https://kubernetes.io/docs/tutorials/kubernetes-basics/)


Une fois Kubernetes installé, exécutez la commande suivante pour s'assurer que `kubectl` est bien accessible depuis votre terminal. 

```shell
kubectl --help
```
Vous devriez avoir toutes les commandes disponibles. Si vous souhaitez de l'information sur une commande en particulier, vous pouvez exécuter la commande `kubtctl get notes --help` par exemple. 


## WebUI

Afin de visualiser les travaux que nous allons faire, nous allons déployer l'interface graphique de Kubernetes. 
(Nous détaillerons le fichier de déploiement par la suite)

Pour créer une ressource à partir d'un fichier nous allons utiliser l'outil de création d'un `deployment` de kubernetes :

```shell
kubectl create -f file
```

Kubernetes a mis en place un fichier de déploiement pour le dashboard à l'adresse suivante : https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml

Nous allons simplement créer une resource avec le fichier de `deployment` fournis par Kubernetes : 

```shell
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

Afin d'accéder à l'API Kubernetes, nous allons utiliser la notion de `proxy` de Kubernetes.

```shell
kubectl proxy
```

Notez que vous pouvez spécifier un `port` précis avec l'argument suivant :

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

Avec cette API nous pouvons accéder à toutes nos ressources Kubernetes, par exemple nous pouvons accéder aux pod avec le endpoint suivant : 

```shell
curl http://localhost:8080/api/v1/namespaces/default/pods
```

Nous pouvons acceder au Dashboard avec l'url suivante : 
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/ 


Attention une authentification est requise avec le dashboard, pour cela nous allons récuperer le token dans un `secret`. (Nous ne rentrerons pas dans les détails durant se tp.)

Pour avoir accès aux secrets Kubernetes, il suffit de faire la commande :

```shell
 kubectl get secret
```

Cependant le token du dashboard est disponible dans la ressource "kubernetes-dashboard-token-xxx" qui se situe dans le namespace "kube-system".

Pour cela il faut faire :

```shell
kubectl -n kube-system get secret
NAME                                             TYPE                                  DATA      AGE
attachdetach-controller-token-px8f2              kubernetes.io/service-account-token   3         14d
bootstrap-signer-token-99qh5                     kubernetes.io/service-account-token   3         14d
certificate-controller-token-w6m4v               kubernetes.io/service-account-token   3         14d
clusterrole-aggregation-controller-token-r22qx   kubernetes.io/service-account-token   3         14d
cronjob-controller-token-qjl64                   kubernetes.io/service-account-token   3         14d
daemon-set-controller-token-mr9rx                kubernetes.io/service-account-token   3         14d
default-token-s8tgw                              kubernetes.io/service-account-token   3         14d
deployment-controller-token-mjrk2                kubernetes.io/service-account-token   3         14d
disruption-controller-token-sxl8r                kubernetes.io/service-account-token   3         14d
endpoint-controller-token-mc8pd                  kubernetes.io/service-account-token   3         14d
generic-garbage-collector-token-vvb29            kubernetes.io/service-account-token   3         14d
horizontal-pod-autoscaler-token-wm95r            kubernetes.io/service-account-token   3         14d
job-controller-token-2h2zh                       kubernetes.io/service-account-token   3         14d
kube-dns-token-jqbsn                             kubernetes.io/service-account-token   3         14d
kube-proxy-token-5jlm9                           kubernetes.io/service-account-token   3         14d
kubernetes-dashboard-certs                       Opaque                                0         14d
kubernetes-dashboard-key-holder                  Opaque                                2         14d
kubernetes-dashboard-token-sltfz                 kubernetes.io/service-account-token   3         14d
namespace-controller-token-62nkx                 kubernetes.io/service-account-token   3         14d
node-controller-token-qlft6                      kubernetes.io/service-account-token   3         14d
persistent-volume-binder-token-26cfg             kubernetes.io/service-account-token   3         14d
pod-garbage-collector-token-rbjtr                kubernetes.io/service-account-token   3         14d
pv-protection-controller-token-pcjlc             kubernetes.io/service-account-token   3         14d
pvc-protection-controller-token-bxgc4            kubernetes.io/service-account-token   3         14d
replicaset-controller-token-n2flm                kubernetes.io/service-account-token   3         14d
replication-controller-token-2nbkt               kubernetes.io/service-account-token   3         14d
resourcequota-controller-token-957f7             kubernetes.io/service-account-token   3         14d
service-account-controller-token-9h2f5           kubernetes.io/service-account-token   3         14d
service-controller-token-lglwx                   kubernetes.io/service-account-token   3         14d
statefulset-controller-token-8xv7z               kubernetes.io/service-account-token   3         14d
token-cleaner-token-czvsg                        kubernetes.io/service-account-token   3         14d
ttl-controller-token-5tjl5                       kubernetes.io/service-account-token   3         14d

```

Afin d'accéder aux détails de notre objet "secret", il faut copier l'ID de celui-ci et de détailler les différentes informations avec la commande suivante : 

```shell
kubectl -n kube-system describe secret kubernetes-dashboard-token-sltfz
Name:         kubernetes-dashboard-token-sltfz
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name=kubernetes-dashboard
              kubernetes.io/service-account.uid=f8b94671-0759-11e9-8b26-00155d380101

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

```

A partir de maintenant vous pouvez acceder à votre dashboard Kubernetes avec le token : http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/ 


![alt text](/images/kubernetesInterface.PNG "Interface graphique Kube")


Avec le Dashboard vous pouvez : 

* Créer une resource, service ou application directement avec une interface graphique.
* Gérer vos différentes ressources Kubernetes :
  * Modifier
  * Créer
  * Détruire
* Gérer facilement la mise à jour et les Replica Sets
* Gérer les configs maps/Secrets
* Gérer le load balancing
* etc ...

Notez que l'interface graphique est une aide précieuse mais elle n'est pas indispensable. Pour les futurs exercices nous allons nous aider de celle-ci mais nous ferrons toutes les manipulations en ligne de commande afin de comprendre l'utilisation de kubectl.