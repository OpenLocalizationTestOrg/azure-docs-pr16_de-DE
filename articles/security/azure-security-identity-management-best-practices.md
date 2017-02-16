<properties
   pageTitle="Azure Identität Verwaltungs- und Access steuern Sie bewährte Methoden für Sicherheit | Microsoft Azure"
   description="Dieser Artikel bietet eine Reihe von bewährte Methoden für die Verwaltung von Identität und in Azure-Funktionen, erstellt Access Steuerelement verwenden."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yurid"/>

# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Bewährte Methoden für Sicherheit zu steuern Azure Identitätsmanagement und Zugriff

Viele halten Identität zu den neuen Begrenzungslinie Layer für Sicherheit, übernimmt die Rolle aus der herkömmlichen Netzwerk reduzierte Perspektive werden. Dieser Entwicklung des primären Drehpunkts für Sicherheit Aufmerksamkeit und Investitionen stammen aus, dass Netzwerkperimeter zunehmend porösem geworden ist und die Shape-Schutz nicht so wirksam, wie sie einmal vor der Auflösung von [BYOD](http://aka.ms/byodcg) Geräte und Cloud Applikationen waren sein.

In diesem Artikel wird eine Sammlung von Azure Identitätsmanagement und bewährte Methoden für Access-Steuerelement Sicherheit besprochen. Bewährten werden von unserer Erfahrung mit [Azure AD-](../active-directory/active-directory-whatis.md) und die Verwendung des Kunden, wie sich selbst abgeleitet.

Für jede bewährte Methode wird erläutert:

- Was ist die beste Methode
- Warum Sie diese bewährte Methode aktivieren möchten
- Was kann das Ergebnis sein, wenn Sie nicht die beste Methode aktivieren
- Mögliche Alternativen bewährte Methode
- Wie können Sie lernen, wie die bewährte Methode aktivieren

Diese Azure Identität Verwaltungs- und die Steuerung des Zugriffs, die bewährte Methoden für die Artikel auf eine Übereinstimmung Meinung und Azure-Plattformfunktionen und Features, basiert, während sie gleichzeitig vorhanden sind in diesem Artikel geschrieben wurde. Meinungen und Technologien Zeitverlauf ändern und in diesem Artikel wird in regelmäßigen Abständen diese Änderungen entsprechend aktualisiert werden.

Azure Identität Verwaltungs- und Access Steuerelement bewährte Methoden für Sicherheit in diesem Artikel beschriebenen umfassen:

- Zentrale Verwaltung Ihrer Identität
- Einmaliges Anmelden (SSO) aktivieren
- Verwaltung der Kennwörter bereitstellen
- Erzwingen der kombinierte Authentifizierung (MFA) für Benutzer
- Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC)
- Steuern der Orte, wo Ressourcen erstellt wurden Ressourcenmanager
- Leitfaden für Entwickler, Identität Funktionen für SaaS-apps zu nutzen.
- Überwachen Sie aktiv für verdächtige Aktivitäten.

## <a name="centralize-your-identity-management"></a>Zentrale Verwaltung Ihrer Identität

Ein wichtiger Schritt zum schützen Ihre Identität ist, um sicherzustellen, dass die IT-Abteilung kann Verwalten von Benutzerkonten von einem einzigen Ort hinsichtlich, wo über dieses Konto erstellt wurde. Während die meisten der Unternehmen IT-Organisationen ihre primäres Konto Directory lokalen aufweist, Cloud-hybridbereitstellungen sind im kommen, und es ist wichtig, dass Sie wissen, wie lokale integrieren und cloud Verzeichnisse und bieten eine nahtlose Verwendung für den Endbenutzer.

Um dieses Szenario [Hybrid Identität](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) zu erreichen empfehlen wir zwei Optionen zur Verfügung:

- Synchronisieren von Ihrem lokalen Verzeichnis mit der Cloud-Verzeichnis mit Azure AD-verbinden
- Ihre Identität lokalen zusammenarbeitet, mit der Cloud-Verzeichnis mit [Active Directory Federation Services](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS)

