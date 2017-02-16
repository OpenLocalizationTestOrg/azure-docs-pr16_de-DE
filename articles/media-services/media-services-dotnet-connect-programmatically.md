<properties 
    pageTitle="Herstellen einer Verbindung mit Media Services-Kontos mit .NET" 
    description="In diesem Thema veranschaulicht, wie mit Media-Dienste Einzelhandelsabonnements .NET verbinden." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Herstellen einer Verbindung mit Media Services-Kontos mit Media Services SDK für .NET

> [AZURE.SELECTOR]
- [REST](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


In diesem Thema beschrieben, wie Sie eine programmgesteuerten Verbindung mit Microsoft Azure Media-Dienste zu erhalten, wenn Sie mit der Media Services SDK für .NET programmieren wird.


## <a name="connecting-to-media-services"></a>Herstellen einer Verbindung mit Media-Dienste

Media-Dienste programmgesteuert zum Verbinden mit müssen Sie haben bereits zuvor ein Azure-Konto eingerichtet, konfiguriert Media-Dienste auf diesem Konto, und klicken Sie dann eingerichtet haben Visual Studio-Projekt für die Entwicklung mit der Media Services SDK für .NET. Weitere Informationen finden Sie unter Einrichten für die Entwicklung mit der Media Services SDK für .NET.

Am Ende des Installationsvorgangs von Media-Dienste-Konto für Ihren Kunden die folgenden Werte für die Verbindung erforderlich. Verwenden Sie diese Option, um programmgesteuerten Verbindungen Media-Dienste vorzunehmen.

- Ihren Kontonamen Media-Dienste.

- Ihren kontoschlüssel Media-Dienste.

Um diese Werte zu finden, wechseln Sie zum Portal Azure-Verwaltung, wählen Sie Ihr Konto Media-Dienst, und klicken Sie auf das Symbol "**Schlüssel verwalten**" an den Fuß des Fensters Portal. Klicken auf das Symbol neben jedem Textfeld kopiert den Wert in die Zwischenablage des Systems.


## <a name="creating-a-cloudmediacontext-instance"></a>Erstellen einer CloudMediaContext-Instanz

Um Programmieren von Media-Dienste zu starten, müssen Sie eine Instanz **CloudMediaContext** erstellen, die im Serverkontext darstellt. Das **CloudMediaContext** umfasst Verweise auf wichtige einschließlich Einzelvorgänge, Anlagen, Dateien, Richtlinien und Locator Websitesammlungen.

>[AZURE.NOTE] Die **CloudMediaContext** -Klasse ist nicht Thread-Sicherheit. Erstellen Sie eine neue CloudMediaContext pro Thread oder Satz von Vorgängen.


CloudMediaContext hat fünf Konstruktorüberladungen. Es wird empfohlen, Konstruktoren verwenden, die **MediaServicesCredentials** als Parameter ausführen. Weitere Informationen finden Sie unter der **Wiederverwendung von Access Steuerelement Dienst Token** , die folgt. 

Im folgende Beispiel wird der öffentlichen CloudMediaContext(MediaServicesCredentials credentials) Konstruktor verwendet:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Wiederverwenden von Access Steuerelement Dienst Token

In diesem Abschnitt wird gezeigt, wie Access Control Service Token mithilfe von CloudMediaContext Konstruktoren, die als Parameter MediaServicesCredentials wiederverwenden.


[Azure-Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (auch bekannt als Steuerelement-Dienst oder ACS) ist ein cloudbasierten Dienst, der eine einfache Möglichkeit authentifizieren und Autorisieren von Benutzern den Zugriff auf ihre Webanwendungen bietet. Microsoft Azure Media Services steuert den Zugriff auf die Dienste durch OAuth-Protokoll, das ein ACS-Token erfordert. Media-Dienste erhält die ACS-Token aus einem Autorisierung-Server.

Bei der Entwicklung mit dem Media Services SDK können Sie auswählen, die Token nicht behandelt, da das SDK Fehlercode Projektmanager für Sie darauf. Führt jedoch zu unnötige token Besprechungsanfragen ganz im SDK die ACS-Token vollständig verwalten können. Anfordern der Token dauert und verbraucht Client- und Server-Ressourcen. Darüber hinaus Steuerung der ACS-Server die Anforderungen aus, wenn der Wert zu hoch ist. Die Grenze liegt bei 30 Anforderungen pro Sekunde, [ACS Diensteinschränkungen](https://msdn.microsoft.com/library/gg185909.aspx) Weitere Details finden Sie unter.

Beginnend mit dem Media Services SDK Version 3.0.0.0, können Sie die ACS-Token wiederverwenden. Die **CloudMediaContext** Konstruktoren, die **MediaServicesCredentials** als Parameter Aktivieren der Freigabe der ACS-Tokens zwischen mehreren Kontexten. Die MediaServicesCredentials-Klasse kapselt die Anmeldeinformationen Media-Dienste. Wenn ein ACS-Token verfügbar und dessen Ablaufzeit bekannt ist, können Sie eine neue Instanz von MediaServicesCredentials erstellen, mit dem Token und an den Konstruktor des CloudMediaContext zu übergeben. Beachten Sie, dass der Media Services SDK Token automatisch aktualisiert, wenn sie ablaufen. Es gibt zwei Methoden zum Wiederverwenden ACS-Token, wie in den folgenden Beispielen dargestellt.

- Sie können das Objekt **MediaServicesCredentials** im Speicher (z. B. in einer Variablen statische Klasse) zwischenspeichern. Klicken Sie dann übergeben Sie das zwischengespeicherte Objekt an den CloudMediaContext-Konstruktor. Das MediaServicesCredentials-Objekt enthält ein ACS-Token, die wiederverwendet werden kann, wenn er noch gültig ist. Wenn das Token nicht gültig ist, wird es vom Media Services SDK mit den Anmeldeinformationen, wobei MediaServicesCredentials Konstruktorargumente aktualisiert werden.

    Beachten Sie, dass das Objekt **MediaServicesCredentials** ein gültiges Token erhält, nachdem die RefreshToken aufgerufen wird. Das **CloudMediaContext** Ruft die **RefreshToken** -Methode im Konstruktor. Wenn Sie beabsichtigen, die token Werte in einer externen Speicher zu speichern, stellen Sie sicher, prüfen, ob der Wert TokenExpiration gültig ist, bevor die token Daten zu speichern. Wenn es nicht gültig ist, rufen Sie RefreshToken vor dem Zwischenspeichern.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Sie können auch die Zeichenfolge AccessToken und die Werte TokenExpiration Zwischenspeichern. Die Werte können später zum Erstellen eines neuen MediaServicesCredentials-Objekts mit den Cache token Daten verwendet werden.  Dies ist besonders für Szenarien, wo das Token sicher für mehrere Prozesse oder Computer freigegeben werden können.

    Die folgenden Codeausschnitte rufen Sie die SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage und UpdateTokenDataInExternalStorageIfNeeded Methoden, die nicht in diesem Beispiel definiert sind. Sie können diese Methoden zum Speichern, abrufen und Aktualisieren von token Daten in einer externen Speicher definieren. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Verwenden Sie die gespeicherte token Werte, um MediaServicesCredentials zu erstellen.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Aktualisieren Sie die token kopieren aus, für den Fall, dass das Token vom Media Services SDK aktualisiert wurde. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Wenn Sie mehrere Konten von Media-Dienste (zum Beispiel für Laden Freigabe Zwecke oder Geo-Verteilung) haben, können Sie MediaServicesCredentials Objekte, die Verwendung der System.Collections.Concurrent.ConcurrentDictionary-Auflistung (die Sammlung ConcurrentDictionary stellt eine Thread-sicher-Auflistung von Schlüssel/Wert-Paare, die von mehreren Threads gleichzeitig zugegriffen werden kann) zwischenspeichern. Die GetOrAdd-Methode können dann die zwischengespeicherten Anmeldeinformationen erhalten. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Herstellen einer Verbindung mit einem Media-Dienste-Konto befindet sich in der Region North China

Wenn Ihr Konto in der Region North China befindet, verwenden Sie den folgenden Konstruktor aus:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Beispiel:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Speichern von Verbindung Werten in Konfiguration

Es wird dringend empfohlen Verbindung Werte, beispielsweise Ihren Benutzernamen und Ihr Kennwort ein, insbesondere vertraulichen Werte in der Konfiguration gespeichert. Darüber hinaus ist es empfiehlt sich vertrauliche Konfigurationsdaten verschlüsseln. Sie können die gesamte Konfigurationsdatei mithilfe der Windows Datei System VERSCHLÜSSELNDE verschlüsseln. Um EFS in einer Datei zu aktivieren, mit der rechten Maustaste in der Datei, wählen Sie **Eigenschaften**aus, und aktivieren Sie die Verschlüsselung der Registerkarte **Erweitert** -Einstellungen. Oder Sie können eine benutzerdefinierte Lösung für die Verschlüsselung von ausgewählten Teilen einer Konfigurationsdatei mithilfe der geschützten Konfiguration erstellen. Finden Sie unter [Verschlüsseln Konfiguration Informationen mit geschützten Konfiguration](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Die folgenden App enthält die erforderliche Verbindung Werte. Die Werte in den <appSettings> Element sind die erforderlichen Werte, die Sie von den Installationsvorgang für Media-Dienste-Konto erhalten haben.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Wenn Verbindung Werte aus der Konfiguration abrufen möchten, können Sie verwenden die **ConfigurationManager** -Klasse und weisen die Werte dann Felder im Code:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
