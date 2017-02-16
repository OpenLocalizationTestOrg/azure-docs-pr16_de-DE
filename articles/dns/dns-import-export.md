<properties
   pageTitle="Importieren und Exportieren eine Zonendatei der Domäne bei Azure DNS-CLI mit | Microsoft Azure"
   description="Informationen Sie zum Importieren und Exportieren einer DNS-Zonendatei bei Azure DNS-mithilfe von Azure CLI"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importieren und Exportieren einer DNS-Zonendatei mithilfe der Azure-CLI


In diesem Artikel werden Sie durch geführt zum Importieren und Exportieren von DNS Zonendateien für Azure DNS die CLI Azure verwenden.

## <a name="introduction-to-dns-zone-migration"></a>Einführung in DNS Zonenmigration

Eine DNS-Zonendatei ist eine Textdatei, die Details zu jedem Datensatz (DNS = Domain Name System) in der Zone enthält. Es folgt ein Standardformat, wodurch es für die Übertragung von DNS-Einträge zwischen DNS-Systemen geeignet. Verwenden einer Zonendatei ist eine schnell, zuverlässig und bequeme Möglichkeit, eine DNS-Zone hinein- oder aus Azure DNS zu übertragen.

Azure DNS-Einträge unterstützt das Importieren und Exportieren von Zonendateien mithilfe der Azure line Interface (CLI). Die Azure ist ein Befehlszeile Plattformen-Tool für die Verwaltung von Azure Services verwendet. Es ist für die Plattformen Windows, Mac und Linux von der [Seite Azure Downloads](https://azure.microsoft.com/downloads/)zur Verfügung. Unterstützung mehrerer Plattformen ist besonders wichtig für importieren und Exportieren von Zonendateien, da die am häufigsten verwendete Name Server-Software, [binden](https://www.isc.org/downloads/bind/), in der Regel auf Linux ausgeführt wird.

## <a name="obtain-your-existing-dns-zone-file"></a>Beziehen Sie die vorhandene DNS-Zonendatei

Bevor Sie eine DNS-Zonendatei in Azure DNS importieren, müssen Sie eine Kopie der Zonendatei zu erhalten. Die Quelle der Datei hängt davon ab, wo die DNS-Zone aktuell gehostet wird.

- Wenn Ihre DNS-Zone von einem Partner-Dienst (beispielsweise einer domänenregistrierungsstelle, dedizierten DNS-Hostinganbieter oder alternativen Cloudanbieter) gehostet wird, sollten diesen Dienst die Möglichkeit zum Herunterladen der DNS-Zonendatei bieten.

- Wenn Ihre DNS-Zone auf Windows-DNS-gehostet wird, ist der Standardordner für die Zonendateien **%systemroot%\system32\dns**aus. Der vollständige Pfad zu jeder Zonendatei werden auch auf der Registerkarte **Allgemein** der DNS-Dienst-Verwaltungskonsole auf.

- Wenn Ihre DNS-Zone mithilfe von BINDUNG gehostet wird, wird der Speicherort der Zonendatei für jede Zone in der BINDUNG Konfiguration Datei **named.conf**angegeben.

**Arbeiten mit Dateien aus GoDaddy Zonen**<BR>
Zonendateien aus GoDaddy heruntergeladen haben, etwas nicht standardmäßige Format. Sie müssen dies zu beheben, bevor Sie diese Zonendateien in Azure DNS importieren. DNS-Namen in der RData für jeden DNS-Eintrag als vollqualifizierten Namen angegeben sind, aber sie verfügen nicht über eine beenden "." Dies bedeutet, dass diese von anderen DNS-Systemen als relative Namen interpretiert werden. Sie müssen zum Bearbeiten der Zonendatei, um die beendet anfügen "." in ihren Namen, bevor Sie sie in Azure DNS importieren.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Importieren Sie eine DNS-Zonendatei in Azure DNS-Einträge


Importieren einer Zonendatei wird eine neue Zone Azure DNS erstellen, wenn noch nicht vorhanden ist. Wenn die Zone bereits vorhanden ist, müssen die Datensätze in der Zonendatei mit den vorhandenen Datensatz Datensätzen zusammengeführt werden.

### <a name="merge-behavior"></a>Zusammenführen von Verhalten

- Standardmäßig werden vorhandene und neue Datensätze zusammengeführt. Identische Datensätze innerhalb eines zusammengeführten Datensatz Satzes sind heben duplizierten.

- Sie können auch durch Angabe der `--force` Option, die Ergebnisse des Importierens ersetzt vorhandenen Datensatz Datensätze mit neuen Datensatz. Vorhandenen Datensatz Datensätze, die keinen entsprechenden Eintrag in der importierten Zonendatei festgelegt haben, werden nicht entfernt.

- Wenn Datensätze zusammengeführt werden, wird die Time to live (TTL), der bereits vorhandenen Datensatz Mengen verwendet. Wenn `--force` wird verwendet, wird die Gültigkeitsdauer der neuen Datensatzgruppe verwendet.

- Beginn der Zertifizierungsstelle (SOA) Parameter (außer `host`) werden immer die importierten Zonendatei, unabhängig davon, ob entnommen `--force` verwendet wird. Für den Namenservereintrag in der Zone Spitze festlegen, die Gültigkeitsdauer auf ähnliche Weise immer aus den importierten Zonendatei übernommen.

- Einer importierten CNAME-Eintrag ersetzt keine vorhandene CNAME-Eintrag mit demselben Namen, es sei denn, die `--force` angegeben wurde.

- Wenn ein Konflikt entsteht zwischen einen CNAME-Eintrag und ein anderer Datensatz mit demselben Namen verschiedene Typen (unabhängig davon, welche vorhanden ist oder neu), wird der vorhandene Datensatz beibehalten. Dies ist unabhängig von der Verwendung von `--force`.

### <a name="additional-information-about-importing"></a>Weitere Informationen zum Importieren von

Die folgenden Hinweise bieten zusätzliche technische Details über die Zone Prozess importieren.

- Die `$TTL` Richtlinie ist optional, und es wird unterstützt. Wenn keine `$TTL` Richtlinie wird angegeben, werden Datensätze ohne eine explizite TTL importierten festlegen, um einen Standardwert TTL 3600 Sekunden. Wenn zwei Datensätze in demselben Satz Datensatz anderen TTLs angeben, wird der niedrigere Wert verwendet.

- Die `$ORIGIN` Richtlinie ist optional, und es wird unterstützt. Wenn keine `$ORIGIN` festgelegt ist, wird der Standardwert verwendet wird, den Zonennamen gemäß Angabe in der Befehlszeile (plus die beendet ".").

- Die `$INCLUDE` und `$GENERATE` Richtlinien werden nicht unterstützt.

- Diese Eintragstypen werden unterstützt: A, AAAA, CNAME, MX, NS, SOA, SRV und TXT.

- SOA-Eintrag wird automatisch von Azure DNS erstellt, wenn eine Zone erstellt wird. Beim Importieren einer Zonendatei alle SOA Parameter werden entnommen der Zone Datei *mit Ausnahme* der `host` Parameter. Für diesen Parameter verwendet den Wert von Azure DNS verwendet. Dies ist, da für diesen Parameter muss sich auf dem primären Namenserver von Azure DNS bereitgestellten beziehen.

- Der Name Server-Eintrag in der Zone Spitze festlegen wird automatisch auch von Azure DNS erstellt, wenn die Zone erstellt wird. Nur die Gültigkeitsdauer für diesen Datensatz Datensatz wird importiert. Diese Datensätze enthalten die Namen Servernamen von Azure DNS bereitgestellt. Die Daten des Datensatzes werden durch die Werte in den importierten Zonendatei nicht überschrieben.

- Während der Public Preview unterstützt Azure DNS nur Single-Zeichenfolge TXT Records. Mehrfachzeichenfolge TXT Records werden verkettet und auf 255 Zeichen abgeschnitten werden.

### <a name="cli-format-and-values"></a>CLI Format und Werten


Das Format des Befehls Azure CLI So importieren Sie eine DNS-Zone ist:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Werte:

- `<resource group>`ist der Name der Ressourcengruppe für die Zone in Azure DNS.
- `<zone name>`ist der Name der Zone.
- `<zone file name>`ist der Pfadnamen der Zonendatei importiert werden sollen.

Wenn eine Zone mit diesem Namen in der Ressourcengruppe nicht vorhanden ist, wird es für Sie erstellt. Wenn die Zone bereits vorhanden ist, werden die importierten Datensätze mit vorhandenen Datensatz zusammengeführt. Um den vorhandenen Datensatz Datensätzen zu überschreiben, verwenden Sie die `--force` Option.

Überprüfung des Formats einer Zonendatei tatsächlich ohne ihn zu importieren, verwenden Sie die `--parse-only` Option.

### <a name="step-1-import-a-zone-file"></a>Schritt 1. Importieren einer Zonendatei

So importieren Sie eine Zonendatei für die Zone **"contoso.com"**.

1. Melden Sie sich bei Ihrem Abonnement Azure mithilfe der CLI Azure.

        azure login

2. Wählen Sie das Abonnement, in dem Sie auf Ihrer neue DNS Zone erstellen möchten.

        azure account set <subscription name>

3. Azure DNS wird eine Azure Ressourcenmanager nur, damit die CLI Azure auf Ressourcenmanager Modus gewechselt werden muss.

        azure config mode arm

4. Bevor Sie den Azure DNS-Dienst verwenden, müssen Sie Ihr Abonnement, um die Microsoft.Network Ressourcenanbieter verwenden registrieren. (Dies ist eine einmaligen Vorgang für jedes Abonnement).

        azure provider register Microsoft.Network

5. Wenn Sie eine bereits besitzen, müssen Sie auch eine Ressourcenmanager Ressourcengruppe erstellen.

        azure group create myresourcegroup westeurope

6. Wenn Sie um die Zone **"contoso.com"** aus der Datei **contoso.com.txt** in einer neuen DNS Zone in der Ressource Gruppe **Myresourcegroup**zu importieren, führen Sie den Befehl `azure network dns zone import`.<BR>Dieser Befehl die Zonendatei laden und analysieren. Der Befehl führt eine Reihe von Befehlen Azure DNS-Dienst die Zone erstellt, und alle den Eintrag in der Zone festgelegt. Der Befehl wird auch im Fenster Konsole zusammen mit Fehlern und Warnungen Fortschritte. Da Datensätze in der Reihe erstellt werden, kann es ein paar Minuten zum Importieren einer großen Zonendatei dauern.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Schritt 2. Überprüfen Sie die zone

Wenn Sie die DNS-Zone überprüfen, nachdem Sie die Datei importieren möchten, können Sie eine der folgenden Methoden verwenden:

- Sie können die Einträge auflisten, indem Sie mit den folgenden Befehl aus Azure CLI.

        azure network dns record-set list myresourcegroup contoso.com

- Sie können die Datensätze mithilfe des PowerShell-Cmdlets Liste `Get-AzureRmDnsRecordSet`.

- Sie können `nslookup` zur Überprüfung mit einer namensauflösung von für die Datensätze. Da die Zone noch delegiert nicht zur Verfügung, müssen Sie die richtige Azure DNS-Namenserver explizit angeben. Im folgenden Beispiel wird gezeigt, wie zum Abrufen der Namen Servernamen der Zone zugewiesen wird. IT auch veranschaulicht, wie den Eintrag "Www" mithilfe von Abfragen `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Schritt 3. Aktualisieren von DNS-Delegierung

Nachdem Sie überprüft haben, dass die Zone ordnungsgemäß importiert wurde, müssen Sie die DNS-Delegierung auf die Azure DNS-Namensserver verweisen zu aktualisieren. Weitere Informationen finden Sie im Artikel [Aktualisieren der DNS-Delegierung](dns-domain-delegation.md)aus.

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportieren einer DNS-Zonendatei aus Azure DNS

Das Format des Befehls Azure CLI So importieren Sie eine DNS-Zone ist:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Werte:

- `<resource group>`ist der Name der Ressourcengruppe für die Zone in Azure DNS.
- `<zone name>`ist der Name der Zone.
- `<zone file name>`ist der Pfadnamen der Zonendatei exportiert werden soll.

Als mit dem Import Zone müssen Sie zuerst sich, wählen Sie Ihr Abonnement und Konfigurieren der Azure CLI um Ressourcenmanager-Modus verwenden.

### <a name="to-export-a-zone-file"></a>So exportieren Sie eine Zonendatei


1. Melden Sie sich bei Ihrem Abonnement Azure mithilfe der CLI Azure.

        azure login

2. Wählen Sie das Abonnement, in dem Sie auf Ihrer neue DNS Zone erstellen möchten.

        azure account set <subscription name>

3. Azure DNS wird eine Azure Ressourcenmanager nur. Die CLI Azure müssen auf Ressourcenmanager Modus gewechselt werden.

        azure config mode arm

4. Führen Sie zum Exportieren von der vorhandenen Azure DNS Zone **"contoso.com"** in der Gruppe für die Ressource **Myresourcegroup** in der Datei **contoso.com.txt** (im aktuellen Ordner) `azure network dns zone export`. Dieser Befehl ruft den Dienst Azure DNS-Eintrag Datensätze in der Zone auflisten und Exportieren der Ergebnisse in einer BINDUNG kompatiblen Zonendatei.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

