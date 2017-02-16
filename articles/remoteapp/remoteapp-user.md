<properties
    pageTitle="Hinzufügen eines Benutzers in Ihrer Websitesammlung Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Benutzer zu Ihrer Websitesammlung Azure RemoteApp hinzufügen"
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

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Zum Hinzufügen eines Benutzers zur Azure RemoteApp Sammlung

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Bevor Sie Ihre Benutzer finden Sie unter und Ihre apps in Azure RemoteApp verwenden können, müssen Sie ihnen Zugriff auf Ihre Sammlung gewähren. Dies ist der einfache: Geben Sie auf der Registerkarte **Des Benutzerzugriffs** die Kontoinformationen für den Benutzer aus, und klicken Sie dann auf das Häkchen.

Benötigen Kontoinformationen Sie? Dies hängt den Typ der Websitesammlung Sie erstellt haben (Cloud oder Hybrid) und gibt an, ob Sie Office 365 ProPlus in dieser Sammlung verwenden.

## <a name="supported-user-identities"></a>Unterstützte Benutzeridentitäten

Die andere Sammlungstypen (im Vergleich zu Hybriden Cloud) Unterstützung für verschiedene Benutzeridentitäten für den Zugriff auf Anwendungen verwenden.  

Für eine Websitesammlung Hybrid von RemoteApp müssen Sie eine Domäne Active Directory-Infrastruktur auf lokale und einem Azure Active Directory-Mandanten mit Verzeichnisintegration einrichten (und optional einmaliges Anmelden). Darüber hinaus müssen Sie einige Active Directory-Objekte im lokalen Verzeichnis zu erstellen.  

Für eine Websitesammlung Cloud von RemoteApp kann alle Benutzer mit Azure Active Directory Identitäten unterstützen des Benutzerzugriffs auf RemoteApp Microsoft Accounts aufnehmen möchten erteilt werden.  Finden Sie in der folgenden Tabelle aus.

Office 365-Benutzer können sich Azure-Active Directory-Benutzer. Wenn sie Azure Active Directory-Hybrid aufweisen, können Directory-Konten synchronisiert sie Benutzerzugriff in einem RemoteApp hybridbereitstellung erteilt werden.   

In dieser Tabelle können als Kurzübersicht für die Identität in Ihrer Websitesammlung und was sind von die Active Directory-Anforderungen unterstützt wird.

|Benutzerkonten |Cloud   |Hybrid|
|--------------|--------|------|
|Microsoft-Konto|     Ja|    Nein|
|Azure Active Directory (Azure AD)| | |
|Nur Azure AD-cloud    |Ja    |Nein |
|ADsync mit Kennwort synchronisieren  |Ja    |Ja    |
|ADsync ohne Kennwort synchronisieren|  Ja |Nein |
|ADsync mit AD FS  |Ja    |Ja    |
|[3rd Drittanbietern Azure unterstützt Identitätsanbieter](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (Beispiel: Ping) |Ja    |Ja|
|Mehrstufige Authentifizierung    |Ja    |Ja    |

Lesen Sie [Weitere Informationen](remoteapp-ad.md) zum Konfigurieren von Active Directory für RemoteApp aus.


> [AZURE.NOTE] Der Azure-Active Directory-Benutzer muss aus den Mandanten, die Ihr Abonnement zugeordnet ist. (Sie können anzeigen und Ändern Ihres Abonnements auf der Registerkarte **Einstellungen** im Portal. Informationen finden Sie unter [Ändern der Azure-Active Directory-Mandanten untersuchten RemoteApp](remoteapp-changetenant.md) Weitere.)

## <a name="office-365-proplus-user-account-information"></a>Office 365 ProPlus Benutzerkontoinformationen
Wenn Sie das Office 365 ProPlus Vorlagenbild in Ihrer Websitesammlung *oder* verwenden werden, wenn Sie ein benutzerdefiniertes Bild erstellt haben, das Office 365 verwendet, dürfen Sie nur Azure-Active Directory-Benutzer hinzufügen, die Office 365-Abonnements für die Standarddomäne Ihres Abonnements enthalten. Weitere Informationen finden Sie unter [Verwenden von Office 365 mit Azure RemoteApp](remoteapp-o365.md) .
