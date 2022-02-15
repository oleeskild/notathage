[[Update Conference 2021]]
### C# Guidelines
Guidelines for NET6 og C#10

**Records**
Bruk records for DTOer. 
Kan bruke == for å sammenligne to records og da vil man faktisk sammenligne hvert felt, og ikke referansen. 

Spesifiserer bare alle feltene i konstruktøren så vil disse bli laget automatisk.
```csharp
public readonly record struct DailyTemperature(double HighTemp, double LowTemp);
```

```csharp
private static DailyTemperature[] data = new DailyTemperature[]
{
    new DailyTemperature(HighTemp: 57, LowTemp: 30), 
    new DailyTemperature(60, 35)
};
```

`data[0].HighTemp`

En record class vil være readonly etter initialiseringen
En record struct vil være read + write.

`[CallerArgumentException("value")]` <= Denne annotasjonen har blitt nevnt flere ganger. Vet ikke hva det er egentlig. 

Bruk "is" for å sjekke for null
`if(something is null)`
`if(something is not null)`
Operatoren == kan overskrives til å være noe annet. Men is er alltid safe.

Bruk `var` kun når datatypen ikke er åpenbar. F.eks. ikke bruk var her:
`var theNumber = MyClass.Number`
Hva er number? Int? BigInt? Decimal? Her bør man bruke
`int theNumber = MyClass.Number`

Bruk static i Actions og anonyme funksjoner, fordi da garanterer man at man ikke endrer noe i instansen i klassen. Bruk det som default, og fjern det heller dersom man faktisk skal endre noe i instansen:
`.DoTheThing(static item => item*2)`

Lag guidelines for prosjekter. Man kan starte med å forke noen andres og heller endre underveis. F.eks. er Brølifinakis sine guidelines her: https://intellitect.github.io/CodingGuidelines/