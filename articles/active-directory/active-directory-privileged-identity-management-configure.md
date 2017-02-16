<properties
    pageTitle="Azure AD-Berechtigungen Identitätsmanagement | Microsoft Azure"
    description="Ein Thema, das wird erläutert, was Azure AD berechtigten Identitätsmanagement ist und wie Sie PIM verwenden, um Ihre Cloud-Sicherheit zu verbessern."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Die Identitätsmanagement Azure AD-Berechtigungen

Mit Berechtigungen Identitätsmanagement Azure Active Directory (AD) können Sie verwalten, steuern und Überwachen von Access innerhalb Ihrer Organisation. Zugriff auf Ressourcen in Azure Active Directory und anderen Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune umfasst.  

> [AZURE.NOTE] Berechtigte Identitätsmanagement steht nur in Verbindung mit der P2 Premium Edition von Azure Active Directory. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Organisationen möchten minimieren die Anzahl der Personen, die auf sichere Informationen oder Ressourcen, zugreifen, da, die die Wahrscheinlichkeit, dass ein bösartiger Benutzer, die von Access erste verringert. Benutzer müssen jedoch weiterhin berechtigten Vorgänge in Azure, Office 365 oder SaaS apps ausführen. Organisationen erteilen der Berechtigungen Benutzerzugriffs in Azure AD ohne Überwachung, was die Benutzer mit ihren admininistratorberechtigungen tun. Azure AD-Berechtigungen Identitätsmanagement können Sie um dieses Risiko zu beheben.  

Azure AD-Berechtigungen Identitätsmanagement hilft Ihnen:  

- Anzeigen Sie, welche Benutzer Azure AD-Administratoren sind
- Aktivieren Sie bei Bedarf, "nur in Time" Administratorzugriff auf Microsoft Online Services wie Office 365 und Intune
- Abrufen von Berichten zum Administrator Zugriff auf den Verlauf und Änderungen in Administrator Zuordnungen
- Erhalten von Benachrichtigungen zum Zugriff auf eine Rolle Stufe

Azure AD berechtigten Identitätsmanagement können die integrierten verwalten Azure AD-organisationsinterne Rollen, einschließlich:  

- Globaler Administrator
- Abrechnung-Administrator
- Dienstadministrator  
- Benutzer-Administrator
- Kennwortadministrator

## <a name="just-in-time-administrator-access"></a>Nur in Administratorzugriff Zeit

In der Vergangenheit können Sie einen Benutzer eine Administratorrolle über den Azure klassischen Portal oder Windows PowerShell zuweisen. Daher wird dieser Benutzer eines **permanent Administrator**, in der Ihnen zugewiesenen Rolle immer aktiv. Azure AD-Berechtigungen Identitätsmanagement Konzept das ein **Administrator berechtigt**. Berechtigte Administratoren sollten Benutzer, die berechtigten Zugriff und wieder, aber nicht täglich benötigen. Die Rolle ist inaktiv, bis der Benutzer Access, benötigt eine Aktivierung abschließen, und machen aktive Administrator für einen zuvor festgelegten Zeitraum.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Aktivieren Sie für Ihr Verzeichnis berechtigten Identitätsmanagement

