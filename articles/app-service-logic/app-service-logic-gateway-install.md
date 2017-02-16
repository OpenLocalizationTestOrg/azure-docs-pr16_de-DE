<properties
   pageTitle="Logik Apps installieren lokaler datenverwaltungsgateway | Microsoft Azure"
   description="Informationen zum lokalen Daten Gateways für die Verwendung in einer app Logik zu installieren."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Installieren des lokalen Daten Gateways für Logik Apps

## <a name="installation-and-configuration"></a>Installation und Konfiguration

### <a name="prerequisites"></a>Erforderliche Komponenten

Minimum:

* 4.5-.NET Framework
* 64-Bit-Version von Windows 7 oder Windows Server 2008 R2 (oder höher)

Empfohlen:

* 8 Core CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

Verwandte Aspekte:

* Sie können kein Gateways auf einem Domain Controller installieren.
* Sie dürfen kein Gateways auf einem Computer installieren, solche einen Laptop, die im deaktiviert, werden kann, oder nicht mit dem Internet verbunden sind, da das Gateway nicht unter diesen Umständen eine ausgeführt werden kann. Darüber hinaus möglicherweise Gateway Leistung über eine drahtlose Netzwerk beeinträchtigt werden.

### <a name="install-a-gateway"></a>Installieren eines Gateways

Sie können das [Installationsprogramm für das lokale datenverwaltungsgateway hier](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409)abrufen.

**Lokaler datenverwaltungsgateway** als den Modus angeben, melden Sie sich mit Ihrer Arbeit oder Schule Konto, und klicken Sie dann entweder ein neues Gateway konfigurieren oder migrieren, wiederherstellen oder eines vorhandenen Gateways übernehmen.

* Um ein Gateway zu konfigurieren, geben Sie einen **Namen** für eine **Wiederherstellung-Taste**, und klicken Sie auf, oder tippen Sie auf **Konfigurieren**.

    Geben Sie einen Wiederherstellungsschlüssel, der mindestens acht Zeichen enthält, und halten Sie es an einem sicheren Ort. Sie benötigen diesen Schlüssel, wenn Sie migrieren, wiederherstellen oder deren Gateway übernehmen möchten.

* Geben Sie zum Migrieren, wiederherstellen oder Übernehmen eines vorhandenen Gateways, der Wiederherstellung-Key, die beim Erstellen des Gateways angegeben wurde.

### <a name="restart-the-gateway"></a>Starten des Gateways

Das Gateway ausgeführt wird als Windows-Dienst und, wie bei allen anderen Windows-Diensten, können Sie starten und beenden Sie es auf mehrere Arten. Sie können beispielsweise, öffnen Sie ein Eingabeaufforderungsfenster mit erweiterten Berechtigungen auf dem Computer, auf dem Gateway läuft, und führen Sie dann einen der folgenden Befehle:

* Zum Beenden des Diensts, führen Sie diesen Befehl aus:

    `net stop PBIEgwService`

* Wenn Sie den Dienst starten möchten, führen Sie diesen Befehl aus:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfigurieren der Firewall oder des Proxyservers

