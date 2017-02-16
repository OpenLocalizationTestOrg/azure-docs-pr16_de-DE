<properties 
    pageTitle="Welche Arten von Websitesammlungen benötigen Sie für Azure RemoteApp? | Microsoft Azure" 
    description="Informationen Sie zu den Arten von Websitesammlungen mit Azure RemoteApp zur Verfügung." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Welche Arten von Websitesammlungen benötigen Sie für Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

RemoteApp Azure ermöglicht Ihnen das Freigeben von apps und Ressourcen für Benutzer auf jedem Gerät. Dazu wird das Erstellen von Sammlungen die apps und Ressourcen enthalten soll, und dann diese Websitesammlungen für Benutzer freigeben. Es gibt 2 andere Sammlungsoptionen, mit anderen Netzwerk herzustellen und Authentifizierungsoptionen – für Sie richtig ist?

Lassen Sie uns durchgehen anderen Besonderheiten sowie den Optionen, die Sie in Ihrer Websitesammlung Azure RemoteApp optimal vornehmen müssen. 


## <a name="quick-differences-between-the-collection-types"></a>Schnelle Unterschiede zwischen den Typen der Websitesammlung

|           | Cloud | Hybrid |
|-----------|-------|--------|
|Verwenden einer vorhandenen VNET| Ja| Ja|
|Erfordert AD-verbundene Konten (DirSync)| Nein| Ja|
|Beitreten zu einer Domäne erfordert| Nein| Ja|
|Erfordert Domain Controller VNET zugreifen können| Nein| Ja|

## <a name="cloud-collections"></a>Cloud-Sammlungen
- Beim Erstellen von - Symbolleiste die Sammlung wird schnell bereitgestellt, d. h., Ihre apps erhalten für Benutzer schneller.
- Bringen Sie Ihre eigenen apps, oder Teilen Sie uns. Sie können ein benutzerdefiniertes Bild (aus einer Azure-virtuellen Computer erstellt) oder eines der Bilder in Ihrem Abonnement enthalten.
- Sie benötigen keine so konfigurieren Sie eine Verbindung zwischen der Sammlung und Ihrer lokalen Domäne.
- Sie können aber optional verwenden Ihrer eigenen Azure VNET zum Bereitstellen des Zugriffs in Ihrer lokalen Umgebung für die gemeinsame Nutzung von Daten oder zum Verwenden von nicht-Windows-Authentifizierung in Ressourcen wie SQL Server (mithilfe der Datenbankauthentifizierung).


OK, wie erstelle ich eine?

- Cloud? Erstellen mit die Option **Symbolleiste erstellen** im Portal.
- Cloud + VNET? Erstellen Sie, verwenden die Option **mit VNET erstellen** , jedoch keiner Domäne hinzugefügt werden sollen.

## <a name="hybrid-collections"></a>Hybrid Websitesammlungen
- Bieten Sie vollständigen Zugriff auf lokale Netzwerk + Azure VNET.
- Enthält die Domäne Verknüpfung Access-apps und Daten. Remote Applications können Authentifizierung bei Ihrem lokalen Active Directory - dann Ressourcen in Ihrer Domäne zugreifen können.
- Aktivieren Sie erweiterte Überwachung und Verwaltung mit vorhandenen System Center Lösungen und Windows-Gruppenrichtlinien (über ein benutzerdefiniertes Bild unter Windows Server 2012 R2 integriert)
- [ExpressRoute](https://azure.microsoft.com/services/expressroute/) zum Verbinden Ihrer Azure VNET mit Ihrem lokalen VNET zu unterstützen.

Erstellen Sie, verwenden die Option **mit VNET erstellen** und WÄHLE Beitritt zu einer Domäne aus.

## <a name="authentication-options"></a>Authentifizierungsoptionen
Azure RemoteApp unterstützt sowohl Microsoft-Konten und Azure-Active Directory-Konten, jedoch nicht aller Websitesammlungen alle Methoden. 

| Kontotyp                      |                                                             | Cloud | Cloud + VNET | Hybrid |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoft-Konto                 |                                                             | Ja   | Ja          | Nein     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Azure Active Directory                                               | Ja   | Ja          | Nein     |
|                                   | AD Verbinden mit Kennwort synchronisieren                               | Ja   | Ja          | Ja    |
|                                   | Verbinden von AD ohne Kennwort synchronisieren                            | Ja   | Ja          | Nein     |
|                                   | AD Verbinden mit AD FS                                       | Ja   | Ja          | Ja    |
|                                   | 3rd Drittanbietern Azure unterstützt Identitätsanbieter (z. B. Ping) | Ja   | Ja          | Ja    |
| Mehrstufige Authentifizierung       |                                                             | Ja   | Ja          | Ja    |



### <a name="cloud-and-cloud--vnet"></a>Cloud und Cloud + VNET 
Mit Cloud-Sammlungen können Sie das Microsoft-Konten, Azure AD-Konten oder eine Mischung aus beiden. Verwenden Sie die Konten, die für Ihre Benutzer am besten geeignet.

Es gibt keine spezifischen Voraussetzungen für die Verwendung von Microsoft-Konten. 

Wenn Sie Azure AD-Konten verwenden möchten, müssen Sie sicherstellen, dass Ihre Azure AD-Mandanten eine mit Ihrem Abonnement verknüpft ist entspricht. Wenn Sie Ihr Abonnement Azure RemoteApp erstellt haben, wurde der zuvor von Ihnen verwendete Azure AD-Mandanten automatisch mit Ihrem Abonnement verknüpft. Jeder Azure AD-Benutzer, den Sie über die Berechtigung zum gewähren muss sich die gleichen Mandanten befinden. Bei Bedarf können Sie [den Azure AD-Mandanten ändern](remoteapp-changetenant.md) , die mit Ihrem Abonnement verknüpft ist.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybrid (oder Cloud + Azure AD + AD)

Azure AD- + lokalen Active Directory ist eine Vorbedingung für eine Websitesammlung Hybrid. Sie müssen AD verbinden verwenden, um die beiden Verzeichnisse zu integrieren. Aber Sie haben eine Auswahl auf, wenn es geht, wie Sie AD verbinden konfigurieren. 

Es gibt 2 AD verbinden Szenarien - Synchronisierung von Kennwörtern verwenden oder Verwenden von Active Directory Federation. Schauen Sie sich die [Informationen AD verbinden](../active-directory/active-directory-aadconnect.md) Sie herausfinden, welche der folgenden Works für Sie am besten geeignet.

Sie können auch mit einer Websitesammlung Cloud Azure AD- + AD verwenden. Stellen Sie sicher, dass Sie die gleichen Schritte einrichten folgen.

Schauen Sie sich [Azure AD + Active Directory-Anforderungen für Azure RemoteApp](remoteapp-ad.md) für die erforderlichen Schritte zum Konfigurieren von Azure AD- und Active Directory.

## <a name="go-create-your-collection"></a>Wechseln der Websitesammlung erstellen!
OK, denke ich, dass wir es nun schon festgestellt haben, sodass nur einem Gegenstand Links führen – Erstellen Ihrer ersten Azure RemoteApp Websitesammlung vorhanden ist.

[Erstellen einer Sammlung Cloud](remoteapp-create-cloud-deployment.md) oder [Erstellen einer Websitesammlung Hybrid](remoteapp-create-hybrid-deployment.md) - erste nur erstellen.
