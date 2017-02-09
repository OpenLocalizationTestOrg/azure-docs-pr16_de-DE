<properties 
    pageTitle="Zum Anpassen der Azure-API Management-Entwicklerportal mithilfe von Vorlagen | Microsoft Azure" 
    description="Informationen Sie zum Anpassen der Azure-API Management-Entwicklerportal mithilfe von Vorlagen." 
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
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>So passen Sie die Verwendung von Vorlagen Azure-API Management-Entwicklerportal

Azure-API Management bietet verschiedene Anpassungsfeatures ermöglichen es Administratoren, [passen Sie das Aussehen und Verhalten des Portals Entwicklertools](api-management-customize-portal.md)sowie Anpassen des Inhalts der Developer Portal Seiten mit einem Satz von Vorlagen, die den Inhalt der Seiten selbst konfigurieren. [DotLiquid](http://dotliquidmarkup.org/) Syntax und einer bereitgestellten Satz von lokalisierten Zeichenfolgenressourcen, Symbole und Seitensteuerelemente verwenden, müssen Sie den Inhalt der Seiten konfigurieren, je nach Bedarf mithilfe von Vorlagen flexibel.

## <a name="developer-portal-templates-overview"></a>Developer Portal Vorlagen (Übersicht)

Developer Portal Vorlagen werden in der Entwicklerportal von Administratoren der API Management Services-Instanz verwaltet. Zum Verwalten von Entwicklertools Vorlagen navigieren Sie zu Ihrer API Management Service-Instanz im klassischen Azure-Portal, und klicken Sie auf **Durchsuchen**.

![-Entwicklerportal][api-management-browse]

Wenn Sie bereits im Portal Publisher sind, können Sie die Entwicklerportal durch Klicken auf **Entwicklerportal**zugreifen.

![Developer Portal-Menü][api-management-developer-portal-menu]

Um Developer Portal Vorlagen zuzugreifen, klicken Sie auf das Symbol anpassen auf der linken Seite, um das Menü Anpassung anzuzeigen, und klicken Sie auf **Vorlagen**.

![Developer Portal Vorlagen][api-management-customize-menu]

Die Liste der Vorlagen zeigt mehrere Vorlagenkategorien darüber liegendem die einzelnen Seiten im Entwicklerportal. Jede Vorlage besteht aus verschiedenen, aber die Schritte zum Bearbeiten und veröffentlichen die Änderungen sind gleich. Klicken Sie auf den Namen der Vorlage, um eine Vorlage zu bearbeiten.

![Developer Portal Vorlagen][api-management-templates-menu]

Durch Klicken auf eine Vorlage, gelangen Sie an die Portalseite Entwickler, die diese Vorlage angepasst werden kann. In diesem Beispiel wird der **Produktliste** wird die Vorlage angezeigt. Die Vorlage **Produktliste** steuert den Bereich des Bildschirms das rote Rechteck erkennbar. 

![Produkte Listenvorlage][api-management-developer-portal-templates-overview]

Einige Vorlagen, wie die Vorlagen **Benutzerprofil** passen Sie verschiedene Teile derselben Seite. 

![Profilvorlagen][api-management-user-profile-templates]

Der Editor für jede Entwicklertools Vorlage Veröffentlichungsportal besteht aus zwei Abschnitten am unteren Rand der Seite angezeigt. Der linken Bildschirmseite zeigt Bearbeitungsbereichs für die Vorlage, und die rechte Seite zeigt das Datenmodell für die Vorlage. 

Die Vorlage bearbeiten Bereich enthält das Markup, das das Aussehen und Verhalten der entsprechenden Seite in der Entwicklerportal gesteuert. Das Markup in der Vorlage verwendet die Syntax der [DotLiquid](http://dotliquidmarkup.org/) . Eine beliebte-Editor für DotLiquid ist [DotLiquid für Designer](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Alle Änderungen vorgenommen während der Bearbeitung der Vorlage in Echtzeit angezeigt werden im Browser, aber sind nicht sichtbar an Ihre Kunden, bis Sie [Speichern](#to-save-a-template) und [Veröffentlichen](#to-publish-a-template) Sie die Vorlage.

![Vorlagenmarkup][api-management-template]

Im Datenbereich der **Vorlage** bietet einen Leitfaden zum Datenmodell für die Personen, für die Verwendung in einer bestimmten Vorlage verfügbar sind. Dieses Handbuch enthält es informiert die live-Daten, die derzeit in der Entwicklerportal angezeigt werden. Sie können die Bereiche der Vorlage durch Klicken auf das Rechteck in der oberen rechten Ecke der **Vorlage** Datenfenster erweitern.

![Vorlage-Datenmodell][api-management-template-data]

Es gibt zwei Produkte in die Entwicklerportal angezeigt, die aus der **Vorlage** Datenbereich angezeigten Daten abgerufen wurden, wie im folgenden Beispiel dargestellt, im vorherigen Beispiel.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Das Markup in der **Produktliste** Vorlage verarbeitet die Daten, um die gewünschte Ausgabe durch das Durchlaufen der Auflistung des anzuzeigenden Informationen und einen Link zu jedem einzelnen Produkt Produkte bieten. Hinweis Die `<search-control>` und `<page-control>` Elemente im Markup. Diese steuern die Anzeige von der Suche und das seitenweise Steuerelemente auf der Seite. `ProductsStrings|PageTitleProducts`ist ein lokalisierte Zeichenfolgenverweis, enthält die `h2` Kopfzeilentext für die Seite. Eine Liste der Zeichenfolge Ressourcen, Seitensteuerelemente und für die verfügbaren Symbole in Developer Portal Vorlagen verwenden, finden Sie unter [API Management Portal Vorlagen Entwicklerreferenz](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Speichern eine Vorlage

Um eine Vorlage zu speichern, klicken Sie in der Vorlagen-Editor auf Speichern.

![Speichern der Vorlage][api-management-save-template]

Gespeicherte Änderungen sind nicht in der Entwicklerportal live, bis sie veröffentlicht werden.

## <a name="to-publish-a-template"></a>Veröffentlichen eine Vorlage

Gespeicherte Vorlagen können entweder einzeln oder überhaupt veröffentlicht werden. Um eine einzelne Vorlage zu veröffentlichen, klicken Sie auf Veröffentlichen in der Vorlagen-Editor.

![Veröffentlichen der Vorlage][api-management-publish-template]

Klicken Sie auf **Ja,** um zu bestätigen, und stellen Sie die Vorlage auf die Entwicklerportal live.

![Bestätigen Sie veröffentlichen][api-management-publish-template-confirm]

Um alle derzeit nicht veröffentlichte Vorlagenversionen zu veröffentlichen, klicken Sie in der Vorlagenliste auf **Veröffentlichen** . Nicht veröffentlichte Vorlagen werden durch ein Sternchen nach dem Vorlagennamen angegeben. In diesem Beispiel werden die Vorlagen **Produktliste** und **Produkt** gerade veröffentlicht.

![Veröffentlichen von Vorlagen][api-management-publish-templates]

Klicken Sie auf **Veröffentlichen Anpassungen** zu bestätigen.

![Bestätigen Sie veröffentlichen][api-management-publish-customizations]

Neu veröffentlichte Vorlagen finden Sie in der Entwicklerportal sofort wirksam.

## <a name="to-revert-a-template-to-the-previous-version"></a>Wiederherstellen eine Vorlage auf die vorherige version

Wenn Sie eine Vorlage zum Anzeigen der veröffentlichten Vorgängerversion wiederherstellen möchten, klicken Sie auf Wiederherstellen in der Vorlagen-Editor.

![Wiederherstellen der Vorlage][api-management-revert-template]

Klicken Sie auf **Ja,** um zu bestätigen.

![Bestätigen][api-management-revert-template-confirm]

Nach Abschluss der während des Wiederherstellungsvorgangs, ist die bereits veröffentlichte Version einer Vorlage in der Entwicklerportal live.

## <a name="to-restore-a-template-to-the-default-version"></a>Wiederherstellen eine Vorlage auf die Standardversion zurück

Wiederherstellen von Vorlagen auf die Standardversion umfasst zwei Schritte. Zuerst die Vorlagen wiederhergestellt werden müssen, und klicken Sie dann die wiederhergestellten Versionen veröffentlicht werden müssen.

Zum Wiederherstellen eine einzelne Vorlage auf die Standardversion klicken Sie auf Wiederherstellen, in der Vorlagen-Editor.

![Wiederherstellen der Vorlage][api-management-reset-template]

Klicken Sie auf **Ja,** um zu bestätigen.

![Bestätigen][api-management-reset-template-confirm]

Um alle Vorlagen, die ihre Standardversionen wiederherzustellen, klicken Sie in der Vorlagenliste auf **Standardvorlagen wiederherstellen** .

![Wiederherstellen von Vorlagen][api-management-restore-templates]

Wiederhergestellten Vorlagen müssen dann einzeln oder alle gleichzeitig veröffentlicht werden, gemäß die Anweisungen in [einer Vorlage veröffentlicht](#to-publish-a-template)wird.

## <a name="developer-portal-templates-reference"></a>Portal Vorlagen Entwicklerreferenz

Referenzinformationen für Entwickler Portal Vorlagen, Zeichenfolgenressourcen, Symbole und Seitensteuerelemente finden Sie unter [API Management Portal Vorlagen Entwicklerreferenz](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Schauen Sie sich einen Überblick video

Schauen Sie sich das folgende Video zum Hinzufügen einer Diskussionsrunde und Bewertungen auf die Seiten-API und der Vorgang in der Entwicklerportal mithilfe von Vorlagen finden Sie unter.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







