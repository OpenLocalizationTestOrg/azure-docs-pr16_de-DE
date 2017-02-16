<properties 
    pageTitle="Fehler bei der Authentifizierung Erkennung" 
    description="Der Datenverbindungs-Assistent für active Directory erkannt einen Authentifizierungstyp nicht kompatibel" 
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

# <a name="error-during-authentication-detection"></a>Fehler bei der Authentifizierung Erkennung

Während der Erkennen von vorherigen Authentifizierungscode, ermittelt der Assistent einen Authentifizierungstyp nicht kompatibel.   

##<a name="what-is-being-checked"></a>Was überprüft wird?

**Hinweis:** Um ordnungsgemäß vorherigen Authentifizierungscode in einem Projekt erkennen, muss das Projekt erstellt werden.  Wenn dieser Fehler und verfügen Sie nicht über einen vorherigen Authentifizierungscode in Ihrem Projekt, neu zu erstellen, und versuchen Sie es erneut.

###<a name="project-types"></a>Projekttypen

Der Assistent überprüft die Art des Projekts beherzigen, damit die richtigen Authentifizierungslogik in das Projekt eingeführt werden kann.  Wenn es einen beliebigen Controller an, die gibt von abgeleitet wird `ApiController` im Projekt, wird das Projekt ein Projekts WebAPI angesehen.  Wenn es nur Controller, die gibt von abgeleitet werden `MVC.Controller` im Projekt, das Projekt berücksichtigt werden MVC-Projekt.  Sonstiges wird vom Assistenten nicht unterstützt.  Web Forms Projekte werden derzeit nicht unterstützt.

###<a name="compatible-authentication-code"></a>Kompatible Authentifizierungscode

Der Assistent sucht auch nach Authentifizierungseinstellungen, die mit dem Assistenten zuvor konfiguriert wurden oder mit dem Assistenten kompatibel sind.  Wenn alle Einstellungen vorhanden sind, gilt eine eintrittsinvariant Groß-/Kleinschreibung und der Assistent öffnen und die Einstellungen anzeigen.  Wenn nur einige Einstellungen vorhanden sind, einen Fehler Fall gilt.

In einem Projekt MVC überprüft der Assistent eine der folgenden Einstellungen, die sich aus der vorherigen Verwendung des Assistenten ergeben:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Darüber hinaus prüft der Assistenten für eine der folgenden Optionen in einem Projekt Web-API, die sich aus der vorherigen Verwendung des Assistenten ergeben:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Inkompatible Authentifizierungscode

Der Assistent sucht schließlich, um Versionen von Authentifizierungscode erkennen, die mit älteren Versionen von Visual Studio konfiguriert wurden. Wenn Sie diese Fehlermeldung erhalten, bedeutet dies, dass es sich bei Ihrem Projekt einen inkompatiblen Authentifizierungstyp enthält. Der Assistent erkennt die folgenden Arten von Authentifizierung aus früheren Versionen von Visual Studio:

* Windows-Authentifizierung 
* Einzelne Benutzerkonten 
* Organisationskonto 
 

Um die Windows-Authentifizierung in einem Projekt MVC erkennen, sucht der Assistent nach der `authentication` Element aus der Datei **web.config** .

<pre>
    &lt;Konfiguration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;Authentifizierungsmodus = "Windows" /&gt;</span>
        &lt;/System.Web&gt;
    &lt;/Configuration&gt;
</pre>

Zum Ermitteln von Windows-Authentifizierung in einem Projekt Web-API sucht der Assistent nach der `IISExpressWindowsAuthentication` Element aus Ihres Projekts **csproj** -Datei:

<pre>
    &lt;Project&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;aktiviert&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/Project        &gt;
</pre>

Sucht der Assistent zum Erkennen von einzelnen Benutzerkonten Authentifizierung für das Paket Element aus der Datei **Packages.config** .

<pre>
    &lt;Pakete&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework Verpacken" Version = "2.1.0" TargetFramework = "net45" /&gt;</span>
    &lt;/Pakete&gt;
</pre>

Sucht der Assistent zum Erkennen von einer alten Form der Organisations-Konto-Authentifizierung für das folgende Element aus **web.config**:

<pre>
    &lt;Konfiguration&gt;
        &lt;AppSettings&gt;
            <span style="background-color: yellow">&lt;fügen Sie Key = "Ida: Bereich" Wert = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Um den Authentifizierungstyp zu ändern, entfernen Sie den inkompatiblen Authentifizierungstyp und führen Sie den Assistenten erneut aus.

Weitere Informationen finden Sie unter [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).