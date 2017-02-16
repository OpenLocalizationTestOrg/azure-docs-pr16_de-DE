<properties
   pageTitle="Erste Schritte mit SQL Data Warehouse Erkennung"
   description="Gewusst wie: Erste Schritte mit Erkennung"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Erste Schritte mit Erkennung

> [AZURE.SELECTOR]
- [Überwachung](sql-data-warehouse-auditing-overview.md)
- [Erkennung](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>(Übersicht)

Erkennung erkennt abweichenden Datenbankaktivitäten potenzielle Sicherheitsrisiko für Ihr der Datenbank angibt. Erkennung befindet sich in der Vorschau und für SQL Data Warehouse unterstützt wird.

Erkennung bietet eine neue Sicherheitsebene, womit Kunden erkennen und Antworten möglicherweise Risiken, wie sie durch die Bereitstellung von Sicherheitshinweisen auf außergewöhnlichen Aktivitäten auftreten können. Benutzer können die verdächtige Ereignisse mit [Azure SQL Data Warehouse Überwachung](sql-data-warehouse-auditing-overview.md) , um festzustellen, ob sie bei dem Versuch, zugreifen, umgehen oder Daten in das Datawarehouse ausnutzen Ergebnis auswerten.
Erkennung ganz einfach Adresse möglicherweise Risiken in das Datawarehouse, ohne dass erweiterten Sicherheit Überwachen von Systemen verwalten oder werden von Experten eines Wertpapiers zurück.

Beispielsweise erkennt Erkennung bestimmte abweichenden Datenbankaktivitäten potenzieller SQL einfügen Versuche angibt. Einfügen von SQL ist eine der Web-Anwendung Sicherheitsprobleme im Internet, basierenden Angriffen verwendet. Angreifer nutzen Anwendungsschwächen bösartige SQL-Anweisungen in Eingabefelder Anwendung, für die Verletzung oder Ändern von Daten in der Datenbank einfügen.


## <a name="set-up-threat-detection-for-your-database"></a>Einrichten von Erkennung für die Datenbank

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).

2. Navigieren Sie zur Konfiguration Falz SQL Data Warehouse zu überwachen. Wählen Sie in den Einstellungen Blade **Überwachung und Erkennung**aus.

    ![Klicken Sie im Navigationsbereich][1]

3. Aktivieren Sie in der Blade- **Überwachung und Erkennung** **auf** Überwachung, der die Bedrohung Erkennung Einstellungen angezeigt werden.

    ![Klicken Sie im Navigationsbereich][2]

4. Aktivieren Sie **auf** Erkennung an.

5. Konfigurieren Sie die Liste der e-Mail-Nachrichten, die von Sicherheitshinweisen bei Erkennen eines abweichenden Datawarehouse Aktivitäten erhalten soll.

6. Klicken Sie in die **Überwachung und Bedrohung Erkennung** Konfiguration Blade zum Speichern der neuen oder aktualisierten Überwachung und Verfahren zum Erstellen von Erkennung Richtlinie auf **Speichern** .

    ![Klicken Sie im Navigationsbereich][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen der abweichenden Datawarehouse Aktivitäten bei Erkennen einer verdächtigen Veranstaltungstag

1. Sie erhalten eine e-Mail-Benachrichtigung bei Erkennen eines abweichenden Datenbankaktivitäten. <br/>
Die e-Mail Informationen werden auf der verdächtigen Sicherheitsereignis, einschließlich der Art der außergewöhnlichen Aktivitäten, Datenbankname, Servernamen und die Uhrzeit des Ereignisses bereitgestellt. Darüber hinaus werden wird, finden Sie Informationen zu den möglichen Ursachen und empfohlene Aktionen zu ermitteln und das potenzielle Risiko für die Datenbank zu verringern.<br/>

    ![Klicken Sie im Navigationsbereich][4]

2. Klicken Sie in der e-Mail auf den Link **Überwachung Azure SQL-Protokoll** , die das klassische Azure-Portal starten und die relevanten Überwachung Datensätze, um die Uhrzeit des Ereignisses verdächtigen anzeigen.

    ![Klicken Sie im Navigationsbereich][5]

3. Klicken Sie auf die Datensätze Audit, um weitere Details anzuzeigen, klicken Sie auf der verdächtigen Datenbankaktivitäten wie etwa SQL-Anweisung Fehler Grund und Client IP.

    ![Navigationsbereich][6]

4. Klicken Sie in das Blade Überwachung Datensätze auf **in Excel öffnen** , um ein vorkonfiguriertes öffnen excel-Vorlage zum Importieren und tieferen Analyse des Überwachungsprotokolls um die Uhrzeit des Ereignisses verdächtigen ausführen.<br/>
**Hinweis:** In Excel 2010 oder höher, ist Power Query und die Einstellung für **Schnelles kombinieren** erforderlich

    ![Klicken Sie im Navigationsbereich][7]

5. Wählen Sie zum Konfigurieren der Einstellungen für **Schnelles kombinieren** – der Menübandregisterkarte **POWER QUERY** **Optionen** , um das Dialogfeld Optionen anzuzeigen. Wählen Sie im Abschnitt Datenschutz aus, und wählen Sie die zweite Option - 'Ignorieren der Sicherheitsstufen und Verbessern der Leistung potenziell' aus:

    ![Klicken Sie im Navigationsbereich][8]

6. Zum Laden SQL-Überwachungsprotokolle sicher, dass die Parameter in den Einstellungen Registerkarte ordnungsgemäß festgelegt sind, und wählen Sie dann im Menüband 'Daten', und klicken Sie auf die Schaltfläche "Alle aktualisieren".

    ![Klicken Sie im Navigationsbereich][9]

7. Die Ergebnisse werden in der **SQL-Überwachungsprotokolle** Blatt, das wodurch Sie tieferen Analyse von außergewöhnlichen Aktivitäten ausführen, die erkannt wurden, und den Einfluss des Ereignisses Sicherheit in Ihrer Anwendung minimieren.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
