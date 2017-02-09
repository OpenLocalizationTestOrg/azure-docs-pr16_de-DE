
<properties
    pageTitle="Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS Anwendungen | Microsoft Azure"
    description="So verwenden Sie Gruppen in Azure Active Directory Premium oder Basic Access SaaS Applikationen zuweisen, die mit Azure Active Directory integriert sind."
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
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Verwalten des Zugriffs auf SaaS Applikationen mithilfe einer Gruppe

Mit einer Lizenz Azure AD-Premium- oder Azure AD grundlegende Azure Active Directory (Azure AD) verwendet, können Sie Gruppen verwenden, zum Zuweisen des Zugriffs auf eine SaaS-Anwendung, die mit Azure AD integriert ist. Beispielsweise wenn Zuweisen des Zugriffs für der marketing-Abteilung fünf verschiedene SaaS Applikationen verwendet werden soll, können Sie erstellen Sie eine Gruppe, die Benutzer in der marketing-Abteilung enthält, und klicken Sie dann dieser Gruppe zuweisen folgende fünf SaaS-Programme, die von der marketing-Abteilung benötigt werden. Auf diese Weise können Sie Zeit sparen, indem Sie die Mitgliedschaft in der marketing-Abteilung an einem Ort verwalten. Benutzer werden dann mit der Anwendung zugewiesen, wenn er als Mitglieder der Gruppe "marketing" hinzugefügt werden, und haben ihre Aufgaben aus der Anwendung entfernt, wenn sie aus der Gruppe "marketing" entfernt werden.

Diese Funktion kann mit mehreren hundert Applikationen verwendet werden, die Sie in der Galerie Azure AD aus hinzufügen können.

**Zuweisen einer Anwendung SaaS Access für eine Gruppe**

1. Wählen Sie in der [Azure klassischen Portal](https://manage.windowsazure.com) **Active Directory** auf der Navigationsleiste auf der linken Seite aus.

2. Wählen Sie die Registerkarte **Verzeichnis** aus, und öffnen Sie das Verzeichnis, in dem Sie eine Anwendung SaaS Zugriff für eine Gruppe zuweisen möchten.

3. Wählen Sie die Registerkarte **Applications** . Wählen Sie eine Anwendung, die Sie aus dem Katalog Anwendung hinzugefügt haben, und klicken Sie dann auf der Registerkarte **Benutzer und Gruppen** .

4. Klicken Sie auf der Registerkarte **Benutzer und Gruppen** im Feld **Felder starten mit** Geben Sie den Namen der Gruppe, die Sie Zugriff zuweisen möchten, und wählen Sie dann das Kontrollkästchen in der oberen rechten Ecke. Sie müssen nur den ersten Teil der Name der Gruppe eingeben.

5. Wählen Sie die Gruppe aus, und wählen Sie dann auf die Schaltfläche **Zugriff zuweisen** . Wählen Sie **Ja** , wenn der bestätigungsmeldung angezeigt wird. Geschachtelte Gruppenmitgliedschaften werden für die Zuordnung Gruppe basierend auf Anwendungen zu diesem Zeitpunkt nicht unterstützt.

6. Sie können auch anzeigen, welche Benutzer mit der Anwendung, direkt oder durch die Mitgliedschaft in einer Gruppe zugewiesen sind. Ändern Sie hierzu für **' alle Benutzer '**im **Dropdownmenü aus 'Gruppen' anzeigen** . Die Liste zeigt Benutzer im Verzeichnis und ob jeder Benutzer mit der Anwendung zugeordnet ist. Die Liste enthält auch an, ob die zugeordneten Benutzer mit der Anwendung, direkt (Zuordnungs-Typ als "Direkt" angezeigt), oder als Mitglied der Gruppe (Zuordnungs-Typ als "Vererbt." angezeigt) zugewiesen sind


> [AZURE.NOTE]
>Sie können die Registerkarte Benutzer und Gruppen anzeigen, nur, nachdem Sie Azure AD Premium oder Azure AD grundlegende aktiviert haben.

##<a name="related-articles"></a>Verwandte Artikel

Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)

* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

* [Azure Active Directory-Cmdlets für die Konfiguration von gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
