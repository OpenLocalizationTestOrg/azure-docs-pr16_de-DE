<properties 
    pageTitle="Erste Schritte mit Azure Active Directory und Visual Studio verbunden Services (MVC Projekte) | Microsoft Azure" 
    description="Erste Schritte mit Azure Active Directory in MVC Projekten, nach dem Herstellen einer Verbindung mit, oder erstellen eine Azure AD mit Visual Studio Services verbunden" 
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

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Erste Schritte mit Azure Active Directory und Visual Studio verbunden Services (MVC Projekte)

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-dotnet-getting-started.md)
> - [Was ist passiert](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Anfordern einer Authentifizierung für Access-Controller 

Alle Controller in Ihrem Projekt wurden mit dem Attribut **Autorisieren** gestaltet. Dieses Attribut wird den Benutzer authentifiziert werden, bevor Sie diese Controller erforderlich. Damit den Controller anonym zugegriffen werden kann, entfernen Sie dieses Attribut aus dem Controller aus. Wenn Sie die Berechtigungen auf einer höheren Ebene festlegen möchten, wenden Sie das Attribut für jede Methode, die Autorisierung, statt ihn auf die Controller-Klasse anwenden erforderlich ist.
 
##<a name="adding-signin--signout-controls"></a>Hinzufügen von Anmeldefenster / Abmeldung-Steuerelemente 

Hinzufügen eines zu Ihrer Ansicht die Steuerelemente Anmeldefenster/Abmeldung können die Teilansicht **_LoginPartial.cshtml** Sie die Funktionalität der Ansichten hinzufügen. Hier ist ein Beispiel für die Funktionalität zur Standardansicht **_Layout.cshtml** Ansicht hinzugefügt. (Beachten Sie das letzte Element in der Div mit Navigationsleiste / reduzieren-Klasse):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
