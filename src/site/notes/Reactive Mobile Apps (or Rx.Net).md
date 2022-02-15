[[Update Conference 2021]]
### Reactive Mobile Apps (or Rx.Net)
* Funksjonell måte å generere liste på som jeg ikke har sett før:
 ```csharp 
 Enumerable.Range(0,5).Select(x=>new Thing())
 ```

Lag en event stream ved å kalle .ToObservable() på en IEnumerable.

Observables gir oss litt mer enn det et vanlig event gir oss. 
I tillegg til å fange opp events kan man handle exceptions og at en Observable er ferdig.
```csharp
.Subscript(
	update => {},
	exception => {},
	()=>{/* Finished. Dispose this */}
)
```

Observables kan også bruke LINQ. 
```csharp
someObservable.Where(x=>x.Age >25).Subscribe()
```

Husk å dispose en observable når man er ferdig, hvis ikke blir det minnelekasje. 

[Acavache](https://github.com/reactiveui/Akavache) fungerer bra sammen med observable mønsteret. Det er en database til bruk på desktop og mobil, og kan brukes til å f.eks. prøve å hente noe fra et API et sted, men vil returnere det siste den har i cachen først, også når den får svar fra tjenesten oppdatere med en ny verdi til de som subscriber.