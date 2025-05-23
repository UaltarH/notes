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