<properties 
    pageTitle="Mithilfe der Hybrid-Verbindungs-Managers | Microsoft Azure" 
    description="Installieren und Konfigurieren der Hybrid-Verbindungs-Manager für lokale Connectors in Logik Apps verbinden" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Verbinden Sie mit lokalen Verbinder mithilfe der Hybrid-Verbindungs-Managers

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus. Allgemeiner Verfügbarkeit von Logik Apps (GA) verwendet ein Gateway für lokale Konnektivität. Weitere Informationen zu den neuen [Gateway](app-service-logic-gateway-connection.md) und [Logik Apps GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Zum Verwenden eines lokalen System verwendet Logik Apps Hybrid Verbindungs-Managers. Einige Verbinder können mit einem lokalen System, wie SQL Server, SAP, SharePoint und usw. verbinden. 

Die Hybrid Verbindung Manager (HCM) ist ein Klick-einmal Installer, die auf einem IIS-Server in Ihrem Netzwerk, sich hinter der Firewall installiert ist. Verwenden ein Azure-Dienstbus Relay, authentifiziert HCM das System lokal mit den Verbinder in Azure. 

> [AZURE.NOTE] Hybrid-Verbindungs-Managers ist erforderlich, nur, wenn Sie sich hinter der Firewall mit einer lokalen Ressource verbunden sind. Wenn Sie nicht zu einem lokalen System herstellen, nicht, müssen Sie den Hybrid-Verbindungs-Manager.

Um anzufangen, müssen Sie folgende Aktionen ausführen:

- Azure Dienstbus Relay Namespace SAS Verbindungszeichenfolge. Finden Sie unter [Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/) , um zu bestimmen, welche Ebene Relays umfasst.
- Lokale Anmeldung Systeminformationen wie Benutzernamen und Ihr Kennwort ein. Wenn Sie eine Verbindung mit einer lokalen SQL Server herstellen, benötigen Sie beispielsweise den SQL Server-Login-Konto und das Kennwort.
- Lokale Serverinformationen, einschließlich Port Zahl und Servername. Wenn Sie eine Verbindung mit einer lokalen SQL Server herstellen, benötigen Sie beispielsweise den SQL Server-Namen und TCP-Anschluss ein.

## <a name="get-the-service-bus-connection-string"></a>Abrufen der Verbindungszeichenfolge von Service Bus

Kopieren Sie im Portal Azure Dienstbus Stamm SAS Verbindungszeichenfolge aus. Diese Verbindungszeichenfolge verbindet den Azure Verbinder mit Ihrem lokalen System. 

1. Klicken Sie im [Azure klassischen Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)wählen Sie Ihre Dienstbus Namespace, und wählen Sie die **Verbindungsinformationen**:

    ![][SB_ConnectInfo]

2. Kopieren Sie die Verbindungszeichenfolge SAS an:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Installieren der Hybrid-Verbindungs-Manager

1. Wählen Sie den Verbinder, die, den Sie erstellt haben, im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Um ihn zu öffnen, können Sie wählen Sie **Durchsuchen**, wählen **API Apps**, und wählen Sie dann Ihre Verbinder oder API App. 
<br/><br/>
In **Hybrid Verbindung**ist die Einrichtung **unvollständig**:
<br/>
![][2] 

2. Wählen Sie **Hybrid Verbindung**. Die Verbindungszeichenfolge Dienstbus zuvor eingegebenen wird aufgeführt.
3. Kopieren Sie die **primäre Konfigurationszeichenfolge**an:
<br/>
![][PrimaryConfigString]

4. Unter **Lokale Hybrid Verbindungs-Managers**können Sie Herunterladen der Hybrid Verbindung-Manager oder es direkt aus dem Portal installieren. 
<br/><br/>
Wechseln Sie zum direkt aus dem Portal installiert haben, navigieren Sie zu Ihrem lokalen IIS-Server-Portal an, und wählen Sie **herunterladen und konfigurieren**.
<br/><br/>
Herunterladen der Hybrid-Verbindungs-Manager, wechseln Sie zu Ihrem lokalen IIS-Server, und wechseln Sie zu der **ClickOnce-Anwendung** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Die Installation startet automatisch, sodass Sie ausgeführt werden können.

5. Klicken Sie im Fenster **Zuhörer einrichten** Geben Sie die **Primäre Konfigurationszeichenfolge** zuvor (in Schritt 3 eingefügten), und wählen Sie **Installieren**.

Wenn das Setup abgeschlossen ist, die folgenden angezeigt:
<br/>
![][3] 

Wenn Sie den Verbinder erneut besuchen, ist der Verbindungsstatus Hybrid jetzt **verbunden**. Möglicherweise müssen Sie den Verbinder zu schließen und erneut:
<br/>
![][4] 

> [AZURE.NOTE] Um die sekundäre Verbindungszeichenfolge zu wechseln, führen Sie die Einrichtung der Hybrid Verbindung erneut aus, und geben Sie die **Sekundäre Konfigurationszeichenfolge**.


## <a name="tcp-ports-and-security"></a>TCP- und Sicherheit

Wenn Sie eine Verbindung Hybrid erstellen, wird eine Website auf dem lokalen lokalen IIS-Server erstellt. IIS-Server kann in einer DMZ handeln. Die TCP-Anschluss Anforderungen auf dem Server IIS umfassen:

TCP- | Warum
--- | ---
 | Es sind keine eingehenden TCP Ports erforderlich.
9350 - 9354 | Diese Ports werden für die Datenübertragung verwendet. Der Manager Dienstbus Relay untersucht Port 9350, um festzustellen, ob TCP-Konnektivität verfügbar ist. Sofern verfügbar, nimmt es, dass Port 9352 auch verfügbar ist. Datenverkehr geht über den Port 9352. <br/><br/>Ausgehende Verbindungen auf diese Ports zulassen.
5671 | Wenn Port 9352 für Datenverkehr verwendet wird, wird als Port 5671 verwendet die den Steuerelement erstellen. <br/><br/>Ausgehende Verbindungen zu diesem Port zulassen 
80, 443 | Wenn Ports 9352 und 5671 nicht verwendet werden können, werden *dann* Ports 80 und 443 fallbackabfragen Ports, die für die Datenübertragung und dem Steuerelement-Kanal verwendet.<br/><br/>Ausgehende Verbindungen auf diese Ports zulassen.
Klicken Sie auf Prem System port | Öffnen Sie den Port vom System verwendet, auf dem lokalen System. SQL Server verwendet, beispielsweise in der Regel Port 1433. Öffnen Sie diesen TCP-Anschluss.

[Eine Firewall mit Dienstbus Hostinganbieter](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Behandlung von Problemen

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Lokale Problembehandlung

1. Bestätigen Sie auf dem IIS-Server die IIS-Web-Rolle installiert ist und die IIS-Dienste gestartet werden.
2. Klicken Sie auf dem IIS-Server zu bestätigen, dass der Hybrid-Verbindungs-Manager installiert sein und ausgeführt wird:
 - Im IIS-Manager (Inetmgr), die Website ***MicrosoftAzureBizTalkHybridListener*** aufgelistet werden soll, und ausgeführt werden. 
 - Diese Website verwendet die ***HybridListenerAppPool*** , die als das lokale *Netzwerkdienst* integrierten-Benutzerkonto ausgeführt wird. Diese AppPool sollten auch gestartet werden.
3. Klicken Sie auf dem IIS-Server zu bestätigen, dass der Verbinder installiert und ausgeführt wird: 
 - Für den Verbinder wird eine Website erstellt. Angenommen, wenn Sie einen Verbinder SQL erstellt haben, besteht eine Website ***MicrosoftSqlConnector_nnn*** . Bestätigen Sie im IIS-Manager (Inetmgr), diese Website aufgeführt und gestartet ist. 
 - Diese Website verwendet einen eigenen IIS-Anwendung Ressourcenpool mit dem Namen ***HybridAppPoolnnn***. Diese AppPool wird als das lokale *Netzwerkdienst* integrierten-Benutzerkonto ausgeführt. Diese Website und AppPool sollten beide gestartet werden. 
 - Durchsuchen des lokalen Verbinders. Beispielsweise, wenn Ihre Website Verbinder Port 6569 verwendet, navigieren Sie zu Http://localhost:6569. Ein Standarddokument nicht konfiguriert ist, damit eine `HTTP Error 403.14 - Forbidden error` wird erwartet.
4. Bestätigen Sie in Ihrer Firewall die in diesem Artikel aufgeführten TCP-geöffnet sind.
5. Schauen Sie sich die Quell- oder Ziel-System:
 - Einige lokale Systeme erfordern zusätzliche Abhängigkeitsdateien. Angenommen, müssen Sie zu lokalen SAP eine Verbindung herstellen, einige zusätzlichen SAP-Dateien auf dem IIS-Server installiert sein.
 - Überprüfen Sie die Verbindung mit dem Anmeldekonto an das System. Beispielsweise muss der vom System verwendeten TCP-wie Anschluss 1433 für SQL Server geöffnet. Das Login-Konto im Azure-Portal eingegebene müssen Zugriff auf das System.
6. Aktivieren Sie auf dem IIS-Server den Ereignisprotokollen Fehler aus. 
7. Aufräumen und der Hybrid-Verbindungs-Manager erneut zu installieren: 
 - Im IIS müssen Sie manuell löschen Sie die Connector-Website und deren Anwendung Ressourcenpool. 
 - Führen Sie die Hybrid-Verbindungs-Manager erneut aus, und bestätigen Sie, dass Sie die richtige **Primäre Konfigurationszeichenfolge** für den Verbinder eingeben.



### <a name="in-the-azure-classic-portal"></a>In der klassischen Azure-portal

1. Der Namespace Dienstbus hat einen **aktiven** Zustand zu bestätigen.
2. Geben Sie die Verbindungszeichenfolge Service Bus SAS, beim Erstellen des Verbinders. Geben Sie die Verbindungszeichenfolge ACS nicht.


## <a name="faq"></a>Häufig gestellte Fragen

**Frage**: Es gibt zwei Hybrid Verbindungs-Manager. Was ist der Unterschied? 

**Antwort**: die [Hybrid Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) -Technologie, die hauptsächlich von Web Apps (vormals Websites) und Mobile-Apps (vormals mobile Services) verwendet wird, die Verbindung zu lokalen vorhanden ist. Diese Hybrid Verbindungen Manager ist eine eigene [Setup](../biztalk-services/integration-hybrid-connection-create-manage.md) und eines Azure BizTalk-Diensts (Hintergrundinformationen) verwendet. Nur TCP- und HTTP-Protokoll unterstützt.

Mit Azure-App-Verwaltungsdienst Verbinder haben wir auch einen Hybrid-Verbindungs-Manager.  Diese Hybrid-Verbindungs-Manager ist *nicht* verwenden, die eine BizTalk Azure Service (Hintergrundinformationen) und unterstützt mehr als TCP und HTTP. Lesen Sie [Verbindern und API Apps-Liste](app-service-logic-connectors-list.md).

Beides verwenden Azure-Dienstbus in Verbindung mit dem lokalen System.

**Frage**: beim Erstellen einer benutzerdefinierten API App kann ich mithilfe der App Dienst Hybrid Verbindungs-Manager auf eine lokale Verbindung? 

**Antwort**: nicht im herkömmlichen Sinn. Sie können einen integrierten Verbinder verwenden, Konfigurieren der App Dienst Hybrid Verbindungs-Manager für die Verbindung mit dem lokalen System. Klicken Sie dann verwenden Sie diesen Connector mit Ihrer benutzerdefinierten API App möglicherweise mit einer Logik App. Derzeit nicht entwickeln oder erstellen Ihre eigenen Hybrid API App (wie die SQL-Verbinder oder den Verbinder Datei).

Wenn Ihre benutzerdefinierten API einen TCP- oder HTTP-Port verwendet, können Sie [Hybrid Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und deren Hybrid-Verbindungs-Managers verwenden. In diesem Szenario wird eine Azure BizTalk-Dienst verwendet. [Verbinden mit lokalen SQL Server mit einem Web-app](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) kann hilfreich sein.  


## <a name="read-more"></a>Weitere Informationen

[Überwachen Sie Ihrer Apps Logik](app-service-logic-monitor-your-logic-apps.md)<br/>
[Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
