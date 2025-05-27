## Composants
- Nodes : Une machine physique ou virutelle qui exécute les conteneurs
- Pod : le plus petit composant de Kubernetes, il peut contenir plusieurs conteneurs. 
- Deployments : manage les pods, scalabilité, déploiement automatique et la mis à jour des pods.
- Services :  Composant réseau (ie: load balancer etc...)
## Architecture
- Les composans système sont définis comme des pods
- Controller manager seulement présent dans les master, surveille l'état global
- Kubelet : binaire présents dans les master et workers, il surveille le bon fonction du conteneur.
- Api Server : communication entre les workers
- Kube proxy : controleur réseau. Permet l'exposition d'un service, d'un port etc..
- ETCD : (présent dans K8s par défaut, mais pas dans K3s qui utilise SQLite) petit base de donnée clé-valeur, logs tout ce qu'il se passe dans le cluster. Ce qui va permettre la restauration d'un cluster.
- Kube-Scheduler : 
## Namespaces
Pour que des pods d'un projet puisse communiquer entre eux, il faut qu'ils appartiennent au même namespace

`kubctl get namespaces`

## Label

Sous forme de clé/valeur, l'une des conditions pour pouvoir communiquer entre les services d'un même node.

Permet de gérer les affinités

Pour gérer ses pods, un depoyment doit avoir le même label que ses pods

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 2
  selector:
	  matchLabels:
	    app: web
  template:
    metadata:
      labels:
        app: web
  spec:
    container:
      - name: nginx
# etc...
```

## Service

1. Cluster IP
   Permet d'exposer un port en interne. Par défaut un service est de type `Cluster IP`.
   `targerPort`: le port de **l'image**
   `port`: port interne à exposer
2. NodePort
   ne pas utiliser en prod pour l'exposition des ports vers l'extérieur
3. LoadBalancer
	1. D'abord créer u clusterIP,  car l'Ingress va devoir se connecter via le port interne exposé. Puis 1 second manifest, le load balancer
## Ingress
Ingress controller: Nginx ou Traefik, on ne peut en avoir qu'un seul par application.
Utilisé par le loadbalancer.
### TLS/SSL
ajouter
```yml
spec:
	tls:
	- hosts:
		- domain.lan
		secretName: domain-tls-secret
```

Créer un certifical TLS auto-signé :
```bash
kubctl create secret tls domain-tls-secret \

```

## BDD
Vaut mieux pas l'intégrer dans kube, sinon il faut utiliser un nodeSelector pour attacher le pod à un Worker donné ou Master, pour être sur où la BDD sera.