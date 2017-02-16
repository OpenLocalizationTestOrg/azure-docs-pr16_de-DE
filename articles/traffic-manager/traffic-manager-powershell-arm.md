<properties
    pageTitle="Azure Ressourcenmanager Support für den Datenverkehr Manager | Microsoft Azure "
    description="Mithilfe der PowerShell für den Datenverkehr Manager mit Azure Ressourcenmanager"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure Ressourcenmanager Unterstützung für Azure Datenverkehr Manager

Ressourcenmanager Azure ist die bevorzugte Management-Benutzeroberfläche für Azure-Dienste. Azure Datenverkehr-Manager-Profile können mit Ressourcenmanager Azure-basierten APIs und Tools verwaltet werden.

## <a name="resource-model"></a>Ressourcenmodell

Azure Datenverkehr-Manager ist eine Zusammenstellung von Einstellungen ein Profils Datenverkehr Manager aufgerufen mit konfiguriert. Dieses Profil enthält die DNS-Einstellungen, Einstellungen für die Weiterleitung von Datenverkehr, Überwachung Einstellungen Endpunkt und eine Liste der Endpunkte, die an denen Datenverkehr weitergeleitet wird.

Jedes Profil Datenverkehr Manager wird durch eine Ressource der Art 'TrafficManagerProfiles' dargestellt. Ebene REST-API liegt der URI für jedes Profil wie folgt aus:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Vergleich mit der klassischen Azure Datenverkehr Manager-API

Die Ressourcenmanager Azure-Unterstützung für den Datenverkehr Manager verwendet andere Terminologie als das Bereitstellungsmodell klassischen. Die folgende Tabelle zeigt die Unterschiede zwischen der Ressourcenmanager und klassischen Ausdrücke:

| Ressourcenmanager Ausdruck | Klassische Ausdruck |
|-----------------------|--------------|
| Datenverkehr-routing Methode | Lastenausgleich Methode |
| Priorität-Methode | Failover-Methode |
| Gewichteten Methode | Round-Robert-Methode |
| Leistung-Methode | Leistung-Methode |

Basierend auf Feedback von Kunden, geändert wir die Terminologie zum Verbessern der übersichtlich und häufige Missverständnisse zu verringern. Es gibt keinen Unterschied zwischen Funktionalität.

## <a name="limitations"></a>Einschränkungen

Zum Verweisen auf einen Endpunkt vom Typ 'AzureEndpoints' für eine Web App können Datenverkehr Manager Endpunkte nur die Standardeinstellung (Herstellung) [Web App Slot](../app-service-web/web-sites-staged-publishing.md)verwiesen werden. Benutzerdefinierte Steckplätze werden nicht unterstützt. Problem zu umgehen können benutzerdefinierte Steckplätze unter Verwendung des Typs 'ExternalEndpoints' konfiguriert sein.

## <a name="setting-up-azure-powershell"></a>Einrichten von Azure PowerShell

Verwenden Sie diese Anweisungen in Microsoft Azure PowerShell aus. Im folgende Artikel wird erläutert, wie installieren und Konfigurieren von Azure PowerShell.

- [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md)

Die Beispiele in diesem Artikel wird davon ausgegangen, dass Sie eine vorhandene Ressourcengruppe haben. Sie können eine Ressourcengruppe mit dem folgenden Befehl erstellen:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort verfügen. Dieser Speicherort ist für Ressourcen in dieser Ressourcengruppe erstellt als Standard verwendet. Jedoch, da Datenverkehr Manager Profilressourcen globale, nicht regionalen sind, wirkt sich die Wahl der Speicherort der Ressource Gruppe nicht auf Azure Datenverkehr Manager.

## <a name="create-a-traffic-manager-profile"></a>Erstellen eines Profils Datenverkehr-Manager

Verwenden Sie zum Erstellen eines Profils Datenverkehr-Managers des Cmdlets New-AzureRmTrafficManagerProfile ein:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Die folgende Tabelle beschreibt die Parameter:

