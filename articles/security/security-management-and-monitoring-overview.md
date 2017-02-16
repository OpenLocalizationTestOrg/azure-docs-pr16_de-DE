<properties
   pageTitle="Sicherheit von Azure-Verwaltung und Überwachung Übersicht | Microsoft Azure"
   description=" Azure bietet Sicherheit Verfahren zur Unterstützung der Verwaltung und Azure-Cloud-Diensten und virtuellen Computern zu überwachen.  Dieser Artikel enthält eine Übersicht über diese zentrale Sicherheitsfeatures und Dienste. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Sicherheit von Azure Management und Überwachung (Übersicht)

Azure bietet Sicherheit Verfahren zur Unterstützung der Verwaltung und Azure-Cloud-Diensten und virtuellen Computern zu überwachen. Dieser Artikel enthält eine Übersicht über diese zentrale Sicherheitsfeatures und Dienste. Links werden, die Details der einzelnen erteilen, damit Sie weitere Artikel bereitgestellt.

Die Sicherheit Ihrer Microsoft Cloud-Dienste ist eine Zusammenarbeit und freigegebenen Zuständigkeit zwischen Ihnen und Microsoft. Freigegebene Zuständigkeit bedeutet Microsoft ist für den Microsoft Azure verantwortlich und physische Sicherheit seiner Daten zentriert (durch die Verwendung von Sicherheitsmaßnahmen wie gesperrte Badge Eintrag Türen, Klammern und Schutz). Darüber hinaus bietet Azure signifikante Sicherheitsebenen Cloud auf der Softwareebene, die die Sicherheit, Datenschutz und Regelkonformität Bedürfnisse seiner Kunden anspruchsvolle entspricht.

Sie besitzen, Ihre Daten und Identitäten, die Zuständigkeit für den Schutz von ihnen, die Sicherheit Ihrer lokalen Ressourcen und die Sicherheit des Cloud-Komponenten für die Sie die Kontrolle haben. Microsoft bietet Ihnen Sicherheit Steuerelemente und Funktionen, mit denen Sie die Daten und Applikationen schützen. Ihre Zuständigkeit für die Sicherheit basiert auf den Typ des Cloud-Dienst.

Das folgende Diagramm enthält eine Zusammenfassung den Saldo Zuständigkeitsbereich für Microsoft- und den Kunden aus.

![Freigegebene Zuständigkeit][1]

Sich ausführlicher Sicherheits-Management finden Sie unter [Sicherheitsmanagement in Azure](azure-security-management.md).

Hier sind die wichtigsten Features, die in diesem Artikel behandelt werden:

- Rollenbasierte Access Control
- Modul
- Mehrstufige Authentifizierung
- ExpressRoute
- Virtuelle Netzwerkgateways
- Verwaltung von Berechtigungen Identität
- Schutz der Identität
- Sicherheitscenter

## <a name="role-based-access-control"></a>Rollenbasierte Access Control

Rollenbasierte Access Steuerelement (RBAC) ermöglicht die Verwaltung von abgestimmte Zugriff für Azure Ressourcen. Mit RBAC, können Sie Personen nur die Menge von Zugriff gewähren, die sie zum Erfüllen ihrer Aufgaben benötigen.  RBAC auch helfen Ihnen sicherzustellen, dass wenn Personen das Unternehmen verlassen sie Zugriff auf Ressourcen in der Cloud verlieren.

Weitere Informationen:

