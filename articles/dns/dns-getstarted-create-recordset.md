<properties
   pageTitle="Erstellen einer Datensatzgruppe und Einträge für eine DNS-Zone mithilfe der PowerShell | Microsoft Azure"
   description="Informationen zum Erstellen von Hosteinträge für Azure DNS. Einrichten von Datensatz legt und Einträge mithilfe der PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Erstellen von DNS-Datensätze und Datensätze mithilfe der PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

In diesem Artikel führt Sie durch die Vorgehensweise zum Erstellen von Datensätzen und Einträge Datensätze mithilfe von Windows PowerShell. Nach dem Erstellen der DNS-Zone an, fügen Sie die DNS-Einträge für Ihre Domäne aus. Dazu müssen Sie zuerst DNS-Einträge und Datensätze zu verstehen.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Stellen Sie sicher, dass Sie die neueste Version von PowerShell haben

Stellen Sie sicher, dass Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets installiert haben. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-a-record-set-and-record"></a>Erstellen Sie einen Datensatz festlegen und Datensatz

In diesem Abschnitt beschrieben, wie eine Anzahl von Datensätzen und Datensatz erstellen.


### <a name="1-connect-to-your-subscription"></a>1. Herstellen einer Verbindung mit Ihrem Abonnement

Öffnen Sie in der PowerShell-Konsole und eine Verbindung mit Ihrem Konto herstellen. Verwenden Sie im folgende Beispiel, damit Sie eine Verbindung herstellen können:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

Geben Sie das Abonnement, das Sie verwenden möchten.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Weitere Informationen zum Arbeiten mit PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Erstellen einer Datensatzgruppe

Erstellen Sie Datensätze mithilfe der `New-AzureRmDnsRecordSet` Cmdlet. Beim Erstellen einer müssen Sie angeben, dass der Eintrag Name, der Zone, die Time to live (TTL) und dem Datensatztyp festgelegt.

Zum Erstellen eines Datensatzes festlegen in der Spitze der Zone (in diesem Fall "contoso.com"), verwenden Sie den Namen des Datensatzes "@", einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Messe.

Im folgenden Beispiel wird einen Datensatz mit dem relativ Namen "Www" in der DNS-Zone "contoso.com" festlegen. Der vollqualifizierten Namen der Datensatzgruppe ist "www.contoso.com". Der Datensatztyp ist "A", und die Gültigkeitsdauer ist 60 Sekunden. Nach Abschluss dieses Schritts, müssen Sie eine leere "Www" Eintrag festlegen, die die Variable *$rs*zugewiesen ist.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Wenn bereits ein Eintrag festlegen vorhanden ist

Wenn Sie ein Eintrag festlegen bereits vorhanden ist, wird der Befehl fehlschlägt, es sei denn, die *-Überschreiben* Schalter verwendet wird. Die *-Überschreiben* Option löst eine Bestätigung gebeten werden, die mithilfe von unterdrückt werden kann die *-Force* wechseln.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


In diesem Beispiel geben Sie die Zone mithilfe der Zone und Ressourcengruppennamen ein. Alternativ können Sie ein Objekt Zone angeben, wie von zurückgegeben `Get-AzureRmDnsZone` oder `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Gibt ein lokales Objekt, das den Eintrag festlegen darstellt, der in Azure DNS erstellt wurde.

### <a name="3-add-a-record"></a>3 einen Datensatz hinzufügen

Wenn die neu erstellten "Www" Datensatzgruppe verwenden möchten, müssen Sie Datensätze hinzufügen. Sie können IPv4 hinzufügen *A* -Datensätze, die den Eintrag "Www" im folgenden Beispiel wird mit festlegen. In diesem Beispiel basiert auf der Variable *$rs* , die Sie im vorherigen Schritt festlegen.

Hinzufügen von Datensätzen mit einem Datensatz festlegen, indem `Add-AzureRmDnsRecordConfig` ist ein offline-Vorgang. Nur die lokale Variable *$rs* wird aktualisiert.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. die Änderungen übernehmen

Die Änderungen in der Datensatzgruppe zu übernehmen. Verwenden Sie `Set-AzureRmDnsRecordSet` können Sie die Änderungen auf den Eintrag festlegen bei Azure DNS-hochladen.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. Abrufen der Datensatzgruppe

Sie können den Eintrag mithilfe von Azure DNS festlegen abrufen `Get-AzureRmDnsRecordSet` wie im folgenden Beispiel dargestellt.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Das Nslookup-Tool oder andere DNS-Tools können Sie auch den neuen Datensatz Satz Abfragen.

Wenn Sie noch nicht die Domäne auf die Azure DNS-Namenserver delegiert haben, müssen Sie den Namen, den Server und die Adresse für Ihre Zone explizit angeben.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Erstellen einer Datensatzgruppe jedes Typs mit einem einzelnen Datensatz


So erstellen Sie einen Eintrag Festlegen der einzelnen Datensatztypen Sie in den folgenden Beispielen. Jeder Datensatzgruppe enthält einen einzelnen Datensatz.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Nächste Schritte

[Zum Verwalten von DNS-Zonen mithilfe der PowerShell](dns-operations-dnszones.md)

[Verwalten von DNS-Einträge und Datensätze mithilfe der PowerShell](dns-operations-recordsets.md)

[Automatisieren von Azure Vorgänge mit .NET SDK](dns-sdk.md)
