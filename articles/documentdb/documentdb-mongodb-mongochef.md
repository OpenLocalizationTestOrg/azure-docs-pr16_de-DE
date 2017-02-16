<properties 
    pageTitle="Verwenden Sie MongoChef mit einem Konto DocumentDB mit Protokoll Unterstützung für MongoDB | Microsoft Azure" 
    description="Erfahren Sie, wie MongoChef mit einem Konto DocumentDB mit Protokoll Unterstützung für MongoDB, jetzt erhältlich für Vorschau verwenden." 
    keywords="mongochef"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="anhoh"/>

# <a name="use-mongochef-with-a-documentdb-account-with-protocol-support-for-mongodb"></a>Verwenden von MongoChef mit einem Konto DocumentDB mit MongoDB Protokoll unterstützt werden

Informationen zum Verbinden mit einer Firma Azure DocumentDB mit Protokoll Unterstützung für MongoDB MongoChef verwenden müssen Sie folgende Aktionen ausführen:

- Herunterladen und Installieren von [MongoChef](http://3t.io/mongochef)
- Haben Sie Ihr Konto DocumentDB mit Protokoll Unterstützung für MongoDB [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) Informationen

## <a name="create-the-connection-in-mongochef"></a>Erstellen Sie die Verbindung in MongoChef  

Führen Sie die folgenden Schritte aus, um Ihr Konto DocumentDB mit Protokoll Unterstützung für MongoDB MongoChef Verbindungs-Managers hinzuzufügen.

1. Abrufen Ihrer DocumentDB mit Protokoll Unterstützung für MongoDB Verbindungsinformationen anhand der Anweisungen [hier](documentdb-connect-mongodb-account.md).

    ![Screenshot des Blades Zeichenfolge Verbindung](./media/documentdb-mongodb-mongochef/ConnectionStringBlade.png)

2. Klicken Sie auf **Verbinden** , um den Verbindungs-Manager zu öffnen, und klicken Sie auf **Neue Verbindung**

    ![Screenshot der MongoChef Verbindungs-manager](./media/documentdb-mongodb-mongochef/ConnectionManager.png)
    
2. Geben Sie im Fenster **Neue Verbindung** auf der Registerkarte **Server** die HOST (FQDN) des Kontos DocumentDB mit Protocol-Unterstützung für MongoDB und den PORT ein.
    
    ![Screenshot der Registerkarte Server MongoChef Verbindung-manager](./media/documentdb-mongodb-mongochef/ConnectionManagerServerTab.png)

3. Im Fenster **Neue Verbindung** auf der Registerkarte **Authentifizierung** Authentifizierungsmodus **Standard (MONGODB-CR oder SCARM-SHA-1)** wählen Sie aus, und geben Sie den Benutzernamen und das Kennwort ein.  Übernehmen Sie die standardmäßigen Authentifizierung Db (Admin), oder geben Sie einen eigenen Wert.

    ![Screenshot der Registerkarte MongoChef Verbindung-Manager-Authentifizierung](./media/documentdb-mongodb-mongochef/ConnectionManagerAuthenticationTab.png)

4. Das Fenster **Neue Verbindung** auf der Registerkarte **SSL** aktivieren Sie das Kontrollkästchen **verwenden SSL-Protokoll Verbindung** und das Optionsfeld **akzeptieren selbstsignierten SSL-Zertifikate** .

    ![Screenshot der Registerkarte MongoChef Verbindung-Manager SSL](./media/documentdb-mongodb-mongochef/ConnectionManagerSSLTab.png)

5. Klicken Sie auf die Schaltfläche **Verbindung testen** , um die Verbindungsinformationen zu überprüfen, klicken Sie auf **OK** , um das Fenster Neue Verbindung zurückzukehren, und klicken Sie dann auf **Speichern**.

    ![Screenshot des Fensters Verbindung testen MongoChef](./media/documentdb-mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Verwenden Sie zum Erstellen einer Datenbank, die Sammlung und Dokumente MongoChef  

Führen Sie zum Erstellen einer Datenbank, die Sammlung und Dokumente mithilfe von MongoChef die folgenden Schritte aus.

1. **Verbindungs-Manager**markieren Sie die Verbindung aus, und klicken Sie auf **Verbinden**.

    ![Screenshot der MongoChef Verbindungs-manager](./media/documentdb-mongodb-mongochef/ConnectToAccount.png)

2. Klicken Sie mit der rechten Maustaste auf den Host, und wählen Sie die **Datenbank hinzufügen**.  Geben Sie einen Datenbanknamen, und klicken Sie auf **OK**.
    
    ![Screenshot der Datenbankoption MongoChef hinzufügen](./media/documentdb-mongodb-mongochef/AddDatabase1.png)

3. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Sammlung hinzufügen**aus.  Geben Sie einen Namen für die Websitesammlung ein, und klicken Sie auf **Erstellen**.

    ![Screenshot der Option MongoChef Sammlung hinzufügen](./media/documentdb-mongodb-mongochef/AddCollection.png)

4. Klicken Sie auf das Menüelement **Auflistung** , und klicken Sie auf **Dokument hinzufügen**.

    ![Screenshot der Menüelement MongoChef Dokument hinzufügen](./media/documentdb-mongodb-mongochef/AddDocument1.png)

5. Klicken Sie im Dialogfeld Dokument hinzufügen fügen Sie die folgenden, und klicken Sie dann auf **Dokument hinzufügen**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
            { "firstName": "Thomas" },
            { "firstName": "Mary Kay"}
        ],
        "children": [
        {
            "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
            "pets": [{ "givenName": "Fluffy" }]
        }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }

    
6. Fügen Sie ein anderes Dokument diesmal mit dem folgenden Inhalt hinzu.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                "givenName": "Lisa", 
                "gender": "female", 
                "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }

7. Ausführen einer Abfrage für die Stichprobe. Beispielsweise für Familien mit dem Nachnamen "Andersen" Suchen und die Eltern und Statusfelder zurückzukehren.

    ![Screenshot der Abfrageergebnisse Mongo Bäckermeister](./media/documentdb-mongodb-mongochef/QueryDocument1.png)
    

## <a name="next-steps"></a>Nächste Schritte

- Untersuchen Sie DocumentDB mit MongoDB [Beispiele](documentdb-mongodb-samples.md)Protokoll unterstützt werden.

 
