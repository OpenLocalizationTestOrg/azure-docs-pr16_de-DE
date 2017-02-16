<properties
    pageTitle="Konzeptionelle Übersicht über benutzerdefinierten Domänennamen in Azure Active Directory | Microsoft Azure"
    description="Erläutert das konzeptionelle Framework bei der Verwendung von benutzerdefinierten Domänennamen in Azure-Active Directory, einschließlich Föderation für einmaliges Anmelden"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Konzeptionelle Übersicht über benutzerdefinierten Domänennamen in Azure Active Directory

Ein Domänennamen ist ein wichtiger Teil des Bezeichners für viele Directory Ressourcen: Es ist Teil einer Benutzernamen oder e-Mail-Adresse für einen Benutzer, einen Teil der Adresse für eine Gruppe, und kann Bestandteil der app-ID-URI für eine Anwendung. Eine Ressource in Azure Active Directory (Azure AD) kann einen Domänennamen enthalten, der bereits sichergestellt ist, dass das Verzeichnis gehören, die die Ressource enthält. Nur globale Administratoren kann Verwaltungsaufgaben für die Domäne in Azure AD ausführen.

Domänennamen in Azure AD-sind global eindeutig. Ein Domänennamen kann von einem einzelnen Azure AD verwendet werden. Wenn eine Azure AD-Verzeichnis einen Domänennamen überprüft wurde, kann keine anderen Azure AD-Verzeichnis überprüfen oder die gleichen Domänennamen verwenden.

## <a name="initial-and-custom-domain-names"></a>Ursprüngliche und benutzerdefinierten Domänennamen

Jeder Domänennamen in Azure AD-ist entweder eine ursprünglichen Domänennamen oder einen benutzerdefinierten Domänennamen.

Jede Azure AD verfügt über eine ursprünglichen Domänennamen in das Formular contoso.onmicrosoft.com. Diese dritte Ebene Domänennamen in diesem Beispiel "contoso.onmicrosoft.com", wurde hergestellt, wenn Verzeichnis, in der Regel durch den Administrator erstellt wurde, die das Verzeichnis erstellt hat. Der ursprünglichen Domänennamen für ein Verzeichnis kann nicht geändert oder gelöscht werden. Der ursprünglichen Domänennamen, während voll funktionsfähige soll vor allem als ein bootstrapping Verfahren verwendet werden, bis, dass Sie keinen benutzerdefinierten Domänennamen sichergestellt ist.

In den meisten Fällen Herstellung ein Verzeichnis muss mindestens eine überprüfte benutzerdefinierte Domäne, beispielsweise "contoso.com", und es ist die benutzerdefinierte Domäne, die für den Endbenutzer sichtbar ist. Ein benutzerdefinierten Domänennamen ist einen Domänennamen aus, der im Besitz und verwendet, die von dieser Organisation, wie etwa "contoso.com" für den Einsatz seiner Website Hostinganbieter. Der Domänenname ist vertraut zu Mitarbeitern, da es Teil den Benutzernamen handelt, mit dem Firmennetzwerk verbunden ist, melden Sie sich zu senden und Empfangen von e-Mails mit.

Bevor sie von Azure AD verwendet werden kann, muss der benutzerdefinierten Domänennamen zum Verzeichnis hinzugefügt und überprüft.

## <a name="verified-and-unverified-domain-names"></a>Überprüft und nicht überprüft Domänennamen

Der ursprünglichen Domänennamen für ein Verzeichnis wird implizit ausgewertet, wie von Azure AD überprüft. Wenn ein Administrator einen benutzerdefinierten Domänennamen zu einer Azure AD hinzufügt, empfiehlt es sich ursprünglich in einem Zustand nicht überprüft. Azure AD lässt sich nicht auf Verzeichnisressourcen einen nicht überprüft Domänennamen verwenden aus. Dies stellt sicher, dass nur ein Verzeichnis auf einen bestimmten Domänennamen können und, dass die Organisation der Domänenname verwendet wird tatsächlich diesen Domänennamen besitzt.

