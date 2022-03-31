---
{"dg-publish":true,"permalink":"/ressurser/slipbox/dot-js-2019/"}
---
# dotJS 2019
[dotJS 2019](https://www.dotjs.io/)

# Match typescript

-   Man kan bruke match i typescript på samme måte som pattern matching i Haskell. Dette kan da f.eks. brukes til å matche på state. Istedenfor å ha en variabel "loading", "loaded", "error" osv, der feile states kan eksistere så kan man ha en state variabel. Deretter kan en matche på dette og det kan da kun oppstå gyldige states.

# Testing library

-   Bibliotek for vue, react, svelte osv for å teste kode uten å vite noe om implementasjonen. Man tester som en bruker istedenfor som en tester. F.eks. istedenfor å hente ut en knapp med klassenavn eller kjøre en metode så henter vi ut en knapp med gitt tekst for så å kalle click på denne. Dermed introduserer man ikke en trede part inn i koden (utvikler og bruker er de allerede eksisterende brukerne av koden)

# Webhint code

-   Plugin til VS Code. Bruker mozillas dokumentasjon til å hjelpe å kode underveis. Får røde squiggly lines dersom man mangler noe, som en alt tekst på et bilde. Hvis man hovrer over forklares det hva som mangler med utdrag fra Mozillas dokumentasjon.

# Web workers + offscreencanvas

-   Istedenfor å eksekvere arbeid på main thread, slik at alt fryser og ingen events blir fanget, kan man sende det til en webworker. Dette er en ekstern prosess som eksekverer parallelt og lar main thread forbli åpen. I tillegg kan man bruke noe som heter offscreencanvas som gjør at man faktisk kan tegne og vise et canvas til brukeren på en annen tråd slik at man kan visualisere rimelig heftige ting uten å låse ned hele tråden.

# Stencil

-   Rammeverk der man kan skrive Web Components for så å konvertere dem til Vue, React, Svelte og andre rammeverk.
-   Perfekt for å brukes til å lage et komponent bibliotek.

# [Webtask.io](http://webtask.io/)

-   Nettside for å sette opp api kall til en eller annen tjeneste hver nte tidsenhet. F.eks. brukes til å trigge en build på netlify hvert 5. minutt.

# Eleventy

-   En enkel static site generator skrevet i Javascript.

# Faunadb
- Serverless database som kan brukes i forbindelse med jamstack. No setup required.

# [Findthat.at/servered](http://findthat.at/servered)
-  Talk fra Netlify dude. 
-  Også link til en presentasjonsnettside

# Nest azure functions
-   Gjør det enkelt å lage azure functions med å bruke nest. Nest har mange opionions om hvordan man skal lage endepunkt, slik at man slipper å selv lage patterns for endepunkt. Bruker node og express i bunn.

# Hexa
-   Visstnok bare verktøy for å lage et cli tool som bruker forskjellige clis under. Brukter på dotjs i forbindelse med å deploye nest som azure function.

# [Nitr.ooo](http://nitr.ooo/)

-   Nettside med info om hvordan man bruker nest og hexa i forbindelse med azure functions.

# [Now.sh](http://now.sh/)

-   Alternativ til netlify. Kan generere en statisk nettside direkte fra terminal til [now.sh](http://now.sh/) og den vil publiseres med en gang. 

# Rollup

-   En bundler for javascript. Ofte brukt av library authors.

# Luxon

-   Tidslibrary som moment.js anbefaler å bruke istedenfor dem selv.

# Temporal

-   Ny dato og tid API som kommer ttil neste Javascript.

# Optimize

-   Talk fra duden som laget leaflet. Enkelt og greit, vær obs på at array.indexOf, Slice osv e lineære. Kan ofte lønne seg å gjøre ting på en litt annen måte. Vis O(nˆ3) så kan man mest sannsynlig minimum redusera med en faktor. Samma for O(nˆ2) osv.

# Actual budget
-   Nytt produkt fra duden som laget Prettier.
-  Talken handlet om hvordan man kan synce til offline systemer uten å få issues. Mest interessante var HLC som er en klokke man kan bruke til å implementere timestamps.

