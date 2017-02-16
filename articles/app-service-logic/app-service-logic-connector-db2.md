<properties
   pageTitle="Verwenden den Verbinder DB2 in Microsoft Azure-App-Dienst | Microsoft Azure"
   description="So verwenden Sie den Verbinder DB2 mit Logik app Trigger und Aktionen"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="db2-connector"></a>DB2-connector
>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus.

Microsoft-Connector für DB2 ist eine app API zum Herstellen einer Applikationen bis Azure-App-Verwaltungsdienst Ressourcen, die in einer IBM DB2-Datenbank gespeichert. Verbinder enthält einen Microsoft-Client für die Verbindung zu Remotecomputern DB2-Server in einem Netzwerk TCP/IP, einschließlich Azure Hybrid Verbindungen mit lokalen DB2-Server mit dem Azure Service Bus Relay Netzwerke unterstützt die folgenden Datenbankvorgänge:

- Lesen von Zeilen mit auswählen
- Umfrage zum Lesen von Zeilen mit Wählen Sie Anzahl gefolgt von auswählen
- Fügen Sie eine Zeile oder mehrere (Massen) Zeilen mit einfügen
- Ändern Sie eine Zeile oder mehrere (Massen) Zeilen mit UPDATE
- Entfernen Sie eine Zeile oder mehrere (Massen) Zeilen mit löschen
- Lesen, zu ändern, wählen Sie CURSOR gefolgt von UPDATE WHERE CURRENT OF CURSOR mit Zeilen
- So entfernen Sie Zeilen mit Wählen Sie CURSOR gefolgt von UPDATE WHERE CURRENT OF CURSOR gelesen
- Führen Sie Verfahren Eingabe- und Parameter, Rückgabewert, Resultset, mithilfe von ANRUFEN
- Benutzerdefinierte Befehle und zusammengesetzte Vorgänge mit auswählen, einfügen, aktualisieren, löschen

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Verbinder unterstützt die folgenden Logik app Trigger und Aktionen:

Trigger | Aktionen
--- | ---
<ul><li>Umfrage Daten</li></ul> | <ul><li>Massen einfügen</li><li>Einfügen</li><li>Massenaktualisieren</li><li>Aktualisieren</li><li>Anrufen</li><li>Massen löschen</li><li>Löschen</li><li>Wählen Sie aus</li><li>Bedingte aktualisieren</li><li>Bereitstellen auf EntitySet</li><li>Bedingte löschen</li><li>Wählen Sie einzelne Einheit aus.</li><li>Löschen</li><li>Upsert zu EntitySet</li><li>Benutzerdefinierte Befehle</li><li>Zusammengesetzte Operationen</li></ul>


## <a name="create-the-db2-connector"></a>Erstellen des DB2-Verbinders
Sie können einen Verbinder innerhalb einer app Logik oder aus dem Azure Marketplace, wie im folgenden Beispiel definieren:  

1. Wählen Sie in der Azure Startboard **Marketplace**aus.
2. Geben Sie im Feld **Suche alles** **db2** in das Blade **Alles** , und klicken Sie dann auf die EINGABETASTE.
3. Die Suche ergibt alles Bereich, und wählen **DB2 Verbinder**.
4. Wählen Sie in der DB2 Verbinder Beschreibung Blade **Erstellen**aus.
5. Geben Sie in das Paket-Blade DB2 Verbinder den Namen (z. B. "Db2ConnectorNewOrders"), Planen der App-Dienst und andere Eigenschaften aus.
6. Wählen Sie **paketeinstellungen**aus, und geben Sie die folgenden paketeinstellungen:  

    Namen | Erforderlich |  Beschreibung
