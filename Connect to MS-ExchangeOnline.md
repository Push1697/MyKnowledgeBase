https://learn.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps

Connect to Exchange Online PowerShell with an interactive login prompt
The following examples work in Windows PowerShell 5.1 and PowerShell 7 for accounts with or without MFA:

This example connects to Exchange Online PowerShell in a Microsoft 365 or Microsoft 365 GCC organization:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName navin@contoso.onmicrosoft.com
This example connects to Exchange Online PowerShell in a Microsoft GCC High organization:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName laura@blueyonderairlines.us -ExchangeEnvironmentName O365USGovGCCHigh
This example connects to Exchange Online PowerShell in a Microsoft 365 DoD organization:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName julia@adatum.mil -ExchangeEnvironmentName O365USGovDoD
This example connects to Exchange Online PowerShell in an Office 365 Germany organization:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName lukas@fabrikam.de -ExchangeEnvironmentName O365GermanyCloud
In the sign-in window that opens, enter your password, and then click Sign in.

Enter your password in the Sign in to your account window.

 Note

In PowerShell 7, browser-based single sign-on (SSO) is used by default, so the sign-in prompt opens in your default web browser instead of a standalone dialog.

MFA only: A verification code is generated and delivered based on the response option that's configured for your account (for example, a text message or the Microsoft Authenticator app on your device).

In the verification window that opens, enter the verification code, and then click Verify.

Enter your verification code in the Sign in to your account window.

PowerShell 7 exclusive connection methods
In PowerShell 7 for accounts without MFA, this example prompts for credentials within the PowerShell window:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName navin@contoso.onmicrosoft.com -InlineCredential
In PowerShell 7 for accounts with or without MFA, this example uses another computer to authenticate and complete the connection. Typically, you use this method on computers that don't have web browsers (users are unable to enter their credentials in PowerShell 7):

Run the following command on the computer where you want to connect:

PowerShell

Copy
Connect-ExchangeOnline -Device
The connection command waits at following output:

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code <XXXXXXXXX> to authenticate.

Note the <XXXXXXXXX> code value.

On any other device with a web browser and internet access, open https://microsoft.com/devicelogin and enter the <XXXXXXXXX> code value from the previous step.

Enter your credentials on the resulting pages.

In the confirmation prompt, click Continue. The next message should indicate success, and you can close the browser or tab.

The command from step 1 continues to connect you to Exchange Online PowerShell.

Connect to Exchange Online PowerShell without a login prompt (unattended scripts)
For complete instructions, see App-only authentication for unattended scripts in Exchange Online PowerShell and Security & Compliance PowerShell.

Connect to Exchange Online PowerShell in customer organizations
For more information about partners and customer organizations, see the following topics:

What is the Cloud Solution Provider (CSP) program?.
Introduction to granular delegated admin privileges (GDAP)
This example connects to customer organizations in the following scenarios:

Connect to a customer organization using a CSP account.

Connect to a customer organization using a GDAP.

Connect to a customer organization as a guest user.

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName navin@contoso.onmicrosoft.com -DelegatedOrganization adatum.onmicrosoft.com
Connect to Exchange Online PowerShell using managed identity
For more information, see Use Azure managed identities to connect to Exchange Online PowerShell.

System-assigned managed identity:

PowerShell

Copy
Connect-ExchangeOnline -ManagedIdentity -Organization "cohovinyard.onmicrosoft.com"
User-assigned assigned managed identity:

PowerShell

Copy
Connect-ExchangeOnline -ManagedIdentity -Organization "constoso.onmicrosoft.com" -ManagedIdentityAccountId <ManagedIdentityAccountIdGuid>
Step 3: Disconnect when you're finished
Be sure to disconnect the session when you're finished. If you close the PowerShell window without disconnecting the session, you could use up all the sessions available to you, and you need to wait for the sessions to expire. To disconnect the session, run the following command:

PowerShell

