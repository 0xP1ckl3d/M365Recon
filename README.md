# M365Recon

**M365Recon** is a PowerShell-based reconnaissance tool for enumerating Microsoft 365 tenant information using unauthenticated methods. It performs DNS, HTTP, SOAP, and NTLM-based probing to reveal publicly exposed services, configurations, and internal metadata.

## Features

- Enumerates Microsoft 365-associated domains via the GetFederationInformation SOAP request
- Resolves MX records for the original and discovered tenant domains
- Queries OpenID configuration to extract tenant ID and token endpoint
- Performs `getuserrealm` lookups to identify federation branding
- Probes OWA autodiscover endpoints and detects Exchange exposure via OWA and NTLM headers
- Extracts internal Active Directory domain names from NTLM challenge headers
- Detects SharePoint Online and OneDrive MySite hosting via tenant-specific URLs
- Outputs a structured summary of findings per tenant

## Usage

```powershell
.\M365Recon.ps1 -Domain example.com
```

## Example Output
```
[+] MX: example-com-au.mail.protection.outlook.com

[*] Querying Microsoft for tenant-linked domains...

[+] Domains associated with tenant:
  - example.com
  - example.onmicrosoft.com
  - test.example.com
  - example.org

[+] Tenant Name: example
[*] Validating example.onmicrosoft.com
[+] MX for example.onmicrosoft.com: example.mail.protection.outlook.com
[!] This endpoint may be targetable via Direct Send and bypass the Secure Email Gateway (SEG)
[*] Fetching OpenID configuration...
[+] Token Endpoint: https://login.windows.net/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/oauth2/token
[+] Tenant ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
[*] Checking user realm for Federation brand...
[+] Federation Brand: Example Org
[*] Probing for OWA...
[+] OWA endpoint discovered (from error): https://mail.example.com/autodiscover/autodiscover.xml
[+] OWA endpoint discovered: https://mail.example.com/autodiscover/autodiscover.xml
[*] Attempting internal domain extraction via NTLM...
[+] Internal Domain (NTLM): internal.example.com
[*] Checking if SharePoint Online is enabled for the tenant...
[+] SharePoint Online (Root) appears active: https://example.sharepoint.com
[+] OneDrive hosting via SharePoint (MySite) appears active: https://example-my.sharepoint.com

[*] Summary:

TenantName            : example
OnMicrosoftDomain     : example.onmicrosoft.com
TenantMXRecord        : example.mail.protection.outlook.com
OriginalDomainMX      : example-com-au.mail.protection.outlook.com
TenantID              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
TokenEndpoint         : https://login.windows.net/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/oauth2/token
FederationBrand       : Example Org
OWA Discovered        : https://mail.example.com/autodiscover/autodiscover.xml
InternalDomain        : internal.example.com
```

## Requirements
- PowerShell 5.1 or later (Windows), or PowerShell Core 7+ (cross-platform)
- Internet access

## License
MIT

---

**Disclaimer:** M365Recon is intended for authorised testing, training, and research only. Do not use against unauthorised targets.

