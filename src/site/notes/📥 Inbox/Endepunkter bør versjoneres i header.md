---
{"dg-publish":true,"permalink":"/inbox/endepunkter-bor-versjoneres-i-header/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Endepunkter bør versjoneres i header

Dersom man versjonerer et API ved å bruke path (myapi.com/v2/customer) så impliserer man at alt under v2/* har endret seg, selv om det kanskje bare er ett endepunkt. 

Om man tar i bruk header for versjonering kan man sette forskjellige versjoner for de ulike endepunktene under customer, og da speile hvilke endepunkter som er nye og hvilke som er som før. 

Ulempen ved å bruke header er at det ikke er like åpenbart at APIet versjoneres for en utvikler som blir introdusert til kodebasen. Men dette handler om å skrive god kode som tydeliggjør for andre utviklere hvilken versjon man tar i bruk. 

