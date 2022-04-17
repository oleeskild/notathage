---
{"dg-publish":true,"permalink":"/ressurser/slipbox/net-maui/","dgHomeLink":true,"dgPassFrontmatter":false}
---


[[🛠 Ressurser/Slipbox/Update Conference 2021|Update Conference 2021]]
### NET MAUI
Nytt UI bibliotek for .NET som kan kompileres til iOS, Android, Windows og Mac.  

Bruker en decoupled arkitektur hvor man har et UI framework på toppen, så et interface lag, og til slutt en plattform som implementerer disse interfacene. F.eks. vil iOS implementere IButton til å være en UIButton fra UIKit. 
Man kan bruke Xamarin XAML eller [[Blazor|Blazor]] som UI rammeverk. Men de jobber også med [[🛠 Ressurser/Slipbox/Comet|Comet]] som er et [[Model View Update|Model View Update]] rammeverk. Dette er det mønsteret som idag blir brukt av f.eks. React og Vue, hvor viewet automatisk reagerer på modell-endringer. 

På Mac brukes Catalyst og Windows brukes et helt nytt UI bibliotek som heter WinUI 3.

Det targeter NET6.0 > . Dette fordi det er her android og ios SDKene er inkludert

[Ressurser](https://codetraveler.io/update-maui/)