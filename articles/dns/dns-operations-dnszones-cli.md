<properties
   pageTitle="Verwalten von DNS-Zonen mit CLI | Microsoft Azure"
   description="Sie können die DNS-Zonen mit Azure CLI verwalten. So aktualisieren, löschen und Erstellen von DNS-Zonen auf Azure DNS-Einträge"
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

# <a name="how-to-manage-dns-zones-using-cli"></a>Zum Verwalten von DNS-Zonen mit CLI

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)


Mit diesem Leitfaden wird zum Verwalten Ihrer DNS-Zonenressourcen mithilfe der Plattformen Azure CLI angezeigt.

Verwenden Sie diese Anweisungen in Microsoft Azure CLI aus. Achten Sie auf die neueste Azure CLI aktualisieren (0.9.8 oder höher), die Azure DNS-Befehle verwenden. Typ `azure -v` zu überprüfen, welche Version Azure CLI derzeit in Ihrem Computer installiert ist. Sie können Azure CLI für Windows, Mac oder Linux installieren. Weitere Informationen finden in [der Azure CLI installieren](../xplat-cli-install.md).

Azure DNS wird eine Azure Ressourcenmanager nur. Es verfügt nicht über eine ASM-API. Sie benötigen, um sicherzustellen, dass die CLI Azure Ressourcenmanager-Modus verwenden konfiguriert ist. Sie können dazu mithilfe der `azure config mode arm` Befehl.<BR>
Wenn Sie die Meldung angezeigt wird "*Fehler: 'Dns' ist nicht mit einer Azure Befehl*", es ist sehr wahrscheinlich, weil Sie Azure CLI im ASM-Modus nicht Ressourcenmanager-Modus verwenden.

## <a name="create-a-new-dns-zone"></a>Erstellen einer neuen DNS zone

Zum Erstellen einer neuen DNS Zone Host für Ihre Domäne finden Sie unter [Erstellen einer Azure DNS Zone über die Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md).

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-zone

Verwenden Sie zum Abrufen einer DNS-Zone `azure network dns zone show`:

    azure network dns zone show myresourcegroup contoso.com

Der Vorgang gibt eine DNS-Zone mit seinen-Id, die Anzahl der Datensätze und Tags.


## <a name="list-dns-zones"></a>Liste DNS-Zonen

Verwenden Sie zum Abrufen von DNS-Zonen innerhalb einer Ressourcengruppe `azure network dns zone list`.

    azure network dns zone list myresourcegroup

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-zone

Änderungen an einer DNS Zone Ressource mit vorgenommen werden können `azure network dns zone set`. Dies wird nicht aktualisiert keines der DNS-Datensätze in der Zone (finden Sie unter [Verwalten von DNS-Einträge](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zone Ressource selbst zu aktualisieren. Dies ist derzeit in der Azure Ressourcenmanager 'Kategorien' für die Ressource Zone beschränkt. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

    azure network dns zone set myresourcegroup contoso.com -t prod=value2

## <a name="delete-a-dns-zone"></a>Löschen einer DNS Zone

DNS-Zonen mit gelöscht `azure network dns zone delete`. Dieser Vorgang weist eine optionale *f -* Wechseln der unterdrückt aufgefordert werden, zu bestätigen, dass Sie die DNS-Zone entfernen möchten.

Vor dem Löschen einer DNS-Zone in Azure DNS, müssen Sie alle Einträge Sätze, eine Ausnahme bilden jedoch die NS und SOA Einträge im Stammverzeichnis der Zone zu löschen, die automatisch erstellt wurden, wenn die Zone erstellt wurde.

    azure network dns zone delete myresourcegroup contoso.com



## <a name="next-steps"></a>Nächste Schritte
Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensatz Sätze und Datensätze](dns-getstarted-create-recordset-cli.md) zum Auflösen von Namen für Ihre Domäne Internet starten aus.
