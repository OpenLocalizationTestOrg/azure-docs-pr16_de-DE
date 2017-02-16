<properties 
    pageTitle="Verbinden mit einem Konto DocumentDB mit Protokoll Unterstützung für MongoDB | Microsoft Azure" 
    description="Erfahren Sie, wie die Verbindung mit einem Konto DocumentDB mit Protokoll Unterstützung für MongoDB, jetzt erhältlich für Vorschau. Mithilfe der Verbindungszeichenfolge MongoDB verbinden." 
    keywords="MongoDB Verbindungszeichenfolge"
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
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Herstellen einer Verbindung mit einem Konto DocumentDB mit MongoDB Protokoll unterstützt werden

Erfahren Sie, wie die Verbindung mit einer Firma Azure DocumentDB mit Protokoll Unterstützung für MongoDB standard MongoDB Format der Verbindungszeichenfolge URI verwenden.  

## <a name="get-the-accounts-connection-string-information"></a>Erhalten von Informationen zur Verbindungszeichenfolge des Kontos

1. Melden Sie sich in einem neuen Fenster der [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie in der **Linken** Navigationsleiste des Kontos Blades auf **Verbindungszeichenfolge**. Zum das **Konto Blade**zu navigieren, klicken Sie auf die Jumpbar klicken Sie auf **Weitere Dienste**auf **DocumentDB (NoSQL)** , und wählen Sie das Konto DocumentDB mit MongoDB Protokoll unterstützt werden.

    ![Screenshot des Blades alle Einstellungen](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. Das **Informationen zur Verbindungszeichenfolge** Blade wird geöffnet und verfügt über alle Informationen für die Verbindung mit dem Konto mit einem Treiber für MongoDB, einschließlich einer vordefinierten erstellte Verbindungszeichenfolge.

    ![Screenshot des Blades Zeichenfolge Verbindung](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Verbindung Zeichenfolge Anforderungen

Es ist wichtig, beachten Sie, dass DocumentDB standard MongoDB Format der Verbindungszeichenfolge URI, zwei spezifische Voraussetzungen unterstützt: DocumentDB Konten erfordert Authentifizierung und sichere Kommunikation über SSL.  Auf diese Weise wird das Format der Verbindungszeichenfolge aus:

    mongodb://username:password@host:port/[database]?ssl=true

Wobei die Werte dieser Zeichenfolge in der Verbindungszeichenfolge Blade abgebildet verfügbar sind.

- Benutzername (erforderlich)
    - Kontoname DocumentDB
- Kennwort (erforderlich)
    - Kontokennwort DocumentDB
- Host (erforderlich)
    - FQDN des DocumentDB-Konto
- Port (erforderlich)
    - 10250
- Datenbank (optional)
    - Die von der Verbindung verwendeten Standarddatenbank
- SSL = WAHR (erforderlich)

Angenommen Sie, als Konto in der Verbindungszeichenfolge oben angezeigt.  Eine gültige Verbindungszeichenfolge lautet:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Herstellen einer Verbindung mit dem C#-Treiber für MongoDB
Wie bereits erwähnt erfordern alle DocumentDB Konten sowohl Authentifizierung und sichere Kommunikation über SSL. Während das MongoDB URI Format der Verbindungszeichenfolge eine Ssl unterstützt = WAHR Abfrageparameter, arbeiten mit der MongoDB C#-Treiber erfordert die Verwendung des Objekts MongoClientSettings beim Erstellen eines MongoClient.  Im folgenden Codeausschnitt veranschaulicht ausgehend von der oben genannten Kontoinformationen, wie eine Verbindung mit dem Konto und Arbeiten mit der Datenbank "Aufgaben".

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Nächste Schritte


- Erfahren Sie, wie mit einem Konto DocumentDB mit Protokoll [verwenden MongoChef](documentdb-mongodb-mongochef.md) Unterstützung für MongoDB.
- Untersuchen Sie DocumentDB mit MongoDB [Beispiele](documentdb-mongodb-samples.md)Protokoll unterstützt werden.

 
