## Intégration avec GCP

Positive
: Voici la documentation utile pour cette étape

- [Quickstart GKE](https://cloud.google.com/kubernetes-engine/docs/quickstart)

Pour terminer cette Nightclazz. nous allons à présent déployer notre cluster **Kubernetes** sur GCP. Le contenu de cette étape est très inspirée du **Quickstart** de GKE.

Avant de commencer, voici les prérequis pour pouvoir déployer notre cluster sur GKE:

- Avoir un compte **gmail**
- Aller sur la console `https://console.cloud.google.com`, et créer un nouveau projet
- Activer ensuite Kubernetes Engine et le _Billing_
- Installer `gcloud` sur votre machine, et connectez-vous via la commande `gcloud auth login`

Voilà, nous sommes prêt pour déployer notre cluster.

- Configurez le projet et la zone utilisée

```shell
gcloud config set project [PROJECT_ID]
gcloud config set compute/zone us-west1-a
```

Le `[PROJECT_ID]` peut être récupéré depuis l'URL de la console GCP.

- Créez votre cluster **nighclazz**

```shell
gcloud container clusters create nightclazz
```

Dans la console GCP, vous devriez voir votre cluster en cours de création via la page https://console.cloud.google.com/kubernetes/list.

- Récuperez les credentials afin de pouvoir intéragir avec votre cluster

```shell
gcloud container clusters get-credentials nightclazz
```

- Appliquez votre configuration créé lors de la **step2**

```shell
kubectl apply -f config.yml
```

Dans la console GCP, vous devriez voir votre `Deployment` en cours de création via la page https://console.cloud.google.com/kubernetes/workload.

- La dernière étape est de rendre accessible vos pods de l'exterieur. Pour cela nous allons créer un service. Soit nous le faisons via un fichier de configuration YML comme vu précédemment, soit en ligne de commande.

```shell
kubectl expose deployment nightclazz --type LoadBalancer --port 8080 --target-port 8080
```

Dans la console GCP, vous devriez voir votre `Service` en cours de création via la page https://console.cloud.google.com/kubernetes/discovery.

- Depuis cette page et l'adresse IP affichée, vous devriez avoir accès à votre application. Si vous raffraichissez votre page, vous allez accéder à un `pod` différent.
