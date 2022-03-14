---
{"dg-publish":true,"permalink":"/ressurser/snippets/kubernetes-cheatsheet/"}
---

# Kubernetes Cheatsheet
## Kubernetes Cluster
Kubernetes koordinerer et cluster av datamaskiner som er koplet sammen til å jobbe som en enkel enhet. Det er altså et abstraheringslag over VMer. Et kubernetes-kluster er det man kaller en instanse av Kubernetes. Klusteret sin oppgave er å automatisere applikasjons (docker) containere effektivt. 
Et kluster består av:
* Control Plane
* Noder

## Control plane oppgaver
* Koorinderer kluseteret
* Holder ønsket state til applikasjonen
* Skalerer
* Ruller ut oppdateringer
* Stedet der "Deployments" ligger

## Node
* Arbeiderne som kjører applikasjonen
* Er en VM eller en fysisk maskin
* En node kan ha flere applikasjoner
* Hver node har en Kubelet som er en agent som snakker med Control Plane. 
* Kommuniserer med Control Plane via Kubernetes API. Dette APIet blir eksponert av control plane. Som bruker kan man interagere med dette APIet direkte.
* Man bør ha minst 3 noder fordi om 1 node går ned så vil både en etcd og en control plane instanse mistes og man har ikke lenger redundancy (forstå ikke hvorfor de sier dette? Er control plane egentlig bare enda en node?)
* En node kan ha flere pods.
* Hver node kjører minium to ting:
	* Kubelet, en prosess som har ansvar for å kommunisere med control plane og noden. Den manager Podsene som kjører på en maskin
	* Et container runtime (som Docker) som er ansvarlig for å pulle bilder, og kjøre applikasjoner. 
![](https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg)

## Kubrenetes Deployments
For å deploye en (dockerized) applikasjon i et kluster så må man lage en "Deployment configuration". Denne forteller Kubernetes hvordan man lager og oppdaterer instanser av appen. Når det er laget vil en Kubernetes Deployment Controller kontinuerlig overvåke instansene til denne appen. Dersom en Node går ned vil denne kontrolleren erstatte instansen med en instanse på en annen node i klusteret. Controlleren lever i Control Plane. 
Forskjellen fra vanlige startup script er altså at i tillegg til å starte apper så holder Kubernetes Deployments dem oppe på tvers av noder. 

