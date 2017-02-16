
<properties 
    pageTitle="Azure AD + Active Directory-Anforderungen für Azure RemoteApp | Microsoft Azure" 
    description="Erfahren Sie, wie Active Directory für die Arbeit mit Azure RemoteApp einrichten." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Active Directory-Anforderungen für Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .


Für Ihre Azure RemoteApp Hybrid Websitesammlung oder für eine Cloud-Websitesammlung, die Sie zusammenarbeitet mit AD verbinden möchten, müssen Sie die folgenden Aktionen ausführen.

### <a name="connect-azure-ad-and-active-directory"></a>Verbinden von Azure AD- und Active Directory

Wenn Sie Ihre Azure AD-Mandanten und Ihrem lokalen Active Directory-Umgebungen eine Verbindung herstellen möchten, verwenden Sie AD verbinden. Es dauert Sie nur [4 Klicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) zwei Verzeichnisse zu verbinden.

Hinweis: Verzeichnissynchronisation für Websitesammlungen Hybrid erforderlich ist.

### <a name="make-sure-your-domaincom-match"></a>Stellen Sie sicher, dass Ihre "@domain.com" entsprechen
Bevor Sie beginnen, stellen Sie sicher, dass der UPN für Ihre lokale Gesamtstruktur des Suffixes Ihrer Azure AD-Domäne entspricht. 

Nach dem Einrichten der Domäne Benutzerprinzipalnamen-Suffix in Azure AD, alle Benutzer in Azure RemoteApp Protokollierung werden melden Sie sich als “user@ <the suffix you set up>". Stellen Sie sicher, dass Benutzer auch mit der gleichen anmelden können user@suffix in der lokalen Domäne. In einigen Fällen können Sie einen Domänennamen in Azure Active Directory, während Sie eine andere Domänensuffix für die Benutzer auf Vorz einrichten In diesem Fall können die Benutzer keine Verbindung jeder Domäne Computern und Ressourcen über Azure RemoteApp werden.

Beispielsweise, wenn Sie Ihre Benutzerprinzipalnamen Domänensuffix in AAD als "contoso.com" einrichten, aber einige Benutzer auf lokale/AD werden so konfiguriert, dass die Anmeldung @contoso.uk, und klicken Sie dann diesen Benutzern nicht ordnungsgemäß in die Sammlung ARA melden werden können. Benutzer Benutzerprinzipalnamen AAD und AD muss für die Anmeldung möglich ist gleich"

### <a name="create-objects-for-azure-remoteapp"></a>Erstellen von Objekten für Azure RemoteApp
Sie müssen die folgenden lokalen Active Directory-Objekte zu erstellen:

- Ein Dienstkonto für den Zugriff auf die Domänenressourcen für RemoteApp-Programme durch Verknüpfen von RDSH Endpunkte mit der lokalen Domäne.
- Eine Organisationseinheit (OU) RemoteApp maschinellen Objekte enthält. Verwenden der Organisationseinheit wird empfohlen (aber nicht erforderlich) zum Isolieren die Konten und Richtlinien, die Sie mit RemoteApp verwendet werden.

Sie benötigen beide dieser Objekte beim Erstellen Ihrer Websitesammlung RemoteApp, daher müssen Sie zuerst folgende Schritte ausführen.