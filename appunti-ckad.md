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
* di un pod a runtime posso cambiare poche cose: immagine, immagine dell'initContainer, e qualche parametro. Se vado in edit, mi impedisce di salvare o mi fa salvare un file yaml localmente. Di un deployment posso cambiare tutto (kubectl edit deployment) xk il pod ne è figlio e quindi ho controllo.
* Se cmq voglio forzare l'edit o delete di un pod con cancellazione posso fare `kubectl replace --force -f deployment.yaml`

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

## Containerization
```dockerfile
FROM Ubuntu
# start from an image (mandatory and first)
# then install dependencies
RUN apt-get update
RUN apt-get install python
# ...
RUN pip install flask flask-mysql
# copy local to remote system
COPY . /opt/source-code
# command when it runs in the container
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
  ```
    
```sh
docker build Dockerfile -t <user>/<app>
docker push <user>/<app>
```

ogni istruz docker crea un layer x:
* riuso se ne faccio tanti
* cache se sto facendo più volte cose simili
* cache se sto fallendo e riprendo da un certo punto
con docker history si vedono i vari layer e il peso di ciascuno in kB e MB

```sh
docker images
docker run -p 8282:8080 webapp-color
# s capire la release di un docker o si guarda la history oppure
docker run python:3.6 cat /etc/*release*
```

Rispetto a un OS, un container mira a runnare un task/processo
X cui se faccio docker run ubuntu, uscirà (exited)

Se invece faccio
```dockerfile
FROM Ubuntu
CMD sleep 5
```
Questa immag vive 5 secondi
CMD accetta anche lista di parametri come CMD ["sleep","5"] ma non ["sleep 5"]
```dockerfile
FROM Ubuntu
ENTRYPOINT ["sleep"]
```
Usando ENTRYPOINT invece di CMD posso rendere parametrico il mio dockerfile xk specifico un programma da eseguire
docker run ubuntu-sleeper 10
```dockerfile
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```
Questo da un default di 5 secondi ma con possibilità cm prima di parametrizzare
Si può fare override anche a runtime docker run --entrypoint

Come passare argomenti a un container in un pod:
```yaml
  spec:
    containers:
	- name: nginx-container
	  image: nginx
    args: ["5"]
  ```
  invece x specificare un entrypoint (il programma da eseguire a runtime) + argomenti
  ```yaml
  spec:
    containers:
	- name: nginx-container
	  image: nginx
    command: ["sleep2.0"]
    args: ["5"]
  ```
  > falso amico! Non è command che fa l'override di CMD!
  > cannot unmarshall number... errore quando sto tentando di parsare un intero come stringa. Tutti comandi e argomenti sono SOLO stringhe
* se voglio vedere un pod esistente con il describe vedo anche il suo command con cui è inizializzato

## Configurazione

* configMap x gestire configurazione con + pod semplicemente, iniettata all'avvio del pod
* kubectl create configmap <my-config-map> --from-literal=APP_COLOR=blue --from-literal=APP_MODE=test
* oppure --from-file=<filepath> xk usare cento from-literal è scomodo anche se si può usare \ per spezzare com in + righe
* approccio dichiarativo
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myconfigmap
data:
  APP_COLOR: blue
  APP_MODE: test
```
* ocio, no spec ma data! no lista ma oggetto
* kubectl get configmaps / kubectl describe configmaps

* si può iniettare tutta la config map in un container con envFrom:
```yaml
  spec:
    containers:
	- name: nginx-container
	  image: nginx
    envFrom:
      - configMapRef:
        name: my-app-config
```

* si può iniettare singola chiave della config map usando env:
```yaml
  spec:
    containers:
	- name: nginx-container
	  image: nginx
    envs:
      - name: ENVIRONM
        value: production
```
corrisponde a docker run -e ENVIRONM=production
* oppure prendi valore da config map o secret

```yaml
  spec:
    containers:
	- name: nginx-container
	  image: nginx
    envs:
      - name: ENVIRONM
        valueFrom:
          configMapKeyRef: xxx
      - name: ENVIRONM
        valueFrom:
          secretKeyRef: xxx
```
* si può montare un'intera config map come file su un volume
```yaml
  spec:
    containers:
    - name: nginx-container
      image: nginx
    volumes:
      name: appConfigVolume
      appConfig:
        name: myAppConfig
