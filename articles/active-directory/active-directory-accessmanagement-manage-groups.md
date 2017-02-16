<properties
    pageTitle="Verwalten von Gruppen in Azure Active Directory | Microsoft Azure"
    description="So erstellen und Verwalten von Gruppen zum Verwalten von Azure Benutzer, die mit Azure Active Directory."
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
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Verwalten von Gruppen in Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure klassischen-portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Eines der Features von Azure Active Directory (Azure AD) Benutzermanagement ist die Möglichkeit zum Erstellen von Gruppen von Benutzern. Sie verwenden eine Gruppe-Verwaltungsaufgaben wie etwa das Zuweisen von Lizenzen oder Berechtigungen zu einer Anzahl von Benutzern auf einmal ausführen. Gruppen können Sie auch über die Zugriffsberechtigung zum Zuweisen

- Ressourcen wie Objekte im Verzeichnis
- Ressourcen außerhalb Verzeichnis SaaS Applications, Azure Services, SharePoint-Websites oder lokalen Ressourcen

Darüber hinaus kann Ressourcenbesitzer einer auch Zugriff auf eine Ressource zu einer Person im Besitz Azure AD-Gruppe zuweisen. Diese Zuordnung wird die Mitglieder dieser Gruppe den Zugriff auf die Ressource erteilt. Klicken Sie dann verwaltet der Besitzer der Gruppe Mitgliedschaft in der Gruppe. Der Ressourcenbesitzer delegiert effektiv, an den Besitzer der Gruppe die Berechtigung zum Zuweisen von Benutzern, die die Ressource.

## <a name="how-do-i-create-a-group"></a>Wie erstelle ich eine Gruppe?

Je nach der Dienste, die Ihre Organisation abonniert hat, können Sie eine Gruppe mithilfe einer der folgenden erstellen:
- im klassischen Azure-portal
- Office 365-Portal-Konto
- das Windows Intune-Konto-portal

Wir werden Aufgaben beschrieben, wie in der klassischen Azure-Portal ausgeführt. Weitere Informationen zur Verwendung von nicht Azure Portals zu Ihrem Verzeichnis Azure AD-verwalten finden Sie unter [Verwalten von Ihrem Azure AD-Verzeichnis](active-directory-administer.md).

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory**, und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus.

3. Wählen Sie die **Gruppe hinzufügen**.

4. Geben Sie im Fenster **Gruppe hinzufügen** den Namen und die Beschreibung einer Gruppe ein.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Wie kann ich hinzufügen oder Entfernen einzelner Benutzer in einer Sicherheitsgruppe?

**Hinzufügen ein einzelnes Benutzers zu einer Gruppe**

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory**, und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus.

3. Öffnen Sie die Gruppe, der Sie Mitglieder hinzufügen möchten. Öffnen Sie die Registerkarte **Mitglieder** der ausgewählten Gruppe aus, sofern es noch nicht anzeigen.

4. Wählen Sie die **Mitglieder hinzufügen**aus.

5. Wählen Sie den Namen des Benutzers oder einer Gruppe, die Sie als Mitglieder dieser Gruppe hinzufügen möchten, klicken Sie auf der Seite **Mitglieder hinzufügen** . Stellen Sie sicher, dass dieser Name in den Bereich **ausgewählte** hinzugefügt wird.


**So entfernen Sie einen einzelnen Benutzer aus einer Gruppe**

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory**, und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus.

3. Öffnen Sie die Gruppe, aus der Sie Mitglieder entfernen möchten.

4. Wählen Sie die Registerkarte **Mitglieder** aus, wählen Sie den Namen des Elements, das Sie aus dieser Gruppe entfernen möchten, und klicken Sie dann auf **Entfernen**.

6. Bestätigen Sie bei der Aufforderung, dass Sie diese Mitglied aus der Gruppe entfernen möchten.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Wie kann ich die Zugehörigkeit zu einer Gruppe dynamisch verwalten?

In Azure AD können Sie einfach eine einfache Regel einrichten zu bestimmen, welche Benutzer sind Mitglied der Gruppe sein. Eine einfache Regel ist eine, das nur einen einfachen Vergleich herstellt. Beispielsweise, wenn eine Anwendung SaaS eine Gruppe zugewiesen ist, können Sie eine Regel einrichten zum Hinzufügen von Benutzern, die eine Position von "Vertriebsmitarbeiter" haben Mit dieser Regel gewährt dann Zugriff auf diese Anwendung SaaS für alle Benutzer mit dieser Position in Ihrem Verzeichnis.

Wenn alle Attribute eines Benutzers ändern, das System auswertet werden alle dynamischen Gruppe Regeln im Verzeichnis um festzustellen, ob das Attribut ändern des Benutzers eine Gruppe auslösen würde hinzugefügt oder entfernt. Wenn ein Benutzer eine Regel einer Gruppe entspricht, in dieser Gruppe als Mitglied hinzugefügt. Wenn sie die Regel einer Gruppe, die, denen Sie Mitglied sind nicht mehr erfüllen, aus dieser Gruppe als eine Mitglieder entfernt.

> [AZURE.NOTE] Sie können eine Regel für die dynamische Mitgliedschaft auf Sicherheitsgruppen oder Office 365-Gruppen einrichten. Geschachtelte Gruppenmitgliedschaften werden derzeit für Arbeitsgruppen-Zuordnung auf Anwendungen nicht unterstützt.
>
> Dynamische Mitgliedschaften für die Gruppen erfordern eine Lizenz Azure AD Premium zugewiesen werden soll
>
> - Der Administrator, der die Regel einer Gruppe verwaltet werden.
> - Alle Mitglieder der Gruppe

**So aktivieren Sie dynamische Mitgliedschaft für eine Gruppe**

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory**, und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus, und öffnen Sie die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren** , und klicken Sie dann auf **Ja**festgelegt **Dynamische Mitgliedschaften aktivieren** .

4. Richten Sie eine einfache einzelne Regel für die Gruppe zu steuern, wie dynamische Mitgliedschaft für diese Gruppenfunktionen aus. Stellen Sie sicher, dass die **Benutzer hinzufügen, in dem** Option ausgewählt ist, und wählen Sie dann eine Eigenschaft aus der Liste (beispielsweise Abteilung, Position usw.),

5. Wählen Sie dann eine Bedingung (nicht gleich ist gleich, nicht beginnt mit, beginnt mit, nicht enthält, enthält, keine Übereinstimmung, Übereinstimmung).

6. Geben Sie einen Vergleichswert für die Eigenschaft ausgewählte Benutzer ein.

Informationen zum Erstellen *Erweiterter* Regeln (Regeln, die mehrere Vergleiche enthalten kann) für dynamische Gruppenmitgliedschaft finden Sie unter [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Weitere Informationen

Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets für die Konfiguration von gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
