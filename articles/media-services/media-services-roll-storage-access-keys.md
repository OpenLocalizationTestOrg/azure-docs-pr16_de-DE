<properties 
    pageTitle="Media-Dienste nach parallelen Speicher Zugriffstasten aktualisieren | Microsoft Azure" 
    description="Dieser Artikel bieten Ihnen Leitfäden zum nach parallelen Speicher Zugriffstasten Media-Dienste zu aktualisieren." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Aktualisieren des Media-Dienste nach dem parallelen Speicher-Tastenkombinationen

Wenn Sie ein neues Azure Media Services-Konto erstellen, müssen Sie ein Konto Azure-Speicher auszuwählen, die zum Speichern Ihrer Medieninhalten verwendet wird. Beachten Sie, dass Sie bei Ihrem Konto Media-Dienste [mehrere Speicherkonten hinzufügen](meda-services-managing-multiple-storage-accounts.md) können.

Beim Erstellen ein neuen Kontos mit Speicher generiert Azure zwei 512-Bit-Speicher-Tastenkombinationen, die zum Authentifizieren des Zugriffs auf Ihr Speicherkonto verwendet werden. Wenn Sie um Ihre Verbindungen Speicher sicherer zu gestalten, empfiehlt es sich regelmäßig neu zu generieren und Ihre Speicher Zugriffstaste drehen. Zwei Zugriffstasten (primären und sekundären) stehen zur Verfügung, und aktivieren Sie zum Verwalten von Verbindungen mit dem Speicherkonto verwenden eine Access-Taste gedrückt, während Sie die anderen Zugriffstaste neu generieren. Dieses Verfahren wird auch als "paralleles Zugriffstasten" bezeichnet.

Media-Dienste hängt einen Speicherschlüssel bereitgestellt, um ihn aus. Die angegebene Speicher Zugriffstaste insbesondere abhängig der Locator, mit denen das Streaming oder Laden Sie Ihre Bestände jederzeit. Beim Erstellen ein Kontos AMS eine Abhängigkeit auf der primären Speicher Zugriffstaste standardmäßig ist aber auch als Benutzer können Sie die Speicher-Taste, die AMS weist aktualisieren. Sie müssen sicher wissen, welche Taste zu verwenden, befolgen Sie die Schritte in diesem Artikel beschriebenen Media-Dienste zu informieren. Wenn Speicher Zugriffstasten parallelen, müssen Sie auch sicherstellen, dass Ihre Locator aktualisieren, damit es wird keine Unterbrechung in Ihrem streaming Service (dieser Schritt ist auch unter dem Thema beschrieben).

>[AZURE.NOTE]Wenn Sie mehrere Speicherkonten verfügen, müssen Sie dieses Verfahren für jedes Speicherkonto durchführen.
>
>Stellen Sie vor dem Ausführen auf einem Konto Herstellung in diesem Thema beschriebenen Schritte, testen Sie diese auf einem Konto noch nicht produziert.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Schritt 1: Erstellen, jeweils sekundären Speicher Zugriffstaste

