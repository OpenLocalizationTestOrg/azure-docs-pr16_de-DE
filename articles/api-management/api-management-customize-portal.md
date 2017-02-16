<properties
    pageTitle="Anpassen der Azure-API Management-Entwicklerportal | Microsoft Azure"
    description="Informationen Sie zum Anpassen der Azure-API Management-Entwicklerportal."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="customize-the-developer-portal-in-azure-api-management"></a>Anpassen der Azure-API Management-Entwicklerportal

In diesem Handbuch wird gezeigt, wie das Aussehen und Verhalten des Portals Entwickler in Azure-API Management für Konsistenz mit Ihrer Marke ändern.

## <a name="change-page-headers"> </a>Ändern Sie den Text oder das Logo auf der Seite

Eine der die wichtigsten Aspekte des Portals Anpassung wird den Text am oberen Rand aller Seiten mit Ihrem Firmennamen oder Ihr Logo ersetzt.

Inhalt innerhalb der Entwicklerportal geändert über das Publisher-Portal, das über das klassische Azure-Portal zugegriffen werden kann. Um die API Publisher-Portal erreicht haben, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

Die Entwicklerportal basiert auf einer Inhaltsverwaltungssystem oder CMS. Die Kopfzeile, die auf jeder Seite angezeigt wird, ist eine spezielle Art von Inhalten, die als ein Widget bezeichnet. Um den Inhalt dieser Widget bearbeiten zu können, klicken Sie im Menü **Entwicklerportal** auf der linken Seite auf **Widgets** , und wählen Sie dann das **Kopfzeile** Widget aus der Liste aus.

![Widgets Kopfzeile][api-management-widgets-header]

Des Inhalts der Kopfzeile wird in **das Nachrichtenfeld** bearbeitet werden. Ändern Sie den Text in "Fabrikam Entwicklerportal", und klicken Sie dann auf am unteren Rand der Seite **zu speichern** .

Nun sollten Sie die neue Kopfzeile auf jeder Seite innerhalb der Entwicklerportal sehen können.

> Klicken Sie zum Öffnen der Entwicklerportal während im Portal Publisher **-Entwicklerportal** in der oberen Leiste auf.

## <a name="change-headers-styling"> </a>Ändern Sie die Auswirkungen der Überschriften

Die Farben, Schriftarten, Größen, Spacings und andere Elemente Formatvorlagen verbundenen einer beliebigen Seite auf das Portal werden durch Formatierungsregeln definiert. Um die Formatvorlagen zu bearbeiten, klicken Sie im Menü **Entwicklerportal** im Publisher-Portal auf **Darstellung** und dann auf **Anpassung beginnen** , um die Sucherergebnisse-Editor aktiviert werden.

Ihr Browser wechselt auf eine ausgeblendete Seite innerhalb der Entwicklerportal, die enthält Beispiele von Inhalten, zusammen mit Beispielen für alle Sucherergebnisse Regeln, die an einer beliebigen Stelle auf der Website verwendet werden. Bewegen Sie den Cursor zum Öffnen des Editors Sucherergebnisse über der graue vertikalen Linie klicken Sie auf die linke Teil der Seite. Symbolleiste des-Editors sollte angezeigt werden.

![Anpassung-Symbolleiste][api-management-customization-toolbar]

Es gibt zwei Hauptfenster Modi Sucherergebnisse Regeln zu bearbeiten: Zeigt eine Liste aller verwendet eine beliebige Stelle, während **Element auswählen** , können Sie ein Element auf der Seite auswählen, die Sie auf Formatvorlage Regeln und Formatvorlagen nur für dieses Element zeigt **alle Regeln zu bearbeiten** .

In diesem Abschnitt, den wir die Auswirkungen der nur die Kopfzeilen ändern möchten. Klicken Sie auf die Option **Element wählen Sie aus** der Symbolleiste auf die Sucherergebnisse-Editor, und klicken Sie dann auf **Wählen Sie ein Element anpassen**. Elemente werden jetzt hervorgehoben, wie Sie sie mit der Maus zeigen auf welche des Elements Formatvorlagen kennzeichnen Sie beginnen möchten, bearbeiten, wenn Sie geklickt haben. Bewegen Sie den Mauszeiger über den Text, der Namen des Unternehmens in der Kopfzeile ("Fabrikam Entwicklerportal" Wenn Sie die Anweisungen im vorherigen Abschnitt befolgt) darstellt, und klicken Sie auf. Innerhalb des Editors Sucherergebnisse wird eine Reihe von benannten und kategorisierten Sucherergebnisse Regeln angezeigt.