- [Active Directory-Teamblog zu RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure rollenbasierte Access Control](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Modul

Modul Software Haupt-Sicherheitsanbieter wie Microsoft, Symantec, Trend Micro, McAfee und Kaspersky können Sie mit Azure Ihren virtuellen Computern bösartige Dateien, Adware und ausgefeilte schützen.

Microsoft Antimalware bietet Ihnen die Möglichkeit, einen Agent Modul für PaaS Rollen und virtuellen Computern zu installieren. Basierend auf System Center Endpunkt Protection, bringt dieses Feature lokale Bewährte Sicherheit Technology in der Cloud.

Wir bieten Ihnen auch Tiefe Integration von Trend [Tiefe Sicherheit](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ und [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ Produkte in die Azure-Plattform. DeepSecurity ist eine Antivirensoftware Lösung und SecureCloud ist eine Lösung für die Verschlüsselung. DeepSecurity werden innerhalb von virtuellen Computern, die mit einem Erweiterungsmodell bereitgestellt werden. Mit dem Portal Benutzeroberfläche und PowerShell, können Sie mit DeepSecurity innerhalb der neuen virtuellen Computern, die erstellt wird, wird sind oder vorhandenen virtuellen Computern, die bereits bereitgestellt werden.

Symantec Endpunkt Schutz (SEP) wird ebenfalls auf Azure unterstützt. Über Portal Integration haben Kunden die Möglichkeit, die angeben, dass er SEP innerhalb eines virtuellen Computers verwenden möchten. SEP auf einen völlig neuen virtuellen über das Azure-Portal installiert werden kann oder auf eine vorhandene virtueller Computer mithilfe der PowerShell installiert werden kann.

Weitere Informationen:

- [Bereitstellen von Modul Lösungen auf Azure-virtuellen Computern](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft-Modul für Azure-Cloud-Diensten und virtuellen Computern](../security/azure-security-antimalware.md)
- [So installieren und Konfigurieren von Trend Micro Tiefe Security als auf einen virtuellen Windows-Dienst](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [So installieren und Konfigurieren von Symantec Endpunkt Schutz auf einen Windows-virtuellen](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Neue Modul Optionen zum Schutz von Azure-virtuellen Computern – McAfee Endpunkt Schutz](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung

Azure kombinierte Authentifizierung (MFA) ist eine Methode der Authentifizierung, die erfordert die Verwendung von mehr als eine Überprüfungsmethode und Benutzer anmelden-ins und Transaktionen kritische zweite Sicherheitsebene hinzugefügt. MFA hilft schützen den Zugriff auf Daten und Applikationen während der Besprechung Benutzer bei Bedarf für ein einfacher Vorgang Anmeldung. Es stellt strenge Authentifizierung über eine Reihe von Überprüfungsoptionen – Anruf, Textnachricht oder mobile-app Benachrichtigung oder Überprüfung Code und 3rd Party Angehörigen Token.

Weitere Informationen:

- [Mehrstufige Authentifizierung](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Was ist die kombinierte Authentifizierung Azure?](../multi-factor-authentication/multi-factor-authentication.md)
- [Funktionsweise von Azure kombinierte Authentifizierung](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute können Sie Ihre lokalen Netzwerken in der Microsoft-Cloud über eine dedizierte private Verbindung von einem Connectivity-Anbieter erleichtert zu erweitern. Mit ExpressRoute können Sie Verbindungen zu Microsoft-Cloud-Diensten, wie etwa Microsoft Azure, Office 365 und CRM Online herstellen. Konnektivität kann aus einer n: n-(IP VPN) Netzwerk, einem Punkt Ethernet-Netzwerk oder eine virtuelle Cross-Verbindung über einen Anbieter Konnektivität in einer gemeinsamen Speicherort Einrichtung sein. ExpressRoute Verbindungen gehen Sie nicht über das öffentliche Internet. Dadurch wird die ExpressRoute-Verbindungen mit mehr Zuverlässigkeit, höhere Geschwindigkeit, unteren Wartezeiten und höhere Sicherheit als normalen Verbindungen über das Internet bieten.

Weitere Informationen:

- [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuelle Netzwerkgateways

VPN-Gateways, Azure virtuelle Netzwerkgateways, die Abkürzung werden verwendet, um die Netzwerkverkehr zwischen virtuelle Netzwerke und lokalen Speicherorten zu senden. Sie werden auch zum Senden von Datenverkehr zwischen mehreren virtuellen Netzwerken in Azure (VNet VNet) verwendet.  VPN-Gateways bieten sicheren Cross lokale Konnektivität zwischen Azure und Ihre Infrastruktur.

Weitere Informationen:

- [Informationen zu VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Übersicht über die Sicherheit von Azure Netzwerk](security-network-overview.md)

## <a name="privileged-identity-management"></a>Verwaltung von Berechtigungen Identität

Manchmal müssen Benutzer berechtigten Vorgänge in Azure Ressourcen oder einer anderen Anwendung SaaS ausführen. Dies bedeutet häufig, dass Organisationen müssen sie permanente berechtigten Zugriff in Azure Active Directory (Azure AD) gewähren. Dies ist ein wachsende Sicherheitsrisiko für Ressourcen Cloud gehostet, da Organisationen ausreichend überwacht werden können, was die Benutzer mit Ihren Zugriff auf die Berechtigungen tun.
Darüber hinaus ist ein Benutzerkonto mit Berechtigungen Zugriff gefährdet, konnte eine dieser Verstoß gegen Ihre Sicherheit für insgesamt Cloud auswirken. Azure AD-Berechtigungen Identitätsmanagement können Sie um dieses Risiko zu lösen, indem Sie Herabsetzen der Uhrzeit anzeigen von Berechtigungen und Erhöhen der Sichtbarkeit in Verwendung.  

Berechtigte Identitätsmanagement Konzept das eines temporären Administrators für eine Rolle oder "nur in Time" Administratorzugriff, die ein Benutzer ist, ein Aktivierungsprozess für zugewiesenen Rolle abzuschließen muss. Der Aktivierungsprozess ändert die Zuordnung des Benutzers zu einer Rolle in Azure AD von inaktiv auf aktiv, für eine angegebene Zeit Periode, z. B. 8 Stunden.

Weitere Informationen:

- [Die Identitätsmanagement Azure AD-Berechtigungen](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Erste Schritte mit Azure AD berechtigten Identitätsmanagement](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Schutz der Identität

Schutz Azure Active Directory (AD) bietet eine konsolidierte Ansicht von verdächtigen Aktivitäten und mögliche Sicherheitslücken zum Schutz Ihres Unternehmens. Schutz der Identität erkennt verdächtige Aktivitäten für Benutzer und Berechtigungen (Administrator) Identitäten, basierend auf Signale wie Bruteforce-Angriffen, wegen Anmeldeinformationen, und melden Sie sich-ins unbekannten Orten und infiziert Geräte.

Durch die Bereitstellung von Benachrichtigungen und empfohlene Behebung, hilft Ihnen Schutz der Identität, Risiken in Echtzeit. Es Benutzer Risiko schwere berechnet, und Sie können das Risiko basierende Richtlinien zum Schutz Anwendung zugreifen, zukünftigen Risiken automatisch Hilfe konfigurieren.

Weitere Informationen:

- [Schutz der Azure-Active Directory-Identität](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD- und Identität anzeigen: Identität Schutz Vorschau](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Sicherheitscenter

Azure-Sicherheitscenter hilft Ihnen zu verhindern, erkennen und Beantworten von Risiken und stellt, dass Sie die Transparenz, und steuern, die Sicherheit Ihrer Azure Ressourcen erhöht. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

Sicherheitscenter hilft Ihnen die optimieren und überwachen die Sicherheit Ihrer Azure Ressourcen durch:

- Aktivieren Sie zum Definieren von Richtlinien für Abonnementressourcen Azure-ab Ihres Unternehmens Sicherheit Anforderungen und Anwendungstyp oder Vertraulichkeit der Daten in jedes Abonnement.
- Überwachen des Status der Ihrer Azure-virtuellen Computern, Netzwerke und Applikationen.
- Bereitstellen einer Liste mit Prioritätsstufe von Sicherheitshinweisen, einschließlich Benachrichtigungen von integrierten partnerlösungen, zusammen mit den Informationen, die Sie schnell ermitteln müssen und Vorschläge zur Behebung von Angriffen.

Weitere Informationen:

- [Einführung in Azure-Sicherheitscenter](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
