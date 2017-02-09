<properties
    pageTitle="Index für Anwendungsverwaltung in Azure-Active Directory-Artikel | Microsoft Azure"
    description="Erfahren Sie, wie Sie das Ablaufdatum für Ihre Zertifikate Föderation anpassen und So erneuern Sie Zertifikate, die bald abläuft."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="markvi"/>

#<a name="article-index-for-application-management-in-azure-active-directory"></a>Artikel Index für Anwendungsverwaltung in Azure-Active Directory

Diese Seite enthält eine umfassende Liste von jedem Dokument geschrieben zu den verschiedenen Anwendung miteinander verwandt in Azure Active Directory (Azure AD).

Es gibt eine kurze Einführung in jeder Featurebereich der wichtigsten sowie Anleitungen welche Beiträge zu lesen, je nachdem welche Informationen gefunden haben. 

##<a name="overview-articles"></a>Artikel (Übersicht)

Die folgenden Artikel sind gute Ausgangspunkte für diejenigen, die eine kurze Beschreibung der Azure AD-Anwendung Verwaltungsfunktionen einfach soll.

| Leitfaden für Artikel |   |
| :---: | --- |
| Eine Einführung in die Anwendung Management Probleme, die Azure AD löst | [Verwalten von Applications with Azure-Active Directory (AD)](active-directory-enable-sso-scenario.md) |
| Eine Übersicht der verschiedenen Features in Azure AD im Zusammenhang mit der Aktivierung einmaligen Anmeldens definieren, wer auf apps zugreifen kann, und wie Benutzer apps starten | [Zugriff auf die Anwendung und Single Sign on in Azure-Active Directory](active-directory-appssoaccess-whatis.md) |
| Einen Blick auf die verschiedenen Schritte beim Integrieren von apps in Ihrer Azure AD | [Integrieren von Applications Azure-Active Directory](active-directory-integrating-applications-getting-started.md)<br /><br />[Einmaliges Anmelden an Apps SaaS aktivieren](active-directory-sso-integrate-saas-apps.md)<br /><br />[Verwalten des Zugriffs auf Apps](active-directory-managing-access-to-apps.md) |
| Eine Erklärung der wie apps im Azure AD dargestellt werden | [Wie und warum Applikationen Azure AD hinzugefügt werden](active-directory-how-applications-are-added.md) |

##<a name="troubleshooting-articles"></a>Artikel zur Problembehandlung

Dieser Abschnitt enthält schnellen Zugriff auf relevante Themen zur Problembehandlung. Weitere Informationen zu jeder Featurebereich finden Sie auf den Rest der auf dieser Seite.

| Feature- |   |
| :---: | --- |
| Partnerverbundkontakte einmaliges Anmelden | [Behandeln von SAML-basierten einmaliges Anmelden](active-directory-saml-debugging.md) |
| Kennwort-basierten einmaliges Anmelden | [Problembehandlung bei der Access-Systemsteuerung-Erweiterung für InternetExplorer](active-directory-saas-ie-troubleshooting.md) |
| Anwendungsproxy | [App-Proxy Handbuch zur Problembehandlung](active-directory-application-proxy-troubleshoot.md) |
| Einmaliges Anmelden zwischen auf Prem AD und Azure AD- | [Problembehandlung bei der Synchronisierung von Kennwörtern](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshooting-password-synchronization)<br /><br />[Problembehandlung bei Kennwort abgeschlossenen Writebackvorgängen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) | 
| Dynamic Gruppenmitgliedschaft | [Problembehandlung bei Dynamic Gruppenmitgliedschaft](active-directory-accessmanagement-troubleshooting.md) |

##<a name="single-sign-on-sso"></a>Einmaliges Anmelden (SSO)

###<a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Partnerverbundkontakte einmaliges Anmelden: Melden Sie sich bei vielen apps mithilfe einer Identität

