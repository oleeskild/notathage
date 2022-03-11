---
{"dg-publish":true,"permalink":"/ressurser/slipbox/nullable-c/"}
---

[[🛠 Ressurser/Slipbox/Update Conference 2021|Update Conference 2021]]
### Nullable C#
* Bruk `someObject is null` istedenfor == null. Is er optimalisert i kompilatoren og i tillegg så kan == overskrives til å bety noe annet.
	* eventuelt `someObject is not null`
* Å bruke "?" bak en type vise at din **intensjon** er at denne typen kan være null. Om du ikke bruker det så betyr det at den i utgangspunktet ikke skal være null
* Dangit operatoren "!" bak en variabel betyr at du sier til kompilatoren at "Jeg vet hva jeg gjør, ikke gi meg warning for at denne muligens kan være null til tross for at den egentlig ikke er nullable". F.eks. ved testing: `DoTheThing(string text)` => `Assert.Throws(DoTheThing(null!))`
* INotifyPropertyChanged er et interface man kan implementere for å lytte på hver gang en endring skjer i en property i klassen.
* ImplicitUsing og global using er en greie som blir nevnt mye.
* `[NotNullWhen(true)]` sier til kompilatoren at f.eks. en out variabel ikke kan være null dersom metoden returnerer true. Typisk på `TryParseSomething(text, out var theThing)`. 
* Om man skrur på Nullability på et eksisterende prosjekt, vær forsiktig med EntityFramework. Dersom man ikke eksplisitt har satt nullability, `string? SomeText` så vil EF ved neste migrering endre alle typer til å ikke være nullable lenger.