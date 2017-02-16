<properties 
    pageTitle="So erstellen und Veröffentlichen eines Produkts in Azure-API Management" 
    description="Informationen Sie zum Erstellen und Veröffentlichen von Produkten in Azure-API Management." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>So erstellen und Veröffentlichen eines Produkts in Azure-API Management

In Azure-API Verwaltung enthält ein Produkt einen oder mehrere APIs als auch eine einsatzkontingent und den Nutzungsbedingungen. Nachdem ein Produkt veröffentlicht wird, können Entwickler mit dem Produkt abonnieren und beginnen mit der Produkt APIs. Das Thema enthält eine Anleitung zum Erstellen eines Produkts, Hinzufügen einer API und ihn für Entwickler veröffentlichen.

## <a name="create-product"> </a>Erstellen eines Produkts

Vorgänge werden hinzugefügt und so konfiguriert, dass eine API im Portal Publisher. Wenn Sie das Publisher-Portal zugreifen zu können, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie auf **Produkte** , klicken Sie im Menü auf der linken Seite, um die Seite **Produkte** anzuzeigen, und klicken Sie auf **Produkt hinzufügen**.

![Produkte][api-management-products]

![Neues Produkt][api-management-add-new-product]

Geben Sie einen beschreibenden Namen für das Produkt in den Feldern **Name** und eine Beschreibung des Produkts in das Feld **Beschreibung** ein.

Produkte in API Management kann **geöffnet** oder **geschützten**. Geschützte Produkte müssen abonniert werden, bevor sie, während Sie geöffnet verwendet werden können Produkte ohne ein Abonnement verwendet werden können. Aktivieren Sie das **Abonnement benötigen** , zum Erstellen eines geschützten Produkts, das ein Abonnement erforderlich sind. Dies ist die Standardeinstellung.

Aktivieren Sie das **Abonnement Genehmigung erforderlich** auf Wunsch ein Administrator zu überprüfen und annehmen oder ablehnen Abonnement Versuche, dieses Produkt. Wenn das Kontrollkästchen deaktiviert ist, werden Abonnements Versuche automatisch genehmigt. Weitere Informationen zu Abonnements finden Sie unter [Ansicht Abonnenten zu einem Produkt][].

Aktivieren Sie das Kontrollkästchen **mehrere Abonnements zulassen** Entwicklertools Konten mehrmals mit dem Produkt zu abonnieren um zu ermöglichen. Wenn dieses Kontrollkästchen nicht aktiviert ist, kann jeder Entwicklerkonto nur einmal mit dem Produkt abonnieren.

![Unbegrenzte mehrere Abonnements][api-management-unlimited-multiple-subscriptions]

Um die Anzahl der mehrere Gleichzeitiges Abonnements zu beschränken, aktivieren Sie das Kontrollkästchen **Anzahl der Gleichzeitiges Abonnements zu beschränken** , und geben Sie die zulässige Anzahl. Im folgenden Beispiel sind Gleichzeitiges Abonnements auf vier pro Entwicklertools Konto beschränkt.

![Vier mehrere Abonnements][api-management-four-multiple-subscriptions]

Nachdem Sie alle Produktoptionen für neues konfiguriert haben, klicken Sie auf **Speichern** , um das neue Produkt zu erstellen.

![Produkte][api-management-products-page]

>Standardmäßig neue Produkte sind nicht veröffentlichte und zur Gruppe **Administratoren** nur sichtbar sind.

Um ein Produkt konfigurieren möchten, klicken Sie auf der Produktname in der Registerkarte **Produkte** .

## <a name="add-apis"> </a>APIs zu einem Produkt hinzufügen

Die Seite **Produkte** enthält vier Links für Konfiguration: **Zusammenfassung**, **Einstellungen**, **Sichtbarkeit**und **Abonnenten**. Die Registerkarte **Zusammenfassung** ist, können Sie hinzufügen APIs und veröffentlichen oder Aufheben der Veröffentlichung eines Produkts.

![Zusammenfassung][api-management-new-product-summary]

Bevor Sie das Produkt veröffentlichen müssen Sie einen oder mehrere APIs hinzufügen. Klicken Sie hierzu auf **API mit Produkt hinzufügen**.

![Hinzufügen von APIs][api-management-add-apis-to-product]

Wählen Sie die gewünschten APIs aus, und klicken Sie auf **Speichern**.

