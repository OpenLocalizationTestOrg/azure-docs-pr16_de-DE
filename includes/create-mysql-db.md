In diesem Handbuch wird gezeigt, wie Sie [ClearDB] zum Erstellen einer MySQL-Datenbank aus dem Azure-Speicher und zum Erstellen einer MySQL-Datenbank als eine verknüpfte Ressource, beim Erstellen einer [Website Azure] [ waws] . [ClearDB] ist ein Fehlertoleranz Datenbank-als-Service-Anbieter, der Sie ausführen, und MySQL-Datenbanken in Azure Rechenzentren verwalten, und verbinden sie aus einer anderen Anwendung ermöglicht.  

> [AZURE.NOTE] Wenn Sie als Teil der Website Erstellungsprozess eine MySQL-Datenbank erstellen, können Sie nur eine kostenlose Datenbank erstellen. Erstellen einer MySQL-Datenbank aus dem Azure-Speicher, können Sie eine kostenlose Datenbank erstellen oder eine Auswahl treffen kostenpflichtiges Optionen.

## <a name="how-to-create-a-mysql-database-from-the-azure-store"></a>So: erstellen eine MySQL-Datenbank aus dem Azure-Speicher

Zum Erstellen einer MySQL-Datenbank aus dem Azure-Speicher folgendermaßen Sie vor:

1. Melden Sie sich bei der [Azure-Verwaltungsportal][portal].
2. Klicken Sie auf **+ neue** am unteren Rand der Seite, und wählen Sie dann **MARKETPLACE**.

    ![Wählen Sie Add-on Store](./media/create-mysql-db/select-store.png)

3. Wählen Sie **ClearDB MySQL-Datenbank**, und klicken Sie auf den Pfeil am Fuß des Rahmens.

    ![Wählen Sie ClearDB MySQL-Datenbank](./media/create-mysql-db/select-cleardb-mysql.png)

4. Wählen Sie einen Plan aus, geben Sie einen Datenbanknamen, wählen Sie einen Bereich, und klicken Sie auf den Pfeil am Fuß des Rahmens.

    ![MySQL-Datenbank aus Speicher kaufen](./media/create-mysql-db/purchase-mysql.png)

5. Klicken Sie auf das Häkchen, um Ihren Kauf abzuschließen.

    ![Überprüfen und Ihren Kauf abschließen](./media/create-mysql-db/complete-mysql-purchase.png)

6. Nachdem Sie die Datenbank erstellt wurde, können Sie es auf der Registerkarte **ADD-ONS** im Verwaltungsportal verwalten.

    ![Verwalten der MySQL-Datenbank Azure-Portal](./media/create-mysql-db/manage-mysql-add-on.png)

7. Sie können die Datenbankverbindungsinformationen abrufen, indem Sie auf **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite (siehe oben).

    ![MySql-Verbindungsinformationen](./media/create-mysql-db/mysql-conn-info.png) 


## <a name="how-to-create-a-mysql-database-as-a-linked-resource-for-azure-website"></a>So: erstellen eine MySQL-Datenbank als eine verknüpfte Ressource für Azure-Website

Zum Erstellen einer MySQL-Datenbank als eine verknüpfte Ressource, beim Erstellen einer [Website Azure][waws], gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Azure-Verwaltungsportal][portal].
2. Klicken Sie auf **+ neue** am unteren Rand der Seite, und wählen Sie dann **zu berechnen**, **WEBSITE**und **Mit der Datenbank erstellen**.

    ![Erstellen der Website mit der Datenbank](./media/create-mysql-db/custom_create.png)

3. Bereitstellen Sie einer **URL** für Ihre Website, wählen Sie die **REGION** für Ihre Website, und wählen Sie aus der Dropdownliste **Datenbank** **Erstellen einer neuen MySQL-Datenbank** aus. Optional können Sie den Standardnamen für die Verbindungszeichenfolge ersetzen. Klicken Sie auf den Pfeil am unteren Rand der Seite.

    ![Bereitstellen von Website-details](./media/create-mysql-db/provide-website-details.png) 

4. Bereitstellen einer Datenbank **NAME**, wählen Sie die **REGION** für die Datenbank (Dies sollte identisch sein, die Region für Ihre Website), stimmen ClearDBs Vertragsbedingungen, und klicken Sie auf das Häkchen am Fuß des Rahmens.

    ![Bereitstellen von MySQL-details](./media/create-mysql-db/provide-mysql-details.png)

5. Nachdem Sie Ihre Website erstellt wurde, klicken Sie auf den Namen Ihrer Website zu der Website Dashboard zu wechseln.

    ![Wechseln Sie zu der Website-dashboard](./media/create-mysql-db/go-to-website-dashboard.png)

6. Klicken Sie auf **Konfigurieren**.

    ![Navigieren Sie zu der Registerkarte Konfigurieren](./media/create-mysql-db/go-to-configure-tab.png)

7. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Verbindungszeichenfolgen** , und klicken Sie auf **Verbindungszeichenfolgen anzeigen**. 

    ![Anzeigen der Verbindungszeichenfolge](./media/create-mysql-db/show-conn-string.png)

8. Kopieren Sie die Verbindungszeichenfolge für die Verwendung in Ihrer Anwendung.

    ![Angezeigte Verbindungszeichenfolge](./media/create-mysql-db/shown-conn-string.png)

> [AZURE.NOTE] Verbindungszeichenfolgen werden auf Ihrer Website Anwendung von Namen für die Verbindungszeichenfolge zugreifen. In .NET Applications sind Verbindungszeichenfolgen in das Objekt **ConnectionStrings** verfügbar. In einer anderen Programmiersprache werden kann als Umgebungsvariablen zugegriffen Verbindungszeichenfolgen. Weitere Informationen finden Sie unter [Konfigurieren von Websites][configure].

[ClearDB]: http://www.cleardb.com/
[waws]: /documentation/services/web-sites/
[portal]: http://manage.windowsazure.com
[configure]: ../articles/app-service-web/web-sites-configure.md
