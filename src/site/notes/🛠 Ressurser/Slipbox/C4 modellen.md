---
{"dg-publish":true,"permalink":"/ressurser/slipbox/c4-modellen/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# C4 modellen
Et rammeverk for å lage arkitektur digrammer, enten for å designe et nytt system eller dokumentere et eksisterende. De 4 Cene er:
* System Context
* Container
* Component
* Code

Modellen i seg selv legger ikke noen føringer for notasjon. Det er først og fremst de fire nevnte abstraksjonene i tillegg til fire komplementerende diagrammer for hver abstraksjon. Som regel holder det å lage diagrammer for de to første nivåene. Spesielt det siste, code, er overflødig og kan heller ved behov bli autogenerert av IDEen man bruker. 

## System context
Diagrammene starter med et overblikk over systemet ditt, hvilke eksterne tjenester som bruker det, og hvilke brukere det har. Eksempelvis:
![](https://c4model.com/img/bigbankplc-SystemContext.png)

## Container
Deretter kan man zoome inn på systemet og vise hvilke "containere" som finnes. Må ikke forvirres med docker-container. En container i C4 kontekst er en applikasjon eller en data store. 
Eksempelvis:
![](https://c4model.com/img/bigbankplc-Containers.png)

## Components
Om man igjen zoomer inn på én av disse firkantene så kan man visualisere "komponentene" i containeren. Dette vil typisk være tydelige abstraksjoner eller interfaces i kodebasen.
Eksempelvis:
![](https://c4model.com/img/bigbankplc-Components.png)

## Code
Dette er som regel lite vits i å lage. Det kan også bli autogenerert av IDEen man bruker ved behov.
Til tross for å være "enterprisey" og overkill kan faktisk UML være en god måte å representere dette laget på. 
Eksempelvis:
![](https://c4model.com/img/bigbankplc-Classes.png)



## Verktøy
[[🛠 Ressurser/Personer/Simon Brown|Simon Brown]] anbefaler ikke tradisjonelle verktøy som Visio, Lucidchart osv. De er for general purpose, og kan ofte være overkill for å lage C4 diagrammer. De er også vanskeligere å refaktorere og holde oppdatert over tid.

Han har derimot laget sitt eget verktøy kalt Structurizr. Dette kommer også med sitt eget [[Domain Specific Language|Domain Specific Language]] som lar en skrive dokumentasjon som kode og deretter generere en visualisering utifra dette. Selve diagramm-dataene og visualisering er lousely coupled og det tilbys en rekke eksporteringer. Man kan bruke Structurizr renderen som rendrer til et webview hvor man kan interaktivt zoome inn på de forskjellige nivåene. Men man kan også eksportere til andre populære formater som mermaid, png, eller svg.    

## Ressurser
[https://c4model.com](https://c4model.com/)

