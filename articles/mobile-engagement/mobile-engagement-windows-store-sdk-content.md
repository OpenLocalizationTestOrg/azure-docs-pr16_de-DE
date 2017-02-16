<properties 
    pageTitle="Windows SDK für Universal Apps-Inhalt" 
    description="Informationen Sie zu den Inhalt des Windows universeller Apps SDK für Azure Mobile Engagement"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-content"></a>Windows SDK für Universal Apps-Inhalt

Dieses Dokument Listen und den Inhalt von SDK in Ihrer Anwendung bereitgestellt werden.

##<a name="the-resources-folder"></a>Die `/Resources` Ordner

Dieser Ordner enthält alle Ressourcen, die Mobile Engagement muss. Sie können diese an Ihre app anpassen.

- `EngagementConfiguration.xml`: Konfigurationsdatei die Mobile-Engagement, dies ist die Stelle, an der Sie die Mobile Engagement Einstellungen (Mobile Engagement Verbindungszeichenfolge, Bericht Absturz...) anpassen können.

### <a name="html-folder"></a>paarweise Ordner

- `EngagementNotification.html`: Die `Notification` web-Ansicht HTML-Design für innerhalb der app-Banner.

- `EngagementAnnouncement.html`: Die `Announcement` web-Ansicht HTML-Design für interstitielles innerhalb der app-Ansichten.

### <a name="images-folder"></a>Ordners

- `EngagementIconNotification.png`: Am linken Rand eine Benachrichtigung angezeigt der-Markensymbol ersetzen diesen Termin durch Ihre Markensymbol.

- `EngagementIconOk.png`: Die `Ok` Symbol der Reichweite Inhaltsseiten für die Schaltfläche Aktion oder Überprüfung.

- `EngagementIconNOK.png`: Die `NOK` Symbol verwendet, wenn die Schaltfläche Überprüfung der Reichweite Inhaltsseiten deaktiviert ist.
 
- `EngagementIconClose.png`: Die `Close` Symbol Reichweite Benachrichtigungen und Inhalt für die Schaltfläche Schließen.

### <a name="overlay-folder"></a>/Overlay Ordner

- `EngagementPageOverlay.cs`: Die Überlagerung Seite hinzufügen des Projekts verantwortlich erreicht haben, in der app-Benutzeroberfläche zu untergeordneten ab.
  