Jede Regel stellt eine Eigenschaft Auswirkungen des ausgewählten Elements. Für den Kopfzeilentext oben ausgewählten, die Größe des Texts beträgt beispielsweise @font-size-h1 während der Name der Schriftart mit alternativen wird @headings-font-family.

> Wenn Sie mit [bootstrap][]vertraut sind, sind diese Regeln wirklich [weniger Variablen][] innerhalb der bootstrap Design von der Entwicklerportal verwendet.

Lassen Sie uns Ändern der Farbe von Text der Überschrift ein. Wählen Sie den Eintrag in der **@headings-color** aus, und geben Sie **#000000**. Dies ist der Hexadezimalcode für Schwarz. Wie Sie dies tun, sehen Sie, dass ein quadratisches Farbe Indikator am Ende des Textfelds angezeigt wird. Wenn Sie diesen Indikator geklickt haben, können Sie eine Farbauswahl um eine Farbe auszuwählen.

![Farbauswahl][api-management-customization-toolbar-color-picker]

Wenn Sie die Änderungen der Formatvorlagen des ausgewählten Elements fertig sind, klicken Sie auf **Vorschau der Änderungen** , um die Ergebnisse auf dem Bildschirm anzuzeigen. Zu diesem Zeitpunkt sind sie nur für Administratoren sichtbar. Damit diese Änderungen für alle Benutzer sichtbar ist, klicken Sie auf die Schaltfläche " **Veröffentlichen** " in den Sucherergebnisse-Editor, und bestätigen Sie die Änderungen vor.

![Menü ' veröffentlichen '][api-management-customization-toolbar-publish-form]

> Um die Formatierungsregeln zu ändern, die zu einem anderen Element auf der Seite anwenden, befolgen Sie dasselbe Verfahren, wie Sie für die Kopfzeile. Klicken Sie auf aus dem Editor Sucherergebnisse **Wählen Sie ein Element aus** , wählen Sie das Element, das, dem Sie interessiert, und beginnen Sie, ändern die Werte von der Formatierungsregeln auf dem Bildschirm angezeigt.

## <a name="edit-page-contents"> </a>Bearbeiten des Inhalts einer Seite

Die Entwicklerportal umfasst automatisch generierte Seiten wie APIs, Produkten, Applikationen, Probleme und manuell schriftliche Inhalte. Da er ein Content Managementsystem basiert, können Sie solche Inhalte nach Bedarf erstellen.

Wenn die Liste der alle vorhandenen Inhaltsseiten anzeigen möchten, klicken Sie auf **Inhalte** aus dem Menü **Entwicklerportal** im Portal Publisher.

![Verwalten von Inhalt][api-management-customization-manage-content]

Klicken Sie auf der Seite **Willkommen** zum Bearbeiten, was auf der Homepage der Entwicklerportal angezeigt wird. Nehmen Sie die Änderungen, die Sie möchten, als Vorschau bei Bedarf, die und klicken Sie dann auf **Jetzt veröffentlichen** , um sie für jeden sichtbar.

> Die Homepage verwendet eine spezielle Layout, mit dem sie ein Banner am oberen angezeigt werden kann. Diese Banner kann aus **Inhaltsabschnitt** nicht bearbeitet werden. Um diese Banner zu bearbeiten, klicken Sie im Menü **Entwicklerportal** **Widgets** auf, wählen Sie aus der Dropdownliste **Aktuelle Ebene** **Homepage** aus, und öffnen Sie das Element **Banner** unter dem **Abschnitt empfohlen**. Der Inhalt dieses Widget können ebenso wie eine andere Seite bearbeitet werden.

## <a name="next-steps"> </a>Nächste Schritte

-   Informationen Sie zum Anpassen des Inhalts der Entwicklertools Portal Seiten mithilfe von [Entwicklertools Portal Vorlagen](api-management-developer-portal-templates.md).

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[Bootstrap]: http://getbootstrap.com/
[WENIGER Variablen]: http://getbootstrap.com/css/
