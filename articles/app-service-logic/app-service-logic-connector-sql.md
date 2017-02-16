<properties
   pageTitle="Verwenden den SQL-Verbinder in Logik Apps | Microsoft Azure-App-Verwaltungsdienst"
   description="So erstellen und Konfigurieren der SQL-Verbinder oder API-app und in einer app Logik in Azure-App-Dienst verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit der Microsoft SQL-Verbinder, und fügen Sie es zu Ihrer Anwendung Logik
>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus. Klicken Sie auf der Azure SQL-2015-08-01-Schema Vorschauversion [SQL Azure-API](../connectors/connectors-create-api-sqlazure.md).

Herstellen einer Verbindung mit einer lokalen SQL Server oder einer SQL Azure-Datenbank zum Erstellen und Ändern der Informationen oder Daten. Connectors können Logik Apps abzurufen, Prozess, oder drücken Sie die Daten als Teil eines "Workflows" verwendet werden. Wenn Sie den SQL-Verbinder in den Workflow verwenden, können Sie eine Vielzahl von Szenarien erzielen. Beispielsweise können Sie:

- Verfügbar machen eines Abschnitts, der die Daten in der SQL-Datenbank mit einem Web oder mobilen Anwendung aus.
- Einfügen von Daten in einer SQL-Datenbanktabelle Storage. Angenommen, können Sie Mitarbeiterdatensätze eingeben, Aktualisieren von Aufträgen usw..
- Abrufen von Daten aus SQL und diese in einem Geschäftsprozess verwenden. Sie können beispielsweise Kundendatensätze abrufen und setzen die Kundendatensätze in Vertrieb.

Sie können Ihre Geschäftsdaten Workflow und Prozess als Teil dieser Workflow innerhalb einer App Logik der SQL-Verbinder hinzufügen. 

## <a name="triggers-and-actions"></a>Trigger und Aktionen
*Trigger* sind Ereignisse, die ausgeführt werden. Beispielsweise wird bei der Aktualisierung Ordnung oder einen neuen Kunden hinzugefügt. Eine *Aktion* ist das Ergebnis des Triggers. Wenn Ordnung aktualisiert wird, senden Sie eine Benachrichtigung an den Verkäufer fest. Oder, wenn ein neuer Kunde hinzugefügt wird, senden eine Willkommen e-Mail-Nachricht an den neuen Kunden.

SQL-Connector kann als einen Trigger oder eine Aktion in einer Logik app und unterstützt in JSON und XML-Formaten verwendet werden. Für jede Tabelle in das Paket eingeschlossen Einstellungen (mehr dazu weiter unten in diesem Thema), es gibt eine Reihe von JSON-Aktionen und eine Reihe von XML-Aktionen.

Der SQL-Verbinder weist die folgenden Trigger und Aktionen:

Trigger | Aktionen
--- | ---
Umfrage Daten | <ul><li>In Tabelle einfügen</li><li>Aktualisierungstabelle</li><li>Wählen Sie aus der Tabelle</li><li>Tabelle löschen</li><li>Rufen Sie gespeicherte Prozedur</li></ul>

## <a name="create-the-sql-connector"></a>Erstellen des SQL-Verbinders

Ein Verbinder innerhalb einer Logik app erstellt werden kann oder direkt aus dem Azure Marketplace erstellt werden. So erstellen Sie einen Verbinder aus dem Marketplace  

1. Wählen Sie in der Azure Startboard **Marketplace**aus.
2. Suchen Sie nach "SQL-Connector", wählen Sie ihn aus, und wählen Sie **Erstellen**.
3. Geben Sie den Namen, Planen der App-Dienst und andere Eigenschaften ein.
4. Geben Sie die folgenden paketeinstellungen aus:

    Namen | Erforderlich |  Beschreibung
