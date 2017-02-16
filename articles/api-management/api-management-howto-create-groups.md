<properties 
    pageTitle="So erstellen und Verwenden von Gruppen zum Verwalten von Entwicklertools Konten bei Azure-API Verwaltung" 
    description="Informationen Sie zum Verwenden von Gruppen in Azure-API Management Entwicklertools-Konten verwalten" 
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

# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>So erstellen und Verwenden von Gruppen zum Verwalten von Entwicklertools Konten bei Azure-API Verwaltung

API-Verwaltung werden Gruppen zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Produkte zuerst sichtbar zu Gruppen vorgenommen wurden, und klicken Sie dann Entwickler in diesen Gruppen anzeigen können, und abonnieren Sie Produkte, die die Gruppen zugeordnet sind. 

Projektmanagement-API weist die folgenden unveränderlichen Systemgruppen.

-   **Administratoren** - Abonnement Azure-Administratoren sind Mitglieder dieser Gruppe. Administratoren verwalten-API Management Service Instanzen, erstellen APIs, Vorgänge und Produkte, die von Entwicklern verwendet werden.
-   **Entwickler** - authentifizierten Entwicklerportal, die Benutzer in dieser Gruppe liegen. Entwickler sind die Kunden, die mit Ihrem APIs Applications erstellen. Entwickler werden erteilt Zugriff auf das Portal Entwicklertools und Erstellen von Applications, die die Vorgänge einer API aufrufen.
-   **Gäste** - Portal Benutzer nicht authentifizierte Entwicklertools, wie z. B. Interessenten des Besuchs der Entwicklerportal einer Instanz-API Management zu dieser Gruppe gehören. Sie können erteilt werden bestimmte schreibgeschützten Zugriff, wie etwa die Möglichkeit, APIs anzeigen, aber nicht aufrufen können.

Zusätzlich zu diesen Systemgruppen können Administratoren benutzerdefinierte Gruppen oder [nutzen Sie externe Gruppen in zugeordneten Azure Active Directory-Mandanten][]erstellen. Benutzerdefinierte und externen Gruppen können entlang Systemgruppen in Entwickler Sichtbarkeit und Zugriff zu API Products zugewiesen verwendet werden. Beispielsweise könnten Sie erstellen eine benutzerdefinierte Gruppe für Entwickler mit einer bestimmten Partnerorganisation verbunden ist, und den sie Zugriff auf die APIs aus einem Produkt, das nur die relevanten APIs enthält. Ein Benutzer kann mehr als einer Gruppe angehören.

Dieses Handbuch zeigt, wie Administratoren einer Instanz-API Management neue Gruppen hinzufügen und diese Produkte und Entwickler zuordnen können.

>[AZURE.NOTE] Sie können zusätzlich zum Erstellen und Verwalten von Gruppen in Publisher-Portal, erstellen und Verwalten Ihrer Gruppen unter Verwendung der Entität API Management REST-API [Gruppe](https://msdn.microsoft.com/library/azure/dn776329.aspx) .

## <a name="create-group"> </a>Erstellen einer Gruppe

Um eine neue Gruppe erstellen möchten, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Dadurch gelangen Sie Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Gruppen** , und klicken Sie dann auf **Gruppe hinzufügen**.

![Neue Gruppe hinzufügen][api-management-add-group]

Geben Sie einen eindeutigen Namen für die Gruppe und eine optionale Beschreibung ein, und klicken Sie auf **Speichern**.

![Neue Gruppe hinzufügen][api-management-add-group-window]

Die neue Gruppe wird auf der Registerkarte Gruppen angezeigt. Wenn Sie den **Namen** oder die **Beschreibung** der Gruppe bearbeiten möchten, klicken Sie auf den Namen der Gruppe in der Liste. Klicken Sie auf **Löschen**, um die Gruppe zu löschen.

![Gruppe hinzugefügt][api-management-new-group]

Nachdem Sie nun die Gruppe erstellt wurde, können sie Produkte und Entwickler zugeordnet werden.

## <a name="associate-group-product"> </a>Ein Produkt eine Gruppe zuordnen

Wenn ein Produkt eine Gruppe zuordnen möchten, klicken Sie im Menü " **API Management** " auf der linken Seite auf **Products** , und klicken Sie dann auf den Namen des gewünschten Produkts.

![Festlegen der Sichtbarkeit][api-management-add-group-to-product]

Wählen Sie die Registerkarte **Sichtbarkeit** hinzufügen und Entfernen von Gruppen und die aktuellen Gruppen für das Produkt anzuzeigen. Zum Hinzufügen oder Entfernen von Gruppen, aktivieren Sie oder deaktivieren Sie die Kontrollkästchen für die gewünschte Gruppe aus, und klicken Sie auf **Speichern**.

![Festlegen der Sichtbarkeit][api-management-add-group-to-product-visibility]

>[AZURE.NOTE] Informationen Sie, [wie Entwicklertools Konten mit Azure Active Directory in Azure-API Management autorisieren](api-management-howto-aad.md)Azure Active Directory-Gruppen hinzuzufügen.
>
>Klicken Sie zum Konfigurieren der Registerkarte **Sichtbarkeit** für ein Produkt Gruppen auf **Gruppen verwalten**.

Nachdem ein Produkt einer Gruppe zugeordnet ist, können Entwickler in dieser Gruppe anzeigen und mit dem Produkt abonnieren.

## <a name="associate-group-developer"> </a>Ordnen Sie Gruppen Entwickler

Um Gruppen Entwickler zuzuordnen, klicken Sie im Menü " **API Management** " auf der linken Seite auf **Benutzer** , und aktivieren Sie dann das Kontrollkästchen neben der Entwickler, die Sie einer Gruppe zuordnen möchten.

![Entwicklertools zur Gruppe hinzufügen][api-management-add-group-to-developer]

Nachdem Sie die gewünschten Entwickler aktiviert sind, klicken Sie auf die gewünschte Gruppe in der Dropdownliste **zur Gruppe hinzufügen** . Entwickler können mithilfe der Dropdownliste **aus Gruppe entfernen** aus Gruppen entfernt. 

![Entwickler][api-management-add-group-to-developer-saved]

Nachdem Sie die Zuordnung zwischen dem Entwickler und der Gruppe hinzugefügt haben, können Sie es auf der Registerkarte **Benutzer** anzeigen.


## <a name="next-steps"> </a>Nächste Schritte

-   Nachdem ein Entwickler zu einer Gruppe hinzugefügt wurde, können sie anzeigen und Abonnieren von Produkte mit dieser Gruppe verknüpften. Weitere Informationen finden Sie unter [wie erstellen und veröffentlichen ein Produkts in Azure-API Management][],
-   Sie können zusätzlich zum Erstellen und Verwalten von Gruppen in Publisher-Portal, erstellen und Verwalten Ihrer Gruppen unter Verwendung der Entität API Management REST-API [Gruppe](https://msdn.microsoft.com/library/azure/dn776329.aspx) .


[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[Wie erstellen und veröffentlichen ein Produkts in Azure-API Management]: api-management-howto-add-products.md

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Nutzen Sie externe Gruppen in zugeordneten Azure Active Directory-Mandanten]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
