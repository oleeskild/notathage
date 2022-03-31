---
{"dg-publish":true,"permalink":"/ressurser/slipbox/logging-i-mikrotjenester-bor-aggregeres/"}
---
[[🛠 Ressurser/Slipbox/Microservices|Microservices]]

Om man har en mikrotjeneste-arkitektur bør all logging aggreggeres på tvers av tjenestene til en sentral loggetjeneste. Dette henger sammen med at [[🛠 Ressurser/Slipbox/Monitorering av mikrotjenester er komplekst|Monitorering av mikrotjenester er komplekst]]. Det vil være tidkrevende å manuelt gå innom hver tjeneste for å bygge seg opp et bildet over hvordan et "helt" kall ser ut. I tillegg bør man fra starten av introdusere en korrelasjons-id slik at man enkelt kan hente ut hele ruten til én request. 