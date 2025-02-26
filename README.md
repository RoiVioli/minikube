# minikube

## Install 

```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

## Start cluster

```bash
minikube start
```

## Kubectl

```bash
minikube kubectl -- get pods -A
```

## Alias Minikube

```bash
alias kubectl="minikube kubectl --"
```

## Addons 

```bash
minikube addons enable metrics-server
```

## Dashboard

```bash
minikube dashboard
```

## Commands

```bash
kubectl cluster-info 
```
```bash
kubectl get nodes
```
```bash
kubectl get ns 
```
```bash
kubectl get all
```
```bash
kubectl describe -n kube-system <name>
```
```bash
kubectl create ns <name>
```

## Deploy an app

```bash
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

```bash
kubectl get deployments
```

## Run proxy

```bash
kubectl proxy
```

## Get the Pod name and access it via the proxy

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

## Access the Pod through the proxy

```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME:8080/proxy/
```

## Kubernetes Glossaire (Simplified)
- Kubernetes Control Plane : c'est un peu le cerveau du cluster, il contient des services vitaux pour notre cluster. Sans lui, on passe un très mauvais quart d'heure.  
- CoreDNS : DNS pour kubernetes, permet de résoudre addresses et url au sein du cluster.  
- Namespaces : abstraction logique dans kubernetes, qui vous permet d'organiser vos applications.  
- Pod : le niveau d'abstraction le plus fin dans kubernetes, c'est une abstraction au niveau des conteneurs. Cela permet à kubernetes d'être technology-agnostic (kubernetes peut utiliser différentes technologies de conteneurisation : docker, kvm, lxc, etc.)
- Virtual network : chaque pod reçoit une adresse IP, c'est comme ça qu'il communique avec le cluster et/ou d'autres pods.  
- Service : IP statique/permanente attachée à un noeud. Peut servir de *Load Balancer* pour les pods. Cependant, un service et ses pods ne sont pas connectés : ils sont indépendants. Cad que si on tue ("kill") un pod, l'IP du service restera.  
  - External Service : c'est un manière d'exposer des pods 
  - Internal Service : c'est un manière d'exposer des pods (reste interne au cluster, use case : DB, etc.)
- Ingress : permet lier des URLs (nom de domaine) à des IPs dans votre cluster  
- ConfigMap : "fichiers" configuration externes qui permettent de configurer vos services 
  - En cas de modification de la configmap, l'abstraction de configmap va redémarrer, mais pas besoin de redémarrer le conteneur ! 
- Secrets : permet de stocker mot de passe et autres donnéed sensible (token, service accounts, ...)
- Volumes : permet d'attacher du stockage physique
  - local : stockage sur le noeud donné 
  - remote (network) : stockage en dehors du cluster kubernetes
- Deployment : une abstraction sur les pods, pour les applications dites StateLESS (cad sans état, exemple : Web API, etc.)
- StatefulSet : une abstraction sur les pods, pour les applications dites StateFUL (cad avec état, exemple : Base de Données)