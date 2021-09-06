## Kubernetes

----

### Orchestrateur de containers

Afin de garantir qu'un container fonctionne, il est nécessaire d'avoir un orchestrateur de container

Celui-ci garanti que le container soit toujours en fonctionnement, même si les noeuds du cluster se mettent à jour ou tombent en panne

----

### Docker Swarm

Docker Swarm est l'orchestrateur par défaut de Docker

Il est inclus dans le moteur et simple d'utilisation

Cependant, il ne répond pas à des contraintes de production

----

### Docker Swarm

<img src="https://cdn-images-1.medium.com/max/1207/1*PYC_TkhBjc0vtaPjjrg6MQ.png" style="background:none; border:none; box-shadow:none;"/>

----

### Kubernetes

<img src="img/kubernets-architecture.png" style="background:none; border:none; box-shadow:none;"/>

----

### Pods

Un pod est un regroupement de container au sein d'une même unité

<img src="https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg" style="background:none; border:none; box-shadow:none;" width="50%"/>

----

### Horizontal pod autoscaler

Lorsqu'une demande est faite de scaler les pods, HPA s'occupe de démarrer les pods sur les noeuds correspondant, respectant les contraintes imposées

----

### Deployment

* Un déploiement est la manière de déployer un pod
* Un déploiement est déclaratif, c'est-à-dire que Kubernetes va s'assurer que le nombre de pods est toujours celui demandé

<img src="https://d33wubrfki0l68.cloudfront.net/152c845f25df8e69dd24dd7b0836a289747e258a/4a1d2/docs/tutorials/kubernetes-basics/public/images/module_02_first_app.svg" style="background:none; border:none; box-shadow:none;" width="45%"/>

----

### Services

* Un service est la manière d'accéder aux pods
* Un service expose une adresse IP, ainsi qu'un port
* Il joue le role de load-balancer

<img src="https://d33wubrfki0l68.cloudfront.net/cc38b0f3c0fd94e66495e3a4198f2096cdecd3d5/ace10/docs/tutorials/kubernetes-basics/public/images/module_04_services.svg" style="background:none; border:none; box-shadow:none;" width="35%"/>

----

### Ingress

L'ingress sert à exposer à l'extérieur du cluster un service

Cela se représente traditionnellement par un load balancer, une URL ainsi que du SSL

----

### Cluster autoscaler

Il sert à augmenter le nombre de noeuds du cluster automatiquement quand celui-ci n'a plus assez de ressource, ou que les noeuds sont sous utilisés

----

### Daemonset

Un daemonset garanti qu'un cluster fasse tourner une copie d'un pod sur chaque noeud automatiquement

Pratique quand il faut de la supervision (ex: datadog), du logging (ex: fluentd) ou du stockage distribué (ex: ceph)

----

### Storageclass

Définie une classe de stockage basée sur la qualité de service, la stratégie de backup ou une policy définie par l'administrateur

----

### Persistent volume

Abstrait la notion de StorageClass pour fournir une API de pilotage de volume

Basé sur un système de plugin (iSCSI, NFS, ...)

----

### Persistent volume claim

Requête de stockage par l'utilisateur qui va se greffer sur un PV

Demandée par un pod

----

### Configmap

Défini une configuration à appliquer sur un pod

```yaml
apiVersion: v1
data:
  redis-config: |
    maxmemory 2mb
    maxmemory-policy allkeys-lru
kind: ConfigMap
metadata:
  creationTimestamp: 2016-03-30T18:14:41Z
  name: example-redis-config
  namespace: default
  resourceVersion: "24686"
  selfLink: /api/v1/namespaces/default/configmaps/example-redis-config
  uid: 460a2b6e-f6a3-11e5-8ae5-42010af00002
```

----

### Secret

Un secret contient des informations sensibles qui ne doivent pas être exposées et accessibles uniquement à un pod

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

----

### Rolling update

<img src="https://d33wubrfki0l68.cloudfront.net/30f75140a581110443397192d70a4cdb37df7bfc/fa906/docs/tutorials/kubernetes-basics/public/images/module_06_rollingupdates1.svg" style="background:none; border:none; box-shadow:none;" width="70%"/>

