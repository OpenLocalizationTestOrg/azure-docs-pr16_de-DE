<properties
    pageTitle="Azure Active Directory Vorschau Explainer | Microsoft Azure"
    description="Ein Thema, das die Unterschiede zwischen Azure Active Directory im Portal klassischen und der Azure-Active Directory-Vorschau im Azure-Portal erläutert."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Vorschau der Azure-Active Directory-Verwaltungsoption Azure-Portal

Die Verwaltungsoption Azure Active Directory (Azure AD) ist in der Vorschau im Azure-Portal. Sie können es verkleinern versuchen, melden Sie sich [der Azure-Portal](https://portal.azure.com) als globaler Administrator Ihres Verzeichnisses. Klicken Sie dann in der Dienstliste wählen Sie Azure Active Directory aus, wenn es angezeigt wird, oder wählen Sie **Weitere Dienste** , die die Liste aller Dienste anzuzeigen. Ist nicht erforderlich, eine Azure-Abonnement verwenden Sie die Azure AD-Verwaltung der Azure-Portal auftreten.


## <a name="capabilities-of-the-preview-experience"></a>Funktionen, die von der Preview-Benutzeroberfläche

Mit der Vorschau können Sie viele Directory-Ressourcen, wie z. B. Benutzer, Gruppen und Anwendungen sowie Directory Einstellungen im Azure-Portal zu verwalten. Wir werden diese Erfahrung nehmen Sie alle Funktionen, die vorhanden sind, in der Azure verbessern AD Management im [Azure klassischen Portal](https://manage.windowsazure.com)auftreten. Es gibt einige Directory Management Aufgaben, die Sie weiterhin müssen in der klassischen Portal durchführen, bis zu diesem Zeitpunkt.

## <a name="manage-the-same-azure-ad-tenants"></a>Verwalten der gleichen Azure AD-Mandanten

Die Vorschau-Oberfläche liest und schreibt in den gleichen Azure Active Directory-Mandanten als klassische Portals und Office 365 Admin Center. In einem der folgenden communityportalen vorgenommene Änderungen sind in allen anderen erscheinen.

## <a name="use-the-same-authorization-logic"></a>Verwenden Sie die gleiche Autorisierungsslogik

Die Vorschau-Oberfläche verwendet dieselbe Autorisierungsslogik als vorhandenen Active Directory-Clients. Benutzer dürfen, Directory-Ressourcen entsprechend ihrer Directory-Funktion, wie z. B. globaler Administrator, Benutzer-Administrator, Ihr Kennwort ein Administrator zu ändern. Gibt es eine Rolle auf Azure Ressourcen oder ein Azure-Abonnement genehmigt kein Benutzers zum Verzeichnisressourcen verwalten. Weitere Informationen Azure AD-Management Rollen, finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md). 

Mit der Vorschau ist für globale Administratoren optimiert. Wenn Sie die Vorschau-Oberfläche verwenden während als Benutzer angemeldet, die kein globaler Administrator angemeldet ist, können Sie eine beeinträchtigt Erfahrung haben. Beispielsweise können Sie möglicherweise eine Schaltfläche auswählen, mit dem Sie eine Aufgabe beginnen, die Sie nicht im Verzeichnis abschließen können. Wir werden diese bald verbessern.
 
## <a name="tell-us-what-you-think"></a>Teilen Sie uns Ihre Meinung

Sie können Ihr Feedback der Preview-Erfahrung im Portal Admin-Abschnitt des [Azure AD-Feedback-Forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc)lassen.
