# New Certificate

```powershell
New-SelfSignedCertificate -DnsName *.mydomainname.local -CertStoreLocation cert:\LocalMachine\My -TestRoot
```