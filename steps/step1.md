author: Zenika
summary: Initiation à Kubernetes
id: kube-codelab

# Kubernetes

Prérequis:

- Docker
- MiniKube

# Installation 

 ### macOS

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

### Linux

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
# ou
mv ./kubectl ~/.local/bin/kubectl
```

### Windows

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.10.3/bin/windows/amd64/kubectl.exe
```

- Ajuster le `PATH` pour pouvoir lancer `kubectl`

### Optionnel mais fortement conseillé sous macOS et Linux : completion

Sous Linux, MacOS ou Windows Bash: Mettre en place la completion bash (ou zsh) pour `kubectl`. Complétion pour [fish](https://fishshell.com/) partielle disponible [ici](https://github.com/evanlucas/fish-kubectl-completions) ou [ici](https://github.com/tuvistavie/fish-kubectl).

Par exemple, sous linux :

```shell
kubectl completion bash > ~/.kube_completion.sh
echo "source ~/.kube_completion.sh" >> ~/.bashrc
```

## Pods et Deployment

Positive
: Voici la documentation utile pour cette étape

- [Blabla](blabla)

### TODO