--- | --- | ---
ConnectionString | Ja | DB2-Client-Verbindungszeichenfolge (z. B. "Network Address = Servername; Netzwerk-Port = 50000; Benutzer-ID = Username; Kennwort = Kennwort; Anfangskatalogs = Stichprobe; Verpacken Websitesammlungen = NWIND; Standard Schema NWIND = ").
Tabellen | Ja | Durch Trennzeichen getrennte Liste der Tabelle, anzeigen und alias-Namen, die erforderlich sind für OData-Vorgänge und generieren swagger Dokumentation mit Beispielen (z. B. "*NEWORDERS*").
Verfahren | Ja | Durch Trennzeichen getrennte Liste der Prozedur und Funktion Namen (z. B. "SPORDERID").
Lokale Edition | Nein | Bereitstellen von lokalen Azure Service Bus Relay verwenden.
ServiceBusConnectionString | Nein | Azure Service Bus Relay Verbindungszeichenfolge.
PollToCheckData | Nein | Wählen Sie COUNT-Anweisung mit einer Logik app Trigger verwenden (z. B. "SELECT Anzahl (\*) von NEWORDERS WHERE SHIPDATE IS NULL").
PollToReadData | Nein | SELECT-Anweisung mit einer Logik app Trigger verwenden (z. B. "Wählen Sie \* aus NEWORDERS, wo finde ich SHIPDATE NULL für UPDATE").
PollToAlterData | Nein | Aktualisieren oder DELETE-Anweisung zur Verwendung mit einem Logik app auslösen (z. B. "UPDATE NEWORDERS festlegen SHIPDATE = aktuelle Datum WHERE CURRENT OF &lt;CURSOR&gt;").

7. Wählen Sie **OK**aus, und wählen Sie dann auf **Erstellen**.
8. Wenn Sie fertig sind, Aussehen der Paketeinstellungen ähnlich wie die folgende:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logik app mit DB2 Verbinder Aktion zum Hinzufügen von Daten ##
Sie können eine Logik app Aktion zum Hinzufügen von Daten zu einer DB2-Tabelle mit einer API einfügen oder Beitrag Entität OData-Operation definieren. Beispielsweise können Sie eine neue Reihenfolge Kundendatensatzes durch Einfügen Verarbeitung einer Einfügen von SQL-Anweisung für eine Tabelle mit einer Identitätsspalte definiert Zurückgeben der Identitätswert oder die Zeilen wirken sich auf die app Logik (ORDID aus endgültige Tabelle auswählen (in NWIND einfügen. NEWORDERS (CUSTID SHIPNAME, AUFGENOMMENE PERSON, LIEFERORT, SHIPREG, SHIPZIP) WERTE (?,?,?,?,?,?))).

> [AZURE.TIP] DB2-Verbindung "*Bereitstellen auf EntitySet*" gibt den Spaltenwert Identität und "*Einfügen API*" gibt Zeilen betroffen

1. Wählen Sie in der Azure Startboard **+** (Pluszeichen) **Web + Mobile**, und klicken Sie dann **Logik app**.
2. Geben Sie den Namen (z. B. "NewOrdersDb2"), App Dienst planen, andere Eigenschaften, und wählen Sie dann auf **Erstellen**.
3. Wählen Sie in der Azure Startboard der Logik app aus, die Sie soeben erstellt haben, **Einstellungen**, und klicken Sie dann **Trigger und Aktionen**.
4. Wählen Sie das Blade Trigger und Aktionen innerhalb der app Logik Vorlagen **ganz neu erstellen** .
5. Im Bereich Apps-API wählen Sie **Wiederholung**, legen Sie einen Häufigkeit und Intervall, und klicken Sie dann **Häkchen**.
6. Klicken Sie im Bereich Apps-API wählen Sie **DB2 Verbinder aus**, erweitern Sie die Liste der Vorgänge zum **Einfügen in NEWORDER**auswählen.
7. Erweitern Sie die Parameterliste Geben Sie die folgenden Werte aus:  

    Namen | Wert
--- | --- 
CUSTID | 10042
SHIPNAME | Verzögerte K Kountry Store 
AUFGENOMMENE PERSON | 12 Orchester Terrasse
LIEFERORT | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Wählen Sie zum Speichern der Einstellungen von Objektaktion, und **Speichern Sie**die **Häkchen** aus.
9. Die Einstellungen sollte wie folgt aussehen:  
![][3]

