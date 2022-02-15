[[Update Conference 2021]]
### Intro til NET 6
* .NET blir supported i 18 mnd fra release
* Tiered Compilation: Istedenfor å optimalisere kode ved startup (og forlenge oppstartstid) så kjører den koden as is, og optimaliserer heller under runtime de metodene som blir kallt ofte
* For å oppgradere endrer man bare TargetFramework i csproj filen fra NET5.0 til NET6.0
* Man kan også targete kun windows med NET6.0-windows. Da får man ekstra biblioteker (som WinForm)
* MAUI er .NET nye rammeverk for å kode apper for iOS og Android
* Programmer trenger ikke lenger en main metode. Det er syntaktisk sukker, og vil bli lagt til ved kompilering. Det betyr at man bare kan begynne å skrive istedenfor. 
* Man kan lage globale using statements slik at man slipper `using System` overalt i alle filer. Dette vil være i en egen fil
* Man kan også legge til implisitte using statement i csproj. Default er at namespace som System o.l. allerede er implicit using. Men man kan også definere dette selv. 
* ArgumentNullException.ThrowIfNull(someVariable)
* Man kan awaite i en while løkke. F.eks. `while(await timer.NextAsync()`
* Paralell støtter også nå async
* Det er kommet nye typer for å kun holde dato eller kun tid: `DateOnly` og `TimeOnly`
* `<Nullable>enable</Nullable>` Gjør at man får warnings dersom man setter noe lik null uten at man har sagt eksplisitt at denne kan være null, ved å sette `string?`. 
* Ved å bruke record kan man bruke `public string Name {get; init;}` og da, til forskjell for readonly, får man lov å sette felt i initialisereringen. (Men kun der).
	* Record er bra å bruke for DTO er fordi man enkelt og greit kan definere dem på en linje. 
**JSON**
* `class Something: IJSonOnDeserialize, IJsonOnSerilize` lar oss hooke på metoder for når objektet blir (de)serialisert. F.eks. kan man da validere dataene. 
* **LINQ** Syntaks sukker for ranges>
	* `source.Take(..5)` Fra 0 til 5
	* `source.ElementAt(^2)` Hent elementet som er 2 fra bakerst. list.Length - 1 - 2.
	* `source.Take(2..5)` Fra 2-5
	* `source.Take(5..)` Fra 5 til endes
	* `source.Take(..^2)` Fra 0 til 2. siste.
* `list.MinBy(x=>x.Age)` Finner minste med tanke på Age
* **Records**
	* `var p2 = new Person("ole", 27)`
	* `var p3 = p2 with {Age=5}` 
	* Lager en dyp kopi men med age endret