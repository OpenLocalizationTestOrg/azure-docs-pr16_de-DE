<properties
    pageTitle="Liste der verfügbaren Verbindern und API Apps | Microsoft Azure-App-Verwaltungsdienst"
    description="Erfahren Sie mehr über die Verbinder und API-Apps in Azure App-Verwaltungsdienst"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Liste von Verbindern und API Apps zur Verwendung in Ihrer Apps Logik
>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus. Die Version Logik Apps allgemeine Verfügbarkeit (GA) finden Sie unter [Neue Verbinder Liste](../connectors/apis-list.md).

Lernen Sie alle verfügbaren Connectors und API Apps erstellt von Microsoft zur Verwendung in Ihrer Apps Logik aus.

Preisinformationen und eine Liste der was für jede Ebene Service enthalten ist, finden Sie unter [Preise zu Azure App-Dienst](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Um mit Logik Apps anzufangen vor für ein Azure-Konto anmelden, wechseln Sie zu [Versuchen Logik App](https://tryappservice.azure.com/?appservice=logic). Sie können eine kurzlebige Starter Logik app sofort im App-Dienst erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="core-connectors"></a>Core Verbinder
Der folgenden Tabelle sind alle verfügbaren Connectors und Microsoft erstellte API-Apps, die als Core Connectors verfügbar sind:

Namen | Beschreibung
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Verwenden von Bing zum Übersetzen von Text in einer anderen Sprache.
[HTTP](app-service-logic-connector-http.md) | Die HTTP-Zuhörer öffnet einen Endpunkt, der als HTTP-Server fungiert und überwacht eingehenden HTTP oder HTTPS-Anfragen. Die Aktion HTTP-API App setzt voraus und ist systembedingt innerhalb Logik Apps unterstützt.
[Pufferzeit](app-service-logic-connector-slack.md) | Verbinden mit Pufferzeit und Nachrichten an Pufferzeit Kanäle senden.


## <a name="enterprise-integration-connectors"></a>Enterprise-Integration von Verbindern
Der folgenden Tabelle sind alle verfügbaren Verbindern und API Apps erstellte Microsoft als Enterprise-Integration von Verbindern zur Verfügung:

Namen  | Beschreibung
------------- | -------------
[BizTalk-Regeln](app-service-logic-use-biztalk-rules.md) | Verwenden von BizTalk Regeln definieren und Geschäftslogik innerhalb einer Organisation steuern. Richtlinien für Unternehmen können ohne zu kompilieren oder ohne erneutes Bereitstellen der zugehörigen Anwendung aktualisiert werden.
[BizTalk XPath extrahieren](app-service-logic-xpath-extract.md) | Sucht und extrahiert Daten aus dem XML-Inhalt auf Grundlage einer XPath-wählen Sie aus.
[DB2-Connector](app-service-logic-connector-db2.md) | Herstellen der Verbindung zu einer IBM DB2-Datenbank lokal, und klicken Sie auf eine Azure-virtuellen Computern ein Windows-Betriebssystem ausführen. Informix Structured Query Language Befehle können Web-API und OData-API Vorgängen zugeordnet werden. <br/><br/>Keine Trigger. Aktionen umfassen Tabelle auswählen, einfügen, aktualisieren, löschen und benutzerdefinierte-Anweisung<br/><br/>Diesen Connector enthält darüber hinaus den Microsoft-Client für DRDA in einem Netzwerk TCP/IP eine Verbindung zu einem Informix-Server.
[Datei](app-service-logic-connector-file.md) | Mit diesem Connector, können Sie mit dem lokalen Dateisystem oder Netzwerk und abgeschlossen verschiedenen Dateiformaten Aufgaben, einschließlich hochladen, löschen, Auflisten von Dateien und verbinden.
[Informix](app-service-logic-connector-informix.md) | Herstellen der Verbindung mit einer IBM Informix-Datenbank lokal und auf einer Azure-virtuellen Computern mit einem Windows-Betriebssystem. Informix Structured Query Language Befehle können Web-API und OData-API Vorgängen zugeordnet werden.<br/><br/>Keine Trigger. Zählen Tabelle auswählen, einfügen, aktualisieren, löschen und benutzerdefinierte-Anweisung.<br/><br/>Bei der Verwendung von lokalen kann VPN oder Azure ExpressRoute verwendet werden. Diesen Connector enthält darüber hinaus eine Microsoft-Client für DRDA in einem Netzwerk TCP/IP eine Verbindung zu einem Informix-Server.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Herstellen der Verbindung zu lokalen SQLServer oder einer SQL Azure-Datenbank. Sie können erstellen, aktualisieren, abrufen und löschen Sie die Einträge in der Tabelle eine SQL-Datenbank.
MQ | Herstellen der Verbindung zu IBM WebSphere MQ Server Version 8, lokalen und eine Azure-virtuellen Computern mit einem Windows-Betriebssystem. Bei der Verwendung von lokalen kann VPN oder Azure ExpressRoute verwendet werden. Der Verbinder enthält auch den Microsoft-Client für MQ.<br/><br/>Keine Trigger. Keine Aktionen.<br/><br/>**Notiz** Zurzeit können mit Logik Apps verwendet werden.

## <a name="connectors-as-triggers"></a>Verbinder als Trigger
Mehrere Connectors bieten Trigger für Logik Apps. Es gibt zwei Arten von diese Trigger:

1. Umfrage Trigger: Diese Trigger Umfrage unter dem Dienst mit einer bestimmten Häufigkeit für neue Daten zu überprüfen. Wenn Sie neue Daten verfügbar ist, wird eine neue Instanz der App Logik mit den Daten als Eingabe ausgeführt. Um zu verhindern, dass dieselben Daten verarbeitet werden mehrmals, der Trigger möglicherweise bereinigen Daten, die mit der Logik übergeben und gelesen wurde. Beispiele für solche Verbinder sind Datei und SQL Azure-Speicher.
2. Pushbenachrichtigungen Trigger: Diese Trigger Abhören für Daten für einen Endpunkt oder ein Ereignis auftritt. Klicken Sie dann eine neue Instanz einer App Logik ausgelöst werden. Beispiele für solche Verbinder sind HTTP Zuhörer und Twitter.

## <a name="connectors-as-actions"></a>Verbinder als Aktionen
Verbinder können auch als Aktionen innerhalb der App Logik verwendet werden. Aktionen sind nützlich für die Suche nach Daten innerhalb der Logik App, die können dann bei der Ausführung verwendet. Möglicherweise müssen Sie beispielsweise Nachschlagen von Daten aus einer SQL-Datenbank für Weitere Informationen zu einem Kunden bei der Verarbeitung von Ordnung. Oder möglicherweise schreiben, aktualisieren und Löschen von Daten in einem Ziel müssen. Sie können dazu die Aktionen, die von der Verbinder bereitgestellt. Aktionen zuordnen Operationen in API-Apps (wie ihre Swagger Metadaten definiert).

## <a name="create-your-own-connectors-and-api-apps"></a>Erstellen Sie eigener Verbindern und API-Apps
[Verbinder und API-Apps-Referenz](http://aka.ms/appservicesconnectorreference)  
[Azure Service-API App app Trigger](../app-service-api/app-service-api-dotnet-triggers.md)  
[Logik App Bezug](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Weitere Informationen zum Verbindern und API-Apps
[Was sind Verbindern und BizTalk-API Apps](app-service-logic-what-are-biztalk-api-apps.md)  
[Verwenden die Hybrid-Verbindungs-Managers in Azure App-Verwaltungsdienst](app-service-logic-hybrid-connection-manager.md)  
[Verwalten Sie und überwachen Sie Ihrer integrierten API Apps und Verbinder](app-service-logic-monitor-your-connectors.md)
