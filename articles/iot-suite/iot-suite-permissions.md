<properties
  pageTitle="Azure IoT Suite und Azure Active Directory | Microsoft Azure"
  description="Beschreibt, wie Azure IoT Suite Azure Active Directory verwendet, um Berechtigungen zu verwalten."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Berechtigungen für die Website azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Was passiert, wenn Sie sich anmelden

Beim ersten Anmelden bei [azureiotsuite.com][lnk-azureiotsuite], die Website bestimmt die Berechtigungsstufen, die Sie auf den aktuell ausgewählten Azure Active Directory (AAD) Mandanten und Azure-Abonnement basierend besitzen.

1.  Die Website findet zuerst heraus aus Azure welche AAD Mandanten angehören und füllen die Liste der Mandanten gesehen neben protokollierten in Ihren Benutzernamen ein. Zu diesem Zeitpunkt kann die Website jeweils nur Benutzertoken für einen Mandanten abrufen. Daher beim Wechseln zu einem anderen Mandanten mithilfe der Dropdownliste in der oberen rechten Ecke, meldet erneut die Website Sie in die Mandanten beziehen Sie die Token für die Mandanten.

2.  Als Nächstes sucht nach die Website von Azure welche Abonnements Sie den ausgewählten Mandanten zugeordnet haben ein. Sie finden Sie unter Verfügbare Abonnements beim Erstellen einer neuen vorkonfigurierten Lösung.

3.  Schließlich werden in die Website alle Ressourcen in der Abonnements und Ressourcengruppen als vorkonfigurierten Lösungen markiert und füllt die Kacheln auf der Startseite abgerufen.

Den folgenden Abschnitten werden die Rollen, die Steuerung des Zugriffs auf den vorkonfigurierten Lösungen.

## <a name="aad-roles"></a>AAD Rollen

Die Rollen AAD steuern die Möglichkeit vorkonfiguriert Bereitstellen von Lösungen und Verwalten von Benutzern in einer vorkonfigurierten Lösung.

Finden Sie weitere Informationen zu Administratorrollen, in AAD in [Zuweisen von Administratorrollen in Azure AD-][lnk-aad-admin], aber in diesem Artikel liegt der Schwerpunkt hauptsächlich auf dem **Globalen Administrator** und die **Domäne** Benutzer/Mitgliederrollen wie von den vorkonfigurierten Lösungen verwendet.

**Globaler Administrator:** Es kann viele globale Administratoren pro Mandant AAD sein. Wenn Sie einem Mandanten AAD erstellen, sind Sie standardmäßig globaler Administrator dieser Mandanten. Globale Administrator kann eine Lösung vorkonfigurierte bereitstellen und **eine Administratorrolle für die Anwendung innerhalb ihrer AAD Mandanten** zugeordnet ist. Wenn ein anderer Benutzer in der gleichen AAD Mandanten Anwendung erstellt, ist jedoch die Standardfunktion, die mit der globale Administrator gewährt wird **Nur implizit lesen**. Globale Administratoren können Rollen für Applikationen mithilfe der [Azure klassischen Portal]zuweisen[lnk-classic-portal].

**Domäne Benutzer/Mitglied:** Es kann viele Domäne Benutzer/Mitglieder pro Mandant AAD sein. Benutzer einer Domäne kann bereitstellen eine vorkonfigurierte Lösung wird über die [azureiotsuite.com] [ lnk-azureiotsuite] Website. Die Standardrolle, die für die Anwendung gewährt werden, die sie bereitstellen handelt es sich um **ADMINISTRATOR**. Sie können eine Anwendung verwenden das Skript build.cmd in der [Azure-Iot-Remote-Überwachung] erstellen[ lnk-rm-github-repo] oder [Azure-Iot-Vorhersage-Wartung] [ lnk-pm-github-repo] Repository, aber die Standardfunktion erteilt wird **Implizit schreibgeschützt**, wenn sie nicht über die Berechtigung zum Zuweisen von Rollen verfügen. Wenn ein anderer Benutzer in den Mandanten AAD Anwendung erstellt, werden sie standardmäßig für die Anwendung die Rolle **Implizit schreibgeschützt** zugewiesen. Sie verfügen nicht über die Möglichkeit, die Rollen für Applikationen zuweisen. Daher hinzufügen keine Benutzer oder Rollen für Benutzer für eine Anwendung, auch wenn sie ihn nach der Bereitstellung.

