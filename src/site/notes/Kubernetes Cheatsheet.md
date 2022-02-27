---
permalink: kubernetes
---
# Kubernetes Cheatsheet
## Kubernetes Cluster
Kubernetes koordinerer et cluster av datamaskiner som er koplet sammen til � jobbe som en enkel enhet. Det er alts� et abstraheringslag over VMer. Et kubernetes-kluster er det man kaller en instanse av Kubernetes. Klusteret sin oppgave er � automatisere applikasjons (docker) containere effektivt. 
Et kluster best�r av:
* Control Plane
* Noder

## Control plane oppgaver
* Koorinderer kluseteret
* Holder �nsket state til applikasjonen
* Skalerer
* Ruller ut oppdateringer
* Stedet der "Deployments" ligger

## Node
* Arbeiderne som kj�rer applikasjonen
* Er en VM eller en fysisk maskin
* En node kan ha flere applikasjoner
* Hver node har en Kubelet som er en agent som snakker med Control Plane. 
* Kommuniserer med Control Plane via Kubernetes API. Dette APIet blir eksponert av control plane. Som bruker kan man interagere med dette APIet direkte.
* Man b�r ha minst 3 noder fordi om 1 node g�r ned s� vil b�de en etcd og en control plane instanse mistes og man har ikke lenger redundancy (forst� ikke hvorfor de sier dette? Er control plane egentlig bare enda en node?)
* En node kan ha flere pods.
* Hver node kj�rer minium to ting:
	* Kubelet, en prosess som har ansvar for � kommunisere med control plane og noden. Den manager Podsene som kj�rer p� en maskin
	* Et container runtime (som Docker) som er ansvarlig for � pulle bilder, og kj�re applikasjoner. 
![](https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg)

## Kubrenetes Deployments
For � deploye en (dockerized) applikasjon i et kluster s� m� man lage en "Deployment configuration". Denne forteller Kubernetes hvordan man lager og oppdaterer instanser av appen. N�r det er laget vil en Kubernetes Deployment Controller kontinuerlig overv�ke instansene til denne appen. Dersom en Node g�r ned vil denne kontrolleren erstatte instansen med en instanse p� en annen node i klusteret. Controlleren lever i Control Plane. 
Forskjellen fra vanlige startup script er alts� at i tillegg til � starte apper s� holder Kubernetes Deployments dem oppe p� tvers av noder. 

