<properties 
    pageTitle="Bereitstellen einer ASP.NET MVC 5 mobile Web-app in Azure-App-Verwaltungsdienst" 
    description="Ein Lernprogramm, die zum Bereitstellen einer Web app Azure-App-Verwaltungsdienst verwenden mobile-Funktionen in ASP.NET MVC 5 Webanwendung zum Erstellen." 
    services="app-service" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/12/2016" 
    ms.author="cephalin;riande"/>


# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Bereitstellen einer ASP.NET MVC 5 mobile Web-app in Azure-App-Verwaltungsdienst

In diesem Lernprogramm vermittelt die Grundlagen zum Erstellen einer ASP.NET MVC 5 Web app, die Mobile geeignet ist, und klicken Sie auf App-Verwaltungsdienst Azure bereitgestellt. In diesem Lernprogramm benötigen Sie [Visual Studio Express 2013 für das Web] [ Visual Studio Express 2013] oder der professional Edition von Visual Studio, wenn Sie bereits mit. [Visual Studio 2015] können aber werden die Screenshots unterschiedlich sein und Sie müssen die ASP.NET 4.x Vorlagen verwenden.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Was werden Sie erstellen

In diesem Lernprogramm werden Sie einfache Konferenz-Auflistung-Anwendung, die im [Startprojekt]bereitgestellt wird mobile Features hinzufügen[StarterProject]. Das folgende Bildschirmabbild zeigt die ASP.NET-Sitzungen in der fertigen Anwendung wie im Browser Emulator in Internet Explorer 11 F12 Entwicklertools angezeigt.

![][FixedSessionsByTag]

Können die Internet Explorer 11 F12 Entwicklertools und das [Tool Fiddler] [ Fiddler] helfen, die Anwendung debuggen. 

## <a name="skills-youll-learn"></a>Qualifikationen finden Sie Informationen, werden

Hier ist Gelernte wird:

-   Informationen zum Visual Studio 2013 verwenden, um Ihre Web-Anwendung, direkt auf eine Web app in Azure-App-Dienst zu veröffentlichen.
-   Wie ASP.NET MVC 5 Vorlagen CSS Bootstrap Rahmen verwenden, um die Anzeige auf mobilen Geräten zu verbessern
-   So erstellen Sie Mobile-spezifische Ansichten zum Adressieren von bestimmter mobiler Browsern wie etwa iPhone und Android
-   So erstellen Sie reagiert Ansichten (Ansichten, die auf verschiedenen Browsern über Geräte reagieren)

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

Richten Sie Ihre Entwicklungsumgebung, indem Sie die Installation von Azure SDK für .NET 2.5.1 oder höher. 

1. Um die Azure SDK für .NET installiert haben, klicken Sie auf den folgenden Link. Wenn Sie Visual Studio 2013 noch nicht installiert haben, wird es durch den Link installiert werden. Dieses Lernprogramm erfordert Visual Studio 2013. [Azure SDK für Visual Studio 2013][AzureSDKVs2013]
1. Klicken Sie im Web Platform Installer klicken Sie auf **Installieren** , und fahren Sie mit der Installation.

Sie benötigen außerdem einen Emulator mobilen Browser. Eine der folgenden Aktionen funktionieren:

-   Browser Emulator in [Internet Explorer 11 F12 Entwicklertools] [ EmulatorIE11] (in allen mobilen Browser Screenshots verwendet). Es hat Benutzer-Agents Zeichenfolge Voreinstellungen für Windows Phone 8, Windows Phone 7 und Apple iPad an.
-   Browser Emulator in [Google Chrome DevTools][EmulatorChrome]. Voreinstellungen für zahlreiche Android-Geräten, ebenso wie Apple iPhone, Apple iPad und Amazon Kindle Fire enthaltenen. Es auch emuliert Touch Ereignisse.
-   [Opera Emulator für Mobilgeräte][EmulatorOpera]

Visual Studio-Projekte mit C\# Quellcode stehen in diesem Thema zu begleiten:

-   [Starter Project herunterladen][StarterProject]
-   [Project-Download abgeschlossen][CompletedProject]