Organisationen, die nicht seine Identität lokal mit seine Identität Cloud integriert werden soll treten höhere Verwaltungsaufwand bei der Verwaltung von Konten, wodurch die Wahrscheinlichkeit von Fehlern und Sicherheit dienen erhöht.

Weitere Informationen zum Azure AD-Synchronisierung finden Sie im Artikel [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Einmaliges Anmelden (SSO) aktivieren

Wenn Sie zum Verwalten mehrerer Verzeichnisse haben, wird dies eine administrative Problem nicht nur für IT, sondern auch für Endbenutzer, die mehrere ein Kennwort. Mithilfe von [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) wird die Benutzern die Möglichkeit des, Anmeldung und Zugriff auf die Ressourcen, die sie unabhängig davon benötigen, für diese Ressource am lokalen oder in der Cloud mit demselben Satz von Anmeldeinformationen bereitgestellt werden.

Verwenden Sie SSO, damit Benutzer ihre [SaaS Applikationen](../active-directory/active-directory-appssoaccess-whatis.md) basierend auf deren Organisations-Konto in Azure AD zugreifen können. Dies gilt nicht nur für Microsoft SaaS apps, sondern auch andere apps, wie etwa [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) und [Vertrieb](../active-directory/active-directory-saas-salesforce-tutorial.md). Die Anwendung kann Azure AD als ein [SAML-basierten](../active-directory/fundamentals-identity.md) Identitätsanbieter verwenden konfiguriert werden. Als ein Steuerelement Sicherheit wird Azure AD nicht Emission ein Token, sodass sie sich in der Anwendung, es sei denn, sie Azure AD-Zugriff gewährt wurde. Sie können Access direkt oder durch eine Gruppe erteilen, dass sie Mitglied sind.

> [AZURE.NOTE] die Entscheidung zur Verwendung von SSO wirkt sich wie Sie Ihrem lokalen Verzeichnis mit der Cloud Directory integrieren. Wenn Sie SSO möchten, müssen Sie Föderation, verwenden, da Verzeichnissynchronisation nur [erzielen Sie denselben für einmaliges Anmelden](../active-directory/active-directory-aadconnect.md)zur Verfügung stellt.

Organisationen, die do not SSO für ihre Benutzer und Applikationen enforce werden weitere für Szenarien verfügbar gemacht, in dem Benutzer mehrere Kennwörter, wodurch sich die Wahrscheinlichkeit, dass Benutzer Kennwörter erneut zu verwenden, oder verwenden unsichere Kennwörter direkt erhöht haben sollen.

Sie erhalten weitere Informationen zu Azure AD SSO, indem Sie im Artikel [AD FS-Verwaltung und Anpassung mit Azure AD verbinden](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Verwaltung der Kennwörter bereitstellen

In Szenarien, in dem Sie mehrere Mandanten haben oder Sie Benutzer [ihr eigenes Kennwort zurücksetzen](../active-directory/active-directory-passwords-update-your-own-password.md)aktivieren möchten ist es wichtig, geeigneter Richtlinien verwenden, um Missbrauch zu verhindern. In Azure können Sie die Self-service-Kennwort zurücksetzen Videofunktionen nutzen und passen Sie die Sicherheitsoptionen an Ihre Bedürfnisse zuschneiden.

Es ist besonders wichtig zum Sammeln von Feedback von diesen Benutzern und aus ihrer Erfahrung erfahren Sie, wie Benutzer versuchen, auf die folgenden Schritte ausführen. Auszuführen Sie auf der Grundlage dieser Erfahrung, weiter, einen Plan aus, um potenzielle Probleme zu verringern, die während der Bereitstellung für eine größere Gruppe auftreten können. Es wird auch empfohlen, dass Sie das [Kennwort zurücksetzen Registrierungsaktivität Bericht](../active-directory/active-directory-passwords-get-insights.md) zum Überwachen der Benutzer, die registrieren in verwenden.

Organisationen, die Kennwort ändern Supportanrufe vermeiden möchten, dass Benutzer eigene Kennwörter zurücksetzen aktiviere sind weitere ausgesetzt Anruf Lautstärke an den Dienst Desk aufgrund von Kennwortproblemen. In Organisationen, die mehrere Mandanten haben, ist es erforderlich, dass Sie diese Art von Videofunktionen implementieren und anderer Benutzer ausführen Kennwortrücksetzung innerhalb von Sicherheitsgrenzen, die in der Sicherheitsrichtlinie festgelegt wurden.

Sie können weitere Informationen zur Kennwort zurücksetzen, indem Sie im Artikel [Management für Bereitstellung von Kennwort und Schulung der Benutzer zur gemeinsamen Nutzung](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Erzwingen der kombinierte Authentifizierung (MFA) für Benutzer

Mehrstufige Authentifizierung ist für Organisationen, die mit Branchenstandards, wie z. B. [PCI DSS Version 3,2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32)kompatibel sein müssen, muss Videofunktionen für Benutzer authentifiziert haben. Außer dass mit Branchenstandards kompatibel, erzwingen MFA Benutzerauthentifizierung kann Organisationen auch helfen, um Anmeldeinformationen Diebstahl Typ von Angriffen, z. B. [Pass-das-Hash (PtH) zu verringern](http://aka.ms/PtHPaper).

Aktivieren Sie Azure MFA für Ihre Benutzer ein, fügen Sie eine zweite Sicherheitsebene Benutzer anmelden-ins und Transaktionen hinzu. In diesem Fall möglicherweise eine Transaktion ein Dokuments in einem Dateiserver oder in Ihrer SharePoint Online zugreifen. Azure MFA auch hilft IT zum Verringern Sie der Wahrscheinlichkeit, dass eine beschädigte Anmeldeinformationen zu Organisation Daten zugreifen kann.

Beispiel: Sie erzwingen Azure MFA für Ihre Benutzer und konfigurieren, um einen Anruf oder Textnachricht als Überprüfung verwenden. Wenn die Anmeldeinformationen des Benutzers gefährdet sind, kann der Angreifer nicht mehr Zugriff auf alle Ressourcen, da er nicht auf Telefon des Benutzers zugreifen kann. Organisationen, die keine zusätzliche Ebenen Identität Schutz hinzufügen sind weitere für Anmeldeinformationen Diebstahl Angriffen, die zur Beschädigung der Daten führen kann.

Eine Alternative für Organisationen, die die gesamte Authentifizierung Steuerelement lokal gespeichert werden sollen sollten [Azure mehrstufige Authentifizierungsserver](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), die Abkürzung MFA lokal verwenden. Mithilfe dieser Methode werden Sie immer noch mehrstufige Authentifizierung, Beibehaltung der MFA Server lokal erzwingen können.

Weitere Informationen zum Azure MFA finden Sie im Artikel [Erste Schritte mit Azure kombinierte Authentifizierung in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC)

Einschränken des Zugriffs basierend auf der [minimalen Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) und [wissen müssen](https://en.wikipedia.org/wiki/Need_to_know) Sicherheit Grundsätze muss unbedingt für Organisationen, die Sicherheitsrichtlinien für den Zugriff auf Daten erzwingen möchten. Azure rollenbasierte Access Steuerelement (RBAC) kann das Zuweisen von Berechtigungen für Benutzer, Gruppen und Anwendungen mit einem bestimmten Bereich verwendet werden. Der Bereich eine rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein.

Sie können [sich RBAC](../active-directory/role-based-access-built-in-roles.md) Rollen in Azure Benutzern Berechtigungen zuweisen nutzen. Erwägen Sie *Speicher Konto Mitwirkender* für Cloud-Operatoren, die zum Verwalten von Speicherkonten und *klassischen Speicher Konto* Teilnehmerrolle zum Verwalten von Benutzerkonten klassischen Speicherplatz benötigen. Cloud-Operatoren, die zum Verwalten von virtuellen Computern und Speicher-Konto muss, sollten Sie in der *virtuellen Computern* Teilnehmerrolle hinzu.

Organisationen, die Access-Steuerelement not enforce DO durch Nutzung der Funktionen wie RBAC möglicherweise mehr Rechte als erforderlich sind, um ihren Benutzern zugewiesen werden. Dies kann dazu führen mit Daten Kompromisse von den Benutzern Zugriff auf bestimmte Arten von Datentypen (z. B. hoher Business Auswirkung), die sie im ersten Schritt sollten nicht.

Sie erhalten weitere Informationen zu Azure RBAC, indem Sie im Artikel [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md)lesen.

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Steuern der Orte, wo Ressourcen erstellt wurden Ressourcenmanager

Es ist wichtig, UFI-Cloud-Operatoren Aufgaben ausführen, während Sie verhindern, dass diese Bruchfestigkeit Konventionen, die Verwaltung Ihres Unternehmens Ressourcen benötigt werden. Organisationen, die zu die jeweiligen Speicherorten steuern, wo die Ressourcen erstellt werden, soll diese Orte codieren.

Um dies zu erreichen, können Organisationen Sicherheitsrichtlinien erstellen, die Definitionen enthalten, in denen die Aktionen oder Ressourcen, die speziell verweigert werden beschrieben. Sie weisen diese Richtliniendefinitionen in den gewünschten Bereich, z. B. das Abonnement, Ressourcengruppe oder eine bestimmte Ressource.

> [AZURE.NOTE] Dies ist nicht identisch mit RBAC, sie tatsächlich RBAC die Benutzerauthentifizierung an, die Berechtigung zum Erstellen von diese Ressourcen nutzt.

Nutzen Sie [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) benutzerdefinierte Richtlinien für Szenarien auch erstellen, in denen die Organisation möchte Vorgänge können nur, wenn die entsprechende Kostenstelle verknüpft ist; Andernfalls können sie die Anforderung ablehnen.

Organisationen, die nicht steuern werden wie Ressourcen erstellt wurden sind weitere für Benutzer, die Missbrauch Dienst durch weitere Ressourcen müssen, sondern auf erstellen. Sichern von den Erstellungsprozess Ressource ist ein wichtiger Schritt ein Szenario mit mehreren Mandanten gesichert.

Sie erhalten weitere Informationen zum Erstellen von Richtlinien mit Azure Ressourcenmanager den Artikel [Verwenden von Richtlinien zum Verwalten von Ressourcen und Steuern des Zugriffs](../resource-manager-policy.md).

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>Leitfaden für Entwickler, Identität Funktionen für SaaS-apps zu nutzen.

Identität des Benutzers werden in vielen Szenarios dazu verwendet wird, wenn Benutzer [SaaS apps](https://azure.microsoft.com/marketplace/active-directory/all/) zugreifen, die lokal integriert werden kann oder Verzeichnis cloud. Allem, empfehlen wir, dass Entwickler sichere Methoden verwenden, um diese apps, wie etwa [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx)entwickeln. Azure AD vereinfacht Authentifizierung für Entwickler durch Identität als Dienst, mit Unterstützung für Industriestandard-Protokolle, wie z. B. [OAuth 2.0](http://oauth.net/2/) , und [Verbinden Sie OpenID](http://openid.net/connect/)sowie Quelle Bibliotheken für verschiedene Plattformen zu öffnen.

Vergewissern Sie sich zum Registrieren einer beliebigen Anwendung, die mit dem Konfigurieren Azure AD-Authentifizierung beauftragt, dies ist eine obligatorische Prozedur. Grund hierfür ist, da Azure AD koordinieren die Kommunikation mit der Anwendung, bei der Behandlung von einmaliges Anmelden (SSO) oder den Austausch von Token muss. Die Sitzung des Benutzers läuft ab, wenn die Gültigkeitsdauer des das Token ausgestellt von Azure AD läuft ab. Immer ausgewertet werden Sie, wenn die Anwendung dieses Mal verwendet werden sollen oder wenn Sie diesmal reduzieren können. Reduzieren der Gültigkeitsdauer kann aus Sicherheitsgründen fungieren, der Benutzer abmelden basierend auf einer Zeitspanne erzwingen wird.

Organisationen, die nicht Identität-Steuerelement, um apps zuzugreifen erzwungen und Entwickler nicht zum apps sicher in ihre Identität Managementsystem zu integrieren Leitfaden möglicherweise mehr ausgesetzt Anmeldeinformationen Diebstahl Art von Angriffen, z. B. [schwachen Verwaltung der Authentifizierung und Sitzung beschrieben geöffneten Web Anwendung Sicherheitsprojekt (OWASP) Top 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet)aus.

Sie können weitere Informationen zur Authentifizierungsszenarien SaaS apps durch [Authentifizierungsszenarien Azure AD](../active-directory/active-directory-authentication-scenarios.md)lesen.

## <a name="actively-monitor-for-suspicious-activities"></a>Überwachen Sie aktiv für verdächtige Aktivitäten.

Nach [Verizon 2016 Daten Verstoß gegen Bericht](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/)wird Anmeldeinformationen Kompromisse noch steigen und zu den wissen, welche Unternehmen für Internetkriminelle. Aus diesem Grund ist es wichtig, zu einem aktiven Identität Monitor System angeordnet haben, die schnell verdächtigen Aktivität erkennen und lösen eine Warnung zur weiteren Untersuchung können. Azure AD weist zwei Bereiche Funktionen, mit denen Organisationen, deren Identitäten überwachen können: Azure AD Premium [Anomalie Berichte](../active-directory/active-directory-view-access-usage-reports.md) und Azure AD- [Identitätsschutz](../active-directory/active-directory-identityprotection.md) Videofunktionen.

Überprüfen Sie, dass die Anomalie Berichte verwenden, um Versuche, [ohne dass](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md) [Hackerangriffen](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) zu mit einem bestimmten-Konto anmelden müssen zu identifizieren versucht, melden Sie sich von mehreren Orten, melden Sie sich von [Geräten infiziert](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) und verdächtige IP-Adressen. Beachten Sie, dass diese Berichte sind beibehalten. Kurzum, müssen Sie Prozesse und Verfahren für IT-Administratoren, diese Berichte auf der täglich oder bei Bedarf (normalerweise in ein Vorfall Antwort-Szenario) ausführen angeordnet haben.

Dagegen Azure AD-Schutz ist eine aktive Überwachung System, und es wird die aktuellen Risiken auf einem eigenen Dashboard kennzeichnen. Außerdem erhalten Sie auch tägliche Zusammenfassung Benachrichtigungen per e-Mail. Es empfiehlt sich, dass Sie die Risiko Ebene entsprechend Ihren geschäftlichen Anforderungen anpassen. Die Risiko Ebene für ein Risikoereignis ist eine Angabe der Schwere der das Risikoereignis (hoch, Mittel oder niedrig). Die Risiko Ebene hilft Identitätsschutz Benutzer priorisieren Sie die Aktionen, die sie ausführen müssen, um das Risiko in ihrer Organisation zu verringern.

Organisationen, die ihre Identitätssysteme nicht aktiv überwachen sind Risiko Benutzeranmeldeinformationen gefährdet. Platzieren Sie ohne wissen, die verdächtige Aktivitäten aufzeichnen sind unter Verwendung dieser Anmeldeinformationen, die Organisationen können nicht mehr dieser Art der Bedrohung zu verringern.
Sie können weitere Informationen zur Azure-Schutz von [Azure Active Directory Identität Protection](../active-directory/active-directory-identityprotection.md)lesen.
