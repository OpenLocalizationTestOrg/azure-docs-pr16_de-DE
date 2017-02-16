<properties 
   pageTitle="Unterstützung und Rente Richtlinie Leitfaden für Azure Gast OS | Microsoft Azure" 
   description="Enthält Informationen zur was Microsoft betreffend auf Azure Gast OS von Cloud-Diensten verwendeten unterstützt werden." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Gast-BS Richtlinie Unterstützung und Einstellung von Websites
Die Informationen auf dieser Seite bezieht sich auf das Betriebssystem Azure Gast ([Gast-BS](cloud-services-guestos-update-matrix.md)) für Cloud Services Worker und Webrollen (PaaS). Es gilt nicht für virtuellen Computern (IaaS). 

Microsoft hat eine veröffentlichten [support-Richtlinien für das Betriebssystem Gast](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Die Seite, die Sie gerade lesen werden jetzt an, wie die Richtlinie implementiert wird.

Die Richtlinie ist 

1. Microsoft unterstützt **mindestens die neuesten zwei Familien des Betriebssystems Gast**. Wenn eine Familie deaktiviert wird, müssen Kunden 12 Monaten ab dem Zeitpunkt offizielle Rente auf eine neuere unterstützten Gast-BS Familie aktualisieren.
2. Microsoft unterstützt die **mindestens die neuesten zwei Versionen der unterstützten Gast-BS Familien**. 
3. Microsoft unterstützt die am **minimalen neuesten zwei Versionen des Azure SDK**. Wenn Sie eine Version des SDK deaktiviert wird, haben Kunden 12 Monaten ab dem Zeitpunkt offizielle Rente auf eine neuere Version aktualisieren. 

Möglicherweise gelegentlich mehr als zwei Familien oder Versionen unterstützt. Offizielle Gast-BS Supportinformationen wird auf [Azure Gast OS Versionen und SDK Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md)angezeigt.


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Wenn eine Gast-BS Familie oder Version Rente 


Eine neue Gast-BS **Familie** wird irgendwann nach der Freigabe einer neuen offiziellen Version des Betriebssystems Windows Server eingeführt werden. Bei jedem neuen Gast-BS Familien eingeführt wird, wird Microsoft die älteste Gast-BS Familie zurückziehen. 

Neue Gast-BS- **Versionen** werden eingeführt, über jeden Monat auf den neuesten Updates für MSRC einbinden. Aufgrund der regulären monatlichen Aktualisierungen ist eine Gast-BS-Version normalerweise deaktiviert 60 Tage nach deren Veröffentlichung an. Auf diese Weise wird mindestens zwei Versionen von Gast-BS für jede Familie zur Verwendung verfügbar. 

### <a name="process-during-a-guest-os-family-retirement"></a>Während einer Gast-BS familiäre Rente Prozess 


Nachdem der Rente angekündigt wurde, müssen Kunden einen Übergangszeitraum 12 Monate "", bevor die ältere Familie formal vom Dienst entfernt wird. Diesmal Übergang kann nach Maßgabe des Microsoft verlängert werden. Updates werden in [Azure Gast OS Versionen und SDK Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md)veröffentlicht werden.

Ein schrittweise Rente Prozess wird 6 Monate in den Übergangszeitraum ausgeführt. Dieses Zeitraums:

1. Microsoft benachrichtigt Kunden von der Rente. 
2. Die neuere Version der Azure SDK wird nicht die deaktivierte Gast-BS-Familie unterstützen.
3. Neue Bereitstellungen und Redeployments Cloud-Dienste nicht auf die deaktivierte Familie zulässig

Microsoft weiterhin neue Gast Betriebssystemversion Einbinden von den neuesten Updates für MSRC bis zum letzten Tag des Zeitraums Übergang, bekannt als "Ablaufdatum" vorstellen. Zu diesem Zeitpunkt wird die Clouddienste ausgeführt unter der Azure Vereinbarung zum SERVICELEVEL nicht unterstützt werden. Microsoft hat nach Maßgabe Upgrade erzwingen, löschen oder diese Dienste nach diesem Datum beenden.



### <a name="process-during-a-guest-os-version-retirement"></a>Prozess während einer Rente Gast Betriebssystemversion 
Wenn Kunden ihre Gast-BS automatisch aktualisieren festlegen möchten, müssen sie nie Gedanken Umgang mit Gast-BS-Versionen. Er werden immer die neueste Version von Gast-BS verwenden.

Gast-BS-Versionen werden jeden Monat veröffentlicht. Aufgrund der Kostensatz regelmäßig neue Versionen ist für jede Version eine feste Lebensdauer.

Bei 60 Tage lang in der Lebensdauer ist eine Version "*deaktiviert*". "Deaktiviert" bedeutet, dass die Version vom klassischen Azure-Portal entfernt wird. Sie können auch nicht mehr aus der Konfigurationsdatei CSCFG festgelegt werden. Vorhandene Bereitstellungen werden weiterhin ausgeführt, aber neue Bereitstellungen und Code und Konfiguration Updates zu vorhandenen Bereitstellungen nicht zulässig. 

Bei einem späteren Zeitpunkt, die Gast-BS-Version "*läuft ab*" und alle Anlagen werden weiterhin mit dieser Version sofort aktualisiert und Festlegen der Gast-BS in Zukunft automatisch zu aktualisieren. Ablauf erfolgt in Stapeln, damit der Zeitraum zwischen Deaktivierung zum Ablauf unterschiedlich sein kann. 

Diese Perioden möglicherweise mehr nach Maßgabe von Microsoft Kunden Übergänge vereinfachen vorgenommen werden. Änderungen werden auf der [Azure Gast OS Versionen und SDK Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md)kommuniziert werden müssen.



### <a name="notifications-during-retirement"></a>Während der Rente Benachrichtigungen 

* **Familiäre Rente** <br>Microsoft wird von Blogbeiträgen und Azure klassischen Portal Benachrichtigung verwendet. Kunden, die noch nicht mehr genutzten Gast-BS Familien werden durch direkte Kommunikation (e-Mail, Portal Nachrichten, Telefonanruf) zugeordneten Dienstadministratoren benachrichtigt. Alle Änderungen werden auf dieser Seite gebucht und des RSS-Feeds am Anfang dieser Seite aufgelistet. 


* **Version Rente** <br>Alle Änderungen werden auf dieser Seite gebucht und des RSS-Feeds am Anfang der Seite, einschließlich der Version deaktiviert und Ablaufdatum aufgeführt. Services-Administratoren, e-Mails erhalten, wenn sie Bereitstellungen für eine deaktivierte Gast Betriebssystemversion und die Familie ausgeführt haben. Die Anzeigedauer für diese e-Mails kann abweichen. Im Allgemeinen sind sie mindestens einen Monat vor der Deaktivierung, obwohl diese Anzeigedauer keine offizielle Vereinbarung zum SERVICELEVEL ist. 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie kann ich die Nachteile der Migration minimieren?**

Sie sollten neuesten Gast-BS Familie für das Entwerfen der Cloud Services verwenden. 

1. Planung der Migration zu einer Familie neuere früh zu starten. 
2. Richten Sie temporäre Test-Bereitstellungen So testen Sie Ihre Cloud-Dienst ausgeführt wird, klicken Sie auf die neue Familie aus. 
3. Legen Sie Ihre Gast-BS-Version auf **Automatische** (OsVersion = * in der Datei [.cscfg](cloud-services-model-and-package.md#cscfg) ), damit die Migration auf neue Gast-BS-Versionen automatisch erfolgt.

**Was geschieht, wenn mein Webanwendung tieferen Integration in das Betriebssystem erfordert?**

Wenn die Architektur einer Anwendung Web tieferen Abhängigkeit von dem zugrunde liegenden Betriebssystem erforderlich ist, unterstützt Verwenden eines Funktionen wie [beim Start von Vorgängen](cloud-services-startup-tasks.md) oder anderen Erweiterungsmechanismen, die möglicherweise in der Zukunft vorhanden ist. Alternativ können Sie auch [Azure-virtuellen Computern](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – Infrastruktur als Dienst) verwenden, in dem Sie für die Verwaltung des zugrunde liegenden Betriebssystems zuständig sind.
 
## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die neuesten [Gast-BS frei](cloud-services-guestos-update-matrix.md).