##<a name="a-namebkmkdeploystarterprojectadeploy-the-starter-project-to-an-azure-web-app"></a><a name="bkmk_DeployStarterProject"></a>Bereitstellen einer Azure Web app die Startprojekts

1.  Laden Sie die Konferenz-Auflistung Anwendung [Starter Project][StarterProject].

2.  Klicken Sie dann im Windows-Explorer mit der rechten Maustaste in der heruntergeladenen ZIP-Datei, und wählen Sie *Eigenschaften*aus.

3.  Wählen Sie die Schaltfläche **Zulassen** aus, klicken Sie im Dialogfeld **Eigenschaften** . (Aufheben der Blockierung wird verhindert, dass eine sicherheitswarnung, die tritt auf, wenn Sie versuchen, eine *ZIP-* Datei verwenden, die Sie aus dem Internet heruntergeladen haben.)

4.  Mit der rechten Maustaste in der ZIP-Datei, und wählen Sie **Alle extrahieren** in der Datei extrahiert werden sollen. 

5.  Öffnen Sie die Datei *C#\Mvc5Mobile.sln* in Visual Studio.

6.  Klicken Sie im Explorer Lösung mit der rechten Maustaste in des Projekts, und klicken Sie auf **Veröffentlichen**.

    ![][DeployClickPublish]

7.  Klicken Sie im Web veröffentlichen auf **Microsoft Azure-App-Dienst**.

    ![][DeployClickWebSites]

8.  Wenn Sie bereits in Azure angemeldet nicht geschehen ist, klicken Sie auf **Konto hinzufügen**.

    ![][DeploySignIn]

9.  Geben Sie nach der Aufforderung zur Anmeldung bei Ihrem Konto Azure.

11. Im Dialogfeld Dienst App sollte jetzt Sie anzeigen, wie Sie sich angemeldet haben. Klicken Sie auf **neu**.

    ![][DeployNewWebsite]  

12. Geben Sie im Feld **Web App-Name** einen eindeutigen app Präfix an. Ihren vollständigen Web app-Namen werden * &lt;Präfix >*. azurewebsites.net. Darüber hinaus aktivieren Sie, oder geben Sie einen neuen Ressource an, in der **Ressourcengruppe**. Klicken Sie dann auf **neu** , um eine neue App-Serviceplan zu erstellen.

    ![][DeploySiteSettings]

13. Konfigurieren Sie der neuen App-Serviceplan, und klicken Sie auf **OK**. 

    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)

13. Klicken Sie auf **Erstellen**, klicken Sie im Dialogfeld App-Verwaltungsdienst erstellen.

    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 

13. Nach der Azure werden Ressourcen erstellt, das Web veröffentlichen Dialogfeld mit den Einstellungen für Ihre neue app gefüllt wird. Klicken Sie auf **Veröffentlichen**.

    ![][DeployPublishSite]

    Nach Abschluss wird Visual Studio Veröffentlichen des Startprojekts zu Azure Web app geöffnet Desktopbrowser live-Web-app angezeigt.

14. Starten den Emulator für mobile Browser, und kopieren Sie die URL für die Konferenz-Anwendung (*<prefix>*. azurewebsites.net) in den Emulator, und klicken Sie auf die rechte obere Schaltfläche, und wählen Sie **nach Tag durchsuchen**. Wenn Sie Internet Explorer 11 als Standardbrowser verwenden, Sie müssen nur eingeben `F12`, dann `Ctrl+8`, und ändern Sie das Browserprofil in **Windows Phone**. Die nachstehende Abbildung zeigt die Ansicht *AllTags* in Hochformat (von auswählen **nach Kategorie durchsuchen**).

    ![][AllTags]

>[AZURE.TIP] Während Sie die Anwendung MVC 5 von Visual Studio debuggen können, können Sie Web app in Azure erneut aus, um zu überprüfen, ob die live-Web app direkt aus Ihren mobilen Browser oder einen Browser Emulator veröffentlichen.

Die Anzeige kann sehr auf einem mobilen Gerät gelesen werden. Sie können auch bereits einige visuellen Effekte angewendet, indem Sie das Framework Bootstrap CSS angezeigt wird.
Klicken Sie auf den Link **ASP.NET** .