| Parameter | Beschreibung |
|-----------|-------------|
| Namen | Der Name der Ressource für den Datenverkehr Manager Profil Ressource. Profile in derselben Ressourcengruppe müssen eindeutige Namen haben. Dieser Name wird unabhängig von den DNS-Namen für die DNS-Abfragen verwendet.|
| ResourceGroupName | Der Name der Ressourcengruppe mit der Ressource für das Profil.|
| TrafficRoutingMethod | Gibt die Datenverkehr-routing Methode verwendet, um festzustellen, welche Endpunkt eine DNS-Abfrage in der Antwort zurückgegeben wird. Mögliche Werte sind 'Leistung', 'Weighted' oder 'Priorität'.|
| RelativeDnsName | Gibt den Hostnamenteil der vom dieses Profil Datenverkehr Manager angegebene DNS-Name. Dieser Wert wird mit der DNS-Domänenname verwendet von Azure Datenverkehr Manager bilden Sie den vollqualifizierten Domänennamen (FQDN) des Profils kombiniert. Beispielsweise wird den Wert von 'Contoso' festlegen 'contoso.trafficmanager.net'.|
| TTL | Gibt die DNS-Time-to-Live (TTL), in Sekunden an. Diese TTL informiert die lokale DNS-Server und DNS-Clients wie lange Cache DNS-Antworten für dieses Profil Datenverkehr-Manager.|
| MonitorProtocol | Gibt das Protokoll an, mit der Endpunkt Systemzustand zu überwachen. Mögliche Werte sind "HTTP" und "HTTPS".|
| MonitorPort | Gibt den TCP-Anschluss verwendet, um den Endpunkt Systemzustand zu überwachen.|
| MonitorPath | Gibt den Pfad relativ zu den Endpunkt Domänennamen verwendet, um für die Suche nach Endpunkt Dienststatus.|

