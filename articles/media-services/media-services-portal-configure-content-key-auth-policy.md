<properties 
    pageTitle="Inhalt Schlüssel Autorisierungsrichtlinie über das Azure-Portal konfigurieren | Microsoft Azure" 
    description="Informationen Sie zum Konfigurieren einer Autorisierungsrichtlinie für einen Inhalt Schlüssel." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Konfigurieren von Key Autorisierungsrichtlinie
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>(Übersicht)

Microsoft Azure Media Services ermöglicht es Ihnen MPEG-Strich, interpolierten Streaming und HTTP-Live-Streaming (HLS) Streams mit erweiterte Verschlüsselung AES (Standard) (mit 128-Bit-Verschlüsselung Tasten) oder [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)geschützt vorführen. AMS können Sie Gedankenstrich Streams mit Widevine DRM verschlüsselt vorführen. Sowohl die PlayReady Widevine verschlüsselt sind nach der Spezifikation der allgemeinen Verschlüsselung (ISO/IEC 23001 bis 7 CENC).

Media-Dienste stellt einen **Schlüssel/Lizenz Übermittlungsdienst** , aus denen Clients AES Tasten oder PlayReady/Widevine Lizenzen verschlüsselten Inhalt zum Ausprobieren abrufen können.

In diesem Thema wird gezeigt, wie das Azure-Portal verwenden, um den Inhalt Key Autorisierung konfigurieren. Die Taste kann später auf den Inhalt dynamisch verschlüsseln verwendet werden. Beachten Sie, dass die aktuell die folgenden streaming verschlüsseln können Formate: HLS, MPEG Gedankenstrich und interpolierten Streaming. Sie können nicht verschlüsseln HDS streaming-Format oder "Progressiv".

Wenn ein Spieler einen Stream, der dynamisch verschlüsselt werden festgelegt ist anfordert, verwendet Media-Dienste den konfigurierten Schlüssel zum dynamisches Anordnen von Inhalten mit AES- oder DRM-Verschlüsselung verschlüsseln. Entschlüsseln des Streams wird der Player die Taste vom Übermittlungsdienst Key anfordern. Um zu entscheiden, und zwar unabhängig davon, ob der Benutzer berechtigt ist, können Sie die Taste gelangen, wertet der Dienst die Autorisierungsrichtlinien, die Sie für den Schlüssel angegeben haben.


Wenn Sie planen, Sie haben mehrere Inhalt Schlüssel oder eine **Schlüssel/Lizenz Übermittlung Service** URL als Dienst Key Übermittlung Media-Dienste angeben möchten, verwenden Sie Media Services .NET SDK oder den REST-APIs.

[Konfigurieren von Schlüssel Autorisierungsrichtlinie mit Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurieren Sie Inhalt Schlüssel Autorisierungsrichtlinie mit Media Services REST-API](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Einige Überlegungen Ursachen zurückzuführen:

- Um dynamische Verpackung und dynamische Verschlüsselung verwenden können, müssen Sie sicher, dass mindestens eine Einheit reservierte streaming vornehmen. Weitere Informationen finden Sie unter [So skalieren Media-Dienst](media-services-portal-manage-streaming-endpoints.md).
- Ihre Anlage muss eine Reihe von adaptive Bitrate MP4s oder adaptive Bitrate interpolierten Streaming-Dateien enthalten. Weitere Informationen finden Sie unter [Codieren einer Anlage](media-services-encode-asset.md).
- Der Schlüssel Übermittlung-Dienst werden ContentKeyAuthorizationPolicy und seine zugehörigen Objekte (Richtlinienoptionen und Einschränkungen) für 15 Minuten zwischengespeichert.  Wenn Sie eine ContentKeyAuthorizationPolicy erstellen und gibt an, dass eine Einschränkung "Token" verwenden, testen Sie ihn, und aktualisieren Sie die Richtlinie klicken Sie dann auf "Öffnen" Einschränkung, dauert es ungefähr 15 Minuten, bevor Sie die Richtlinie auf die "Öffnen" Version der Richtlinie wechselt.


##<a name="how-to-configure-the-key-authorization-policy"></a>So: die Autorisierungsrichtlinie Key konfigurieren

Wählen Sie zum Konfigurieren der Autorisierungsrichtlinie Key **CONTENT PROTECTION** Seite aus.

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern, die wichtigsten anzufordern. Die Autorisierungsrichtlinie Inhalt Key kann haben, **Öffnen**, **token**oder **IP** Autorisierung Einschränkungen (**IP** können mit den restlichen oder .NET SDK konfiguriert sein).

###<a name="open-restriction"></a>Öffnen der Einschränkung

Die Beschränkung **Öffnen** bedeutet dies, dass Vorführen des Systems die Taste an eine Person, die wichtigsten anfordert. Diese Einschränkung kann zu Testzwecken nützlich sein.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Token Einschränkung

Um die token eingeschränkte Richtlinie auszuwählen, drücken Sie die **TOKEN** .

Die **token** beschränkt Richtlinie ein Token ausgestellt von **Secure Token Service** (STS) beizufügen. Media Services unterstützt Token in den **Einfachen Web Token** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) und **JSON Web Token** (JWT) Format an. Informationen hierzu finden Sie unter [JWT token Authentifizierung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services bietet keine **Secure Token Services**. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS zu Problem Token nutzen können. Der STS müssen konfiguriert sein, um ein Token mit der angegebenen Schlüssel und Problem Ansprüchen aus, die Sie in der Konfiguration token Einschränkung angegeben angemeldet zu erstellen. Dienst Key Übermittlung Media-Dienste zurück des Verschlüsselungsschlüssels an den Kunden an, wenn das Token gültig ist und die Ansprüche im Token, die für den Inhalt Schlüssel konfiguriert sind entsprechen. Weitere Informationen finden Sie unter [Verwenden Azure ACS zu Problem Token](http://mingfeiy.com/acs-with-key-services).

Wenn die Richtlinie konfigurieren **TOKEN** beschränkt werden, müssen Sie Werte für die **Überprüfung Schlüssel**, **Herausgeber** und **Zielgruppe**festlegen. Die primäre Überprüfung Taste enthält den Schlüssel, dem mit das Token signiert wurde, Herausgeber ist der secure token Dienst, der das Token ausgestellt. Die Zielgruppe (auch als Bereich bezeichnet) beschreibt die Absicht des Token oder die Ressource das Token autorisiert Zugriff auf. Die wichtigsten Übermittlung Medien Dienste überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen.

###<a name="playready"></a>PlayReady

Beim Inhalt mit **PlayReady**schützen, ist eine der Maßnahmen, die Sie in Ihrer Autorisierungsrichtlinie angeben müssen einer XML-Zeichenfolge, die die PlayReady Lizenz Vorlage definiert. Standardmäßig ist die folgende Richtlinie festlegen:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Sie können klicken Sie auf die Schaltfläche zum **Importieren von Xml Richtlinie** und Bereitstellen einer anderen XML die entspricht dem XML-Schema definiert [hier](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Als Nächstes

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

