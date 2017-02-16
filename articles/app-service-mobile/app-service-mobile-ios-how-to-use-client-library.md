<properties
    pageTitle="So verwenden Sie iOS SDK für Azure Mobile-Apps"
    description="So verwenden Sie iOS SDK für Azure Mobile-Apps"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>So verwenden Sie iOS-Client-Bibliothek für Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Mit diesem Leitfaden zum Erstellen Sie allgemeine Szenarien, die mit der neuesten [Azure Mobile-Apps iOS SDK]ausführen[1]. Wenn Sie neu bei Azure Mobile-Apps sind, ersten abgeschlossen [Schnellstart von Azure Mobile Apps] zum Erstellen einer Back-End, erstellen Sie eine Tabelle, und Herunterladen einer vordefinierten iOS Xcode Projekt. In diesem Handbuch liegt der Schwerpunkt auf der clientseitige iOS SDK. Weitere Informationen zu den serverseitigen SDK für die Back-End-finden Sie unter der Server SDK HOWTOs.

## <a name="reference-documentation"></a>Dokumentation zur

Der Dokumentation für den iOS-Client SDK befindet sich hier: [Azure Mobile-Apps iOS Client Bezug][2].

## <a name="supported-platforms"></a>Unterstützte Plattformen

IOS SDK unterstützt die Ziel-C Projekte, Swift 2.2 Projekte und Swift 2.3 Projekte für iOS Versionen 8.0 oder höher.

Die "Server Bewegung"-Authentifizierung verwendet eine WebView für die präsentierten Benutzeroberfläche an.  Wenn das Gerät nicht können eine UI WebView präsentieren und dann eine andere Methode der Authentifizierung erforderlich ist ist, die außerhalb des Gültigkeitsbereichs des Produkts.  
Dieses SDK eignet sich daher nicht für anzeigen oder vom Typ auf ähnliche Weise eingeschränkter Geräte.

##<a name="a-namesetupasetup-and-prerequisites"></a><a name="Setup"></a>Setup und erforderliche Komponenten

Mit diesem Leitfaden wird davon ausgegangen, dass Sie einer Back-End-mit einer Tabelle erstellt haben. Mit diesem Leitfaden wird davon ausgegangen, dass die Tabelle dasselbe Schema wie die Tabellen in diesen Lernprogrammen hat. Mit diesem Leitfaden wird vorausgesetzt, dass im Code, Sie verweisen `MicrosoftAzureMobile.framework` und Importieren von `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="a-namecreate-clientahow-to-create-client"></a><a name="create-client"></a>So: Erstellen von Kunden

Für den Zugriff auf eine Azure Mobile-Apps Back-End-im Projekt Erstellen einer `MSClient`. Ersetzen Sie `AppUrl` mit der app-URL. Sie können lassen `gatewayURLString` und `applicationKey` leer. Wenn Sie ein Gateway für die Authentifizierung eingerichtet, füllen `gatewayURLString` mit dem Gateway-URL.

**Ziel-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="a-nametable-referenceahow-to-create-table-reference"></a><a name="table-reference"></a>So: Tabellenverweis erstellen

In Access oder Aktualisieren von Daten erstellen Sie einen Bezug auf die Back-End-Tabelle. Ersetzen Sie `TodoItem` mit dem Namen der Tabelle

**Ziel-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="a-namequeryingahow-to-query-data"></a><a name="querying"></a>Wie: Abfragen von Daten

Zum Erstellen einer Datenbankabfrage Abfrage der `MSTable` Objekt. Die folgende Abfrage ruft alle Elemente `TodoItem` und den Text aller gesendeten Elemente protokolliert.

**Ziel-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="a-namefilteringahow-to-filter-returned-data"></a><a name="filtering"></a>How to: Filter zurückgegebenen Daten

Um die Ergebnisse zu filtern, gibt es viele verfügbare Optionen.

Verwenden Sie zum Verwenden eines Prädikats filtern, eine `NSPredicate` und `readWithPredicate`. Die folgenden Filter zurückgegebenen Daten zum Suchen von Elementen für nur unvollständiger erledigen.

**Ziel-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="a-namequery-objectahow-to-use-msquery"></a><a name="query-object"></a>How to: Verwenden Sie MSQuery

Führen Sie eine komplexe Abfrage (einschließlich sortieren und Seitennavigation), erstellen Sie ein `MSQuery` -Objekt, direkt oder durch Verwenden eines Prädikats:

**Ziel-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`können Sie mehrere Abfrage Verhalten zu steuern.

