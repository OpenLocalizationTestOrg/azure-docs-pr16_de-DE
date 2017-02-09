<properties
    pageTitle="Azure AD-verbinden: Integration Ihrer lokalen Identitäten mit Azure Active Directory. | Microsoft Azure"
    description="Azure AD verbinden wird die lokalen Verzeichnissen mit Azure Active Directory integrieren. Dies können Sie eine allgemeine Identität für Office 365, Azure und SaaS Applikationen mit Azure AD integriert bereitstellen."
    keywords="Einführung in Azure AD verbinden, installieren Sie Azure AD verbinden Übersicht, was Azure AD verbinden, ist active Directory"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="billmath"/>

# <a name="integrating-your-on-premises-identities-with-azure-active-directory"></a>Integrieren von Ihrem lokalen Identitäten in Azure Active Directory

Azure AD verbinden wird die lokalen Verzeichnissen mit Azure Active Directory integrieren. Dies können Sie eine allgemeine Identität für Ihre Benutzer für Office 365, Azure und SaaS Applikationen mit Azure AD integriert bereitstellen. In diesem Thema führt Sie durch die Planung, Bereitstellung und Vorgang Schritte. Es ist eine Auflistung von Links zu den Themen im Zusammenhang mit diesem Bereich.

> [AZURE.IMPORTANT] [Azure AD verbinden ist die beste Methode zum Verbinden von Ihrem lokalen Verzeichnis mit Azure AD und Office 365. Dies ist eine großartige Zeit, auf Azure AD-Verbinden von Windows Azure Active Directory-Synchronisierung (DirSync) oder Azure AD synchronisieren zu aktualisieren, wie diese Tools jetzt veraltet sind und Ende des Supports auf 13 April 2017 gelangen.](active-directory-aadconnect-dirsync-deprecated.md)

![Was ist Azure AD-Verbindung herstellen](./media/active-directory-aadconnect/arch.png)

## <a name="why-use-azure-ad-connect"></a>Gründe für die Verwendung Azure AD-verbinden
Integrieren von Ihrem lokalen Verzeichnissen in Azure AD Ihrer Anwender macht produktiver durch Bereitstellen einer gemeinsamen Identität für den Zugriff auf sowohl die Cloud lokalen Ressourcen. Benutzer und Organisationen können die folgenden nutzen:

- Benutzer können mithilfe eine einzelne Identität Zugriff auf lokale Applikationen und cloud-Diensten wie Office 365.

- Einzelne Tool um eine einfache Bereitstellung Erfahrung für Synchronisierung und-Anmeldung bereitzustellen.

- Stellt die neuesten Funktionen für Ihre Szenarios an. Azure AD verbinden ersetzt ältere Versionen von Identität Integration von Tools wie DirSync und Azure AD synchronisieren. Weitere Informationen finden Sie unter [Vergleich der Hybrid Identität Directory Integration](active-directory-hybrid-identity-design-considerations-tools-comparison.md).


### <a name="how-azure-ad-connect-works"></a>Wie Azure AD verbinden funktioniert
Azure Active Directory-Verbindung besteht aus drei primären Komponenten: Die Synchronisierungsdienste, die optionale Active Directory Federation Services-Komponente und die Überwachung Komponente mit dem Namen [Azure AD verbinden Dienststatus](active-directory-aadconnect-health.md).

