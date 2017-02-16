<properties 
    pageTitle="Media-Dienste Versionsinformationen | Microsoft Azure" 
    description="Media-Dienste – Versionsinformationen" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Lassen Sie Notizen Azure Media-Dienste

Diese Versionsinformationen zusammenfassen Änderungen aus früheren Versionen und bekannte Probleme.

>[AZURE.NOTE] Wir möchten von unseren Kunden hören und Beheben von Problemen, die Sie beeinflussen konzentrieren. Stellen Sie in der [Azure Media Services MSDN-Forum], wenn Sie ein Problem melden oder Fragen.


##<a name="a-idissuesacurrently-known-issues"></a><a id="issues"></a>Derzeit bekannte Probleme

### <a name="a-idgeneralissuesamedia-services-general-issues"></a><a id="general_issues"></a>Allgemeine Probleme Media-Dienste

Problem|Beschreibung
---|---
Mehrere häufig verwendete HTTP-Header werden nicht in die REST-API bereitgestellt.|Bei der Entwicklung von Medienservices-Clientanwendungen, die die REST-API verwenden Sie zu finden, die allgemeinen Felder von einigen HTTP (CLIENT-Anforderung-ID, einschließlich Anforderung-ID und zurück-CLIENT-Anfrage-ID) werden nicht unterstützt. Die Kopfzeilen werden in einem zukünftigen Update hinzugefügt werden.
Prozentkodierung ist nicht zulässig.|Media-Dienste verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Aus diesem Grund ist die prozentkodierung nicht zulässig. Der Wert der **Name** -Eigenschaft kann nicht die folgenden [Zeichen %-Codierung-reserviert](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)haben: !*'();:@&=+$,/?%#[]". Darüber hinaus gibt es nur kann eine "." für die Dateinamenerweiterung.
Die ListBlobs-Methode, die Teil der Azure-Speicher SDK Version 3.x fehlerhaft ist.|Media-Dienste generiert SAS URLs basierend auf der [2012-02-12](http://msdn.microsoft.com/library/azure/dn592123.aspx) -Version. Wenn Sie die Liste Blobs in einem Container Blob Azure-Speicher SDK verwenden möchten, verwenden Sie die [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) -Methode, die Teil des Azure-Speicher SDK-Version ist 2.x. Die ListBlobs Methode, die Teil der Azure-Speicher SDK Version 3.x schlägt fehl.
Media-Dienste begrenzungsebene Verfahren schränkt die Ressource: Einsatz für Applikationen, die übermäßige Anforderung an den Dienst vornehmen. Der Dienst möglicherweise den Dienst nicht verfügbar (503) HTTP-Statuscode zurück.|Weitere Informationen finden Sie unter der Beschreibung des 503 HTTP Statuscodes im Thema [Azure Media Services Fehlercodes](http://msdn.microsoft.com/library/azure/dn168949.aspx) .
Bei der Abfrage Elemente besteht maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST Version 2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. | Sie müssen **Überspringen** und **Optimieren** (.NET) verwenden / **oben** (REST) als in [diesem Beispiel .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und [in diesem Beispiel REST-API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)beschrieben. 
Einige Clients können auf ein Problem wiederholen Tag im Manifest interpolierten Streaming stoßen.|Weitere Informationen finden Sie [in diesem](media-services-deliver-content-overview.md#known-issues) Abschnitt aus.
Azure Media Services .NET SDK Objekte nicht serialisiert werden und daher funktionieren nicht mit Azure Zwischenspeichern.|Wenn Sie versuchen, des SDK AssetCollection Objekts zum Zwischenspeichern von Azure hinzugefügt, wird eine Ausnahme ausgelöst.
Codierung Aufträge möglicher Fehler mit einer Nachrichtenzeichenfolge "Phase: DownloadFile. Code: System.NullReferenceException ".|Der Codierung Workflow normalerweise ist video zu übersetzenden Datei auf eine Anlage Eingabewerte hochladen, und senden einen oder mehrere Codierung Einzelvorgänge für die Eingabewerte Objekt ohne weiteren ändern, die Anlage Eingabemethoden. Jedoch, wenn Sie die Eingabewerte Anlage (beispielsweise durch Hinzufügen/Löschen/Umbenennen von Dateien in der Anlage) ändern, möglicherweise dann nachfolgende Aufträge mit einem Fehler DownloadFile fehl. Die umgangen werden, die Eingabewerte Anlage löschen und erneut auf eine neue Anlage zu übersetzenden Datei hochladen. 

##<a name="a-idrestversionhistoryarest-api-version-history"></a><a id="rest_version_history"></a>REST-API Versionsverlauf

Informationen zu den Versionsverlauf Media Services REST-API finden Sie unter [Azure Media Services REST-API-Referenz].

##<a name="a-idjulychanges16ajuly-2016-release"></a><a id="july_changes16"></a>Juli 2016 Release

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Updates an Dateiliste (*. ISM) generiert Codieren von Aufgaben

Wenn eine Aufgabe Codierung Media Encoder Standard oder Azure Media Encoder gesendet wird, generiert der Codierung Vorgang eine [streaming Manifestdatei](media-services-deliver-content-overview.md) (* .ism) Datei in die Ausgabe der Anlage. Mit der neuesten Dienstversion wurde zur Syntax dieses streaming Manifestdatei aktualisiert.

>[AZURE.NOTE]Die Syntax der streaming Manifest (.ism) Datei ist für die interne Verwendung reserviert und kann in zukünftigen Versionen geändert. Bitte nicht ändern Sie oder bearbeiten Sie des Inhalts dieser Datei.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Eine neue Clientmanifest (*. Datei ISMC) wird in der Ausgabe generiert Anlage, wenn eine Aufgabe Codierung eine oder mehrere MP4-Dateien ausgegeben

Beginnend mit der neuesten Dienstversion nach Abschluss der eine Codierung Aufgabe, die eine generiert, weitere MP4-Dateien, die Ausgabe wird Anlage auch eine streaming Client Manifest (*.ismc) Datei enthalten. Die Datei .ismc verbessert die Leistung von dynamischen streaming. 

>[AZURE.NOTE]Die Syntax der Datei Client Manifest (.ismc) ist für die interne Verwendung reserviert und kann in zukünftigen Versionen geändert. Bitte nicht ändern Sie oder bearbeiten Sie des Inhalts dieser Datei.

Weitere Informationen finden Sie in [diesem](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) Blog.

### <a name="known-issues"></a>Bekannte Probleme

Einige Clients können Aross ein Problem wiederholen Tag im Manifest interpolierten Streaming stammen. Weitere Informationen finden Sie [in diesem](media-services-deliver-content-overview.md#known-issues) Abschnitt aus.

##<a name="a-idaprchanges16aapril-2016-release"></a><a id="apr_changes16"></a>April 2016 Release

### <a name="azure-media-analytics"></a>Azure Media Analytics

Azure Medien-Diensten eingeführt Azure Medien Analytics für leistungsfähige video Intelligence. Ausführliche Informationen finden Sie unter [Azure Media Services Analytics Übersicht](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Preview)

Azure Media Services können Sie Ihre HTTP-Live Streaming (HLS) Inhalt mit Apple FairPlay dynamisch verschlüsseln nun. Sie können auch AMS Lizenz Übermittlungsdienst verwenden, FairPlay Lizenzen Clients vorführen. Ausführlichere Informationen finden Sie unter [verwenden Azure Media-Dienste zu übertragen Sie Ihre HLS Inhalte geschützten mit Apple FairPlay ](media-services-protect-hls-with-fairplay.md).
  
##<a name="a-idfebchanges16afebruary-2016-release"></a><a id="feb_changes16"></a>Februar 2016 Release

Die neueste Version von Azure Media Services SDK für .NET (3.5.3) enthält eine verwandte Widevine-Fehlerkorrektur. Wurde das Problem: AssetDeliveryPolicy konnten nicht für mehrere Anlagen mit Widevine verschlüsselt wiederverwendet werden. Als Teil diese Fehlerkorrektur wurde die folgende Eigenschaft hinzugefügt, um das SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a name="a-idjanchanges16ajanuary-2016-release"></a><a id="jan_changes_16"></a>Januar 2016 Release

Reservierte Einheiten Codierung umbenannt, um Verwirrung Encoder Namen zu verringern.

Die Basic, Standard und Premium codieren reservierten Einheiten s1, S2, umbenannt werden und S3 reserviert Einheiten, Hilfethemas.  Grundlegende Codierung RUs heute mit Kunden werden als Bezeichnung Azure-Portal (und in der Rechnung) S1 angezeigt, während Standard und Premium wird die Etiketten S2 und S3 Hilfethemas angezeigt. 

##<a name="a-iddecchanges15adecember-2015-release"></a><a id="dec_changes_15"></a>Dezember 2015 Release

Das Team Azure SDK veröffentlicht eine neue Version des Pakets [Azure SDK für PHP](http://github.com/Azure/azure-sdk-for-php) , die Updates und neue Funktionen für Microsoft Azure Media Services enthält. Insbesondere der Azure Media Services SDK für PHP jetzt die neuesten Features für den [Schutz von Inhalten](media-services-content-protection-overview.md) unterstützt: dynamische Verschlüsselung mit AES und DRM (PlayReady und Widevine) mit und ohne Einschränkung Token. Darüber hinaus unterstützt Skalierung [Codierung Einheiten](media-services-dotnet-encoding-units.md).

Weitere Informationen finden Sie unter:

- Die [Microsoft Azure Media Services SDK für PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) -Blog.
- Schneller Einstieg in den folgenden [Codebeispielen](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) helfen Ihnen:
    - **vodworkflow_aes.PHP**: Dies ist eine Datei von PHP, die wie AES-128 dynamisches Verschlüsselung und Übermittlung Schlüsselverwaltungsdienst verwendet werden. Es basiert auf den .NET Beispieldaten, die in [diesem](media-services-protect-with-aes128.md) Artikel erläutert.
    - **vodworkflow_aes.PHP**: Dies ist eine Datei von PHP, die wie Dynamic PlayReady-Verschlüsselung und Lizenz Übermittlungsdienst verwendet werden. Es basiert auf den .NET Beispieldaten, die in [diesem](media-services-protect-with-drm.md) Artikel erläutert.
    - **scale_encoding_units.PHP**: Dies ist eine Datei von PHP, die wird gezeigt, wie Codierung reservierte Einheit skalieren.


##<a name="a-idnovchanges15anovember-2015-release"></a><a id="nov_changes_15"></a>November 2015 Release

Azure Media Services bietet nun Google Widevine Lizenz Übermittlungsdienst in der Cloud. Weitere Informationen hierzu finden Sie in [diesem Blog Ankündigung](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Siehe auch [in diesem Lernprogramm](media-services-protect-with-drm.md) und [GitHub Repository](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)aus. 

Beachten Sie, dass Widevine Lizenz Übermittlung Dienste von Azure Media Services befindet sich in der Vorschau. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a name="a-idoctchanges15aoctober-2015-release"></a><a id="oct_changes_15"></a>Oktober 2015 Release

Azure Media Services (AMS) ist in den folgenden Daten Centers live: Brasilien Süd, Indien "Westen", Indien Süd und Indien zentralen. Können Sie jetzt im klassischen Azure-Portal [Erstellen Medien Dienstkonten](media-services-portal-create-account.md) verwenden und verschiedene beschriebene Aufgaben ausführen [können](https://azure.microsoft.com/documentation/services/media-services/). Live-Codierung ist jedoch in diese Data Center nicht aktiviert. Darüber hinaus sind nicht alle Arten von reservierte Einheiten Codierung in diese Data Center zur Verfügung.

- Brasilien Süd: Nur Standard- und grundlegende Codierung reservierte Einheiten verfügbar sind.
- Indien Westen, Indien Süd und Indien zentralen: stehen nur grundlegende Codierung reservierte Einheiten


##<a name="a-idseptemberchanges15aseptember-2015-release"></a><a id="september_changes_15"></a>September 2015 Release 

- AMS bietet jetzt die Möglichkeit, die sowohl Demand (Bestellung) als und Live Streams mit Technologie Widevine modulare DRM schützen. Verwenden der folgenden Übermittlung Services-Partner können Sie beim Vorführen Widevine Lizenzen: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [CastLabs](http://castlabs.com/company/partners/azure/). Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (beginnend mit der Version 3.5.1) oder die REST-API können Sie Ihre AssetDeliveryConfiguration zum Verwenden von Widevine konfigurieren.  

- AMS hinzugefügt Unterstützung für Apple ProRes Videos. Sie können jetzt Ihre Videos QuickTime-Quelldateien hochladen, die Apple ProRes oder andere Codecs verwenden. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Jetzt können Media Encoder Standard untergeordnete Zuschneiden und live Archiv extrahieren möchten. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Die folgenden Filterung Aktualisierungen vorgenommen wurden: 

    - Sie können jetzt Apple HTTP Live Streaming (HLS) Format mit nur-Audio-Filter verwenden. Dieses Update ermöglicht es Ihnen, nur-Audio-Titel entfernen, indem Sie angeben (nur Audio = False) in der URL.
    - Beim Definieren von Filtern für Ihre Anlagen haben Sie jetzt die Möglichkeit zum Kombinieren von mehreren (bis zu 3) Filter in einer einzelnen URL.

    Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.

- AMS unterstützt jetzt ich-Frames in HLS v4. I-Frame Support optimiert Filme vor- und zurückspulen Vorgänge an. Standardmäßig sind alle HLS v4 Ausgänge I-Frame Wiedergabeliste (EXT-X-I-FRAME-STREAM-INF).
 
    Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.

##<a name="a-idaugustchanges15aaugust-2015-release"></a><a id="august_changes_15"></a>August 2015 Release

- Azure Media Services SDK für Java V0.8.0 Release und neue Beispiele sind jetzt verfügbar. Weitere Informationen finden Sie unter:

    - [Blogbeitrag](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Java-Beispiele repository](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Aktualisieren der Azure Media Player mit Unterstützung für mehrere Audiodatenstrom. Weitere Informationen finden Sie unter:
    - [Blogbeitrag](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a name="a-idjulychanges15ajuly-2015-release"></a><a id="july_changes_15"></a>Juli 2015 Release

- Ankündigung, die allgemeine Verfügbarkeit von Media Encoder Standard. Weitere Informationen finden Sie unter [diesen Blogbeitrag](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder Standard verwendet Voreinstellungen in [diesem](http://go.microsoft.com/fwlink/?LinkId=618336) Abschnitt beschrieben. Beachten Sie, dass beim codiert mithilfe einer Vorgabe für 4k, den Typ der **Premium** reserviert Einheit beziehen soll. Weitere Informationen finden Sie unter [So skalieren Codierung](media-services-scale-media-processing-overview.md).
- Live in Echtzeit Beschriftungen mit Azure Media Services und Player an. Weitere Informationen finden Sie unter [diesen Blogbeitrag](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Media-Dienste .NET SDK Updates

Azure Media Services .NET SDK ist Version 3.4.0.0. Die folgende Funktionalität wurde in dieser Version hinzugefügt:  

- Implementiert Unterstützung für live archivieren. Beachten Sie, dass Sie eine Anlage herunterladen können, die ein live-Archiv enthält.
- Implementiert Unterstützung für dynamischen Filtern.
- Implementiert Funktionen, die Benutzern Speichercontainer zu belassen, während die Anlage löschen ermöglicht.
- Updates im Zusammenhang mit Richtlinien in Kanäle wiederholen.
- Aktiviert **Media Encoder Premium Workflow**an.

##<a name="a-idjunechanges15ajune-2015-release"></a><a id="june_changes_15"></a>Juni 2015 Release

###<a name="media-services-net-sdk-updates"></a>Media-Dienste .NET SDK Updates

Azure Media Services .NET SDK ist Version 3.3.0.0. Die folgende Funktionalität wurde in dieser Version hinzugefügt:  

- Support für OpenId verbinden Discovery-Spezifikation
- Unterstützung für die Verarbeitung von Tasten Rollover auf Identität Anbieterseite. 

Wenn Sie einen Identitätsanbieter verwenden das Discovery-Dokument OpenID verbinden verfügbar macht (so, wie die folgenden Anbieter: Azure Active Directory, Google, Vertrieb), können Sie anweisen, Azure Media Services, um signierenden Schlüssel beziehen, für die Überprüfung des JWT Token aus OpenID verbinden Discovery-Spezifikation. 

Weitere Informationen finden Sie unter [Verwenden von Json Web Keys aus OpenID verbinden Discovery-Spezifikation für die Arbeit mit JWT token Authentifizierung in Azure Media-Dienste](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a name="a-idmaychanges15amay-2015-release"></a><a id="may_changes_15"></a>Mai 2015 Release

Ankündigung die folgenden neuen Features:

- [Eine Vorschau der Live-Codierung mit Media-Dienste](media-services-manage-live-encoder-enabled-channels.md)
- [Dynamische manifest](media-services-dynamic-manifest-overview.md)
- [Eine Vorschau der Azure Medien Hyperlapse Media-Prozessor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a name="a-idaprilchanges15aapril-2015-release"></a><a id="april_changes_15"></a>April 2015 Release

 ###<a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

- [Ankündigung von Azure MediaPlayer](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Beginnend mit Media Services REST 2.10 Kanäle, die so konfiguriert sind, dass ein Protokoll RTMP Aufnahme werden mit primären erstellt und sekundären Aufnahme URLs. Weitere Informationen finden Sie unter [Channel Aufnahme Konfigurationen](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Medien Indexer updates
- Unterstützung für Spanisch
- Neue Konfiguration XML-format

Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Media-Dienste .NET SDK Updates

Azure Media Services .NET SDK ist Version 3.2.0.0.

Im folgenden werden einige der Kundennamen gegenüberliegende Updates:

- **Unterbrechen der Änderung**: **TokenRestrictionTemplate.Issuer** und **TokenRestrictionTemplate.Audience** vom Typ String werden geändert.
- Wiederholen Sie Aktualisierungen im Zusammenhang mit Erstellen benutzerdefinierte Richtlinien.
- Updates im Zusammenhang mit Dateien hochladen/herunterladen.
- Die **MediaServicesCredentials** -Klasse akzeptiert jetzt primären und sekundären Access Steuerelement Endpunkt authentifizieren.



##<a name="a-idmarchchanges15amarch-2015-release"></a><a id="march_changes_15"></a>März 2015 Release

### <a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

- Media Services bietet nun Azure CDN Integration. Die Eigenschaft **CdnEnabled** wurde zur Unterstützung von Integration von **StreamingEndpoint**hinzugefügt.  **CdnEnabled** mit ab Version 2.9 REST-APIs verwendet werden kann (Weitere Informationen finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** mit .NET SDK ab Version 3.1.0.2 verwendet werden kann (Weitere Informationen finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- Ankündigung von **Media Encoder Premium Workflow**. Weitere Informationen finden Sie unter [Einführung in Premium Codierung in Azure Media-Dienste](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a name="a-idfebruarychanges15afebruary-2015-release"></a><a id="february_changes_15"></a>Februar 2015 Release

### <a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

Media Services REST-API ist jetzt Version 2.9. Beginnen mit dieser Version, können Sie die Azure CDN Integration mit streaming Endpunkte aktivieren. Weitere Informationen finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a name="a-idjanuarychanges15ajanuary-2015-release"></a><a id="january_changes_15"></a>Januar 2015 Release

### <a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

Ankündigung der allgemeinen Verfügbarkeit (GA) zum Schutz des Content mit dynamic-Verschlüsselung. Weitere Informationen finden Sie unter [Azure Media-Dienste auch streaming Sicherheit mit allgemeinen Verfügbarkeit der DRM-Technologie verbessert](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Media-Dienste .NET SDK Updates

Azure Media Services .NET SDK ist Version 3.1.0.1.

Diese Version den Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate Standardkonstruktor als veraltet gekennzeichnet Der neue Konstruktor akzeptiert "TokenType" als Argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a name="a-iddecemberchanges14adecember-2014-release"></a><a id="december_changes_14"></a>Dezember 2014 Release

###<a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

- Einige Updates und neue Funktionen wurden die Azure Indexer Medien Prozessor hinzugefügt. Weitere Informationen finden Sie unter [Versionsinformationen Azure Medien Indexer Version 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Eine neue REST-API, die Sie aktualisieren Codierung ermöglicht reserviert Einheiten hinzugefügt: [EncodingReservedUnitType mit weiteren](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- CORS hinzugefügte Unterstützung für Key Übermittlungsdienst.
- Verbesserte Leistung von Abfragen Autorisierung Richtlinienoptionen wurden fertig.
- In China Data Center die [Taste Übermittlung URL](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) ist jetzt pro Kunde (genau wie in anderen Data Center).
- Hinzugefügten HLS Auto Ziel Dauer. Wenn live streaming ausführen, wird die HLS immer dynamisch verpackt. Standardmäßig berechnet Media-Dienste automatisch HLS Segment Verpackung Verhältnis (FragmentsPerSegment) entsprechend dem Keyframe Intervall (KeyFrameInterval), auch als Gruppe von Bildern – GOP, die von den Live Encoder empfangene bezeichnet. Weitere Informationen finden Sie unter [Arbeiten mit Azure Media Services Live Streaming].
 
###<a name="media-services-net-sdk-updates"></a>Media-Dienste .NET SDK Updates

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) ist Version 3.1.0.0.
- Aktualisiert die Abhängigkeit .net SDK für .NET Framework 4.5.
- Eine neue API, die Sie aktualisieren Codierung reservierte Einheiten ermöglicht wird hinzugefügt. Weitere Informationen finden Sie unter [Aktualisieren der Typ der reservierte Einheit und zunehmender Codierung RUs verwenden .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Hinzugefügten JWT (JSON Web Token) Unterstützung für token Authentifizierung. Weitere Informationen finden Sie unter [JWT token Authentifizierung in Azure Media Services und Dynamic-Verschlüsselung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Hinzugefügten relativen Versatz für "BeginDate" und ExpirationDate in der Vorlage PlayReady Lizenz.


##<a name="a-idnovemberchanges14anovember-2014-release"></a><a id="november_changes_14"></a>November 2014 Release

- Media Services können Sie live interpolierten (FMP4) Streaming-Inhalten über SSL-Verbindung Aufnahme nun. Um über SSL Aufnahme, stellen Sie sicher, dass die Erfassung URL zu HTTPS aktualisiert.  Weitere Informationen zu live streaming, finden Sie unter [Arbeiten mit Azure Media Services Live Streaming].
- Beachten Sie, dass aktuell, Sie über eine SSL-Verbindung einen RTMP live Stream Aufnahme können nicht.
- Sie können auch den Inhalt über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen.
- Beachten Sie, dass Sie nur über SSL streamen können, wenn der streaming Endpunkt aus dem Sie den Inhalt vorführen nach 10. September 2014 erstellt wurde. Wenn Ihre streaming URLs auf das streaming Endpunkte erstellt nach 10. September basieren, enthält die URL "streaming.mediaservices.windows.net" (das neue Format). SSL unterstützt Streaming URLs mit "origin.mediaservices.windows.net" (das alte Format) nicht. Wenn Ihre URL im alten Format ist und kann über SSL, [erstellen einen neuen streaming Endpunkt](media-services-portal-manage-streaming-endpoints.md)übertragen werden soll. Verwenden Sie URLs erstellt basierend auf den neuen streaming Endpunkt Streamen von Inhalten über SSL aus.

##<a name="a-idoctoberchanges14aoctober-2014-release"></a><a id="october_changes_14"></a>Oktober 2014 Release

### <a name="a-idnewencoderreleaseamedia-services-encoder-release"></a><a id="new_encoder_release"></a>Media Encoder Release-Dienste

Ankündigung von der neuen Version von Media Services Azure Media Encoder. Mit den neuesten Azure Media Encoder nur für unterliegen ausgeben GB, aber andernfalls wird des neuen Encoders Features, die mit der vorherigen Encoder kompatibel. Weitere Informationen [Media Services Preise Details]).

### <a name="a-idoctsdkamedia-services-net-sdk"></a><a id="oct_sdk"></a>.NET SDK Media-Dienste 

Media Services SDK für .NET Extensions ist Version 2.0.0.3.

Media Services SDK für .NET ist Version 3.0.0.8.

Die folgenden Änderungen vorgenommen wurden:

- Umgestaltung in die Richtlinie Klassen "Wiederholen".
- Hinzufügen von Benutzer-Agentzeichenfolge auf HTTP-Anforderungsheader ein.
- Hinzufügen von Nuget wiederherstellen Buildschritt aus.
- Beheben von Szenario Tests durch, um X509 verwenden Zertifikat aus Repository.
- Überprüfen von Einstellungen aus, wenn Channel aktualisieren und streaming zu beenden.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Neues GitHub Repository zu Beispielen für Host-Media-Dienste

Beispiele befinden sich in [Azure Media Services Beispiele GitHub Repository](https://github.com/Azure/Azure-Media-Services-Samples).


##<a name="a-idseptemberchanges14aseptember-2014-release"></a><a id="september_changes_14"></a>September 2014 Release

Media Services REST Metadaten ist jetzt Version 2.7. Weitere Informationen zu den neuesten Updates für die restlichen finden Sie unter [Azure Media Services REST-API-Referenz].

Media Services SDK für .NET ist jetzt Version 3.0.0.7
 
### <a name="a-idsept14breakingchangesabreaking-changes"></a><a id="sept_14_breaking_changes"></a>Unterbrechen der Änderungen

* **Origin** wurde in [StreamingEndpoint]umbenannt.
* Eine Änderung im Standardverhalten bei Verwendung des **Klassischen Azure-Portal** codieren, und klicken Sie dann veröffentlichen MP4-Dateien.

Zuvor, wenn das klassische Azure-Portal Veröffentlichen einer einzelnen Datei MP4 Videoelemente für eine SAS-URL verwenden würden erstellt werden (SAS-URLs können Sie das Video von einer Blob-Speicher herunterladen). Wenn Sie der klassischen Azure-Portal mithilfe codieren, und klicken Sie dann Veröffentlichen einer einzelnen Datei Videoelemente für MP4, zeigt die generierte URL derzeit auf einen Endpunkt streaming Azure Media-Dienste.  Diese Änderung wirkt sich nicht auf MP4-Videos aus, die direkt geladen Media-Dienste und ohne Codierung von Azure Media Services veröffentlicht werden.

Aktuell, müssen Sie die folgenden zwei Optionen zur Lösung des Problems.

* Aktivieren von streaming Einheiten und Verwenden von dynamischen Verpackung MP4 Anlage auf eine Präsentation mit interpolierten streaming streamen.

* Erstellen einer SAS-Url zum Herunterladen der MP4 (oder schrittweise Wiedergabe). Weitere Informationen zum Erstellen eines SAS Locators finden Sie unter [Inhalt vorführen].


### <a name="a-idsept14gachangesanew-featuresscenarios-that-are-part-of-ga-release"></a><a id="sept_14_GA_changes"></a>Neue Features/Szenarien, die Bestandteil von GA-release

* **Indexer Medienprozessor**. Weitere Informationen finden Sie unter [Mediendateien mit Azure Medien Indexer Indizierung].

* Die [StreamingEndpoint] Entität ermöglicht jetzt mit benutzerdefinierten Domänennamen (Host) hinzufügen.

    Für einen benutzerdefinierten Domänennamen als streaming Media-Dienste-Endpunktnamen verwendet werden müssen Sie benutzerdefinierte Hostnamen an Ihre streaming Endpunkt hinzufügen. Verwenden Sie den REST-APIs von Medien Dienste oder .NET SDK benutzerdefinierte Hostnamen hinzufügen.
    
    Folgendes gilt:
    
    * Sie müssen der Besitzer des benutzerdefinierten Domänennamens.
    
    * Von Azure Media Services muss der Besitzer des Domänennamens überprüft werden. Erstellen Sie zum Überprüfen der Domäne einen, die Zuordnungen CName <MediaServicesAccountId>.<parent domain> So Verifydns. < Mediaservices Dns Zone >. 
    
    * Sie müssen ein anderes CName erstellen, die den benutzerdefinierten Hostnamen (z. B. sports.contoso.com), Ihre Medien Services StreamingEndpont Hostname (z. B. amstest.streaming.mediaservices.windows.net) zugeordnet ist.


    Weitere Informationen finden Sie unter der Eigenschaft **CustomHostNames** im Thema [StreamingEndpoint] .

### <a name="a-idsept14previewchangesanew-featuresscenarios-that-are-part-of-the-public-preview-release"></a><a id="sept_14_preview_changes"></a>Neue Features/Szenarien, die die public Preview-Version gehören

* Livevorschau Streaming. Weitere Informationen finden Sie unter [Arbeiten mit Azure Media Services Live Streaming].

* Key Übermittlungsdienst. Weitere Informationen finden Sie unter [verwenden AES-128 dynamisches Verschlüsselung und Übermittlung Schlüsselverwaltungsdienst].

* Dynamische AES-Verschlüsselung. Weitere Informationen finden Sie unter [verwenden AES-128 dynamisches Verschlüsselung und Übermittlung Schlüsselverwaltungsdienst].

* PlayReady Lizenz Übermittlungsdienst. Weitere Informationen finden Sie unter [Dynamic PlayReady-Verschlüsselung verwenden und Lizenz Übermittlungsdienst].

* Dynamic PlayReady-Verschlüsselung. Weitere Informationen finden Sie unter [Dynamic PlayReady-Verschlüsselung verwenden und Lizenz Übermittlungsdienst].

* Media-Dienste PlayReady Lizenz Vorlage. Weitere Informationen finden Sie unter [Übersicht über Media Services PlayReady Lizenz Vorlage].

* Streaming Speicher verschlüsselt Posten. Weitere Informationen finden Sie unter [Streaming-Speicher verschlüsselt Inhalt].

##<a name="a-idaugustchanges14aaugust-2014-release"></a><a id="august_changes_14"></a>August 2014 Release

Wenn Sie eine Anlage codieren, wird eine Anlage Ausgabe nach Abschluss des Auftrags Codierung erstellt. Bis zu dieser Version gefertigt Azure Media Services Encoder Metadaten für die Ausgabe Posten. Beginnen mit dieser Version des Encoders erzeugt auch Metadaten für die Eingabewerte Posten. Weitere Informationen finden Sie unter den Themen [Metadaten Eingabe] und [Ausgabe Metadaten] .


##<a name="a-idjulychanges14ajuly-2014-release"></a><a id="july_changes_14"></a>Juli 2014 Release

Die folgenden Updates wurden für die Azure Media Services-Manager und Encryptor vorgenommen:

* Nur Audio wiedergegeben wird, wenn Transmuxing einer Anlage live Archiv HTTP Live Streaming – dies behoben wurde und jetzt Audio und Video wiedergegeben werden.

* Beim Packen von einer Anlage auf HTTP-Live Streaming und AES 128-Bit-Umschlag Verschlüsselung verpackt Streams nicht wieder auf dem Android-Geräten wiedergeben – dieses Problem behoben wurde, und verpackt Streams gibt wieder auf dem Android-Geräten, die HTTP-Streaming Live unterstützen.

##<a name="a-idmaychanges14amay-2014-release"></a><a id="may_changes_14"></a>Mai 2014 Release

### <a name="a-idmay14changesageneral-media-services-updates"></a><a id="may_14_changes"></a>Allgemeine Media Services-Updates

Sie können jetzt [Dynamische Verpackung] zum Stream HTTP Live Streaming (HLS) v3 verwenden. Zum Streamen HLS v3, fügen Sie das folgende Format auf den Ursprung Locator Pfad: * .ism/manifest(format=m3u8-aapl-v3). Weitere Informationen finden Sie unter [Nick Drouins Blog].

Dynamische Verpackung jetzt Vorführung HLS (v3 und v4) mit PlayReady verschlüsselt unterstützt auch auf interpolierten Streaming statisch mit PlayReady verschlüsselt basierend. Informationen zum Verschlüsseln interpolierten Streaming mit PlayReady finden Sie unter [schützen interpolierten Stream mit PlayReady].

### <a name="a-namemay14donnetchangesamedia-services-net-sdk-updates"></a><a name="may_14_donnet_changes"></a>Media-Dienste .NET SDK Updates

Dazu gehören die folgenden Verbesserungen in die Medien Services .NET SDK 3.0.0.5-Version:

* Höhere Geschwindigkeit und Stabilität für den Upload/Download Media-Objekten.

* Verbesserte Behandlung von "Wiederholen" Logik und vorübergehend Ausnahmen: 

    * Vorübergehenden Fehler Erkennung und "Wiederholen" Logik wurden verbessert, für Ausnahmen, die indem Sie Abfragen, Änderungen zu speichern, Dateien hoch- oder heruntergeladen verursacht werden. 
    
    * Wenn Web Ausnahmen (z. B. während einer ACS-Anforderung token) wiedergegeben, sehen Sie sich, dass schwerwiegende Fehler schneller jetzt Fehlschlagen sind.

Weitere Informationen finden Sie unter [Wiederholen Logik im Media Services-SDK für .NET].

##<a name="a-idaprilchanges14aapril-2014-encoder-release"></a><a id="april_changes_14"></a>April 2014 Encoder Release

### <a name="a-nameapril14enocerchangesamedia-services-encoder-updates"></a><a name="april_14_enocer_changes"></a>Media Encoder Updates-Dienste

* Hinzugefügte Unterstützung für Aufnahme AVI-Dateien mit dem Gras Valley EDIUS nichtlinear-Editor erstellt, in dem das Video leicht ist komprimiert Gras Valley HQ/HQX Codec verwenden. Weitere Informationen finden Sie unter [Gras Valley Ankündigung EDIUS 7 Streaming über die Cloud].

* Hinzugefügte Unterstützung für die Benennungskonvention für den Media Encoder erstellten Dateien angeben. Weitere Informationen finden Sie unter [Steuern Medien Dienst Encoder Ausgabedateinamen].

* Unterstützung für video und/oder audio überlagert hinzugefügt. Weitere Informationen finden Sie unter [Erstellen von überlagert].

* Unterstützung für Zusammenfügen zusammen mehrere video Segmente hinzugefügt. Weitere Informationen finden Sie unter [Video Segmente zusammenfügen].

* Feste Einheiten ein Fehlers im Zusammenhang mit Transcoding der MP4s, in dem die Audiodatei mit MPEG-1 Audio Layer 3 (QuickInfos MP3) codiert wurde.


##<a name="a-idjanfebchanges14ajanuaryfebruary-2014-releases"></a><a id="jan_feb_changes_14"></a>Januar/Februar 2014 Versionen

### <a name="a-namejanfab14donnetchangesaazure-media-services-net-sdk-3001-3002-and-3003"></a><a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 und 3.0.0.3

Die Änderungen in 3.0.0.1 und 3.0.0.2 umfassen:

* Feste Probleme bei der Verwendung von LINQ-Abfragen mit OrderBy Anweisungen.

* Teilung Test Lösungen in [GitHub] in Einheit-basierten und Szenario-basierten Tests.

Weitere Details zu den Änderungen, finden Sie unter: [Azure Media Services .NET SDK 3.0.0.1 und 3.0.0.2 frei].

3.0.0.3 wurden folgenden Änderungen vorgenommen:

* Aktualisierte Azure-Speicher Abhängigkeiten Version 3.0.3.0 verwendet werden soll. 

* Abwärtskompatibilität Problem für 3.0 behoben. *.* Gibt frei. 


##<a name="a-iddecemberchanges13adecember-2013-release"></a><a id="december_changes_13"></a>Dezember 2013 Release

### <a name="a-namedec13donnetchangesaazure-media-services-net-sdk-3000"></a><a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0

>[AZURE.NOTE] 3.0.x.x Versionen sind nicht abwärtskompatibel mit 2.4.x.x Versionen.

Die neueste Version des Media Services SDK ist jetzt 3.0.0.0. Sie können das neueste Paket von Nuget herunterladen oder aus [GitHub]kennenzulernen.

Beginnend mit dem Media Services SDK Version 3.0.0.0, können Sie die Token [Azure Active Directory Access Control Service (ACS)] wiederverwenden. Weitere Informationen finden Sie im Abschnitt "Wiederverwendung von Access Steuerelement Dienst Token" im Thema [Herstellen einer Verbindung mit Media-Dienste mit dem Media Services SDK für .NET] aus.

### <a name="a-namedec13donnetextchangesaazure-media-services-net-sdk-extensions-2000"></a><a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK Erweiterungen 2.0.0.0

Die Azure Media Services .NET SDK Erweiterungen ist eine Reihe von Erweiterungsmethoden und Helper-Funktionen, die Vereinfachung des Codes und zur Entwicklung mit Azure Media-Dienste zu erleichtern. Sie können die neuesten Bits von [Azure Media Services .NET SDK Erweiterungen]abrufen.

##<a name="a-idnovemberchanges13anovember-2013-release"></a><a id="november_changes_13"></a>November 2013 Release

### <a name="a-namenov13donnetchangesaazure-media-services-net-sdk-changes"></a><a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK Änderungen

Beginnend mit dieser Version übernimmt die Media Services SDK für .NET vorübergehende Fehlerstrukturanalyse-Fehlern, die auftreten können, wenn Sie den Layer Media Services REST-API aufrufen.

##<a name="a-idaugustchanges13aaugust-2013-release"></a><a id="august_changes_13"></a>August 2013 Release

### <a name="a-nameaug13powershellchangesamedia-services-powershell-cmdlets-included-in-azure-sdk-tools"></a><a name="aug_13_powershell_changes"></a>Media Services PowerShell-Cmdlets im Lieferumfang von Azure Sdk-Tools

Die folgenden Media Services PowerShell-Cmdlets sind jetzt in [Azure-Sdk-Tools]enthalten.

* Get-AzureMediaServices 

    Beispielsweise `Get-AzureMediaServicesAccount`.

* Neue AzureMediaServicesAccount 

    Beispielsweise `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Neue AzureMediaServicesKey 

    Beispielsweise `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Entfernen-AzureMediaServicesAccount 

    Beispielsweise `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a name="a-idjunechanges13ajune-2013-release"></a><a id="june_changes_13"></a>Ab Juni 2013 Release

### <a name="a-namejune13generalchangesaazure-media-services-changes"></a><a name="june_13_general_changes"></a>Ändert Azure Media-Dienste

Die Änderungen, die in diesem Abschnitt erwähnt sind Updates, die in den Versionen ab Juni 2013 Media-Dienste enthalten.

* Die Möglichkeit, mehrere Speicherkonten mit einer Medien Dienstkontos zu verknüpfen. 

    StorageAccount
    
    Asset.StorageAccountName und Asset.StorageAccount

* Die Möglichkeit, Job.Priority zu aktualisieren. 

* Benachrichtigung verwandten Elemente und Eigenschaften: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Position

* Asset.Uri 

* Locator.Name 

### <a name="a-namejune13dotnetchangesaazure-media-services-net-sdk-changes"></a><a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK Änderungen

Die folgenden Änderungen einbezogen werden im Juni 2013 Media Services SDK frei. Das aktuelle Media Services SDK ist auf GitHub verfügbar.

* Beginnend mit der Version 2.3.0.0 der Media Services SDK unterstützt mehrere Speicher verknüpfen Konten, mit einem Konto Media-Dienste. Dieses Feature wird die folgenden APIs unterstützt:
    
    Die Art des IStorageAccount.
    
    Die Eigenschaft Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    Die Eigenschaft StorageAccount.
    
    Die Eigenschaft StorageAccountName.
    
    Weitere Informationen finden Sie unter [Verwalten von Dienstleistungen Medienobjekten über mehrere Speicherkonten hinweg].

* Benachrichtigung verknüpften APIs. Beginnen mit der Version 2.2.0.0, dass Sie die Möglichkeit, Azure Warteschlange Speicher Benachrichtigungen anhören haben. Weitere Informationen finden Sie unter [Behandeln von Medien Services Auftrag Benachrichtigungen].
    
    Die Eigenschaft Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Die Art des Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
    
    Die Art des Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
    
    Die Art des Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
    
    Die Art des Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
    
    Die Art des Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.

* Abhängigkeit von der Azure-Speicher Client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Abhängigkeit von OData 5.5 (Microsoft.Data.OData.dll).


##<a name="a-iddecemberchanges12adecember-2012-release"></a><a id="december_changes_12"></a>Version Dezember 2012

### <a name="a-namedec12dotnetchangesaazure-media-services-net-sdk-changes"></a><a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK Änderungen

* IntelliSense: Hinzugefügt fehlende Intellisense-Dokumentation für eine Vielzahl an.

* Microsoft.Practices.TransientFaultHandling.Core: Dadurch ein Problem weiterhin hatten eine Beziehung zu einer älteren Version von dieser Assembly, wo das SDK. Das SDK verweist auf jetzt Version 5.1.1209.1 dieser Assembly.

Updates, die Probleme im November 2012 SDK gefunden:

* IAsset.Locators.Count: Diese Anzahl wird jetzt richtig auf neue IAsset Schnittstellen gemeldet, nachdem alle Locator gelöscht wurden.

* IAssetFile.ContentFileSize: Dieser Wert wird nun ordnungsgemäß nach einem Upload von IAssetFile.Upload(filepath) festgelegt.

* IAssetFile.ContentFileSize: Diese Eigenschaft kann jetzt festgelegt werden, wenn Sie eine Bilddatei zu erstellen. Es wurde zuvor schreibgeschützt.

* IAssetFile.Upload(filepath): Dadurch ein Problem, wurde diese Methode synchron hochladen den folgenden Fehler beim Auslösen mehrerer Dateien auf die Anlage hochgeladen wird. Fehler: "Fehler bei der Authentifizierung der Anfrage-Server. Stellen Sie sicher, dass der Wert von Autorisierung Kopfzeile ordnungsgemäß einschließlich der Signatur formatiert ist."

* IAssetFile.UploadAsync: Dadurch ein Problem, wo konnte nicht mehr als 5 Dateien gleichzeitig hochgeladen werden.

* IAssetFile.UploadProgressChanged: Dieses Ereignis ist jetzt vom SDK bereitgestellt.

* IAssetFile.DownloadAsync (String, BlobTransferClient, ILocator, CancellationToken): Überladung dieser Methode wird jetzt bereitgestellt.

* IAssetFile.DownloadAsync: Dadurch ein Problem, wo konnte nicht mehr als 5 Dateien gleichzeitig heruntergeladen werden.

* IAssetFile.Delete(): Dadurch ein Problem einen löschen kann, eine Ausnahme auslösen, wenn keine Datei für den IAssetFile hochgeladen wurde.

* Aufträge: Dadurch einem Problem würde, in dem nicht Erstellen einer "MP4 interpolierten Streams Vorgang" mit einem "PlayReady Schutz Vorgang" mithilfe einer Auftragsvorlage verketten alle Aufgaben überhaupt.

* EncryptionUtils.GetCertificateFromStore(): Diese Methode löst nicht mehr eine null-Verweisausnahme aufgrund einer Fehlern suchen das Zertifikat basierend auf Konfigurationsprobleme Zertifikat.

##<a name="a-idnovemberchanges12anovember-2012-release"></a><a id="november_changes_12"></a>Version November 2012

Die Änderungen, die in diesem Abschnitt erwähnt wurden Updates, die im Lieferumfang von November 2012 (Version 2.0.0.0) SDK. Diese Änderungen erfordern möglicherweise Code geschrieben für die Juni 2012-Vorschau SDK lassen, um portiert oder geändert werden.

* Posten
    
    IAsset.Create(assetName) ist die Funktion zum Erstellen von nur Anlage. IAsset.Create uploads Dateien nicht mehr als Teil der Methode Anruf. Verwenden Sie IAssetFile für das Hochladen aus.
    
    Die Methode IAsset.Publish und der Wert der Enumeration AssetState.Publish wurden aus dem SDK Services entfernt. Code, der von diesem Wert abhängt muss neu geschrieben werden.

* FileInfo

    Diese Klasse wurde entfernt und durch IAssetFile ersetzt.

    IAssetFiles

    IAssetFile ersetzt FileInfo und verhält sich anders. Instanziieren Sie für die Verwendung des IAssetFiles-Objekts, gefolgt von einer Datei hochladen, entweder mithilfe der Media Services SDK oder der Azure-Speicher SDK aus. Folgenden IAssetFile.Upload überladenen können verwendet werden:

    * IAssetFile.Upload(filePath): Eine synchroner Methode, die blockiert den Thread und empfiehlt sich nur, wenn Sie eine einzelne Datei hochladen.
    
    * IAssetFile.UploadAsync (Dateipfad, BlobTransferClient, Locator, CancellationToken): eine asynchrone Methode. Dies ist das bevorzugte Upload Verfahren aus. 

    Bekanntes Problem: mit CancellationToken wird tatsächlich um den Upload; Abbrechen der Kündigung Status der Aufgaben kann jedoch eine ganze Reihe von Staaten sein. Sie müssen ordnungsgemäß fangen und behandeln Ausnahmen.

* Locator
    
    Die Origin-spezifische Versionen wurden entfernt. Der SAS-spezifischen Kontext. Locators.CreateSasLocator (Objekt, AccessPolicy) wird nicht mehr unterstützter oder durch GA entfernt gekennzeichnet: Finden Sie im Abschnitt Locator unter neue Funktionalität aktualisierte Verhalten aus.


##<a name="a-idjunechanges12ajune-2012-preview-release"></a><a id="june_changes_12"></a>Preview-Version von Juni 2012

Die folgende Funktionalität wurde eingeführt November des SDK.

* Löschen von Einheiten

    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey Objekte werden jetzt auf Objektebene, d. h. IObject.Delete(), statt mit Anforderung einer löschen in der Auflistung cloudMediaContext.ObjCollection.Delete(objInstance) wird gelöscht.

* Locator

    Locator müssen jetzt mithilfe der Methode CreateLocator erstellt werden, und verwenden die Werte der Enumeration LocatorType.SAS oder LocatorType.OnDemandOrigin als Argument für die bestimmten Typ von Locator, dass Sie erstellen möchten.

    Neue Eigenschaften wurden hinzugefügt Locator zu vereinfachen verwendbar URIs für den Inhalt zu erhalten. Diese Überarbeitung der Locator war bieten größere Flexibilität für zukünftige Drittanbieter-Erweiterbarkeit und vergrößern Center für erleichterte Bedienung für Medien-Clientanwendungen vorgesehen.

* Asynchrone Methode Support

    Asynchrone Unterstützung wurde auf alle Methoden hinzugefügt.


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST-API-Referenz]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Informationen zur Preisgestaltung Media-Dienste]: http://azure.microsoft.com/pricing/details/media-services/
[Eingeben von Metadaten]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Ausgeben von Metadaten]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Bereitstellung von Inhalten]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Mediendateien mit Azure Media Indexer Indizierung]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Arbeiten mit Azure Media Services Live Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Verwendung von AES-128 dynamisches Verschlüsselung und Key Übermittlungsdienst]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Mithilfe von PlayReady Dynamic Verschlüsselung und Lizenz Übermittlung Service]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media-Dienste PlayReady Lizenz Vorlage (Übersicht)]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Streaming Speicher verschlüsselten Inhalt]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynamische Verpackung]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouins-Blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Schützen von interpolierten Stream mit PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Wiederholen Sie die Logik in die Mediendienste SDK für .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Gras Valley Ankündigung EDIUS 7 Streaming durch die Cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Steuernde Media Encoder Ausgabedateinamen-Service]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Erstellen von überlagert]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Video Segmente zusammenfügen]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 und 3.0.0.2-Versionen]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure-Active Directory Access Control Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Verbinden mit Media-Dienste mit den Diensten Medien SDK für .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK Erweiterungen]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure-Sdk-tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Verwalten von Medien Services Posten über mehrere Speicherkonten hinweg]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Behandeln von Medien Services Auftrag Benachrichtigungen]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