* Geben Sie an der Ergebnisse
* Welche Felder zurückzugebenden einschränken
* Einschränken Sie, wie viele Datensätze zurück
* Angeben der Gesamtzahl Reaktion
* Angeben von benutzerdefinierten Zeichenfolge Abfrageparameter in Anforderung
* Anwenden von zusätzlichen Funktionen

Ausführen einer `MSQuery` Abfrage mithilfe des `readWithCompletion` auf das Objekt.

## <a name="a-namesortingahow-to-sort-data-with-msquery"></a><a name="sorting"></a>So: Sortieren von Daten mit MSQuery

Um die Ergebnisse zu sortieren, sehen wir uns ein Beispiel. Rufen Sie zum Sortieren nach aufsteigender Feld 'Text' und dann 'abgeschlossen' absteigend nach `MSQuery` wie hier:

**Ziel-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="a-nameselectingaa-nameparametersahow-to-limit-fields-and-expand-query-string-parameters-with-msquery"></a><a name="selecting"></a><a name="parameters"></a>So: Einschränken der Felder und Parameter der Abfragezeichenfolge mit MSQuery erweitern

Um beschränken Felder in einer Abfrage zurückgegeben werden soll, geben Sie die Namen der Felder in der Eigenschaft **SelectFields** ein. In diesem Beispiel gibt nur den Text und fertigen Felder:

**Ziel-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Zum Einschließen von zusätzliche Zeichenfolge Abfrageparameter in der Besprechungsanfrage Server (z. B., weil sie ein benutzerdefinierter serverseitigen Skripts wird verwendet) füllen `query.parameters` wie hier:

**Ziel-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="a-namepagingahow-to-configure-page-size"></a><a name="paging"></a>How to: Konfigurieren von Seitengröße

Mit Azure Mobile-Apps steuert die Seitengröße die Anzahl der Datensätze, die jeweils aus der Back-End-Tabellen abgerufen werden. Anruf an `pull` Daten würde dann Stapel von Daten auf Grundlage dieser Seitengröße, bis es keine weiteren Datensätze sind zu extrahieren.

Es ist möglich, ein Seitenformat **MSPullSettings** verwenden, wie unten dargestellt zu konfigurieren. Seite Standardgröße 50 und im folgenden Beispiel wird es mit 3.

Sie können eine andere Seitengröße aus Gründen der Leistungsfähigkeit konfigurieren. Wenn Sie eine große Anzahl von Datensätzen kleine verfügen, reduziert eine hohe Seitengröße die Anzahl der Server Schleifen aus. 

Diese Einstellung steuert nur die Seitengröße clientseitig. Wenn eine größere Seitengröße als die Mobile-Apps Back-End unterstützt-Client anfordert, wird die Größe einer Seite auf die höchste Obergrenze, die die Back-End-Unterstützung konfiguriert ist. 

Diese Einstellung ist auch die _Anzahl_ von Datensätzen, die nicht die _Bytegröße_.

Wenn Sie die Seitengröße Client, [erhöhen Sie auch die Größe einer Seite auf dem Server](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size)erhöhen.

**Ziel-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="a-nameinsertingahow-to-insert-data"></a><a name="inserting"></a>So: Einfügen von Daten

Um eine neue Tabellenzeile einzufügen, Erstellen einer `NSDictionary` und Aufrufen `table insert`. Wenn [Dynamische Schema] aktiviert ist, generiert der App-Verwaltungsdienst Azure mobilen Back-End-automatisch neue Spalten, die auf der Grundlage der `NSDictionary`.

Wenn `id` nicht bereitgestellt, die Back-End-automatisch eine neue eindeutige ID generiert. Geben Sie einen eigenen `id` e-Mail mit Adressen, Benutzernamen, oder ein eigenen benutzerdefinierten Werte als ID an. Bereitstellen von eigene ID möglicherweise Verknüpfungen und Business-bezogene Datenbanklogik vereinfachen.

