---
{"dg-publish":true,"permalink":"/ressurser/slipbox/tid-dato-og-kode/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Tid, dato og kode

## Nodatime
Instant i Nodatime representerer et objektivt punkt på tidslinjen. Det er sekunder siden 1. Jan 1970 som ligger til grunne for representasjonen, med en rekke funksjoner på toppen som operer på denne dataen.

LocalDate er et dato-objekt. Uten tid og dermed uten tidssone. 02. August 1980 i Oslo er ikke nødvendigvis samme tidspunkt som 02. August 1980 i Malibu. Det representerer et tidspunkt i ett kalendersystem. Det inneholder ikke noe info om UTC eller lignende. 

ZonedDateTime er en dato og et klokkeslett med en tilhørende tidssone. Her har man altså også informasjon om hva offsett er i forhold til UTC. 


[Kode24 Artikkel](https://www.kode24.no/guider/tid---hvor-vanskelig-kan-det-vaere/71490509)
[NodaTime Design philosophy](https://nodatime.org/1.3.x/userguide/design)

