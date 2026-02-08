# M365Connect

Connect to Microsoft 365 services with a single command.

Supports PowerShell 5.1 (Desktop) and PowerShell 7+ (Core).

## Installation

```powershell
# Install from PowerShell Gallery
Install-Module -Name M365Connect -Scope CurrentUser
Import-Module M365Connect
```

Or install from source:

```powershell
# Clone the repo
git clone https://github.com/nikkelly/M365Connect.git

# Import for current session
Import-Module .\M365Connect\M365Connect.psd1

# Or install to your modules directory for permanent use
Copy-Item -Recurse .\M365Connect $env:PSModulePath.Split(';')[0]
Import-Module M365Connect
```

To auto-load on every session, add `Import-Module M365Connect` to your [PowerShell profile](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles).

## First Time Setup

```powershell
# Save credentials (username + password encrypted to environment variables)
Add-MSAccount

# Enable MFA mode (uses browser-based interactive auth)
Add-MSMFA

# Or configure app registration / service principal auth
Add-MSAppRegistration
```

## Usage

### Connection Commands

| Command | Alias | Service | Documentation |
|---------|-------|---------|---------------|
| `Connect-MSTeams` | `Teams` | Microsoft Teams | [Docs](https://learn.microsoft.com/en-us/MicrosoftTeams/teams-powershell-overview) |
| `Connect-MSExchange` | `Exchange` | Exchange Online | [Docs](https://learn.microsoft.com/en-us/powershell/exchange/exchange-online-powershell) |
| `Connect-MSAzureAD` | `AzureAD` | Azure AD | [Docs](https://learn.microsoft.com/en-us/powershell/module/azuread/) |
| `Connect-MSGraph` | `MSOnline` | Microsoft Graph | [Docs](https://learn.microsoft.com/en-us/powershell/microsoftgraph/overview) |
| `Connect-MSSharePoint` | `SharePoint` | SharePoint Online | [Docs](https://learn.microsoft.com/en-us/powershell/sharepoint/sharepoint-online/introduction-sharepoint-online-management-shell) |
| `Connect-MSSecurityCompliance` | `Security_Compliance` | Security & Compliance | [Docs](https://learn.microsoft.com/en-us/powershell/exchange/connect-to-scc-powershell) |
| `Connect-MSIntune` | `Intune` | Intune | [Docs](https://learn.microsoft.com/en-us/powershell/microsoftgraph/overview) |
| `Connect-MSExchangeServer` | `ExchangeServer` | Exchange On-Prem | [Docs](https://learn.microsoft.com/en-us/powershell/exchange/connect-to-exchange-servers-using-remote-powershell) |

### Bulk Commands

| Command | Alias | Description |
|---------|-------|-------------|
| `Connect-AllMSServices` | `connectAll` | Connect to all services at once |
| `Disconnect-AllMSServices` | `Disconnect` | Close all active connections |

### Account Management

| Command | Alias | Description |
|---------|-------|-------------|
| `Add-MSAccount` | `Add-Account` | Save credentials to environment variables |
| `Remove-MSAccount` | `Remove-Account` | Remove saved credentials |
| `Add-MSMFA` | `Add-MFA` | Enable MFA mode |
| `Remove-MSMFA` | `Remove-MFA` | Disable MFA mode |
| `Add-MSAppRegistration` | - | Configure service principal auth |
| `Remove-MSAppRegistration` | - | Clear service principal config |

### Status

| Command | Description |
|---------|-------------|
| `Get-MSConnectionStatus` | View current connection state and auth config |
| `Show-MSCommands` | Display available commands |

## Authentication Methods

1. **Interactive** (default) - Browser-based auth, supports MFA
2. **Credential** - Username/password saved as encrypted strings in environment variables
3. **Service Principal** - App registration with certificate or client secret

## PowerShell 7+ Notes

- AzureAD and MSOnline modules are deprecated and not supported on PS 7+. Their aliases (`AzureAD`, `MSOnline`) automatically redirect to `Connect-MSGraph`.
- The `Microsoft.Graph.Intune` module is deprecated. `Connect-MSIntune` uses `Microsoft.Graph.DeviceManagement` on PS 7+.

## Notes

- Username and password are encrypted using DPAPI (Windows) before saving to environment variables
- Non-Windows platforms have limited encryption support; certificate-based service principal auth is recommended
- All connection functions check for required modules and prompt for installation if missing
- `Remove-*` functions support `-WhatIf` and `-Confirm`

## Changelog

### v1.0.0
- Initial release on PowerShell Gallery
- PowerShell module format (.psd1 / .psm1) with Public/Private function layout
- PowerShell 5.1 and 7+ support with Microsoft Graph fallback for deprecated modules
- Service principal / app registration authentication (certificate or client secret)
- Interactive and stored credential authentication with optional MFA
- Connection tracking and status reporting via `Get-MSConnectionStatus`
- `ShouldProcess` support (`-WhatIf` / `-Confirm`) on state-changing functions
- Backward-compatible aliases for all original commands

### v2.0
- Refactored the entire project to be more dynamic
- No more auto-prompt for credentials
- Now allows for blank passwords
- Fixed an issue with SharePoint
