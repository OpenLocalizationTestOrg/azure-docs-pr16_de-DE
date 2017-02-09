<properties
   pageTitle="So starten Sie eine Access-überprüfen | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer Access-Überprüfung für berechtigte Identitäten mit der Anwendung Azure berechtigten Identitätsmanagement."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Starten eine Access-Überprüfung bei Azure AD berechtigten Identität Verwaltung von

Rollenzuweisungen werden "veralteten", wenn Benutzer Zugriff mit Berechtigungen verfügen, die sie nicht mehr benötigen. Das Risiko im Zusammenhang mit dieser veralteten rollenzuweisungen zu reduzieren, Rolle Stufe Administratoren sollten regelmäßig die Rollen, die Benutzer erhalten haben. Dieses Dokument beschreibt die Schritte zum Starten einer Access-Überprüfung in Azure AD berechtigten Identitätsmanagement (PIM) an.

## <a name="start-an-access-review"></a>Starten einer Access-überprüfen
> [AZURE.NOTE] Wenn Sie die Anwendung PIM zum Dashboard Azure-Portal hinzugefügt haben, lesen Sie die Schritte in [Erste Schritte mit Azure berechtigten Identitätsmanagement](active-directory-privileged-identity-management-getting-started.md)

Es gibt drei Methoden zum Starten einer Access-überprüfen, aus der PIM Anwendung Hauptseite:

- **Zugriff auf die Bewertungen** > **Hinzufügen**
- **Rollen** > Schaltfläche "**Überprüfen** "
- Wählen Sie aus der jeweiligen Rolle aus der Rollenliste überprüft werden > Schaltfläche " **Überprüfen** "

Beim Klicken auf die Schaltfläche " **Überprüfen** ", wird das Blade **eine Access-Überprüfung starten** . In diesem Blade würden Sie die Bewertung mit einem Namen und Zeitlimit konfigurieren, und wählen Sie eine Rolle zu überprüfen und zu entscheiden, wem die Bewertung ausgeführt werden soll.

![Starten einer Access-überprüfen - screenshot][1]

### <a name="configure-the-review"></a>Konfigurieren Sie die Bewertung

Zum Erstellen einer Access-überprüfen, müssen Sie einen Namen zu, und legen Sie ein Start- und Datum.

![Zusammenfassung – Screenshot konfigurieren][2]

Stellen Sie die Länge der Überprüfung so lange für Benutzer, um sie auszuführen. Wenn Sie vor dem Enddatum abgeschlossen haben, können Sie immer die Bewertung frühzeitig beenden.

### <a name="choose-a-role-to-review"></a>Wählen Sie eine Rolle aus, um zu überprüfen

Jede überprüfen Schwerpunkt nur eine Rolle aus. Außer wenn Sie die Access-Bewertung aus einer bestimmten Rolle Blade gestartet haben, müssen Sie nun eine Rolle auswählen.

1. Navigieren Sie zu **Überprüfen Rollenmitgliedschaft**

    ![Überprüfen Sie die Rollenmitgliedschaft - screenshot][3]

2. Wählen Sie eine Rolle aus der Liste aus.

### <a name="decide-who-will-perform-the-review"></a>Entscheiden Sie, wem die Bewertung ausgeführt werden soll

Es gibt drei Optionen zur Durchführung einer überprüfen. Sie können Person führen Sie die Bewertung zuweisen, können Sie dies tun sich selbst oder Sie von jedem Benutzer eigene Access überprüfen lassen.

1. Navigieren Sie zum **Auswählen von Bearbeitern**

    ![Wählen Sie die Bearbeiter - screenshot][4]

2. Wählen Sie eine der Optionen aus:
    - **Wählen Sie aus den Bearbeiter**: Verwenden Sie diese Option aus, wenn Sie nicht wissen, wer Zugriff darauf benötigt. Mit dieser Option können Sie eine Ressourcenbesitzer oder ein Gruppenleiter zum Abschließen die Überprüfung zuweisen.
    - **Mich**: nützlich, wenn eine Vorschau wie Access Arbeit überprüft werden soll, oder im Auftrag von Personen zu prüfen, wer kann nicht erstellt werden soll.
    - **Überprüfen Sie selbst Mitglieder**: Verwenden Sie diese Option, dass die Benutzer ihre eigenen rollenzuweisungen zu überprüfen.

### <a name="start-the-review"></a>Starten Sie die Bewertung

Schließlich müssen Sie die Möglichkeit, die erfordern, dass Benutzer einen Grund angeben, wenn sie ihre Access genehmigen. Fügen Sie bei Bedarf eine Beschreibung der Überprüfung, und wählen Sie **Starten**aus.

Stellen Sie sicher, dass Sie Ihre Benutzer wissen, dass es ist eine Access überprüfen sie warten, und zeigen sie, [wie zum Ausführen einer Access-überprüfen](active-directory-privileged-identity-management-how-to-perform-security-review.md)lassen.

## <a name="manage-the-access-review"></a>Verwalten Sie die Access-Bewertung

Sie können den Fortschritt nachverfolgen, führen Sie die Bearbeiter Überprüfungen im Dashboard Azure AD PIM im Abschnitt Zugriff Prüfungen. Keine Zugriffsrechte werden im Verzeichnis, bis [die Überprüfung abgeschlossen hat](active-directory-privileged-identity-management-how-to-complete-review.md)geändert werden.

Bis Überprüfungsperiode über befindet, können Sie erinnern Sie Benutzer, deren Prüfung abgeschlossen haben, oder die Bewertung Frühester aus dem Abschnitt der Access-Prüfungen beenden.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>Inhaltsverzeichnis PIM
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