Beginnen Sie mit Neuerstellen sekundären Speicher-Taste. Standardmäßig ist die sekundäre Taste nicht von Medien-Diensten verwendet.  Informationen zum Speicher Tasten einsatzbereit finden Sie unter [So: anzeigen, kopieren und neu generieren Speicher Tasten Zugriff](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a name="a-idstep2astep-2--update-media-services-to-use-the-new-secondary-storage-key"></a><a id="step2"></a>Schritt 2: Aktualisieren Media-Dienste verwenden Sie den neuen sekundären Speicherschlüssel

Aktualisieren Sie Media-Dienste, um die Zugriffstaste sekundären Speicher zu verwenden. Sie können eine der folgenden beiden Methoden die Taste neu generierte Speicher mit Media-Diensten synchronisiert.

- Verwenden des Azure-Portals: um den Namen und Schlüssel Werte zu finden, wechseln Sie zu der Azure-Portal, und wählen Sie Ihr Konto. Das Fenster Einstellungen wird auf der rechten Seite angezeigt. Wählen Sie im Fenster Einstellungen Schlüssel aus. Je nach welcher Speicherschlüssel für die Media-Dienste zu synchronisierende mit aus, wählen Sie den Primärschlüssel synchronisieren oder sekundäre Key Synchronisierungsschaltfläche. In diesem Fall verwenden Sie die sekundäre Taste.

- Verwenden Sie Media-Dienste Verwaltung REST-API.

Im folgenden Code wird gezeigt, wie die https://endpoint/***SubscriptionId/Services/Mediaservices/Konten/Kontoname*/StorageAccounts/*StorageAccountName*Installeroption Anforderung zum Synchronisieren von des angegebenen Speicher Schlüssels mit Media-Dienste zu erstellen. In diesem Fall wird der sekundären Speicher Schlüsselwert verwendet. Weitere Informationen finden Sie unter [How to: verwenden Media Services Management REST-API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Aktualisieren Sie nach diesem Schritt vorhandene Locator (die den alten Speicherschlüssel abhängig sind), wie im folgenden Schritt gezeigt.

>[AZURE.NOTE]Warten Sie 30 Minuten bevor Operationen mit Media-Dienste (eine Locator von beispielsweise erstellen neue) akzeptieren, um alle Einfluss auf anstehende Aufträge zu verhindern.

##<a name="step-3-update-locators"></a>Schritt 3: Aktualisieren Locator

>[AZURE.NOTE]Wenn Speicher Zugriffstasten parallelen, müssen Sie sicherstellen Ihrer vorhandenen Locator aktualisieren, damit es keine Unterbrechung in Ihrem streaming Service ist.

Warten Sie mindestens 30 Minuten nach der Synchronisierung von den neuen Speicherschlüssel mit AMS. Klicken Sie dann können Sie Ihre Locator auf-Anforderung neu erstellen, damit sie die Taste angegebenen Speicher Abhängigkeit übernehmen und die vorhandene URL verwalten.

Beachten Sie, dass beim SAS Locator aktualisieren (oder neu erstellen), wird die URL jederzeit ändern.

>[AZURE.NOTE] Stellen Sie sicher, dass Sie die vorhandenen URLs der Ihrer auf-Anforderung Locator beibehalten, müssen Sie den vorhandenen Locator löschen und Erstellen eines neuen Kontos mit der gleichen ID.

.NET Beispiel unten zeigt, wie einen Locator mit der gleichen ID. neu erstellt

Private statische ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / Eigenschaften eines vorhandenen Locator speichern.
Varianz Anlage = Locator. Anlage; Var AccessPolicy = Locator. AccessPolicy; Var LocatorId = Locator. ID; Varianz StartDate = Locator. Startzeit; Var LocatorType = Locator. Geben Sie ein; Var LocatorName = Locator. Name;

Löschen Sie ALTER Locator.
Locator. Delete();

Wenn (Locator. ExpirationDateTime < = DateTime.UtcNow) {neue Ausnahme auslösen (String.Format ("kann nicht neu erstellen Locator Id = {0}, da deren Locator Ablaufzeit in der Vergangenheit liegt", Locator. ID)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Schritt 5: Erstellen, jeweils primären Speicher Zugriffstaste

Zugriffstaste primären Speicher neu zu generieren. Informationen zum Speicher Tasten einsatzbereit finden Sie unter [So: anzeigen, kopieren und neu generieren Speicher Tasten Zugriff](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Schritt 6: Update-Media-Dienste verwenden Sie den neuen primären Speicherschlüssel
    
Verwenden Sie dasselbe Verfahren, das wie in [Schritt 2](media-services-roll-storage-access-keys.md#step2) nur diesmal beschrieben synchronisieren die neuen primären Speicher Zugriffstaste mit dem Konto Media-Dienste.

>[AZURE.NOTE]Warten Sie 30 Minuten bevor Operationen mit Media-Dienste (eine Locator von beispielsweise erstellen neue) akzeptieren, um alle Einfluss auf anstehende Aufträge zu verhindern.

##<a name="step-7-update-locators"></a>Schritt 7: Update Locator  

Nach 30 Minuten können Sie Ihre auf-Anforderung Locator neu erstellen, damit diese den neuen primären Speicherschlüssel Abhängigkeit übernehmen und die vorhandene URL beibehalten.

Verwenden Sie dasselbe Verfahren, wie in [Schritt 3](media-services-roll-storage-access-keys.md#step-3-update-locators)beschrieben.


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Danksagungen 

Wir bestätigen Sie die folgenden Personen, die eine bestimmte Abteilung in Richtung dieses Dokument erstellen möchten: Cenk Dingiloglu, Mailand Gada, Seva Titov.
