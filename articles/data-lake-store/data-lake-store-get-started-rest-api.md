<properties 
   pageTitle="Erste Schritte mit Lake Datenspeicher mit REST-API | Microsoft Azure" 
   description="Verwenden Sie zum Ausführen von Vorgängen auf Lake Datenspeicher WebHDFS REST-APIs" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Erste Schritte mit Azure Lake Datenspeicher mithilfe von REST-APIs

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

In diesem Artikel erfahren Sie, wie WebHDFS REST-APIs und Daten dem Store REST APIs Konto Management sowie Azure Lake Datenspeicher Dateisystem Vorgänge ausführen. Azure Lake Datenspeicher stellt einen eigenen REST-APIs für Konto Management Vorgänge zur Verfügung. Da Lake Datenspeicher mit HDFS und Hadoop Netz kompatibel ist, unterstützt jedoch es WebHDFS REST APIs für Vorgänge Dateisystem verwenden.

>[AZURE.NOTE] Die REST-API ausführliche Informationen für Lake Datenspeicher unterstützen Sie, finden Sie unter [Azure Daten dem Store REST-API-Referenz](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Erstellen einer Azure-Active Directory-Anwendung**. Die Anwendung Lake Datenspeicher mit Azure AD-authentifizieren mithilfe Sie die Azure AD-Anwendung. Es gibt verschiedene Ansätze mit Azure AD Authentifizierung die **Endbenutzer** oder **Dienst - Authentifizierung**sind. Anleitungen und Weitere Informationen zum Authentifizieren finden Sie unter [Authentifizieren mit dem Datenspeicher verwenden Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [Aufrollen](http://curl.haxx.se/). In diesem Artikel verwendet cURL, um zu veranschaulichen, wie REST-API Aufrufe für ein Konto Lake Datenspeicher machen.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie authentifiziert ich mit Azure Active Directory?

Sie können zwei Ansätze authentifizieren mit Azure Active Directory verwenden.

### <a name="end-user-authentication-interactive"></a>Endbenutzer-Authentifizierung (interaktiv)

In diesem Szenario der Anwendung aufgefordert, den Benutzer sich anmelden und alle Vorgänge im Kontext des Benutzers ausgeführt werden. Führen Sie die folgenden Schritte aus, für die interaktive Authentifizierung.

1. Durchlaufen der Anwendung leiten Sie den Benutzer auf den folgenden URL ein:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > zur Verwendung in einer URL codiert werden muss. Verwenden Sie für https://localhost, also, `https%3A%2F%2Flocalhost`)

    Innerhalb dieses Lernprogramms können Platzhalterwerte in der URL oben ersetzen und fügen Sie ihn in der Adressleiste des Webbrowsers. Sie werden zum Authentifizieren mit Ihrer Azure Login weitergeleitet. Nachdem Sie sich erfolgreich angemeldet haben, wird die Antwort in der Adressleiste des Browsers angezeigt. Die Antwort werden in folgendem Format:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Erfassen Sie den Autorisierungscode aus der Antwort ein. In diesem Lernprogramm können Sie den Autorisierungscode in der Adressleiste des Webbrowsers kopieren und übergeben Sie dieses an die POST-Anforderung der token Endpunkt, wie unten dargestellt:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] In diesem Fall die \<REDIRECT-URI > nicht codiert werden müssen.

3. Die Antwort ist ein JSON-Objekt, das eine Access-Token enthält (z. B. `"access_token": "<ACCESS_TOKEN>"`) und ein Token aktualisieren (z. B. `"refresh_token": "<REFRESH_TOKEN>"`). Ihrer Anwendung verwendet das Token Access beim Zugriff auf Azure Lake Datenspeicher und dem Aktualisieren Token auf ein anderes Access Token erhalten, wenn eine Access-Sicherheitstoken abläuft.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Wenn die Access-Sicherheitstoken abläuft, können Sie ein neues Access Token mit dem Token aktualisieren anfordern, wie unten dargestellt:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Weitere Informationen über interaktive Benutzerauthentifizierung finden Sie unter [Autorisierungscode Fluss erteilen](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Dienst-Authentifizierung (nicht interaktiv)

In diesem Szenario das die Anwendung stellt einen eigenen Anmeldeinformationen ein, um die Vorgänge ausführen. Für dieses stellen Sie eine POST-Anforderung wie unten dargestellt aus. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung wird ein Autorisierungstoken enthalten (gekennzeichnet durch `access-token` in der Ausgabe unten), die Sie später mit Ihre Anrufe REST-API übergeben werden. Speichern Sie dieses Authentifizierungstoken in eine Textdatei aus. Sie benötigen diese später in diesem Artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In diesem Artikel wird verwendet, die **nicht interaktiven** Ansatz. Weitere Informationen zu nicht interaktiven (Dienst-Aufrufe) finden Sie unter [Service Dienst Anrufe mit Anmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Erstellen Sie ein Konto Lake Datenspeicher

Dieser Vorgang basiert auf die REST-API definiert Anruf [hier](https://msdn.microsoft.com/library/mt694078.aspx).

Verwenden Sie den folgenden Befehl aus cURL. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Ersetzen Sie im obigen Befehl, \< `REDACTED` \> mit dem Autorisierungstoken abgerufen einer früheren Version. Der Anforderungsnutzlast für diesen Befehl enthalten ist, in der **input.json** -Datei, die für bereitgestellt wird die `-d` oben angegebenen Parameter. Der Inhalt der Datei input.json in etwa Folgendes aus:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Erstellen von Ordnern in einem Lake Datenspeicher-Konto

Dieser Vorgang basiert auf die WebHDFS REST-API definiert Anruf [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Verwenden Sie den folgenden Befehl aus cURL. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Ersetzen Sie im obigen Befehl, \< `REDACTED` \> mit dem Autorisierungstoken abgerufen einer früheren Version. Dieser Befehl erstellt ein Verzeichnis namens **Mytempdir** im Stammordner Ihres Kontos Lake Datenspeicher.

Eine Antwort wie folgt sollte angezeigt werden, wenn der Vorgang erfolgreich abgeschlossen wurde:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Liste Ordner in einem Lake Datenspeicher-Konto

Dieser Vorgang basiert auf die WebHDFS REST-API definiert Anruf [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Verwenden Sie den folgenden Befehl aus cURL. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Ersetzen Sie im obigen Befehl, \< `REDACTED` \> mit dem Autorisierungstoken abgerufen einer früheren Version.

Eine Antwort wie folgt sollte angezeigt werden, wenn der Vorgang erfolgreich abgeschlossen wurde:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Hochladen von Daten in ein Konto Lake Datenspeicher

Dieser Vorgang basiert auf die WebHDFS REST-API definiert Anruf [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Hochladen von Daten mithilfe der WebHDFS REST-API umfasst zwei Schritte, wie unten beschrieben.

1. Anfordern eines HTTP setzen ohne Senden der Dateidaten hochgeladen werden. Ersetzen Sie den folgenden Befehl aus, ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Die Ausgabe für diesen Befehl werden enthält eine temporäre Umleitung URL wie den unten gezeigten.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Sie müssen jetzt einen anderen setzen HTTP-Anforderung gegen die URL für die Eigenschaft **Position** in der Antwort aufgeführt senden. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Die Ausgabe ist ähnlich wie der folgende aus:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Lesen von Daten aus einem Lake Datenspeicher-Konto

Dieser Vorgang basiert auf die WebHDFS REST-API definiert Anruf [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Lesen von Daten aus einem Lake Datenspeicher ist Konto einen zweistufigen Prozess ausführen.

* Sie zuerst eine GET-Anforderung gegen den Endpunkt senden `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Dadurch wird einen Speicherort zum Übermitteln Sie der nächsten GET-Anforderung, zurückgegeben.
* Klicken Sie dann senden Sie die GET-Anforderung gegen den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Damit wird der Inhalt der Datei angezeigt.

Da es keinen Unterschied zwischen den Eingabeparameter zwischen dem ersten und zweiten Schritt gibt, Sie können jedoch die `-L` Parameter und übermitteln Sie die erste Anforderung. `-L`Option kombiniert zwei Anfragen in einer im Wesentlichen und kopiere cURL, wiederholen die Anforderung auf die neue Position. Schließlich die Ausgabe Aufrufe der Anfrage wird angezeigt, wie unten gezeigt. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Umbenennen einer Datei in einem Lake Datenspeicher-Konto

Dieser Vorgang basiert auf die WebHDFS REST-API definiert Anruf [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Verwenden Sie den folgenden Befehl aus cURL zum Umbenennen einer Datei ein. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Löschen einer Datei von einem Konto Lake Datenspeicher

Dieser Vorgang basiert auf die WebHDFS REST-API definiert Anruf [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Verwenden Sie den folgenden cURL-Befehl zum Löschen einer Datei aus. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Sie sollte eine Ausgabe wie die folgende angezeigt:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Löschen eines Kontos Lake Datenspeicher

Dieser Vorgang basiert auf die REST-API definiert Anruf [hier](https://msdn.microsoft.com/library/mt694075.aspx).

Verwenden Sie den folgenden Befehl aus cURL, um ein Konto Lake Datenspeicher löschen. Ersetzen Sie ** \<Yourstorename >** mit Ihrem Namen Lake Datenspeicher.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Sie sollte eine Ausgabe wie die folgende angezeigt:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Siehe auch

- [Öffnen Sie die Quelle Big Data Applications mit Azure Lake Datenspeicher kompatibel](data-lake-store-compatible-oss-other-applications.md)
 