```
* secret sono esattamente identici alle config map, sia i comandi che i manifest. Basterebbe fare un replace word-by-word. es. envFrom.secretRef, volumes[0].secret.secretName. NOTA: in create va specificato che vogliamo creare un secret generic (opaco)
* nota: i value dei secret dentro il campo data del manifest sono in base64, altrimenti stringData
* echo -n "ciao beo" | base64 # ->> encode a string into base64
* echo -n "Y2lhbyBiZW8=" | base64 --decode
* sicurezza dei secret:
  * sono encoded non encrypted! -> non vanno committati con il codice
  * non sono encrypted in ETCD -> configurare [encryption at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data) x certe risorse
  * sono visibili da tutti in un namespace -> configurare access policy role based
  * si possono usare provider cloud per iniettare secret AWS, GCP, Azure. Oppure Helm Secret o HashiCorp Vault
  * si può usare [CSI driver](https://www.youtube.com/watch?v=MTnQW9MxnRI&ab_channel=KodeKloud) -- TODO guardare di che si tratta

---
### encryption at rest
```sh
apt-get install etcd-client
etcdctl <...certificates> get my-super-secret | hexdump -C # it appears that secrets are not encrypted
# check kubeapis se è gia attiva la encrypt
ps -aux | grep kube-api | grep "encryption-provider-config"
cat /etc/kubernetes/manifests/kube-apiserver.yaml 
# crea file
/etc/enc.yaml
# incolla dalla doc una EncryptionConfiguration
# specifica x k risorse (secret) e k provider (ordine conta!) cioè quali alg di cifratura
# incolla
--encryption-provider-config=/etc/enc.yaml # montata come volume nel pod (vedi docum)
# fai tutte le mdoficihe necessarie al file /etc/kubernetes/manifests/kube-apiserver.yaml 
# salvando, il server si riavvia x i cambiamenti e ci mette un po'. se fai "k get po" fallsice un po'
# ora, i nuovi secret sono encrypti, i vecchi no, a meno k non fai
k get secrets -A -o json | k replace -f -
```

## Docker security
rispetto vm, i container nn sono isolati al 100% dall'host. condividono il kernel ma separano i processi usando i namespace
dentro docker vedi 1 solo processo con pid 1 (namespace isolato)
ma dall'host vedi i processi docker con pid diverso dall'altro namespace
* di default docker usa utente root sia nell'host che dentro il container
* se si vuole cambiare il default: `docker run --user=1000` (userid) o mettendo nel dockerfile `USER 1000`
* root dell'host è potentissimo. root del docker di default ha dei permessi in meno (es. nn può terminare processi di sistema o modificare networking)
* se si vuole fare override docker run --cap-drop KILL o docker run --cap-add <permission> o addirittura --privileged
* nel pod si può aggiungere queesti parametri di sicurezza sia a livello di pod (globale x tutti i container) che nel signolo container
```yaml
kind: Pod
spec:
  securityContext:
    runAsUser: 1001
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
```
oppure sul container (ank capabilities):
```yaml
kind: Pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]  
      securityContext:
        runAsUser: 1001
        capabilities:
          add: ["MAC_ADMIN"]
```
altro modo x vedere k utente esegue un pod è
k exec -i <pod_name> -- whoami

### Risorse
* CPU e RAM usate dallo scheduler x bilanciare carico nei nodi. Se è tutto pieno, scheduling fallisce
* CPU è definita in unità (0.1 - 1 - infinito) oppure milliunità (1m - 2000m - infinito) - 1vCPU/hyperthread/core nome dipende dal provider
* RAM è definita in Mebibyte di solito (potenze di 2. 1Kibibyte = 1024 B, 1 KiloByte sono 1000 B. 1Mib = 1048576B)
* Si può definire ram con unità Ki Mi Gi o K M G (kibi, mebi, gibi, kilo mega giga)
* si definisce min max come richieste e limiti
```yaml
kind: Pod
spec:
  containers:
    - name: nginx
      image: nginx
      resources:
        requests:
          cpu: 500m
          memory: "500Mi"
        limits:
          cpu: 2
          memory: "2Gi"
```
* quando pod eccede limiti? se è cpu throttla. se è memoria, viene lasciato fino all'OOM (errore out of memory)
* non settare risorse è scorretto xk possono competere in risorse
* CPU: settare il massimo è utile (es. evita picchi) ma a volte controproducente: se ho risorse xk altri pod sono scarichi, xk limitare un pod k ha bisogno? - l'approccio vincente è settare le risorse minime di cpu e non le max. un caso è quello di evitare azioni come mining crypto x cui limitarre cpu di pod
* la memoria nn si può throttlare. l'unico modo x liberarla è killare il pod
* cm settare limiti x tutto il cluster? default è limite massimo, default req è minimo
```yaml
apiVersion: v1
kind: LimitRange
spec:
  limits:
    - default:
        cpu: 500m
      defaultRequest:
        cpu: 500m
      max:
        cpu: 2
      min:
        cpu: 100m
      type: Container