10. Wählen Sie in der Liste **Alle ausgeführt wird** unter **Vorgänge**zuerst aufgeführten Element (die letzte Ausführung) ein. 
11. Wählen Sie das Blade **Logik app ausführen** der **Aktion** Element **db2connectorneworders**.
12. Klicken Sie in das Blade **Logik app Aktion** **EINGABEN LINK**ein. DB2 Verbinder verwendet die Eingaben, um eine parametrisierte INSERT-Anweisung zu verarbeiten.
13. Klicken Sie in das Blade **Logik app Aktion** **Ausgaben LINK**ein. Die Eingaben sollte wie folgt aussehen:  
![][4]

#### <a name="what-you-need-to-know"></a>Was Sie sonst noch wissen

- Verbinder kürzt DB2 Tabellennamen beim Logik app Aktionsnamen bilden. Beispielsweise wird der Vorgang **in NEWORDERS einfügen** zum **Einfügen von NEWORDER**abgeschnitten.
- Nach dem Speichern der app Logik **Trigger und Aktionen**verarbeitet Logik app den Vorgang an. Möglicherweise eine Verzögerung eine Anzahl von Sekunden (z. B. 3 bis 5 Sekunden), bevor Sie Logik app verarbeitet den Vorgang. Optional können Sie **Jetzt ausführen,** um den Vorgang zu verarbeiten klicken.
- DB2 Verbinder definiert EntitySet Mitglieder mit Attributen, z. B., ob das Element einer DB2-Spalte mit einem Standardwert entspricht oder generiert Spalten (z. B. Identität). Logik app zeigt ein rotes Sternchen neben dem EntitySet Mitglied-ID-Namen, um DB2 Spalten zu kennzeichnen, die Werte erfordern. Sie sollten keinen Wert für das Element ORDID eingeben der DB2 Identitätsspalte entspricht. Sie können Werte für andere optionale Mitglieder (Elemente, ORDDATE, REQDATE, SHIPID, Fracht, SHIPCTRY) eingeben, die DB2 Spalten mit Standardwerten entsprechen. 
- DB2 Verbinder gibt zu Logik app die Antwort auf den Beitrag zu EntitySet, die die Werte für Identitätsspalten, die von der DRDA SQLDARD (SQL Daten Bereich Antwortdaten) abgeleitet wird auf der vorbereiteter Einfügen von SQL-Anweisung enthält. DB2-Server gibt die eingefügten Werte für Spalten mit Standardwerten nicht zurück.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logik app mit DB2 Verbinder Aktion Datenmengen hinzufügen ##
Sie können eine Logik app Aktion zum Hinzufügen von Daten zu einer DB2-Tabelle mit einer API Massen einfügen-Operation definieren. Beispielsweise können Sie zwei neue Reihenfolge Kundendatensätze, einfügen, indem Sie Verarbeitung einer Einfügen von SQL-Anweisung mit einer Matrix zurück, der Zeilenwerte für eine Tabelle mit einer Identitätsspalte definiert zurückgeben die Zeilen wirken sich auf die app Logik (ORDID aus endgültige Tabelle auswählen (in NWIND einfügen. NEWORDERS (CUSTID SHIPNAME, AUFGENOMMENE PERSON, LIEFERORT, SHIPREG, SHIPZIP) WERTE (?,?,?,?,?,?))).

1. Wählen Sie in der Azure Startboard **+** (Pluszeichen) **Web + Mobile**, und klicken Sie dann **Logik app**.
2. Geben Sie den Namen (z. B. "NewOrdersBulkDb2"), App Dienst planen, andere Eigenschaften, und wählen Sie dann auf **Erstellen**.
3. Wählen Sie in der Azure Startboard der Logik app aus, die Sie soeben erstellt haben, **Einstellungen**, und klicken Sie dann **Trigger und Aktionen**.
4. Wählen Sie das Blade Trigger und Aktionen innerhalb der app Logik Vorlagen **ganz neu erstellen** .
5. Im Bereich Apps-API wählen Sie **Wiederholung**, legen Sie einen Häufigkeit und Intervall, und klicken Sie dann **Häkchen**.
6. Klicken Sie im Bereich Apps-API wählen Sie **DB2 Verbinder aus**, erweitern Sie die Liste der Vorgänge zum **Einfügen von Massen in neu**auswählen.
7. Geben Sie den Wert für die **Zeilen** als Array. Beispielsweise Kopieren Sie, und fügen Sie die folgenden:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Wählen Sie zum Speichern der Einstellungen von Objektaktion, und **Speichern Sie**die **Häkchen** aus. Die Einstellungen sollte wie folgt aussehen:  
![][6]

