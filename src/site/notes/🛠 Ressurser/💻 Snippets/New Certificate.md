---
{"dg-publish":true,"permalink":"/ressurser/snippets/new-certificate/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# New Certificate

```powershell
New-SelfSignedCertificate -DnsName *.mydomainname.local -CertStoreLocation cert:\LocalMachine\My -TestRoot
```