```
* ocio k se creo un pod cicciotto, poi le limitrange, il pod nn è intaccato, ma solo i nuovi o quelli aggiornati
* cm settare x tutto il clister? usando le resourcequotas
```yaml
apiVersion: v1
kind: ResourceQuota
spec:
  hard:
    requests.cpu: 500m
    limits.cpu: 2
    requests.memory: "500Mi"
    limits.memory: "2Gi"
```

### Utenti
* account
   * user account - umano, dev, admin
   * service account - prometheus o jenkins
trattiamo i service account
   * k create serviceaccount my-sa # crea account, token nei secret
   * k get sa, k describe sa, k get secret serviceaccount-token-xhsmc
   * tokrn usabile cm bearer nelle richieste rest --header Authorization: Bearer <token:base64>
   * il sa di default c'e in ogni ns (limitato): lo vedi col describe. viene montato di default su tti i pod (cosi possono accedere a kube api x es). x robe + potenti, es. modifica dei pod, va montato a manella
   * k exec -it <pod> -- ls /var/run/secrets/kubernetes.io/serviceaccounts # dentro token c'è il secret
   * x override: spec.automountServiceAccountToken: false
   * 1.22: ora nn viene montato direttamente il token ma un ogg TokenRequestAPI k ha un token time-bound (prima era illimintato)
   * 1.24: ora nn viene + creato di default il token del sevice account.
     * k create serviceaccount my-dashboard
     * k create ***token*** my-dashboard # con eventuale expiry allungato
     * si può cmq creare un secret con annotation e tipo specifico x renderlo illimitato, ma nn è il default. e' sconsigliato, a favore invece di TokenRequestAPI
     * kubectl set serviceaccount deploy/web-dashboard dashboard-sa oppure nelle spec del pod si mette serviceAccountName e il nome del SA

### Taints and toleration
* taint su un nodo e toleration su un pod servono x lo scheduler x definire cosa va e cosa no su un nodo
* di default nessun nodo ama i taint. Solo con la toleration ci può andare (cm insetticida e insetti)
* kubectl taint nodes <node-name> key=value:<taint-effect>
* key value come app=blue
* taint effect è cs succede se un po non tollera quel taint: NoSchedule|PreferNoSchedule|NoExecute (ultimo cn effetto retroattivo, fa eviction dei po esistenti)
* toleration:
```yaml
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```
* NB: per qualche motivo va tutto tra doppi apici
* NB2: taint e toleration fanno si k in un nodo finiscano solo certe cose, ma non garantiscono nulla. Se dico k nodo X accetta solo pod ti tipo B, questo può andare su un altro nodo Y, senza problemi.
* sto meccanismo è usato nei cluster di default x dire k nel master nodo non puoi deployare workload.
* kubectl describe node kubemaster | grep Taint
* x rimuovere taint si usa k taints node nodename a=b:NoSchedule-

### nodeSelectors e affinity
* servono a dire k certi pod vadano su certi nodi
* k label node <node-name> key=value
* dopo containers si aggiunge
```
  containers:
  # ...
  nodeSelector:
    size: Large
```
utile ma limitato xk nn posso fare robe complesse cm dire k un pod va in medio o grande o negazione

con l'affinity si possono usare operatori complessi e comporre regole ma pagando in semplicità
operatori In, NotIn, Exist (matcha la chiave, conqualsiasivalore)
ma senon matcha nulla? dipende dal node affinity type
```
  containers:
  # ...
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: NotIn
              values:
                - Large
```
requiredDuringSchedulingIgnoredDuringExecution -> required in scheduling. Se nn c'è la label non schedula
prefered... -> Se nn c'è la label schedula cmq
required...required... -> se nn c'è la label o la rimuovo a runtime, non schedula o fa eviction del pod!

* quando usare taint e tolerations insieme? Nel caso in cui: voglio k i pod vadano su nodi specifici e k altri pod non vadano su quei nodi. es. pod R G B X Y e nodi r g b x y. voglio Rr Gg Bb e non mi interessa X e Y. Mi basta che non vadano Rg o Rx o Xg...

alias kk=kubectl
export KUBECTL_EDITOR="nano"

## pod multicontainer