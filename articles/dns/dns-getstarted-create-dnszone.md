<properties
   pageTitle="Erste Schritte mit Azure DNS | Microsoft Azure"
   description="Informationen Sie zum Erstellen von DNS-Zonen für Azure DNS. Dies ist eine schrittweise Anleitung zum Abrufen Ihrer ersten DNS Zone erstellt haben, um Ihre DNS-Domäne mithilfe der PowerShell Hostinganbieter zu starten."
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

# <a name="create-a-dns-zone-using-powershell"></a>Erstellen einer DNS-Zone mithilfe der Powershell

> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

In diesem Artikel führt Sie durch die Schritte zum Erstellen einer DNS Zone mithilfe der PowerShell. Sie können auch eine DNS-Zone mithilfe von CLI oder Azure-Portal erstellen.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="a-nametagetagaabout-etags-and-tags"></a><a name="tagetag"></a>Informationen zu Etags und tags

### <a name="a-nameetagsaetags"></a><a name="etags"></a>Etags

Angenommen, zwei Personen oder zwei Prozesse versuchen, einen DNS-Eintrag gleichzeitig ändern. Welche eine wins? Und der Gewinner weiß, dass er nur Änderungen erstellt von einer anderen Person überschrieben haben?

Azure DNS verwendet Etags gleichzeitige Änderungen an der gleichen Ressource sicheres behandeln. Jede DNS-Ressource (Zone oder Datensatzgruppe) verfügt über eine Etag zugeordnet. Sobald eine Ressource abgerufen wird, wird auch seine Etag abgerufen. Wenn Sie eine Ressource zu aktualisieren, müssen Sie die Option wieder dem Etag übergeben, damit Azure DNS-Einträge zu überprüfen, ob kann das Etag auf dem Server entspricht. Da das Etag erneut generiert wird jedes Update an eine Ressource ergibt, zeigt ein Konflikt Etag eine gleichzeitige Änderung aufgetreten ist. Etags werden auch beim Erstellen einer neuen Ressource verwendet, um sicherzustellen, dass die Ressource noch nicht vorhanden ist.

Standardmäßig verwendet Azure DNS-PowerShell Etags gleichzeitige Änderungen Zonen blockieren und Sätze aufzeichnen. Das optionale *-Überschreiben* wechseln zum Unterdrücken Etag Prüfungen verwendet werden können, in der Groß-/Kleinschreibung gleichzeitigen Änderungen, die aufgetreten sind überschrieben werden.

Auf oberster Ebene der restlichen DNS Azure-API werden Etags über HTTP-Header angegeben.  Ihr Verhalten wird in der folgenden Tabelle angegeben:

|Kopfzeile|Verhalten|
|------|--------|
|Keine|SICH erfolgreich ist immer (keine Etag überprüft)|
|If-match<etag>|SICH funktioniert nur, wenn Ressourcen vorhanden ist und Etag entspricht.|
|If-Match *     | SICH funktioniert nur, wenn Ressourcen vorhanden ist.|
|If-keine-Match * |  SICH funktioniert nur, wenn die Ressource nicht vorhanden ist|

### <a name="a-nametagsatags"></a><a name="tags"></a>Kategorien

Kategorien unterscheiden sich von Etags. Kategorien sind eine Liste von Name-Wert-Paare und Rechnungsadresse oder Gruppierungsebene Zwecken von Azure Ressourcenmanager zu Bezeichnung Ressourcen verwendet. Weitere Informationen zu Kategorien finden Sie unter [Verwenden von Kategorien, um Ihre Azure Ressourcen zu organisieren](../resource-group-using-tags.md).

Azure DNS-PowerShell unterstützt Kategorien auf Zonen und Datensätze angegeben haben, verwenden die Optionen `-Tag` Parameter.


## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie mit die Konfiguration beginnen.

- Ein Azure-Abonnement. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.

- Sie müssen die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets (1.0 oder höher) installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="step-1---sign-in"></a>Schritt 1: anmelden