## Pod
[Dokumentasjon](https://kubernetes.io/docs/concepts/workloads/pods/)
Pods er minste mulige computing enhet man kan deploye i Kubernetes. (Kan kalles Kubernetes' atomiske enhet)
Det er en grupper med 1 eller flere containere, og disse deler lagringsplass og nettverksressurser. Tanken er at den skal modellere en applikasjons-spesifikk "logisk host". Applikasjons containerene som befinner seg i en pod skal v�re tightly coupled (Se [[Coupling]]). De deler IP-adresse og port-space og kj�rer alltid i en delt kontekst p� samme node. 
Slik ser en konfigurasjonsfil for � kj�re en pod med 1 docker container ut:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```

Eksempel p� en Pod med flere instanser er en Pod som inneholder en Node.js app s�vel som en annen container som feeder dataene som Node.js webserveren skal publisere. 
Ofte vil man bare kj�re en instanse per pod. De skal bare kj�re i samme dersom de er n�dt til � dele ressurser slik som samme diskplass. 

![](https://d33wubrfki0l68.cloudfront.net/fe03f68d8ede9815184852ca2a4fd30325e5d15a/98064/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)

## Nettverk
Pods som kj�rer inne i Kubernetes kj�rer p� et privat isolert nettverk. Default-oppf�rselen er at de kan sees fra ande pods og tjenester i samme kluster, men ikke fra utsiden av dette nettverket. For � kommunisere med applikasjonen v�r m� vi alts� ta i bruk kubectl som igjen bruker et eksponert API endepunkt. 
Vha. kubectl kan man lage en proxy som forwarder kommunikasjon inn til det private nettverket for � kunne snakke med APIet direkte. 

## Service
Servicer er en abstrahering som definerer et logisk sett med Pods og en policy for � kunne n� dem. Man trenger alts� en Service (som alts� bare er et sett med nettverksregler) for � la pods motta trafikk fra utsiden av kubernetes klusteret. Man definerer en tjeneste med en YAML (eller JSON) fil. 
Det finnes ulike m�ter � eksponere en Servie p�. Disse velger man ved � spesifisere "type" i ServiceSpec.
* ClusterIP (Default)
	* Servicen blir eksponer p� en intern IP i clusteret. Denne tjenesten kan kun n�s inne i klusteret
* NodePort
	* Eksponerer en Service p� samme port p� alle de valgte Nodene i klusteret ved � bruke [[NAT]]. Servicen kan da n�s fra utsiden av clustered ved � bruke `<NodeIP>:<NodePort>`.  Man n�r alts� Servicen ved � g� via en Node og en spesiell port, som da proxyer videre inn til servicen. Supersett av ClusterIP, s� den har fortsatt egen intern IP, men kan alts� ogs� n�s via Nodes om man har satt opp disse til � ha ekstern IP adresse f.eks. Da kan man sette opp sin egen loadbalancer konfigurasjon ved � proxye gjennom en node.
* LoadBalancer
	* Lager en extern [[Load Balancer]] i clouden man er i (om det er st�ttet), og gir en fixed extern IP to tjenesten. Supertset av NodePort. Det er alts� en ferdig implementert LoadBalancer som bruker NodePort implementasjonen for � eksponere seg ut fra det interne klusternettverket.
* Externalname
	* Lager et CNAME record for � kunne n� noe utenfor clusteret. F.eks. kan man mappe en tjeneste (som f�r en lokal adresse i clusteret), til et eksternt navn. Dette skjer p� DNS niv�, og ikke proxy niv�. F.eks. vil en Service definition for � n� en database p� utsiden se slik ut:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: prod
spec:
  type: ExternalName
  externalName: my.database.example.com
```
* Da vil my-service.prod.svc.cluster.local returnere en CNAME record med verdien my.database.example.com.

### Labels og selector
Servier er som sagt en abstrahering over et sett med Pods. M�ten man matcher et POds sett p� er ved � bruke labels og selectors. Det er en primitiv som lar deg gj�re logiske operasjoner p� Kubernetes objekter. Labels er key-value pairs (app=myappname) som er knyttet til objekter. 
Man kan bruke det til � lage egne objekter for dev, test og prod. Man kan lage versjonerings tags, eller man kan klassifisere objekter med tag. Selector er "operasjonen" av � velge objekter basert p� labels. 
![](https://d33wubrfki0l68.cloudfront.net/7a13fe12acc9ea0728460c482c67e0eb31ff5303/2c8a7/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)


## Ingress
Ingress er ikke det samme som en service, men en m�te � eksponere en Service p�. Den fungerer som et entry point for clusteret ditt. Den kan eksponere flere Serivcer under samme IP-adresse. F.eks. kan man lage en NGINX ingress som igjen kan mappe forskjellige hostnames til forskjellige Servicer. 

# Deployment og Scaling
En deployment lever i control plane delen av kubernetes. Den deployer en logisk applikasjon. F.eks. vil en webserver bare ha en deployment med �n pod. Men n�r trafikken �ker m� vi skalere appen v�r.
Scaling skjer ved � endre antall replicas i en Deployment.
N�r det skjer en Scaling av en Deployment s� vli nye Pods bli opprettet til Noder som har ledige ressurser. Podsene kan v�re p� forskjellige Noder, eller samme Node, eller begge deler. Man kan scale b�de manuelt og automatisk. 
I tillegg, n�r man har flere instanser av en application, s� kan man rulle ut oppdateringer uten downtime. Servicer vil automatisk distribuere trafikken mellom alle Pods av en eksponert Deployment. 

### Replica sets
En deployment vil lage et ReplicaSet. Dette sier noe om hvor mange instanser vi �nsker av deploymenten v�r. 

### Rolling updates
Man kan oppdatere Deployments fortl�pende med zero downtime ved � inkrementerende erstatte pod instanser med nye. Man lager alts� nye pods med oppdatert kode, f�r man litt etter litt endrer servicen til � peke p� de nye Podsene (som n� har nye IP-adresser siden de er nye Pods), f�r de gamle podsene termineres. Hver update f�r en versjon og etter man har deployet kan man reversere tilbake til en tidligere versjon dersom noe gikk galt.


# Verkt�y


## Minikube
En lightweight Kubernetest implementasjon. Lager en VM p� lokalmaskinen og deployer et cluster som inneholder 1 node. Kommer med et CLI for � jobbe med clusteret. 

### Cheatsheet

**Start cluster**
```bash
minikube start
```




## Kubectl
Cli for � snakke med Kubernetes. Bruker Kubernetes API for � interagere med klusteret. 
Formatet er vanligvis: `kubectl action resource.` Man kan bruke `--help` bak kommandoer. 

### Cheatsheet

**Se detaljer om kluseret**
```bash
kubectl cluster-info
```

**List ressurser**
```bash
kubectl get <resource-type>
# ex resource-type => pods, deployments, services
```

**Viser detaljer informasjon om en ressurs**
```bash
kubectl describe <resource-type>
# ex resource-type => pods, services
```
Describe kommandoen skal printe i menneske-leselig format, og b�r ikke brukes i scripting sammenheng.

**Vis detaljer om spesifikk service**
```bash
kubectl describe services/kubernetes-bootcamp
```

**Printer loggene fra en kontainer i en pod**
```bash
kubectl logs <resource>
# ex resource => $POD_NAME
```

**Eksekverer en kommando p� en container i en pod**
```bash
kubectl exec $POD_NAME -- <command>
#ex command => env, bash
```
Om man har flere containere i en pod m� man ogs� spesifisere dette

**Starte et interaktivt bash shell**
```bash
kubectl exec -ti $POD_NAME -- bash
```

**Se nodene i kluseteret**
```bash
kubectl get nodes
```

**List deployments**
```bash
kubectl get deployments
```

**List pods**
```bash
kubectl get pods
```

**List replicasts**
```bash
kubectl get rs
```


**Lag ny deployment**
```bash
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```
Gir navnet "kubernetes-bootcamp" til deployment og bruker det gitte dockerbildet. Finner en node som passer, starter appen p� noden og konfugurerer klusteret til � starte instansen p� ny node n�r det trengs

**Eksponer en deployment til VM IPen**
```bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```
Med denne kommandoen kan man n� containeren knyttet til deploymenten sin 8080 port vha
```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}') echo NODE_PORT=$NODE_PORT
```
`curl $VM_IP:$NODE_PORT`
(Merk at NODE_PORT ikke er 8080. Dette er en randomly assigned port p� VMen som proxyer videre til port 8080 hos containeren).

**Scale en deployment til 4 instanser/pods**
```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

**Rull ut oppdatering p� deployment med nytt docker bilde**
```bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```

**Sjekk status av utrulling til deployment**
```bash
kubectl rollout status deployments/kubernetes-bootcamp
```

**Rull tilbake**
```bash
kubectl rollout undo deployments/kubernetes-bootcamp
```

**Select pods based on labels**
```bash
kubectl get pods -l app=kubernetes-bootcamp
```
Labelet app=appnavn blir laget automatisk ved deploy av en app. Man kan ogs� gj�r samme selector p� en Service som er knyttet til deployen

**Legg til nytt label p� en pod**
```bash
kubectl label pods $POD_NAME version=v1
```

Poden vil da f� en ny label: version=v1. Kan da hentes ut med
```bash
kubectl get pods -l version=v1
```

**Slett tjeneste baser p� label**
```bash
kubectl delete service -l app=kubernetes-bootcamp
```



**Start proxy for Kubernetes APIet inn til kluster-nettverket**
```bash
kubectl proxy
```
Stoppes med control-C, og vil ikke vise noe output n�r den kj�rer. 



## Kubernetes API

**Versjonsinfo**
```bash
curl http://localhost:8001/version
```

**Hent pod metadata og informasjon** 
```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/
```

**Snakke med APIet til applikasjonen**
```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```