Sie können beginnen, Azure AD berechtigten Identitätsmanagement im [Portal Azure](https://portal.azure.com/)verwenden.

>[AZURE.NOTE] Sie müssen ein globaler Administrator mit einem organisationskonto (z. B. @yourdomain.com), kein Microsoft-Konto (z. B. @outlook.com), Azure AD berechtigten Identitätsmanagement für ein Verzeichnis zu aktivieren.

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Verzeichnisses.
2. Wenn Ihre Organisation mehr als ein Verzeichnis verfügt, wählen Sie Ihren Benutzernamen in der oberen rechten Ecke des Portals Azure ein. Wählen Sie das Verzeichnis, in dem Azure AD berechtigten Identitätsmanagement soll verwendet werden.
3. Wählen Sie **Weitere Dienste** und verwenden Sie das Filters Textfeld um zu suchenden **Azure AD berechtigten Identitätsmanagement**.
4. Aktivieren Sie **zum Dashboard Pin** , und klicken Sie dann auf **Erstellen**. Die Anwendung berechtigten Identitätsmanagement wird geöffnet.

Wenn Sie die erste Person Azure AD berechtigten Identitätsmanagement in Ihrem Verzeichnis verwendet haben, führt Sie den [Datensicherheits-Assistenten](active-directory-privileged-identity-management-security-wizard.md) durch die ursprüngliche Zuordnung Oberfläche. Anschließend werden Sie automatisch die erste **Security Administrator** "und" **Rolle Stufe Administrator** des Verzeichnisses.

Nur ein Rolle Stufe-Administrator kann Zugriff für andere Administratoren verwalten. Sie können [anderen Benutzern die Möglichkeit, PIM verwalten](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Berechtigte Identitätsmanagement-dashboard

Azure AD-Berechtigungen Identitätsmanager bietet ein Dashboard, die Sie wichtige Informationen wie bietet:

- Benachrichtigungen, die sich Verkaufschancen zur Verbesserung der Sicherheit verweisen
- Die Anzahl der Benutzer, die jeder Rolle Stufe zugewiesen sind  
- Die Anzahl der in Frage kommenden und permanent Administratoren
- Laufenden Access Prüfungen

![PIM Dashboard - screenshot][2]

## <a name="privileged-role-management"></a>Rolle Stufe management

Mit Azure AD berechtigten Identität Verwaltung können Sie die Administratoren verwalten, indem Sie hinzufügen oder Entfernen von permanenten oder berechtigt Administratoren, die jeder Rolle.

![Hinzufügen/Entfernen-Administratoren PIM - screenshot][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurieren Sie die Rolle Aktivierungs-Einstellungen

Mit der [Rolle Einstellungen](active-directory-privileged-identity-management-how-to-change-default-settings.md) können Sie die Eigenschaften in Frage kommenden Rolle Aktivierung einschließlich konfigurieren:

- Die Dauer der Rolle Aktivierung Periode
- Die Benachrichtigung über die Aktivierung von Rolle
- Während der Aktivierung Rolle zur Verfügung stellen muss die Informationen eines Benutzers  

![Screenshot der PIM-Einstellungen - Administrator Aktivierung –][4]

Beachten Sie, dass im Bild, die Schaltflächen für die **Kombinierte Authentifizierung** deaktiviert sind. Mit Berechtigungen für bestimmte, hochgradig, Rollen, wir MFA für erhöhten Schutz erforderlich.

## <a name="role-activation"></a>Rolle Aktivierung  

[Aktivieren eine Rolle](active-directory-privileged-identity-management-how-to-activate-role.md)fordert Administrator berechtigt eine Uhrzeit gebundene "Aktivierung" für die Rolle. Die Aktivierung kann angefordert werden, verwenden die Option **Aktivieren meine Rolle** in Azure AD berechtigten Identitätsmanagement.

Eine Rolle aktivieren möchte der Administrator benötigt Initialisierung Azure AD berechtigten Identitätsmanagement Azure-Portal.

Rolle Aktivierung kann angepasst werden. In den Einstellungen PIM können Sie ermitteln der Länge der Aktivierung und welche Informationen der Administrator, um die Rolle zu aktivieren bereitstellen muss.

![PIM Administrator Anforderung Rolle Aktivierungs - screenshot][5]

## <a name="review-role-activity"></a>Überprüfen Sie die Rolle Aktivität

Es gibt zwei Methoden zum Nachverfolgen, wie Ihre Mitarbeiter und Administratoren Berechtigungen Rollen verwenden. Die erste Option ist [überwachenden Verlauf](active-directory-privileged-identity-management-how-to-use-audit-log.md)verwenden. Der Verlauf Audit meldet Änderungen Nachverfolgen in Rolle Stufe Zuordnungen und Rolle Aktivierung im Verlauf.

![PIM Aktivierung History - screenshot][6]

Die zweite Option ist zum normalen [Access überprüft](active-directory-privileged-identity-management-how-to-start-security-review.md)einrichten. Diese Access-Prüfungen ausgeführte Arbeit von und Bearbeiter (wie einer Teamwebsite-Manager) zugewiesen werden können oder die Mitarbeiter können selbst überprüfen. Dies ist die beste Methode zum Überwachen, wer Zugriff weiterhin erforderlich ist, und wer nicht mehr unterstützt.


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
