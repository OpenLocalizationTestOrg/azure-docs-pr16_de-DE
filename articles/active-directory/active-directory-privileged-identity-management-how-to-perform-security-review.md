<properties
   pageTitle="So führen Sie eine Access-überprüfen | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine Bewertung für die Azure berechtigten Identitätsmanagement-Anwendung ausführen."
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
   ms.date="09/16/2016"
   ms.author="kgremban"/>

# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a>So führen Sie eine Access-Überprüfen bei Azure AD berechtigten Identität Verwaltung

Azure Active Directory (AD) berechtigten Identitätsmanagement vereinfacht wie Unternehmen berechtigten Zugriff auf Ressourcen in Azure Active Directory und anderen Microsoft-Onlinedienste wie Office 365 oder Microsoft Intune verwalten.  

Wenn Sie einer Administratorrolle zugewiesen sind, kann der Administrator Ihrer Organisation Rolle Stufe aufgefordert, regelmäßig zu bestätigen, dass Sie diese Rolle für Ihre Arbeit noch benötigen. Möglicherweise erhalten Sie eine e-Mail, die einen Link enthält, oder Sie können direkt auf das [Portal Azure](https://portal.azure.com)wechseln. Führen Sie die Schritte in diesem Artikel ausführen Self Überprüfen Ihrer zugewiesenen Rollen aus.

Wenn Sie Administrator einer Rolle Stufe Access Prüfungen interessiert sind, erhalten Sie weitere Details an, [wie eine Access-Überprüfung starten](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Fügen Sie die Berechtigungen Identitätsmanagement-Anwendung

Die Anwendung Azure AD berechtigten Identität Management (PIM) können im [Azure-Portal](https://portal.azure.com/) Sie Ihre Bewertung ausführen.  Wenn Sie die Azure AD berechtigten Identitätsmanagement-Anwendung auf das Portal besitzen, folgendermaßen Sie vor, um anzufangen.

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)aus.
2. Wählen Sie Ihren Benutzernamen in der oberen rechten Ecke des Azure-Portal, und wählen Sie das Verzeichnis, in dem Sie Sie werden, ausgeführt werden.
3. Wählen Sie **Weitere Dienste** und verwenden Sie das Filters Textfeld um zu suchenden **Azure AD berechtigten Identitätsmanagement**.
4. Überprüfen Sie **Pin zum Dashboard** , und klicken Sie dann auf **Erstellen**. Die Anwendung berechtigten Identitätsmanagement wird geöffnet.


## <a name="approve-or-deny-access"></a>Genehmigen oder ablehnen von access

Wenn Sie genehmigen oder den Zugriff verweigern, veranlassen Sie, ob Sie diese Rolle oder nicht weiterhin verwenden nur dem Bearbeiter. Wählen Sie **Genehmigen** , wenn Sie wünschen, um in der Rolle oder **Verweigern** zu bleiben, wenn Sie die Access nicht mehr benötigen. Ihren Status wird nicht sofort ändern, bis der Bearbeiter die Ergebnisse angewendet wird.
Wie folgt vor, um zu suchen, und führen Sie die Access-Bewertung:

1. Wählen Sie in der Anwendung PIM **berechtigten Zugriff überprüfen**. Wenn Sie alle Prüfungen steht noch aus Access haben, werden sie in das Azure Active Directory Access Prüfungen Blade angezeigt.
2. Wählen Sie die Bewertung, die durchgeführt werden soll.
3. Es sei denn, Sie die Bewertung erstellt haben, werden Sie als Benutzer nur bei der Überprüfung angezeigt. Wählen Sie das Häkchen neben Ihrem Namen ein.
4. Wählen Sie entweder **Genehmigen** oder **Ablehnen**. Möglicherweise müssen Sie einen Grund für Ihre Entscheidung in das Textfeld **Bereitstellen einer Grund** aufnehmen möchten.  
5. Schließen Sie das Blade **Überprüfen Azure AD-Rollen** .


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
