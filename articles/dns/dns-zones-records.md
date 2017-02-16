<properties 
   pageTitle="DNS-Zonen und Einträge | Microsoft Azure" 
   description="Übersicht über die Unterstützung für das Hosten von DNS-Zonen und in Microsoft Azure-DNS-Einträge." 
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
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-Zonen und Einträge

Auf dieser Seite wird erläutert, die wichtigsten Konzepte von Domänen, DNS-Zonen, und DNS-Einträge und Datensätze und wie sie in Azure DNS-Einträge unterstützt werden.

## <a name="domain-names"></a>Domänennamen
 
Domain Name System ist eine Hierarchie von Domänen. Die Hierarchie beginnt mit der Domäne "Root", deren Name einfach ist '**.**'.  Gelangen Sie unter diese Domänen auf oberster Ebene, wie z. B. 'com', 'Netto', 'Organigramm', 'uk' oder 'jp' ein.  Sind Sie unter diese Domänen der zweiten Ebene, wie z. B. 'org.uk' oder 'co.jp' ein. Die Domänen in der DNS-Hierarchie sind global verteilt von DNS-Namenserver auf der ganzen Welt gehostet wird.

Eine Domänennamen-Registrierungsstelle ist eine Organisation, die Sie einen Domänennamen, wie etwa "contoso.com" kaufen kann.  Erwerben eines Domänennamens bietet Ihnen das Recht, die DNS-Hierarchie unter diesen Namen, indem Sie den Namen "www.contoso.com' auf der Website Ihres Unternehmens leiten beispielsweise steuern. Die Registrierungsstelle kann die Domäne in einem eigenen Namenserver in Ihrem Auftrag hosten, oder Sie können auch alternativen Namenserver angeben.

Azure DNS bietet eine Global verteilten, hoher Verfügbarkeit Name Server-Infrastruktur, die Sie verwenden können, um Ihre Domäne zu hosten. Durch das Hosten von Domänen in Azure DNS, können Sie Ihre DNS-Einträge mit den gleichen Anmeldeinformationen, APIs, Tools, Abrechnung und Support als anderer Azure Dienste verwalten.

Azure DNS unterstützt erwerben von Domänennamen derzeit nicht. Wenn Sie einen Domänennamen kaufen möchten, müssen Sie eine Drittanbieter-Domänennamen-Registrierungsstelle verwenden. Die Registrierungsstelle wird in der Regel eine kleine jährliche Gebühr. Die Domänen können dann in Azure DNS für die Verwaltung von DNS-Einträgen gehostet werden. Details finden Sie unter [Stellvertretung einer Domäne zu Azure DNS](dns-domain-delegation.md) .

## <a name="dns-zones"></a>DNS-Zonen 

Eine DNS-Zone wird verwendet, um die DNS-Einträge für eine bestimmte Domäne gehostet wird. Um Ihre Domäne in Azure DNS-hosting zu beginnen, müssen Sie eine DNS-Zone für diesen Domänennamen zu erstellen. Jede DNS-Eintrag für Ihre Domäne wird dann innerhalb dieser DNS Zone erstellt. 

Die Domäne "contoso.com" enthalten möglicherweise mehrere DNS-Einträge, wie etwa "mail.contoso.com" (für eine e-Mail-Server) und "www.contoso.com" (für eine Website).

Beim Erstellen einer DNS-Zone Azure DNS muss der Namen der Zone innerhalb der Ressourcengruppe eindeutig sein. Zone gleichnamigen kann in einer anderen Ressourcengruppe oder ein anderes Azure-Abonnement wiederverwendet werden. Wobei mehrere Zonen denselben Namen haben, wird jede Instanz anderen Namen Serveradressen zugewiesen. Mit den Domänennamen-Registrierungsstelle kann nur einen Satz von Adressen konfiguriert werden.

>[AZURE.NOTE] Sie müssen nicht zum Erstellen einer DNS Zone mit diesen Domänennamen in Azure DNS Domänennamen besitzen. Sie müssen jedoch die Domäne aus, um die Azure DNS-Namensserver als die richtige Namenserver für den Domänennamen mit den Domänennamen-Registrierungsstelle konfigurieren besitzen.

Weitere Informationen finden Sie unter [Stellvertretung einer Domäne zu Azure DNS](dns-domain-delegation.md).

## <a name="dns-records"></a>DNS-Einträge

### <a name="record-types"></a>Eintragstypen

Jede DNS-Eintrag hat einen Namen und einen Typ. Einträge sind in verschiedenen Typen entsprechend den Daten organisiert darin enthaltenen. Der am häufigsten verwendete Typ ist ein "A"-Eintrag, der einen Namen zu einer IPv4-Adresse zugeordnet ist. Ein anderer ist allgemeine ein Eintrag 'MX', der einen Namen mit einer e-Mail-Server zugeordnet ist.

Azure DNS unterstützt allgemeine DNS-Eintragstypen: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV und TXT.

### <a name="record-names"></a>Aufzeichnen von Namen

