
<properties
    pageTitle="Problembehandlung bei dynamischen Mitgliedschaft für Gruppen | Microsoft Azure"
    description="Hinweise zur Problembehandlung für die dynamische Mitgliedschaft für Azure AD-Gruppen."
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


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problembehandlung bei dynamischen Mitgliedschaften für die Gruppen

**Ich habe eine Regel auf eine Gruppe konfiguriert, aber keine Mitgliedschaften erhalten Sie in der Gruppe aktualisiert**<br/>Stellen Sie sicher, dass die Einstellung **Aktivieren delegierte Verwaltung von Gruppen** in der Registerkarte **Konfigurieren** auf **Ja** festgelegt ist. Sehen Sie diese Einstellung nur, wenn Sie als Benutzer angemeldet sind, die eine Lizenz Azure Active Directory Premium zugewiesen ist. Überprüfen Sie die Werte für die Benutzerattribute der Regel: Gibt es Benutzer, die die Regel erfüllen?

**Ich habe eine Regel konfiguriert, aber jetzt die vorhandene Mitglieder der Regel werden entfernt**<br/>Dies ist erwartet. Wenn eine Regel aktiviert oder geändert wird, werden vorhandene Mitglieder der Gruppe entfernt. Die Benutzer, die aus der Auswertung der Regel zurückgegeben werden als Mitglieder der Gruppe hinzugefügt.     

**Wird nicht angezeigt, dass die Mitgliedschaft ändert sich sofort beim Hinzufügen oder ändern eine Regel, warum nicht?**<br/>Auswertung dedizierten Mitgliedschaft erfolgt regelmäßig in einem Prozess asynchronen Hintergrund. Wie lange der Vorgang dauert wird durch die Anzahl der Benutzer in Ihrem Verzeichnis und die Größe der Gruppe erstellt, die als Ergebnis der Regel bestimmt. Verzeichnisse mit wenigen Benutzern werden in der Regel, die Gruppenmitgliedschaft ändert kleiner als ein paar Minuten angezeigt. Verzeichnisse mit einer großen Anzahl von Benutzern können 30 Minuten dauern, oder mehr gefüllt wird.

Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
