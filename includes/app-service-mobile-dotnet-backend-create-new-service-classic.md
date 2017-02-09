1. Melden Sie sich bei der [Azure-Portal].

2. Klicken Sie auf **+ neue** > **Web + Mobile** > **Mobile-App**, geben Sie dann einen Namen für Ihre Mobile-App Back-End.

3. Wählen Sie für die **Ressourcengruppe**eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Zuordnung (mit demselben Namen wie Ihre app). 
 
    Sie können einer anderen App Serviceplan auswählen oder Erstellen eines neuen Kontos. Weitere Informationen zu App Services Pläne und zum Erstellen eines neuen Plans in einer anderen Preise gestuft und an der gewünschten Stelle angezeigt, finden Sie unter [Azure-App-Verwaltungsdienst Pläne detaillierter Überblick](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Für die **App-Serviceplan**ist der Plan Standard (in der [Ebene Standard](https://azure.microsoft.com/pricing/details/app-service/)) aktiviert. Sie können auch einen anderen Plan oder [einen neuen erstellen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)auswählen. Der App-Serviceplan Einstellungen bestimmen [Speicherort, Features, Kosten und Berechnen von Ressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihre app zugeordnet. 

    Nachdem Sie auf den Plan entschieden, haben klicken Sie auf **Erstellen**. Dadurch wird die Mobile-App Back-End-erstellt. 
    
6. Klicken Sie in der **Einstellungen** für die neue Mobile-App Back-End-Blade auf **Schnellstart** > Ihre app-Clientplattform > **Verbinden einer Datenbank**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Klicken Sie in das Blade **Verbindung zum Hinzufügen von Daten** auf **SQL-Datenbank** > **neue Datenbank erstellen**, geben Sie die **Namen**die Datenbank, eine Preisgestaltung Stufe, wählen Sie dann klicken Sie auf **Server**.  Sie können diese neue Datenbank wiederverwenden. Wenn Sie bereits eine Datenbank an derselben Stelle verfügen, können Sie stattdessen die **vorhandene Datenbank verwenden**auswählen. Die Verwendung einer Datenbank an einem anderen Speicherort ist nicht aufgrund von Kosten für Bandbreite und höhere Wartezeit empfohlen.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. Geben Sie in das **neue Server** -Blade einen eindeutigen Servernamen in das Feld **Name Server** , geben Sie einen Benutzernamen und Ihr Kennwort ein, aktivieren Sie **Zulassen Azure Services Server Zugriff auf**, und klicken Sie auf **OK**. Dadurch wird die neue Datenbank erstellt.

9. Klicken Sie auf **Verbindungszeichenfolge**wieder in das Blade **Verbindung zum Hinzufügen von Daten** , geben Sie die Werte für Benutzername und Kennwort für die Datenbank, und klicken Sie auf **OK**. Warten Sie einige Minuten, damit die Datenbank erfolgreich bereitgestellt werden, bevor Sie fortfahren.

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/
