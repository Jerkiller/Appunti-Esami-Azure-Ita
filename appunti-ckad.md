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

[Link a corso Udemy](TODO)

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
  * `kubectl run hello-minikube` esegue deploy di una applicazione
  * `kubectl cluster-info` x avere info sul cluster
  * `kubectl get nodes` x avere info nodi

* storia: kube nasce tightly-coupled con docker, poi si disaccoppia grazie a un sistema: definisce un interfaccia (CRI: container runtime interface) dove ogni vendor si può attaccare se usa gli standard OCI (open container initiative) con imagespec (cm immag viene buildata) e runtimespec (cm eseguire immag)
* x continuare a supportare docker nativamente (bypassando la CRI) crea un hack chiamato dockershim
* docker è molto più della CRI:
  * cli, API, build, volume, auth, security demon (containerd)
* docker decide di supportare containerd e CRI e dalla 1.24 droppa supporto alla dockershim. Tutto continua a funzionare xk docker supportato con containerd xk docker supporta imagespec e runtimespec
* containerd ora è diventato proj separato della CNCF (idealmente posso usarlo senza docker) - si interagisce con ctr, comando usato solo x debug, set limitato di funzionalità.
  * `ctr images pull <image>`
  * `ctr run <image> <image-name>`
* altro modo + comodo è nerdctl, una cli più simile alla cli docker, con molte più funzionalità su containerd (creata da community containerd). Supporta docker-compose, lazy pulling, P2P distrib, immagini criptate, namespace, firma e verifica immagini
  * `docker run --name redis redis:alpine` --> `nerdctl run --name redis redis:alpine`
* crictl - cli x accedere a CRI, creata dalla community kube e supporta ogni standard OCI. Non comoda, ma x scopi di debug. Interagisce con kubelet, anche pestandosi i piedi xk kubelet tende a eliminare ciò che viene creato al di fuori della sua visibilità
  * `crictl pull busybox`
  * `crictl images`
  * `crictl ps -a`
  * `crictl exec -i -t <container_id> ls`
  * `crictl logs <container_id>`
  * `crictl pods` (differenza rispetto a comandi docker-like: crictl è consapevole anche dei pod)
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
* `kubectl run --image nginx nginx` --> scarica immagine nginx da dockerhub (repo pubblica) e rilascia pod -- UTILE!
* `kubectl get pods` --> lista pod
* `kubectl describe pod <pod-name>` --> x avere info in dettaglio

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
* x ottenere yaml di un pod `kubectl get pod <pod-name> -o yaml > pod-definition.yaml`
* oppure x ottenere un manifest con tutti i pod basta `kubectl get pod -o yaml > pod-definition.yaml`
* poi conviene fare k delete pod --all
* x edit pod: `kubectl edit pod <property> <value>` ma solo alcune property sono editabili. Non il nome o le label x esempio

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
* `apiVersion: apps/v1` - altrimenti errore che dice unable to recognize... no matches for / kind=ReplicaSet
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
  * aggiornare il manifest yaml e lanciare `kubectl replace -f rs-definition.yml`
  * `kubectl scale --replicas=6 rs-definition.yml`
  * `kubectl scale --replicas=6 replicaset my-app-rs` # type e name del RS
  * con autoscaler ma + avanzato
* `kubectl delete replicaset <rs-name>`
* `kubectl edit rs <nome-risorsa>` mi apre vim sul manifest del file. Per orientarsi in vim:
  * `kubectl edit --help` ha un esempio di come impostare nano come editor
  * `:wq` salva il file ed esce, `i` serve a inserire inplace e esc serve a tornare alla command mode

### Deployment

* a che serve?
  * gestire versioni dei pod, gestire aggiornamenti facilmente
  * gestire rolling update (aggiorna a, poi b, poi c...)
  * gestire rollback (a->v1 b->v1 ...)
  * mettere in pausa o riavviare i cambiamenti
