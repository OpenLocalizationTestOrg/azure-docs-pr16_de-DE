<properties 
    pageTitle="Azure Media-Dienste Konzepte | Microsoft Azure" 
    description="Dieses Thema enthält eine Übersicht der Azure Media Services Konzepte" 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>

#<a name="azure-media-services-concepts"></a>Azure Media-Dienste (Konzepte) 

Dieses Thema bietet einen Überblick über die wichtigsten Konzepte Media-Dienste.

##<a name="a-idassetsaassets-and-storage"></a><a id="assets"></a>Bestand und Speicher

###<a name="assets"></a>Posten

Eine [Anlage](https://msdn.microsoft.com/library/azure/hh974277.aspx) enthält digitalen Dateien (einschließlich Video, Audio, Bilder, Miniaturansichten Websitesammlungen, Textspuren und Untertitel-Dateien) und die Metadaten für diese Dateien. Nachdem die digitalen Dateien in einer Anlage hochgeladen werden, könnten sie in die Codierung und Workflows streaming Media-Dienste verwendet werden.

Eine Anlage zu einem Container Blob im Speicher Azure-Konto zugeordnet ist, und die Dateien in der Anlage werden als Blobs im Container gespeichert.

Bei der Entscheidung, welche Media-Inhalte hochladen und Speichern einer Anlageguts, gelten die folgenden Punkte:

- Eine Anlage sollte nur eine einzelne, eindeutige Instanz von Medieninhalten enthalten. Beispielsweise eine einzelne Bearbeiten eines TV-Folge, Film oder Werbung.
- Eine Anlage sollten nicht mehrere Formatvarianten oder Bearbeitungen einer audiovisuellen-Datei enthalten. Ein Beispiel für eine falsche Verwendung des eines Wirtschaftsguts würden versuchen, mehr als eine TV-Folge, Werbung oder mehrere Kamerawinkel aus einem einzelnen Herstellung innerhalb einer Anlage zu speichern. Speichern von mehreren Formatvarianten oder Bearbeitungen einer audiovisuellen-Datei in eine Anlage kann dazu führen, dass Ansprechpartner Codierung Aufträge, streaming und Schutz der Übermittlung der Anlage später im Workflow weiterleiten.  

###<a name="asset-file"></a>Bilddatei 
Eine [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) stellt eine tatsächliche Video- oder Audioclip-Datei, die in einem Blob-Container gespeichert ist. Eine Bilddatei immer eine Anlage zugeordnet ist, und eine Anlage kann eine oder mehrere Dateien enthalten. Der Vorgang Media Services Encoder schlägt fehl, wenn ein Objekt Dateiobjekt nicht mit einer digitalen Datei in einem Blob-Container verknüpft ist.

Die Instanz **AssetFile** und die ist-Media-Datei sind zwei verschiedene Objekte. Die Instanz AssetFile enthält Metadaten über die Media-Datei, während die Media-Datei der tatsächlichen Medieninhalten enthält.

Sie sollten nicht versuchen, die Inhalte der Blob-Container ändern, die ohne Media Service-APIs von Media Services generiert wurden.

###<a name="asset-encryption-options"></a>Optionen für Objekt-Verschlüsselung

Je nach Art der Inhalte, die Sie hochladen, speichern und vorführen möchten, bietet Media-Dienste verschiedene Verschlüsselungsoptionen, denen Sie auswählen können.

**Keine** Keine Verschlüsselung verwendet wird. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht übertragen werden oder bei Rest im Speicher geschützt ist.

Wenn Sie beabsichtigen, eine MP4 vorführen mit schrittweisen herunterladen, verwenden Sie diese option, um Ihre Inhalte hochzuladen.

**StorageEncrypted** – mit dieser Option können Sie Ihre löschen Inhalte lokal mit AES 256-Bit-Verschlüsselung Verschlüsseln und anschließendes Hochladen auf Azure-Speicher Speicherort auf Rest verschlüsselt werden. Posten mit Speicher Verschlüsselung geschützt werden automatisch entschlüsselt in eine verschlüsselte Dateisystem vor Codierung platziert und optional vor dem Hochladen wieder als eine neue Ausgabe Anlage erneut verschlüsselt. Die primäre Anwendungsfall-für die Verschlüsselung der Speicher ist, wenn Sie von hoher Qualität von Mediendateien mit Verschlüsselung statisch sind auf dem Datenträger sichern möchten. 

Um eine verschlüsselte Speicher-Anlage vorführen möchten, müssen Sie der Anlage Übermittlung Richtlinie konfigurieren, damit Media-Dienste weiß, wie Sie Ihre Inhalte bereitstellen möchten. Vor der Anlage gestreamt werden kann, der streaming-Server die Verschlüsselung Speicher entfernt, und streamt von Inhalten, die Richtlinie angegebenen Übermittlung (z. B. AES, PlayReady oder keine Verschlüsselung) verwenden. 

**CommonEncryptionProtected** - verwenden Sie diese option, wenn Sie verschlüsseln (oder bereits verschlüsselt hochladen) möchten Inhalt mit allgemeine Verschlüsselung oder PlayReady DRM (z. B. interpolierten Streaming mit PlayReady DRM geschützt).

**EnvelopeEncryptionProtected** – verwenden Sie diese option, wenn Sie schützen (oder bereits geschützt hochladen) möchten HTTP Live Streaming (HLS) verschlüsselt mit erweiterte Verschlüsselung AES (Standard). Beachten Sie, dass, wenn Sie bereits mit AES verschlüsselt HLS hochladen, es vom transformieren-Manager verschlüsselt worden sein muss.

###<a name="access-policy"></a>Zugriffsrichtlinie 

Eine [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) definiert Berechtigungen (wie lesen, schreiben und Liste) und Dauer des Zugriffs auf eine Anlage. Sie möchten ein Objekt AccessPolicy normalerweise zu einem Locator übergeben, die Zugriff auf die Dateien in einer Anlage enthaltenen verwendet werden sollte.


###<a name="blob-container"></a>BLOB-container

Ein Blob-Container bietet eine Gruppierung einer Reihe von Blobs. BLOB-Container werden in Media-Dienste als Begrenzungslinie Punkt für Access-Steuerelement und freigegebene Access Signatur (SAS) Locator zu Anlagen verwendet. Ein Konto Azure-Speicher kann eine unbegrenzte Anzahl von Blob-Container enthalten. Ein Containers kann eine unbegrenzte Anzahl von Blobs speichern.

>[AZURE.NOTE]Sie sollten nicht versuchen, die Inhalte der Blob-Container ändern, die ohne Media Service-APIs von Media Services generiert wurden.

###<a name="a-idlocatorsalocators"></a><a id="locators"></a>Locator

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s bieten einen Einstiegspunkt in einer Anlage enthaltenen Dateien zugreifen. Eine Zugriffsrichtlinie dient zum Definieren von Berechtigungen und Dauer ein Clients Zugriff auf eine bestimmte Ressource verfügt. Locator können-eine Beziehung mit einer Zugriffsrichtlinie haben, sodass andere Locator anderen Start- und Verbindungstypen für verschiedene Clients bereitstellen können während alle verwenden die gleiche Berechtigung und Dauer Einstellungen; wegen einer freigegebenen Access Richtlinie Einschränkung Festlegen von Diensten Azure-Speicher müssen Sie jedoch können nicht mehr als fünf eindeutige Locator gleichzeitig mit einer angegebenen Anlage zugeordnet ist. 

Media Services unterstützt zwei Arten von Locator: OnDemandOrigin Locator, verwendet, um das Streaming von Medien (z. B. MPEG Gedankenstrich, HLS oder interpolierten Streaming) oder schrittweise herunterladen Medien und Locator SAS-URL, hochladen oder Herunterladen von Medien Dateien To\from Azure-Speicher verwendet. 

Beachten Sie, dass die Berechtigung (AccessPermissions.List) beim Erstellen eines OrDemandOrigin Locators nicht verwendet werden soll. 

###<a name="storage-account"></a>Speicher-Konto

Alle Zugriff auf Azure-Speicher erfolgt über ein Speicherkonto. Ein oder mehrere Speicherkonten kann eine Medien Dienstkontos zuordnen. Ein Konto kann eine unbegrenzte Anzahl von Containern, enthalten, solange die Gesamtgröße unter 500 TB pro Speicher-Konto ist.  Media Services bietet SDK Ebene Tools zum ermöglichen es Ihnen, verwalten Sie mehrere Speicherkonten, und laden die Verteilung Ihrer Anlagen beim Upload auf diese Konten auf Kennzahlen oder zufällige Verteilung nach Saldo. Weitere Informationen finden Sie unter Arbeiten mit [Azure-Speicher](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

##<a name="jobs-and-tasks"></a>Projekte und Vorgänge

Eine [Position](https://msdn.microsoft.com/library/azure/hh974289.aspx) in der Regel wird den Prozess (z. B. indizieren oder codieren) eine Präsentation von Audio und Video. Wenn Sie mehrere Videos verarbeiten, erstellen Sie einen Auftrag für jedes Video codiert werden.

Ein Projekt enthält Metadaten für die Verarbeitung ausgeführt werden. Jeder Auftrag enthält mindestens ein [Vorgang](https://msdn.microsoft.com/library/azure/hh974286.aspx)s, die angeben, dass eine Aufgabe atomaren Verarbeitung, deren Eingabewerte Posten, Bestand, einen Medienprozessor und der zugehörigen Einstellungen ausgeben. Vorgänge innerhalb eines Auftrags zusammen, verkettet werden können, in dem die Anlage Ausgabe eines Vorgangs als die Eingabewerte Anlage zur nächsten Aufgabe angegeben wird. Auf diese Weise kann eine Position gesamte Verarbeitung notwendigen für eine Präsentation Medien enthalten.

##<a name="a-idencodingaencoding"></a><a id="encoding"></a>Codierung

Azure Media Services bietet mehrere Optionen für die Codierung von Medien in der Cloud.

Beim Starten von mit den Diensten von Medien ab, ist es wichtig, den Unterschied zwischen den Formaten Codecs und Datei zu verstehen.
Codecs sind die Software, die die Komprimierung/ursprünglich Algorithmen implementiert während Dateiformate Container sind, die das komprimierte Video zu halten.

Media Services bietet dynamische Verpacken, dem Sie Ihre adaptive Bitrate MP4 oder interpolierten Streaming codierte Inhalt von Media-Dienste (MPEG Gedankenstrich HLS, interpolierten Streaming, HDS) unterstützten Formate streaming vorführen kann ohne dass Sie in den folgenden Formaten streaming erneut verpacken.

Um [dynamische Verpackung](media-services-dynamic-packaging-overview.md)nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren der Datei Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien oder adaptive Bitrate interpolierten Streaming-Dateien (die Codierung Schritte sind weiter unten in diesem Lernprogramm gezeigt).
- Holen Sie mindestens eine bei Bedarf streaming Einheit für den streaming Endpunkt, aus dem Sie bis zur Bereitstellung des Inhalts planen. Weitere Informationen finden Sie unter [So skalieren bei Bedarf Streaming reservierte Einheiten](media-services-portal-manage-streaming-endpoints.md).

Media-Dienste unterstützt die folgenden auf Demand Encoder, die in diesem Artikel beschrieben sind:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Informationen zu unterstützten Encoder finden Sie unter [Encoder](media-services-encode-asset.md).


##<a name="live-streaming"></a>Live Streaming

Im Azure Media Services stellt ein Kanal einer Verkaufspipeline für die Verarbeitung von live streaming von Inhalten. Ein Kanal empfängt live Eingabewerte Streams in einem der beiden folgenden Arten:

- Ein lokale live Encoder sendet Multi-Bitrate RTMP oder interpolierten Streaming (MP4 fragmentiert) an den Kanal an. Sie können die folgenden live Encoder, die Multi-Bitrate interpolierten Streaming ausgeben: Elementen, Envivio, Cisco. Die folgenden live Encoder ausgeben RTMP: Adobe Flash Live, Telestream Wirecast und Tricaster Kodierungsprogramme. Die Motor angesaugten Streams passieren Kanäle ohne weitere Verarbeitung. Wenn Sie aufgefordert werden, bietet ein Media-Dienste Streams.

- Ein einzelnes Bitrate Stream (in einem der folgenden Formate: RTP (MPEG-Terminaldienste)), RTMP oder interpolierten Streaming (MP4 fragmentiert)) wird gesendet, um den Kanal, die auszuführenden live Codierung mit den Diensten von Medien aktiviert ist. Klicken Sie dann der Kanal führt live-Codierung des eingehenden einzelnen Bitrate Stream zu einem Videodatenstrom Multi-Bitrate (adaptive). Wenn Sie aufgefordert werden, bietet ein Media-Dienste Streams.

###<a name="channel"></a>Kanal

Media-Dienste [Channel](https://msdn.microsoft.com/library/azure/dn783458.aspx)s Verarbeitung live streaming-Inhalten verantwortlich sind. Ein Kanal stellt von außen liegenden Tabellenblättern (Aufnahme URL), klicken Sie dann auf einem live Kodierungsprogramm anzugeben. Der Kanal empfängt live Eingabewerte Streams aus der live Kodierungsprogramm und für das streaming über eine oder mehrere StreamingEndpoints bereitgestellt. Kanäle bieten auch einen Endpunkt Preview (Vorschau-URL), den Sie in der Vorschau und überprüfen Ihre Stream, bevor Sie weitere Verarbeitung und Übermittlung verwenden.

Wenn Sie den Kanal erstellen, können Sie die URL für die Erfassung und die Vorschau-URL erhalten. Um diese URLs zu gelangen, verfügt der Kanal nicht im Schritte Zustand sein. Wenn Sie zum Senden der Daten aus einem live Kodierungsprogramm in den Kanal starten bereit sind, muss der Kanal gestartet werden. Nachdem die live Kodierungsprogramm Aufnahme Daten startet, können Sie Ihre Stream Vorschau anzeigen.

Jedes Konto Media-Dienste kann mehrere Kanäle, mehrere Programme und mehrere StreamingEndpoints enthalten. Je nach den Erfordernissen Bandbreite und Sicherheit können StreamingEndpoint Services auf einen oder mehrere Kanäle reserviert sein. Alle StreamingEndpoint kann von einem beliebigen Kanal abrufen.


###<a name="program"></a>Programm

Ein [Programm](https://msdn.microsoft.com/library/azure/dn783463.aspx) ermöglicht es Ihnen, die für die Veröffentlichung und Speicherung Segmente in einem live Stream steuern. Kanäle verwalten Programme. Die Beziehung Channel und Programm ist sehr ähnlich wie herkömmliche Medien, wo ein Kanal hat einen Konstanten Videodatenstrom von Inhalten und ein Programm ist auf einigen terminierten Ereignis in diesem Kanal ausgelegte.
Sie können angeben, die Anzahl der Stunden, die den aufgezeichneten Inhalt für das Programm durch Festlegen der Eigenschaft **ArchiveWindowLength** beibehalten werden sollen. Dieser Wert kann maximal 25 Stunden aus mindestens 5 Minuten festgelegt werden.

ArchiveWindowLength schreibt auch vor, dass die maximale Größe des Zeit-Clients aus der die aktuelle Position live zeitlich rückwärts anfordern kann. Programme ausgeführt werden können, über den festgelegten Zeitraum, aber Inhalt, der hinter dem Fensterlänge fällt wird kontinuierlich verworfen. Dieser Wert dieser Eigenschaft bestimmt auch an, wie lange die Client-Manifeste wachsen können.

Jedes Programm ist eine Anlage zugeordnet. Um das Programm zu veröffentlichen, müssen Sie einen Locator für die zugeordnete Anlage erstellen. Diese Locator Probleme aktivieren Sie zum Erstellen eines streaming URL, die für Kunden bereitgestellt werden können.

Ein Kanal unterstützt bis zu drei gleichzeitig ausgeführte Programme an, sodass Sie mehrere Archiven des gleichen eingehende Streams erstellen können. So können Sie veröffentlichen und anderen Teile eines Ereignisses archivieren, je nach Bedarf. Beispielsweise ist Ihre geschäftliche Anforderung 6 Stunden eines Programms archivieren, sondern nur in den letzten 10 Minuten übertragen. Um dies zu erreichen, müssen Sie zwei gleichzeitig ausgeführte Programme erstellen. Archivieren von 6 Stunden des Ereignisses ein Programm festgelegt ist, aber das Programm wird nicht veröffentlicht. Das andere Programm für 10 Minuten archivieren festgelegt ist, und dieses Programm veröffentlicht wird.


Weitere Informationen finden Sie unter:

- [Arbeiten mit Kanäle, die zum Ausführen mit Azure Media Services Codierung Live aktiviert sind](media-services-manage-live-encoder-enabled-channels.md)
- [Arbeiten mit Kanäle, die Multi-Bitrate Live Stream von lokalen Encoder erhalten](media-services-live-streaming-with-onprem-encoders.md)
- [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Schützen von Inhalten

###<a name="dynamic-encryption"></a>Dynamic-Verschlüsselung

Azure Media Services ermöglicht Ihnen, Ihre Medien ab dem Zeitpunkt zu schützen, wenn sie Ihren Computer über Speicher, Verarbeitung und Übermittlung verlässt. Media Services können Sie den Inhalt dynamisch mit erweiterte Verschlüsselung AES (Standard) (mit 128-Bit-Verschlüsselung Tasten) und allgemeine Verschlüsselung (CENC) mit PlayReady und/oder Widevine DRM verschlüsselt vorführen. Media-Dienste stellt einen Dienst für autorisierte Clients Vortrags AES Tasten und PlayReady Lizenzen.

Aktuell, können Sie die folgenden streaming-Formate verschlüsseln: HLS, MPEG Gedankenstrich und interpolierten Streaming. Sie können nicht verschlüsseln HDS streaming-Format oder "Progressiv".

Wenn Sie für Media-Dienste zu eine Anlage verschlüsseln möchten, müssen Sie Zuordnen eines Verschlüsselungsschlüssels (CommonEncryption oder EnvelopeEncryption) mit der Anlage und auch Autorisierungsrichtlinien für die Taste konfigurieren.

Wenn Sie eine verschlüsselte Speicher-Anlage übertragen möchten, müssen Sie der Anlage Übermittlung Richtlinie konfigurieren, um anzugeben, wie Sie Ihre Anlage vorführen möchten.

Wenn ein Spieler ein Stream anfordert, verwendet Media-Dienste den angegebenen Schlüssel, um dynamisch von Inhalten über einen Umschlag-Verschlüsselung (mit AES) oder allgemeine Verschlüsselung (mit PlayReady Widevine) verschlüsseln. Entschlüsseln des Streams wird der Player die Taste vom Übermittlungsdienst Key anfordern. Um zu entscheiden, und zwar unabhängig davon, ob der Benutzer berechtigt ist, können Sie die Taste gelangen, wertet der Dienst die Autorisierungsrichtlinien, die Sie für den Schlüssel angegeben haben.


###<a name="token-restriction"></a>Token Einschränkung

Die Autorisierungsrichtlinie Inhalt Key konnte eine oder mehrere Autorisierung Einschränkungen haben: öffnen, Einschränkung oder IP-Einschränkung token. Die token eingeschränkte Richtlinie ein Token ausgestellt von einem Secure Token Service (STS) beizufügen. Media Services unterstützt Token in den einfachen Web Token (SWT) und JSON Web Token (JWT) Format an. Media Services bietet keine Secure Token Services. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS zu Problem Token nutzen können. Der STS müssen konfiguriert sein, um ein Token mit der angegebenen Schlüssel und Problem Ansprüchen aus, die Sie in der Konfiguration token Einschränkung angegeben angemeldet zu erstellen. Die wichtigsten Übermittlung Medien Dienste gibt die angeforderten Schlüssel (oder Lizenz) den Client, wenn das Token gültig ist und die Ansprüche im token Übereinstimmung so konfiguriert, die dass für die Taste (oder Lizenz) zurück.

Wenn die Richtlinie konfigurieren das Token beschränkt werden, müssen Sie die primäre Überprüfung Schlüssel, Herausgeber und Zielgruppe Parameter angeben. Die primäre Überprüfung Taste enthält den Schlüssel, dem mit das Token signiert wurde, Herausgeber ist der secure token Dienst, der das Token ausgestellt. Die Zielgruppe (auch als Bereich bezeichnet) beschreibt die Absicht des Token oder die Ressource das Token autorisiert Zugriff auf. Die wichtigsten Übermittlung Medien Dienste überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen.

Weitere Informationen finden Sie unter den folgenden Artikeln:

[Schützen von Inhalten Übersicht](media-services-content-protection-overview.md)
[Schützen mit AES-128](media-services-protect-with-aes128.md)
[mit DRM schützen](media-services-protect-with-drm.md)

##<a name="delivering"></a>Vorführen

###<a name="a-iddynamicpackagingadynamic-packaging"></a><a id="dynamic_packaging"></a>Dynamische Verpackung

Bei der Arbeit mit Media-Dienste, es empfohlen wird, um Ihre Mezzanine-Dateien in einer adaptive Bitrate MP4 codieren, festzulegen und zu konvertieren klicken Sie dann auf das gewünschte Format mithilfe der [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)festlegen.


###<a name="streaming-endpoint"></a>Streaming Endpunkt

Eine StreamingEndpoint stellt einen streaming-Dienst, mit der Inhalt vorführen, direkt auf eine Player-Clientanwendung oder zu einem Inhalt Delivery Network (CDN) für die weitere Verteilung (Azure Media Services bietet nun die Azure CDN-Integration). Ausgehende Streams aus einem Dienst StreamingEndpoint kann live-Streams, oder ein Video bei Bedarf Anlage in Ihrem Konto Media-Dienste. Darüber hinaus können Sie die Kapazität des Diensts StreamingEndpoint wachsende Bandbreite Anforderungen Anpassen von Farben-Skala Einheiten (auch bekannt als streaming Einheiten) zu behandeln steuern. Es wird empfohlen, eine oder mehrere Maßeinheiten für Applikationen in Herstellung-Umgebung zugewiesen werden. Maßeinheiten bieten Ihnen dedizierten Ausgang Kapazität, die in Schritten 200 MB / erworben werden kann und zusätzliche Funktionen, die aktuell verwenden dynamische Verpackung enthält.

Es wird empfohlen, dynamische Verpackung Sperren dynamic-Verschlüsselung verwendet wird. Um diese Features verwenden zu können, müssen Sie mindestens eine streaming Einheit für den Endpunkt verfügen, aus denen Sie übertragen möchten. Weitere Informationen finden Sie unter [Skalierung streaming Einheiten](media-services-portal-manage-streaming-endpoints.md).

Standardmäßig können bis zu 2 streaming Endpunkte in Ihr Konto Media-Dienste sein. Um einen höheren Grenzwert anfordern zu können, finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

Sie werden nur berechnet, wenn Ihre StreamingEndpoint in den Zustand "aktiv" ist.

###<a name="asset-delivery-policy"></a>Anlage Übermittlung Richtlinie

Einer der Schritte im Workflow Bereitstellung von Inhalten Media-Dienste ist [Übermittlung Richtlinien für Anlagen ](https://msdn.microsoft.com/library/azure/dn799055.aspx)konfigurieren die gewünschten gestreamt werden. Die Anlage Übermittlung Richtlinie teilt Media-Dienste wie für Ihre Anlage übermittelt werden sollen: in welcher streaming Protokoll sollte der Anlage dynamisch verpackt (z. B. MPEG Gedankenstrich, HLS, interpolierten Streaming, oder alle), ob Sie Ihre Anlage dynamisch verschlüsseln möchten, und wie (Briefumschlag oder allgemeine Verschlüsselung).

Wenn Sie eine verschlüsselte Speicher-Objekt, vor der Anlage gestreamt werden kann, der streaming-Server entfernt die Verschlüsselung Speicher und den Inhalt unter Verwendung der angegebenen Übermittlung Richtlinie streamt. Legen Sie beispielsweise Vorführung Ihrer Ressource verschlüsselt mit Verschlüsselungsschlüssels für erweiterte Verschlüsselung AES (Standard), den Richtlinientyp zu DynamicEnvelopeEncryption. Zum Entfernen von Speicher Verschlüsselung und Übertragen der Anlage löschen, legen Sie den Richtlinientyp auf NoDynamicEncryption aus.

###<a name="progressive-download"></a>Schrittweisen herunterladen

Schrittweisen herunterladen können Sie zum Starten der Wiedergabe von Medien, bevor die gesamte Datei heruntergeladen wurde. Sie können nur schrittweise MP4-Datei herunterladen.

Beachten Sie, dass verschlüsselte Posten entschlüsselt werden müssen, wenn Sie zur zum schrittweisen Download zur Verfügung stehen soll.

Um Benutzern mit schrittweisen Download URLs bereitstellen zu können, müssen Sie zuerst einen OnDemandOrigin Locator erstellen. Erstellen des Locators, bietet Ihnen den Basis Pfad auf die Anlage. Sie müssen den Namen der MP4-Datei anfügen. Beispiel:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Streaming URLs

Streaming von Inhalten an Clients. Um Benutzern mit streaming URLs bereitstellen zu können, müssen Sie zuerst einen OnDemandOrigin Locator erstellen. Erstellen des Locators, bietet Ihnen den grundlegenden Pfad auf die Anlage, die den Inhalt enthält, die, den Sie übertragen möchten. Damit dieses Inhaltstyps übertragen werden müssen Sie jedoch weiteren dieser Pfad ändern. Um eine vollständige URL der streaming Manifestdatei erstellen zu können, müssen Sie Verketten des Locator Pfadwert und das Manifest (filename.ism) Dateinamen ein. Anfügen von klicken Sie dann auf den Pfad Locator/Manifest und eine entsprechende Format (falls erforderlich).

Sie können auch den Inhalt über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen.

Beachten Sie, dass Sie nur über SSL streamen können, wenn der streaming Endpunkt aus dem Sie den Inhalt vorführen nach 10. September 2014 erstellt wurde. Wenn Ihre streaming URLs auf das streaming Endpunkte erstellt nach 10. September basieren, enthält die URL "streaming.mediaservices.windows.net" (das neue Format). SSL unterstützt Streaming URLs mit "origin.mediaservices.windows.net" (das alte Format) nicht. Wenn Ihre URL im alten Format ist und kann über SSL übertragen werden soll, erstellen Sie einen neuen streaming Endpunkt. Verwenden Sie URLs erstellt basierend auf den neuen streaming Endpunkt Streamen von Inhalten über SSL aus.

Die folgende Liste beschreibt die verschiedenen streaming-Formate und Beispiele für:

- Interpolierten Streaming

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest


- MPEG-BINDESTRICH

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=MPD-Time-CSF)



- Apple HTTP Live Streaming (HLS) V4

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL)



- Apple HTTP Live Streaming (HLS) V3

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL-V3)

- HDS (für nur Adobe vorzeigbare/Access Lizenznehmern)

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
