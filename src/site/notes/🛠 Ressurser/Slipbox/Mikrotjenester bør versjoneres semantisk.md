---
{"dg-publish":true,"permalink":"/ressurser/slipbox/mikrotjenester-bor-versjoneres-semantisk/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Mikrotjenester bør versjoneres semantisk

Om man ser seg nødt til å vedlikeholde flere versjoner av en mikrotjeneste bør man ikke deploye 2 forskjellige instanser av denne mikrotjenesten.  Dette vil føre til høyere kostnad (flere instanser), data-kompatibilitet på tvers av to versjoner og bug-fiksing to steder.

Istedet bør man bruke semantisk versjonering. Dvs at både v1 og v2 ligger i samme kodebase, og i samme instanse. Da unngår man problemene over. Man kan versjonere semantisk på flere måter men [[🛠 Ressurser/Slipbox/Endepunkter bør versjoneres i header|Endepunkter bør versjoneres i header]].