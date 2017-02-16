<properties
   pageTitle="Überwachung in SQL Azure Datawarehouse | Microsoft Azure"
   description="Erste Schritte mit Azure SQL-Data Warehouse Überwachung"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Überwachung in SQL Azure Datawarehouse

> [AZURE.SELECTOR]
- [Überwachung](sql-data-warehouse-auditing-overview.md)
- [Erkennung](sql-data-warehouse-security-threat-detection.md)

SQL Data Warehouse Überwachung können Sie zum Datensatz, die Ereignisse in Ihrer Datenbank zu einer Audit in Ihr Konto Azure-Speicher protokollieren. Überwachung helfen Ihnen bei der Einhaltung von Richtlinien verwalten, Datenbankaktivität verstehen und Einblick abweichungen und Bildschirmdarstellung auftreten, die zeigt den Business Bedenken oder vermutet Sicherheitsverstöße. SQL Data Warehouse Überwachung integriert auch mit Microsoft Power BI für Drilldown-Berichte und Analysen.

Überwachungstools aktivieren und erleichtern die Einhaltung von Vorschriften Standards aber nicht Vorschriften zu gewährleisten. Weitere Informationen zu Azure Programme, die Einhaltung von Standards unterstützen, finden Sie im <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Sicherheitscenter Azure</a>.

+ [Überwachung datenbankgrundlagen]
+ [Richten Sie die Überwachung für die Datenbank]
+ [Analysieren von Überwachungsprotokollen und Berichten]

##<a name="a-idsubheading-1aazure-sql-data-warehouse-database-auditing-basics"></a><a id="subheading-1"></a>Azure SQL Data Warehouse Datenbank Überwachung-Grundlagen


SQL Data Warehouse Datenbank Überwachung können Sie die folgenden Schritte ausführen:

- **Beibehalten** der ausgewählten Ereignisse ein Protokoll. Sie können die Kategorien der, die zu überwachenden Datenbankaktionen definieren.
- **Bericht** zur Datenbankaktivität. Vorkonfigurierten Berichten und eines Dashboards können Sie schnell mit den Aktivitäten und Ereignis reporting beginnen.
- Berichte zu **Analysieren** . Sie können die verdächtige Ereignisse, ungewöhnliche Aktivitäten und Trends suchen.

Sie können die Überwachung für die folgenden Ereigniskategorien konfigurieren:

**Einfache SQL** und **Parametrisierte SQL** für die der zusammengestellten Überwachungsprotokolle werden als klassifiziert.  

- **Zugriff auf Daten**
- **Schemänderungen (DDL)**
- **Daten Änderungen (DML)**
- **Konten, Rollen und Berechtigungen (DCL)**
- **Gespeicherte Prozedur**, **Login** und **Verwaltung Transaktionen**.

Für jede Kategorie die Überwachung von **Erfolg** und **fehlgeschlagener** Vorgänge werden separat konfiguriert.

Weitere Informationen zu den Aktivitäten und Ereignisse überwacht finden Sie unter <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Bezug (Doc-Datei herunterladen)</a>.

Überwachungsprotokolle werden in Ihrem Konto Azure-Speicher gespeichert. Sie können eine Audit Log Aufbewahrungszeitraum definieren.

Überwachungsrichtlinien kann für eine bestimmte Datenbank oder als Standardrichtlinie Server definiert werden. Eine ü Server Standardrichtlinie wendet auf alle Datenbanken auf einem Server denen keine bestimmte überschreiben Datenbank ü Richtlinie definiert ist.

Vor dem festlegen wird oben ü Kontrollkästchen überwachenden Wenn Sie ein ["für kompatiblen Client"](sql-data-warehouse-auditing-downlevel-clients.md)verwenden.


##<a name="a-idsubheading-2aset-up-auditing-for-your-database"></a><a id="subheading-2"></a>Richten Sie die Überwachung für die Datenbank

1. Starten Sie das <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>an.

2. Navigieren Sie zur Konfiguration Falz SQL Data Warehouse-Datenbank / SQL Server überwacht werden sollen. Klicken Sie auf die Schaltfläche **Einstellungen** im Vordergrund, und klicken Sie dann in der Einstellung Blade, und wählen Sie die **Überwachung**.

    ![][1]

3. Deaktivieren Sie das Kontrollkästchen für die **Überwachung Einstellungen erben vom Server** zuerst, um in der ü Blade-Konfiguration. So können Sie die Einstellungen für eine bestimmte Datenbank angeben.

    ![][2]

4. Aktivieren Sie als Nächstes Überwachung, indem Sie **auf** die Schaltfläche klicken.

    ![][3]

5. Wählen Sie in der ü Konfiguration Blade- **SPEICHERDETAILS** , um das Audit Protokolle Speicher Blade öffnen aus. Wählen Sie aus dem Azure-Speicher-Konto, in dem die Protokolle gespeichert werden, und die Aufbewahrungszeitraum. **Tipp:** Verwenden Sie das gleichen Speicherkonto für alle geprüften Datenbanken, um optimal nutzen der vorkonfigurierten Berichtvorlagen.

    ![][4]

