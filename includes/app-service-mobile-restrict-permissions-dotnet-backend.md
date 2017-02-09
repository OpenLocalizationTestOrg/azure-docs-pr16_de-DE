
Standardmäßig können in einer Back-End-Mobile-App-APIs anonym aufgerufen werden. Als Nächstes müssen Sie zum Einschränken des Zugriffs auf nur authentifizierten Clients.  

+ **Node.js Back-End-(über Portal)** :  
    
    Klicken Sie in Ihrem Mobile-App- **Einstellungen**auf **Einfache Tabellen** , und wählen Sie die Tabelle aus. Klicken Sie auf **Berechtigungen ändern**, wählen Sie **nur authentifizierter Zugriff** für alle Berechtigungen aus, und klicken Sie auf **Speichern**. 

+ **.NET Back-End-(c#)**:  

    Navigieren Sie in der Project Server zu **Controller** > **TodoItemController.cs**. Hinzufügen der `[Authorize]` Attribut der Klasse **TodoItemController** wie folgt. Zum Einschränken des Zugriffs nur für bestimmte Methoden können Sie auch dieses Attribut nur auf diese Methoden anstelle der Klasse anwenden. Veröffentlichen Sie das Serverprojekt erneut.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js Back-End-(über Node.js Code)** :  
    
    Wenn Sie für den Zugriff auf die Tabelle Authentifizierung erforderlich ist, fügen Sie die folgende Zeile an das Node.js Serverskript:


        table.access = 'authenticated';

    Weitere Informationen hierzu finden Sie unter [wie: Authentifizierung für den Zugriff auf Tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). So laden Sie das Codeprojekt Schnellstart aus Ihrer Website finden Sie unter [wie: herunterladen das Node.js Back-End-Schnellstart-Codeprojekt Git mit](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

