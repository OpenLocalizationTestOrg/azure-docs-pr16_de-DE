<properties
   pageTitle="Verwalten von DNS-Datensätze und Einträge mithilfe des Azure-Portals | Microsoft Azure"
   description="Verwalten von DNS-Datensätze und Azure-DNS-Einträge aus, wenn Ihre Domäne auf Azure DNS-Hostinganbieter. Alle PowerShell-Befehle für Vorgänge für Datensätze und Einträge."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>Verwalten von DNS-Einträge und Datensätze mithilfe der PowerShell



> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



In diesem Artikel wird gezeigt, wie Datensätze und Einträge für Ihre DNS-Zone mithilfe von Windows PowerShell verwaltet werden können.

Es ist wichtig, den Unterschied zwischen DNS-Datensätze und einzelnen DNS-Einträge zu verstehen. Ein Datensatz ist die Sammlung von Datensätzen in einer Zone, die denselben Namen haben und den gleichen Typ. Weitere Informationen finden Sie unter [Erstellen von DNS-Datensätze und Einträge mithilfe des Azure-Portals](dns-getstarted-create-recordset-portal.md).

Zum Verwalten Ihrer Datensätze und Einträge benötigen Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets. Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zum Arbeiten mit PowerShell finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Erstellen einer neuen Datensatzgruppe und eines Datensatzes