Die `result` enthält das neue Element, das eingefügt wurde. Je nach der Serverlogik muss es möglicherweise zusätzliche oder geänderte Daten im Vergleich zu was auf dem Server zurückgegeben wurde.

**Ziel-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="a-namemodifyingahow-to-modify-data"></a><a name="modifying"></a>So: Ändern von Daten

Klicken Sie zum Aktualisieren einer vorhandenen Zeile zu ändern, ein Element, und rufen `update`:

**Ziel-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Sie können auch Geben Sie an, die Zeilen-ID und das aktualisierte Feld:

**Ziel-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Mindestens die `id` Attribut muss festgelegt werden, wenn Sie Updates vornehmen.

##<a name="a-namedeletingahow-to-delete-data"></a><a name="deleting"></a>So: Löschen von Daten

Rufen Sie zum Löschen eines Elements `delete` mit dem Element:

**Ziel-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Alternativ können, indem Sie eine Zeilen-ID löschen:

**Ziel-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Mindestens die `id` Attribut muss festgelegt werden, wenn machen löscht.

##<a name="a-namecustomapiahow-to-call-custom-api"></a><a name="customapi"></a>So: benutzerdefinierte API anrufen

Mit einer benutzerdefinierten API können Sie alle Back-End-Funktionalität verfügbar machen. Es verfügt nicht über eine Tabelle Vorgang zuordnen. Nicht nur erhalten Sie mehr Kontrolle über messaging, Sie können sogar gelesen/Satz Überschriften und Textkörper das Format der Antwort ändern. So erstellen Sie eine benutzerdefinierte API auf die Back-End-Informationen hierzu [Benutzerdefinierten APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Um eine benutzerdefinierte API einzuwählen `MSClient.invokeAPI`. Die Anforderung und Antwort Inhalte werden als JSON behandelt. Verwenden Sie andere Medientypen [mit der anderen Überladung der `invokeAPI` ] [ 5].  Vornehmen einer `GET` statt Anfordern einer `POST` anfordern, festlegen Parameter `HTTPMethod` zu `"GET"` und Parameter `body` zu `nil` (da GET-Anfragen nicht Nachrichtentext verfügen.) Wenn Ihre benutzerdefinierten API anderen HTTP-Verben unterstützt, ändern `HTTPMethod` ordnungsgemäß.

**Ziel-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="a-nametemplatesahow-to-register-push-templates-to-send-cross-platform-notifications"></a><a name="templates"></a>So: Register Pushbenachrichtigungen Vorlagen Plattformen Benachrichtigungen senden

Übergeben Sie, um Vorlagen zu registrieren, Vorlagen mit Ihrer Methode **client.push RegisterDeviceToken** in Ihrer Client-app.

**Ziel-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Ihre Vorlagen werden vom Typ NSDictionary und können mehrere Vorlagen im folgenden Format enthalten:

**Ziel-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alle Kategorien werden aus der Anforderung für Sicherheit entfernt.  Um Kategorien Installationen oder Vorlagen in Installationen hinzuzufügen, finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure][4].  Zum Senden von Benachrichtigungen über diese registrierten Vorlagen arbeiten mit [Benachrichtigung Hubs APIs][3].

##<a name="a-nameerrorsahow-to-handle-errors"></a><a name="errors"></a>So: Fehler

Wenn Sie eine App-Verwaltungsdienst Azure mobilen Back-End-aufrufen, der Fertigstellung blockieren enthält eine `NSError` Parameter. Wenn ein Fehler auftritt, ist dieser Parameter nicht NULL. Im Code sollten Sie diesen Parameter überprüfen und behandeln den Fehler bei Bedarf aus, wie in die vorherige Codeausschnitte gezeigt.

Die Datei [`<WindowsAzureMobileServices/MSError.h>`] [6] definiert die Konstanten `MSErrorResponseKey`, `MSErrorRequestKey`, und `MSErrorServerItemKey`. So erhalten Sie weitere Daten, die im Zusammenhang mit dem Fehler:

**Ziel-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Darüber hinaus werden die Datei Konstanten für jede Fehlercode definiert:

**Ziel-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="a-nameadalahow-to-authenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>So: Benutzerauthentifizierung mit der Active Directory-Authentifizierung-Bibliothek

