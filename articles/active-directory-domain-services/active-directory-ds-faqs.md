<properties
    pageTitle="Häufig gestellte Fragen - Azure-Active Directory-Domänendiensten | Microsoft Azure"
    description="Häufig gestellte Fragen zur Azure Active Directory-Domänendiensten"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure-Active Directory-Domänendiensten: Häufig gestellte Fragen (FAQs)

Auf dieser Seite finden Sie Antworten auf häufig gestellte Fragen zur Azure Active Directory-Domänendienste. Aktivieren Sie behalten wieder nach Updates suchen.

### <a name="troubleshooting-guide"></a>Leitfadens zur Problembehandlung
Unsere [Problembehandlung Leitfaden](active-directory-ds-troubleshooting.md) für Lösungen für häufig auftretende Probleme beim Konfigurieren oder Verwalten von Azure Active Directory-Domänendiensten finden Sie unter.


### <a name="configuration"></a>Konfiguration

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Können mehrere Domänen für ein einzelnes erstellen Azure AD-Verzeichnis?
Nein. Sie können nur eine einzelne Domäne durch Azure Active Directory-Domänendiensten für ein einzelnes unterstützte erstellen Azure AD-Verzeichnis.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kann ich Azure Active Directory-Domänendienste in einem Ressourcenmanager Azure-virtuellen Netzwerk aktivieren?
Nein. Azure-Active Directory-Domänendiensten können nur in einem klassischen Azure virtuelle Netzwerk aktiviert sein. Sie können das klassische virtuelle Netzwerk zu einem Ressourcenmanager virtuellen Netzwerk virtuelles Netzwerk peering die verwaltete Domäne in einem Ressourcenmanager virtuelle Netzwerk verbinden.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kann ich Azure Active Directory-Domänendiensten verfügbar in mehrere virtuelle Netzwerke innerhalb des Abonnements machen?
Der Dienst selbst unterstützt dieses Szenario nicht direkt. Azure-Active Directory-Domänendiensten wird jeweils nur in einem virtuellen Netzwerk zur Verfügung. Allerdings können Sie die Verbindungen zwischen mehreren virtuelle Netzwerke Azure Active Directory-Domänendiensten an andere virtuelle Netzwerke verfügbar machen konfigurieren. Dieser Artikel beschreibt, wie Sie [virtuelle Netzwerke in Azure herstellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)können.

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kann ich Azure Active Directory-Domänendiensten mithilfe der PowerShell aktivieren?
PowerShell/automatisierte Bereitstellung von Azure Active Directory-Domänendiensten ist derzeit nicht verfügbar.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Steht Azure-Active Directory-Domänendiensten im neuen Azure-Portal?
Nein. Azure-Active Directory-Domänendiensten können nur in der [Azure klassischen Portal](https://manage.windowsazure.com)konfiguriert sein. Wir erwarten Sie Unterstützung für das [Azure-Portal](https://portal.azure.com) zukünftig zu erweitern.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kann ich eine Azure-Active Directory-Domänendiensten verwalteten Domäne Domänencontroller hinzufügen?
Nein. Die Domäne, die von Azure Active Directory-Domänendiensten bereitgestellt ist eine verwaltete Domäne. Sie nicht benötigen, bereitstellen, konfigurieren oder verwalten andernfalls Domänencontroller für diese Domäne - werden diese Aktivitäten Management als Dienst von Microsoft bereitgestellt. Sie können keine daher zusätzliche Domain Controller (Lese-und Schreibzugriff oder schreibgeschützt) für die verwaltete Domäne hinzufügen.

### <a name="administration-and-operations"></a>Verwaltung und Betrieb

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kann ich eine Verbindung mit dem Domänencontroller für meine verwalteten Domäne mithilfe von Remotedesktop herstellen?
Nein. Sie verfügen nicht über die Berechtigungen für die Verbindung zu Domänencontroller für die Domäne, die über Remote Desktop verwalteten. Mitglieder der Gruppe 'AAD DC Administratoren' können die verwaltete Domäne mit dem AD-Verwaltungstools wie das Active Directory Administration Center (ADAC) oder AD-PowerShell verwalten. Diese Tools installiert sind, verwenden das Feature 'Remoteserver-Verwaltungstools' auf einem WindowsServer der verwalteten Domäne hinzugefügt.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ich habe den Azure-Active Directory-Domänendiensten aktiviert haben. Welche Benutzerkonto verwende ich Domäne Verknüpfung Maschinen in dieser Domäne?
Benutzer aus, die Sie der administrativen Gruppe (beispielsweise "AAD DC Administratoren") hinzugefügt haben, können Join-Domäne Autos. Darüber hinaus werden Benutzer in dieser Gruppe remote desktop-Zugriff auf Computern erteilt, die der Domäne hinzugefügt wurde.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Kann ich die Domänenadministratorrechte für die Domäne, die von Azure Active Directory-Domänendiensten bereitgestellt haben?
Nein. Sie werden nicht über Administratorrechte in der verwalteten Domäne gewährt. Sowohl ' Domäne Administrator' und ' Unternehmensadministrator ' Berechtigungen sind nicht verfügbar für Ihre Verwendung innerhalb der Domäne. Vorhandenen Domänenadministrator oder Dienstadministrator-Gruppen in Ihrem Verzeichnis Azure AD-Enterprise sind auch keine Domäne/Enterprise Administratorberechtigungen auf die Domäne gewährt.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Kann ich mithilfe von LDAP- oder andere AD-Verwaltung auf Domänen, die von Azure Active Directory-Domänendiensten bereitgestellt Gruppenmitgliedschaft ändern?
Nein. Klicken Sie auf Domänen, die vom Azure Active Directory-Domänendiensten bearbeitete können Gruppenmitgliedschaft geändert werden. Dasselbe gilt für Benutzerattribute. Sie können jedoch in Azure AD- oder in der lokalen Domäne Gruppenmitgliedschaften oder Benutzerattribute ändern. Solche Änderungen werden automatisch mit Azure Active Directory-Domänendiensten synchronisiert.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Kann das Schema der Domäne von Azure Active Directory-Domänendiensten bereitgestellten?
Nein. Microsoft wird das Schema der verwalteten Domäne verwaltet. Schema Erweiterungen werden von Azure Active Directory-Domänendiensten nicht unterstützt.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kann ich ändern oder Hinzufügen von DNS-Einträge in meiner Domäne verwalteten?
Ja. Benutzer, die Gruppe 'AAD DC Administratoren' angehören, werden ' DNS-Administrator' Berechtigungen zum Ändern der DNS-Einträge in der verwalteten Domäne erteilt. Diese Benutzer können die DNS-Manager-Konsole auf einem Computer unter Windows Server der verwalteten Domäne, Sie DNS verwalten. Wenn Sie die DNS-Manager-Konsole verwenden, installieren Sie 'DNS-Server-Tools', die Teil der optionales Feature 'Remoteserver-Verwaltungstools' auf dem Server ist. Weitere Informationen zu [Dienstprogrammen zum Verwalten, für die Überwachung und Problembehandlung bei DNS](https://technet.microsoft.com/library/cc753579.aspx) steht auf TechNet.


### <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Ist, dass Azure AD-Domäne einen kostenpflichtigen Dienst Services?
Ja. Weitere Informationen finden Sie unter die [Preise Seite](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Gibt es eine kostenlose Testversion für den Dienst?
Dieser Dienst ist in der kostenlosen Testversion für Azure enthalten. Sie können für eine [kostenlose Testversion von einen Monat von Azure](https://azure.microsoft.com/pricing/free-trial/)anmelden.

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Erhalte ich Azure Active Directory-Domänendiensten als Teil der Enterprise Mobilität Suite (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Benötige ich Azure AD Premium Azure Active Directory-Domänendiensten verwenden?
Nein. Azure-Active Directory-Domänendiensten ist eine nutzungsbasierte Azure Service und nicht Teil EMS ist. Azure Active Directory-Domänendiensten stehen für alle Editionen von Azure AD (kostenlose, einfache und, Premium) und stündlich, je nach Verwendung in Rechnung gestellt werden.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Welche Azure Regionen ist der Dienst in verfügbar?
Schlagen Sie in der Seite [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services/) , um eine Liste der Stelle, an der Azure-Active Directory-Domänendiensten steht Azure Regionen anzuzeigen.
