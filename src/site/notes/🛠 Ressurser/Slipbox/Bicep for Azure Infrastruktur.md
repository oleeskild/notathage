---
{"dg-publish":true,"permalink":"/ressurser/slipbox/bicep-for-azure-infrastruktur/"}
---
[[🛠 Ressurser/Slipbox/Update Conference 2021|Update Conference 2021]]
### Bicep for Azure Infrastruktur
Er et domenespesifikt språk for å programmere infrastruktur i Azure
Kompilerer til ARM, er bare en wrapper rundt det. 
Er i preview så det kan komme breaking changes men det er prodksjonsklart. 

Man kan lage moduler og sende parametre inn. Basically en slags template
VSCode har en kjempegod extension som hjelper med autocomplete.

Man kan lage et biceprepository(!!) i Azure. Her lager man gjenbrukbare moduler som man så kan ta i bruk i sine egen bicep filer på prosjektet sitt, ved å enkelt bare gi adressen til repoet. Mega aktuelt å ta i bruk!

Man kan kjøre bicep filer enten via azure cli eller via powershell. Det er også sånn man gjør det i Azure Devops eller GitHub Actions. 

For å verifisere at en bicep fil er som den skal kan man lage en bicepconfig.json som definerer hvilke feil som er warnings og hva som er errors. Errors vil stoppe bygge til f.eks. Azure pipeline.

I Terraform bruker man en state fil for å tracke hvordan staten på miljøet er nå. Men denne kan bli corrupted og gå ut av sync om noen endrer fra et annet sted enn terraform. I Bicep så er det det faktiske miljøet i Azure som er state fila! 10/10 kult.