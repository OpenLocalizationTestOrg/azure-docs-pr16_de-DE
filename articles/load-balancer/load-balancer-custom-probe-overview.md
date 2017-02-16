<properties
  pageTitle="Laden Sie Benutzerdefinierte Probes Lastenausgleich und Überwachung Integritätsstatus | Microsoft Azure"
  description="Erfahren Sie, wie benutzerdefinierte führt eine Überprüfung auf Azure Lastenausgleich Instanzen hinter Lastenausgleich überwachen"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Grundlegendes zu laden Lastenausgleich Prüfpunkte

Azure Lastenausgleich bietet die Möglichkeit, die Integrität des Server-Instanzen mithilfe von Prüfpunkte zu überwachen. Wenn Sie ein Prüfpunkt nicht reagiert, beendet Lastenausgleich das Senden von neuer Verbindungen den fehlerhaften Instanz. Vorhandenen Verbindungen sind hiervon nicht betroffen und neue Verbindungen fehlerfrei Instanzen gesendet wird.

Cloud-Dienstverwaltungsrollen (Worker-Rollen und Webrollen) verwenden einen Gast-Agent zum Überwachen der Prüfpunkt. TCP- oder HTTP-Benutzerdefinierte Probes müssen konfiguriert sein, bei der Verwendung von virtuellen Computern hinter Lastenausgleich.

## <a name="understand-probe-count-and-timeout"></a>Grundlegendes zu Prüfpunkt zählen und timeout

Prüfpunkt Verhalten hängt:

- Die Anzahl der erfolgreichen Prüfpunkte, mit denen eine Instanz bezeichnet werden können als aktiv.
- Die Anzahl der Fehler beim Prüfpunkte, die dazu führen, dass eine Instanz bezeichnet werden als nicht verfügbar.

Timeout und Häufigkeit festlegen in SuccessFailCount zu bestimmen, ob eine Instanz bestätigt wird ausgeführt oder nicht ausgeführt werden. Das Timeout wird im Portal Azure zweimal den Wert für die Häufigkeit festgelegt.

Die Konfiguration Prüfpunkt aller Lastenausgleich Instanzen für einen Endpunkt (d. h., eine Gruppe mit Lastenausgleich) muss übereinstimmen. Dies bedeutet, dass Sie eine andere Prüfpunkt Konfiguration für jede Instanz der Rolle oder virtuellen Computern in der gleichen gehosteten Dienst für einen bestimmten Endpunkt Kombination haben können. Beispielsweise muss jede Instanz identische lokale Ports und Zeitlimit aufweisen.

>[AZURE.IMPORTANT] Ein Lastenausgleich Prüfpunkt verwendet die IP-Adresse 168.63.129.16. Diese öffentliche IP-Adresse ermöglicht die Kommunikation mit internen Plattform Ressourcen für die schalten your-Besitzer-IP-Azure virtuellen Netzwerkszenario. Die virtuelle öffentliche IP-Adresse 168.63.129.16 wird in allen Regionen verwendet und nicht ändern. Es empfiehlt sich, dass Sie diese IP-Adresse in eine lokale Firewall-Richtlinien zulassen. Es sollte kein Sicherheitsrisiko angesehen werden, da die interne Azure Plattform eine Nachricht von dieser Adresse Datenquelle kann. Wenn Sie dies tun, werden in einer Vielzahl von Szenarien wie konfigurieren die gleichen IP-Adresse Spektrum 168.63.129.16 und IP-Adressen Probleme dupliziert unerwartetes Verhalten.

## <a name="learn-about-the-types-of-probes"></a>Informationen Sie zu den Arten von Prüfpunkten

### <a name="guest-agent-probe"></a>Gast Agent Prüfpunkt

Dieser Prüfpunkt ist nur für Azure Cloud Services verfügbar. Lastenausgleich nutzt den Gast-Agent innerhalb des virtuellen Computers, und klicken Sie dann überwacht und reagiert mit einer HTTP 200 OK-Antwort nur, wenn die Instanz im Zustand bereit ist (d. h., nicht in einem anderen Zustand wie gebucht, Wiederverwendung oder Beenden).