* incapsula P replica set che incapsula Q repliche di 1 pod che incapsula R container! Quindi è High-order object e può gestire molte cose
* e il manifest? Esattamente uguale al replicaset cambiando il kind: Deployment!!!! (sempre apps/v1, sempre spec.template.(metadata, spec), spec.replicas, spec.selector.matchLabels.(*:*))
* `kubectl get all` --> serve a vedere tutti gli oggetti creati
* `kubectl create deployment --name \<name> --replicas 3 --image \<image>` --> utile!
* `-o` specifica l'output del comando. le opzioni utili sono: `yaml|json|name` (solo i nomi) `|wide` (info estese)
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
* manifest

  ```yaml
  apiVersion: v1
  kind: Namespace
  metadata:
    name: my-namespace
  ```

* oppure `kubectl create namespace <ns-name>`
* contesto: il mio default in cui sono
* `kubectl config set-context $(kubectl config current-context) --namespace=dev`
* `kubectl get pods --all-namespaces` x vedere ogni cosa (oppure `-A`)
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

con `docker history` si vedono i vari layer e il peso di ciascuno in kB e MB

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
CMD accetta anche lista di parametri come `CMD ["sleep","5"]` ma non ~~`["sleep 5"]`~~

```dockerfile
FROM Ubuntu
ENTRYPOINT ["sleep"]
```

Usando `ENTRYPOINT` invece di CMD posso rendere parametrico il mio dockerfile xk specifico un programma da eseguire (es. `docker run ubuntu-sleeper 10`)

```dockerfile
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```

Questo da un default di 5 secondi ma con possibilità cm prima di parametrizzare
Si può fare override anche a runtime `docker run --entrypoint`

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
* `kubectl create configmap <my-config-map> --from-literal=APP_COLOR=blue --from-literal=APP_MODE=test`
* oppure `--from-file=<filepath>` xk usare cento from-literal è scomodo anche se si può usare `\` per spezzare com in + righe
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

* corrisponde a `docker run -e ENVIRONM=production` oppure prendi valore da config map o secret

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

* secret sono esattamente identici alle config map, sia i comandi che i manifest. Basterebbe fare un replace word-by-word. es. `envFrom.secretRef`, `volumes[0].secret.secretName`. NOTA: in create va specificato che vogliamo creare un secret generic (opaco)
* nota: i value dei secret dentro il campo data del manifest sono in base64, altrimenti stringData
* `echo -n "ciao beo" | base64` --> encode a string into base64
* `echo -n "Y2lhbyBiZW8=" | base64 --decode`
* sicurezza dei secret:
  * sono encoded non encrypted! -> non vanno committati con il codice
  * non sono encrypted in ETCD -> configurare [encryption at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data) x certe risorse
  * sono visibili da tutti in un namespace -> configurare access policy role based
  * si possono usare provider cloud per iniettare secret AWS, GCP, Azure. Oppure Helm Secret o HashiCorp Vault
  * si può usare [CSI driver](https://www.youtube.com/watch?v=MTnQW9MxnRI&ab_channel=KodeKloud) -- TODO guardare di che si tratta

---

### Encryption at rest

```sh
apt-get install etcd-client
netcdctl <...certificates> get my-super-secret | hexdump -C # it appears that secrets are not encrypted
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

* rispetto vm, i container nn sono isolati al 100% dall'host. condividono il kernel ma separano i processi usando i namespace
* dentro docker vedi 1 solo processo con pid 1 (namespace isolato) ma dall'host vedi i processi docker con pid diverso dall'altro namespace
* di default docker usa utente root sia nell'host che dentro il container
* se si vuole cambiare il default: `docker run --user=1000` (userid) o mettendo nel dockerfile `USER 1000`
* root dell'host è potentissimo. root del docker di default ha dei permessi in meno (es. nn può terminare processi di sistema o modificare networking)
* se si vuole fare override `docker run --cap-drop KILL` o `docker run --cap-add <permission>` o addirittura `--privileged`
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

