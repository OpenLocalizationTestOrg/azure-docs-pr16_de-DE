<properties 
    pageTitle="Verwalten von Benutzerkonten in Azure-API Management wie | Microsoft Azure" 
    description="Informationen Sie zum Erstellen oder Einladen von Benutzern in Azure-API Management" 
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

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Zum Verwalten von Benutzerkonten in Azure-API Management

API-Verwaltung sind Entwickler die Benutzer die APIs, die Sie verfügbar machen Management-API verwenden. In diesem Handbuch werden, zum Erstellen und Laden Sie Entwickler die APIs und Produkte nutzen, dass Sie diese mit Ihrem API Management Instanz zur Verfügung stellen. Informationen zum Verwalten von Benutzerkonten programmgesteuert finden Sie unter der Dokumentation [Entität Benutzer](https://msdn.microsoft.com/library/azure/dn776330.aspx) im Bezug [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) .

## <a name="create-developer"> </a>Erstellen einer neuen Entwicklertools

Klicken Sie zum Erstellen einer neuen Entwicklertools **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst auf. Dadurch gelangen Sie Publisher-API Verwaltungsportal. Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

![Publisher-portal][api-management-management-console]

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Benutzer** , und klicken Sie dann auf **Benutzer hinzufügen**.

![Erstellen von Entwicklertools][api-management-create-developer]

Geben Sie die **E-Mail**, **Kennwort**und **Namen** für die neue Entwickler, und klicken Sie auf **Speichern**.

![Erstellen von Entwicklertools][api-management-add-new-user]

Standardmäßig neu erstellten Entwicklertools Konten **aktiv**sind, und der **Entwickler** Gruppe zugeordnet.

![Neue Entwicklertools][api-management-new-developer]

Entwicklertools-Konten, die in einem **aktiven** Zustand sind können verwendet werden, um alle APIs zuzugreifen, für die sie Abonnements verfügen. Wenn den neu erstellten Entwickler weitere Gruppen zuordnen möchten, finden Sie unter [Gruppen mit Entwickler zuzuordnen][].

## <a name="invite-developer"> </a>Einen Entwickler einladen

Wenn Sie einen Entwickler einladen möchten, klicken Sie im Menü " **API Management** " auf der linken Seite auf **Benutzer** , und klicken Sie dann auf **Benutzer einladen**.

![Einladen von Entwicklertools][api-management-invite-developer]

Geben Sie die Namen und e-Mail-Adresse des Entwicklers, und klicken Sie auf **einladen**.

![Einladen von Entwicklertools][api-management-invite-developer-window]

Es wird eine bestätigungsmeldung angezeigt, aber der Entwickler neu eingeladene wird nicht in der Liste bis angezeigt, nachdem sie die Einladung akzeptiert. 

![Laden Sie zur Bestätigung][api-management-invite-developer-confirmation]

Wenn ein Entwickler eingeladen wurde, wird eine e-Mail-Nachricht an den Entwickler gesendet. Diese e-Mail-Nachricht mithilfe einer Vorlage generiert und kann angepasst werden. Weitere Informationen finden Sie unter [Konfigurieren e-Mail-Vorlagen][].

Nachdem Sie die Einladung akzeptiert wird, wird das Konto aktiv.

## <a name="block-developer"></a> Deaktivieren oder Reaktivieren eines Kontos Entwicklertools

Standardmäßig sind neu erstellte oder eingeladene Entwicklertools Konten **aktiv**. Wenn Sie ein Konto Entwicklertools deaktiviert haben, klicken Sie auf **Blockieren**. Um ein Konto gesperrten Entwicklertools reaktivieren möchten, klicken Sie auf **Aktivieren**. Einem gesperrten Entwicklertools-Konto können Sie nicht Zugriff auf die Entwicklerportal oder APIs aufgerufen. Wenn Sie ein Benutzerkonto löschen möchten, klicken Sie auf **Löschen**.

![Blockieren Entwicklertools][api-management-new-developer]

## <a name="reset-a-user-password"></a>Zurücksetzen eines Benutzerkennworts

Klicken Sie auf den Namen der Firma, zum Zurücksetzen des Kennworts für ein Benutzerkonto.

![Zum Zurücksetzen von Kennwörtern][api-management-view-developer]

Klicken Sie auf **Kennwort zurücksetzen** , um einen Link zu dem Benutzer, dessen Kennwort zurücksetzen zu senden.

![Zum Zurücksetzen von Kennwörtern][api-management-reset-password]

Um programmgesteuert mit Benutzerkonten arbeiten, finden Sie unter der [Benutzer Entität](https://msdn.microsoft.com/library/azure/dn776330.aspx) Dokumentation im Bezug [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) . Zum Zurücksetzen des Kennworts für ein auf einen bestimmten Wert, können Sie verwenden Sie den Vorgang zum [Aktualisieren eines Benutzers](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) und geben das gewünschte Kennwort ein.

## <a name="pending-verification"></a>Ausstehende Überprüfung

![Ausstehende Überprüfung][api-management-pending-verification]

## <a name="next-steps"> </a>Nächste Schritte

Nachdem ein Entwicklerkonto erstellt wurde, können Sie Rollen zuordnen und Produkte und APIs abonnieren. Weitere Informationen finden Sie unter [Erstellen und verwenden Gruppen][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[So erstellen und Verwenden von Gruppen]: api-management-howto-create-groups.md
[So Entwickler Gruppen zuordnen]: api-management-howto-create-groups.md#associate-group-developer

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Konfigurieren von e-Mail-Vorlagen]: api-management-howto-configure-notifications.md#email-templates