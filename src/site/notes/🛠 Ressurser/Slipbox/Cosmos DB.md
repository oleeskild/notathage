---
{"dg-publish":true,"permalink":"/ressurser/slipbox/cosmos-db/"}
---

[[🛠 Ressurser/Slipbox/Update Conference 2021|Update Conference 2021]]
### Cosmos DB
Cosmos DB er en NoSql implementasjon av Microsoft. Man kjører det vanligvis i Azure, men man kan også kjøre en CosmosDB emulator på sin egen maskin eller i et datacenter. Det finnes bl.a. et docker bilde.

Det finnes to typer partisjoneringer: Physical partitions, logical partitions. 
Physical er den faktisk databaseservere, logical er kontainere som er inndelt innad i en "fysisk" server. 
Henger ikke helt med på disse konseptene, må undersøke nærmere. 

Strukturen er ca. slik
```mermaid
	flowchart TD;
	Leader[(Leader)]
	Follower1[(Follower)]
	Follower2[(Follower)]
	Follower3[(Follower)]
	Leader-->Follower1
	Leader-->Follower2
	Leader-->Follower3
```

Cosmos tilbyr også en rekke forskjellige protokoller for å snakke med databasen: Cassandra, Mongo, SQL, Gremlin og Table. SQL er den som er maintainet av Microsoft og er derfor den som får den siste funksjonaliteten først. 

Cosmos DB tilbyr 3 moduser
**Gateway Mode**
```mermaid
graph LR;
	Client1(Klient)
	Client2(Klient)
	Database1[(DB)]
	Database2[(DB)]
	Database3[(DB)]
	Gateway((Gateway))
	Client1--Mongo API over HTTPS-->Gateway
	Client2--SQL API over HTTPS-->Gateway
	Gateway--TCP-->Database1
	Gateway--TCP-->Database2
	Gateway--TCP-->Database3
```

Denne legger selve database koplingen bak en brannmur og manager hvilke partisjoner man vil snakke med osv. Enklest å bruke.  

**Direct Mode**
```mermaid
graph LR;
	Client1(Klient)
	Client2(Klient)
	Database1[(DB)]
	Database2[(DB)]
	Database3[(DB)]
	Client1--TCP-->Database1
	Client1--TCP-->Database2
	Client2--TCP-->Database2
	Client2--TCP-->Database3
	Client1--TCP-->Database3
```

Om man ønske mer kontroll og enda raskere kommunikasjon (et mindre hopp å gjøre). 

**Direct Gateway**
Kom nylig, for noen uker siden. Ser likt ut som gateway mode men er dyrere og skal visstnok gi noen ekstra feature som f.eks. muligheten til å cache resultater i gatewayen. 


Partition Key er det som bestemmer hvilken container hvert objekt skal i. Hver kontainer er en "bucket" med en hash, så jo flere ting man har i hver kontainer, jo lengre tid vil det ta å finne dem, da man da må loope gjennom alle objektene som er i kontaineren.