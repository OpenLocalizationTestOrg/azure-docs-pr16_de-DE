<properties
   pageTitle="Verwalten von DNS-Zonen mithilfe der PowerShell | Microsoft Azure"
   description="Sie können die DNS-Zonen mithilfe der Powershell Azure verwalten. So aktualisieren, löschen und Erstellen von DNS-Zonen auf Azure DNS-Einträge"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="how-to-manage-dns-zones-using-powershell"></a>Zum Verwalten von DNS-Zonen mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



In diesem Artikel zeigt Ihnen, wie Sie Ihre DNS-Zone mithilfe der PowerShell verwalten. Um diese Schritte ausführen, müssen Sie die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets (1.0 oder höher) installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .


## <a name="create-a-new-dns-zone"></a>Erstellen einer neuen DNS zone

Zum Erstellen einer DNS-Zone finden Sie unter [Erstellen eines DNS-Zone mithilfe der PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-zone

Zum Abrufen einer DNS-Zone verwenden Sie die `Get-AzureRmDnsZone` Cmdlet. Dieser Vorgang gibt ein entsprechendes einer vorhandenen Zone in Azure DNS DNS Zone Objekt. Das Objekt enthält Daten über die Zone (beispielsweise die Anzahl der Datensätze), aber keine die Eintrag Sätzen selbst enthalten.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Liste DNS-Zonen

Durch das Auslassen der Zonenname `Get-AzureRmDnsZone`, Sie können alle Zonen in einer Ressourcengruppe auflisten. Dieser Vorgang gibt ein Array von Objekten Zone an.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-zone

Änderungen an einer DNS Zone Ressource vorgenommen werden können, mithilfe von `Set-AzureRmDnsZone`. Dies wird nicht aktualisiert keines der DNS-Datensätze in der Zone (finden Sie unter [Verwalten von DNS-Einträge](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zone Ressource selbst zu aktualisieren. Dies ist derzeit in der Azure Ressourcenmanager 'Kategorien' für die Ressource Zone beschränkt. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Verwenden Sie eine der folgenden zwei Methoden zum Aktualisieren von DNS-Zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Geben Sie die Zone mithilfe der Zone Name und Ressourcen-Gruppe

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Geben Sie die Zone mithilfe eines Objekts $zone

Geben Sie die Zone mithilfe eines Objekts $zone von `Get-AzureRmDnsZone`. Bei Verwendung von `Set-AzureRmDnsZone` mit einem $zone-Objekt die Etag Prüfungen verwendet werden, um sicherzustellen, dass gleichzeitige Änderungen werden nicht überschrieben. Sie können das optionale *-Überschreiben* wechseln, wird diese überprüft. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Löschen einer DNS Zone

DNS-Zonen können mithilfe des Cmdlets entfernen-AzureRmDnsZone gelöscht werden.

Vor dem Löschen einer DNS-Zone in Azure DNS, müssen Sie alle Einträge Sätze, eine Ausnahme bilden jedoch die NS und SOA Einträge im Stammverzeichnis der Zone zu löschen, die automatisch erstellt wurden, wenn die Zone erstellt wurde.

Verwenden Sie eine der folgenden zwei Methoden zum Entfernen einer DNS-Zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geben Sie die Zone mithilfe der Zone und Ressourcengruppennamen

Dieser Vorgang weist eine optionale *-Force* Wechseln der unterdrückt aufgefordert werden, bestätigen Sie die DNS-Zone entfernen möchten.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Geben Sie die Zone mithilfe eines Objekts $zone

Geben Sie die Zone mithilfe eines Objekts $zone von `Get-AzureRmDnsZone`. Dieser Vorgang weist eine optionale *-Force* Wechseln der unterdrückt aufgefordert werden, bestätigen Sie die DNS-Zone entfernen möchten. Wie bei `Set-AzureRmDnsZone`, die Zone unter Verwendung eines Objekts $zone angeben ermöglicht Etag überprüft, um sicherzustellen, dass gleichzeitige Änderungen werden nicht gelöscht. <BR>
Das optionale *-Überschreiben* Kennzeichnung unterdrückt diese überprüft. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Das Objekt Zone kann auch als Parameter übergebene geleitet werden:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensatz Sätze und Datensätze](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Domäne Internet starten aus.