**Gast Gastbenutzer:** Es kann viele Gast Benutzer/Gäste pro Mandant AAD sein. Gastbenutzer enthält eine begrenzte Anzahl von Rechte den AAD Mandanten. Daher können keine Gastbenutzer eine vorkonfigurierte-Lösung in den Mandanten AAD bereitstellen.

Weitere Informationen finden Sie unter den folgenden Ressourcen:

- [Erstellen oder Bearbeiten von Benutzern in Azure Active Directory][lnk-create-edit-users]
- [Zuweisen der App Rollen in AAD][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Administratorrollen Azure-Abonnement

Die Azure-Administratorrollen steuern die Möglichkeit, ein Azure-Abonnement zu einem AD-Mandanten zuzuordnen.

Sie können erfahren Sie mehr über die gemeinsame Azure-Administrator, Dienstadministratoren und Konto manuell konfigurieren Rollen im Artikel [Hinzufügen oder Ändern von gemeinsame Azure-Administrator, Dienstadministratoren und Konto manuell konfigurieren][lnk-admin-roles].

## <a name="application-roles"></a>Anwendungsrollen

Die Anwendungsrollen Steuern des Zugriffs auf Geräte in Ihrer vorkonfigurierten Lösung.

Es gibt zwei definiert und ein implizit Rolle in der Anwendung, die erstellt wird, wenn Sie eine Lösung vorkonfigurierte Bereitstellung definiert.

-   **ADMINISTRATOR:** Verfügt über Vollzugriff hinzufügen, verwalten und Entfernen von Geräten

-   **Schreibgeschützt:** Hat die Möglichkeit zum Anzeigen von Geräten

-   **Implizit schreibgeschützt:** Identisch mit schreibgeschützten Dies ist für alle Benutzer von Ihrem Mandanten AAD gewährt wird. Dies erfolgte zur Vereinfachung während der Entwicklung. Sie können diese Rolle entfernen, indem Sie die [RolePermissions.cs] ändern[ lnk-resource-cs] Quelldatei.

### <a name="changing-application-roles-for-a-user"></a>Ändern der Anwendungsrollen für einen Benutzer

Das folgende Verfahren können ein Benutzer in Ihrer Active Directory Administrator Ihrer Lösung vorkonfigurierten vornehmen.

Sie müssen ein globaler Administrator AAD Rollen für einen Benutzer ändern können:

1. Wechseln Sie zu der [Azure klassischen Portal][lnk-classic-portal].

2. Wählen Sie aus **Active Directory**.

3. Klicken Sie auf den Namen Ihrer AAD Mandanten (Dies ist ein Verzeichnis, die, das Sie auf azureiotsuite.com ausgewählt haben, wenn Sie nach der Bereitstellung Ihrer Lösung).

4. Klicken Sie auf **Anwendungen**.

5. Klicken Sie auf den Namen der Anwendung, die Ihren Namen vorkonfigurierten Lösung entspricht. Wenn Sie eine Anwendung in der Liste angezeigt werden, wechseln Sie das **Anzeigen** Dropdown-Liste **Applikationen wurde von meiner Firma zugegriffen** , und klicken Sie auf das Häkchen.

7. Klicken Sie auf **Benutzer**.

8. Wählen Sie den Benutzer, die Rollen wechseln möchten.

9. Klicken Sie auf **zuweisen** , und wählen Sie die Rolle (z. B. **Administrator**) möchte der Benutzer zuweisen, klicken Sie auf das Häkchen.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Ich bin ein Dienstadministrator, und ich möchte die Zuordnung Directory zwischen mein Abonnement und einen bestimmten AAD Mandanten zu ändern. Wie kann ich tun dies?

1. Wechseln Sie zu der [Azure klassischen Portal][lnk-classic-portal], klicken Sie in der Liste der Dienste auf der linken Seite auf **Einstellungen** .

2. Wählen Sie das Abonnement, dem Sie für die Zuordnung Verzeichnis zu ändern möchten.

3. Klicken Sie auf **Verzeichnis bearbeiten**.

4. Wählen Sie das **Verzeichnis** , die Sie verwenden möchten, in der Dropdownliste aus. Klicken Sie auf den Pfeil weiter.

5. Bestätigen Sie die Zuordnung Verzeichnis und Co-Administratoren betroffen. Beachten Sie, dass Sie in ein anderes Verzeichnis verschieben, alle CO-Administratoren aus dem ursprünglichen Verzeichnis entfernt werden.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ich bin Benutzer/Mitglied der Domäne klicken Sie auf den Mandanten AAD und eine vorkonfigurierte Lösung erstellt haben. Wie zugewiesen ich eine Rolle für meine Anwendung?

Bitten Sie so weisen Sie Sie als globaler Administrator auf den Mandanten AAD abzurufenden Berechtigungen zuweisen Rollen für Benutzer selbst einen globalen Administrator, oder bitten Sie einen globalen Administrator Sie eine Rolle zuweisen. Wenn Sie für dem Mandanten AAD Ihre vorkonfigurierte Lösung ändern möchten wurde bereitgestellt, um die nächste Frage finden Sie unter.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Wie wechsle ich den Mandanten AAD, dem meiner remote Überwachung vorkonfigurierten Lösung und Anwendung zugeordnet werden?

Sie können eine Cloudbereitstellung von <https://github.com/Azure/azure-iot-remote-monitoring> ausführen und mit einem neu erstellten AAD Mandanten erneut. Da Sie standardmäßig ein globaler Administrator beim Erstellen eines neuen AAD Mandanten sind, haben Sie Zugriff für das Hinzufügen von Benutzern und Zuweisen von Rollen für diesen Benutzer.

1. Erstellen Sie ein neues AAD Verzeichnis im [Verwaltungsportal Azure][lnk-classic-portal].

2. Wechseln Sie zu <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Führen Sie `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (z. B. `build.cmd cloud debug myRMSolution`)

4. Wenn Sie dazu aufgefordert werden, legen Sie die **Tenantid** der neu erstellten Mandanten anstelle der vorherigen Mandanten sein.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ich möchte zum Ändern eines Dienstadministrator oder gemeinsame Administrator, wenn Sie mit einem Organisationsstruktur Konto angemeldet

Finden Sie im Artikel [Dienstadministrator ändern und gemeinsame Administrator, wenn Sie mit einem Organisationsstruktur Konto angemeldet][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Warum werden dieser Fehler angezeigt? "Ihr Konto weist nicht die erforderlichen Berechtigungen zum Erstellen einer Lösung desktopsetups. Wenden Sie sich an den Administrator der Benutzerkonten oder versuchen Sie es mit einem anderen Konto."

Schauen Sie sich im nachstehenden Diagramm:

![][img-flowchart]

> [AZURE.NOTE] Wenn Sie weiterhin finden Sie unter der Fehler nach dem validieren Sie sind ein globaler Administrator auf den Mandanten AAD und gemeinsame Administrator auf das Abonnement, bitten Sie Ihren Kontoadministrator zu entfernen und erneut zuweisen erforderlichen Berechtigungen in dieser Reihenfolge: den Benutzer als globaler Administrator hinzuzufügen, und klicken Sie dann Benutzer als gemeinsame Administrator für die Azure-Abonnement hinzufügen. Wenn Probleme weiterhin besteht, wenden Sie sich an, [Hilfe und Support][lnk-help-support].

**Warum werden dieser Fehler angezeigt, wenn ich ein Azure-Abonnement verfügen?** *Ein Azure-Abonnement ist erforderlich, um vorkonfiguriertes Lösungen zu erstellen. Sie können ein kostenloses Testversion Konto nur wenigen Minuten erstellen.*

Wenn Sie, dass Sie ein Azure-Abonnement besitzen sicher, überprüfen Sie den Mandanten für Ihr Abonnement Zuordnung, und stellen Sie sicher, dass der richtige Mandanten in der Dropdownliste ausgewählt ist. Wenn Sie überprüft haben der gewünschte Mandanten korrekt ist, gehen Sie vor dem Diagramm oben, und überprüfen Sie die Zuordnung von Ihr Abonnement und dieses AAD Mandanten.

## <a name="next-steps"></a>Nächste Schritte

Um den Vorgang fortzusetzen Kennenlernen IoT Suite finden Sie unter wie [Anpassen einer vorkonfigurierte Lösung]können Sie[lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