> altro modo x vedere k utente esegue un pod è `k exec -i <pod_name> -- whoami`

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
  * `k create serviceaccount my-sa` # crea account, token nei secret
  * `k get sa`, `k describe sa`, `k get secret serviceaccount-token-xhsmc`
  * token usabile cm bearer nelle richieste rest `--header Authorization: Bearer <token:base64>`
  * il sa di default c'e in ogni ns (limitato): lo vedi col describe. viene montato di default su tti i pod (cosi possono accedere a kube api x es). x robe + potenti, es. modifica dei pod, va montato a manella
  * `k exec -it <pod> -- ls /var/run/secrets/kubernetes.io/serviceaccounts` # dentro token c'è il secret
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
* `kubectl taint nodes <node-name> key=value:<taint-effect>`
* key value come `app=blue`
* taint effect è cs succede se un po non tollera quel taint: `NoSchedule|PreferNoSchedule|NoExecute` (ultimo cn effetto retroattivo, fa eviction dei po esistenti)
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
* `k label node <node-name> key=value`
* dopo containers si aggiunge

  ```yaml
    containers:
    # ...
    nodeSelector:
      size: Large
  ```

utile ma limitato xk nn posso fare robe complesse cm dire k un pod va in medio o grande o negazione

con l'affinity si possono usare operatori complessi e comporre regole ma pagando in semplicità
operatori In, NotIn, Exist (matcha la chiave, conqualsiasivalore)
ma senon matcha nulla? dipende dal node affinity type