Azure AD überprüft Besitz eines Domänennamens durch Suchen nach einem bestimmten Eintrag in der Zonendatei Domain Name Service (DNS) für den Domänennamen. Zum Überprüfen der Besitzrechte an einen Domänennamen, ruft ein Administrator des DNS-Eintrags aus Azure AD ab, Azure AD gesucht wird, und die DNS-Zonendatei für den Domänennamen dieser Eintrag hinzugefügt. Die DNS-Zonendatei wird von der Domänennamen-Registrierungsstelle für diese Domäne verwaltet. Die Schritte zum Überprüfen der Domäne sind im Artikel zum [Hinzufügen einer benutzerdefinierten Domänennamens in Ihr Verzeichnis Azure AD-](active-directory-add-domain.md)aufgeführt.

Hinzufügen eines DNS-Eintrags in der Zonendatei für den Domänennamen wirkt sich nicht auf andere Domänendienste wie e-Mail oder Bereitstellung auf einem Webserver.

## <a name="federated-and-managed-domain-names"></a>Partnerverbundkontakte und verwaltete Domänennamen

Ein benutzerdefinierten Domänennamen in Azure AD-kann konfiguriert werden, um Benutzern in Erfahrung zwischen Ihrem lokalen Active Directory und Azure AD-partnerverbundkontakte bei der Anmeldung. Konfigurieren einer Domäne für die Föderation erfordert Updates auf berechtigten Ressourcen in Azure AD- und auch Ihre Windows Server Active Directory. Konfigurieren, die eine verbunddomäne von Azure AD verbinden erledigt werden muss, oder mithilfe der PowerShell. Eine benutzerdefinierte Domäne Föderation kann nicht vom klassischen Azure-Portal initiiert werden. [Schauen Sie sich dieses Video an, erfahren Sie mehr über das Konfigurieren von AD FS für Benutzer melden Sie sich mit Azure AD verbinden](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domänen, die keine partnerverbundkontakte hinzugefügt werden, werden als verwaltete Domänen bezeichnet. Die ursprüngliche Domäne für ein Verzeichnis Azure AD-wird implizit als verwalteten Domäne ausgewertet.

## <a name="primary-domain-names"></a>Primäre Domänennamen

Primärer Domänenname für ein Verzeichnis ist den Domänennamen, der vorab als den Standardwert für den '' Domänenteil den Benutzernamen ausgewählt ist, erstellt ein Administrator einen neuen Benutzer in der [Azure klassischen Portal](https://manage.windowsazure.com/) oder ein anderes wie dem Verwaltungsportal von Office 365-Portal an. Ein Verzeichnis kann nur einen primären Domänennamen besitzen. Der Administrator kann eine benutzerdefinierte Domäne, die keine partnerverbundkontakte hinzugefügt wird, überprüft werden des primären Domänennamens ändern oder die ursprüngliche Domäne.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domänennamen in Azure AD- und andere Microsoft Online Services

In Azure AD, bevor sie von einem anderen Microsoft Online-Dienst wie Exchange Online, SharePoint Online und Intune verwendet werden kann, muss ein Domänennamen überprüft werden. Diese Dienste erfordern in der Regel ein Administrator einen oder mehrere DNS-Einträge hinzufügen, die speziell für den Dienst gelten.

Eine Azure Web-app verwendet eigenes Verfahren Besitzrechte für eine Domäne überprüft. Selbst wenn sie zuvor für die Verwendung von einer Azure Web-app in einem Abonnement überprüft wurden, die auf dieser Azure AD basiert, muss eine Domäne für die Verwendung mit Azure AD überprüft werden. Eine Azure Web-app kann einen Domänennamen verwenden, der in einem anderen Verzeichnis aus dem Verzeichnis überprüft wurde, die das Web app sichert.

## <a name="managing-domain-names"></a>Verwalten von Domänennamen

Domäne Verwaltungsaufgaben können vom klassischen Azure-Portal und von PowerShell ausgeführt werden. Viele Aufgaben können mithilfe der Azure AD Graph-API (in public Preview-Version) ausgeführt werden.

-   [Hinzufügen und überprüfen einen benutzerdefinierten Domänennamen](active-directory-add-domain.md)

-   [Verwalten von Domänen in der klassischen Azure-portal](active-directory-add-manage-domain-names.md)

-   [Mithilfe der PowerShell Domänennamen in Azure AD verwalten](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Verwenden die Azure AD Graph-API zum Verwalten von Domänennamen in Azure Active Directory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
