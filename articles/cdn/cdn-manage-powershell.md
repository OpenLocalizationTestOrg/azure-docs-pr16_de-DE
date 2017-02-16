<properties
    pageTitle="Verwalten von Azure CDN mit PowerShell | Microsoft Azure"
    description="Erfahren Sie, wie die Azure PowerShell-Cmdlets zum Verwalten von Azure CDN verwenden."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Verwalten von Azure CDN mit PowerShell

PowerShell bietet eine der am häufigsten flexiblen Methoden zum Verwalten von Benutzerprofilen Azure CDN und Endpunkte.  Sie können PowerShell interaktiv oder durch Schreiben von Skripts verwenden, um Verwaltungsaufgaben automatisieren.  In diesem Lernprogramm veranschaulicht einige der am häufigsten ausgeführten Aufgaben können Sie erreichen mit PowerShell zum Verwalten von Benutzerprofilen Azure CDN und Endpunkte.

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn beim Verwalten von Benutzerprofilen Azure CDN und Endpunkte PowerShell verwenden möchten, müssen Sie das Azure PowerShell-Modul installiert haben.  Informationen zum Azure PowerShell installieren, und verbinden Sie bei der Verwendung von Azure die `Login-AzureRmAccount` -Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Sie müssen Sie sich in mit `Login-AzureRmAccount` bevor Azure PowerShell-Cmdlets ausgeführt werden kann.

## <a name="listing-the-azure-cdn-cmdlets"></a>Die Cmdlets Azure CDN auflisten

Sie können eine Liste der alle der Azure CDN Cmdlets mithilfe der `Get-Command` Cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Aufrufen der Hilfe

Anfordern von Hilfe mit dieser Cmdlets mithilfe der `Get-Help` Cmdlet.  `Get-Help`Verwendung und Syntax enthält, und zeigt optional Beispiele.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Eintrag vorhandene Azure CDN Profile

Die `Get-AzureRmCdnProfile` Cmdlet ohne Parameter werden alle vorhandenen CDN Profile abgerufen.

```powershell
Get-AzureRmCdnProfile
```

Diese Ausgabe kann zu Cmdlets für die Enumeration geleitet werden.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Sie können auch ein einzelnes Profil zurückgeben, indem Sie die Profil Name und Ressourcen die Gruppe angeben.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Es ist möglich, mehrere CDN Profile mit den gleichen Namen haben, solange sie in anderen Ressourcengruppen sind.  Auslassen der `ResourceGroupName` Parameter gibt alle Profile mit einem übereinstimmenden Namen.

## <a name="listing-existing-cdn-endpoints"></a>Eintrag vorhandene CDN Endpunkte

`Get-AzureRmCdnEndpoint`einen einzelnen Endpunkt oder die Endpunkte eines Profils abrufen können.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Erstellen von Profilen CDN und Endpunkte

`New-AzureRmCdnProfile`und `New-AzureRmCdnEndpoint` werden verwendet, um die CDN Profile und Endpunkte zu erstellen.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Überprüfen der Verfügbarkeit von Endpunkt Namen

`Get-AzureRmCdnEndpointNameAvailability`Gibt ein Objekt gibt an, ob der Endpunktname eines verfügbar ist.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Hinzufügen einer benutzerdefinierten Domänennamens

`New-AzureRmCdnCustomDomain`Fügt einen benutzerdefinierten Domänennamen zu einem vorhandenen Endpunkt an.

>[AZURE.IMPORTANT] Sie müssen den CNAME-Eintrag bei Ihrem DNS-Anbieter in [wie benutzerdefinierte Domäne zu Content Delivery Network (CDN) Endpunkt zuordnen](./cdn-map-content-to-custom-domain.md)beschriebenen einrichten.  Sie können die Zuordnung vor dem Ändern der Endpunkt mit testen `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Ändern von außen liegenden Tabellenblättern

`Set-AzureRmCdnEndpoint`Ändert einen vorhandenen Endpunkt an.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Bereinigen/Pre-loading CDN Anlagen

`Unpublish-AzureRmCdnEndpointContent`bereinigt Cache Posten, während `Publish-AzureRmCdnEndpointContent` vorab lädt Anlagen auf unterstützten Endpunkte.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Starten/Beenden CDN Endpunkte
`Start-AzureRmCdnEndpoint`und `Stop-AzureRmCdnEndpoint` zum Starten und beenden einzelne Endpunkte oder Gruppen von Endpunkten verwendet werden können.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Löschen von CDN Ressourcen

`Remove-AzureRmCdnProfile`und `Remove-AzureRmCdnEndpoint` zum Entfernen von Benutzerprofilen und Endpunkte verwendet werden können.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Automatisieren von Azure CDN mit [.NET](./cdn-app-dev-net.md) oder [Node.js](./cdn-app-dev-node.md).

Weitere Informationen zu CDN Features finden Sie unter [CDN Übersicht](./cdn-overview.md).