```yaml
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

* `requiredDuringSchedulingIgnoredDuringExecution` -> required in scheduling. Se nn c'è la label non schedula
* prefered... -> Se nn c'è la label schedula cmq
* required...required... -> se nn c'è la label o la rimuovo a runtime, non schedula o fa eviction del pod!

* quando usare taint e tolerations insieme? Nel caso in cui: voglio k i pod vadano su nodi specifici e k altri pod non vadano su quei nodi. es. pod R G B X Y e nodi r g b x y. voglio Rr Gg Bb e non mi interessa X e Y. Mi basta che non vadano Rg o Rx o Xg...

`alias kk=kubectl`
`export KUBECTL_EDITOR="nano"`

## pod multicontainer

* pod con + container xk è utile a volte k certe applicazioni abbiano microservizi comuni con stesse esigenze di scalabilità
* stesso spazio di networking (con localhost), stesso volume, stesso ciclo di vita/fate
* pattern
  * container co-locati - semplicemente 2 cont devono stare insieme, li creo insieme e muoiono insieme
  * regular init container - parte all-inizio e poi muore es. inizializzazione DB
    * viene impostato mettendo N container in initContainers (eseguiti insequenza). Spesso viene montato lo stesso volume dentro tutti questi pod, es busybox con `sh -c until nslookup myservice do echo waiting for myservice; sleep 2; done;`
    * finché l'init sta andando il main container è in waiting/pendingg
    * quando finisce, linit container va in completed e si avvia il passo successivo (altri init oppure main)
  * sidecar container - decido ordine di avvio, poi continua x tutta la vita del cont principale. es. logger
    * initContainers con un restartPolicy: Always. Spesso viene montato lo stesso volume dentro tutti questi pod
    l'importante è la restartPolicy

### Logging & Monitoring

* `docker logs -f <nome_container>`
* `k logs -f <pod_name>` ma, se ci sono più container fallisce
* `k logs -f <pod_name> \<container_name>`
* soluz di monitoraggio
  * open source
    * elasticstack
    * prometheus
  * proprietarie
    * datadog
    * dynatrace
    * groundcover
  * agli albori c'era heapster ma è deprecato
  * metrics server registra metriche in-memory e non su disco
  * cm funziona? ogni nodo ha kubelet k è un agente che runna i pod. Dentro kubelet c'è cAdvisor che preleva le metriche
  * su minikube: `minikube addons enable metrics-server`
  * oppure git clone della repo e `kubectl apply -f /deploy/<version>`
  * `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`
  * `kubectl top node`
  * `kubectl top pod`

### Raggruppamenti

* label e selector servono a fare raggruppamenti multipli

  ```yaml
  metadata:
    labels:
      app: app1
      functionality: xyz
      env: test
  ```

* ocio: Nei replica set ci sono le label del replica set e quelle deel template di pod.
* nel replica set c'è in spec il selector x identificare N pod. DEVE matchare con il template

  ```yaml
  spec:
    selector:
      matchLabels:
        app: app1
  ```

* service ha i selector x identiificare gruppo di pod
* annotations servono x altri scopi informativi o integrativi e sono dentro i metadati
* `k get po --selector=app=app1`
* `k get po --selector=bu=finance,env=prod,tier=frontend`
* `k get all --selector=app=app1`

### Strategie di Deploy

* posso fare `k rollout history deployment/myapp-deploy --revision=1` per vedere il dettaglio di una revisione (cosa cambia)
* quando creo un deployment parte un rollout. Ogni aggiornamento immagine avvia un rollout: sequenzialmente vengono aggiornati tutti i pod
* rolling update è la default deployment strategy, ma ce ne sono di alternative tipo la recreate dove tiro giù N pod e ne ricreo N (con downtime)
* oltre ad aggiornare manifest, si può usare comando imperativo `k set image deployment/myapp-deploy nignx-container=nginx:1.9.1`
* nel describe di un deployment si vedono tra gli eventi le strategie di deploy. Un recreate ha l'evento scale down to zero. Il rollout ha molti più eventi (tiro giù il pod, creo il pod x N volte)
* Nel rollout, il deployment crea un replica set B dove, man mano che uccide il replica set A pod dopo pod, crea un pod nel RS B
* `k rollout undo deployment/myapp-deployment` per fare rollback
* `k rollout status deployment/myapp-deploy`
* `k rollout history deployment/myapp-deploy` -> mi da l'elenco delle revisioni con un indice numerico di revisione
* la history mi da anche la causa del cambiamento. Ma di default non viene registrata
* per aggiungere la causa: quando si lancia `k set image` o `k edit deploy` va messo il flag `--record`
* per tornare alla versione X si fa `k rollout undo deployment/myapp-deploy --to-revision=X`
* BLUE/GREEN: switch dalla vecchia (blue) alla nuova solo quando tutti i test sulla nuova passano
* in Kube, si può attuare per esempio con i service e i selettori con tag es. version: v1 -> version: v2
* CANARY: faccio routing di una piccola percentuale di traffico verso la nuova vers del servizio. Quando sono confindente, aggiorno tutto il traffico
* Come farla? se ho 1 deployment con 5 po, creo l'altro deploy con 1 po. Uso una label comune così il 17% del traffico va alla nuova versione. Quando sono confidente, alzo il nuovo deploy a 5/5 e abbasso il vecchio a 0/5.
* **ISTIO** è un servizio di terze parti x fare service mesh e consente di gestire bene il routing in percentuale o meccanismi di deploy

### JOB

* quando servono: batch processing, analytics, reporting, manutenz periodica, ecc
* In docker `docker run ubuntu expr 1+2` - container si avvia, stampa e termina (con codice 0 - nessun errore)
* un modo per farlo in docker è con un pod, ma va messo RestartPolicy: Never altrimenti viene continuamente riavviato!
* Ma di norma si usa il job (analogo al replica set x parallelismo) che si avvia e arriva a completamento
* `k get job`, `k get pod`, `k delete job` -> elimina anche pod annessi
  
  ```yaml
  apiVersion: batch/v1
  type: Job
  metadata:
    name: myJob
  spec:
    completions: 3 # questo significa che vengono creati in sequenza 3 pod, avviato 1, completato 1, avviato 2, completato 2... JOB concluso. Se però fallisce può creare N esecuzioni finché non ottiene 3 completamenti. FAIL FAIL FAIL SUCCESS FAIL SUCCESS FAIL FAIL FAIL FAIL FAIL SUCCESS
    parallelism: 3 # in combinata con completions, definisce il parallelismo con cui crea i JOB. Se c'è un fail, ritenta con parallelismo minore es. SUCCESS+SUCCESS+FAIL FAIL SUCCESS
    
    template:
      spec: # the same as pod
        containers:
        - name: math-add
          image: ubuntu
          command: ['expr','3','+','2']
        restartPolicy: Never
  ```

* Cronjob serve x schedulare job invece che avviarli istantaneamente
  
  ```yaml
  apiVersion: batch/v1beta1
  type: CronJob
  metadata:
    name: myJob
  spec:
    schedule: "*/1 * * * *"
    jobTemplate:
      spec:
        completions: 3 # questo significa che vengono creati in sequenza 3 pod, avviato 1, completato 1, avviato 2, completato 2... JOB concluso. Se però fallisce può creare N esecuzioni finché non ottiene 3 completamenti. FAIL FAIL FAIL SUCCESS FAIL SUCCESS FAIL FAIL FAIL FAIL FAIL SUCCESS
        parallelism: 3 # in combinata con completions, definisce il parallelismo con cui crea i JOB. Se c'è un fail, ritenta con parallelismo minore es. SUCCESS+SUCCESS+FAIL FAIL SUCCESS
        
        template:
          spec: # the same as pod
            containers:
            - name: math-add
              image: ubuntu
              command: ['expr','3','+','2']
            restartPolicy: Never
  ```

  * La cron expression è fatta così: minuti(0-59) ore(0-23) giornomese(1-31) mese(1-12) giornosett(0domenica-6sabato)
  * ocio k diventa complesso: 3 spec (cronjob > job > pod)
  * i Job che falliscono si riavviano con exponential backoff (attesa incrementale) 10s - 20s - 40s - 1m 20s - 2m 40s - 5m 20s - 6m - 6m - ...
  * il job che ritenta 6 volte fallendo, dopo viene considerato "fallito". Questo è modificabile con il parametro backoffLimit
  * ttlSecondsAfterFinished: 100 per rendere log accessibili
  * `k cronjob create \<nomecronjob> --image=\<imagename> --schedule="30 21 * * *"` -> crea il job x le 21.30 di ogni giorno

## Servizi

* un service serve a garantire il disaccoppiamento tra microserv, e alla raggiungibilità
* il tipo NodePort funge da port remap per esporre un servizio dal di fuori del nodo
* ClusterIP serve x scopi interni di raggiungibilità di servizi
* LoadBalancer serve per esporre bilanciando il carico
* `k get svc` / `k describe svc <service-name>`

### NodePort

* il service ha concetti di porte nell'ordine:
  * La **target port** è quella di destinazione (cioè quella del pod)
  * La **port** è quella interna del node con cui si interfaccia verso i pod
  * La **node port** la sorgente di traffico (quella esposta esternamente dal service/node) - può andare da 30000 a 32767
* Segue manifest

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: my-svc
  spec:
    type: NodePort
    ports:
      - targetPort: 80
        port: 80
        nodePort: 30008
    selector:
      app: my-app
      tier: frontend
  ```

