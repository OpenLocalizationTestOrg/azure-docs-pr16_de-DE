<properties
   pageTitle="Verwalten von DNS-Datensätze und Datensätze auf Azure DNS-Einträge mit Azure CLI | Microsoft Azure"
   description="Verwalten von DNS-Datensätze und Azure-DNS-Einträge aus, wenn Ihre Domäne auf Azure DNS-Hostinganbieter. Alle CLI-Befehle für Vorgänge für Datensätze und Einträge."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Verwalten von DNS-Einträge und Datensätze mithilfe von CLI


> [AZURE.SELECTOR]
- [Azure-Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


In diesem Artikel wird gezeigt, wie Datensätze und Einträge für Ihre DNS-Zone mithilfe der Plattformen Azure line Interface (CLI) verwaltet werden können.

Es ist wichtig, den Unterschied zwischen DNS-Datensätze und einzelnen DNS-Einträge zu verstehen. Eine Datensatzgruppe ist eine Zusammenstellung von Datensätzen in einer Zone, die denselben Namen haben und den gleichen Typ. Weitere Informationen finden Sie unter [Grundlegendes zu Recordsets und Datensätze](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Konfigurieren der Plattformen Azure CLI

Azure DNS wird eine Azure Ressourcenmanager nur. Es verfügt nicht über eine Azure Service Management-API. Stellen Sie sicher, dass die CLI Azure Ressourcenmanager Modus mithilfe verwenden konfiguriert ist die `azure config mode arm` Befehl.

Wenn Sie sehen **Fehler: 'Dns' ist nicht mit einer Azure Befehl**, es ist sehr wahrscheinlich, weil Sie Azure CLI im Modus Azure Servicemanagement, Ressourcenmanager-Modus nicht verwenden.

## <a name="create-a-new-record-set-and-record"></a>Erstellen eines neuen Datensatzes festlegen und Datensatz

Zum Erstellen eines neuen Datensatzes Festlegen der Azure-Portal finden Sie unter [Erstellen eines Datensatzes festlegen und Einträge](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Abrufen einer Datensatzgruppe

Um einen vorhandenen Datensatz Satz abzurufen, verwenden Sie `azure network dns record-set show`. Geben Sie die Ressourcengruppe Zonennamen festlegen Datensatz relative Name und Datensatztyp. Verwenden Sie das Beispiel unten die Werte durch ein eigenes ersetzen.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Liste Datensätze

Sie können alle Datensätze in einer DNS Zone auflisten, indem Sie mit der `azure network dns record-set list` Befehl. Sie müssen die Gruppe Ressourcenname und Zonennamen angeben.

### <a name="to-list-all-record-sets"></a>In der Liste alle Datensätze

In diesem Beispiel gibt alle Datensätze, unabhängig von den Namen oder Datensatztyp:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>In der Liste Datensätze eines bestimmten Typs

In diesem Beispiel gibt alle Datensätze, die den angegebenen Datensatztyp (in diesem Fall "A" Datensätze) entsprechen:

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Hinzufügen eines Datensatzes zu einer Datensatzgruppe

Hinzufügen von Einträgen, um Datensätze mithilfe der `azure network dns record-set add-record`Befehl. Der Parameter für das Hinzufügen von Datensätzen zu einer variieren je nach Typ des Datensatzes, die festgelegt wird. Beispielsweise, wenn Sie einen Satz Datensatz vom Typ "A" verwenden, können Sie nur angeben Datensätze mit dem Parameter `-a <IPv4 address>`.

Verwenden Sie zum Erstellen einer der `azure network dns record-set create`Befehl. Geben Sie die Ressourcengruppe Zonennamen festlegen Eintrag relative Name, Datensatztyp und Zeit zu live (TTL). Wenn die `--ttl` Parameter nicht definiert ist, wird der Wert (in Sekunden) standardmäßig auf vier.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Nach dem Erstellen der "A" Datensatzgruppe hinzufügen die IPv4-Adresse mithilfe der `azure network dns record-set add-record`Befehl.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


So erstellen Sie einen Eintrag Festlegen der einzelnen Datensatztypen Sie in den folgenden Beispielen. Jeder Datensatzgruppe enthält einen einzelnen Datensatz.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Aktualisieren eines Datensatzes in einem Datensatz

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Legen Sie ("Www"), um eine andere IP-Adresse (1.2.3.4) zu einer vorhandenen Eintrag "A" hinzuzufügen:

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>So entfernen Sie einen vorhandenen Wert aus einer Datensatzgruppe
Verwenden Sie `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Entfernen eines Datensatzes aus einer Datensatzgruppe

Datensätze können aus einem Datensatz festlegen, indem entfernt werden `azure network dns record-set delete-record`. Der Datensatz, der entfernt wird, muss eine genaue Übereinstimmung mit einem vorhandenen Datensatz über alle Parameter.

Entfernen den letzten Datensatz in einem Datensatz Satz wird die Datensatzgruppe nicht gelöscht. Weitere Informationen finden Sie im Abschnitt dieses Artikels aufgerufen, [Löschen Sie einen Eintrag festlegen](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Entfernen von AAAA-Datensatz aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Entfernen Sie einen CNAME-Eintrag aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Entfernen eines MX-Eintrags aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Entfernen Sie aus den Datensatz ein NS-Eintrag

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Entfernen eines PTR-Eintrags aus einer Datensatzgruppe
In diesem Fall ' Meine-Arpa-Zone.com' ARPA Zeitzone darstellt IP-Bereichs darstellt.  Jede PTR-Eintrag festlegen in dieser Zone entspricht eine IP-Adresse in diesem Bereich IP.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Einen SRV-Eintrag in einem Datensatz Satz entfernen

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Entfernen Sie einen TXT-Eintrag aus einer Datensatzgruppe

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="a-namedeleteadelete-a-record-set"></a><a name="delete"></a>Löschen einer Datensatzgruppe

Datensätze können gelöscht werden, mithilfe der `Remove-AzureRmDnsRecordSet` Cmdlet. Die SOA können Sie nicht löschen und NS-Eintrag in der Zone Spitze legt (Name = "@") , wurden automatisch erstellt, wenn die Zone erstellt wurde. Sie werden automatisch gelöscht, wenn die Zone gelöscht wird.

Im folgenden Beispiel wird der "A" Datensatzgruppe "Test-a" aus der "contoso.com" DNS Zone entfernt werden:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Der optional *– f* -Schalter kann verwendet werden, wird die bestätigungsaufforderung angezeigt wird.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Übersicht über Azure DNS](dns-overview.md). Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und Datensatz legt .NET SDK verwenden](dns-sdk.md).

Wenn Sie mit reverse-DNS-Einträgen arbeiten möchten, finden Sie unter [reverse DNS-Einträge für Ihre Dienstleistungen mit der Azure CLI verwalten](dns-reverse-dns-record-operations-cli.md).