6. Klicken Sie auf die Schaltfläche **OK** , um die Konfiguration der Speicher Details speichern.


7. Klicken Sie auf **Erfolg** und **Fehler** , um alle Ereignisse melden Sie sich unter **Protokollierung von Ereignis**oder wählen Sie einzelne Ereigniskategorien aus.


8. Wenn Sie für eine Datenbank Überwachung konfigurieren, müssen Sie möglicherweise so ändern Sie die Verbindungszeichenfolge von Ihren Kunden, um sicherzustellen, dass die Überwachung Daten ordnungsgemäß erfasst werden. Aktivieren Sie unter das Thema [Ändern Server FDQN in der Verbindungszeichenfolge](sql-data-warehouse-auditing-downlevel-clients.md) für kompatiblen Clientverbindungen.

9. Klicken Sie auf **OK**.


##<a name="a-idsubheading-3analyze-audit-logs-and-reportsa"></a><a id="subheading-3">Analysieren von Überwachungsprotokollen und Berichten</a>

Überwachungsprotokolle werden in einer Sammlung von Store Tabellen mit dem Präfix **SQLDBAuditLogs** in das Konto Azure-Speicher aggregiert, die Sie während der Installation ausgewählt haben. Sie können mithilfe eines Tools, wie z. B. <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure-Speicher-Explorer</a>-Protokolldateien anzeigen.

Eine Berichtsvorlage vorkonfigurierten Dashboard steht Ihnen ein Protokolldaten schnell zu analysieren <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">herunterladbaren Excel-Kalkulationstabelle</a> zur Verfügung. Wenn die Vorlage für Ihre Überwachungsprotokolle verwenden möchten, benötigen Sie Excel 2013 oder höher und Power Query, die Sie herunterladen können <a href="http://www.microsoft.com/download/details.aspx?id=39379">hier</a>.

Ist das fiktive Beispieldaten in der Vorlage, und Sie können Power Query einrichten, um Ihre Überwachungsprotokoll direkt aus Ihrem Konto Azure-Speicher zu importieren.

Lesen Sie weitere detaillierte Informationen zum Arbeiten mit der Berichtsvorlage die <a href="http://go.microsoft.com/fwlink/?LinkId=506731">How To (Doc Download)</a>.

![][5]


##<a name="a-idsubheading-4practices-for-usage-in-productiona"></a><a id="subheading-4">Methoden für die Verwendung in der Herstellung</a>
Die Beschreibung in diesem Abschnitt bezieht sich auf Bildschirmausschnitte oben. Möglicherweise entweder <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder <a href= "https://manage.windowsazure.com/" target="_bank">Klassische klassischen Azure-Portal</a> verwendet werden.


##<a name="a-idsubheading-5astorage-key-regeneration"></a><a id="subheading-5"></a>Das Generieren Speicher

Herstellung werden Sie wahrscheinlich Ihre Speicherschlüssel regelmäßig zu aktualisieren. Beim Aktualisieren der Schlüssel müssen Sie die Richtlinie erneut zu speichern. Der Prozess sieht wie folgt aus:.


1. Wechseln Sie in das ü Konfiguration Blade (oben in der einrichten Überwachung Abschnitt beschriebenen) die **Zugriffstaste Speicher** vom *primären* *sekundäre* und **Speichern**aus.
![][4]
2. Wechseln Sie im Speicher Konfiguration Blade und **generiert,** die *Access-Primärschlüssel*.

3. Wechseln Sie zurück zur das ü Konfiguration Blade Wechseln der **Zugriffstaste Speicher** vom *sekundären* zum *primären* und drücken Sie **Speichern**.

4. Kehren Sie zum die Speicherung Benutzeroberfläche und **neu generieren** des *Sekundären Zugriffstaste* (als Vorbereitung für den nächsten Zyklus der Schlüssel aktualisieren.

##<a name="a-idsubheading-6aautomation"></a><a id="subheading-6"></a>Automatisierung
Es gibt mehrere PowerShell-Cmdlets, die Sie zum Konfigurieren der Überwachung in Azure SQL-Datenbank verwenden können. Zugriff auf die Cmdlets ü muss PowerShell im Ressourcenmanager Azure-Modus ausgeführt werden.

> [AZURE.NOTE] Das Modul [Azure Ressourcenmanager](https://msdn.microsoft.com/library/dn654592.aspx) gibt es zurzeit in der Vorschau. Es möglicherweise nicht die gleiche Verwaltungsfunktionen wie das Modul Azure bereit.

Ausgeführt werden im Modus Azure Ressourcenmanager `Get-Command *AzureSql*` in der Liste der verfügbaren Cmdlets.


<!--Anchors-->
[Überwachung datenbankgrundlagen]: #subheading-1
[Richten Sie die Überwachung für die Datenbank]: #subheading-2
[Analysieren von Überwachungsprotokollen und Berichten]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