## <a name="add-description"> </a>Beschreibende Informationen zu einem Produkt hinzufügen

Die Registerkarte **Einstellungen** können Sie detaillierte Informationen über das Produkt wie ihren Zweck, die darüber Zugriff auf APIs und andere nützliche Informationen bereitstellen. Der Inhalt ist der Entwickler ausgelegt, die wird die API aufrufen werden in nur-Text- oder HTML-Markup geschrieben werden können.

![Produkt-Einstellungen][api-management-product-settings]

Aktivieren Sie zum Erstellen eines geschützten Produkts, das ein Abonnement zu verwendenden erfordert **Abonnement benötigen** , oder deaktivieren Sie das Kontrollkästchen, um ein Öffnen Produkt erstellen, ohne ein Abonnement aufgerufen werden können.

Wählen Sie das **Abonnement Genehmigung erforderlich** manuell alle Produkt Abonnement Anfragen genehmigen soll. Standardmäßig werden alle Produkt-Abonnements automatisch erteilt.

Entwicklertools Konten mehrmals mit dem Produkt zu abonnieren um zu ermöglichen, aktivieren Sie das Kontrollkästchen **mehrere Abonnements zulassen** , und geben Sie optional eine Beschränkung. Wenn dieses Kontrollkästchen nicht aktiviert ist, kann jeder Entwicklerkonto nur einmal mit dem Produkt abonnieren.

Füllen Sie optional im Feld **Nutzungsbedingungen** , beschreibt die nutzungsbestimmungen für das Produkt aus, welche Abonnenten akzeptieren müssen, um das Produkt zu verwenden.

## <a name="publish-product"> </a>Veröffentlichen ein Produkts

Bevor Sie die in einem Produkt-APIs aufgerufen werden können, muss das Produkt veröffentlicht werden. Klicken Sie auf der Registerkarte **Zusammenfassung** für das Produkt aus klicken Sie auf **Veröffentlichen**, und klicken Sie dann auf **Ja, veröffentlichen** , um zu bestätigen. Wenn Sie eine bereits veröffentlichte Produkt als privat einstufen, klicken Sie auf **Veröffentlichung aufheben**.

![Produkt veröffentlichen][api-management-publish-product]

## <a name="make-visible"> </a>Ein Produkt für Entwickler sichtbar zu machen

Die Registerkarte **Sichtbarkeit** können Sie auswählen, welche Rollen sind in der Lage ist, finden Sie unter dem Produkt auf die Entwicklerportal und das Produkt abonnieren.

![Produkt Sichtbarkeit][api-management-product-visiblity]

Zum Aktivieren oder Deaktivieren der Sichtbarkeit eines Produkts für die Entwickler in einer Gruppe, aktivieren Sie oder deaktivieren Sie das Kontrollkästchen neben der Gruppe, und klicken Sie dann auf **Speichern**.

>Weitere Informationen finden Sie unter [Erstellen und verwenden zum Verwalten von Entwicklertools Konten in Azure-API Management Gruppen][].

## <a name="view-subscribers"> </a>Abonnenten zu einem Produkt anzeigen

Die Registerkarte **Abonnenten** Listet die Entwickler, die mit dem Produkt abonniert haben. Die Details und Einstellungen für jeden Entwickler können, indem Sie auf den Namen des Entwicklers angezeigt werden. In diesem Beispiel haben keine Entwickler mit dem Produkt noch nicht abonniert.

![Entwickler][api-management-developer-list]

## <a name="next-steps"> </a>Nächste Schritte

Sobald die gewünschten APIs hinzugefügt werden, und das Produkt veröffentlicht, können Entwickler mit dem Produkt abonnieren und beginnt, die APIs aufrufen. Ein Lernprogramm, das diese Elemente sowie erweiterte Produktkonfiguration veranschaulicht, finden Sie unter [zum Erstellen und Konfigurieren von Einstellungen für erweiterte Produkt Azure-API Management][].

Weitere Informationen zum Arbeiten mit Produkten finden Sie im folgende Video.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Anzeigen von Abonnenten zu einem Produkt]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[So erstellen und Verwenden von Gruppen zum Verwalten von Entwicklertools Konten bei Azure-API Verwaltung]: api-management-howto-create-groups.md
[Zum Erstellen und Konfigurieren von Produkt Erweiterte Einstellungen in Azure-API Management]: api-management-howto-product-with-rules.md 