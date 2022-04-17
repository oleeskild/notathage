---
{"dg-publish":true,"permalink":"/ressurser/slipbox/monitorering-av-mikrotjenester-er-komplekst/","dgHomeLink":true,"dgPassFrontmatter":false}
---

[[🛠 Ressurser/Slipbox/Microservices|Microservices]]

I en ikke-distribuert kodebase er det enkelt å se hvor i koden det eksploderte dersom noe kaster en exception. I distribuerte tjenester er dette verre. En feil i tjeneste A kan ha skjedd pga. en feil i tjeneste B som igjen feilet fordi tjeneste C ga uventede data. Å finne et stacktrace på tvers av disse diskrete tjenestene kan være utfordrende.
Løsningen er å fra starten av ta i bruk en korrelasjons id. Dermed kan man bruke loggverktøyets søkefunksjon til å se hele kallet fra start til slutt.