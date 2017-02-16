<properties
    pageTitle="Erste Schritte, indem Sie die Datenbank aktivieren für gedehnt-Assistenten ausführen | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren einer Datenbank für gedehnt Datenbank durch die Datenbank aktivieren für gedehnt-Assistenten ausführen."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Erste Schritte, indem Sie die Datenbank aktivieren für gedehnt-Assistenten ausführen

Führen Sie zum Konfigurieren einer Datenbank für gedehnt Datenbank der Datenbank aktivieren für gedehnt-Assistenten.  In diesem Thema werden die Informationen, die Sie zum eingeben und Optionen aus, die Sie im Assistenten treffen müssen.

Weitere Informationen zum gedehnt Datenbank finden Sie unter [Datenbank gedehnt](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Später, wenn Sie gedehnt Datenbank deaktivieren, beachten Sie, dass gedehnt Datenbank deaktivieren, einer Tabelle oder für eine Datenbank das remote-Objekt nicht gelöscht. Wenn Sie die entfernte Tabelle oder die Remotedatenbank löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Der remote-Objekte weiterhin Azure Kosten entstehen, bis Sie manuell löschen. 

## <a name="launch-the-wizard"></a>Starten Sie den Assistenten

1.  Wählen Sie die Datenbank, die auf der Sie gedehnt aktivieren möchten, in SQL Server Management Studio im Objekt-Explorer.

2.  Rechts\-klicken Sie auf Wählen Sie **Aufgaben**, und wählen Sie dann auf **gedehnt**, und wählen Sie dann auf **Aktivieren** , um den Assistenten zu starten.

## <a name="a-nameintroaintroduction"></a><a name="Intro"></a>Einführung
Überprüfen Sie den Zweck des Assistenten und die erforderlichen Komponenten.

Die wichtige Vorkenntnisse gehören die folgenden:

-   Sie müssen ein Administrator zum Ändern der Einstellungen der Datenbank sein.
-   Sie müssen über ein Microsoft Azure-Abonnement verfügen.
-   SQL Server muss mit dem Remoteserver Azure kommunizieren können.

![Einführungsseite des Assistenten gedehnt][StretchWizardImage1]

## <a name="a-nametablesaselect-tables"></a><a name="Tables"></a>Wählen Sie Tabellen aus.
Wählen Sie die Tabellen, die Sie für gedehnt aktivieren möchten.

Tabellen mit vielen Zeilen werden am oberen Rand der sortierte Liste. Bevor der Assistent die Liste der Tabellen anzeigt, analysiert es, für die Datentypen, die von gedehnt Datenbank derzeit nicht unterstützt werden.

![Wählen Sie die Seite des Assistenten gedehnt Tabellen][StretchWizardImage2]

|Spalte|Beschreibung|
|----------|---------------|
|(kein Titel)|Aktivieren Sie das Kontrollkästchen in dieser Spalte die ausgewählte Tabelle für gedehnt aktivieren.|
|**Namen**|Gibt den Namen der Spalte in der Tabelle an.|
|(kein Titel)|Ein Symbol in dieser Spalte möglicherweise eine Warnung darstellen dieser\'t verhindern, dass Sie die ausgewählte Tabelle für gedehnt aktiviert. Es möglicherweise auch eine Kompatibilitätsproblems, das Aktivieren der ausgewählten Tabelle für gedehnt verhindert darstellen \- angenommen, da die Tabelle einen nicht unterstützten Datentyp verwendet. Zeigen Sie auf das Symbol, um weitere Informationen in einer QuickInfo anzuzeigen. Weitere Informationen finden Sie unter [Einschränkungen für gedehnt Datenbank](sql-server-stretch-database-limitations.md).|
|**Es gedehnt wird**|Gibt an, ob die Tabelle für gedehnt bereits aktiviert ist.|
|**Migrieren von**|Sie können eine gesamte Tabelle (**Gesamte Tabelle**) migrieren, oder geben Sie einen Filter auf eine vorhandene Spalte in der Tabelle. Wenn Sie eine anderen Filter-Funktion verwenden Sie zum Auswählen von Zeilen migrieren möchten, führen Sie die ALTER TABLE-Anweisung, um den Filter-Funktion anzugeben, nachdem Sie den Assistenten zu beenden. Weitere Informationen zu den Filter-Funktion finden Sie unter [Wählen Sie Zeilen mithilfe einer Filterfunktion migrieren](sql-server-stretch-database-predicate-function.md). Weitere Informationen dazu, wie Sie die Funktion anwenden finden Sie unter [Aktivieren von gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md) oder [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Zeilen**|Gibt die Anzahl der Zeilen in der Tabelle an.|
|**Größe (KB)**|Gibt die Größe der Tabelle in KB an.|

## <a name="a-namefilteraoptionally-provide-a-row-filter"></a><a name="Filter"></a>Geben Sie optional einen Zeilenfilter

Wenn Sie eine Filter-Funktion zum Auswählen von Zeilen migrieren angeben möchten, führen Sie die folgenden Elemente auf der Seite **Tabellen auswählen** .

1.  Klicken Sie auf **Gesamte Tabelle** in der Zeile für die Tabelle, in der Liste **Wählen Sie die Tabellen, die Sie vergrößern möchten** . Im Dialogfeld **Wählen Sie Zeilen gestreckt** wird geöffnet.

    ![Definieren einer Filter-Funktion][StretchWizardImage2a]

2.  Wählen Sie im Dialogfeld **Wählen Sie Zeilen gestreckt wird,** **Wählen Sie Zeilen**aus.

3.  Geben Sie im **Feld Name**einen Namen für den Filter-Funktion.

4.  Der **Where** -Klausel wählen Sie eine Spalte aus der Tabelle aus, wählen Sie einen Operator aus, und geben Sie einen Wert.

5. Klicken Sie auf zum Testen der Funktion **Überprüfen** . Die Funktion aus der Tabelle - Ergebnisse zurück, wenn Zeilen zu migrieren, die die Bedingung - erfüllen meldet d. h., die Teststatistik eines **Erfolg**.

    >   [AZURE.NOTE] Das Textfeld, das die Filterabfrage angezeigt wird, ist schreibgeschützt. Sie können die Abfrage in das Textfeld nicht bearbeiten.

6.  Klicken Sie auf Fertig, um zur Seite **Select Tabellen** zurückzukehren.

Filter-Funktion wird in SQL Server erstellt nur, wenn Sie den Assistenten zu beenden. Bis zu diesem Zeitpunkt können Sie zu der Seite **Wählen Sie Tabellen** ändern oder benennen Sie die Filterfunktion zurückkehren.

![Wählen Sie die Seite Tabellen nach dem Definieren einer Filter-Funktion][StretchWizardImage2b]

Wenn Sie einen anderen Typ von Filter-Funktion verwenden Sie zum Auswählen von Zeilen migrieren möchten, führen Sie eine der folgenden Aktionen aus.  

-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung gedehnt für die Tabelle zu aktivieren und eine Filterfunktion anzugeben. Weitere Informationen finden Sie unter [Aktivieren von gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md).  

-   Führen Sie die ALTER TABLE-Anweisung, um eine Filterfunktion anzugeben, nachdem Sie den Assistenten zu beenden. Die erforderlichen Schritte finden Sie unter [Hinzufügen einer Filterfunktion nach dem Ausführen des Assistenten](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="a-nameconfigureaconfigure-azure-deployment"></a><a name="Configure"></a>Konfigurieren von Azure-Bereitstellung

1.  Melden Sie sich beim Microsoft Azure mit einem Microsoft-Konto an.

    ![Melden Sie sich bei Azure - Datenbank gedehnt-Assistenten][StretchWizardImage3]

2.  Wählen Sie das vorhandene Azure-Abonnement für gedehnt Datenbank verwendet werden soll.

3.  Wählen Sie eine Azure Region aus.
    -   Wenn Sie einen neuen Server erstellen, wird der Server in diesem Bereich erstellt.  
    -   Wenn Sie vorhandene Server in der ausgewählten Region haben, listet der Assistent, wenn Sie **vorhandene Server**auswählen.

    Wählen Sie um Wartezeit minimieren, aus der Azure Region, in der SQL Server gespeichert ist. Weitere Informationen zu Regionen finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/).

4.  Geben Sie an, ob Sie einen vorhandenen Server verwenden oder einen neuen Azure Server erstellen möchten.

    Wenn Active Directory auf dem SQL Server mit Azure Active Directory verbunden ist, können Sie optional eine partnerverbundkontakte Dienstkontos für SQL Server zur Kommunikation mit dem Remoteserver Azure verwenden. Weitere Informationen zu den Anforderungen für diese Option finden Sie unter [ALTER festlegen Datenbankoptionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Erstellen von neuen server**

        1.  Erstellen Sie einen Benutzernamen und ein Kennwort für den Serveradministrator.

        2.  Verwenden Sie optional eine partnerverbundkontakte Dienstkontos für SQL Server zur Kommunikation mit dem Remoteserver Azure ein.

        ![Erstellen Sie neuen Azure Server - gedehnt Datenbank-Assistent][StretchWizardImage4]

    -   **Vorhandene server**

        1.  Wählen Sie den vorhandenen Azure-Server.

        2.  Wählen Sie die Authentifizierungsmethode aus.

            -   Wenn Sie die **SQL Server-Authentifizierung**auswählen, geben Sie Administratorbenutzernamen und-Kennwort an.

            -   Wählen einer partnerverbundkontakte Dienstkontos für SQL Server verwendet zur Kommunikation mit dem Remoteserver Azure- **Active Directory-integrierte Authentifizierung** . Wenn der ausgewählte Server nicht mit Azure Active Directory integriert ist, wird diese Option nicht angezeigt wird.

        ![Wählen Sie aus vorhandenen Azure Server - gedehnt Datenbank-Assistent][StretchWizardImage5]

## <a name="a-namecredentialsasecure-credentials"></a><a name="Credentials"></a>Schützen von Anmeldeinformationen
Sie müssen verfügen über eine Datenbank Master-Taste zum Schützen der Anmeldeinformationen, die Datenbank gedehnt wird verwendet, um die Remotedatenbank Verbindung.  

Wenn Sie ein Datenbank Master-Schlüssel bereits vorhanden ist, geben Sie das Kennwort für sie ein.  

![Schützen von Anmeldeinformationen Seite des Assistenten gedehnt Datenbank][StretchWizardImage6b]

Wenn die Datenbank nicht mit einer vorhandenen Master-Schlüssels verfügt, geben Sie ein sicheres Kennwort zum Erstellen einer Datenbank Master-Taste.  

![Schützen von Anmeldeinformationen Seite des Assistenten gedehnt Datenbank][StretchWizardImage6]

Weitere Informationen über die Datenbank Master-Taste finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) und [Schlüssel Master Datenbank erstellen](https://msdn.microsoft.com/library/aa337551.aspx). Weitere Informationen zu den Anmeldeinformationen, die der Assistent erstellt wird, finden Sie unter [CREATE Datenbank ausgelegte CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="a-namenetworkaselect-ip-address"></a><a name="Network"></a>Wählen Sie die IP-Adresse
Verwenden Sie die Subnetz IP-Adressbereiche (empfohlen), oder die öffentliche IP-Adresse von SQL Server, um eine Firewall-Regel auf Azure zu erstellen, können SQL Server Kommunikation mit dem Remoteserver Azure, ein.

Den Azure Server eingehende Daten, Abfragen und Management Vorgänge initiiert von SQL Server, um die Azure-Firewall passieren dürfen anweisen, die IP-Adressen, die Sie auf dieser Seite bereitstellen. Der Assistent ändern nicht in der Firewalleinstellungen auf dem SQL Server nichts.

![Wählen Sie die IP-Adressseite des Assistenten gedehnt Datenbank][StretchWizardImage7]

## <a name="a-namesummaryasummary"></a><a name="Summary"></a>Zusammenfassung
Überprüfen Sie die Werte, die Sie eingegeben haben und die Optionen, die Sie im Assistenten und die geschätzten Kosten auf Azure ausgewählt haben. Wählen Sie dann gedehnt aktivieren **Fertig stellen** .

![Seite "Zusammenfassung" des Assistenten gedehnt][StretchWizardImage8]

## <a name="a-nameresultsaresults"></a><a name="Results"></a>Ergebnisse
Überprüfen Sie die Ergebnisse an.

Um den Status der Datenmigration zu überwachen, finden Sie unter [Überwachen und Problembehandlung bei der Migration von Daten (gedehnt Datenbank)](sql-server-stretch-database-monitor.md).

![Seite des Assistenten gedehnt Ergebnisse][StretchWizardImage9]

## <a name="a-nameknownissuesatroubleshooting-the-wizard"></a><a name="KnownIssues"></a>Problembehandlung des Assistenten
**Fehler bei der gedehnt Datenbank-Assistent**
Gedehnt Datenbank noch keine Ebene des Servers aktiviert ist, und führen Sie den Assistenten ohne das System Administratorberechtigungen, um ihn zu aktivieren, schlägt der Assistent. Bitten Sie den Systemadministrator, gedehnt Datenbank auf dem lokalen Server zu aktivieren, und klicken Sie dann führen Sie den Assistenten erneut aus. Weitere Informationen finden Sie unter [vorbereitende: über die Berechtigung zum Aktivieren gedehnt Datenbank auf dem Server](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Nächste Schritte
Aktivieren Sie weitere Tabellen für gedehnt Datenbank ein. Migration von Daten zu überwachen und Verwalten von gedehnt\-Datenbanken und Tabellen aktiviert.

-   [Aktivieren gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md) Weitere Tabellen aktivieren.

-   [Überwachen und Problembehandlung bei der Migration von Daten](sql-server-stretch-database-monitor.md) um den Status der Datenmigration anzuzeigen.

-   [Anhalten Sie und fortsetzen Sie gedehnt Datenbank](sql-server-stretch-database-pause.md)

-   [Verwalten von und Problembehandlung bei gedehnt Datenbank](sql-server-stretch-database-manage.md)

-   [Zusätzliche gedehnt aktiviert Datenbanken](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Siehe auch

[Aktivieren Sie gedehnt Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren Sie gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
