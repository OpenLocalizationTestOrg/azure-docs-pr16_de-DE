<properties 
    pageTitle="Erste Schritte mit Azure Active Directory und Visual Studio verbunden Services (WebApi Projekte) | Microsoft Azure" 
    description="Erste Schritte mit Azure Active Directory in Projekten WebApi nach dem Herstellen einer Verbindung mit, oder erstellen eine Azure AD mit Visual Studio verbunden services" 
  services="active-directory"
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Erste Schritte mit Azure Active Directory und Visual Studio verbunden Services (WebApi Projekte)

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> - [Was ist passiert](vs-active-directory-webapi-what-happened.md)

##<a name="requiring-authentication-to-access-controllers"></a>Anfordern einer Authentifizierung für Access-Controller
 
Alle Controller in Ihrem Projekt wurden mit dem Attribut **Autorisieren** gestaltet. Dieses Attribut wird den Benutzer authentifiziert werden, bevor Sie den Zugriff auf die APIs definiert werden, indem Sie diese Controller erforderlich. Damit den Controller anonym zugegriffen werden kann, entfernen Sie dieses Attribut aus dem Controller aus. Wenn Sie die Berechtigungen auf einer höheren Ebene festlegen möchten, wenden Sie das Attribut für jede Methode, die Autorisierung, statt ihn auf die Controller-Klasse anwenden erforderlich ist.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
 