Öffnen Sie in der PowerShell-Konsole und eine Verbindung mit Ihrem Konto herstellen. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Ressourcenmanager](../powershell-azure-resource-manager.md).

Verwenden Sie im folgende Beispiel, damit Sie eine Verbindung herstellen können:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

Geben Sie das Abonnement, das Sie verwenden möchten.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Schritt 2 – Erstellen einer Ressourcengruppe

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen globale, nicht regionalen sind, hat die Wahl der Speicherort der Ressource Gruppe jedoch keine Auswirkung auf Azure DNS.

Wenn Sie eine vorhandene Ressourcengruppe verwenden, können Sie diesen Schritt überspringen.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Schritt 3: Register

Der Azure DNS-Dienst wird von den Microsoft.Network Ressourcenanbieter verwaltet. Ihr Abonnement Azure muss registriert sein, um diese Ressourcenanbieter verwenden, bevor Sie Azure DNS verwenden können. Dies ist eine einmaligen Vorgang für jedes Abonnement.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Schritt 4: Erstellen einer DNS-zone

Eine DNS-Zone wird erstellt, mit der `New-AzureRmDnsZone` Cmdlet. Es gibt Beispiele unter zum Erstellen einer DNS-Zone, mit oder ohne Tags ein. Weitere Informationen zu Kategorien finden Sie im Abschnitt auf [Kategorien](#tags) in diesem Artikel.

>[AZURE.NOTE] In Azure-DNS Zonennamen angegeben werden sollte, ohne dass eine beendet **'.'**. In diesem Fall als "**contoso.com**" statt "**contoso.com.**'.

### <a name="to-create-a-dns-zone"></a>Zum Erstellen einer DNS zone

Im folgenden Beispiel wird eine DNS-Zone Namen *"contoso.com"* in der Ressourcengruppe aufgerufen *MyResourceGroup*erstellt. Erstellen einer DNS Zone, ersetzen die Werte für Ihren eigenen anhand des Beispiels.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>So erstellen Sie eine DNS-Zone mit Kategorien

Im folgenden Beispiel wird veranschaulicht, wie das Erstellen einer DNS Zone mit zwei Kategorien, *Project Demo =* und *Env = Test*. Erstellen einer DNS Zone, ersetzen die Werte für Ihren eigenen anhand des Beispiels.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Anzeigen von Datensätzen

Erstellen einer DNS-Zone erstellt auch die folgenden DNS-Einträge:

- Der Datensatz *Start of Authority* (SOA). Dies ist im Stammverzeichnis jeder DNS Zone präsentieren.

- Die autorisierende (NS) Namenservereinträge. Diese anzeigen, welche Namenserver die Zone gehostet werden. Azure DNS verwendet einen Pool von Namenserver, und so unterschiedlich Namenserver können für verschiedene Zonen in Azure DNS zugewiesen werden. [Delegieren eine Domäne bei Azure DNS-](dns-domain-delegation.md) Informationen finden Sie unter.

Verwenden Sie zum Anzeigen dieser Datensätze `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Eintrag legt am Stamm (oder *Spitze*) eine Bedienung DNS Zone **@** als der Eintrag Name fest.


## <a name="test"></a>Test

Sie können Ihre DNS-Zone mithilfe der DNS-Tools, wie z. B. Nslookup, der Vorgang oder das [Auflösen-DNS-Name PowerShell-Cmdlet](https://technet.microsoft.com/library/jj590781.aspx)testen.

Wenn Sie Ihre Domäne für die neue Zone in Azure DNS verwenden noch delegiert nicht geschehen ist, müssen Sie die DNS-Abfrage direkt auf eine der Namenserver für Ihre Zone direkte. Die Namenserver für Ihre Zone erhalten in die NS-Einträge aufgeführt durch `Get-AzureRmDnsRecordSet` über. Stellen Sie sicher das Wechseln der richtigen Werte für Ihre Zone in den folgenden Befehl.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensatz Sätze und Datensätze](dns-getstarted-create-recordset.md) zum Auflösen von Namen für Ihre Domäne Internet starten aus.