* Campo ports: una LISTA di mapping di porte
  * port è obbligatorio
  * Se non metto targetPort, il default è identico a port
  * Se non metto nodePort, il default è una porta libera tra 30000 e 32767
* dal selector seleziono varie label a cui corrispondono un insieme di pod
  * se ci sono + pod selezionati, viene fatto load balancing con algor random by design e session affinity
  * se i pod selezionati sono su + nodi, viene creato un service trasversale ai nodi, così che qualsiasi sia l'IP del nodo utilizzato, viene sempre raggiunto il service

### ClusterIP

* Tipo di default del service (non servirebbe nemmeno specificarlo)
* Serve alla raggiungibilità interna dei pod (es. FE to BE)
* Segue manifest

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: my-svc
  spec:
    type: ClusterIP
    ports:
      - targetPort: 80
        port: 80
    selector:
      app: my-app
      tier: backend
  ```

## NetworkPolicies

* di default i nodi, i pod, i servizi: si vedono tra loro. Sono in una rete privata dove si raggiungono e si può fare qualsiasi connessione di traffico entrante o uscente con regola AllowAll
* NetworkPolicy: oggetto Kube (non core) che fa sì che solo certi routing siano consentiti (es. DB raggiungibile solo da API server)
* `k get netpol`
* Segue manifest

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: db-policy-to-accept-only-incoming-api-traffic
  spec:
    podSelector:
      matchLabels:
        app: db
    policyTypes:
    - Ingress
    ingress:
    - from:
      - podSelector:
        matchLabels:
          app: api
      ports:
      - protocol: TCP
        port: 3306
  ```

* policyTypes: se c'è ingress, solo il traffico entrante è isolato. Quello uscente è sempre consentito
* non su tutti i cluster sono supportate le NetPolicy (ocio k nn mostra errore, ma proprio nn funziona)
  * NON su FLANNEL
  * SI' su KubeRouter Calico Romana WeaveNet