Copy
Disconnect-ExchangeOnline
To silently disconnect without a confirmation prompt, run the following command:

PowerShell

Copy
Disconnect-ExchangeOnline -Confirm:$false
 Note

The disconnect command will likely fail if the profile path of the account that you used to connect contains special PowerShell characters (for example, $). The workaround is to connect using a different account that doesn't have special characters in the profile path.

How do you know you've connected successfully?
If you don't receive any errors, you've connected successfully. A quick test is to run an Exchange Online PowerShell cmdlet, for example, Get-AcceptedDomain, and see the results.

If you receive errors, check the following requirements:

A common problem is an incorrect password. Run the connection steps again and pay close attention to the username and password that you use.

The account that you use to connect to must be enabled for PowerShell access. For more information, see Enable or disable access to Exchange Online PowerShell.

TCP port 80 traffic needs to be open between your local computer and Microsoft 365. It's probably open, but it's something to consider if your organization has a restrictive internet access policy.

If your organization uses federated authentication, and your identity provider (IDP) and/or security token service (STS) isn't publicly available, you can't use a federated account to connect to Exchange Online PowerShell. Instead, create and use a non-federated account in Microsoft 365 to connect to Exchange Online PowerShell.

REST-based connections to Exchange Online PowerShell require the PowerShellGet module, and by dependency, the PackageManagement module, so you'll receive errors if you try to connect without having them installed. For example, you might see the following error:

The term 'Update-ModuleManifest' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.

For more information about the PowerShellGet and PackageManagement module requirements, see PowerShellGet for REST-based connections in Windows.

After you connect, you might received an error that looks like this:

Could not load file or assembly 'System.IdentityModel.Tokens.Jwt,Version=<Version>, Culture=neutral, PublicKeyToken=<TokenValue>'. Could not find or load a specific file.

This error happens when the Exchange Online PowerShell module conflicts with another module that's imported into the runspace. Try connecting in a new Windows PowerShell window before importing other modules.

Appendix: Comparison of old and new connection methods
This section attempts to compare older connection methods that have been replaced by the Exchange Online PowerShell module. The Basic authentication and OAuth token procedures are included for historical reference only and are no longer supported.

Connect without multi-factor authentication
Exchange Online PowerShell module with interactive credential prompt:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName admin@contoso.onmicrosoft.com
Exchange Online PowerShell module without interactive credential prompt:

PowerShell

Copy
$secpasswd = ConvertTo-SecureString '<Password>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $secpasswd)

Connect-ExchangeOnline -Credential $o365cred
Basic authentication:

PowerShell

Copy
$secpasswd = ConvertTo-SecureString '<Password>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $secpasswd)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/ -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
New-PSSession with OAuth token:

PowerShell

Copy
$oauthTokenAsPassword = ConvertTo-SecureString '<EncodedOAuthToken>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $oauthTokenAsPassword)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?BasicAuthToOAuthConversion=true -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
Connect with multi-factor authentication
Exchange Online PowerShell module with interactive credential prompt:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName admin@contoso.onmicrosoft.com
Basic authentication: Not available.

New-PSSession with OAuth token: Not available.

Connect to a customer organization with a CSP account
Exchange Online PowerShell module:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName admin@contoso.onmicrosoft.com -DelegatedOrganization delegated.onmicrosoft.com
Basic authentication:

PowerShell

Copy
$secpasswd = ConvertTo-SecureString '<Password>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $secpasswd)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?DelegatedOrg=delegated.onmicrosoft.com&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@delegated.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
New-PSSession with OAuth token:

PowerShell

Copy
$oauthTokenAsPassword = ConvertTo-SecureString '<EncodedOAuthToken>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $oauthTokenAsPassword)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/? DelegatedOrg=delegated.onmicrosoft.com&BasicAuthToOAuthConversion=true&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@delegated.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
Connect to a customer organization using GDAP
Exchange Online PowerShell module:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName admin@contoso.onmicrosoft.com -DelegatedOrganization delegated.onmicrosoft.com
Basic authentication:

