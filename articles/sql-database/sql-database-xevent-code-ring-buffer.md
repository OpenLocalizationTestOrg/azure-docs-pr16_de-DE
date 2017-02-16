<properties 
    pageTitle="XEvent anrufen Puffer Code für SQL-Datenbank | Microsoft Azure" 
    description="Enthält ein Beispiel der Transact-SQL-Code, die durch die Verwendung des Ziels anrufen Puffer in Azure SQL-Datenbank, schnell und einfach besteht." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Anrufen Sie Puffer Zielcode für erweiterte Ereignisse in SQL-Datenbank

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Sie möchten eine Stichprobe vollständige Code am einfachsten schnelle erfassen und Berichts-Informationen für ein Ereignis erweiterte während eines Tests. Das einfachste Ziel für erweiterte Ereignisdaten ist das [Ziel Puffer anrufen](http://msdn.microsoft.com/library/ff878182.aspx).


Dieses Thema enthält ein Beispiel für Transact-SQL-Code, die:


1. Erstellt eine Tabelle mit Daten, die mit veranschaulichen.

2. Erstellt eine Sitzung für ein vorhandenes erweiterte Ereignis, nämlich **sqlserver.sql_statement_starting**.
    - Das Ereignis ist auf SQL-Anweisungen, die eine bestimmte Update Zeichenfolge enthalten: **Anweisung wie "% UPDATE TabEmployee %"**.
    - Wählt das Ergebnis des Ereignisses an eines Ziels vom Typ anrufen Puffer, nämlich **package0.ring_buffer**senden.

3. Startet die Sitzung Ereignis.

4. Gibt ein paar einfache SQL-UPDATE-Anweisung aus.

5. Probleme mit einer SQL-SELECT zum Abrufen von Ereignis Ausgabe aus dem Puffer anrufen.
    - **Sys.dm_xe_database_session_targets** und andere Ansichten dynamische Management (DMVs) verknüpft sind.

6. Beendet die Sitzung Ereignis.

7. Löscht das Ziel Puffer anrufen, um die Freigabe von Ressourcen an.

8. Löscht der Veranstaltung und die Demo-Tabelle.


## <a name="prerequisites"></a>Erforderliche Komponenten


- Ein Azure-Konto und Ihr Abonnement. Sie können für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.


- Jede Datenbank, die Sie eine Tabelle in erstellen können.
 - Optional können Sie [erstellen eine **AdventureWorksLT** Demo-Datenbank](sql-database-get-started.md) in Minuten.


- SQL Server Management Studio (ssms.exe), idealerweise seine aktuelle monatliche Updateversion. Laden Sie die neuesten ssms.exe aus:
 - Thema mit dem Titel [Herunterladen von SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)aus.
 - [Eine direkte Verknüpfung zur Downloadwebsite.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Beispiel für Code


Mit sehr geringfügigen ändern kann das folgende Beispiel anrufen Puffer auf Azure SQL-Datenbank oder Microsoft SQL Server ausgeführt werden. Die Differenz ist das Vorhandensein des Knotens 'voran _Datenbank' Name in einigen Ansichten dynamic Management (DMVs), in der FROM-Klausel in Schritt 5 verwendet werden. Beispiel:

- Sys.dm_xe**voran _Datenbank**_session_targets
- Sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Anrufen Pufferinhalt


Ssms.exe verwendet, um das Codebeispiel auszuführen.


Zum Anzeigen der Ergebnisse geklickt wir die Zelle unter der Spalte Kopfzeile **Target_data_XML**aus.

Dann geklickt im Ergebnisbereich wir die Zelle unter der Spalte Kopfzeile **Target_data_XML**. Dieser auf einer anderen Registerkarte ' Datei ' in ssms.exe, in der der Inhalt der Ergebniszelle, als XML angezeigt wurde, erstellt.


Die Ausgabe wird in den folgenden Block angezeigt. Es sieht so aus lange, aber es sieht nur zwei **<event>** Elemente.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Freigeben von Ressourcen frei, die für Ihren anrufen Puffer


Wenn Sie mit Ihren anrufen Puffer fertig sind, können Sie es entfernen und lassen Sie wieder los seine Ausgeben einer **ALTER** wie den folgenden Ressourcen:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Die Definition Ihrer Veranstaltung wird aktualisiert, aber nicht gelöscht. Sie können Ihre Veranstaltung später einer anderen Instanz von Anrufen Puffer hinzufügen:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Weitere Informationen


Artikel mit der Primärschlüssel für erweiterte Ereignisse Azure SQL-Datenbank ist:


- [Erweiterte Ereignis Aspekte in SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md), die einige Aspekte der erweiterten Ereignisse steht im Gegensatz zu, die SQL Azure-Datenbank im Vergleich zu Microsoft SQL Server unterschiedlich sein.


Andere Themen Code Stichprobe erweiterte Ereignisse stehen unter den folgenden Links. Müssen prüfen Sie regelmäßig Beispiele, um festzustellen, ob die Stichprobe Microsoft SQL Server im Vergleich zu Azure SQL-Datenbank ausgerichtet. Dann können Sie entscheiden, ob geringfügige Änderungen erforderlich sind, um das Beispiel auszuführen.


- Code Stichproben Azure SQL-Datenbank: [Ereignisdatei Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
