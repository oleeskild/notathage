# Git ekskluder lokalt

Åpne .git/info/exclude filen

Skriv inn filen som skal ignoreres. Eks: *connectionString.config

Kjør denne kommandoen for å hente endringene i exclude:

```bash
git update-index --assume-unchanged <file>
```