<properties
    pageTitle="Gruppen in Azure Active Directory dedizierter | Microsoft Azure"
    description="Übersicht über wie dedizierte Gruppen funktionieren in Azure Active Directory und wie sie erstellt werden."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Dedizierte Gruppen in Azure Active Directory

In Azure Active Directory (Azure AD) das Feature dedizierte Gruppen automatisch erstellt und füllt Mitgliedschaft für Azure AD-vordefinierter Gruppen. Mitglieder der dedizierte Gruppen kann nicht hinzugefügt oder entfernt werden mit dem Azure klassischen Portal, Windows PowerShell-Cmdlets oder programmgesteuert.

>[AZURE.NOTE] Dedizierte Gruppen erfordern, dass eine Lizenz Azure AD Premium zugewiesen ist
>- der Administrator, der die Regel einer Gruppe verwaltet werden.
>- alle Benutzer, die von der Regel ausgewählt werden, ein Mitglied der Gruppe sein.

**So aktivieren Sie dedizierte Gruppen**

1. Im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory aus**, und öffnen Sie dann auf das Firmenverzeichnis.

2. Wählen Sie die Registerkarte **Gruppen** aus, und klicken Sie dann öffnen Sie die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren** , und klicken Sie dann auf **Ja**festgelegt **Dedizierte Gruppen aktivieren** .

Nachdem Sie der Schalter dedizierte Gruppen aktivieren auf **Ja**festgelegt ist, können Sie weitere Verzeichnis automatisch alle Benutzer dedizierte Gruppe durch Festlegen erstellen Aktivieren der **Aktivieren "Alle Benutzer" Gruppe** wechseln auf **Ja**. Sie können auch bearbeiten den Namen dieser dedizierte Gruppe durch Eingabe der **Anzeigenamen für "Alle Benutzer" Gruppe** Feld.

Die Gruppe alle Benutzer kann verwendet werden, dieselben Berechtigungen für alle Benutzer in Ihrem Verzeichnis zuweisen. Beispielsweise können Sie alle Benutzer in Ihrem Verzeichniszugriff auf eine SaaS-Anwendung erteilen, durch das Zuweisen von Access für alle Benutzer dedizierten Gruppe zu dieser Anwendung.

Alle Benutzer dedizierte Gruppe umfasst alle Benutzer im Verzeichnis, einschließlich Gäste und externe Benutzer. Wenn Sie eine Gruppe benötigen, und schließen Sie externe Benutzer, die dann hierzu können Sie durch Erstellen einer Gruppe mit einem Attribut-basierte dynamische Regel beispielsweise die folgende:

                (user.userPrincipalName -notContains "#EXT#@")

Verwenden Sie für eine Gruppe, die alle Gäste ausschließt eine Regel wie folgt aus:

                (user.userType -ne "Guest")

Informationen zum Erstellen *Erweiterter* Regeln (Regeln, die mehrere Vergleiche enthalten kann) für dynamische Gruppenmitgliedschaft finden Sie unter [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md).


Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