In Azure DNS werden Datensätze mit relativen Namen angegeben. Ein *vollqualifizierten* Domänennamen (FULLY) enthält den Zonennamen, wohingegen *relativer* Name nicht der Fall ist. Beispielsweise bietet relativ Datensatz namens "Www" in der Zone "contoso.com" den Eintrag vollständig qualifizierte Namen "www.contoso.com".

Ein Eintrag *Spitze* wird eines DNS-Eintrags am Stamm (oder *Spitze*) einer DNS-Zone an. In der DNS-Zone "contoso.com" ist ein Datensatz Spitze beispielsweise auch den vollqualifizierten Namen "contoso.com" (Dies ist eine Domäne *naked* bezeichnet).  Üblicherweise, den Namen der relativen '@' wird verwendet, um die Einträge Spitze zu erstellen.

### <a name="record-sets"></a>Datensätze
Manchmal müssen Sie mehrere DNS-Eintrag mit einem angegebenen Namen und Typ zu erstellen. Nehmen Sie beispielsweise an, dass die Website 'www.contoso.com' auf zwei verschiedene IP-Adressen gehostet wird. Die Website erfordert zwei verschiedenen A-Einträge, eine für jede IP-Adresse. Dies ist ein Beispiel für einen Datensatz festlegen:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS verwaltet die DNS-Einträge mit *Eintrag legt fest*. Ein Datensatz (auch bekannt als eine *Ressource* Datensatzgruppe) ist die Sammlung von DNS-Einträge in einer Zone, die denselben Namen haben und vom gleichen Typ sind. Die meisten Datensätze enthalten einen einzelnen Datensatz, wie dieser, in denen einer mehr als ein Datensatz enthält Beispiele sind jedoch nicht selten.

Beispielsweise angenommen, Sie bereits in der Zone "contoso.com" ein A-Datensatzes "Www" erstellt haben, zeigt die IP-Adresse '134.170.185.46' (den ersten Eintrag oben).  Wenn Sie den zweiten Datensatz erstellen würden Sie den Datensatz an die vorhandenen Datensatz hinzufügen, anstatt einen neuen Datensatz erstellen.

Die SOA und CNAME Datensatztypen werden Ausnahmen. Die DNS-Standards nicht mehrere Datensätze mit demselben Namen für diese Dateitypen ermöglichen, daher können diese Sätze mit Einträgen nur einen einzigen Datensatz enthalten.

### <a name="time-to-live"></a>Time-to-live

Die Gültigkeitsdauer oder die Gültigkeitsdauer (TTL) gibt an, wie lange jeden Datensatz von Clients zwischengespeichert werden, bevor Sie erneut abgefragt wird. Im obigen Beispiel ist die Gültigkeitsdauer 3.600 Sekunden oder 1 Stunde.

In Azure DNS ist die Gültigkeitsdauer für den Datensatz ein, nicht für jeden Datensatz angegeben, daher der gleiche Wert für alle Datensätze innerhalb dieser Anzahl von Datensätzen verwendet wird.  Sie können einen beliebigen TTL-Wert zwischen 1 und 2.147.483.647 Sekunden angeben.

### <a name="wildcard-records"></a>Platzhalterzeichen Datensätze

Azure DNS unterstützt [Platzhalterzeichen Datensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). (Es sei denn, eine näher Übereinstimmung in einem anderen als Datensatz Satz), werden die Platzhalterzeichen Datensätze als Antwort auf eine Abfrage mit einem übereinstimmenden Namen zurückgegeben. Für alle Datensatztypen außer NS und SOA werden Platzhalterzeichen Datensatz-Gruppen unterstützt.  

Verwenden Sie zum Erstellen einer Datensatzgruppe Platzhalterzeichen der Datensatz SetName '\*'. Alternativ können Sie auch einen Namen mit '\*'wie die linke Beschriftung, beispielsweise'\*.foo'.

### <a name="cname-records"></a>CNAME-Einträge

CNAME-Datensätze können nicht mit anderen Datensatz mit demselben Namen verwendet werden. Sie können keine beispielsweise einen CNAME-Eintrag mit dem relativ Namen "Www" festlegen und eines A-Datensatzes mit relativ namens "Www" zur gleichen Zeit erstellen.

Da der Zone Spitze (Name = ‘@’) enthält immer die NS und SOA aufzeichnen Datensätze, die erstellt wurden, wenn die Zone erstellt wurde, Sie können nicht in der Zone Spitze festlegen einen CNAME-Eintrag erstellen.

Dieser Einschränkungen entstehen durch den DNS-Standards und sind nicht Schwächen Azure DNS.

### <a name="ns-records"></a>Die NS-Einträge

Ein NS-Datensatzgruppe wird automatisch erstellt, in der Spitze jeder Zone (Name = ‘@’), und wird automatisch gelöscht, wenn die Zone gelöscht wird (es kann nicht gelöscht werden getrennt).  Sie können die Gültigkeitsdauer für diesen Datensatz Datensatz ändern, aber die Datensätze, die vorkonfiguriertes verweisen auf die Azure DNS-Namenserver der Zone zugewiesen sind nicht ändern.

