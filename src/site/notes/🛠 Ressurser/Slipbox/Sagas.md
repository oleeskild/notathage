---
{"dg-publish":true,"permalink":"/ressurser/slipbox/sagas/","dgHomeLink":true,"dgPassFrontmatter":false}
---

[[🛠 Ressurser/Slipbox/Microservices|Microservices]]

# Sagas

En saga er en sekvens av transaksjoner som skjer i sekvens på tvers av flere tjenester. F.eks. kan det å booke en ferie inneholde følgende tre transaksjoner:
1. Book en bil
2. Book et hotell
3. Book en flyreise

Alle disse tre transaksjonene kan skje i tre forskjellige tjenester, men det er likevel én handling. Denne handlingen kalles en saga. 

Om en av disse tre transaksjonene feiler, så må de foregående transaksjonene endre tilbake. Det vil si: Dersom feilen skjer på trinn 3, bookingen av flyreisen, må det eksistere en mekanisme for å sørge for at man tilbake stiller trinn 1 og 2 og fjerner bookingen av hotellet og av bilen.

Dette kan typisk gjøres på to måter: Orkestrert saga, eller koreografert saga.

## Orkestrert saga
I en orkestrert saga har man en sentral "orkestrator" som limer sammen alle tjenestene som inngår i handlingen man ønsker å utføre. Da har man typisk ett API som utfører businesslogikken, men tar i bruk flere andre tjenester for å utføre de forskjellige handlingene. Eksempelvis kan en tjeneste som har ansvar for å onboarde en kunde i et system inneholde logikk for å:
1. Opprette kunde
2. Sende velkomst-epost til kunde
3. Sende SMS til kunde

Da vil orkestrerings APIet inneholde logikk for å gjøre disse tingene, men vil kalle på et Kunde API for å opprette, et epost API for å sende epost, og et SMS API for å sende SMS. I tillegg må det inneholde logikk for å tilbakestille foregående steg om noe går galt underveis.


## Koreografert saga
Koreografert saga er et mønster hvor logikken for å utføre alt er implisitt. Ofte brukes det sammen med et event-system. Med eksempelet over vil man starte med å sende en forespørsel om å opprette en kunde i Kunde APIet. Da vil kunde APIet kringkaste en melding på event-køen om at en kunde er opprettet. Deretter vil andre APIer reagere på dette. Epost APIet lytter på dette eventet, og vil sende epost. SMS APIet vil gjøre det samme. Om noe feiler kan de sende et event om dette, så må andre APIer reagere på dette.

Her kan det være vanskeligere å få oversikt over hvordan logikken egentlig er og hva som helhetlig skjer. Fordelen er at det er decoupled og distribuert og dermed lett å skalere.

## Ressurser
[Original paper om Sagas](https://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf)
https://microservices.io/patterns/data/saga.html