Informationen zum Bereitstellen Proxyinformationen für das Gateway finden Sie unter [Konfigurieren von Proxyeinstellungen](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Sie können überprüfen, ob Ihre Firewall oder des Proxyservers, Verbindungen blockieren werden möglicherweise durch den folgenden Befehl aus eine Aufforderung PowerShell ausgeführt. Dies wird der Azure-Dienstbus Konnektivität zu testen. Dieser nur Netzwerkkonnektivität überprüft und nicht etwas mit der Cloud-Server-Dienst oder das Gateway zu tun haben. Feststellen, ob Ihr Computer mit dem Internet tatsächlich aufrufen kann kann.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Die Ergebnisse sollten in diesem Beispiel ähneln. Wenn **TcpTestSucceeded** nicht true ist, können Sie durch eine Firewall blockiert werden.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Wenn vollständig sein soll, ersetzen Sie die Werte **ComputerName** und **den Port** durch finden Sie unter [Konfigurieren von Ports](#configure-ports) weiter unten in diesem Thema.

Blockiert die Firewall möglicherweise auch die Verbindungen, die der Azure-Dienstbus an die Azure Data Centers vornimmt. Wenn dies der Fall ist, werden Sie weißen möchten (Aufheben der Blockierung) alle IP-Adressen für Ihre Region für diese Daten Centers. Sie können eine Liste der [Azure-IP-Adressen hier](https://www.microsoft.com/download/details.aspx?id=41653)abrufen.

### <a name="configure-ports"></a>Konfigurieren von ports

Das Gateway wird eine ausgehende Verbindung mit Azure Service erstellt. Er kommuniziert auf ausgehende Ports: TCP 443 (Standard), 5671, 5672, 9350 bis 9354. Das Gateway erforderlich keine eingehenden Ports.

Weitere Informationen zu [Hybrid Lösungen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMÄNENNAMEN | AUSGEHENDE PORTS | BESCHREIBUNG |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671-5672 | Erweiterte Nachrichtenwarteschlangen-Protokoll (AMQP) |
| *. servicebus.windows.net | 443, 9350-9354 | Listener auf Service Bus Relay über TCP (erfordert 443 für Access Control token Acquisition) |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. von Core.Windows.NET befinden. | 443 | HTTPS |
| Login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Verwendet, um eine Verbindung mit dem Internet zu testen, ob das Gateway vom Power BI-Dienst nicht erreichbar ist. |

Wenn Sie in die weiße Liste IP-Adressen anstelle der Domänen benötigen, können Sie herunterladen und verwenden Sie die [Liste der Microsoft Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653). In einigen Fällen werden die Azure-Dienstbus Verbindungen mit IP-Adresse anstelle der vollqualifizierten Domänennamen vorgenommen werden.

### <a name="sign-in-account"></a>Anmelden Konto

Benutzer werden melden Sie sich mit entweder eine Arbeit oder Schule Konto. Dies ist Ihre Organisation-Konto. Wenn Sie für ein Office 365-Angebot registriert und der ist-Arbeit-e-Mail nicht angeben, können Sie wie folgt aussehen jeff@contoso.onmicrosoft.com. Ihr Konto in einen Cloud-Service wird in einen Mandanten in Azure Active Directory (AAD) gespeichert. In den meisten Fällen entspricht Ihres Kontos AAD UPN die e-Mail-Adresse aus.

### <a name="windows-service-account"></a>Windows-Dienstkonto

Lokaler Daten Gateways konfiguriert ist NT SERVICE\PBIEgwService für die Windows-Anmeldeinformationen-Dienst verwenden. Standardmäßig weist sie rechts neben der Log als Dienst. Dies ist im Zusammenhang mit dem Computer, auf dem Gateway installiert sind.

Dies ist das Konto eine Verbindung zu lokalen Datenquellen oder das geschäftlichen oder schulnotizbücher-Konto, mit dem Sie melden Sie sich in die cloud Services, nicht.

##<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="general"></a>Allgemeine

**Frage**: welche Datenquellen unterstützt das Gateway?<br/>
**Antwort**: zum Zeitpunkt der Erstellung dieses Dokuments, SQL Server.

**Frage**: benötige ich einen Gateway für Datenquellen in der Cloud, wie etwa SQL Azure? <br/>
**Antwort**: Nein. Ein Gateway verbindet auf lokale Datenquellen nur.

**Frage**: Was ist der tatsächliche Windows-Dienst aufgerufen?<br/>
**Antwort**: Dienste In das Gateway wird Power BI Enterprise Gateway Service bezeichnet.

**Frage**: Gibt es eingehenden Verbindungen mit dem Gateway aus der Cloud? <br/>
**Antwort**: Nein. Das Gateway wird ausgehende Verbindungen zu Azure Service Bus verwendet.

**Frage**: Was passiert, wenn ich ausgehende Verbindungen blockieren? Was muss ich öffnen? <br/>
**Antwort**: finden Sie unter die Ports und Hosts, die das Gateway verwendet.


**Frage**: besitzt des Gateways auf dem gleichen Computer als Datenquelle installiert werden? <br/>
**Antwort**: Nein. Das Gateway wird mit der Datenquelle mithilfe der Verbindungsinformationen, die bereitgestellte verbinden. Denken Sie als Clientanwendung in diesem Sinne des Gateways. Sie müssen nur in den Servernamen eine Verbindung herstellen können, die zur Verfügung gestellt wurde.


**Frage**: Was ist von der Wartezeit für das Ausführen von Abfragen mit einer Datenquelle über das Gateway? Was ist die beste Architektur? <br/>
**Antwort**: Installieren Sie das Gateway zum Verringern Netzwerkwartezeit so nah wie möglich die Datenquelle an. Wenn Sie das Gateway auf der tatsächlichen Datenquelle installieren können, wird es die Wartezeit eingeführt werden minimiert werden. Erwägen Sie auch die Data Centers. Wenn es sich bei Ihrem Dienst ist das Westen US Data Center und SQL Server auf eine Azure-virtuellen Computer gehostet haben, sollten Sie beispielsweise den Azure-virtuellen Computer in Westen USA sowie haben. Dies minimiert Wartezeit und Ausgang Gebühren des Azure-virtuellen Computers vermeiden.


**Frage**: Gibt es Anforderungen für Netzwerk-Bandbreite? <br/>
**Antwort**: Es wird empfohlen, dass guten Durchsatz für Ihre Netzwerkverbindung. Jede Umgebung ist anders, und die Menge der Daten, die gesendet, wirkt sich die Ergebnisse. Mit ExpressRoute konnte helfen, um eine Ebene des Datendurchsatzes zwischen lokalen und den Centers Azure-Daten zu gewährleisten.

Die app Drittanbieter-Tool Azure Geschwindigkeitstest können helfen, was Ihre Durchsatz ist-Zeitachse.


**Frage**: das Gateway Windows-Dienst ausgeführt werden kann mit einem Azure Active Directory-Konto? <br/>
**Antwort**: Nein. Der Windows-Dienst muss ein gültiges Windowskonto verfügen. Standardmäßig wird es mit der Dienst-SID NT SERVICE\PBIEgwService ausgeführt.


**Frage**: wie Ergebnisse gesendet werden wieder in die Cloud? <br/>
**Antwort**: Dies erfolgt über die Azure-Dienstbus. Weitere Informationen finden Sie unter Funktionsweise.


**Frage**: sind meine Anmeldeinformationen gespeichert? <br/>
**Antwort**: die Anmeldeinformationen, die für eine Datenquelle eingegeben werden in der Cloud-Gatewaydienst verschlüsselt gespeichert. Die Anmeldeinformationen sind in den Betrieben Gateway entschlüsselt.

### <a name="high-availabilitydisaster-recovery"></a>Hohe Verfügbarkeit/Wiederherstellung

**Frage**: Gibt es Pläne für das Aktivieren der hohen Verfügbarkeit Szenarien mit dem Gateway? <br/>
**Antwort**: Dies ist der Roadmap, aber wir haben eine Zeitachse noch nicht.


**Frage**: Welche Optionen für die Wiederherstellung verfügbar sind? <br/>
**Antwort**: Sie können die Taste Wiederherstellung verwenden, wiederherstellen oder Verschieben eines Gateways. Geben Sie die Taste Wiederherstellung, bei der Installation des Gateways.


**Frage**: Was die Vorteile von der Wiederherstellungsschlüssel ist? <br/>
**Antwort**: Es bietet eine Möglichkeit zum Migrieren oder Wiederherstellen Ihrer gatewayeinstellungen nach einem Datenverlust.

### <a name="troubleshooting"></a>Behandlung von Problemen

**Frage**:, in dem die Protokolle der Gateways werden? <br/>
**Antwort**: finden Sie unter Tools weiter unten in diesem Thema.


**Frage**: wie kann ich feststellen, welche Abfragen werden an die lokale Datenquelle gesendet werden? <br/>
**Antwort**: Sie können aktivieren gezeichnet, die Abfrage, die Abfragen gesendet werden sollen. Denken Sie daran, um ihn wieder in der Originalwert Abschluss Problembehandlung ändern. Verlassen der Abfrage Protokollierung aktiviert bewirkt, dass die Protokolle größer sein.

Sie können auch Tools anzeigen, die die Datenquelle für Tracing Abfragen enthält. Beispielsweise können Sie Extended Events oder SQL Profiler für SQL Server und Analysis Services verwenden.

## <a name="how-the-gateway-works"></a>Funktionsweise des Gateways

Wenn ein Benutzer ein Element interagiert, die mit einer lokalen Datenquelle verbunden ist:

1. Cloud-Dienst erstellt eine Abfrage, zusammen mit den verschlüsselten Anmeldeinformationen für die Datenquelle, und sendet die Abfrage an die Warteschlange für das Gateway zu verarbeiten.
1. Der Dienst analysiert die Abfrage und legt die Anforderung an den Azure Service Bus.
1. Lokaler Daten Gateways abfragt Azure Service Bus für ausstehende Anfragen.
1. Das Gateway Ruft die Abfrage ab, die Anmeldeinformationen entschlüsselt und eine Verbindung herstellt, die Datenquelle(n) mit diesen Anmeldeinformationen.
1. Das Gateway sendet die Abfrage an die Datenquelle für die Ausführung.
1. Die Ergebnisse werden aus der Datenquelle mit dem Gateway zurück, und klicken Sie dann auf die Cloud-Dienst gesendet. Der Dienst verwendet dann die Ergebnisse.

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="update-to-the-latest-version"></a>Auf die neueste Version aktualisieren

Wenn die Gateway-Version älter ist, kann zahlreiche Probleme bereitstellen.  Es ist eine bewährte, um sicherzustellen, dass Sie auf die neueste Version sind.  Wenn Sie das Gateway für einen Monat oder mehr nicht aktualisiert haben, möchten Sie möglicherweise Installieren der neuesten Version des Gateways berücksichtigen und angezeigt, wenn Sie das Problem reproduzieren können.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Fehler: Fehler beim Benutzer zur Gruppe hinzufügen. (-2147463168 PBIEgwService Performance Log Users)

Möglicherweise erhalten Sie diese Fehlermeldung, wenn Sie versuchen, des Gateways auf einem Domain Controller, installieren, die nicht unterstützt wird. Sie müssen des Gateways auf einem Computer bereitstellen, die einen Domänencontroller nicht zur Verfügung.

## <a name="tools"></a>Tools

### <a name="collecting-logs-from-the-gateway-configurator"></a>Erfassung von Protokollen in der Gateway-Konfiguration

Sie können mehrere Protokolle für das Gateway sammeln. Beginnen Sie immer mit den Protokollen!

#### <a name="installer-logs"></a>Installer-Protokolle

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfiguration von Protokollen

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise-Gateway-Dienst von Protokollen

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Ereignisprotokollen

Die Protokolle Datenverwaltungsgateway und PowerBIGateway sind unter **Anwendung und Dienste Protokolle**aufgeführt.

### <a name="fiddler-trace"></a>Fiddler Spur

[Fiddler](http://www.telerik.com/fiddler) ist ein kostenloses Tool von Telerik, die HTTP-Verkehr überwacht.  Können, finden Sie in den Hintergrund rücken und vierte mit Power BI-service aus dem Clientcomputer. Dies möglicherweise Fehler und andere verwandte Informationen anzeigen.

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen Sie eine lokale Verbindung zu Logik Apps](app-service-logic-gateway-connection.md)
- [Enterprise-Funktionen](app-service-logic-enterprise-integration-overview.md)
- [Logik Apps Verbinder](../connectors/apis-list.md)