--- | --- | ---
Servername | Ja | Geben Sie den SQL Server-Namen ein. Geben Sie zum Beispiel *SQLserver/SQL Express* oder *SQLserver.mydomain.com*ein.
Port | Nein | Standardmäßig ist 1433.
Benutzername | Ja | Geben Sie einen Benutzernamen, der sich anmelden kann, in der SQL Server. Wenn Sie eine Verbindung mit einer lokalen SQL Server herstellen möchten, geben Sie Anmeldeinformationen für die SQL-Authentifizierung.
Kennwort | Ja | Geben Sie den Benutzer und das Kennwort ein.
Datenbankname | Ja | Geben Sie die Datenbank, die Sie eine Verbindung herstellen. Beispielsweise können Sie die *Kunden* oder *Dbo/Bestellungen*eingeben.
Lokal | Ja | Standardmäßig ist falsch. Geben Sie auf falsch, wenn eine Verbindung mit einer SQL Azure-Datenbank herstellen. Geben Sie True, wenn eine Verbindung mit einer lokalen SQL Server herstellen.
Service Bus Verbindungszeichenfolge | Nein | Wenn Sie zu der lokalen herstellen, geben Sie die Dienstbus Relay Verbindungszeichenfolge.<br/><br/>[Mithilfe der Hybrid-Verbindungs-Managers](app-service-logic-hybrid-connection-manager.md)<br/>[Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/)
Partner-Servernamens | Nein | Wenn der primäre Server nicht verfügbar ist, können Sie einen Partnerserver als Alternative oder zusätzliche Server eingeben.
Tabellen | Nein | Liste der Datenbanktabellen, die durch die Verbindung aktualisiert werden können. Geben Sie zum Beispiel *OrdersTable* oder *EmployeeTable*ein. Wenn keine Tabellen eingegeben werden, können alle Tabellen verwendet werden. Gültiger Tabellen und/oder gespeicherte Prozeduren sind erforderlich, um diesen Connector als eine Aktion verwenden.
Gespeicherten Prozeduren | Nein | Geben Sie eine vorhandene gespeicherte Prozedur, die von der Verbinder aufgerufen werden kann. Geben Sie beispielsweise *Sp_IsEmployeeEligible* oder *Sp_CalculateOrderDiscount*. Gültiger Tabellen und/oder gespeicherte Prozeduren sind erforderlich, um diesen Connector als eine Aktion verwenden.
Verfügbare Datenabfrage | Für die Unterstützung von auslösen | SQL-Anweisung, um festzustellen, ob alle Daten für das Abrufen einer SQL Server-Datenbanktabelle verfügbar sind. Dadurch sollte einen numerischen Wert, der die Anzahl von Zeilen mit Daten zur Verfügung zurückgegeben. Beispiel: SELECT COUNT(*) aus Tabellenname.
Umfrage Datenabfrage | Für die Unterstützung von auslösen | Die SQL-Anweisung in der SQL Server-Datenbanktabelle Umfrage. Sie können eine beliebige Anzahl von SQL-Anweisungen durch ein Semikolon getrennt eingeben. Diese Anweisung ist überführen ausgeführt und nur gespeichert wird, wenn die Daten in Ihrer app Logik sicher gespeichert sind. Beispiel: Wählen Sie *aus Tabellenname; Löschen von Tabellenname. <br/>**Note**<br/>Sie müssen eine Umfrage-Anweisung angeben, die eine unbegrenzte Schleife beseitigt, nach dem Löschen, verschieben oder aktualisieren die ausgewählte Daten, um sicherzustellen, dass dieselbe Daten erneut abgefragt nicht zur Verfügung.

5. Wenn Sie fertig sind, Aussehen der Paketeinstellungen ähnlich wie die folgende:  
![][1]  

6. Wählen Sie auf **Erstellen**. 


## <a name="use-the-connector-as-a-trigger"></a>Verwenden Sie den Verbinder als Trigger
Sehen wir uns eine einfache Logik-app, die fragt Daten aus einer SQL-Tabelle, die Daten aus einer anderen Tabelle hinzugefügt und die Daten aktualisiert.