9. Klicken Sie in der Liste **Alle ausgeführt wird** unter **Vorgänge**auf das Element zuerst aufgeführten (die letzte Ausführung).
10. Klicken Sie in das Blade **Logik app ausführen** der **Aktion** Element auf.
11. Klicken Sie auf den **LINK EINGABEN**, in das Blade **Logik app Aktion** . Die Ausgaben sollte wie folgt aussehen:  
[][7]
12. Klicken Sie auf den **LINK Ausgaben**, in das Blade **Logik app Aktion** . Die Ausgaben sollte wie folgt aussehen:  
![][8]

#### <a name="what-you-need-to-know"></a>Was Sie sonst noch wissen

- Verbinder kürzt DB2 Tabellennamen beim Logik app Aktionsnamen bilden. Beispielsweise wird der Vorgang **Massen in NEWORDERS einfügen** , **Massen in neu einzufügen**abgeschnitten.
- Durch das Auslassen Identitätsspalten (z. B. ORDID), NULL-Werte zulässt Spalten (z. B. SHIPDATE) und Spalten mit Standardwerten (z. B. ORDDATE, REQDATE, SHIPID, Fracht, SHIPCTRY), generiert DB2-Datenbank Werte.
- Durch die Angabe "heute" und "morgen", DB2-Connector generiert "Datum" und "Aktuelles Datum + 1 Tag" Funktionen (z. B. REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logik app mit DB2 Verbinder Trigger zum Lesen, ändern oder Löschen von Daten ##
Sie können einen Logik app Trigger zum Umfrage und zum Lesen von Daten aus einer DB2-Tabelle mit einer API Umfrage Daten composite-Vorgang definieren. Sie können beispielsweise lesen eine oder mehrere neue Reihenfolge Kundendatensätze, die Datensätze mit der Logik zurückzugeben. DB2 Verbindungseinstellungen Paket/app sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) aus, wo SHIPDATE NULL ist, NEWORDERS
PollToReadData | Wählen Sie \* aus NEWORDERS, wo finde ich SHIPDATE NULL für aktualisieren,
PollToAlterData | <no value specified>


Sie können auch eine Logik app auslösen, um die Umfrage, lesen und Ändern von Daten in einer DB2-Tabelle mit einer API Umfrage Daten composite-Vorgang definieren. Sie können beispielsweise lesen eine oder mehrere neue Reihenfolge Kundendatensätze, Aktualisieren der Zeilenwerte (vor Aktualisierung) ausgewählten Datensätze mit der Logik zurückgeben. DB2 Verbindungseinstellungen Paket/app sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) aus, wo SHIPDATE NULL ist, NEWORDERS
PollToReadData | Wählen Sie \* aus NEWORDERS, wo finde ich SHIPDATE NULL für aktualisieren,
PollToAlterData | UPDATE NEWORDERS festlegen SHIPDATE = aktuelles Datum, in dem der aktuellen &lt;CURSOR&gt;


Darüber hinaus können Sie einen app-Trigger Logik, um Umfrage, lesen und Entfernen von Daten aus einer DB2-Tabelle mit einer API Umfrage Daten composite-Vorgang definieren. Sie können beispielsweise lesen eine oder mehrere neue Reihenfolge Kundendatensätze, löschen Sie die Zeilen (vor dem Löschen) ausgewählten Datensätze mit der Logik zurückgeben. DB2 Verbindungseinstellungen Paket/app sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) aus, wo SHIPDATE NULL ist, NEWORDERS
PollToReadData | Wählen Sie \* aus NEWORDERS, wo finde ich SHIPDATE NULL für aktualisieren,
PollToAlterData | NEWORDERS löschen, in dem der aktuellen &lt;CURSOR&gt;