Die Active Directory Authentifizierung Bibliothek (ADAL) können sich Benutzer in Ihrer Anwendung mit Azure Active Directory. Client-Fluss-Authentifizierung mit einem Identitätsanbieter SDK empfiehlt sich, mit der `loginWithProvider:completion:` Methode.  Client-Fluss Authentifizierung bietet eine weitere systemeigenen UX Verhalten und weitere Anpassung ermöglicht.

1. Konfigurieren der mobilen app Back-End-für Anmeldung AAD anhand der [App-Verwaltungsdienst für Active Directory-Konto konfigurieren] [ 7] Lernprogramm. Vergewissern Sie sich in der Registrierung einer systemeigenen Clientanwendung die optional Schritt abgeschlossen haben. Für iOS, wir empfehlen, die die Umleitung URI die Form hat `<app-scheme>://<bundle-id>`. Weitere Informationen finden Sie unter den [ADAL iOS Schnellstart][8].

2. Installieren Sie ADAL Cocoapods verwenden. Bearbeiten Sie Ihre Podfile, wenn Sie die folgende Definition, **Ihre PROJEKTBEZOGENEN** durch den Namen Ihres Projekts Xcode ersetzt werden:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   und den Pod:

        pod 'ADALiOS'

3. Führen Sie mit dem Terminal `pod install` aus dem Verzeichnis, enthält Ihr Projekt, und öffnen Sie den generierten Xcode Arbeitsbereich (nicht das Projekt).

4. Fügen Sie den folgenden Code an Ihrer Anwendung, entsprechend der Sprache, die Sie verwenden. Stellen Sie in den einzelnen diese Ersatz aus:

    * Ersetzen Sie **Einfügen-ZERTIFIZIERUNGSSTELLE-HERE** mit dem Namen der den Mandanten, in dem Sie die Anwendung bereitgestellt. Das Format sollte https://login.windows.net/contoso.onmicrosoft.com sein. Dieser Wert kann auf der Registerkarte Domäne in Ihrem Azure Active Directory im [Azure klassischen Portal] kopiert werden.
    * Ersetzen Sie mit der Client-ID für Ihre mobile-app Back-End- **Einfügen-Ressource-ID-HERE** ein. Sie können die Client-ID von der Registerkarte **Erweitert** unter **Azure Active Directory-Einstellungen** im Portal erhalten.
    * Ersetzen Sie **Einfügen-CLIENT-ID-HERE** durch die Client-ID, die Sie in die native Client-Anwendung kopiert haben.
    * **Einfügen-REDIRECT-URI-HERE** mit Ihrer Website _/.auth/login/done_ Endpunkt, mit dem HTTPS-Schema zu ersetzen. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_sein.

**Ziel-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="a-namefacebook-sdkahow-to-authenticate-users-with-the-facebook-sdk-for-ios"></a><a name="facebook-sdk"></a>So: Benutzerauthentifizierung mit Facebook SDK für iOS

Das Facebook SDK für iOS können sich Benutzer in Ihrer Anwendung mit Facebook.  Mithilfe einer Client-Authentifizierung Fluss wird empfohlen, mit der `loginWithProvider:completion:` Methode.  Die Client-Fluss-Authentifizierung bietet eine weitere systemeigenen UX Verhalten und zur weiteren Anpassung ermöglicht.

1. Konfigurieren der mobilen app Back-End-Facebook-Anmeldung anhand der [App-Verwaltungsdienst für Facebook-Anmeldung konfigurieren] [ 9] Lernprogramm.

2. Installieren Sie das Facebook-SDK für iOS anhand der [Facebook-SDK für iOS - erste Schritte] [ 10] Dokumentation. Statt eine app zu erstellen, können Sie Ihre vorhandene Registrierung iOS-Plattform hinzufügen. 

3. Die Facebook-Dokumentation enthält entsprechendem Code, der Ziel-C in der App-Stellvertretung. Wenn Sie **Swift**verwenden, können Sie die folgenden Übersetzungen für AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Über das Hinzufügen von `FBSDKCoreKit.framework` zu Ihrem Projekt, fügen Sie auch einen Verweis auf `FBSDKLoginKit.framework` auf die gleiche Weise. 