Zum Erstellen eines Datensatzes festlegen, indem Sie mithilfe der PowerShell finden Sie unter [Erstellen von DNS-Datensätze und Datensätze mithilfe der PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Abrufen einer Datensatzgruppe

Um einen vorhandenen Datensatz Satz abzurufen, verwenden Sie `Get-AzureRmDnsRecordSet`. Geben Sie an, dass der Eintrag relative Namen, den Datensatztyp, und die Zone festgelegt.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Wie bei `New-AzureRmDnsRecordSet`, den Namen des Datensatzes muss ein relativer Name, d. h., sie müssen den Zonennamen ausschließen.

Sie können die Zone mithilfe von entweder die Zone und Ressourcengruppennamen oder mithilfe eines Objekts Zone angeben:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Gibt ein lokales Objekt, das den Eintrag festlegen darstellt, der in Azure DNS erstellt wurde.

## <a name="list-record-sets"></a>Liste Datensätze

Können Sie auch`Get-AzureRmDnsRecordSet` Liste Datensatz Gruppen fehlt der *– Namen* und/oder *– RecordType* Parameter.

### <a name="to-list-all-record-sets"></a>In der Liste alle Datensätze

In diesem Beispiel gibt alle Datensätze, unabhängig von den Namen oder Datensatztyp:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Aufzeichnen Liste Gruppen ein angegebenen Datensatztyp

In diesem Beispiel gibt alle Datensätze, die den angegebenen Datensatztyp entsprechen. In diesem Fall ist der Datensatzgruppe zurückgegeben "A" Einträge:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Die Zone kann mithilfe von entweder die *– ZoneName* und *– ResourceGroupName* Parameter (siehe) oder durch die Angabe einer Zone Object angegeben werden:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Hinzufügen eines Datensatzes zu einer Datensatzgruppe

Hinzufügen von Einträgen, um Datensätze mithilfe der `Add-AzureRmDnsRecordConfig` Cmdlet. Dies ist ein offline-Vorgang. Nur das lokale Objekt, das die Datensatzgruppe darstellt, wird geändert.

Der Parameter für das Hinzufügen von Datensätzen zu einer variieren je nach Typ der Datensatzgruppe. Beispielsweise, wenn Sie einen Satz Datensatz vom Typ "A" verwenden, Sie können nur angeben Datensätze mit dem Parameter *-IPv4Address*.

Zusätzliche Einträge hinzugefügt werden können, zu jedem Datensatz festlegen, indem Sie zusätzliche Anrufe an `Add-AzureRmDnsRecordConfig`. Sie können bis zu 20 Datensätze an eine beliebige Anzahl von Datensätzen hinzufügen. Jedoch Datensätze vom Typ "CNAME" können höchstens ein Datensatz enthalten, und eine Datensatzgruppe keine zwei identische Datensätze enthalten. Leere Datensätze (mit 0 (null) Records) erstellt werden können, aber nicht auf die Azure DNS-Namensserver angezeigt.

Nach dem Aufzeichnen festlegen die gewünschte Auflistung der Datensätze enthält, müssen Sie es mit commit der `Set-AzureRmDnsRecordSet` Cmdlet. Nachdem eine Datensatzgruppe zugesichert wurde, wird den vorhandenen Datensatz in Azure DNS festlegen ersetzt.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Zum Erstellen eines A-Datensatzes mit einem einzelnen Datensatz festlegen

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Vorgänge zum Erstellen eines Datensatzes kann auch erwiesen *geleitet*, was bedeutet, dass Sie das Objekt Datensatzgruppe Übergabe mithilfe der Pipe, anstatt sie als Parameter zu übergeben. Beispiel:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Zusätzliche Datensatztyp Beispiele

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Ändern der vorhandenen Datensatz-sets

Die Schritte zum Ändern eines vorhandenen Datensatz Satzes sind ähnlich wie mit den Schritten fort, die Sie beim Erstellen von Datensätzen nutzen. Die Reihenfolge der Operationen sieht wie folgt aus:

1.  Abrufen von den vorhandenen Datensatz mit festlegen `Get-AzureRmDnsRecordSet`.

2.  Ändern den Eintrag festlegen, indem Sie Datensätze hinzufügen, Entfernen von Einträgen, um die Datensatzparameter ändern, oder ändern den Eintrag Time to live (TTL) festlegen. Dies ist ein offline-Vorgang. Nur das lokale Objekt, das die Datensatzgruppe darstellt, wird geändert.

3.  Übernehmen Sie die Änderungen mithilfe der `Set-AzureRmDnsRecordSet` Cmdlet. Diese Option ersetzt den vorhandenen Datensatz in Azure DNS festlegen.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Zum Aktualisieren eines Datensatzes in einem vorhandenen Datensatz Satz

In diesem Beispiel ändern wir die IP-Adresse eines vorhandenen Eintrag "A":

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die `Set-AzureRmDnsRecordSet` -Cmdlet Etag Prüfungen verwendet, um sicherzustellen, dass gleichzeitige Änderungen nicht überschrieben werden. Verwenden der *-Überschreiben* Kennzeichnung diese überprüft wird. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Ändern eines Datensatzes SOA

Sie können keine Datensätze hinzufügen oder Entfernen von automatisch erstellt SOA-Eintrag in der Zone Spitze festlegen (Name = "@"). Allerdings können Sie jede der Parameter innerhalb des SOA-Eintrags (außer "Host") ändern, und legen Sie der Eintrag TTL.

Im folgenden Beispiel wird gezeigt, wie die *E-Mail* -Eigenschaft des SOA-Eintrags ändern:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>So ändern Sie die NS-Einträge in der Zone Spitze

Sie können nicht hinzufügen möchten, entfernen oder ändern Sie die Datensätze automatisch erstellt NS Datensatz festlegen in der Zone Spitze (Name = "@"). Die einzige Änderung, die darf besteht darin Datensatzgruppe TTL ändern.

Im folgenden Beispiel wird gezeigt, wie die TTL-Eigenschaft des Recordsets NS ändern:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Zum Hinzufügen von Datensätzen zu einer vorhandenen Datensatz festlegen

In diesem Beispiel fügen wir an den vorhandenen Datensatz zwei zusätzliche MX-Einträge hinzu:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Entfernen eines Datensatzes aus einer vorhandenen Datensatz festlegen

Datensätze können aus einem Datensatz festlegen, indem entfernt werden `Remove-AzureRmDnsRecordConfig`. Der Datensatz, der entfernt wird, muss eine genaue Übereinstimmung mit einem vorhandenen Datensatz über alle Parameter. Änderungen zugesichert werden müssen, mithilfe von `Set-AzureRmDnsRecordSet`.

Entfernen den letzten Datensatz in einem Datensatz Satz wird die Datensatzgruppe nicht gelöscht. Weitere Informationen finden Sie unter unter [Löschen eines Datensatzes festlegen](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Die Reihenfolge der Operationen zum Entfernen eines Datensatzes aus einer kann was bedeutet, dass Sie das Objekt Datensatzgruppe Übergabe Übergabe als Parameter, sondern verwenden die Pipe auch geleitet werden. Beispiel:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Entfernen von AAAA-Datensatz aus einer Datensatzgruppe

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Entfernen Sie einen CNAME-Eintrag aus einer Datensatzgruppe

Da Sie ein CNAME-Datensatz Satz höchstens ein Datensatz enthalten kann, belässt den Datensatz entfernen einer leeren Datensatzgruppe.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Entfernen eines MX-Eintrags aus einer Datensatzgruppe

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Entfernen Sie aus den Datensatz ein NS-Eintrag

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Einen SRV-Eintrag in einem Datensatz Satz entfernen

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Entfernen Sie einen TXT-Eintrag aus einer Datensatzgruppe

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Löschen einer Datensatzgruppe

Datensätze können gelöscht werden, mithilfe der `Remove-AzureRmDnsRecordSet` Cmdlet. Die SOA können Sie nicht löschen und NS-Eintrag in der Zone Spitze legt (Name = "@") , wurden automatisch erstellt, wenn die Zone erstellt wurde. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

Verwenden Sie eine der folgenden drei Methoden an, um einen Datensatz Satz entfernen:

### <a name="specify-all-the-parameters-by-name"></a>Geben Sie alle Parameter anhand des Namens

Das optionale *-Force* Schalter kann verwendet werden, wird die bestätigungsaufforderung angezeigt wird.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Legen Sie den Datensatz Satz nach Name und Typ aus, und geben Sie die Zone vom Objekt

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Geben Sie den Eintrag festlegen, indem Sie Objekt

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Wenn Sie den Eintrag festlegen, indem Sie ein Objekt angeben, ermöglicht es Etag überprüft, um sicherzustellen, dass gleichzeitige Änderungen nicht gelöscht werden. Das optionale *-Überschreiben* Kennzeichnung unterdrückt diese überprüft. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

Das Objekt Datensatzgruppe kann auch als Parameter übergebene geleitet werden:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Übersicht über Azure DNS](dns-overview.md). Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und Datensatz legt .NET SDK verwenden](dns-sdk.md).

Weitere Informationen zu reverse-DNS-Einträge finden Sie unter [Verwalten von reverse DNS-Einträge für Ihre Dienstleistungen mithilfe der PowerShell](dns-reverse-dns-record-operations-ps.md).
