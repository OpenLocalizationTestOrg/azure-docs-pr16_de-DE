<properties
   pageTitle="So aktivieren oder Deaktivieren einer Rolle | Microsoft Azure"
   description="Erfahren Sie, wie Rollen für berechtigte Identitäten durch die Azure berechtigten Identitätsmanagement-Anwendung zu aktivieren."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>So aktivieren oder Deaktivieren der Rollen in Azure AD berechtigten Identitätsmanagement

Azure Active Directory (AD) berechtigten Identitätsmanagement vereinfacht wie Unternehmen berechtigten Zugriff auf Ressourcen in Azure Active Directory und anderen Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune verwalten.  

Wenn Sie Anspruch auf einer Administratorrolle vorgenommen wurden, die bedeutet, dass Sie diese Rolle aktivieren können, wenn Sie eine Aufgabe auszuführen, die diese Rolle erfordert, müssen. Angenommen, wenn Sie Office 365-Features gelegentlich verwalten, möglicherweise Ihrer Organisation Rolle Stufe Administratoren nicht Sie ein permanent globaler Administrator, da diese Rolle andere Dienste auch wirkt sich auf. Stattdessen machen sie Sie für Azure AD-Rollen wie Exchange Online-Administrator berechtigt. Sie können anfordern, aktivieren die Rolle aus, benötigen Sie seine Berechtigungen, und klicken Sie dann Sie müssen Administrator Steuerelement für eine zuvor festgelegten Zeitraum.

Dieser Artikel ist für Administratoren, die ihre Rolle in Azure AD berechtigten Identitätsmanagement (PIM) aktivieren müssen. Es führt Sie durch die Schritte zum Aktivieren einer Rolle, wenn Sie die Berechtigungen benötigen, und deaktivieren die Rolle aus, wenn Sie fertig sind.


## <a name="add-the-privileged-identity-management-application"></a>Fügen Sie die Berechtigungen Identitätsmanagement-Anwendung

Verwenden Sie die Azure AD berechtigten Identitätsmanagement-Anwendung im [Portal Azure](https://portal.azure.com/) zum Anfordern einer Rolle Aktivierungs, selbst wenn Sie vertraut sind, in ein anderes Portal oder PowerShell ausgeführt werden. Wenn Sie die Anwendung Azure AD berechtigten Identitätsmanagement Ihrer Azure-Portal besitzen, folgendermaßen Sie vor, um anzufangen.

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)aus.
2. Wählen Sie Ihren Benutzernamen in der oberen rechten Ecke des Azure-Portal, und wählen Sie das Verzeichnis, in dem Sie Sie werden, ausgeführt werden.
3. Wählen Sie **Weitere Dienste** und verwenden Sie das Filters Textfeld um zu suchenden **Azure AD berechtigten Identitätsmanagement**.
4. Überprüfen Sie **Pin zum Dashboard** , und klicken Sie dann auf **Erstellen**. Die Anwendung berechtigten Identitätsmanagement wird geöffnet.

## <a name="activate-a-role"></a>Aktivieren einer Rolle

Wenn Sie eine Rolle übergeben müssen, können Sie die Aktivierung über die Schaltfläche **Aktivieren Sie meine Rollen** in der Anwendung Azure AD berechtigten Identitätsmanagement anfordern.


1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) an, und wählen Sie die Kachel Azure AD berechtigten Identitätsmanagement.
2. Wählen Sie **Mein Rollen aktivieren**aus. Eine Liste aller Rollen, die Ihnen zugewiesen wird angezeigt.
3. Wählen Sie die Rolle aus, die Sie aktivieren möchten.
4. Wählen Sie **Aktivieren**aus. Das **Anfordern von Rolle Aktivierung** Blade wird angezeigt.
5. Einige Rollen erfordern mehrstufige Authentifizierung (MFA), bevor Sie die Rolle aktivieren können. Sie müssen nur einmal pro Sitzung authentifizieren.

    ![Mit MFA zu überprüfen, bevor die Rolle Aktivierungs - screenshot][2]

6. Geben Sie den Grund für die Aktivierung Anforderung im Textfeld ein.  Einige Rollen ist es erforderlich, eine Ticketnummer Problemen angeben.
7. Wählen Sie **OK**aus.  Jetzt die Rolle aktiviert ist, und die Änderung der Rolle in der Microsoft Online Services sichtbar ist.

## <a name="deactivate-a-role"></a>Deaktivieren einer Rolle

Nachdem eine Rolle aktiviert wurde, wird sie automatisch, wenn die Uhrzeit erreicht ist deaktiviert.

Wenn Sie früh fertig sind, können Sie auch eine Rolle manuell in die Azure AD berechtigten Identitätsmanagement-Anwendung deaktivieren.  Wählen Sie **Meine Rollen aktivieren**aus, und wählen Sie die Rolle Sie fertig sind, verwenden, und wählen Sie **Deaktivieren**.  


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen zur Verwaltung von Azure AD berechtigten Identität interessante befinden, müssen die folgenden Links Informationen aus.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