Um den SQL-Connector als Trigger verwenden, geben Sie die **Datenabfrage verfügbar** und **Umfrage Datenabfrage** Werte aus. **Verfügbare Datenabfrage** wird auf den Terminplan ausgeführt, Sie eingeben und bestimmt, ob alle Daten verfügbar sind. Da diese Abfrage nur eine skalare Zahl zurückgibt, können Sie optimiert und für häufige Ausführung optimiert werden.

**Umfrage Datenabfrage** nur ausgeführt, wenn die Datenabfrage verfügbar gibt an, dass die Daten verfügbar sind. Diese Anweisung innerhalb einer Transaktion ausgeführt wird und steht nur gespeichert wird, wenn die extrahierten Daten in den Workflow als gespeichert ist. Es ist wichtig, vermeiden endlos erneut dieselben Daten zu extrahieren. Löschen oder aktualisieren die Daten, um sicherzustellen, dass es das nächste Mal gesammelt nicht zur Verfügung, die Daten abgefragt werden, kann des Transaktionsverhaltens der dieser Ausführung verwendet werden.

> [AZURE.NOTE] Diese Anweisung zurückgegebene Schema identifiziert die verfügbaren Eigenschaften in den Verbinder. Alle Spalten müssen den Namen.

#### <a name="data-available-query-example"></a>Beispiel für die Abfrage verfügbar von Daten

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Umfrage Abfrage Daten (Beispiel)

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Fügen Sie dem Trigger hinzu
1. Beim Erstellen oder Bearbeiten einer app Logik, wählen Sie als das Auslösen der SQL-Verbinder, die Sie erstellt haben. Dadurch werden die verfügbaren Trigger aufgelistet: **Umfrage Daten (JSON)** und **Umfrage Daten (XML)**:  
![][5]

2. Markieren Sie die **Umfrage Daten (JSON)** auslösen, geben Sie die Häufigkeit aus, und klicken Sie auf die ✓:  
![][6]

3. Der Trigger wird nun angezeigt, wie in der Logik app konfiguriert. Die Output(s) des Triggers werden angezeigt und kann als Eingaben in alle nachfolgenden Aktionen verwendet werden:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Verwenden Sie den Verbinder als eine Aktion
Mit unseren einfachen Logik app Szenario, das Daten aus einer SQL-Tabelle abfragt fügt die Daten in einer anderen Tabelle hinzu, und die Daten aktualisiert.

Um den SQL-Connector als eine Aktion verwenden möchten, geben Sie den Namen der Tabellen und/oder gespeicherte Prozeduren, die Sie eingegeben haben, wenn Sie den SQL-Connector erstellt haben:

1. Nach der Trigger (oder wählen Sie "diese Logik manuell ausführen"), fügen Sie der SQL-Verbinder, die Sie erstellt, aus dem Katalog haben hinzu. Wählen Sie eine der Aktionen einfügen, wie *Einfügen in TempEmployeeDetails JSON ()*:  
![][8]

2. Geben Sie die Eingabewerte des Datensatzes eingefügt werden, und klicken auf die ✓ aus:  
![][9]

3. Wählen Sie im Katalog des gleichen SQL-Verbinders, die, den Sie erstellt haben. Wählen Sie als Aktion klicken Sie auf der gleichen Tabelle, wie *Update EmployeeDetails*Aktion aktualisieren:  
![][11]

4. Geben Sie die Eingabewerte für die Aktualisierungsaktion, und klicken Sie auf der ✓ auf:  
![][12]

Durch Hinzufügen eines neuen Datensatzes in der Tabelle, die abgerufen werden, können Sie die app Logik testen.

## <a name="what-you-can-and-cannot-do"></a>Was können und kann nicht