![][SessionsByTagASP.NET]

ASP.NET-Tag-Ansicht ist Zoom-gemäß dem Bildschirm Bootstrap automatisch ausgeführt wird. Sie können jedoch diese Ansicht im mobilen Browser besser entsprechend verbessern. Die Spalte " **Datum** " beträgt beispielsweise schwer zu lesen. Ändern Sie die Ansicht *AllTags* ausreicht Mobile geeignete später im Lernprogramm.

##<a name="a-namebkmkbootstrapa-bootstrap-css-framework"></a><a name="bkmk_bootstrap"></a>Bootstrap CSS Framework

Vorlage ist neu in der MVC 5 integrierten Bootstrap Support. Sie haben bereits gesehen, wie die verschiedenen Ansichten in Ihrer Anwendung sofort verbessert. Die Navigationsleiste am oberen beträgt beispielsweise automatisch reduzierbare, wenn die Breite des Browserfensters kleiner ist. Klicken Sie auf die Desktopbrowser versuchen Sie, Ändern der Größe des Browserfensters, und sehen Sie, wie die Navigationsleiste Aussehen und Funktionsweise ändert. Dies ist der reagiert Webdesign, das in Bootstrap integriert ist.

Öffnen, um anzuzeigen, wie das Web app ohne Bootstrap aussehen würde *App\_starten\\BundleConfig.cs* , und kommentieren Sie die Zeilen, die *bootstrap.js* und *bootstrap.css*enthalten. Im folgenden Code wird die letzten beiden Aussagen von der `RegisterBundles` Methode nach der Änderung:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Drücken Sie `Ctrl+F5` zum Ausführen der Anwendung.

Beachten Sie, dass die reduzierbare Navigationsleiste jetzt nur eine einfache Symbolen Liste ist. Klicken Sie erneut auf **nach Kategorie durchsuchen** , und klicken Sie auf **ASP.NET**.
In der mobilen Emulator befinden können Sie sehen jetzt, da es ist nicht mehr auf dem Bildschirm Zoom gemäß und seitwärts um rechts neben der Tabelle finden Sie unter Bildlauf müssen.

![][SessionsByTagASP.NETNoBootstrap]

Machen Sie Ihre Änderungen rückgängig, und aktualisieren Sie den mobilen Browser, um sicherzustellen, dass die Anzeige Mobile geeignete wiederhergestellt wurde.

Bootstrap ist nicht spezifisch für ASP.NET MVC 5, und Sie können diese Funktionen nutzen in jede Web-Anwendung. Aber es ist jetzt in die ASP.NET MVC 5-Projektvorlage integriert, damit Ihre MVC 5 Webanwendung Bootstrap standardmäßig nutzen kann.

Weitere Informationen zu Bootstrap, wechseln Sie zu der [Bootstrap] [ BootstrapSite] Website.

Im nächsten Abschnitt sehen Sie, wie Mobile-Browser bestimmte Ansichten bereitgestellt.

##<a name="a-namebkmkoverrideviewsa-override-the-views-layouts-and-partial-views"></a><a name="bkmk_overrideviews"></a>Überschreiben Sie die Ansichten, Layouts und teilweise Ansichten

Sie können einer beliebigen Ansicht (einschließlich Layouts und teilweise Ansichten) für mobile Browser im Allgemeinen für eine einzelne mobile Browser oder für eine bestimmte Browser außer Kraft setzen. Um eine Ansicht für Mobile-spezifische bereitzustellen, können Sie Kopieren einer Datei anzeigen und *hinzufügen. Mobile* auf den Dateinamen. Angenommen, um zur mobilen Ansicht *Index* zu erstellen, können Sie kopieren *Ansichten\\Start\\Index.cshtml* zu *Ansichten\\Start\\Index.Mobile.cshtml*.

In diesem Abschnitt erstellen Sie eine Layoutdatei Mobile-spezifische.