In diesem Beispiel wird Logik app Umfrage, lesen, aktualisieren, und klicken Sie dann Daten in der Tabelle DB2 erneut zu lesen.

1. Wählen Sie in der Azure Startboard **+** (Pluszeichen) **Web + Mobile**, und klicken Sie dann **Logik app**.
2. Geben Sie den Namen (z. B. "ShipOrdersDb2"), App Dienst planen, andere Eigenschaften, und wählen Sie dann auf **Erstellen**.
3. Wählen Sie in der Azure Startboard der Logik app aus, die Sie soeben erstellt haben, **Einstellungen**, und klicken Sie dann **Trigger und Aktionen**.
4. Wählen Sie das Blade Trigger und Aktionen innerhalb der app Logik Vorlagen **ganz neu erstellen** .
5. Im Bereich Apps-API wählen Sie **DB2 Verbinder aus**, legen Sie einen Häufigkeit und Intervall, und klicken Sie dann **Häkchen**.
6. Klicken Sie im Bereich Apps-API wählen Sie **DB2 Verbinder aus**, erweitern Sie die Liste der Vorgänge zum auswählen **von NEWORDERS wählen**.
7. Wählen Sie zum Speichern der Einstellungen von Objektaktion, und **Speichern Sie**die **Häkchen** aus. Die Einstellungen sollte wie folgt aussehen:  
![][10]  
8. Klicken Sie auf, um das Blade **Trigger und Aktionen** zu schließen, und klicken Sie dann auf, um das Blade **Einstellungen** zu schließen.
9. Klicken Sie in der Liste **Alle ausgeführt wird** unter **Vorgänge**auf das Element zuerst aufgeführten (die letzte Ausführung).
10. Klicken Sie in das Blade **Logik app ausführen** der **Aktion** Element auf.
11. Klicken Sie auf den **LINK Ausgaben**, in das Blade **Logik app Aktion** . Die Ausgaben sollte wie folgt aussehen:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logik app mit DB2 Verbinder Aktion Daten entfernen ##
Sie können eine Aktion Logik app, um Daten aus einer DB2-Tabelle mit einer API löschen oder Beitrag zu Entität OData-Vorgang entfernen definieren. Beispielsweise können Sie eine neue Reihenfolge Kundendatensatzes durch Einfügen Verarbeitung einer Einfügen von SQL-Anweisung für eine Tabelle mit einer Identitätsspalte definiert Zurückgeben der Identitätswert oder die Zeilen wirken sich auf die app Logik (ORDID aus endgültige Tabelle auswählen (in NWIND einfügen. NEWORDERS (CUSTID SHIPNAME, AUFGENOMMENE PERSON, LIEFERORT, SHIPREG, SHIPZIP) WERTE (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Entfernen von Daten mit DB2 Verbinder Logik-app erstellen ##
Sie können eine neue Logik app aus dem Azure Marketplace erstellen und verwenden Sie dann den Verbinder DB2 als eine Aktion Kundenaufträge entfernen. Beispielsweise können den DB2 Verbinder bedingten Löschvorgang zur Verarbeitung einer Löschen der SQL-Anweisung (Löschen von NEWORDERS, ORDID > = 10000).

1. Klicken Sie im Menü der Karte Azure **Starten** Hub auf **+** (Pluszeichen), klicken Sie auf **Web + Mobile**, und klicken Sie dann auf **app Logik**. 
2. Geben Sie einen **Namen**, beispielsweise **RemoveOrdersDb2**das Blade **app Logik erstellen** .
3. Wählen Sie aus, oder definieren Sie Werte für die anderen Einstellungen (z. B. Serviceplan, Ressourcengruppe).
4. Die Einstellungen sollte wie folgt aussehen. Klicken Sie auf **Erstellen**:  
![][12]  
5. Klicken Sie in das Blade **Einstellungen** auf **Trigger und Aktionen**.
6. Klicken Sie in vorher **Trigger und Aktionen** , in der Liste **Logik app-Vorlagen** auf **ganz neu erstellen**.
7. Klicken Sie in vorher **Trigger und Aktionen** , in der Systemsteuerung **API Apps** innerhalb der Ressourcengruppe auf **Serientyp**.
8. Klicken Sie auf der Entwurfsoberfläche der app Logik auf das Element der **Serie** , legen Sie einen **Häufigkeit** und das **Intervall**, beispielsweise **Tage** und **1**, und klicken Sie dann auf das **Häkchen** , um die Einstellungen der Serie Element zu speichern.
9. Klicken Sie in vorher **Trigger und Aktionen** , in der Systemsteuerung **API Apps** innerhalb der Ressourcengruppe auf **DB2-Verbinder**.
10. Klicken Sie auf der Entwurfsoberfläche der app Logik auf den **Verbinder DB2** Aktionselement, klicken Sie auf die Auslassungspunkte****(...), um die Liste der Vorgänge zu erweitern, und klicken Sie dann auf **bedingte Löschen von N**.
11. Geben Sie auf der DB2 Verbinder Aktionselement **ORDID Seite 10000** für einen **Ausdruck, der eine Teilmenge der Einträge identifiziert,**ein.
12. Klicken Sie auf das **Häkchen** , um die aktionseinstellungen zu speichern, und klicken Sie dann auf **Speichern**. Die Einstellungen sollte wie folgt aussehen:  
![][13]  
13. Klicken Sie auf, um das Blade **Trigger und Aktionen** zu schließen, und klicken Sie dann auf, um das Blade **Einstellungen** zu schließen.
14. Klicken Sie in der Liste **Alle ausgeführt wird** unter **Vorgänge**auf das Element zuerst aufgeführten (die letzte Ausführung).
15. Klicken Sie in das Blade **Logik app ausführen** der **Aktion** Element auf.
16. Klicken Sie auf den **LINK Ausgaben**, in das Blade **Logik app Aktion** . Die Ausgaben sollte wie folgt aussehen:  
![][14]

**Hinweis:** App-Designer Logik kürzt Tabellennamen an. Beispielsweise wird der Vorgang **bedingte löschen NEWORDERS** auf **bedingte Löschen von N**gekürzt.


> [AZURE.TIP] Verwenden Sie die folgenden SQL-Anweisungen, um die Beispieltabelle und gespeicherten Prozeduren erstellen. 

Sie können die NEWORDERS Beispieltabelle mithilfe der folgenden Aussagen DB2 SQL DDL erstellen:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Sie können das Beispiel SPOERID gespeicherte Prozedur mithilfe der folgenden DB2 DDL-Anweisung erstellen:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hybrid-Konfiguration (Optional)

> [AZURE.NOTE] Dieser Schritt ist erforderlich, nur bei Verwendung von DB2 Verbinder lokalen hinter der Firewall.

App-Dienst verwendet den Hybrid-Konfigurations-Manager eine sichere Verbindung zu Ihrem lokalen System herstellen. Wenn Connector einer lokalen IBM DB2-Server-für Windows verwendet wird, ist der Hybrid-Verbindungs-Manager erforderlich.

[Mithilfe der Hybrid-Verbindungs-Managers](app-service-logic-hybrid-connection-manager.md)finden Sie unter.


## <a name="do-more-with-your-connector"></a>Führen Sie den Verbinder
Jetzt, da der Verbinder erstellt wurde, können Sie es mit einem Business Workflow mit einer Logik app hinzufügen. Finden Sie unter [Was Logik apps werden?](app-service-logic-what-are-logic-apps.md).

Erstellen der API Apps REST-APIs verwenden. Finden Sie unter [Verbindern und API Apps verweisen](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung von Statistiken und Steuerelement Sicherheit des Verbinders überprüfen. Finden Sie unter [Verwalten und Überwachen Ihrer integrierten API Apps und Verbinder](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