4. Fügen Sie den folgenden Code an Ihrer Anwendung, entsprechend der Sprache, die Sie verwenden. 

**Ziel-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="a-nametwitter-fabricahow-to-authenticate-users-with-twitter-fabric-for-ios"></a><a name="twitter-fabric"></a>So: Benutzerauthentifizierung mit Twitter-Fabric für iOS

Fabric für iOS können sich Benutzer in Ihrer Anwendung mit Twitter. Client-Fluss Authentifizierung empfiehlt sich, mit der `loginWithProvider:completion:` -Methode, bietet eine weitere systemeigenen UX Verhalten und zur weiteren Anpassung ermöglicht.

1. Konfigurieren der mobilen app Back-End-Twitter-Anmeldung anhand des Lernprogramms [zum Konfigurieren der App-Verwaltungsdienst für Twitter Login](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Hinzufügen von Fabric zu einem Projekt nach der [Fabric für iOS - erste Schritte] -Dokumentation folgen und TwitterKit einrichten.

    > [AZURE.NOTE] Standardmäßig wird Fabric Twitter-Anwendung für Sie erstellt. Sie können vermeiden, erstellen eine Anwendung durch das Registrieren der Consumer Schlüssel und Consumer geheim mithilfe einer früheren Version auf die folgenden Codeausschnitte erstellt wurden.  Alternativ können Sie die Taste Consumer und Consumer geheim Werte ersetzen, die für die App-Dienst mit den Werten bereitgestellt wird, die im [Dashboard Fabric]angezeigt werden. Wenn Sie diese Option auswählen, müssen Sie den Rückruf-URL auf einen Platzhalterwert festlegen, z. B. `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Falls gewünscht Kennwörter verwenden, die Sie zuvor erstellt haben, fügen Sie den folgenden Code für Ihre App Stellvertretung hinzu:
    
    **Ziel-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Fügen Sie den folgenden Code an Ihrer Anwendung, entsprechend der Sprache, die Sie verwenden. 

**Ziel-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="a-namegoogle-sdkahow-to-authenticate-users-with-the-google-sign-in-sdk-for-ios"></a><a name="google-sdk"></a>So: Benutzerauthentifizierung mit dem Google Anmeldung SDK für iOS

Google Anmeldung SDK für iOS können sich Benutzer in Ihrer Anwendung mit einem Google-Konto.  Google angekündigt zuletzt Änderungen an ihren OAuth Sicherheitsrichtlinien.  Diese Änderungen Richtlinie werden die Verwendung von Google SDK in der Zukunft erforderlich ist.

1. Konfigurieren von Google-Anmeldung Ihre mobile-app Back-End-anhand des Lernprogramms [zum Konfigurieren der App-Verwaltungsdienst für Google Login](app-service-mobile-how-to-configure-google-authentication.md) .

2. Installieren Sie das Google SDK für iOS, anhand der Dokumentation [Google Anmeldung für iOS - Integration zu starten](https://developers.google.com/identity/sign-in/ios/start-integrating) . Sie können im Abschnitt "Authentifizieren mit einem Back-End-Server" auslassen.

3. Fügen Sie die folgenden Ihre Stellvertretung `signIn:didSignInForUser:withError:` Methode, entsprechend der Sprache verwenden.

**Ziel-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Stellen Sie sicher, dass Sie auch mit die folgenden hinzufügen `application:didFinishLaunchingWithOptions:` in Ihrer app Stellvertretung, ersetzen "SERVER_CLIENT_ID" mit der gleichen ID, die Sie zum Konfigurieren der App-Dienst in Schritt 1 verwendet.

**Ziel-C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Fügen Sie den folgenden Code an Ihrer Anwendung in einer UIViewController, die implementiert die `GIDSignInUIDelegate` Protokoll entsprechend der Sprache verwenden.  Werden Sie abgemeldet, bevor Sie angemeldet auf erneut, und auch wenn Sie nicht Ihre Anmeldeinformationen erneut eingeben müssen, die ein Zustimmungsdialogfeld angezeigt.  Rufen Sie diese Methode nur, wenn das Sitzungstoken abgelaufen ist.
 
 **Ziel-C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure mobilen Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamische Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Fabric-Dashboard]: https://www.fabric.io/home
[Fabric für iOS - erste Schritte]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
