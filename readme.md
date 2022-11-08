# Fred.PowerShell.Certificate.Parameter

Welcome to the project to make certificate parameters in PowerShell easier to manage.
With this library you make your parameter of type `CertificateParameter` and that's all that is needed to accept certificates.
Commands using this accept ...

+ X509Certificate2 objects, as retrieved from the certificate store or loaded from pfx file
+ Subject Names (which will then be used to search the certificate stores for the latest valid version with a private key)
+ Thumbprint (which will retrieve exactly that version from the certificate stores)

It ensures that only certificates with a private key can be passed in and will otherwise generate an exception.

## Using it

This library can be used in both c#-based commands (cmdlets) or script-based commands (functions).

> C#

```csharp
/// <summary>
/// Certificate to connect with.
/// </summary>
[Parameter()]
public CertificateParameter Certificate
```

> PowerShell

Example in script:

```powershell
function Connect-Exchange {
	[CmdletBinding()]
	param (
		[Parameter(Mandatory = $true)]
		[string]
		$ClientID,

		[Parameter(Mandatory = $true)]
		[string]
		$TenantID,

		[Parameter(Mandatory = $true)]
		[Fred.PowerShell.Certificate.Parameter.CertificateParameter]
		$Certificate
	)

	Connect-ExchangeOnline -AppID $ClientID -Organization $TenantID -Certificate $Certificate

	#TODO: Add whatever else the command is supposed to do
}