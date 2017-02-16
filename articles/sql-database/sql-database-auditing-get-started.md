<properties
    pageTitle="Erste Schritte mit der SQL-Datenbank Überwachung | Microsoft Azure"
    description="Erste Schritte mit SQL-Datenbank Überwachung"
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="CarlRabeler; ronitr; giladm"/>

# <a name="get-started-with-sql-database--auditing"></a>Erste Schritte mit SQL-Datenbank Überwachung
Azure SQL-Datenbank Überwachung verfolgt Datenbankereignisse und schreibt, damit eine Prüfung in Ihrem Speicher Azure-Konto anmelden.

Überwachung helfen Ihnen bei der Einhaltung von Richtlinien verwalten, Datenbankaktivität verstehen und Einblick abweichungen und Bildschirmdarstellung auftreten, die zeigt den Business Bedenken oder vermutet Sicherheitsverstöße.

Überwachung ermöglicht und erleichtert die Einhaltung von Vorschriften Standards aber nicht Vorschriften zu gewährleisten. Weitere Informationen zu Azure Programme, die Einhaltung von Standards unterstützen, finden Sie im [Sicherheitscenter Azure](https://azure.microsoft.com/support/trust-center/compliance/).

+ [Azure SQL-Datenbank Überwachung (Übersicht)]
+ [Richten Sie die Überwachung für die Datenbank]
+ [Analysieren von Überwachungsprotokollen und Berichten]

##<a name="a-idsubheading-1aazure-sql-database-auditing-overview"></a><a id="subheading-1"></a>Azure SQL-Datenbank Überwachung (Übersicht)

SQL-Datenbank Überwachung können Sie die folgenden Schritte ausführen:

- **Beibehalten** der ausgewählten Ereignisse ein Protokoll. Sie können die Kategorien der, die zu überwachenden Datenbankaktionen definieren.
- **Bericht** zur Datenbankaktivität. Vorkonfigurierten Berichten und eines Dashboards können Sie schnell mit den Aktivitäten und Ereignis reporting beginnen.
- Berichte zu **Analysieren** . Sie können die verdächtige Ereignisse, ungewöhnliche Aktivitäten und Trends suchen.


Es gibt zwei **Methoden Überwachung**aus:

- **Überwachung Blob** - Protokolle in Azure BLOB-Speicher geschrieben werden. Dies ist eine neuere ü-Methode, die bietet eine **höhere Leistungsfähigkeit**, unterstützt **höhere Genauigkeit auf Objektebene Überwachung**und **kostengünstiger**ist.
- Die **Tabelle Überwachung** - Protokolle in Azure Table Storage geschrieben werden.

Sie können konfigurieren Überwachung für verschiedene Arten von Ereigniskategorien, wie im Abschnitt [Richten Sie die Überwachung für die Datenbank](#subheading-2) erläutert.

<!--For each Event Category, auditing of **Success** and **Failure** operations are configured separately.-->

Überwachungsrichtlinien kann für eine bestimmte Datenbank oder als Standardrichtlinie Server definiert werden. Eine ü Server Standardrichtlinie gilt für alle vorhandenen und neu erstellten Datenbanken auf einem Server.


##<a name="a-idsubheading-2aset-up-auditing-for-your-database"></a><a id="subheading-2"></a>Richten Sie die Überwachung für die Datenbank

Im folgenden Abschnitt werden die Konfiguration des ü mithilfe der Azure-Portal an.

###<a name="a-idsubheading-2-1i-blob-auditinga"></a><a id="subheading-2-1">i Blob Überwachung</a>

1. Starten Sie das [Azure-Portal](https://portal.azure.com) unter https://portal.azure.com.

2. Navigieren Sie zu der SQL-Datenbank Falz Einstellungen / SQL Server überwacht werden sollen. Wählen Sie in den Einstellungen Blade **Überwachung und Bedrohung Erkennung**aus.

    <a id="auditing-screenshot"></a>
    ![Klicken Sie im Navigationsbereich][1]

3. In der Datenbank Konfiguration Blade Überwachung können Sie überprüfen, dass das Kontrollkästchen **Einstellungen übernehmen, vom Server** um, die diese Datenbank bestimmen des Servers Einstellungen überwacht werden sollen. Wenn diese Option aktiviert ist, sehen Sie einen Link **ü servereinstellungen anzeigen** , mit dem Sie anzeigen oder Ändern des Servers, Überwachung Einstellungen von diesem Kontext aus.

    ![Klicken Sie im Navigationsbereich][2]

4. Wenn Sie lieber Blob Überwachung auf Ebene der Datenbank (zusätzlich oder alternativ zur Überwachung auf Elementebene Server) aktivieren, **Deaktivieren Sie** die Option **Einstellungen Überwachung erben, vom Server** **auf** Überwachung aktivieren, und wählen die Überwachung **Blob** -Typ.

    > Server-Blob Überwachung aktiviert ist, wird das Datenbank, die so konfiguriert, dass Audit neben den Server Blob Audit vorhanden ist.  

    ![Klicken Sie im Navigationsbereich][3]

5. Wählen Sie **Speicherdetails** um das Audit Protokolle Speicher Blade zu öffnen. Wählen Sie das Konto Azure-Speicher, wo Protokolle werden gespeichert, und der Aufbewahrungszeitraum nach dem die Protokolle der alten werden gelöscht und dann auf **OK** klicken Sie unten, an. **Tipp:** Verwenden Sie das gleichen Speicherkonto für alle geprüften Datenbanken, um optimal nutzen ü Berichtvorlagen.

    <a id="storage-screenshot"></a>
    ![Klicken Sie im Navigationsbereich][4]

6. Wenn Sie die geprüften Ereignisse anpassen möchten, Sie können dafür über PowerShell oder REST-API - finden Sie unter der [Automatisierung (PowerShell / ruhen lassen API)](#subheading-7) Abschnitt Weitere Details.

8. Klicken Sie auf **Speichern**.


###<a name="a-idsubheading-2-2ii-table-auditinga"></a><a id="subheading-2-2">Überwachung der Tabelle II.</a>

> [AZURE.NOTE] Aktivieren Sie vor dem Einrichten von **Tabelle Überwachung**, wenn Sie ein ["für kompatiblen Client"](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)verwenden. Auch, wenn Sie nur Firewalleinstellungen haben, wenden Sie sich bitte beachten Sie, dass die [IP-Endpunkt der Datenbank ändert sich](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) bei der Aktivierung der Tabelle Überwachung.

1. Starten Sie das [Azure-Portal](https://portal.azure.com) unter https://portal.azure.com.

2. Navigieren Sie zu der SQL-Datenbank Falz Einstellungen / SQL Server überwacht werden sollen. Wählen Sie das Blade Einstellungen **Überwachung und Bedrohung Erkennung** (*[finden Sie im Abschnitt Blob Überwachung Screenshot](#auditing-screenshot)*).

3. Sie können in der Datenbank Konfiguration Blade Überwachung überprüfen, dass das Kontrollkästchen **Einstellungen übernehmen, vom Server** um, die diese Datenbank bestimmen des Servers Einstellungen überwacht werden sollen. Wenn diese Option aktiviert ist, sehen Sie einen Link **ü servereinstellungen anzeigen** , mit dem Sie anzeigen oder Ändern des Servers, Überwachung Einstellungen von diesem Kontext aus.

    ![Klicken Sie im Navigationsbereich][2]

4. Wenn Sie nicht Überwachung Einstellungen vom Server lieber, **Deaktivieren Sie** die Option **Einstellungen Überwachung erben, vom Server** erben, aktivieren Sie **auf** Überwachung, und wählen Sie Überwachung **Table** -Datentyp.

    ![Klicken Sie im Navigationsbereich][3-tbl]

5. Wählen Sie **Speicherdetails** , um das Audit Protokolle Speicher Blade öffnen aus. Wählen Sie das Azure-Speicher-Konto, in dem Protokolle werden gespeichert, und der Aufbewahrungszeitraum, nach dem die Protokolle der alten werden gelöscht. **Tipp:** Verwenden Sie das gleichen Speicherkonto für alle geprüften Datenbanken, um optimal nutzen ü Berichtvorlagen (*[finden Sie im Abschnitt Blob Überwachung Screenshot](#storage-screenshot)*).

6. Klicken Sie auf **Ereignisse erfasst:** anpassen die zu überwachenden Ereignisse an. Klicken Sie auf **Erfolg** und **Fehler** , um alle Ereignisse melden Sie sich das Blade **nach Wettkampf Protokollierung** , oder wählen Sie einzelne Ereigniskategorien aus.

    > Über PowerShell oder REST-API realisiert werden kann – finden Sie unter Anpassen des geprüften Ereignisse im [Automatisierung (PowerShell / ruhen lassen API)](#subheading-7) Abschnitt Weitere Details.

    ![Klicken Sie im Navigationsbereich][5]

7. Nachdem Sie Ihre Einstellungen ü konfiguriert haben, können Sie das neue Feature der **Erkennung** (Preview) aktivieren und Konfigurieren der e-Mails zum Empfangen von Sicherheitshinweisen. Erkennung können Sie proaktive Benachrichtigungen auf abweichenden Datenbankaktivitäten zu erhalten, die Sicherheitsrisiken hindeuten. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Erkennung](sql-database-threat-detection-get-started.md) .

8. Klicken Sie auf **Speichern**.


##<a name="a-idsubheading-3aanalyze-audit-logs-and-reports"></a><a id="subheading-3"></a>Analysieren von Überwachungsprotokollen und Berichten

Überwachungsprotokolle werden in das Konto Azure-Speicher aggregiert, die Sie während der Installation ausgewählt haben.

Sie können mithilfe eines Tools, wie z. B. [Azure-Speicher-Explorer](http://storageexplorer.com/)Überwachungsprotokolle vertraut machen.

Finden Sie unter Einzelheiten zur Analyse der **Tabelle** und der Überwachungsprotokolle **Blob** .

###<a name="a-idsubheading-3-1i-blob-auditinga"></a><a id="subheading-3-1">i Blob Überwachung</a>

BLOB Überwachung Protokolle werden als eine Zusammenstellung von BLOB-Dateien in einem Container mit dem Namen "**Sqldbauditlogs**" gespeichert.

Weitere Details zu den Blob überwachenden Protokolle Speicher Ordnerhierarchie, BLOB-Benennungskonvention, und melden Sie sich formatieren, finden Sie [Blob überwachenden Log Format Bezug (Doc-Datei herunterladen)](https://go.microsoft.com/fwlink/?linkid=829599).

Es gibt mehrere Methoden zum Blob Überwachung Protokolle anzeigen:

1. Über das Azure Portal - **finden Sie unter Methode (1) in der [Tabelle Überwachung Abschnitt](#activity-ui) unterhalb** (Excel-Download nicht unterstützt).

2. Laden Sie Protokolldateien aus Ihrem Container BLOB-Speicher von Azure über das Portal oder mithilfe eines Tools, wie z. B. [Azure-Speicher-Explorer](http://storageexplorer.com/).

    Nachdem Sie die Protokolldatei lokal heruntergeladen haben, können Sie die Datei öffnen, anzeigen und analysieren die Protokolle in SSMS doppelklicken.

    Zusätzliche Methoden zur Verfügung:
    - Sie können **mehrere Dateien gleichzeitig herunterladen** über Azure-Speicher-Explorer - Maustaste auf einen bestimmten Unterordner (z. B. einen Unterordner, die alle Protokolldateien für ein bestimmtes Datum enthält), und wählen Sie "Speichern unter" zum Speichern in einem lokalen Ordner.

        Nach dem Herunterladen mehrerer Dateien (oder einen ganzen Tag, wie oben beschrieben), können Sie diese lokal wie folgt zusammen:

        **Öffnen SSMS-Datei > Öffnen -> -> Seriendruck Extended Events -> alle Dateien zum Zusammenführen auswählen**

    - Programmgesteuert:
        - Erweiterte Ereignisse Reader **C#-Bibliothek** ([hier weitere Informationen](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/))
        - Abfragen von erweiterten Ereignisse Dateien mithilfe der **PowerShell** ([hier weitere Informationen](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/))

###<a name="a-idsubheading-3-2ii-table-auditinga"></a><a id="subheading-3-2">Überwachung der Tabelle II.</a>

Tabelle Überwachung Protokolle werden als eine Zusammenstellung von Azure-Speichertabellen mit dem Präfix **SQLDBAuditLogs** gespeichert.

Weitere Details der Tabelle Audit Log-Format finden Sie unter [Überwachen von Log Format Tabellenverweis (Doc-Datei herunterladen)](http://go.microsoft.com/fwlink/?LinkId=506733).

Es gibt mehrere Methoden zum Anzeigen der Tabelle Überwachung Protokolle:

<a id="activity-ui"></a>

1. Klicken Sie über das [Portal Azure](https://portal.azure.com) - am oberen Rand der Blade **Überwachung und Bedrohung Erkennung** auf klicken Sie auf **Weitere** , und klicken Sie dann auf **Durchsuchen**. Ein Blade **Audit-Datensätzen** wird geöffnet, in dem Sie die Protokolle angezeigt werden sollen werden.
    - Sie können auch bestimmte Termine anzeigen, indem Sie auf **Filter** im oberen Bereich des Blades Datensätze Audit
    - Sie können herunterladen und Anzeigen der Überwachungsprotokolle im Excel-Format, indem Sie auf **in Excel öffnen** im oberen Bereich des Blades Audit Datensätze

    ![Klicken Sie im Navigationsbereich][7]

2. Alternativ steht eine Berichtsvorlage vorkonfigurierten als eine [Excel-Kalkulationstabelle herunterladbare](http://go.microsoft.com/fwlink/?LinkId=403540) helfen Sie Log Daten schnell zu analysieren. Wenn die Vorlage für Ihre Überwachungsprotokolle verwenden möchten, benötigen Sie Excel 2013 oder höher und Power Query, die Sie herunterladen können [hier](http://www.microsoft.com/download/details.aspx?id=39379).

    Sie können auch direkt aus Ihrem Azure-Speicher-Konto mithilfe von Power Query Überwachungsprotokolle in der Excel-Vorlage importieren. Dann können Sie untersuchen der Audit-Einträge und Erstellen von Dashboards und Berichten über die Log-Daten.

    ![Klicken Sie im Navigationsbereich][9]




##<a name="a-idsubheading-5apractices-for-usage-in-production"></a><a id="subheading-5"></a>Methoden für die Herstellung Verwendung
<!--The description in this section refers to screen captures above.-->

###<a name="a-idsubheading-6auditing-geo-replicated-databasesa"></a><a id="subheading-6">Überwachung Geo repliziert Datenbanken</a>

Bei der Verwendung von Datenbanken Geo repliziert ist es möglich, auf die primäre Datenbank, die sekundäre Datenbank oder beide, je nach Typ Audit Überwachung einrichten.

Die **Tabelle Überwachung** – Sie können konfigurieren eine separate Richtlinie, klicken Sie auf die Datenbank oder der Ebene der Server für jede der beiden Datenbanken (primären und sekundären) wie im Abschnitt [Richten Sie für die Datenbank Überwachung](#subheading-2-2) beschrieben.

**Überwachung Blob** - Anweisungen:

1. **Primäre Datenbank** – entweder auf dem Server oder die Datenbank selbst, **Blob Überwachung** aktivieren, wie im Abschnitt [für die Datenbank Überwachung einrichten](#subheading-2-1) beschrieben.

2. **Sekundäre Datenbank** - Blob Überwachung können nur aktiviert werden ein-/ausschalten aus der primären Datenbank überwachungseinstellungen.
    - Aktivieren Sie **Blob Überwachung** der **primären Datenbank**, wie im Abschnitt [Richten Sie die Überwachung für die Datenbank](#subheading-2-1) beschrieben. (**Hinweis:** Blob Überwachung muss aktiviert sein, auf die Datenbank selbst, nicht auf dem Server).
    - Nachdem Blob Überwachung der primären Datenbank aktiviert ist, wird es auch auf die sekundäre Datenbank aktiviert werden.
    - **Wichtige:** Standardmäßig werden die **Speicher Einstellungen** für die sekundäre Datenbank identisch mit denen der primären Datenbank, **Cross-Region Datenverkehr**verursacht. Sie können dies vermeiden, indem der **Sekundäre Server** Blob Überwachung aktivieren und Konfigurieren einer **lokalen Speicher** in den sekundären Speicher servereinstellungen (dies außer Kraft setzen den Speicherort für die sekundäre Datenbank und dazu führen, dass jede Datenbank Speichern der Überwachungsprotokolle in einem lokalen Speicher).  


<br>
###<a name="a-idsubheading-6storage-key-regenerationa"></a><a id="subheading-6">Das Generieren Speicher</a>

In der Herstellung können Sie wahrscheinlich Ihre Speicherschlüssel regelmäßig zu aktualisieren. Wenn der Schlüssel zu aktualisieren, müssen Sie die Richtlinie für die Überwachung erneut zu speichern. Der Prozess sieht wie folgt aus:


1. Klicken Sie in der Blade-Speicher-Details wechseln Sie der **Zugriffstaste Speicher** vom *primären* zum *sekundären*, und klicken Sie unten auf **OK** . Klicken Sie dann auf am oberen Rand der ü Konfiguration Blade **Speichern** .

    ![Klicken Sie im Navigationsbereich][6]

2. Wechseln Sie im Speicher Konfiguration Blade und **generiert,** die *Access-Primärschlüssel*.

    ![Klicken Sie im Navigationsbereich][8]

3. Wechseln Sie zur das ü Konfiguration Blade zurückzuwechseln **Speicher Zugriffstaste** vom *sekundären* auf *Primär*, und klicken Sie unten auf **OK** . Klicken Sie dann auf am oberen Rand der ü Konfiguration Blade **Speichern** .

4. Kehren Sie die Speicher Konfiguration Blade und **generiert,** der *Sekundäre Zugriffstaste* (in Vorbereitung für den nächsten Zyklus der Schlüssel aktualisieren).

##<a name="a-idsubheading-7aautomation-powershell--rest-api"></a><a id="subheading-7"></a>Automatisierung (PowerShell / ruhen lassen API)

Sie können auch die Überwachung in Azure SQL-Datenbank mit den folgenden Automatisierungstools konfigurieren:

1. **PowerShell-cmdlets**

    - [Get-AzureRMSqlDatabaseAuditingPolicy][101]
    - [Get-AzureRMSqlServerAuditingPolicy][102]
    - [Entfernen-AzureRMSqlDatabaseAuditing][103]
    - [Entfernen-AzureRMSqlServerAuditing][104]
    - [AzureRMSqlDatabaseAuditingPolicy festlegen][105]
    - [Set-AzureRMSqlServerAuditingPolicy][106]
    - [Verwenden Sie-AzureRMSqlServerAuditingPolicy][107]

2. **REST-API - Blob Überwachung**

    * [Erstellen Sie oder aktualisieren Sie die Datenbank Blob Überwachungsrichtlinien](https://msdn.microsoft.com/en-us/library/azure/mt695939.aspx)
    * [Erstellen oder Aktualisieren von Server Blob Überwachungsrichtlinien](https://msdn.microsoft.com/en-us/library/azure/mt771861.aspx)
    * [Abrufen von Datenbank Blob Überwachungsrichtlinien](https://msdn.microsoft.com/en-us/library/azure/mt695938.aspx)
    * [Erste Server Blob Überwachungsrichtlinien](https://msdn.microsoft.com/en-us/library/azure/mt771860.aspx)
    * [Erste Server Blob Überwachung Ergebnis des Vorgangs](https://msdn.microsoft.com/en-us/library/azure/mt771862.aspx)


2. **REST-API - Tabelle Überwachung**

    * [Erstellen Sie oder aktualisieren Sie die Datenbank, die Richtlinie Überwachung](https://msdn.microsoft.com/en-us/library/azure/mt604471.aspx)
    * [Erstellen oder Aktualisieren von Überwachungsrichtlinien Server](https://msdn.microsoft.com/en-us/library/azure/mt604383.aspx)
    * [Erste Überwachungsrichtlinien Datenbank](https://msdn.microsoft.com/en-us/library/azure/mt604381.aspx)
    * [Besorgen Sie sich Server Überwachungsrichtlinie](https://msdn.microsoft.com/en-us/library/azure/mt604382.aspx)






<!--Anchors-->
[Azure SQL-Datenbank Überwachung (Übersicht)]: #subheading-1
[Richten Sie die Überwachung für die Datenbank]: #subheading-2
[Analysieren von Überwachungsprotokollen und Berichten]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7


<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[3-tbl]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on_table.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_audited_events.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_storage_key_regeneration.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_activity_log.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_regenerate_key.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_report_template.png


[101]: https://msdn.microsoft.com/en-us/library/azure/mt603731(v=azure.200).aspx
[102]: https://msdn.microsoft.com/en-us/library/azure/mt619329(v=azure.200).aspx
[103]: https://msdn.microsoft.com/en-us/library/azure/mt603796(v=azure.200).aspx
[104]: https://msdn.microsoft.com/en-us/library/azure/mt603574(v=azure.200).aspx
[105]: https://msdn.microsoft.com/en-us/library/azure/mt603531(v=azure.200).aspx
[106]: https://msdn.microsoft.com/en-us/library/azure/mt603794(v=azure.200).aspx
[107]: https://msdn.microsoft.com/en-us/library/azure/mt619353(v=azure.200).aspx