<center>![Azure AD verbinden Stapel](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

- Synchronisierung – ist diese Komponente verantwortlich für das Erstellen von Benutzern, Gruppen und anderen Objekten. Es ist auch verantwortlich sicherzustellen, dass die Cloud übereinstimmende persönliche Informationen für den lokale Benutzer und Gruppen.
- AD FS - Föderation ist ein optionaler Bestandteil von Azure AD verbinden und kann zum Konfigurieren einer hybridumgebung mit einer lokalen AD FS-Infrastruktur. Dies kann von Organisationen zu komplexen Bereitstellungen Adresse, wie z. B. beitreten zu einer Domäne SSO, Durchsetzung von AD Anmeldung Richtlinie und Smartcard oder 3rd Party MFA verwendet werden.
- Überwachen des Systemzustands - Azure AD verbinden Gesundheit bieten robuste Überwachung und bieten einen zentralen Speicherort im Azure-Portal zu dieser Aktivität anzeigen können. Weitere Informationen finden Sie unter [Azure Active Directory verbinden Dienststatus](active-directory-aadconnect-health.md).

## <a name="install-azure-ad-connect"></a>Verbinden von Azure AD-Installation

Sie können den Download für Azure AD verbinden [Microsoft Download](http://go.microsoft.com/fwlink/?LinkId=615771)Center suchen.


Lösung | Szenario
----- | ----- |
Bevor Sie beginnen - [Hardware und Komponenten](active-directory-aadconnect-prerequisites.md) | <li>Schritte zum Abschließen, bevor Sie beginnen, Azure AD verbinden zu installieren.</li>
[Express-Einstellungen](./connect/active-directory-aadconnect-get-started-express.md) | <li>Wenn Sie eine einzelne Gesamtstruktur AD haben ist dies die empfohlene Option zu verwenden.</li> <li>Benutzer melden Sie sich mit der Synchronisierung von Kennwörtern mit dasselbe Kennwort.</li>
[Benutzerdefinierten Einstellungen](./connect/active-directory-aadconnect-get-started-custom.md) | <li>Verwendet, wenn Sie mehrere Gesamtstrukturen haben. Unterstützt viele lokale [Topologien](active-directory-aadconnect-topologies.md).</li> <li>Passen Sie Ihre Option Anmeldung an, wie ADFS für Föderation oder Verwenden einer 3rd Identitätsanbieter party.</li> <li>Passen Sie die Synchronisierung Features wie filtern und abgeschlossenen writebackvorgängen.</li>
[Upgrade von DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Verwendet, wenn Sie einen vorhandenen Dirsync-Server bereits ausgeführt haben.</li>
[Aktualisieren von Azure AD synchronisieren oder Azure AD verbinden](active-directory-aadconnect-upgrade-previous-version.md)| <li>Es gibt verschiedene Methoden nach Wunsch.</li>


[Nach der Installation](active-directory-aadconnect-whats-next.md) sollten Sie diese überprüfen ist wie erwartet funktionieren und den Benutzern Lizenzen zuweisen.

### <a name="next-steps-to-install-azure-ad-connect"></a>Nächste Schritte zum Installieren von Azure AD verbinden

Thema |  
--------- | ---------
Download Azure AD verbinden | [Download Azure AD verbinden](http://go.microsoft.com/fwlink/?LinkId=615771)
Installieren Sie Express-Einstellungen | [Express-Installation von Azure AD-verbinden](./connect/active-directory-aadconnect-get-started-express.md)
Installieren Sie die angepasste Einstellungen verwenden | [Benutzerdefinierte Installation von Azure AD-verbinden](./connect/active-directory-aadconnect-get-started-custom.md)
Upgrade von DirSync | [Aktualisieren von Azure Active Directory-Synchronisierungstool (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
Nach der installation | [Überprüfen Sie die Installation und Zuweisen von Lizenzen](active-directory-aadconnect-whats-next.md)

### <a name="learn-more-about-install-azure-ad-connect"></a>Weitere Informationen zum Installieren von Azure AD verbinden

Sie möchten [Betrieb](active-directory-aadconnectsync-operations.md) Probleme bereiten. Möglicherweise möchten einen Standby-Server verfügen, sodass Sie einfach liegen können über, wenn eine [Disaster Katastrophe](active-directory-aadconnectsync-operations.md#disaster-recovery). Wenn Sie beabsichtigen, häufig verwendeten Konfiguration Änderungen vornehmen, sollten Sie für einen Server [staging Modus](active-directory-aadconnectsync-operations.md#staging-mode) planen.

Thema |  
--------- | ---------
Unterstützte Topologien | [Topologien für Azure AD verbinden](active-directory-aadconnect-topologies.md)
Entwurfskonzepte | [Azure AD verbinden Entwurfskonzepte](active-directory-aadconnect-design-concepts.md)
Konten für die Installation verwendet | [Weitere Informationen zum Verbinden von Azure AD-Anmeldeinformationen und Berechtigungen](./connect/active-directory-aadconnect-accounts-permissions.md)
Betrieb planen | [Synchronisieren von Azure AD verbinden: Aspekte und Betriebsaufgaben](active-directory-aadconnectsync-operations.md)
Anmeldung Benutzeroptionen | [Azure AD verbinden Anmeldung Benutzeroptionen](active-directory-aadconnect-user-signin.md)

## <a name="configure-sync-features"></a>Synchronisieren von Features konfigurieren
Azure AD verbinden verfügt über verschiedene Features, die Sie optional aktivieren können, oder sind standardmäßig aktiviert. Einige Features möglicherweise manchmal in bestimmten Szenarien und Topologien weitere Konfiguration erforderlich.

[Filterung](active-directory-aadconnectsync-configure-filtering.md) wird verwendet, wenn beschränken, welche Objekte in Azure Active Directory synchronisiert werden soll. Standardmäßig werden alle Benutzer, Kontakte, Gruppen und Windows 10 Computer synchronisiert. Sie können ändern, die Filterung basierend auf Domänen, Organisationseinheiten oder Attributen.

[Synchronisierung von Kennwörtern](active-directory-aadconnectsync-implement-password-synchronization.md) synchronisiert den Kennworthash in Active Directory zu Azure AD. Der Endbenutzer kann verwenden das gleiche Kennwort lokal und in der Cloud, sondern nur verwalten es in eine beliebige Stelle. Da es als die Zertifizierungsstelle Ihrer lokalen Active Directory verwendet, können Sie auch eigene Kennwortrichtlinien.

[Kennwort abgeschlossenen writebackvorgängen](active-directory-passwords-getting-started.md) können Ihre Benutzer ändern und ihr Kennwort in der Cloud zurücksetzen und Ihrem lokalen Kennwortrichtlinien angewendet haben.

[Gerät abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-device-writeback.md) lässt auf einem Gerät registriert in Azure AD, um wieder zu lokalen Active Directory geschrieben werden, sodass sie für bedingten Zugriff verwendet werden kann.

Das Feature [unbeabsichtigte Löschen sperren](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ist standardmäßig aktiviert und zahlreichen löscht zur gleichen Zeit Ihrem Verzeichnis Cloud verhindert. Standardmäßig ermöglicht es 500 Löschvorgänge pro ausführen. Sie können diese Einstellung je nach der Größe Ihrer Organisation ändern.

[Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) ist standardmäßig für express-Einstellungen-Installationen aktiviert und stellt sicher, dass Ihre Azure AD Verbinden immer auf dem neuesten Stand mit der neuesten Version wird.

### <a name="next-steps-to-configure-sync-features"></a>Weitere Schritte zum Synchronisieren Features konfigurieren

Thema |  
--------- | --------- |
Konfigurieren von Filtern | [Synchronisieren von Azure AD verbinden: Konfigurieren von Filtern](active-directory-aadconnectsync-configure-filtering.md)
Synchronisierung von Kennwörtern | [Synchronisieren von Azure AD verbinden: Implementieren der Synchronisierung von Kennwörtern](active-directory-aadconnectsync-implement-password-synchronization.md)
Kennwort abgeschlossenen writebackvorgängen | [Erste Schritte mit der Verwaltung der Kennwörter](active-directory-passwords-getting-started.md)
Gerät abgeschlossenen writebackvorgängen | [Aktivieren der Gerät abgeschlossenen writebackvorgängen in Azure AD-Verbindung herstellen](active-directory-aadconnect-feature-device-writeback.md)
Unbeabsichtigte Löschen sperren | [Synchronisieren von Azure AD verbinden: unbeabsichtigte Löschen sperren](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
Automatische Aktualisierung | [Azure AD-verbinden: Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md)

## <a name="customize-azure-ad-connect-sync"></a>Anpassen von Azure AD verbinden synchronisieren
Azure AD verbinden synchronisieren verfügt über ein Standardkonfiguration, die für die meisten Kunden und Topologien entwickelt vorgesehen ist. Es gibt jedoch einige immer Situationen, in dem die standardmäßige Konfiguration nicht funktioniert und angepasst werden muss. Es wird unterstützt, wenn Sie Änderungen als dokumentierten in diesem Abschnitt und verknüpfte Themen vornehmen.

Wenn Sie nicht mit einer Synchronisierung Suchtopologie gearbeitet haben, bevor Sie die Grundlagen und die in der [technischen Konzepte](active-directory-aadconnectsync-technical-concepts.md)beschriebenen verwendeten Begriffe verstehen beginnen möchten. Verbinden von Azure AD ist die Weiterentwicklung von MIIS 2003, ILM2007 und FIM2010. Auch wenn einige Aktionen identisch sind, weist ebenfalls viele geändert.

Der [Standard-Konfiguration](active-directory-aadconnectsync-understanding-default-configuration.md) wird davon ausgegangen, dass es möglicherweise mehrere Gesamtstrukturen in der Konfiguration. In diesen Topologien kann ein Benutzerobjekt in einer anderen Gesamtstruktur als Kontakt dargestellt werden. Der Benutzer möglicherweise auch ein verknüpftes Postfach in einem anderen Ressourcengesamtstruktur sind. [Benutzer](active-directory-aadconnectsync-understanding-users-and-contacts.md)und Kontakte wird das Verhalten der Standardkonfiguration beschrieben.

Das Konfigurationsmodell synchron heißt [deklarative bereitgestellt](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). [Funktionen](active-directory-aadconnectsync-functions-reference.md) verwenden die erweiterten Attribut Zahlungen Attribut Transformationen express. Sie sehen und untersuchen können die gesamte Konfiguration mithilfe der Tools, die mit Azure AD verbinden stammt. Wenn Sie die Konfiguration Änderungen vornehmen müssen, feststellen Sie, ob den [bewährte Methoden](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) , sodass er besser zu neue Versionen ergreifen ist beschrieben.

### <a name="next-steps-to-customize-azure-ad-connect-sync"></a>Weitere Schritte zum Anpassen von Azure AD verbinden synchronisieren

Thema |  
--------- | ---------
Alle Azure AD verbinden synchronisieren Artikel | [Synchronisieren von Azure AD verbinden](active-directory-aadconnectsync-whatis.md)
Technische Konzepte | [Synchronisieren von Azure AD verbinden: technischen Konzepte](active-directory-aadconnectsync-technical-concepts.md)
Grundlegendes zu der Standard-Konfiguration | [Synchronisieren von Azure AD verbinden: Grundlegendes zu der Standard-Konfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)
Grundlegendes zu Benutzern und Kontakten | [Synchronisieren von Azure AD verbinden: Grundlegendes zu Benutzern und Kontakten](active-directory-aadconnectsync-understanding-users-and-contacts.md)
Deklarative bereitgestellt. | [Azure AD verbinden synchronisieren: Grundlegendes zu deklarative Bereitstellung Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
Ändern der Standard-Konfiguration | [Bewährte Methoden zum Ändern der Standard-Konfigurations](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)

## <a name="configure-federation-features"></a>Föderationsfeatures konfigurieren
ADFS kann konfiguriert werden, um [mehrere Domänen](active-directory-aadconnect-multiple-domains.md)zu unterstützen. Beispielsweise müssen Sie mehrere oberste Domänen, die Sie für die Föderation verwenden müssen.

Wenn der ADFS-Server nicht so konfiguriert wurde, dass automatisch aktualisiert Zertifikate aus Azure Active Directory oder verwenden Sie eine nicht-ADFS-Lösung, und Sie werden benachrichtigt, wenn Sie zum [Aktualisieren von Zertifikaten](active-directory-aadconnect-o365-certs.md)haben.

### <a name="next-steps-to-configure-federation-features"></a>Nächste Schritte Föderationsfeatures konfigurieren

Thema |  
--------- | ---------
Alle AD FS-Artikel | [Azure AD verbinden und Föderation](active-directory-aadconnectfed-whatis.md)
Konfigurieren von ADFS mit Unterdomänen | [Für eine Föderation mit Azure AD Unterstützung für mehrere Domänen](active-directory-aadconnect-multiple-domains.md)
Verwalten von AD FS-farm | [AD FS-Verwaltung und bei der Anpassung mit Azure AD-verbinden](active-directory-aadconnect-federation-management.md)
Manuelles Aktualisieren Föderation Zertifikate | [Föderation Zertifikate für Office 365 und Azure AD-erneuern](active-directory-aadconnect-o365-certs.md)

## <a name="more-information-and-references"></a>Weitere Informationen und Verweise

Thema |  
--------- | --------- |
Versionsverlauf | [Versionsverlauf](active-directory-aadconnect-version-history.md)
Vergleichen von DirSync, Azure ADSync und Azure AD-verbinden | [Vergleich der Directory-integration](active-directory-hybrid-identity-design-considerations-tools-comparison.md)
Nicht-ADFS-Kompatibilitätsliste zur Azure AD- | [Azure Active Directory Federation Kompatibilitätsliste](active-directory-aadconnect-federation-compatibility.md)
Attribute synchronisiert | [Attribute synchronisiert](active-directory-aadconnectsync-attributes-synchronized.md)
Überwachung mit Azure AD verbinden Dienststatus | [Azure AD verbinden Dienststatus](active-directory-aadconnect-health.md)
Häufig gestellte Fragen | [Azure AD verbinden häufig gestellte Fragen](active-directory-aadconnect-faq.md)


**Zusätzliche Ressourcen**


Zündschnur zu legen 2015 Präsentation auf Ihrem lokalen Verzeichnissen in der Cloud zu erweitern.

>[AZURE.VIDEO microsoft-ignite-2015-extending-on-premises-directories-to-the-cloud-made-easy-with-azure-active-directory-connect]