SQL-Abfrage | Unterstützt | Nicht unterstützt
--- | --- | ---
Wo Klausel | <ul><li>Operatoren: Und, oder, =, <>, <, < =, >, > = und wie</li><li>Mehrere Sub Konditionen können kombiniert werden, indem '(' und')'</li><li>Zeichenfolge: Literalen, "DateTime" (eingeschlossene in einfache Anführungszeichen), Zahlen (darf nur numerische Zeichen enthalten)</li><li>Grundsätzlich sollten in einem Format binären Ausdruck wie ((operand operator operand) und/oder (Operand Operator Operand)) *</li></ul> | <ul><li>Operatoren: Zwischen, IN</li><li>Alle integrierte Funktionen wie ADD(), MAX() jetzt()", POWER(), usw.</li><li>Mathematische Operatoren wie *, -, +, usw.</li><li>Verwenden von Zeichenfolgen-Konkatinierungen +.</li><li>Alle Verknüpfungen</li><li>IST NULL und ist nicht Null</li><li>Alle Zahlen mit nicht numerischen Zeichen, wie hexadezimale Zahlen</li></ul>
Felder (in der Select-Abfrage) | <ul><li>Gültige Spaltennamen durch Kommas getrennt ein. Keine Tabelle Präfixe zulässig (Verbinder Works auf jeweils einer Tabelle).</li><li>Namen mit Escapezeichen umgewandelt werden ' [' und ']'</li></ul> | <ul><li>Schlüsselwörter wie TOP, DISTINCT, usw.</li><li>Verwendung von Aliasen, wie Straße, Ort + Zip-AS-Adresse</li><li>Alle integrierte Funktionen, wie ADD(), MAX() jetzt()", POWER(), usw.</li><li>Mathematische Operatoren, wie *, -, +, usw.</li><li>Verwenden von Zeichenfolgen-Konkatinierungen +</li></ul>

#### <a name="tips"></a>Tipps

- Es wird empfohlen, für erweiterte Abfragen erstellen eine gespeicherte Prozedur und mit dem Verfahren ausführen Gespeicherte API ausführen.
- Bei der Verwendung von innerer Abfragen, die verwenden Sie diese innerhalb von gespeicherten Prozeduren.
- Für die Teilnahme an mehrere Konditionen, können Sie das 'und' und 'Oder' Operatoren.

## <a name="hybrid-configuration-optional"></a>Hybrid-Konfiguration (Optional)

> [AZURE.NOTE] Dieser Schritt ist erforderlich, nur bei Verwendung von SQL Server lokal hinter der Firewall.

App-Dienst verwendet den Hybrid-Konfigurations-Manager eine sichere Verbindung zu Ihrem lokalen System herstellen. Wenn Sie Verbinder Verwendung einer lokalen SQL Server befinden, ist der Hybrid-Verbindungs-Manager erforderlich.

> [AZURE.NOTE] Wenn Sie mit Azure Logik Apps Schritte, bevor Sie für ein Azure-Konto anmelden möchten, wechseln Sie zu [Logik App versuchen Sie](https://tryappservice.azure.com/?appservice=logic), wo Sie eine kurzlebige Starter Logik app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

[Mithilfe der Hybrid-Verbindungs-Managers](app-service-logic-hybrid-connection-manager.md)finden Sie unter.


## <a name="do-more-with-your-connector"></a>Führen Sie den Verbinder
Jetzt, da der Verbinder erstellt wurde, können Sie es mit einem Business Workflow mit einer Logik App hinzufügen. Finden Sie unter [Was Logik Apps werden?](app-service-logic-what-are-logic-apps.md).

Zeigen Sie die Swagger REST-API-Referenz bei [Verbindern und API Apps Bezug](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung von Statistiken und Steuerelement Sicherheit des Verbinders überprüfen. Finden Sie unter [Verwalten und Überwachen Ihrer integrierten API Apps und Verbinder](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
