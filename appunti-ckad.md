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
* kubectl run --image nginx nginx -> scarica immagine nginx da dockerhub (repo pubblica) e rilascia pod
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