PowerShell

Copy
$secpasswd = ConvertTo-SecureString '<Password>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $secpasswd)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?DelegatedOrg=delegated.onmicrosoft.com&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@delegated.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
New-PSSession with OAuth token:

PowerShell

Copy
$oauthTokenAsPassword = ConvertTo-SecureString '<EncodedOAuthToken>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $oauthTokenAsPassword)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?DelegatedOrg=delegated.onmicrosoft.com&BasicAuthToOAuthConversion=true&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@delegated.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
Connect to a customer organization as a guest user
Exchange Online PowerShell module:

PowerShell

Copy
Connect-ExchangeOnline -UserPrincipalName admin@contoso.onmicrosoft.com -DelegatedOrganization delegated.onmicrosoft.com
Basic authentication:

PowerShell

Copy
$secpasswd = ConvertTo-SecureString '<Password>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $secpasswd)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?DelegatedOrg=delegated.onmicrosoft.com&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@delegated.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
New-PSSession with OAuth token:

PowerShell

Copy
$oauthTokenAsPassword = ConvertTo-SecureString '<EncodedOAuthToken>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $oauthTokenAsPassword)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?DelegatedOrg=delegated.onmicrosoft.com&BasicAuthToOAuthConversion=true&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@delegated.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
Connect to run unattended scripts
Exchange Online PowerShell module:

Certificate thumbprint:

 Note

The CertificateThumbprint parameter is supported only in Microsoft Windows.

PowerShell

Copy
Connect-ExchangeOnline -CertificateThumbPrint "012THISISADEMOTHUMBPRINT" -AppID "36ee4c6c-0812-40a2-b820-b22ebd02bce3" -Organization "contoso.onmicrosoft.com"
Certificate object:

PowerShell

Copy
Connect-ExchangeOnline -Certificate <%X509Certificate2Object%> -AppID "36ee4c6c-0812-40a2-b820-b22ebd02bce3" -Organization "contoso.onmicrosoft.com"
Certificate file:

PowerShell

Copy
Connect-ExchangeOnline -CertificateFilePath "C:\Users\navin\Desktop\automation-cert.pfx" -CertificatePassword (ConvertTo-SecureString -String "<Password>" -AsPlainText -Force) -AppID "36ee4c6c-0812-40a2-b820-b22ebd02bce3" -Organization "contoso.onmicrosoft.com"
For more information, see App-only authentication for unattended scripts in Exchange Online PowerShell and Security & Compliance PowerShell.

Basic authentication:

PowerShell

Copy
$secpasswd = ConvertTo-SecureString '<Password>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $secpasswd)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/ -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
New-PSSession with OAuth token:

PowerShell

Copy
$oauthTokenAsPassword = ConvertTo-SecureString '<EncodedOAuthToken>' -AsPlainText -Force

$o365cred = New-Object System.Management.Automation.PSCredential ("admin@contoso.onmicrosoft.com", $oauthTokenAsPassword)

$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/PowerShell-LiveID/?BasicAuthToOAuthConversion=true&email=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@contoso.onmicrosoft.com -Credential $o365cred -Authentication Basic -AllowRedirection

Import-PSSession $Session
Connect using managed identity
Exchange Online PowerShell module:

System-assigned managed identity:

PowerShell

Copy
Connect-ExchangeOnline -ManagedIdentity -Organization "contoso.onmicrosoft.com"
User-assigned managed identity:

PowerShell

Copy
Connect-ExchangeOnline -ManagedIdentity -Organization "contoso.onmicrosoft.com" -ManagedIdentityAccountId <UserAssignedManagedIdentityPrincipalIdValue>
For more information, see Use Azure managed identities to connect to Exchange Online PowerShell.

Basic authentication: Not available.

New-PSSession with OAuth token: Not available.
