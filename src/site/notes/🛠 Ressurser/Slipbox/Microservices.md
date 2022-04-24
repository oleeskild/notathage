---
{"dg-publish":true,"permalink":"/ressurser/slipbox/microservices/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Microservices
> A **microservice** architecture – a variant of the [service-oriented architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture "Service-oriented architecture") (SOA) structural style – arranges an application as a collection of [loosely-coupled](https://en.wikipedia.org/wiki/Loose_coupling "Loose coupling") services. In a microservices architecture, services are fine-grained and the protocols are [lightweight](https://en.wikipedia.org/wiki/Lightweight_protocol "Lightweight protocol"). The goal is that teams can bring their services life independent of others. Loose coupling reduces all types of dependencies and the complexities around it, as service developers do not need to care about the users of the service, they do not force their changes onto users of the service.
Kilde: [Wikipedia](https://en.wikipedia.org/wiki/Microservices)

Mikrotjenester er en **type** tjeneste-orientert arkitektur. På samme måte som Scrum er en stil eller type innen [[🛠 Ressurser/Slipbox/Agile Metoder|Agile Metoder]] så er mikrotjenester en stil innen tjenseste-orientert arkitektur. 

## Kjennetegn

Tjenestene er avhengige av hverandre på kryss og tvers. Selv om bare en tjeneste går ned, vil dette ofte ha en effekt på mange andre tjenester som bruker denne.

### Fordeler
Uavhengige deploys. Dette er blant de største fordelene med mikrotjenester.

[[🛠 Ressurser/Slipbox/Mikrotjenester klarifiserer strukturen til et informasjonssystem|Mikrotjenester klarifiserer strukturen til et informasjonssystem]]
[[🛠 Ressurser/Slipbox/Mikrotjenester kjøper deg muligheter|Mikrotjenester kjøper deg muligheter]]
[[🛠 Ressurser/Slipbox/Mikrotjenester legger til rette for autonome team|Mikrotjenester legger til rette for autonome team]]

### Ulemper
Distribuerte systemer er vanskelige 
> “A _distributed system_ is one in which the failure of a _computer_ you didn't even _know_ existed can render your own _computer_ unusable.” — Leslie Lamport.

[[🛠 Ressurser/Slipbox/Monitorering av mikrotjenester er komplekst|Monitorering av mikrotjenester er komplekst]]
[[🛠 Ressurser/Slipbox/Ved sikkerhetshull må alle mikrotjenester patches|Ved sikkerhetshull må alle mikrotjenester patches]]

## Relaterte notater 
[[🛠 Ressurser/Slipbox/Sagas|Sagas]]
[[🛠 Ressurser/Slipbox/Logging i mikrotjenester bør aggregeres|Logging i mikrotjenester bør aggregeres]]
[[🛠 Ressurser/Slipbox/Mikrotjenester bør versjoneres semantisk|Mikrotjenester bør versjoneres semantisk]]
[[🛠 Ressurser/Slipbox/Endepunkter bør versjoneres i header|Endepunkter bør versjoneres i header]]


## Notater fra talks
[[🛠 Ressurser/Slipbox/Information Patterns in Microservices|Information Patterns in Microservices]]
[[📥 Inbox/Microservices Workshop|Microservices Workshop]]
