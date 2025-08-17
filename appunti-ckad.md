# CKAD

[corso introduttivo](https://trainingportal.linuxfoundation.org/learn/course/introduction-to-cloud-infrastructure-technologies)
## Cloud
* cloud privato - interno o esterno (es. con openstack)
* cloud ibrido - tipico scenario quando dati sono su cloud privato, computing su pubblico, utile per i burst/picchi
* multi or poly cloud x evitare vendor lock-in e sfruttare il meglio di ciascun cloud (es. finops)

## Virtualizz
* tipo 1 (bare-metal): hyperV, aws-Nitro, VMWare ESXi, RedHat...
* tipo 2 (on top of OS): vbox, vmware player o vmware ws
* ibrido (entrambi): kvm e bhive
* emulatori in accoppiata con virtualizzatori x performance. es. QEMU tool di traduz dinamica dei programmi

### KVM
> Kernel-based virtual machine
* open source e gratuita, virtualizza tutti i principali sistemi con grande scalabilità (virt tipo 1)
* originariamente x arch x86 (intel) portato poi su ARM, PowerPC, FreeBSD ...
* usa QEMU (x emulaz hw, o altri sistemi come Kubevirt) e linux kernel x accedere a risorse fisiche come CPU
* crea al suo interno un kernel con i driver e uno stack applicativo -> OS virtualizzato
* GUI come VMM (virtual machine manager)
* disco almeno 10GB (meglio su FS differente)
* network bridged o NAT

### Virtual Box
* oracle (GNU GPL v2)
* open source e gratuita, virtualizza tutti i principali sistemi - virtual di tipo 2 (non nel core di linux), facile
* x86 e AMD64
* in installaz è suggerito di installare le vbox guest additions. All'avvio dell'OS aggiornale!

## Vagrant
* hashicorp
* cross-platform CLI tool x gestire più vm e loro ciclo di vita, mentre chi poi le tira su è un provider (es. vbox o kvm, hyperv...)
* utile x team, ambienti complessi, tutto sommato semplice da configurare
* supporta vm e container - chiamate box. Si possono scaricare da vagrant box repository
* estensibile con plugin
* vagrant file = dichiarativo sintassi ruby x dire cosa voglio nel progetto. il comando legge il file
* synced folder x mount di cartelle in fs
* provisioner x installare sw ed eseguire comandi custom su macchina host


## Altro
> Dogfooding = usare quotidianamente gli strumenti k sviluppi

## Corso num 2
[]()

* Cluster è formato da nodi (macch. virtuali o fisiche) una volta detti minions
* nodo multiplo x ridondanza, carico suddiviso
* In ogni nodo:
  * API server (frontend x cli, strumenti vari)
  * etcd (KV storage distribuito sui nodi con tutte le info x usare kube. implementa lock x evitare conflitti)
  * scheduler (resoponsab distribuz carico lavoro, cerca nuovi container e li sposta di nodo)
  * controller (la mente, il servizio che risponde e agisce quando nodi, container, endpoint vanno giù)
  * container runtime (sw x far funzionare i container, di solito docker, Cri-o, Rocket)
  * kubelet (agent su ogni nodo k si assicura k i container girino cm atteso)
* nodi master (gestiscono gli altri) e nodi worker
  * master - api-server (questo lo rende master, accetta comandi), etcd, scheduler e controller
  * worker - kubelet (x interagire con master), container runtime (necessario)
* kube control è la CLI x interagire con cluster (kubectl)
  * kubectl run hello-minikube esegue deploy di una applicazione
  * kubectl cluster-info x avere info sul cluster
  * kubectl get nodes x avere info nodi

* storia: kube nasce tightly-coupled con docker, poi si disaccoppia grazie a un sistema: definisce un interfaccia (CRI: container runtime interface) dove ogni vendor si può attaccare se usa gli standard OCI (open container initiative) con imagespec (cm immag viene buildata) e runtimespec (cm eseguire immag)
* x continuare a supportare docker nativamente (bypassando la CRI) crea un hack chiamato dockershim
* docker è molto più della CRI:
  * cli, API, build, volume, auth, security demon (containerd)
* docker decide di supportare containerd e CRI e dalla 1.24 droppa supporto alla dockershim. Tutto continua a funzionare xk docker supportato con containerd xk docker supporta imagespec e runtimespec
* containerd ora è diventato proj separato della CNCF (idealmente posso usarlo senza docker) - si interagisce con ctr, comando usato solo x debug, set limitato di funzionalità.
  * ctr images pull <image>
  * ctr run <image> <image-name>
* altro modo + comodo è nerdctl, una cli più simile alla cli docker, con molte più funzionalità su containerd (creata da community containerd). Supporta docker-compose, lazy pulling, P2P distrib, immagini criptate, namespace, firma e verifica immagini
  * docker run --name redis redis:alpine -> nerdctl run --name redis redis:alpine
* crictl - cli x accedere a CRI, creata dalla community kube e supporta ogni standard OCI. Non comoda, ma x scopi di debug. Interagisce con kubelet, anche pestandosi i piedi xk kubelet tende a eliminare ciò che viene creato al di fuori della sua visibilità
  * crictl pull busybox
  * crictl images
  * crictl ps -a
  * crictl exec -i -t <container_id> ls
  * crictl logs <container_id>
  * crictl pods (differenza rispetto a comandi docker-like: crictl è consapevole anche dei pod)
  * dalla 1.24 sono cambiati gli endpoint (deprecato dockershim.sock), ora vanno specificati a manella:
  ```sh
  crictl --runtime-endpoint
  export CONTAINER_RUNTIME_ENDPOINT
  ```

### Pods
* Kube non deploya direttamente container, ma li mette in un oggetto chiamato **pod** (unità minima usata x replica di un workload) - generalmente 1:1 con container
* i pod vengono scalati creandone e uccidendone alla bisogna
* in un pod posso avere container multipli (ma non lo stesso + volte) - utile in casi di coesistenza come sito web e processo di upload dei file
* tutto ciò che è in un pod ha una sua sotto-rete dove può comunicare cm localhost. Condivide anche lo stesso spazio di storage. Condivide anche il "fate", cioè il ciclo di vita è comune: creati e distrutti
* immaginare di fare a mano tutto con docker sarebbe un suicidio: crea 1 container, creane 1 altro, fai il link tra questi, crea un volume e attaccalo a entrambi, crea una rete e attaccala a entrambi, gestisci una mappa di risorse condivise, replica tutto N volte, gestisci il ciclo di vita di tutti gli N insiemi di container. Kube fa tutto con i pod
* quindi: anche se mi serve 1 solo container, in kube deployo 1 pod
* kubectl run --image nginx nginx -> scarica immagine nginx da dockerhub (repo pubblica) e rilascia pod -- UTILE!
* kubectl get pods -> lista pod
* kubectl describe pod <pod-name> -> x avere info in dettaglio

### YAML
* tutto su kube ha struttura di base
* 4 livelli base: apiVersion, kind, metadata, spec
* apiVersion: stringa x dire quali api usare. pod e servizi in v1. Alcune cose in v1-beta, molte cose in apps/v1 (come replica set o deployment)
* kind: stringa PascalCase x dire cosa sto creando: Pod, ReplicaSet, Deployment, Service...
* metadata: dizionario/oggetto dove salvo info aggiuntive dell'oggetto kube
  ```yaml
  metadata:
    name: my-app-pod
    labels:
      app: my-app
  ```
  > Non importa quanti spazi ci siano sotto metadata (x>0), ma tutto dentro deve essere indentato equamente. labels non deve diventare figlio di name!
  * name: stringa, come voglio chiamare il pod
  * labels: dizionario stringa: any dove posso etichettare oggetti x identificarne a gruppi (es. se ho centinaia di oggetti)
* spec: dizionario/oggetto x dire cosa voglio creare e come (unica definizione per ogni kind)
    ```yaml
  spec:
    containers:
	- name: nginx-container
	  image: nginx
  ```
  * containers è una lista di oggetti. In questo caso sono i container dentro al pod
* kubectl create -f deployment-pod.yml -> rilascia un pod a partire da un template yaml
* x ottenere yaml di un pod kubectl get pod <pod-name> -o yaml > pod-definition.yaml
* x edit pod: kubectl edit pod <property> <value> ma solo alcune property sono editabili. Non il nome o le label x esempio

### Replica
* replication controller fa il lavoro sporco di tenere le istanza al numero desiderato
  * se voglio più repliche, mi garantisce il deploy di nuovi pod
  * se voglio repliche su più nodi, le fa
  * se voglio anche 1 solo pod, e questo muore, prova a deployarlo
* nelle spec si mette template e tutto il contenuto di un pod (metadata e spec)
* kubectl get replicationcontroller
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
spec:
  template:
    metadata:
      name: my-app
      labels:
        app: my-app
    spec:
      containers:
        - image: nginx
          name: nginx
  replicas: 3
```
* ora la nuova tecnologia è replica set
* apiVersion: apps/v1 - altrimenti errore che dice unable to recognize... no matches for / kind=ReplicaSet
* può usare il selector x puntare ad altri pod via label. Va scritto x forza
* garantisce che ci siano x repliche, anche con pod esistenti. Se sono già x non fa nulla. Se sono x-n ne deploya n. Se sono x+n ne killa n (penso). Se uccido 1 nodo di un replica set ne deploya 1 nuovo: x questo il template è obbligatorio!
* kubectl get replicaset
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app-rs
spec:
  template:
    metadata:
      name: my-app
      labels:
        app: my-app
    spec:
      containers:
        - image: nginx
          name: nginx
  replicas: 3
  selector:
    matchLabels:
      app: my-app
```
* come scalare?
  * aggiornare il manifest yaml e lanciare kubectl replace -f rs-definition.yml
  * kubectl scale --replicas=6 rs-definition.yml
  * kubectl scale --replicas=6 replicaset my-app-rs # type e name del RS
  * con autoscaler ma + avanzato
* kubectl delete replicaset <rs-name>
* kubectl edit rs <nome-risorsa> mi apre vim sul manifest del file. Per orientarsi in vim:
  * kubectl edit --help ha un esempio di come impostare nano come editor
  * :wq salva il file ed esce, "i" serve a inserire inplace e esc serve a tornare alla command mode

### Deployment
* a che serve?
  * gestire versioni dei pod, gestire aggiornamenti facilmente
  * gestire rolling update (aggiorna a, poi b, poi c...)
  * gestire rollback (a->v1 b->v1 ...)
  * mettere in pausa o riavviare i cambiamenti
* incapsula P replica set che incapsula Q repliche di 1 pod che incapsula R container! Quindi è High-order object e può gestire molte cose
* e il manifest? Esattamente uguale al replicaset cambiando il kind: Deployment!!!! (sempre apps/v1, sempre spec.template.(metadata, spec), spec.replicas, spec.selector.matchLabels.(*:*))
* kubectl get all - serve a vedere tutti gli oggetti creati
* kubectl create deployment --name <name> --replicas 3 --image <image> - utile!
* -o specifica l'output del comando. le opzioni utili sono: yaml|json|name (solo i nomi)|wide (info estese)

### Namespace
* analogia con casa e cognomi. Qui sono Luca. In casa di altri, sono Luca F. Fuori casa tutti hanno cognome
* tutto di default creato nel default :-D
* kube-system è x le risorse kube che non vanno cancellate o modificate accidentalmente
* kube-public x le risorse accessibili x tutti gli utenti
* ognuno può creare namespace x isolare le risorse: es. dev e prod
* in 1 namespace si possono indicare rbac policies, quotas x risorse
* in 1 namespace l'hostname è semplicemente il nome del service, es. "mysql"
* ci si può collegare a un altro svc di un diverso namespace (dev) per esempio usando "mysql.dev.svc.cluster.local"
* cluster.local è il domain name del cluster
* si può aggiungere nei comandi --namespace (-n). di default il listing è solo su default e le risorse sono create/eliminate solo dal default
* si può aggiungere nei metadati un diverso namespace x assicurarsi che ogni volta la risorsa viene creata in quel NS
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```
* oppure kubectl create namespace <ns-name>
* contesto: il mio default in cui sono
* kubectl config set-context $(kubectl config current-context) --namespace=dev
* kubectl get pods --all-namespaces x vedere ogni cosa (oppure -A)
* se voglio limitare le risorse posso farlo (num pod, cpu o ram max e min)
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-namespace-quotas
  namespace: my-namespace
spec:
  hard:
    pods: 10
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "8"
    limits.memory: 16Gi
```
* x creare un template di pod in 1 comando posso usare --dry-run=client x simulare l'esecuzione di un comando e uscire lo yaml in un file
  ```shell
  #pod
  kubectl run my-redis --image=redis --labels="tier=db" --dry-run=client -o yaml > my-redis.yaml
  kubectl run my-redis --image=redis --labels="tier=db" --expose=true --port=80 # x un pod già esposto
  #deployment
  kubectl create deployment my-nginx --image=nginx --replicas=2 --dry-run=client -o yaml > my-nginx.yaml
  #svc
  kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml # assumendo app=redis cm selettore
  kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml #
  kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
  kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
  ```