<properties 
    pageTitle="Konfigurieren von Benachrichtigungen und e-Mail-Vorlagen in Azure-API Management" 
    description="Informationen Sie zum Konfigurieren von Benachrichtigungen und e-Mail-Vorlagen in Azure-API Management." 
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

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Konfigurieren von Benachrichtigungen und e-Mail-Vorlagen in Azure-API Management

Projektmanagement-API ermöglicht es so konfigurieren Sie Benachrichtigungen für bestimmte Ereignisse und so konfigurieren Sie die e-Mail-Vorlagen, die zur Kommunikation mit dem Administratoren und Entwickler einer Instanz-API Management verwendet werden. In diesem Thema veranschaulicht, wie Konfigurieren von Benachrichtigungen für die verfügbaren Ereignisse und bietet einen Überblick über die zum Konfigurieren der e-Mail-Vorlagen für diese Ereignisse verwendet.

## <a name="publisher-notifications"> </a>Publisher-Benachrichtigungen zu konfigurieren

Klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst, um Benachrichtigungen zu konfigurieren. Dadurch gelangen Sie Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie im Menü " **API Management** " auf der linken Seite die verfügbaren Benachrichtigungen anzeigen auf **Benachrichtigungen** .

![Publisher-Benachrichtigungen][api-management-publisher-notifications]

Die folgende Liste von Ereignissen kann für Benachrichtigungen konfiguriert werden.

-   **Abonnement Anfragen (Genehmigung anfordern)** – der angegebenen e-Mail-Empfänger und den Benutzer, e-Mail-Benachrichtigungen zu Abonnement Besprechungsanfragen für API-Produkte, die die Genehmigung erhalten.
-   **Neue Abonnements** – die angegebenen e-Mail-Empfänger und den Benutzer, e-Mail-Benachrichtigungen zu neuen API Produkt Abonnements erhalten.
-   **Anwendung Katalog Anfragen** - die angegebenen e-Mail-Empfänger und den Benutzer, e-Mail-Benachrichtigungen erhalten, wenn neue Applikationen an der Galerie übertragen werden.
-   **BCC** - die angegebenen e-Mail-Empfänger und den Benutzer, e-Mail-Blindkopien für alle Entwicklern gesendeten e-Mails erhalten.
-   **Neues Problem oder Kommentar** – die angegebenen e-Mail-Empfänger und den Benutzer, e-Mail-Benachrichtigungen, wenn ein neues Problem erhalten oder Kommentar auf die Entwicklerportal übermittelt wird.
-   **Schließen Konto Nachricht** - die angegebenen e-Mail-Empfänger und den Benutzer, e-Mail-Benachrichtigungen erhalten, wenn ein Konto geschlossen wurde.
-   **Approaching Abonnement Speicherkontingent für** – die folgenden e-Mail-Empfänger und den Benutzer, e-Mail-Benachrichtigungen erhalten, wenn Abonnementverwendung in der Nähe einsatzkontingent erhält.

Für jedes Ereignis können Sie e-Mail-Empfänger in das Textfeld e-Mail-Adresse angeben, oder Sie können Benutzer aus einer Liste auswählen.

Um die e-Mail-Adressen benachrichtigt werden angeben möchten, geben Sie diese in das Textfeld der e-Mail-Adresse ein. Wenn Sie mehrere e-Mail-Adressen verfügen, trennen Sie diese mit Kommas getrennt ein.

![Empfänger der Benachrichtigung][api-management-email-addresses]

Zum Angeben der Benutzer benachrichtigt werden, klicken Sie auf **Empfänger hinzuzufügen**, aktivieren Sie das Kontrollkästchen neben den Benutzern benachrichtigt werden, und klicken Sie auf **OK**.

>Beachten Sie, dass nur Administratoren in der Liste angezeigt werden.

Klicken Sie nachdem die Benachrichtigungsempfänger konfiguriert haben auf **Speichern** , um die aktualisierten Benachrichtigungsempfänger anzuwenden.

>Wenn Sie die Registerkarte **Benachrichtigungen Publisher** verlassen benachrichtigt Sie Publisher-Portal ist es Änderungen nicht gespeicherten sind.

## <a name="email-templates"> </a>Konfigurieren e-Mail-Vorlagen

Projektmanagement-API bietet e-Mail-Vorlagen für die e-Mail-Nachrichten, die im Verlauf verwalten, und verwenden den Dienst gesendet werden. Die folgenden e-Mail-Vorlagen bereitgestellt werden.

-   Anwendung Katalog Einreichung genehmigt
-   Entwicklertools Abschied Buchstaben
-   Entwicklertools Speicherkontingent für Annäherung an Benachrichtigung
-   Benutzer einladen
-   Zu einem Problem neu hinzugefügten Kommentar
-   Neues Problem erhalten
-   Neues Abonnement aktiviert
-   Abonnement erneuert Bestätigung
-   Abonnementsanfrage ablehnt
-   Abonnement Anforderung empfangen

Diese Vorlagen können geändert werden, wie gewünscht.

Um anzuzeigen, und konfigurieren Sie die e-Mail-Vorlagen für Ihre API Management-Instanz aus, klicken Sie im Menü " **API Management** " auf der linken Seite auf **Benachrichtigungen** , und wählen Sie die Registerkarte **E-Mail-Vorlagen** .

![E-Mail-Vorlagen][api-management-email-templates]

Zum Anzeigen oder Ändern einer bestimmten Vorlage, wählen Sie es in der Dropdown-Liste von **Vorlagen** aus.

![Liste der e-Mail-Vorlagen][api-management-email-templates-list]

Jede e-Mail-Vorlage enthält einen Betreff im nur-Text und eine Definition Text im HTML-Format. Jedes Element kann angepasst werden wie gewünscht.

![E-Mail-Vorlagen-editor][api-management-email-template]

Die Liste **Parameter** enthält eine Liste von Parametern, das bei in Betreff oder Text eingefügt wird, kann ich den festgelegten Wert ersetzt, wenn die e-Mail-Nachricht gesendet wird. Klicken Sie zum Einfügen eines Parameters, platzieren Sie den Cursor auf die Stelle, an der Sie den Parameter zu wechseln möchten, und klicken Sie auf den Pfeil nach links neben dem Namen des Parameters.

Klicken Sie auf **Vorschau** oder **einen Testanruf zu senden** , um anzuzeigen, wie die e-Mail Aussehen oder senden eine-e-Testmail.

>Beachten Sie, dass die Parameter nicht mit der tatsächlichen Werte, die bei der Vorschau, oder senden einen Testanruf ersetzt werden.
>
>Speichern der Änderungen auf die e-Mail-Vorlage, klicken Sie auf **Speichern**, oder Abbrechen klicken Sie auf die Änderungen **Abbrechen**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance