
<properties
    pageTitle="Nächste Schritte für Access Management Entwurfsphase | Microsoft Azure"
    description="Erweiterte wie – für die Verwaltung von Sicherheitsgruppen und wie Sie diesen Gruppen zum Verwalten des Zugriffs auf eine Ressource."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="managing-owners-for-a-group"></a>Verwalten von Besitzer für eine Gruppe
Sobald die Ressourcenbesitzer einer Zugriff auf eine Ressource zu einer Gruppe Azure AD-zugewiesen, wird die Mitgliedschaft in der Gruppe durch den Besitzer der Gruppe verwaltet. Der Ressourcenbesitzer delegiert effektiv die Berechtigung zum Zuweisen von Benutzern, die der Ressource an den Besitzer der Gruppe.

## <a name="assigning-group-ownership"></a>Zuweisen der Besitzrechte einer Gruppe

**Hinzufügen ein Besitzers zu einer Gruppe**

1. Im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory aus**, und öffnen Sie dann auf das Firmenverzeichnis.

2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie die Gruppe aus, der Sie Besitzer zum hinzufügen möchten.

3. Wählen Sie **Besitzer hinzufügen**.

4. Wählen Sie den Benutzer, den Sie als Besitzer dieser Gruppe hinzufügen, und stellen Sie sicher, dass dieser Name hinzugefügt wird, in den Bereich **ausgewählte** möchten, klicken Sie auf der Seite **Besitzer hinzufügen** .


**So entfernen Sie einen Besitzer aus einer Gruppe**

1. Im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory aus**, und öffnen Sie dann auf das Firmenverzeichnis.

2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie die Gruppe aus, der Sie einen Besitzer aus entfernen möchten.

4. Wählen Sie die Registerkarte **Besitzer** aus.

5. Wählen Sie den Besitzer, den Sie aus dieser Gruppe entfernen möchten, und wählen Sie dann auf **Entfernen**.

## <a name="additional-information"></a>Weitere Informationen

Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)
* [Azure Active Directory-Cmdlets für die Konfiguration von gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