Das Cmdlet Datenverkehr Manager Profil in Azure erstellt und ein entsprechendes Profilobjekt zu PowerShell gibt. An diesem Punkt enthält das Profil keine Endpunkte. Weitere Informationen zum Hinzufügen von Endpunkten zu einem Profil Datenverkehr Manager finden Sie unter [Den Datenverkehr Manager Endpunkte hinzufügen](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Abrufen eines Profils Datenverkehr-Managers

Um ein vorhandenes Datenverkehr Manager Profilobjekt abrufen möchten, verwenden Sie das Cmdlet "Get-AzureRmTrafficManagerProfle":

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Dieses Cmdlet gibt ein Datenverkehr Manager Profilobjekt.

## <a name="update-a-traffic-manager-profile"></a>Aktualisieren eines Profils Datenverkehr-Managers

Ändern den Datenverkehr-Manager-Profilen umfasst einen 3-Schritt-Prozess:

1. Abzurufen Sie das Profil mit Get-AzureRmTrafficManagerProfile, oder verwenden Sie das Profil neu-AzureRmTrafficManagerProfile zurückgegebene.
2. Ändern Sie das Profil ein. Hinzufügen und Entfernen von Endpunkten, oder Endpunkt oder Profil Parameter ändern. Diese Änderungen sind offline Vorgänge. Sie sind nur das lokale Objekt im Arbeitsspeicher ändern, die das Profil darstellt.
3. Übernehmen Sie die Änderungen mithilfe des Cmdlets Set-AzureRmTrafficManagerProfile.

Alle Profileigenschaften können mit Ausnahme des Profils RelativeDnsName geändert werden. Um die RelativeDnsName zu ändern, müssen Sie das Profil und ein neues Profil unter einem neuen Namen löschen.

Im folgenden Beispiel wird veranschaulicht, wie des Profils TTL zu ändern:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Es gibt drei Arten von Datenverkehr Manager Endpunkte aus:

1. **Azure Endpunkte** sind in Azure gehostete Dienste
2. **Externe Endpunkte** sind außerhalb Azure gehostete Dienste
3. **Geschachtelt Endpunkte** werden verwendet, um geschachtelte Hierarchien von Profilen Datenverkehr-Manager zu erstellen. Geschachtelte Endpunkte aktivieren erweiterte Datenverkehr-routing Konfigurationen für komplexe Applikationen.

In allen drei Fällen können die Endpunkte auf zwei Arten hinzugefügt werden:

1. Verwenden die oben beschriebenen Schritte aus einer 3-Schritt. Der Vorteil dieser Methode ist, dass mehrere Endpunkt in ein einzelnes Update geändert werden kann.
2. Verwenden das Cmdlet "New-AzureRmTrafficManagerEndpoint" ein. Dieses Cmdlet hinzugefügt ein vorhandenes Datenverkehr-Manager-Profil in einem einzigen Vorgang einen Endpunkt.

## <a name="adding-azure-endpoints"></a>Hinzufügen von Azure Endpunkten

Azure Endpunkte in Azure gehostete Dienste verwiesen werden. Drei Arten von Azure Endpunkte werden unterstützt:

1. Azure Web Apps
2. 'Klassisch' cloud Services (die entweder eines PaaS-Diensts oder IaaS virtuellen Computern enthalten können)
3. Azure Öffentl.IP-Ressourcen (d. h. ein Lastenausgleich oder eines virtuellen Computers NIC angefügt werden können). Die Öffentl.IP müssen einen DNS-Namen zugewiesen, in den Datenverkehr Manager verwendet werden.

In jedem Fall:

- Der Dienst wird mit dem Parameter 'TargetResourceId' Add-AzureRmTrafficManagerEndpointConfig oder neu-AzureRmTrafficManagerEndpoint angegeben.
- Die 'Target' und 'EndpointLocation' werden von den TargetResourceId impliziert.
- Angeben die 'Gewichtung' ist optional. -Stärken werden nur verwendet, wenn das Profil für die Verwendung der 'Weighted' Datenverkehr-routing Methode konfiguriert ist. Andernfalls werden sie ignoriert. Wenn angegeben, muss der Wert eine Zahl zwischen 1 und 1000. Der Standardwert ist "1".
- Angeben die 'Priorität' ist optional. Prioritäten werden nur verwendet, wenn das Profil für die Verwendung der 'Priorität' Datenverkehr-routing Methode konfiguriert ist. Andernfalls werden sie ignoriert. Gültige Werte liegen zwischen 1 und 1000 mit niedrigere Werte, die eine hohe Priorität angibt. Wenn für einen Endpunkt angegeben haben, müssen sie für alle Endpunkte angegeben werden. Wenn nicht angegeben, werden Standardwerte beginnend von "1" in der Reihenfolge angewendet, die Endpunkte aufgelistet sind.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Beispiel 1: Hinzufügen von Web App Endpunkte hinzufügen-AzureRmTrafficManagerEndpointConfig verwenden

In diesem Beispiel werden Erstellen eines Profils Datenverkehr Manager und Hinzufügen von zwei Web App Endpunkten mithilfe des Cmdlets hinzufügen-AzureRmTrafficManagerEndpointConfig.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 2: Hinzufügen eines 'Klassisch' Cloud-Endpunkts mit neu-AzureRmTrafficManagerEndpoint

In diesem Beispiel wird ein 'klassischer' Cloud-Dienst Endpunkt eines Profils Datenverkehr Manager hinzugefügt. In diesem Beispiel angegebenen wir das Profil die Gruppennamen Profil- und Ressourcen verwenden, anstatt ein Profilobjekt zu übergeben. Beide Methoden werden unterstützt.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 3: Hinzufügen eines Öffentl.IP Endpunkts neu-AzureRmTrafficManagerEndpoint verwenden

In diesem Beispiel wird eine öffentliche IP-Adressenressource den Datenverkehr-Manager-Profil hinzugefügt. Die öffentliche IP-Adresse muss einen DNS-Namen konfiguriert haben, und die NIC eines virtuellen Computers oder ein Lastenausgleich gebunden werden kann.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Hinzufügen von externen Endpunkten

Datenverkehr Manager verwendet externe Endpunkte direkte Datenverkehr zu Diensten, die außerhalb von Azure gehostet. Wie bei Azure Endpunkte, können externe Endpunkte entweder mithilfe der hinzufügen-AzureRmTrafficManagerEndpointConfig gefolgt von Set-AzureRmTrafficManagerProfile oder neu-AzureRMTrafficManagerEndpoint hinzugefügt werden.

Wenn Sie externe Endpunkte angeben:

- Der Endpunkt Domänenname muss unter Verwendung des Parameters "Ziel" angegeben werden
- Wenn die "Leistung" Datenverkehr-routing Methode verwendet wird, ist die 'EndpointLocation' erforderlich. Andernfalls ist optional. Der Wert muss einen [gültigen Azure Regionsname](https://azure.microsoft.com/regions/)sein.
- Die 'Gewicht' und 'Priorität' sind optional.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Beispiel 1: Hinzufügen von externen Endpunkte mithilfe von hinzufügen-AzureRmTrafficManagerEndpointConfig und Set-AzureRmTrafficManagerProfile

In diesem Beispiel erstellen Sie ein Profil Datenverkehr Manager wir, zwei externe Endpunkte hinzufügen, und die Änderungen übernehmen.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 2: Hinzufügen von externen Endpunkte mithilfe von neu-AzureRmTrafficManagerEndpoint

In diesem Beispiel hinzufügen wir einen externen Endpunkt zu einem bestehenden Profil. Verwenden die Profil- und Ressourcen Gruppennamen das Profil angegeben.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>Hinzufügen von Endpunkten 'Geschachtelt'

Jeder Datenverkehr-Manager-Profil gibt eine einzelne Datenverkehr-routing Methode an. Es gibt jedoch Szenarien, die erfordern komplexere Datenverkehr routing als das routing von einem einzigen Datenverkehr Manager Profil zur Verfügung gestellt. Sie können den Datenverkehr Manager-Profile, um die Vorteile von mehreren Datenverkehr-routing Methode kombinieren schachteln. Geschachtelte Profilen können Sie das Standardverhalten der Datenverkehr Manager unterstützt größere und komplexere Anwendung Bereitstellungen außer Kraft setzen. Weitere ausführliche Beispiele finden Sie unter [Profile geschachtelt Datenverkehr Manager](traffic-manager-nested-profiles.md).

Geschachtelte Endpunkte werden in der übergeordneten Profil, das mit einer bestimmten Endpunkt 'NestedEndpoints' konfiguriert. Wenn Sie geschachtelte Endpunkte angeben:

- Der Endpunkt muss mit dem Parameter 'TargetResourceId' angegeben werden muss
- Wenn die "Leistung" Datenverkehr-routing Methode verwendet wird, ist die 'EndpointLocation' erforderlich. Andernfalls ist optional. Der Wert muss einen [gültigen Azure Regionsname](http://azure.microsoft.com/regions/)sein.
- Die 'Gewicht' und 'Priorität' sind optional, wie bei Azure Endpunkte.
- Der Parameter 'MinChildEndpoints' ist optional. Der Standardwert ist "1". Wenn die Anzahl der verfügbaren Endpunkte unter diesem Schwellenwert fällt, betrachtet das übergeordnete Profil auf, das Profil der untergeordneten 'Heruntergestuft' und leitet den Datenverkehr an die anderen Endpunkte in der übergeordneten Profil.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Beispiel 1: Hinzufügen von verschachtelten Endpunkte hinzufügen-AzureRmTrafficManagerEndpointConfig und Set-AzureRmTrafficManagerProfile verwenden

In diesem Beispiel neue Datenverkehr Manager untergeordneten und übergeordneten Profile erstellen wir, die untergeordneten wie eine geschachtelte Endpunkt an das übergeordnete Element hinzufügen, und die Änderungen übernehmen.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

In diesem Beispiel aus Platzgründen haben wir beliebigen anderen Endpunkten nicht auf die untergeordneten oder übergeordneten Profile hinzugefügt.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 2: Hinzufügen von verschachtelten Endpunkte mithilfe von neu-AzureRmTrafficManagerEndpoint

In diesem Beispiel hinzufügen wir ein vorhandenes untergeordneten Profil wie eine geschachtelte Endpunkt zu einem vorhandenen übergeordneten Profil ein. Verwenden die Profil- und Ressourcen Gruppennamen das Profil angegeben.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Aktualisieren Sie einen Endpunkt Datenverkehr-Manager

Es gibt zwei Methoden zum Aktualisieren eines vorhandenen Datenverkehr Manager Endpunkts aus:

1. Abrufen des Datenverkehr-Manager-Profils Get-AzureRmTrafficManagerProfile, aktualisieren Sie die Endpunkteigenschaften innerhalb des Profils und Set-AzureRmTrafficManagerProfile mit Änderungen übernehmen. Diese Methode hat den Vorteil, können mehr als einen Endpunkt in einem einzigen Vorgang zu aktualisieren.
2. Erhalten Sie den Datenverkehr Manager Endpunkt Get-AzureRmTrafficManagerEndpoint verwenden, aktualisieren Sie die Endpunkteigenschaften und übernehmen Sie Set-AzureRmTrafficManagerEndpoint mit Änderungen. Diese Methode ist einfacher, da es nicht erforderlich ist, in die Endpunkte Matrix im Profil Indizierung.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Beispiel 1: Aktualisieren Endpunkte mithilfe von Get-AzureRmTrafficManagerProfile und Set-AzureRmTrafficManagerProfile

In diesem Beispiel ändern wir die Priorität auf zwei Endpunkten in ein vorhandenes Profil aus.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Beispiel 2: Aktualisieren von außen liegenden Tabellenblättern mit Get-AzureRmTrafficManagerEndpoint und Set-AzureRmTrafficManagerEndpoint

In diesem Beispiel ändern wir die Stärke eines einzelnen Endpunkts in ein vorhandenes Profil aus.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Aktivieren und Deaktivieren von Endpunkten und Benutzerprofilen

Datenverkehr-Manager können einzelne Endpunkte aktiviert und deaktiviert, sowie gleicht aktivieren und Deaktivieren des gesamten Profile werden.
Diese Änderungen können durch Abrufen/aktualisieren/einrichten die Endpunkt oder Profil Ressourcen vorgenommen werden. Wenn Sie um diesen Vorgang allgemeine zu optimieren, werden sie auch über dedizierte Cmdlets unterstützt.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Beispiel 1: Aktivieren und deaktivieren ein Profil Datenverkehr-Manager

Verwenden Sie zum Aktivieren eines Profils Datenverkehr Manager aktivieren-AzureRmTrafficManagerProfile aus. Das Profil kann mithilfe eines Profilobjekts angegeben werden muss. Das Profilobjekt übergeben werden kann, über die Verkaufspipeline oder mithilfe der '-TrafficManagerProfile' Parameter. In diesem Beispiel geben wir das Profil der Gruppenname Profil- und Ressourcen aus.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

So deaktivieren Sie ein Profil Datenverkehr-Manager

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Das Deaktivieren-AzureRmTrafficManagerProfile-Cmdlet fordert zur Bestätigung auf. Diese Aufforderung kann mithilfe unterdrückt werden die '-Force' Parameter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Beispiel 2: Aktivieren und deaktivieren einen Endpunkt Datenverkehr-Manager

Verwenden Sie zum Aktivieren eines Endpunkts Datenverkehr Manager aktivieren-AzureRmTrafficManagerEndpoint aus. Es gibt zwei Möglichkeiten, um den Endpunkt angeben.

1. Verwendung einer TrafficManagerEndpoint über der Verkaufspipeline an, oder Verwenden der '-TrafficManagerEndpoint' Parameter
2. Verwenden den Endpunktnamen, Profilname und Endpunkttyp Ressourcengruppennamen ein:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Auf ähnliche Weise, um einen Endpunkt Datenverkehr-Manager zu deaktivieren:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Wie bei deaktivieren-AzureRmTrafficManagerProfile, fordert das Cmdlet deaktivieren-AzureRmTrafficManagerEndpoint zur Bestätigung ein. Diese Aufforderung kann mithilfe unterdrückt werden die '-Force' Parameter.

## <a name="delete-a-traffic-manager-endpoint"></a>Löschen Sie einen Endpunkt Datenverkehr-Manager

Verwenden Sie das Cmdlet AzureRmTrafficManagerEndpoint entfernen, um einzelne Endpunkte zu entfernen:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Dieses Cmdlet fordert zur Bestätigung auf. Diese Aufforderung kann mithilfe unterdrückt werden die '-Force' Parameter.

## <a name="delete-a-traffic-manager-profile"></a>Löschen eines Profils Datenverkehr-Managers

Verwenden Sie zum Löschen eines Profils Datenverkehr Manager entfernen-AzureRmTrafficManagerProfile Cmdlets, die Profil- und Ressourcen Gruppennamen angeben:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Dieses Cmdlet fordert zur Bestätigung auf. Diese Aufforderung kann mithilfe unterdrückt werden die '-Force' Parameter.

Das Profil zu löschenden kann auch mithilfe eines Profilobjekts angegeben werden:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Diese Sequenz kann auch geleitet werden:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Nächste Schritte

[Überwachen von Datenverkehr-Manager](traffic-manager-monitoring.md)

[Datenverkehr Manager Leistungsaspekte](traffic-manager-performance-considerations.md)