## Pod
[Dokumentasjon](https://kubernetes.io/docs/concepts/workloads/pods/)
Pods er minste mulige computing enhet man kan deploye i Kubernetes. (Kan kalles Kubernetes' atomiske enhet)
Det er en grupper med 1 eller flere containere, og disse deler lagringsplass og nettverksressurser. Tanken er at den skal modellere en applikasjons-spesifikk "logisk host". Applikasjons containerene som befinner seg i en pod skal være tightly coupled (Se [[🛠 Ressurser/Slipbox/Coupling|Coupling]]). De deler IP-adresse og port-space og kjører alltid i en delt kontekst på samme node. 
Slik ser en konfigurasjonsfil for å kjøre en pod med 1 docker container ut:
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

Eksempel på en Pod med flere instanser er en Pod som inneholder en Node.js app såvel som en annen container som feeder dataene som Node.js webserveren skal publisere. 
Ofte vil man bare kjøre en instanse per pod. De skal bare kjøre i samme dersom de er nødt til å dele ressurser slik som samme diskplass. 

![](https://d33wubrfki0l68.cloudfront.net/fe03f68d8ede9815184852ca2a4fd30325e5d15a/98064/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)

## Nettverk
Pods som kjører inne i Kubernetes kjører på et privat isolert nettverk. Default-oppførselen er at de kan sees fra ande pods og tjenester i samme kluster, men ikke fra utsiden av dette nettverket. For å kommunisere med applikasjonen vår må vi altså ta i bruk kubectl som igjen bruker et eksponert API endepunkt. 
Vha. kubectl kan man lage en proxy som forwarder kommunikasjon inn til det private nettverket for å kunne snakke med APIet direkte. 

## Service
Servicer er en abstrahering som definerer et logisk sett med Pods og en policy for å kunne nå dem. Man trenger altså en Service (som altså bare er et sett med nettverksregler) for å la pods motta trafikk fra utsiden av kubernetes klusteret. Man definerer en tjeneste med en YAML (eller JSON) fil. 
Det finnes ulike måter å eksponere en Servie på. Disse velger man ved å spesifisere "type" i ServiceSpec.
* ClusterIP (Default)
	* Servicen blir eksponer på en intern IP i clusteret. Denne tjenesten kan kun nås inne i klusteret
* NodePort
	* Eksponerer en Service på samme port på alle de valgte Nodene i klusteret ved å bruke [[NAT]]. Servicen kan da nås fra utsiden av clustered ved å bruke `<NodeIP>:<NodePort>`.  Man når altså Servicen ved å gå via en Node og en spesiell port, som da proxyer videre inn til servicen. Supersett av ClusterIP, så den har fortsatt egen intern IP, men kan altså også nås via Nodes om man har satt opp disse til å ha ekstern IP adresse f.eks. Da kan man sette opp sin egen loadbalancer konfigurasjon ved å proxye gjennom en node.
* LoadBalancer
	* Lager en extern [[Load Balancer]] i clouden man er i (om det er støttet), og gir en fixed extern IP to tjenesten. Supertset av NodePort. Det er altså en ferdig implementert LoadBalancer som bruker NodePort implementasjonen for å eksponere seg ut fra det interne klusternettverket.
* Externalname
	* Lager et CNAME record for å kunne nå noe utenfor clusteret. F.eks. kan man mappe en tjeneste (som får en lokal adresse i clusteret), til et eksternt navn. Dette skjer på DNS nivå, og ikke proxy nivå. F.eks. vil en Service definition for å nå en database på utsiden se slik ut:

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
Servier er som sagt en abstrahering over et sett med Pods. Måten man matcher et POds sett på er ved å bruke labels og selectors. Det er en primitiv som lar deg gjøre logiske operasjoner på Kubernetes objekter. Labels er key-value pairs (app=myappname) som er knyttet til objekter. 
Man kan bruke det til å lage egne objekter for dev, test og prod. Man kan lage versjonerings tags, eller man kan klassifisere objekter med tag. Selector er "operasjonen" av å velge objekter basert på labels. 
![](https://d33wubrfki0l68.cloudfront.net/7a13fe12acc9ea0728460c482c67e0eb31ff5303/2c8a7/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)


## Ingress
Ingress er ikke det samme som en service, men en måte å eksponere en Service på. Den fungerer som et entry point for clusteret ditt. Den kan eksponere flere Serivcer under samme IP-adresse. F.eks. kan man lage en NGINX ingress som igjen kan mappe forskjellige hostnames til forskjellige Servicer. 

# Deployment og Scaling
En deployment lever i control plane delen av kubernetes. Den deployer en logisk applikasjon. F.eks. vil en webserver bare ha en deployment med én pod. Men når trafikken øker må vi skalere appen vår.
Scaling skjer ved å endre antall replicas i en Deployment.
Når det skjer en Scaling av en Deployment så vli nye Pods bli opprettet til Noder som har ledige ressurser. Podsene kan være på forskjellige Noder, eller samme Node, eller begge deler. Man kan scale både manuelt og automatisk. 
I tillegg, når man har flere instanser av en application, så kan man rulle ut oppdateringer uten downtime. Servicer vil automatisk distribuere trafikken mellom alle Pods av en eksponert Deployment. 

### Replica sets
En deployment vil lage et ReplicaSet. Dette sier noe om hvor mange instanser vi ønsker av deploymenten vår. 

### Rolling updates
Man kan oppdatere Deployments fortløpende med zero downtime ved å inkrementerende erstatte pod instanser med nye. Man lager altså nye pods med oppdatert kode, før man litt etter litt endrer servicen til å peke på de nye Podsene (som nå har nye IP-adresser siden de er nye Pods), før de gamle podsene termineres. Hver update får en versjon og etter man har deployet kan man reversere tilbake til en tidligere versjon dersom noe gikk galt.


# Verktøy


## Minikube
En lightweight Kubernetest implementasjon. Lager en VM på lokalmaskinen og deployer et cluster som inneholder 1 node. Kommer med et CLI for å jobbe med clusteret. 

### Cheatsheet

**Start cluster**
```bash
minikube start
```




## Kubectl
Cli for å snakke med Kubernetes. Bruker Kubernetes API for å interagere med klusteret. 
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
Describe kommandoen skal printe i menneske-leselig format, og bør ikke brukes i scripting sammenheng.

**Vis detaljer om spesifikk service**
```bash
kubectl describe services/kubernetes-bootcamp
```

**Printer loggene fra en kontainer i en pod**
```bash
kubectl logs <resource>
# ex resource => $POD_NAME
```

**Eksekverer en kommando på en container i en pod**
```bash
kubectl exec $POD_NAME -- <command>
#ex command => env, bash
```
Om man har flere containere i en pod må man også spesifisere dette

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
Gir navnet "kubernetes-bootcamp" til deployment og bruker det gitte dockerbildet. Finner en node som passer, starter appen på noden og konfugurerer klusteret til å starte instansen på ny node når det trengs

**Eksponer en deployment til VM IPen**
```bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

(Merk at NODE_PORT ikke er 8080. Dette er en randomly assigned port på VMen som proxyer videre til port 8080 hos containeren).

**Scale en deployment til 4 instanser/pods**
```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

**Rull ut oppdatering på deployment med nytt docker bilde**
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
Labelet app=appnavn blir laget automatisk ved deploy av en app. Man kan også gjør samme selector på en Service som er knyttet til deployen

**Legg til nytt label på en pod**
```bash
kubectl label pods $POD_NAME version=v1
```

Poden vil da få en ny label: version=v1. Kan da hentes ut med
```bash
kubectl get pods -l version=v1
```

**Slett tjeneste baser på label**
```bash
kubectl delete service -l app=kubernetes-bootcamp
```



**Start proxy for Kubernetes APIet inn til kluster-nettverket**
```bash
kubectl proxy
```
Stoppes med control-C, og vil ikke vise noe output når den kjører. 



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