Wenn Sie beginnen möchten, kopieren *Ansichten\\freigegeben\\\_Layout.cshtml* zu *Ansichten\\freigegeben\\\_Layout.Mobile.cshtml*. Open * \_Layout.Mobile.cshtml* und ändern Sie den Titel von **MVC5 Anwendung** in **MVC5-Anwendung (Mobil)**.

In den einzelnen `Html.ActionLink` für die Navigationsleiste anrufen, "Nach Durchsuchen" in den einzelnen Links *ActionLink*entfernen. Im folgenden Code wird der fertigen `<ul class="nav navbar-nav">` Tag der Datei Layout für mobile Geräte.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Kopieren der *Ansichten\\Start\\AllTags.cshtml* legen Sie *Ansichten\\Start\\AllTags.Mobile.cshtml*. Öffnen Sie die neue Datei und ändern die `<h2>` Element aus "Kategorien" auf "Kategorien (M)":

    <h2>Tags (M)</h2>

Navigieren Sie zu der Seite Kategorien, einem Desktopbrowser und mobile Browser Emulator verwenden. Der mobile Browser Emulator zeigt zwei vorgenommenen Änderungen (den Titel aus * \_Layout.Mobile.cshtml* und den Titel aus *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Im Gegensatz dazu die desktop-Anzeige wurde nicht geändert (mit Titeln aus * \_Layout.cshtml* und *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

##<a name="a-namebkmkbrowserviewsa-create-browser-specific-views"></a><a name="bkmk_browserviews"></a>Erstellen von Browser-spezifische anzeigen

Zusätzlich zu Mobile-spezifische und Desktop-spezifische Ansichten können Sie Ansichten für eine einzelne Browser erstellen. Beispielsweise können Sie Ansichten erstellen, die speziell für iPhone oder Android-Browser sind. In diesem Abschnitt erstellen Sie ein Layout für den iPhone-Browser und ein iPhone-Version der Ansicht *AllTags* .

Öffnen Sie die Datei *Global.asax* , und fügen Sie den folgenden Code an das Ende der `Application_Start` Methode.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Dieser Code definiert einen neuen Anzeigemodus mit dem Namen "iPhone", die für jede eingehende Anforderung gesucht werden soll. Wenn die eingehende Anforderung der Bedingung entspricht, die Sie definiert (d. h., wenn der Benutzer-Agents die Zeichenfolge "iPhone" enthält), wird ASP.NET-MVC Ansichten suchen, deren Name das Suffix "iPhone" enthält.

>[AZURE.NOTE] Beim Hinzufügen von Anzeigemodi für mobile Browser-spezifische, beispielsweise für iPhone und Android, müssen Sie das erste Argument festlegen auf `0` (Fügen Sie am oberen Rand der Liste) um sicherzustellen, dass Sie der Browser-spezifische Modus mobile-Vorlage Vorrang vor (*. Mobile.cshtml). Wenn die mobile Vorlage stattdessen am oberen Rand der Liste ist, wird es über Ihren vorgesehenen Anzeigemodus (die erste Übereinstimmung Vorrang, und die mobile Vorlage entspricht allen mobile Browsern) ausgewählt werden. 

In den Code, mit der Maustaste `DefaultDisplayMode`, wählen Sie **beheben**aus, und wählen Sie dann `using System.Web.WebPages;`. Dies fügt einen Verweis auf die `System.Web.WebPages` Namespace, der Where ist die `DisplayModeProvider` und `DefaultDisplayMode` Typen definiert sind.

![][ResolveDefaultDisplayMode]

Alternativ können Sie nur manuell die folgende Zeile zum Hinzufügen der `using` Abschnitt der Datei.

    using System.Web.WebPages;

Die Änderungen zu speichern. Kopieren der *Ansichten\\freigegeben\\\_Layout.Mobile.cshtml* legen Sie *Ansichten\\freigegeben\\\_Layout.iPhone.cshtml*. Öffnen Sie die neue Datei und ändern Sie dann den Titel aus `MVC5 Application (Mobile)` auf `MVC5 Application (iPhone)`.

Kopieren der *Ansichten\\Start\\AllTags.Mobile.cshtml* legen Sie *Ansichten\\Start\\AllTags.iPhone.cshtml*. Ändern Sie die neue Datei, die `<h2>` Element von "Tags (M)", "Tags (iPhone)".

Führen Sie die Anwendung. Führen Sie einen Emulator für mobile Browser, stellen Sie sicher, dass die Benutzer-Agents "iPhone" festgelegt ist, und navigieren Sie zu der Ansicht *AllTags* . Wenn Sie den Emulator in Internet Explorer 11 F12 Entwicklertools verwenden, konfigurieren Sie Emulation wie folgt:

-   Browserprofil = **Windows Phone**
-   Benutzer-Agentzeichenfolge = **Custom**
-   Benutzerdefinierte Zeichenfolge = **Apple-iPhone5C1/1001.525**

Das folgende Bildschirmabbild zeigt die *AllTags* Ansicht im Emulator in Internet Explorer 11 F12 Entwicklertools mit diesem benutzerdefinierten Text gerendert (Dies ist eine iPhone 5 C Benutzer-Agentzeichenfolge).

![][AllTagsIPhone_LayoutIPhone]

Wählen Sie die **Lautsprecher** -Link im mobilen Browser aus. Da es keine zur mobilen Ansicht (*AllSpeakers.Mobile.cshtml*), die Lautsprecher Standardansicht (*AllSpeakers.cshtml*) gerendert wird in der Ansicht Layout für mobile Geräte (*\_Layout.Mobile.cshtml*). Wie unten dargestellt, wird der Titel **MVC5-Anwendung (Mobil)** definiert * \_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Sie können eine (nicht-mobilen) Standardansicht vom Rendern in einem Layout für mobile Geräte global deaktivieren, indem `RequireConsistentDisplayMode` zu `true` in den *Ansichten\\\_ViewStart.cshtml* Datei wird wie folgt:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

Wenn `RequireConsistentDisplayMode` auf festgelegt ist `true`, das Layout für mobile Geräte (*\_Layout.Mobile.cshtml*) nur für mobile Ansichten verwendet wird (d. h., wenn die Ansichtsdatei des Formulars * **ViewName**ist. Mobile.cshtml*). Sie möchten möglicherweise festlegen `RequireConsistentDisplayMode` auf `true` Wenn Layout für mobile Geräte für nicht Mobile Ansichten gut funktioniert. Im folgenden Screenshot zeigt, wie die *Lautsprecher* Seite wann gerendert `RequireConsistentDisplayMode` auf festgelegt ist `true` (ohne die Zeichenfolge "(Mobil)" in der Navigationsleiste oben).

![][AllSpeakers_LayoutMobileOverridden]

Sie können in einer bestimmten Ansicht einheitliches-Modus deaktivieren, indem `RequireConsistentDisplayMode` zu `false` in der Datei anzeigen. Das folgende Markup in die *Ansichten\\Start\\AllSpeakers.cshtml* legt `RequireConsistentDisplayMode` auf `false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

In diesem Abschnitt haben wir gesehen, wie Sie mobile Layouts und Ansichten erstellen und zum Erstellen des Layouts und Ansichten für bestimmte Geräte wie dem iPhone.
Der wichtigste Vorteil von CSS Bootstrap Framework ist jedoch das reagiert Layout, was bedeutet, dass ein einzelnes Stylesheet kann, über den Desktop, Smartphone und Tablet-Browser angewendet werden, um ein einheitliches Aussehen und Verhalten zu erstellen. Im nächsten Abschnitt sehen Sie, wie Sie Bootstrap zum Erstellen von Mobile-Ansichten nutzen können.

##<a name="a-namebkmkimprovespeakerslista-improve-the-speakers-list"></a><a name="bkmk_Improvespeakerslist"></a>Verbessern der Lautsprecher-Liste

Haben Sie gerade gesehen, die *Lautsprecher* Ansicht lesbar ist, aber die Links sind klein und sind, erschweren auf einem mobilen Gerät tippen Sie auf. In diesem Abschnitt werden die Ansicht *AllSpeakers* Mobile geeignete vorgenommenen die großen, einfach tippen Links angezeigt und enthält kein Suchfeld, um den Lautsprechern Ihres Computers schnell zu finden.

Können die Bootstrap [verknüpfte Listengruppe][] formatieren zur Verbesserung der *Lautsprecher* -Ansicht. In *Ansichten\\Start\\AllSpeakers.cshtml*, ersetzen Sie den Inhalt der Razor-Datei durch den folgenden Code.

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

Die `class="list-group"` Attribut in der `<div>` Kategorie gilt die Sucherergebnisse Bootstrap Liste und die `class="input-group-item"` Attribut gilt Bootstrap Liste Element Sucherergebnisse für jede Verknüpfung.

Aktualisieren Sie den mobilen Browser. Aktualisierte Ansicht sieht wie folgt aus:

![][AllSpeakersFixed]

Die Sucherergebnisse Bootstrap [verknüpfte Listengruppe][] macht das gesamte Feld für jede Verknüpfung geklickt werden kann, also viel übersichtlicher zu gestalten. Wechseln Sie zu der Desktopansicht, und beobachten Sie das einheitliche Aussehen und Verhalten zu.

![][AllSpeakersFixedDesktop]

Obwohl die Ansicht für mobile Browser verbessert wurde, ist es schwierig, die lange Liste der Lautsprecher zu navigieren. Bootstrap einer Suche Filter Funktionalität Out-of-the-Box nicht zur Verfügung, jedoch können Sie es mit wenigen Codezeilen hinzufügen. Sie zuerst ein Suchfeld in der Ansicht hinzufügen und anschließend mit den JavaScript-Code für den Filter-Funktion einbinden. In *Ansichten\\Start\\AllSpeakers.cshtml*, Hinzufügen einer \<Formular\> kategorisieren nur nach der \<h2\> tag, wie unten dargestellt:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

Beachten Sie, dass die `<form>` und `<input>` Tags beide haben die Bootstrap Formatvorlagen angewendet werden. Die `<span>` Element im Suchfeld ein Bootstrap [Glyphicon][] hinzugefügt.

Fügen Sie eine JavaScript-Datei namens *filter.js*im Ordner *Skripts* hinzu. Öffnen Sie die Datei, und fügen Sie den folgenden Code ein:

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

Sie benötigen ferner filter.js in Ihrer registrierten bündeln aufnehmen möchten. Open *App\_starten\\BundleConfig.cs* , und ändern Sie die erste Pakete. Ändern der ersten `bundles.Add` -Anweisung (für das Paket **Jquery** ) aufnehmen möchten *Skripts\\filter.js*, wie folgt:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

**Jquery** -Paket wird standardmäßig der bereits gerendert * \_Layout* anzeigen. Später können Sie den gleichen JavaScript-Code zum Anwenden der Filters Funktionalität zu anderen Listenansichten nutzen.

Aktualisieren Sie den mobilen Browser, und wechseln Sie zur Ansicht *AllSpeakers* . Geben Sie "sc" in das Suchfeld ein. Die Liste der Lautsprecher sollte jetzt nach der zu durchsuchenden Zeichenfolge gefiltert werden.

![][AllSpeakersFixedSearchBySC]

##<a name="a-namebkmkimprovetagsa-improve-the-tags-list"></a><a name="bkmk_improvetags"></a>Verbessern der Liste Kategorien

Wie in der Ansicht *Lautsprecher* der Ansicht " *Kategorien* " lesbar ist, aber die Links sind kleine und schwierig, tippen Sie auf einem mobilen Gerät. Sie können die Ansicht *Kategorien* die gleiche Weise, wie Sie die Ansicht *Lautsprecher* beheben, wenn Sie den Code geändert, die zuvor beschriebenen verwenden, jedoch mit den folgenden beheben `Html.ActionLink` Methodensyntax in *Ansichten\\Start\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

Aktualisiert Desktopbrowser sieht wie folgt aus:

![][AllTagsFixedDesktop]

Und aktualisierte mobile Browser sieht wie folgt aus: 

![][AllTagsFixed]

>[AZURE.NOTE] Wenn Sie feststellen, dass die ursprüngliche Liste Formatierung weiterhin angezeigt, im mobilen Browser wird und fragen sich, was mit Ihrem schönen Bootstrap Sucherergebnisse passiert ist, ist dies eine Folge der früheren Aktion an mobile bestimmte Ansichten erstellen. Jedoch jetzt, da Sie zum Erstellen eines reagiert Web Bootstrap CSS Rahmen verwenden, wechseln Sie Kopf, und entfernen Sie diese Mobilität bezogene und das für Mobile-spezifische Layoutansichten. Nachdem Sie getan haben, wird aktualisierte mobile Browser den Bootstrap Sucherergebnisse angezeigt.

##<a name="a-namebkmkimprovedatesa-improve-the-dates-list"></a><a name="bkmk_improvedates"></a>Verbessern der Liste von Datumsangaben

Können Sie die Ansicht *Datumsangaben* verbessern, wie Sie die *Lautsprecher* und *Tags* Ansichten verbessert, wenn Sie den Code geändert, die zuvor beschriebenen verwenden, jedoch mit den folgenden `Html.ActionLink` Methodensyntax in *Ansichten\\Start\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

Sie erhalten eine Browseransicht aktualisiert mobile wie folgt aus:

![][AllDatesFixed]

Sie können weiteren die Ansicht *Datumsangaben* verbessern, indem Organisieren von der Datum-/ Uhrzeit-Werten nach Datum. Dies kann mit den Bootstrap [Bereiche][] Sucherergebnisse erfolgen. Ersetzen Sie den Inhalt der *Ansichten\\Start\\AllDates.cshtml* Datei mit den folgenden Code:

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

Dieser Code erstellt eine Separate `<div class="panel panel-primary">` für jedes distinct Datum in der Liste Kategorie und wird wie zuvor die [verknüpfte Listengruppe][] für die jeweiligen Links verwendet. So sieht wie im mobile Browser wie wenn dieser Code ausgeführt wird:

![][AllDatesFixed2]

Wechseln Sie zu der Desktopbrowser. Beachten Sie erneut, die einheitliche Darstellung zu erzielen.

![][AllDatesFixed2Desktop]

##<a name="a-namebkmkimprovesessionstablea-improve-the-sessionstable-view"></a><a name="bkmk_improvesessionstable"></a>Verbessern der SessionsTable-Ansicht

In diesem Abschnitt werden Sie die Ansicht *SessionsTable* mehr Mobile geeignete vornehmen. Diese Änderung ist umfassender vorherigen Änderungen.

Im mobilen Browser, tippen Sie auf die Schaltfläche " **Kategorie** ", und geben Sie dann `asp` in das Suchfeld.

![][AllTagsFixedSearchByASP]

Tippen Sie auf den Link **ASP.NET** .

![][SessionsTableTagASP.NET]

Wie Sie sehen können, wird der anzeigen als Tabelle, formatiert, die aktuell für die Anzeige in der Desktopbrowser ausgelegt ist. Es ist jedoch etwas schwer zu lesen, klicken Sie auf einem mobilen Browser. Öffnen Sie zum Beheben dieses Problems *Ansichten\\Start\\SessionsTable.cshtml* und Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

Der Code führt 3 Punkte:

-   der Bootstrap [benutzerdefinierten verknüpfte Listengruppe][] verwendet, um die Sitzungsinformationen vertikal, formatieren, damit alle diese Informationen auf einem mobilen Browser (mit Klassen, z. B. Liste-Gruppe-Element-Text) lesbar ist
-   Das Layout, gilt damit die Sitzungselemente horizontal Desktopbrowser und vertikal im mobilen Browser (mit der SP-Md-4-Klasse) Datenfluss im [Rastersystem][]
-   verwendet die [reagiert Dienstprogramme][] zum Ausblenden der Sitzung Kategorien Anzeigeergebnis im mobilen Browser (mit der ausgeblendeten x-Klasse)

Tippen Sie auf einen Titellink, um zu der entsprechenden Sitzung zu wechseln. Im Bild unten spiegeln den Code geändert.

![][FixedSessionsByTag]

Das Bootstrap-Rastersystem, das Sie automatisch angewendet werden die Sitzungen vertikal im mobilen Browser angeordnet. Beachten Sie, dass die Kategorien nicht angezeigt werden. Wechseln Sie zu der Desktopbrowser.

![][SessionsTableFixedTagASP.NETDesktop]

Beachten Sie in der Desktopbrowser, dass die Tags jetzt angezeigt werden. Darüber hinaus können Sie sehen, dass das Bootstrap Rastersystem, die, das Sie angewendet haben, die Sitzungselemente in zwei Spalten angeordnet. Wenn Sie im Browser zu vergrößern, sehen Sie sich, dass die Anordnung in drei Spalten verwandelt hat.

##<a name="a-namebkmkimprovesessionbycodea-improve-the-sessionbycode-view"></a><a name="bkmk_improvesessionbycode"></a>Verbessern der SessionByCode-Ansicht

Schließlich erhalten Sie die Ansicht *SessionByCode* ausreicht Mobile geeignete beheben.

Im mobilen Browser, tippen Sie auf die Schaltfläche " **Kategorie** ", und geben Sie dann `asp` in das Suchfeld.

![][AllTagsFixedSearchByASP]

Tippen Sie auf den Link **ASP.NET** . Sitzungen für das ASP.NET Tag werden angezeigt.

![][FixedSessionsByTag]

Wählen Sie den Link zum **Erstellen einer einzelnen Seite Anwendung mit ASP.NET und AngularJS** aus.

![][SessionByCode3-644]

Die desktop Standardansicht ist in Ordnung, aber Sie können das Aussehen einfach verbessern, indem Sie einige Komponenten des Bootstrap-Benutzeroberfläche.

Open *Ansichten\\Start\\SessionByCode.cshtml* und Ersetzen Sie den Inhalt durch das folgende Markup:

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

Das neue Markup verwendet Bootstrap Bereiche zur Verbesserung der mobile Ansicht formatieren. 

Aktualisieren Sie den mobilen Browser. Die folgende Abbildung gibt wieder den Code geändert, die Sie soeben erstellt haben:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Umbrechen von und überprüfen

In diesem Lernprogramm wurde, wie ASP.NET MVC 5 zum Entwickeln von Webanwendungen Mobile geeignete verwendet wird. Hierzu gehören:

-   Bereitstellen einer ASP.NET MVC 5-Anwendung auf einer App-Dienst Web app
-   Verwenden von Bootstrap reagiert Weblayout in Ihrer Anwendung MVC 5 erstellen
-   Überschreiben Sie Layout, Ansichten und teilweise Ansichten, global und für eine einzelne Ansicht
-   Steuerungslayout und teilweise überschreiben Durchsetzung mithilfe der `RequireConsistentDisplayMode` Eigenschaft
-   Erstellen von Ansichten, die bestimmte Browser, wie etwa iPhone-Browser als Ziel
-   Anwenden von Bootstrap Sucherergebnisse in Razor-code

## <a name="see-also"></a>Siehe auch

-   [9 Grundprinzipien reagiert Webdesign](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
-   [Bootstrap][BootstrapSite]
-   [Offizielle Bootstrap-Blog][]
-   [Bootstrap Twitter-Lernprogramm aus Lernprogramm Republik][]
-   [Der Bootstrap-Umgebung][]
-   [Bewährte Methoden für W3C Empfehlungen Mobile Web-Anwendung][]
-   [W3C Candidate Empfehlungen für Medienabfragen][]

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[Verknüpfte Listengruppe]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[Bereiche]: http://getbootstrap.com/components/#panels
[Benutzerdefinierte verknüpfte Listengruppe]: http://getbootstrap.com/components/#list-group-custom-content
[Rastersystem]: http://getbootstrap.com/css/#grid
[reagiert Dienstprogramme]: http://getbootstrap.com/css/#responsive-utilities
[Offizielle Bootstrap-Blog]: http://blog.getbootstrap.com/
[Bootstrap Twitter-Lernprogramm aus Lernprogramm Republik]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[Der Bootstrap-Umgebung]: http://www.bootply.com/
[Bewährte Methoden für W3C Empfehlungen Mobile Web-Anwendung]: http://www.w3.org/TR/mwabp/
[W3C Candidate Empfehlungen für Medienabfragen]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png
 