* oltre a podSelector posso avere regole sul namespace (una regola composta è valutata in AND)
  
  ```yaml
  - from:
    - podSelector:
        matchLabels:
          app: api
      namespaceSelector:
        matchLabels:
          name: prod
  ```

* in from posso avere + regole (valutate in OR) quando c'è un array. quando è un oggetto sono in AND
* es. dal db voglio avere accesso da degli IP per fare il backup:

  ```yaml
  - from:
    - podSelector:
        matchLabels:
          app: api
      namespaceSelector:
        matchLabels:
          name: prod
    - ipBlock:
        cidr: 192.168.0.15/32
  ```

* es. per egress:

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: db-policy-to-accept-only-incoming-api-traffic
  spec:
    podSelector:
      matchLabels:
        app: db
    policyTypes:
    - Ingress
    - Egress
    ingress:
    - from:
      - podSelector:
        matchLabels:
          app: api
      ports:
      - protocol: TCP
        port: 3306
    egress:
    - to:
      # the same... rules for egress
    - to:
      # the same... rules for egress IN OR
  ```

## Ingress

* Il service di tipo nodeport al max mi da un url del genere: `http://11.24.33.105:33080` mentre ne vorrei uno come `https://my-site.com`.
* In ambienti cloud posso usare load balancer (a pagamento) per esporre un servizio
* Posso creare un ingress k è come un reverse proxy k fa:
  * mapping di porte (es. 80->33080)
  * SSL
  * routing tra vari nodi, basato su path, virtual hosting, ecc
* `k get ingress`
* senza Ingress si userebbe nginx, HAProxy, Traefik, Contour, Istio, Envoy come reverse proxy, ma vanno configurati
* con ingress, si usa un reverse proxy (IngressController) e si configurano delle regole (IngressResource)
* `kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"`
* se nn viene specificato un backend viene usato default-backend-service
* manifest IngressController

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Deployment
  metadata:
    name: my-ingr-cont
  spec:
    containers:
      - image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
        name: ingress-controller
    args:
      - /nginx-ingress-controller
      - -- configmap=$(POD_NAMESPACE)/nginx-configuration # passo la config map x nginx
    env:
      - name: POD_NAME
        valueFrom:
          fieldRef:
            fieldPath: metadata.name
      - name: POD_NAMESPACE
        valueFrom:
          fieldRef:
            fieldPath: metadata.namespace
    ports:
      - name: http
        containerPort: 80
      - name: https
        containerPort: 443

  ---
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: nginx-configuration
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-ingress
  spec:
    type: NodePort
    ports:
      - name: http
        protocol: TCP
        targetPort: 80
        port: 80
      - name: https
        protocol: TCP
        targetPort: 443
        port: 443
    selector:
      name: nginx-ingress
  ---
  apiVersion: v1
  kind: ServiceAccount # serve all'ingress controller x fare cose intelligenti
  metadata:
    name: nginx-ingress-serviceaccount
  ```

* di norma l'ingress controller viene deployato nel namespace `ingress-nginx`
* routing semplice (pass through nessuna regola):

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: my-simple-routing
  spec:
    backend:
      service:
        name: wear-service
        port:
          number: 80
  ```

* routing basato su path

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: my-routing
  spec:
    rules:
      - http:
          paths:
            - path: /wear
              backend:
                service:
                  name: wear-service
                  port:
                    number: 80
            - path: /watch
              backend:
                service:
                  name: watch-service
                  port:
                    number: 80
  ```

* routing basato su host

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: my-routing
  spec:
    rules:
      - host: wear.my-site.com
        http:
          paths:
            - backend:
                service:
                  name: wear-service
                  port:
                    number: 80
      - host: watch.my-site.com
        http:
          paths:
            - backend:
                service:
                  name: watch-service
                  port:
                    number: 80
  ```

* `nginx.ingress.kubernetes.io/rewrite-target: /` serve a dire che se accedo a un ingress con `/path`, questo rimappa nel servizio con `/` invece che con `/path`
* nel comando imperativo ricorda di mettere `--annotation a=b`
