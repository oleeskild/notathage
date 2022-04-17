---
{"dg-publish":true,"permalink":"/ressurser/snippets/powershell-cheatsheet/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Powershell Cheatsheet

### Sett UI språk til engelsk
```powershell
Set-WinUILanguageOverride -Language en-US
```

### Sluk output
```powershell
. "C:\Source\Function.ps1" | Out-Null
```

### Lag virtuelle partisjoner
Lager en path "D:\" som kan nås i shellet, som peker på en mappe 
```powershell
New-Item -ItemType Directory C:\VirtualD -ErrorAction Ignore
New-Item -ItemType Directory C:\VirtualE -ErrorAction Ignore
New-Item -ItemType Directory C:\VirtualF -ErrorAction Ignore

subst.exe D: C:\VirtualD
subst.exe E: C:\VirtualE
subst.exe F: C:\VirtualF

$regPath = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices"
Set-ItemProperty -Path $regPath -Name "D:" -Value "\??\C:\VirtualD"
Set-ItemProperty -Path $regPath -Name "E:" -Value "\??\C:\VirtualE"
Set-ItemProperty -Path $regPath -Name "F:" -Value "\??\C:\VirtualF"
```

### Last inn nye ENV variabler
```powershell
refreshenv
```

### Les inn user input
```powershell
$ACCESS_TOKEN = Read-Host "Paste Your Personal Access Token"
```

### Pipe string inn til fil
```powershell
"some text"
| Set-Content $Home/config.txt
```
