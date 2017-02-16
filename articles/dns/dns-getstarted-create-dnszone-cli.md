<properties
   pageTitle="Erstellen eine DNS-Zone mit CLI | Microsoft Azure"
   description="Erfahren Sie, wie DNS-Zonen für Azure DNS schrittweise So starten Sie Ihre DNS-Domäne mit CLI Hostinganbieter erstellen"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Erstellen einer Azure DNS Zone mit CLI


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


In diesem Artikel führt Sie durch die Schritte zum Erstellen einer DNS Zone mithilfe von CLI. Sie können auch eine DNS-Zone mithilfe der PowerShell oder der Azure-Portal erstellen.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Vorbemerkung

Verwenden Sie diese Anweisungen in Microsoft Azure CLI aus. Achten Sie auf die neueste Azure CLI aktualisieren (0.9.8 oder höher), die Azure DNS-Befehle verwenden. Typ `azure -v` zu überprüfen, welche Version Azure CLI derzeit in Ihrem Computer installiert ist.

## <a name="step-1---set-up-azure-cli"></a>Schritt 1: Einrichten von Azure CLI

### <a name="1-install-azure-cli"></a>1. Azure CLI installieren

Sie können Azure CLI für Windows, Mac oder Linux installieren. Die folgenden Schritte müssen abgeschlossen sein, bevor Sie mit Azure CLI Azure-DNS verwalten können. Weitere Informationen finden in [der Azure CLI installieren](../xplat-cli-install.md). Die DNS-Befehle erfordern Azure CLI Version 0.9.8 oder höher.

Alle Befehle Netzwerk Anbieter auf CLI können mit dem folgenden Befehl gefunden werden kann:

    azure network

### <a name="2-switch-cli-mode"></a>2. CLI-Modus wechseln

Azure DNS verwendet Azure Ressourcenmanager. Stellen Sie sicher, dass Sie CLI Modus um Cloud Befehle verwenden wechseln.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3 melden Sie sich bei Ihrem Konto Azure

Sie werden aufgefordert, Ihre Anmeldeinformationen nicht authentifiziert. Lassen Sie beachten Sie, dass Sie nur WEITERENTWICKELT Konten verwenden können.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4 Wählen Sie 4 das Abonnement

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5 eine Ressourcengruppe erstellen

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen globale, nicht regionalen sind, hat die Wahl der Speicherort der Ressource Gruppe jedoch keine Auswirkung auf Azure DNS.

Wenn Sie eine vorhandene Ressourcengruppe verwenden, können Sie diesen Schritt überspringen.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. register

Der Azure DNS-Dienst wird von den Microsoft.Network Ressourcenanbieter verwaltet. Ihr Abonnement Azure muss registriert sein, um diese Ressourcenanbieter verwenden, bevor Sie Azure DNS verwenden können. Dies ist eine einmaligen Vorgang für jedes Abonnement.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Schritt 2: Erstellen einer DNS-zone

Eine DNS-Zone wird erstellt, mit der `azure network dns zone create` Befehl. Optional können Sie eine DNS-Zone zusammen mit Kategorien erstellen. Kategorien sind eine Liste von Name-Wert-Paare und Rechnungsadresse oder Gruppierungsebene Zwecken von Azure Ressourcenmanager zu Bezeichnung Ressourcen verwendet. Weitere Informationen zu Kategorien finden Sie unter [Verwenden von Kategorien, um Ihre Azure Ressourcen zu organisieren](../resource-group-using-tags.md).

In Azure-DNS Zonennamen angegeben werden sollte, ohne dass eine beendet **'.'**. In diesem Fall als "**contoso.com**" statt "**contoso.com.**'.


### <a name="to-create-a-dns-zone"></a>Zum Erstellen einer DNS zone

Im folgenden Beispiel wird eine DNS-Zone Namen *"contoso.com"* in der Ressourcengruppe aufgerufen *MyResourceGroup*erstellt.

Verwenden Sie das Beispiel, um Ihre DNS-Zone, ersetzen die Werte für Ihre eigenen erstellen.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>So erstellen eine DNS-Zone und Tags.

Azure DNS CLI unterstützt Kategorien von DNS-Zonen mithilfe der optionalen angegebenen *-Tag* Parameter. Im folgenden Beispiel wird zum Erstellen einer DNS-Zone mit zwei Kategorien project = Demo und Env = Test.

Verwenden Sie zum Erstellen eines DNS-Zone und Kategorien, ersetzen die Werte für Ihren eigenen im folgenden Beispiel wird ein.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Anzeigen von Datensätzen

Erstellen einer DNS-Zone erstellt auch die folgenden DNS-Einträge:

- Der Datensatz 'Start of Authority' (SOA). Dies ist im Stammverzeichnis jeder DNS Zone präsentieren.

- Die autorisierende (NS) Namenservereinträge. Diese anzeigen, welche Namenserver die Zone gehostet werden. Azure DNS verwendet einen Pool von Namenserver, und so unterschiedlich Namenserver für verschiedene Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Stellvertretung einer Domäne zu Azure DNS](dns-domain-delegation.md) .

Verwenden Sie zum Anzeigen dieser Datensätze `azure network dns-record-set show`.<BR>
*Syntax: Netzwerk-Dns-Eintrag-Set anzeigen < Ressourcengruppe >< Dns-Zone-Name > <name><type>*


Im folgenden Beispiel, wenn Sie den Befehl mit Ressourcen Gruppe *Myresourcegroup*, ausführen Eintrag festlegen Namen *"@"* (für einen Datensatz aus), und geben Sie ein *SOA*, verwendet die folgende Ausgabe, führt:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Um mit der Zone erstellt die NS-Einträge anzeigen möchten, verwenden Sie den folgenden Befehl aus:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Eintrag legt am Stamm (oder *Spitze*) eine Bedienung DNS Zone **@** als der Eintrag Name fest.

## <a name="test"></a>Test

Sie können Ihre DNS-Zone mithilfe der DNS-Tools, wie z. B. Nslookup, der Vorgang, testen oder die `Resolve-DnsName` PowerShell-Cmdlet.

Wenn Sie Ihre Domäne für die neue Zone in Azure DNS verwenden noch delegiert nicht geschehen ist, müssen Sie die DNS-Abfrage direkt auf eine der Namenserver für Ihre Zone direkte. Die Namenserver für Ihre Zone erhalten die NS-Einträge oberhalb von "Azure Netzwerk DNS-Eintrag-Set anzeigen" nicht aufgeführt. Stellen Sie sicher das Wechseln der richtigen Werte für Ihre Zone in den folgenden Befehl.

Im folgende Beispiel wird der Vorgang zum Abfragen von der Domäne "contoso.com" verwenden die Namenserver für die DNS-Zone zugewiesen verwendet. Die Abfrage an einen Namensserver verweisen für den wir verwendet hat * @ * und der Vorgang mit Zonenname.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Erstellen einer DNS-Zone, [Datensatz Sätze und Datensätze](dns-getstarted-create-recordset-cli.md) zum Auflösen von Namen für Ihre Domäne Internet starten aus.