Weitere Informationen finden Sie unter [Konfigurieren der Definition Dienstdatei (Csdef) für den Systemzustand Prüfpunkte](https://msdn.microsoft.com/library/azure/ee758710.aspx) oder [Erste Schritte beim Erstellen eines Internet zugänglichen Lastenausgleich für Clouddienste](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Wodurch zeichnet sich einen Gast Agent Prüfpunkt eine Instanz als fehlerhaft kennzeichnen?

Wenn der Gast Agent nicht reagiert mit HTTP 200 OK, Lastenausgleich markiert die Instanz als nicht reagiert, und stoppt den Datenverkehr auf diese Instanz senden. Lastenausgleich weiterhin die Instanz Signal an. Wenn der Gast-Agent mit einer HTTP 200 reagiert, sendet Lastenausgleich Verkehr auf diese Instanz erneut ein.

Wenn Sie eine Webrolle verwenden, in der Regel der Website Code ausgeführt wird, in w3wp.exe, die nicht von der Azure überwacht wird Fabric oder Gast Agent. Dies bedeutet, dass Fehler in w3wp.exe (z. B. HTTP 500 Antworten) nicht an den Gast Agent gemeldet werden und Lastenausgleich nicht die betreffende Instanz von Drehung dauert.

### <a name="http-custom-probe"></a>Benutzerdefinierte HTTP-probe

Die benutzerdefinierte HTTP-Lastenausgleich Probe überschreibt den standardmäßigen Gast Agent Prüfpunkt, was bedeutet, dass Sie Ihre eigenen benutzerdefinierte Logik Ermittlung die Integrität des die Instanz der Rolle erstellen können. Lastenausgleich, sucht den Endpunkt alle 15 Sekunden, standardmäßig an. Die Instanz wird in die laden Lastenausgleich Drehung sein, wenn es in der vorgegebenen Zeit (standardmäßig 31 Sekunden) mit einer HTTP 200 reagiert angesehen.

Dies kann sinnvoll, wenn Sie eine eigene Logik zum Entfernen von Instanzen aus laden Lastenausgleich Drehung implementieren möchten sein. Beispielsweise könnten Sie entscheiden, zu eine Instanz entfernen, beispielsweise wenn sie über 90 % CPU ist, und einen Status nicht 200 gibt. Wenn Sie Webrollen, die w3wp.exe verwenden verfügen, das auch bedeutet, erhalten Sie automatische Überwachung Ihrer Website, da bei Ihrer Website Code zum Laden Lastenausgleich Prüfpunkt Status nicht 200 zurückgeben.

>[AZURE.NOTE] Die benutzerdefinierte HTTP-Probe unterstützt relative Pfade und nur HTTP-Protokoll. HTTPS wird nicht unterstützt.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Wodurch zeichnet sich eine benutzerdefinierte HTTP-Probe eine Instanz als fehlerhaft kennzeichnen?

- Die HTTP-Anwendung gibt einen HTTP-Antwortcode als 200 (z. B. 403, 404 oder 500). Hierbei handelt es sich um eine positive Bestätigung, die die Instanz der Anwendung sofort genommen werden sollte.
- HTTP-Server reagiert gar nicht nach dem Punkt Zeitlimit. Je nach den Timeoutwert festgelegt ist dies bedeutet möglicherweise, die mehrere Prüfpunkt anfordert Gehe zu unbeantwortet, bevor der Prüfpunkt markiert wird als nicht ausgeführt (d. h., bevor Sie SuccessFailCount Prüfpunkte gesendet werden).
- Der Server schließt die Verbindung über ein TCP zurücksetzen.

### <a name="tcp-custom-probe"></a>Benutzerdefinierte Probe TCP

TCP Prüfpunkte initiieren eine Verbindung nach Durchführung eines dreifach-Handshakes mit den definierten Port an.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Wodurch zeichnet sich eine TCP benutzerdefinierte Probe eine Instanz als fehlerhaft kennzeichnen?

- Der Server TCP reagiert gar nicht nach dem Punkt Zeitlimit. Wenn der Prüfpunkt markiert ist, als nicht ausgeführt, hängt von der Anzahl der Fehler beim Prüfpunkt Anfragen, die so konfiguriert wurden, dass unbeantwortete wechseln, bevor Sie den Prüfpunkt als nicht ausgeführt.
- Der Prüfpunkt erhält eine TCP aus der Rolleninstanz zurücksetzen.

Weitere Informationen zum Konfigurieren einer HTTP-Dienststatus Prüfpunkt oder einen Prüfpunkt TCP finden Sie unter [Erste Schritte beim Erstellen eines Internet zugänglichen Lastenausgleich in Ressourcenmanager mithilfe der PowerShell](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Hinzufügen von fehlerfrei Instanzen wieder in Last Lastenausgleich Drehung

TCP- und HTTP-Prüfpunkte werden als einwandfrei eingestuft und die Instanz der Rolle als fehlerfrei wann kennzeichnen:

- Lastenausgleich erhält einen positiven Prüfpunkt beim ersten der virtuellen Computer gestartet wird.
- Die Zahl SuccessFailCount (wie zuvor beschrieben) definiert den Wert erfolgreich Prüfpunkte, die erforderlich sind, um die Instanz der Rolle als fehlerfrei zu kennzeichnen. Wenn eine Instanz der Rolle entfernt wurde, muss die Anzahl der erfolgreichen, aufeinanderfolgende Prüfpunkte gleich oder darf den Wert von SuccessFailCount, um die Instanz der Rolle als aktiv markieren.

>[AZURE.NOTE] Wenn Sie der Integritätsstatus einer Instanz der Rolle entsprechend ist, wartet Lastenausgleich mehr vor dem Einfügen der Rolleninstanz wieder in die ordnungsgemäßen Zustand. Dies erfolgt über die Richtlinie zum Schutz der Benutzer und die Infrastruktur.

## <a name="use-log-analytics-for-load-balancer"></a>Log Analytics für Lastenausgleich verwenden

[Log Analytics für Lastenausgleich](load-balancer-monitor-log.md) können Prüfpunkt Integritätsstatus und Prüfpunkt zählen überprüfen. Protokollierung kann mit Power BI oder Azure Betrieb Einsichten zu Statistiken zur Integritätsstatus Lastenausgleich bieten verwendet werden.