Einmaliges Anmelden kann Benutzer auf eine Vielzahl von apps und Diensten über nur einen Satz von Anmeldeinformationen zugreifen. Föderation ist eine Methode, die über die Sie einmaliges Anmelden aktivieren können. Wenn Benutzer versuchen, melden Sie sich bei partnerverbundkontakte apps, diese werden in ihrer Organisation offizielle Anmeldeseite von Azure Active Directory gerendert umgeleitet erhalten, und klicken Sie dann wieder bei der app nach der erfolgreichen Authentifizierung umgeleitet werden.

| Artikel Leitfaden |   |
| :---: | --- |
| Eine Einführung in Föderation und andere Typen von einmaliges Anmelden | [Einmaliges Anmelden mit Azure AD](active-directory-appssoaccess-whatis.md) |
| Tausende von SaaS apps, die vorab mit Azure AD mit integrierten sind vereinfachtes einzelnen anmelden Konfigurationsschritte | [Erste Schritte mit der Anwendung Azure AD-Katalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Vollständige Liste der vordefinierten integrierte Apps, die Unterstützung des Verbunds](http://aka.ms/aadfederatedapps)<br /><br />[Wie Sie Ihre App zur Azure AD App Galerie hinzufügen](active-directory-app-gallery-listing.md) |
| Mehr als 150 app Lernprogramme zum Konfigurieren der einzelnen Anmeldung für apps, wie z. B. [Vertrieb](active-directory-saas-salesforce-tutorial.md), [ServiceNow](active-directory-saas-servicenow-tutorial.md), [Google Apps](active-directory-saas-google-apps-tutorial.md), [Arbeitstag](active-directory-saas-workday-tutorial.md)und viele weitere | [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md) |
| So manuell einrichten und Anpassen der einzelnen Konfigurations anmelden | [Wie zu konfigurieren Partnerbenutzern einmaliges Anmelden an Apps, die nicht in der Azure Active Directory-Anwendung Galerie sind](active-directory-saas-custom-apps.md)<br /><br />[Wie Sie in der SAML-Token für integrierte Apps ausgestellt Ansprüche anpassen](active-directory-saml-claims-customization.md) |
| Problembehandlung bei der partnerverbundkontakte apps, die SAML-Protokoll verwenden | [Behandeln von SAML-basierten einmaliges Anmelden](active-directory-saml-debugging.md) |
| Zum Konfigurieren Ihrer app Zertifikatsname Ablaufdatum, und wie Sie Ihre Zertifikate erneuern | [Verwalten von Zertifikaten für Partnerverbundkontakte einmaliges Anmelden in Azure-Active Directory](active-directory-sso-certs.md) |

Partnerverbundkontakte einmaliges Anmelden steht für alle Editionen von Azure AD für bis zu zehn apps pro Benutzer. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) unterstützt unbegrenzte Applications. Wenn Ihre Organisation [Azure AD einfacher](https://azure.microsoft.com/pricing/details/active-directory/) oder [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/)hat, können Sie [Gruppen zuweisen des Zugriffs auf partnerverbundkontakte Anwendungen verwenden](#managing-access-to-applications).

###<a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Kennwort-basierten einmaliges Anmelden: gemeinsame Nutzung von Konten und SSO für apps nicht Partnersuche

Um einmaliges Anmelden auf Anwendungen zu aktivieren, die Föderation nicht unterstützen, bietet Azure AD Kennwort Verwaltungsfunktionen, die Kennwörter SaaS apps sicher zu speichern und automatisch Benutzer an diese apps signieren können. Sie können einfach Verteilen von Anmeldeinformationen für den neu erstellten Konten und Team Konten für mehrere Personen freigeben. Benutzer müssen nicht unbedingt die Anmeldeinformationen den Konten Informationen, die ihnen Zugriff auf erteilt haben.

| Leitfaden für Artikel |   |
| :---: | --- |
| Einführung in die wie Kennwörtern basierende SSO funktioniert und eine kurze Übersicht über die technischen | [Kennwort-basierten einmaliges Anmelden mit Azure AD](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| Eine Zusammenfassung der Szenarien, die im Zusammenhang mit der Firma freigeben, und wie diese Probleme behoben werden, indem Azure AD | [Freigeben von Konten für Azure AD](active-directory-sharing-accounts.md) |
| Automatisches Ändern des Kennworts für bestimmte apps in regelmäßigen Abständen | [Automatisierte Kennwort Rollover (Preview)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Bereitstellung und Problembehandlung Handbücher für die Internet Explorer-Version von der Azure AD-Kennwort-Erweiterung | [Wie Sie die Access-Systemsteuerung-Erweiterung für Internet Explorer mithilfe von Gruppenrichtlinien bereitstellen](active-directory-saas-ie-group-policy.md)<br /><br />[Problembehandlung bei der Access-Systemsteuerung-Erweiterung für InternetExplorer](active-directory-saas-ie-troubleshooting.md) |

Kennwort-basierten einmaliges Anmelden steht für alle Editionen von Azure AD für bis zu zehn apps pro Benutzer. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) unterstützt unbegrenzte Applications. Wenn Ihre Organisation [Azure AD einfacher](https://azure.microsoft.com/pricing/details/active-directory/) oder [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/)hat, können Sie [Gruppen zuweisen des Zugriffs auf Anwendungen verwenden](#managing-access-to-applications). Automatisierte Kennwort Rollover ist ein Feature [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) .

###<a name="app-proxy-single-sign-on-and-remote-access-to-on-premises-applications"></a>App-Proxy: Einzelne anmelden und remote Zugriff auf lokale Applikationen

Wenn Sie Applications in Ihrem privaten Netzwerk, die von Benutzern und Geräten außerhalb des Netzwerks zugegriffen werden müssen haben, können Sie Azure AD-Anwendungsproxy sicheren remote Zugriff auf diese apps aktivieren verwenden.

| Leitfaden für Artikel |   |
| :---: | --- |
| Übersicht über Azure AD-Anwendungsproxy und deren Funktionsweise | [Bereitstellen von sicheren Remotezugriff auf lokale Applikationen](active-directory-application-proxy-get-started.md) |
| Lernprogramme Anwendungsproxy konfigurieren und wie Sie Ihre erste Anwendung veröffentlichen | [So richten Sie Azure AD-App-Proxy](active-directory-application-proxy-enable.md)<br /><br />[So Hintergrundinstallation den App Proxy-Verbinder](active-directory-application-proxy-silent-installation.md)<br /><br />[Zum Veröffentlichen von Applications mithilfe der App-Proxy](active-directory-application-proxy-publish.md)<br /><br />[Wie Sie Ihren eigenen Domänennamen verwenden.](active-directory-application-proxy-custom-domains.md) |
| So aktivieren Sie einzelne anmelden und bedingten Zugriff für apps veröffentlicht mit Proxy-App | [Einmaliges Anmelden mit Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)<br /><br />[Bedingte Zugriff und Anwendung Proxyservers](active-directory-application-proxy-conditional-access.md) |
| Hilfe zur Anwendungsproxy für die folgenden Szenarien verwenden. | [Wie systemeigene Clientanwendungen unterstützt](active-directory-application-proxy-native-client.md)<br /><br />[So Ansprüche unterstützende Applikationen unterstützen.](active-directory-application-proxy-claims-aware-apps.md)<br /><br />[Wie zur Unterstützung von Applications in separaten Netzwerken und Speicherorte veröffentlicht](active-directory-application-proxy-connectors.md) |
| Problembehandlung bei der Anwendungsproxy | [App-Proxy Handbuch zur Problembehandlung](active-directory-application-proxy-troubleshoot.md) |

Anwendungsproxy steht für alle Editionen von Azure AD für bis zu zehn apps pro Benutzer. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) unterstützt unbegrenzte Applications. Wenn Ihre Organisation [Azure AD einfacher](https://azure.microsoft.com/pricing/details/active-directory/) oder [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/)hat, können Sie [Gruppen zuweisen des Zugriffs auf Anwendungen verwenden](#managing-access-to-applications).

[Azure-Active Directory-Domänendiensten](../active-directory-domain-services/active-directory-ds-overview.md), interessiert möglicherweise auch, die Sie Ihrem lokalen Applications in Azure und dabei weiterhin auf die Anforderungen Identität diese Applikationen migrieren können.

###<a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Aktivieren der einmaligen Anmeldens zwischen Azure AD- und lokalen AD

Wenn Ihre Organisation ein Windows Server Active Directory lokal zusammen mit Ihrer Azure Active Directory in der Cloud, verwaltet, und Sie es wahrscheinlich einmaliges Anmelden zwischen den beiden Systemen aktivieren möchten. Azure AD verbinden (das Tool, das diese beiden Systeme zusammen integriert) bietet mehrere Optionen zum Einrichten von einmaliges Anmelden: Aktivieren Sie die Synchronisierung von Kennwörtern oder Föderation mit ADFS oder einem anderen Anbieter für Föderation herzustellen.

| Leitfaden für Artikel |   |
| :---: | --- |
| Eine Übersicht über die einzelnen Optionen anmelden angeboten in Azure AD verbinden sowie Informationen zum Verwalten von hybridumgebungen | [Melden Sie sich zu den Optionen in Azure AD Benutzer verbinden](active-directory-aadconnect-user-signin.md) |
| Allgemeine Anleitung für die Verwaltung von Umgebungen mit beide lokalen Active Directory und Azure Active Directory | [Gibt Azure AD-Hybrid Identität](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Integrieren von Ihrem lokalen Identitäten in Azure-Active Directory](active-directory-aadconnect.md) |
| Verwenden zum Aktivieren von SSO Kennwort synchronisieren Anleitungen | [Synchronisierung von Kennwörtern implementieren mit Azure AD-verbinden](active-directory-aadconnectsync-implement-password-synchronization.md)<br /><br />[Problembehandlung bei der Synchronisierung von Kennwörtern](https://support.microsoft.com/en-us/kb/2855271) |
| Anleitung für den Einsatz von Kennwort abgeschlossenen Writebackvorgängen zum Aktivieren von SSO | [Erste Schritte mit Password Management in Azure Active Directory](active-directory-passwords-getting-started.md)<br /><br />[Behandeln von Problemen mit Kennwort abgeschlossenen Writebackvorgängen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Hinweise zur Verwendung von Drittanbieter-Identitätsanbieter zum Aktivieren von SSO | [Liste der kompatiblen Drittanbieter-Identitätsanbieter, die verwendet werden können, aktivieren Sie einmaliges Anmelden](https://aka.ms/ssoproviders) | 
| Wie können Benutzer Windows 10 Vorteile des einmaligen Anmeldens über Azure AD teilnehmen nutzen zu können | [Erweitern der Cloud-Funktionen, die auf Windows teilnehmen an 10 Geräte über Azure-Active Directory](active-directory-azureadjoin-overview.md) |

Verbinden von Azure AD steht für [alle Editionen von Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD Self-Service Kennwort zurücksetzen steht für [Azure AD Standard-](https://azure.microsoft.com/pricing/details/active-directory/) und [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Kennwort abgeschlossenen Writebackvorgängen auf Prem AD ein Feature [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) befindet. 

###<a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Bedingte Access: Erzwingen Sie zusätzliche Sicherheit Anforderungen für apps mit hohem Risiko

Sobald Sie einen einzelnen melden Sie sich für den Zugriff auf Ihre apps und Ressourcen eingerichtet haben, können Sie weiteren Applications vertrauliche sichern, indem bestimmte Sicherheit Anforderungen auf jeder Anmeldung bei dieser app erzwingen. Beispielsweise können Sie Azure AD zu Demand, dass alle Zugriff auf eine bestimmte app immer mehrstufige Authentifizierung benötigen, unabhängig davon, ob die app von Natur aus dieser Funktionen unterstützt. Allgemeine ein weiteres Beispiel für bedingte Access ist zu erfordern, dass Benutzer mit der Organisation vertrauenswürdigen Netzwerk verbunden sein, um eine besonders vertrauliche Anwendung zugreifen.

| Leitfaden für Artikel |   |
| :---: | --- |
| Eine Einführung in die Funktionen bedingten Zugriff über Azure AD angeboten Intune und Office 365 | [Verwalten von Risiken mit bedingten Zugriff](active-directory-conditional-access.md) |
| So aktivieren Sie bedingte Zugriff für die folgenden Arten von Ressourcen | [Bedingte Zugriff für SaaS-Apps](active-directory-conditional-access-azuread-connected-apps.md)<br /><br />[Bedingte Zugriff für Office 365-Diensten](active-directory-conditional-access-device-policies.md)<br /><br />[Bedingte Zugriff für lokale Applikationen](active-directory-conditional-access-on-premises-setup.md)<br /><br />[Für lokale Applikationen bedingten Zugriff über einen Azure AD App Proxyserver veröffentlicht](active-directory-application-proxy-conditional-access.md) |
| So Geräte mit Azure Active Directory registrieren, um bedingte Access Gerät basierende Richtlinien aktivieren | [Übersicht der Registrierung von Azure-Active Directory-Gerät](active-directory-conditional-access-device-registration-overview.md)<br /><br />[So aktivieren Sie automatische Gerät Registrierung für die Domäne beigetreten Windows-Geräten](active-directory-conditional-access-automatic-device-registration.md)<br />– [Schritte für Windows 8.1-Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)<br />– [Schritte für Windows 7-Geräten](active-directory-conditional-access-automatic-device-registration-windows7.md) |
| Wie die Android Version der app Azure Authentifizierung für Richtlinien betreffen kombinierte Authentifizierung verwendet werden soll. | [Azure Authentifizierung für Android](active-directory-conditional-access-azure-authenticator-app.md) |

Bedingte Zugriff ist ein Feature [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) .


##<a name="apps--azure-ad"></a>Apps und Azure AD-

###<a name="cloud-app-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud-App-Suche: Suchen Sie, welche apps SaaS in Ihrer Organisation verwendet werden

Cloud-App Discovery hilft IT-Abteilung erfahren Sie, welche apps SaaS in der gesamten Organisation verwendet werden. Es kann app Verwendung messen und Beliebtheit, sodass die IT feststellen kann, welche apps, die am häufigsten, nicht Dienstleistung übernommene unter IT-Steuerung und Integration mit Azure AD.

| Leitfaden für Artikel |   |
| :---: | --- |
| Eine allgemeine Übersicht über die Funktionsweise | [Suchen nach unbestätigter Cloudanwendungen mit der Cloud-App-Suche](active-directory-cloudappdiscovery-whatis.md) |
| Sich ausführlicher, mit Antworten auf Fragen auf Datenschutz Funktionsweise | [Sicherheit und Datenschutz Aspekte](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) |
| Häufig gestellte Fragen | [Häufig gestellte Fragen zur Ermittlung der Cloud-App](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |
| Für die Bereitstellung von Cloud App Discovery Lernprogramme | [Gruppenrichtlinie Bereitstellungshandbuch](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)<br /><br />[Bereitstellungshandbuch für System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)<br /><br />[Klicken Sie auf Proxy-Servern mit benutzerdefinierte Ports installieren](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| Das Änderungsprotokoll nach Updates suchen in der Cloud App Discovery-agent | [Änderungsprotokoll](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx) |

Cloud-App Discovery ist eine [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) -Funktion.

###<a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automatisches Bereitstellen und Entziehen von Benutzerkonten in SaaS-apps

Automatisierung der Erstellung, Wartung, und Entfernen von Benutzeridentitäten in SaaS Clientanwendungen wie Dropbox, Vertrieb, ServiceNow und vieles mehr. Abgleichen und Synchronisieren von vorhandenen Identitäten zwischen Azure AD Ihren SaaS apps, und Steuern des Zugriffs mit Konten automatisch zu deaktivieren, wenn Benutzer die Organisation verlassen.

| Leitfaden für Artikel |   |
| :---: | --- |
| Erfahren Sie mehr über die Funktionsweise und Antworten auf häufig gestellte Fragen | [Automatisieren von Benutzer bereitgestellt und Entfernung zu SaaS Apps](active-directory-saas-app-provisioning.md) |
| Konfigurieren, wie Informationen zwischen Azure AD zugeordnet sind und Ihre SaaS-app | [Anpassen von Zuordnungen Attribut](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Schreiben von Ausdrücken für Zuordnungen Attribut](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| So aktivieren Sie automatisierten Bereitstellung in eine beliebige app, die das SCIM-Protokoll unterstützt | [Einrichten von automatisierte Benutzers bei einem beliebigen SCIM-Enabled App Bereitstellung](active-directory-scim-provisioning.md) |
| Erhalten einer Benachrichtigung der Bereitstellung von Fehlern | [Bereitstellung von Benachrichtigungen](active-directory-saas-account-provisioning-notifications.md) |
| Einschränken, wer zur Anwendung auf Grundlage ihrer Attributwerte bereitgestellt wird | [Festlegen des Gültigkeitsbereichs von Filtern](active-directory-saas-scoping-filters.md) |

Automatisches Benutzer bereitgestellt ist für alle Editionen von Azure AD für bis zu zehn apps pro Benutzer verfügbar. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) unterstützt unbegrenzte Applications. Wenn Ihre Organisation [Azure AD einfacher](https://azure.microsoft.com/pricing/details/active-directory/) oder [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/)hat, können Sie [Gruppen zum Verwalten der Benutzer nach der Bereitstellung abrufen verwenden](#managing-access-to-applications).

###<a name="building-applications-that-integrate-with-azure-ad"></a>Baustein-Anwendungen, die mit Azure AD integriert werden soll.

Wenn Ihre Organisation entwickelt oder Verwalten von Line-of-Business (LoB) Applications oder wenn Sie eine app-Entwickler mit Kunden, die Azure Active Directory verwenden befinden, die folgenden Lernprogramme hilft integrieren Sie Ihre Applikationen mit Azure AD an. 

| Leitfaden für Artikel |   |
| :---: | --- |
| Leitfaden für IT-Profis und Entwickler zur Integration von apps mit Azure AD | [IT Pro Leitfaden für die Entwicklung von Applications für Azure AD](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[Des Entwicklers Leitfaden für Azure-Active Directory](active-directory-developers-guide.md) |
| Wie können zur Anwendung Lieferanten ihre apps im Azure AD App Katalog hinzufügen | [Auflisten von Ihrer Anwendung in der Azure Active Directory-Anwendung Galerie](active-directory-app-gallery-listing) |
| Zum Verwalten des Zugriffs auf entwickeltes Azure Active Directory mithilfe von applications | [Wie Sie Benutzer Zuordnung für Applikationen entwickeltes aktivieren](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Zuweisen von Benutzern zu Ihrer Anwendung](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Zuweisen der Gruppe zu Ihrer Anwendung](active-directory-applications-guiding-developers-assigning-groups.md) |

Wenn Sie einen Consumer zugänglichen Anwendungen entwickeln, können Sie möglicherweise interessiert [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) verwenden, damit keine eigene Identität System zum Verwalten Ihrer Benutzer zu entwickeln. [Erfahren Sie mehr](../active-directory-b2c/active-directory-b2c-overview.md).


##<a name="managing-access-to-applications"></a>Verwalten des Zugriffs auf Anwendungen

###<a name="using-groups-and-self-service-to-manage-who-has-access-to-which-apps"></a>Verwenden von Gruppen und Self-Service verwalten, die auf welche apps zugreifen

Um Hilfe Sie verwalten, wer Zugriff auf welche Ressourcen sein soll, können mit Azure Active Directory bei Verwendung von Gruppen Zuordnungen und Berechtigungen festlegen. IT zum Aktivieren von Self-service-Funktionen, sodass die Benutzer einfach über die Berechtigung anfordern können bei Bedarf auswählen können.

| Leitfaden für Artikel |   |
| :---: | --- |
| Übersicht über Azure AD-Access Verwaltungsfunktionen | [Einführung in das Verwalten des Zugriffs auf Apps](active-directory-managing-access-to-apps.md)<br /><br />[Funktionsweise von Access Management in Azure AD](active-directory-manage-groups.md)<br /><br />[Wie Sie Gruppen zum Verwalten des Zugriffs auf SaaS Applikationen](active-directory-accessmanagement-group-saasapps.md) |
| Aktivieren der Self-service-Verwaltung von apps und Gruppen | [Self-Service Anwendungsverwaltung](active-directory-self-service-application-access.md)<br /><br />[Verwaltung von Self-Service-Gruppen](active-directory-accessmanagement-self-service-group-management.md) |
| Anweisungen zum Einrichten Ihrer Gruppen in Azure Active Directory | [So erstellen Sie Sicherheitsgruppen](active-directory-accessmanagement-manage-groups.md)<br /><br />[So bestimmen Sie Besitzer für eine Gruppe](active-directory-accessmanagement-managing-group-owners.md)<br /><br />[So verwenden Sie die Gruppe "Alle Benutzer"](active-directory-accessmanagement-dedicated-groups.md) |
| Verwenden Sie dynamische Gruppen automatisch Gruppenmitgliedschaft mithilfe von Regeln für die Mitgliedschaft Attribut-basierte gefüllt wird | [Dynamic Gruppenmitgliedschaft: Erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md)<br /><br />[Problembehandlung bei Dynamic Gruppenmitgliedschaft](active-directory-accessmanagement-troubleshooting.md) |

Gruppe basierenden Anwendung Access Management steht für [Azure AD Standard-](https://azure.microsoft.com/pricing/details/active-directory/) und [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Self-service-Gruppe Verwaltung, Self-service-Anwendung und dynamische Gruppen, gibt [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) -Zusatzfunktionen.

###<a name="b2b-collaboration-enable-partner-access-to-applications"></a>: B2B Zusammenarbeit Partnerzugriff auf Anwendungen

Wenn Ihr Unternehmen mit anderen Unternehmen kann wurde, ist es wahrscheinlich benötigen Sie zum Verwalten von Partnerzugriff auf Ihre corporate Applications. Azure Active Directory B2B Zusammenarbeit bietet eine einfache und sichere Möglichkeit zum Freigeben von Ihrer apps für Partner zur Verfügung. Diese Funktion ist derzeit in der Vorschau.

| Leitfaden für Artikel |   |
| :---: | --- |
| Eine Übersicht der verschiedenen Azure AD verfügt, können Sie externe Benutzer verwalten, wie Hilfe Partnern und Kunden usw. an. | [Vergleich der Funktionen für die Verwaltung von externer Identitäten in Azure Active Directory](active-directory-b2b-compare-external-identities.md) |
| Eine Einführung in Vorschau B2B Zusammenarbeit und erste Schritte | [Einfache, sichere Cloud-Partner-Integration mit Azure AD-](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Azure-Active Directory B2B für die Zusammenarbeit](active-directory-b2b-collaboration-overview.md) |
| Sich ausführlicher Azure AD B2B Verwaltung von Zusammenarbeit und zur gemeinsamen Nutzung | [Zusammenarbeit an Dokumenten B2B: Funktionsweise](active-directory-b2b-how-it-works.md)<br /><br />[Aktuelle Schwächen Azure AD-B2B Zusammenarbeit Vorschau](active-directory-b2b-current-preview-limitations.md)<br /><br />[Ausführliche exemplarische Vorgehensweise mit Azure AD B2B Zusammenarbeit Preview](active-directory-b2b-detailed-walkthrough.md) |
| Verweis Artikel mit technischen Details zur Funktionsweise von Azure AD B2B für die Zusammenarbeit | [CSV-Dateiformat für Partnerbenutzer hinzufügen](active-directory-b2b-references-csv-file-format.md)<br /><br />[Zusammenarbeit an Dokumenten Azure AD-B2B betroffenen Benutzerattribute](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[Benutzer Token Format für Partnerbenutzer](active-directory-b2b-references-external-user-token-format.md) |

Die Zusammenarbeit an Dokumenten B2B Vorschau steht derzeit für [alle Editionen von Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

###<a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Access-Systemsteuerung: Portal für den Zugriff auf apps und Self-service-Funktionen

Der Azure AD-Access-Bereich ist, in dem Endbenutzer ihre apps starten können und Zugriff auf die Self-service-Features, mit denen ihre apps und Gruppenmitgliedschaft verwalten können. Weitere Optionen für den Zugriff auf apps SSO aktiviert sind zusätzlich klicken Sie im Bereich Access unten in der Liste enthalten. 

| Leitfaden für Artikel |   |
| :---: | --- |
| Vergleich von der verschiedenen verfügbaren Optionen zum Bereitstellen von einzelnen anmelden apps für Benutzer | [Bereitstellen von Azure AD integriert Applikationen für Benutzer](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) |
| Übersicht des Bereichs Access und deren mobilen entspricht MyApps | [Einführung in Access Systemsteuerung und MyApps](active-directory-saas-access-panel-introduction.md)<br />– [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| So Azure AD-apps von Office 365-Website zugreifen | [Verwenden das Startprogramm für Office 365-App](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| So Access Azure AD-apps mit der Intune verwaltet Browser mobile-app | [Intune verwalteten Browser](https://technet.microsoft.com/en-us/library/dn878029.aspx)<br />– [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Zugreifen auf Azure AD-apps mit Tiefe Links einleiten einmaliges Anmelden | [Erste direkten Anmelden Links zu Ihrer Apps](active-directory-appssoaccess-whatis.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Access-Systemsteuerung steht für [alle Editionen von Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

###<a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-to-apps"></a>Berichte: Einfach überwachen Sie app Access Änderungen und überwachen Sie Sign-ins von apps

Azure-Active Directory bietet verschiedene Berichte und Benachrichtigungen Ihrer Organisation Zugriff auf Anwendungen zu überwachen. Benachrichtigungen für abweichenden Sign-ins zu Ihrer apps empfangen werden können, und Sie können nachverfolgen, wann und warum ein Benutzer Zugriff auf eine Anwendung geändert wurde.

| Leitfaden für Artikel |   |
| :---: | --- |
| Übersicht über die Berichterstellungsfeatures in Azure Active Directory | [Erste Schritte mit Azure AD-Reporting](active-directory-reporting-getting-started.md) |
| Zum Überwachen der Sign-ins und des app-Verwendung Ihrer Benutzer | [Zeigen Sie Ihrer Access und Verwendungsberichte an](active-directory-view-access-usage-reports.md) |
| Nachverfolgen von Änderungen vorgenommen, wer eine bestimmte Anwendung zugreifen kann | [Azure-Active Directory-Bericht Ereignissen](active-directory-reporting-audit-events.md) |
| Exportieren Sie die Daten der diese Berichte zu Ihrer bevorzugten Extras mithilfe der Reporting-API | [Erste Schritte mit der Azure AD-API Reporting](active-directory-reporting-api-getting-started.md) |

Um finden Sie unter sind die Berichte in verschiedenen Versionen von Azure Active Directory, [Klicken Sie hier](active-directory-view-access-usage-reports.md#report-editions).

##<a name="see-also"></a>Siehe auch

[Was ist Azure Active Directory?](active-directory-whatis.md)

[B2C Azure-Active Directory](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure-Active Directory-Domänendiensten](https://azure.microsoft.com/services/active-directory-ds/)

[Azure kombinierte Authentifizierung](https://azure.microsoft.com/services/multi-factor-authentication/)