Erstellen und andere NS-Einträge in der Zone, in der Zone-Spitze nicht löschen können.  Dies können Sie untergeordneten Zonen konfigurieren (siehe [Delegieren Unterdomänen in Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>SOA-Einträge

Eine Datensatzgruppe SOA wird automatisch erstellt, in der Spitze jeder Zone (Name = ‘@’), und wird automatisch gelöscht, wenn die Zone gelöscht wird.  SOA Einträge können nicht erstellt oder gelöscht getrennt. 

Sie können alle Eigenschaften des SOA-Eintrags eine Ausnahme bilden jedoch die Eigenschaft 'Host' ändern, also vorkonfiguriert, verweisen auf den Namen der Name des primären Servers durch Azure DNS bereitgestellt.

### <a name="spf-records"></a>SPF-Einträge

Sender Policy Framework (SPF) Einträge, dienen zur zurück, der angibt, welche e-Mail-Server zum Senden von e-Mail für einen angegebenen Domänennamen zulässig sind.  Ordnungsgemäße Konfiguration von SPF-Einträge ist wichtig, um zu verhindern, dass Empfänger kennzeichnen von e-Mails als "Junk".

Den DNS-RFCs eingeführt ursprünglich einen neuen 'SPF –' Datensatztyp, um dieses Szenario zu unterstützen. Zur Unterstützung von älteren Namenserver ausgewählt sie auch die Verwendung des Typs TXT-Eintrag SPF-Einträge angeben.  Diese Mehrdeutigkeit geführten zu Verwirrung, die vom [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)aufgelöst wurde.  Dies angegeben SPF-Einträge nur mit dem Datensatztyp TXT erstellt werden soll, und nicht mehr unterstützt den SPF-Datensatztyp. 

**SPF-Einträge von Azure DNS-Einträge unterstützt werden, und verwenden den Typ der TXT-Eintrag erstellt werden soll.** Der Typ der veraltete SPF-Eintrag wird nicht unterstützt. Beim [Importieren einer DNS zone Datei](dns-import-export.md), alle SPF-Einträge mit dem Datensatztyp SPF, werden konvertiert auf den TXT-Eintrag. 

### <a name="srv-records"></a>SRV-Einträge

[SRV-Einträge](https://en.wikipedia.org/wiki/SRV_record) werden von verschiedenen Diensten verwendet, Serverspeicherorte angeben. Wenn Sie einen SRV-Eintrag in DNS Azure angeben:

- Der *Dienst* und *Protokoll* müssen als Teil der Datensatz, mit dem Präfix Unterstriche Setname angegeben werden.  Beispielsweise '\_sip. \_tcp.name'.  Für einen Eintrag in der Zone Spitze, es ist nicht angegeben '@' im Feld Eintragsname einfach mit der Dienst und Protokoll, z. B. '\_sip. \_Tcp'.
- Die *Priorität* *Stärke*, *Port*, *Ziel* angegeben sind, und als Parameter jeden Datensatz Datensatz festlegen. 

## <a name="tags-and-metadata"></a>Kategorien und Metadaten

### <a name="tags"></a>Kategorien
Kategorien sind eine Liste von Name-Wert-Paare und werden durch Azure Ressourcenmanager verwendet, um Ressourcen der Bezeichnung.  Ressourcenmanager Azure mithilfe von Tags gefilterte Ansichten Ihrer Rechnung Azure aktivieren, und auch ermöglicht Ihnen das eine Richtlinie für die Kategorien erforderlich sind. Weitere Informationen zu Kategorien finden Sie unter [Verwenden von Kategorien, um Ihre Azure Ressourcen zu organisieren](../resource-group-using-tags.md).

Azure DNS unterstützt Azure Ressourcenmanager Kategorien auf DNS Zonenressourcen verwenden.  Es unterstützt keine Kategorien auf DNS-Datensätze.

### <a name="metadata"></a>Metadaten

Als Alternative zu Datensatzgruppe Tags unterstützt Azure DNS Stapels Datensätze, die mit "Metadata".  Ähnlich wie Kategorien, kann Metadaten Sie jeder Datensatzgruppe Name-Wert-Paare zuordnen.  Dies kann sinnvoll sein, beispielsweise zum Datensatz den Zweck der einzelnen Datensatz sein.  Im Gegensatz zu Tags Metadaten kann nicht verwendet werden, um einer gefilterten Ansicht Ihrer Rechnung Azure bereitzustellen und kann nicht in einer Richtlinie Azure Ressourcenmanager angegeben werden.

## <a name="limits"></a>Grenzwerte

Die folgenden standardmäßigen Grenzwerte wenden Sie bei Verwendung von Azure DNS-Einträge:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwenden von Azure DNS, zum [Erstellen einer DNS Zone](dns-getstarted-create-dnszone-portal.md) und [Erstellen von DNS-Einträgen](dns-getstarted-create-recordset-portal.md).
- Informationen zum Migrieren einer bestehenden DNS Zone zum [Importieren und DNS Zone File](dns-import-export.md).
