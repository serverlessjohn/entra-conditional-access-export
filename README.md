# Entra ID Conditional Access Export

Exports every Conditional Access policy in a Microsoft Entra ID tenant
to JSON — one file per policy — so changes can be tracked, reviewed,
and rolled back.

## Why

Conditional Access is the backbone of access control in Entra ID, but
the portal gives you no version history. A policy gets edited, something
breaks three days later, and there's no record of what changed or who
changed it.

Across the tenants I work in, the answer has usually been a screenshot
in a ticket. That isn't a backup and it isn't auditable.

This exports the full policy set as structured JSON you can diff,
commit, and restore from.

## What it does

- Connects to Microsoft Graph with read-only permissions
- Pulls every Conditional Access policy in the tenant
- Writes each policy to its own JSON file, named for the policy
- Drops them in a timestamped folder so runs can be compared

## Requirements

- PowerShell 7+
- Microsoft.Graph PowerShell SDK
  (`Install-Module Microsoft.Graph -Scope CurrentUser`)
- An account with the `Policy.Read.All` Graph scope

## Usage

```powershell
Connect-MgGraph -Scopes "Policy.Read.All"
.\Export-ConditionalAccessPolicies.ps1 -OutputPath .\exports
```

## Output
<img width="544" height="102" alt="image" src="https://github.com/user-attachments/assets/20cd3030-dd92-4b7e-99e7-b65b54d65d3c" />



Each file contains the full policy definition — conditions, grant
controls, session controls, assignments, and state.

## Notes

- Read-only. The script does not create, modify, or delete anything.
- Built and tested against a Microsoft 365 developer tenant, not a
  production or client environment.
- Policy display names are sanitized for filesystem-safe characters.

## Next

This is the export half of a larger goal: managing Conditional Access
as code. Planned additions —

- Diff between two export runs to surface out-of-band changes
- Deploy policies from JSON back into a tenant
- Report-only to enforced promotion workflow