----

### Rolling update

<img src="https://d33wubrfki0l68.cloudfront.net/678bcc3281bfcc588e87c73ffdc73c7a8380aca9/703a2/docs/tutorials/kubernetes-basics/public/images/module_06_rollingupdates2.svg" style="background:none; border:none; box-shadow:none;" width="70%"/>

----

### Rolling update

<img src="https://d33wubrfki0l68.cloudfront.net/9b57c000ea41aca21842da9e1d596cf22f1b9561/91786/docs/tutorials/kubernetes-basics/public/images/module_06_rollingupdates3.svg" style="background:none; border:none; box-shadow:none;" width="70%"/>

----

### Rolling update

<img src="https://d33wubrfki0l68.cloudfront.net/6d8bc1ebb4dc67051242bc828d3ae849dbeedb93/fbfa8/docs/tutorials/kubernetes-basics/public/images/module_06_rollingupdates4.svg" style="background:none; border:none; box-shadow:none;" width="70%"/>

----

### Liveness and readiness probes

```yaml
livenessProbe:
    exec:
      command:
      - cat
      - /tmp/healthy-liveness
    initialDelaySeconds: 5
    periodSeconds: 5
```

----

### Liveness and readiness probes

```yaml
readinessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy-readiness
  initialDelaySeconds: 5
  periodSeconds: 5
```

----

### Logging
#### Node level

<img src="https://d33wubrfki0l68.cloudfront.net/59b1aae2adcfe4f06270b99a2789012ed64bec1f/4d0ad/images/docs/user-guide/logging/logging-node-level.png" style="background:none; border:none; box-shadow:none;"/>

----

### Logging
#### Node logging agent

<img src="https://d33wubrfki0l68.cloudfront.net/2585cf9757d316b9030cf36d6a4e6b8ea7eedf5a/1509f/images/docs/user-guide/logging/logging-with-node-agent.png" style="background:none; border:none; box-shadow:none;"/>

----

### Logging
#### Streaming sidecar container

<img src="https://d33wubrfki0l68.cloudfront.net/c51467e219320fdd46ab1acb40867b79a58d37af/b5414/images/docs/user-guide/logging/logging-with-streaming-sidecar.png" style="background:none; border:none; box-shadow:none;"/>

----

### Logging
#### Sidecar with a logging agent

<img src="https://d33wubrfki0l68.cloudfront.net/d55c404912a21223392e7d1a5a1741bda283f3df/c0397/images/docs/user-guide/logging/logging-with-sidecar-agent.png" style="background:none; border:none; box-shadow:none;"/>

----

### Logging
#### Exposing log

<img src="https://d33wubrfki0l68.cloudfront.net/0b4444914e56a3049a54c16b44f1a6619c0b198e/260e4/images/docs/user-guide/logging/logging-from-application.png" style="background:none; border:none; box-shadow:none;"/>

----

### Monitoring and alerting

Le monitoring d'une application se fait avec des outils tierces, comme Prometheus par exemple

----

### Monitoring and alerting

<img src="https://camo.githubusercontent.com/37141c73c72e1e73ec0669ae6451d834ebe1cd2e/68747470733a2f2f7777772e63616d696c2e6f72672f636f6e74656e742f696d616765732f323031372f636c75737465722e706e67" style="background:none; border:none; box-shadow:none;"/>

----

### Offres managés cloud

La plupart des CSP offrent aujourd'hui des Kubernetes managés, comme par exemple AKS, EKS, GKE

----

### Helm

Helm est le package manager de Kubernetes

Déploit des paquets appelés chart, contenant l'architecture complète du logiciel

C'est l'équivalent de apt-get ou yum

----

### Helm architecture

<img src="img/helm-architecture.png" style="background:none; border:none; box-shadow:none;"/>

----

### Service Mesh

Un service mesh est un service qui assure la communication entre différentes versions d'une même application

Il sert aussi pour tout ce qui est distributed tracing et debugging

----

### Istio architecture

<img src="img/istio-architecture.png" style="background:none; border:none; box-shadow:none;"/>
