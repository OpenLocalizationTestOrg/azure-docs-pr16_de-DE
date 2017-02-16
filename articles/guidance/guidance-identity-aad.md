<properties
   pageTitle="Implementieren der Azure-Active Directory | Microsoft Azure"
   description="Informationen zum Implementieren einer sicheren Hybrid Netzwerkarchitektur Azure Active Directory verwenden."
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Implementieren der Azure-Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden bewährte Methoden für die Integration von lokalen Active Directory (AD) Domänen und Gesamtstrukturen mit Azure Active Directory-Identitätsauthentifizierung cloudbasierten bereitzustellen.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Bezug Architektur verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

Sie können Verzeichnis und Identität Dienste, wie die standardmäßig im Lieferumfang von Active Directory-Verzeichnisdienst (AD DS) mit Identitäten authentifiziert. Diese Identitäten können Benutzer, Computer, Applikationen oder andere Ressourcen, die Bestandteil einer Sicherheit Begrenzung angehören. Sie können Directory hosten und Identität services lokalen, aber in einem Hybriden Szenario, in dem Elemente einer Anwendung in Azure befinden, können sein, um dieses Feature in der Cloud zu erweitern. Dieser Ansatz kann die Verzögerung durch Senden Authentifizierung reduzieren und lokalen Autorisierungsanfragen aus der Cloud zu ausgeführten lokalen Verzeichnis und Identität Dienste zurück.

Azure bietet zwei Lösungen für die Implementierung von Verzeichnis und Identität Services in der Cloud:

1. Sie können [Azure Active Directory (AAD)] [ azure-active-directory] AD-Domäne in der Cloud erstellen und verknüpfen Sie es mit einer lokalen AD-Domäne. AAD ermöglicht es Ihnen so konfigurieren Sie einmaliges Anmelden (SSO) für Applikationen Zugriff über die Cloud-Benutzer. AAD nutzt [Azure AD verbinden] [ azure-ad-connect] im lokalen Repository frei lokalen repliziert Objekte und Identitäten ausgeführt, die in der Cloud.

    Sie können auch AAD implementieren, ohne mit einem lokalen Verzeichnis. In diesem Fall fungiert AAD als die primäre Quelle für alle Identitätsinformationen statt mit Daten aus einem lokalen Verzeichnis repliziert.

    Beachten Sie, dass das Verzeichnis in der Cloud **nicht** Erweiterung aus einem lokalen Verzeichnis. Lieber, empfiehlt es sich um eine Kopie, die die gleichen Objekte und Identitäten enthält. An diese Elemente lokalen vorgenommene Änderungen werden in der Cloud kopiert, aber die Änderungen vorgenommen werden, die Cloud **sind nicht** wieder auf die lokale Domäne repliziert werden.

    >[AZURE.NOTE] Die Azure Active Directory Premium-Editionen unterstützen schreiben-Rückseite Benutzerkennwörter, Aktivieren der lokale Benutzer zum Ausführen von Self-service das Zurücksetzen von Kennwörtern in der Cloud. Dies ist die einzige Information, die AAD zurück zu einem lokalen Verzeichnis synchronisiert. Weitere Informationen zu den verschiedenen Versionen von AAD und ihre Features finden Sie unter [Azure-Active Directory-Editionen][aad-editions].

2. Sie können von Ihrem lokalen Datacenter ein virtuellen Computers ausgeführt AD DS als Domänencontroller in Azure, erweitern Ihre vorhandene AD-Infrastruktur (einschließlich AD DS und AD FS) bereitstellen. Dieser Ansatz empfiehlt häufiger für Szenarien, wo die lokale und Azure virtuelle Netzwerk durch eine Verbindung VPN und/oder ExpressRoute verbunden sind. Diese Lösung unterstützt auch bidirektionale Replikation aktivieren Sie nehmen Sie Änderungen in der Cloud und lokalen, wo es am besten geeignet ist.

Diese Architektur Schwerpunkt Lösung 1 aus. Weitere Informationen zu den zweiten Lösung, finden Sie unter [Active Directory erweitern, um Azure][guidance-adds].

Typische Nutzung Fällen für diese Architektur umfassen:

- Bereitstellen von SSO Endbenutzer SaaS und andere Programme ausgeführt, in der Cloud, verwenden für lokale Applikationen dieselben Anmeldeinformationen, die sie angeben.

- Für die Veröffentlichung lokalen Webanwendungen durch die Cloud den Zugriff auf remote-Benutzer, die zu Ihrer Organisation gehören.

- Implementieren der Self-service-Funktionen für Endbenutzer, wie ihre Kennwörter zurücksetzen und Zuweisen der Verwaltung von Gruppen. 

    >[AZURE.NOTE] Diese Funktionen können von einem Administrator über Gruppenrichtlinien gesteuert werden.

- Situationen, in dem die lokale und Azure virtuelle Netzwerk Hostinganbieter Cloud Applikationen nicht direkt verknüpft sind mithilfe eines VPN-Tunnel oder ExpressRoute Verbindung.

Beachten Sie, dass AAD nicht alle Funktionen von AD bereitstellt. Beispielsweise verarbeitet AAD aktuell nur Benutzerauthentifizierung statt Computerauthentifizierung. Einige Anwendungen und Dienste, wie etwa SQL Server, Computerauthentifizierung erfordern möglicherweise in diesem Fall diese Lösung nicht geeignet ist. Darüber hinaus AAD ist möglicherweise nicht geeignet für Systeme, in dem Komponenten über die lokale/Cloud Grenze migriert werden konnte, sondern nur kopierte.

> [AZURE.NOTE] Eine ausführliche Erläuterung der Funktionsweise von Azure Active Directory, schauen Sie sich [Microsoft Active Directory-Erläuterung][aad-explained].

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur. Weitere Informationen zu den Elementen Arbeitsbelastung in Azure finden Sie unter [Ausführen von virtuellen Computern, für eine mehrstufige Architektur auf Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Zur Vereinfachung dieses Diagramm nur zeigt die direkt im Zusammenhang mit AAD Verbindungen und Web Browser Anforderung leitet keine darzustellen oder andere Protokoll Datenverkehr, die als Teil der Authentifizierung und Benutzeridentität Föderation Prozess auftreten können. Beispielsweise ein Benutzers (lokal oder Remote) greift normalerweise auf eine Web app über einen Browser, und das Web app möglicherweise den Webbrowser, um die Anforderung über AAD authentifizieren transparent umzuleiten. Nach der Authentifizierung kann die Anforderung an das Web app zusammen mit den entsprechenden Identitätsinformationen zurückgegeben werden.

[! [0]][0]

- **Azure Active Directory-Mandanten**. Dies ist eine Instanz der AAD von Ihrer Organisation erstellt. Es fungiert als einfachen Verzeichnisdienst für Applikationen Cloud (es enthält Objekte, die von der lokalen kopierte AD), und stellt Identität zur Verfügung.

- **Web Ebene Subnetz**. Dieses Subnetz enthält virtuellen Computern, die eine benutzerdefinierte cloudbasierten Anwendung entwickelt in Ihrer Organisation und für die AAD als eine Identität Bank dienen kann implementieren.

- **Lokalen AD DS-Server**. Ihrer Organisation sind wahrscheinlich haben bereits einen vorhandenen Verzeichnisdienst wie AD DS. Sie können viele der Elemente in Ihrem Verzeichnis AD DS (z. B. Benutzer und Gruppeninformationen) mit AAD, AAD zu diesen Informationen zur Authentifizierung von Identitäten aktivieren synchronisieren.

- **Verbinden von Azure AD Synchronisierungsserver**. Dies ist eine dem lokalen Computer, der den Azure AD verbinden Synchronisierungsdienst ausgeführt wird. Installieren Sie diesen Dienst mithilfe der Software Azure AD verbinden. Der Azure AD verbinden synchronisieren-Dienst synchronisiert Informationen (Benutzer, Gruppen, Kontakte usw.) frei, die in der lokalen AD zu AAD in der Cloud. Angenommen, Sie können bereitstellen oder Entziehen von Gruppen und Benutzer lokale und diese Änderungen an AAD weitergegeben werden. Der Dienst Azure AD verbinden synchronisieren übergibt Informationen an den AAD Mandanten.

    >[AZURE.NOTE] Aus Sicherheitsgründen werden Kennwörter des Benutzers nicht direkt in AAD gespeichert. Vielmehr enthält AAD einen Hash jedes Kennwort. Dies ist ausreichend, um das Kennwort eines Benutzers zu überprüfen. Wenn ein Benutzer Zurücksetzen eines Kennworts erforderlich ist, muss dies ausgeführte lokalen und den neuen Hash AAD gesendet. AAD Premium-Editionen enthalten Features, die diese Aufgabe aus, damit Benutzer ihre eigenen Kennwörter zurücksetzen können automatisieren können.

## <a name="recommendations"></a>Empfehlungen

In diesem Abschnitt werden die Vorgehensweisen für die Implementierung von AAD wie folgt zusammengefasst.

- Verbinden von Active Directory
- Sicherheit

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD verbinden synchronisieren Dienst Empfehlungen

Synchronisierung mit um sicherzustellen, dass Benutzer Identitätsinformationen in der Cloud, die lokal gehalten entspricht befasst. Der Zweck des Windows Azure AD verbinden synchronisieren Diensts ist diese Konsistenz verwalten. Die folgenden Punkte zusammenfassen Empfehlungen für den Azure AD verbinden synchronisieren implementiert:

- Vor der Implementierung Azure AD verbinden synchronisieren, sollten Sie die Synchronisierung Anforderungen Ihrer Organisation ermitteln (was, in welche Domänen synchronisieren und wie oft. Im Artikel [ermitteln Directory-Synchronisierung Anforderungen] [ aad-sync-requirements] beschreibt die Punkte, die Sie berücksichtigen sollten.

- Verwenden eines virtuellen Computers Azure AD verbinden Synchronisierungsdienst ausgeführt werden kann oder ein Computer gehostet lokalen werden. Je nach der Flüchtigkeit der Informationen in Ihrem Verzeichnis AD nachdem die ursprüngliche Synchronisierung mit AAD durchgeführt wurde ist Auslastung Azure AD verbinden synchronisieren-Dienst wahrscheinlich nicht hoch sein. Verwenden eines virtuellen Computers ermöglicht es Ihnen zu skalieren einfacher auf den Server (Monitor die Aktivität im Abschnitt [Überwachung Aspekte](#monitoring-considerations) beschriebenen feststellen, ob dies erforderlich ist).

- Wenn Sie mehrere lokale Domänen in einer Gesamtstruktur haben, können Sie speichern und Synchronisieren von Informationen für die gesamte Struktur auf einen einzelnen AAD Mandanten (Dies ist ein empfohlen). Filtern von Daten für Identitäten, die in mehr als eine Domäne auftreten, dass jede Identität wird nur einmal in AAD statt wird dupliziert, da dies zu Inkonsistenzen führen kann, wenn Daten synchronisiert werden. Dieser Ansatz erfordert mehrere Azure AD verbinden synchronisieren Server implementieren. Weitere Informationen finden Sie unter dem Szenario mehrere AAD im Abschnitt [Suchtopologie Aspekte](#topology-considerations) . 

- Verwenden von Filtern in AAD gespeichert werden, um nur die Daten einschränken, die erforderlich ist. Beispielsweise könnten Ihrer Organisation nicht Informationen inaktive oder nicht persönliche Konten in AAD speichern möchten. Filtern Gruppe basierend, Domänen basierendes, Organisationseinheit-basierten oder Attribut-basierte sein kann, und Filter, um komplexere Regeln generieren können kombiniert werden. Beispielsweise können Sie auswählen, um nur die Objekte frei, die in einer Domäne zu synchronisieren, die einen bestimmten Wert in einer ausgewählten Attribut aufweisen. Ausführliche Informationen finden Sie unter [Azure AD verbinden synchronisieren: Konfigurieren von Filtern][aad-filtering].

- Führen Sie zum Implementieren der hohen Verfügbarkeit für den AD verbinden Synchronisierungsdienst einen sekundären staging-Server aus. Weitere Informationen finden Sie im Abschnitt [Suchtopologie Aspekte](#topology-considerations) .

### <a name="security-recommendations"></a>Sicherheit Empfehlungen

Die folgenden Elemente Zusammenfassung die primären Sicherheit Empfehlungen für die Durchführung AAD, je nach den Anforderungen Ihrer Organisation:

- Verwalten von Benutzerkennwörter aus.

    Wenn Sie eine Premium-Edition von AAD verwenden, können Sie Kennwort schreiben-Back aus AAD in Ihrem lokalen Verzeichnis nach Durchführung einer benutzerdefinierten Installations von Azure AD verbinden aktivieren:

    [! [9]][9]

    Dieses Feature ermöglicht Benutzern, ihre eigenen Kennwörter aus innerhalb des Portals Azure zurücksetzen, aber nur nach Ihrer Organisation Sicherheit Kennwortrichtlinien überprüfen aktiviert werden soll. Beispielsweise können Sie einschränken, welche Benutzer ihre Kennwörter ändern können, und das Kennwort Verwaltungsoption zum Anpassen. Weitere Informationen finden Sie unter [Anpassen Password Management an den Anforderungen Ihrer Organisation anpassen][aad-password-management].

- Verwalten von Schutz für lokale Applikationen, die extern zugegriffen werden kann.

    Verwenden Sie den Azure AD-Anwendungsproxy gesteuerten Zugriff auf lokale Webanwendungen von externen Benutzern über AAD bereit. Dieser Ansatz Loss Ihrer Anwendung von nicht direkt mit dem Internet verfügbar machen. Nur Benutzer mit gültigen Anmeldeinformationen in Ihrem Verzeichnis Azure können Ihre Applikationen erreicht haben. Weitere Informationen finden Sie im Artikel [Aktivieren Anwendungsproxy Azure-Portal][aad-application-proxy].

- Überwachen aktiv AAD auf Vorzeichen verdächtigen.

    Erwägen Sie AAD Premium P2 Edition, wozu auch AAD Identitätsschutz. Schutz der Identität verwendet adaptive maschinellen Learning Algorithmen und Heuristik Bildschirmdarstellung auftreten erkennen und Risiko Ereignisse, die hindeuten, dass eine Identität geworden ist. Beispielsweise können sie erkennen, potenziell abweichenden Aktivität z. B. unregelmäßiges Anmeldung Aktivitäten, melden Sie sich ins von unbekannten Quellen oder IP-Adressen durch verdächtige Aktivitäten, oder melden Sie sich-ins von Geräten, die möglicherweise infiziert. Mit diesen Daten generiert Identitätsschutz Berichte und Benachrichtigungen, die ermöglicht es Ihnen, diese Risikoereignisse ermitteln, und führen Sie die geeigneten Sicherheitsmaßnahmen oder Minderungsaktion. Weitere Informationen finden Sie unter [Azure Active Directory Identität Protection][aad-identity-protection].

    Das Berichterstellungsfeature der AAD Azure-Portal können Sie auch überwachen verdächtige und andere Sicherheits-Aktivitäten in Ihrem System durchgeführt werden. AAD kann eine Reihe von Zusammenfassungsberichte generiert werden:

    [! [17]][17]

    Weitere Informationen zur Verwendung dieser Berichte finden Sie unter [Azure Active Directory Reporting Guide][aad-reporting-guide].

## <a name="topology-considerations"></a>Suchtopologie Aspekte

Wenn Sie einem lokalen Verzeichnis mit AAD integrieren, konfigurieren Sie Azure AD-verbinden, um ein Suchtopologie implementieren, die die Anforderungen Ihrer Organisation am besten entspricht. Die folgenden: Topologien, die Azure AD verbinden unterstützt

- **Einzelne Gesamtstruktur, einzelnes AAD Verzeichnis**. In diesem Suchtopologie mit Azure AD verbinden Objekte synchronisiert werden und Identitätsinformationen in eine oder mehrere Domänen in einem einzigen am lokalen Gesamtstruktur mit einem einzelnen AAD Mandanten. Dies ist die standardmäßige Suchtopologie implementiert, indem Sie die express-Installation von Azure AD verbinden.

    [! [1]][1]

    Nicht erstellen mehrere Azure AD verbinden synchronisieren Server zum andere Domänen in der gleichen lokalen Struktur den gleichen AAD Mandanten herstellen, es sei denn, Sie einen Azure AD verbinden Synchronisierungsserver in das staging Modus ausgeführt werden (siehe unten Staging Server).

- **Mehrere Gesamtstrukturen, einzelnes AAD Verzeichnis**. Verwenden Sie diese Suchtopologie, wenn Sie mehr als einer lokalen Gesamtstruktur haben. Sie können Identitätsinformationen konsolidieren, damit jeden einzelnen Benutzer im Verzeichnis AAD einmal vorhanden ist, selbst wenn derselbe Benutzer in mehr als einer Gesamtstruktur vorhanden ist. Alle Gesamtstrukturen verwenden denselben Azure AD verbinden Synchronisierungsserver. Azure AD verbinden Sync-Server muss nicht Teil einer beliebigen Domäne sein, aber es aus allen Gesamtstrukturen muss erreichbar sein:

    [! [2]][2]

    In dieser Suchtopologie, verwenden Sie keine separaten Azure AD verbinden Servern auf einen einzelnen AAD Mandanten jeder Gesamtstruktur lokale Verbindung synchronisieren. Dies kann duplizierten Identität Informationen in AAD führen, wenn Benutzer in mehr als einer Gesamtstruktur vorhanden sind.

- **Mehrere Gesamtstrukturen, separaten Topologien**. Dieser Ansatz ermöglicht es Ihnen Identitätsinformationen aus getrennten Gesamtstrukturen in einem einzelnen AAD Mandanten zusammenführen. Diese Strategie ist sinnvoll, wenn Sie Gesamtstrukturen aus anderen Organisationen (nach einer Beurteilung von Fusionen und Übernahme, beispielsweise kombinieren) und die Identitätsinformationen für jeden Benutzer, die nur in einer Gesamtstruktur frei ist:

    Wenn die Adresslisten in den einzelnen Gesamtstrukturen synchronisiert werden, kann ein Benutzer in einer Gesamtstruktur in ein anderes als Kontakt vorliegen. Dies kann auftreten, wenn beispielsweise Ihre Organisation GALSync mit Forefront Identität Manager 2010 oder Microsoft Identität-Manager 2016 implementiert hat. In diesem Szenario können Sie angeben, dass Benutzer mit ihrer *E-Mail* -Attribut bestimmt werden soll. Sie können auch mit den Attributen *ObjectSID* und *MsExchMasterAccountSID* Identitäten entsprechen. Dies ist sinnvoll, wenn Sie eine oder mehrere Ressourcengesamtstrukturen mit deaktivierten Konten verfügen.

- **Bereitstellungsserver**. In dieser Konfiguration führen Sie eine zweite Instanz von Azure AD verbinden Sync-Server parallel mit dem ersten aus. Diese Struktur unterstützt Szenarien wie:

    - Hohe Verfügbarkeit.

    - Testen und Bereitstellen einer neuen Konfigurations des Servers synchronisieren Azure AD verbinden.

    - Einführung in einem neuen Server und Betrieb einer älteren Konfigurations. 

    In diesen Fällen wird die zweite Instanz *staging-Modus*ausgeführt. Der Server Einträge importierte Objekte und von Synchronisierungsdaten in der Datenbank, aber nicht leitet die Daten zu AAD. Nur, wenn Sie staging Modus deaktivieren, startet der Server Schreiben von Daten in AAD und auch Starts leistungsfähigeren Kennwort Schreiben-in die lokale Verzeichnisse gegebenenfalls Zustellung:

    [! [4]][4]

    Weitere Informationen finden Sie unter [Azure AD verbinden synchronisieren: Aspekte und Betriebsaufgaben][aad-connect-sync-operational-tasks].

- **Mehrere AAD Verzeichnisse durchsuchen**. Es wird empfohlen, dass Sie ein einzelnes AAD Verzeichnis für eine Organisation erstellen, aber es Situationen jedoch kann, in denen Sie Informationen über separate AAD Verzeichnisse müssen. In diesem Fall sollten Sie sicherstellen, dass jedes Objekt aus der lokalen Gesamtstruktur nur in einem AAD-Verzeichnis, um Probleme mit der Synchronisierung und Kennwort schreiben-Back zu vermeiden wird angezeigt.

    Um dieses Szenario zu implementieren, konfigurieren Sie separate Azure AD verbinden Servern für jedes AAD Verzeichnis synchronisieren und filtern, sodass jeder Azure AD verbinden Synchronisierungsserver auf eine gegenseitig Reihe von Objekten auswirkt verwenden: 

    [! [5]][5]

Weitere Informationen zu diesen Topologien, finden Sie unter [Topologien für Azure AD verbinden][aad-topologies].

## <a name="user-sign-in-considerations"></a>Benutzer Anmeldung Aspekte

Der Dienst AAD geht standardmäßig davon aus, dass sich Benutzer anmelden können, indem Sie dasselbe Kennwort verwenden Sie lokale und Azure AD verbinden Sync-Server konfiguriert die Synchronisierung von Kennwörtern zwischen der lokalen Domäne und AAD. Für viele Organisationen Dies ist die entsprechenden sollten Sie bestehende Richtlinien und Infrastruktur Ihrer Organisation. Beispiel:

- Die Sicherheitsrichtlinie Ihrer Organisation möglicherweise verbieten Kennworthashes in der Cloud zu synchronisieren.

- Sie möglicherweise, dass Benutzer nahtlose SSO erzielen Sie (ohne zusätzliche Kennwort auffordert) benötigt, wenn Autos Domäne Cloudressourcen zugreifen auf das Unternehmensnetzwerk verknüpft werden.

- Ihre Organisation möglicherweise bereits ADFS oder eine dritte Partei Föderation Anbieter bereitgestellt. Sie können zum Verwenden dieser Infrastruktur implementieren der Authentifizierung und SSO und nicht mithilfe von Kennwortinformationen frei, die in der Cloud AAD konfigurieren.

Weitere Informationen finden Sie unter [Azure AD verbinden Benutzer anmelden Optionen][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD-Anwendung Proxy Aspekte

Verwenden Sie für den Zugriff auf lokale Anwendungen Azure AD.

- Verfügbar machen der lokalen Webanwendungen mithilfe der Anwendungsproxy, die von der Azure AD-Anwendung Proxy-Komponente Verbinder zu verwalten. Der Anwendung Proxy Verbinder wird eine ausgehende Verbindung mit dem Azure AD-Proxy-Anwendung geöffnet. Remotebenutzer Anfragen werden an der Web apps wieder von AAD über diese Verbindung weitergeleitet. Dieses Verfahren entfernt die müssen eingehenden Ports im lokalen Firewall, Angriffsfläche verfügbar gemacht werden, indem Sie Ihrer Organisation zu öffnen.

Weitere Informationen finden Sie unter [Veröffentlichen von Applications mit Azure AD-Anwendungsproxy][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Objekt Synchronisierung Aspekte

Die Standardkonfiguration von Azure AD verbinden synchronisiert Objekte aus Ihrem lokalen Active Directory-Verzeichnis basierend auf den Satz von Regeln, die im Artikel angegebenen [Azure AD verbinden synchronisieren: Grundlegendes zu der Standard-Konfiguration][aad-connect-sync-default-rules]. Nur Objekte, die diese Regeln synchronisiert werden, werden andere ignoriert erfüllen. Beispielsweise Objekte Benutzer müssen einen eindeutigen *SourceAnchor* Attribut und das Attribut *AccounEnabled* ausgefüllt werden muss. Benutzer-Objekte, die nicht über ein Attribut *sAMAccountName* oder *AAD_* oder *MSOL_* mit dem Text beginnen, werden nicht synchronisiert. Verbinden von Azure AD gilt Benutzer, Kontakt, gruppieren, ForeignSecurityPrincipal und Computer-Objekte mehrere Regeln. Wenn Sie die Standardgruppe von Regeln ändern müssen, verwenden Sie die Synchronisierung Regel-Editor mit Azure AD verbinden installiert (auch in dokumentierten [Azure AD verbinden synchronisieren: Grundlegendes zu Standard-Konfiguration][aad-connect-sync-default-rules]).

Sie können eigene Filter zum Einschränken der Objekte nach Domäne oder Organisationseinheit synchronisiert werden definieren. Oder in beschriebenen implementieren komplexere benutzerdefiniertes Filtern, [Azure AD verbinden synchronisieren: Konfigurieren von Filtern][aad-filtering].

## <a name="security-considerations"></a>Zur Sicherheit

Verwenden Sie bedingte Access-Steuerelement Authentifizierungsanfragen von unerwarteten Quellen verwehren möchten:

- Auslösen [Azure mehrstufige Authentifizierung (MFA)] [ azure-multifactor-authentication] , wenn ein Benutzer versucht, die Verbindung von einem nicht vertrauenswürdigen Speicherort (z. B. über das Internet) und einem vertrauenswürdigen Netzwerk nicht.

- Verwenden Sie den Typ des Geräts Plattform des Benutzers (iOS, Windows Mail unter Windows Mobile, Android) Zugriffsrichtlinie auf Programme und Funktionen zu bestimmen.

- Aufzeichnen von Geräten der Benutzer den Zustand aktiviert/deaktiviert, und diese Informationen in Access Richtlinie Prüfungen integrieren. Beispielsweise bei Verlust oder Diebstahl eines Benutzers Telefon sollten sie aufgezeichnet werden als deaktiviert, um zu verhindern, dass es für den Zugriff verwendet wird.

- Steuern der Zugriffsebene für einen Benutzer basierend auf Gruppenmitgliedschaft an. Verwenden von [Regeln für die dynamische Mitgliedschaft AAD] [ aad-dynamic-membership-rules] in der Gruppe Verwaltung zu vereinfachen. Eine kurze Übersicht über die Funktionsweise dieser finden Sie unter [Einführung in dynamische Mitgliedschaften für die Gruppen][aad-dynamic-memberships].

- Verwenden Sie bedingte Risiko-Richtlinien mit AAD Identität Protection, um erweiterte Schutz basierend auf ungewöhnliche Aktivitäten oder andere Ereignisse bereitzustellen.

Weitere Informationen finden Sie unter [bedingte Azure Active Directory-Zugriffs][aad-conditional-access].

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Skalierbarkeit wird durch den Dienst AAD und die Konfiguration des Servers synchronisieren Azure AD-verbinden berücksichtigt werden:

- Für den Dienst AAD müssen Sie keine Optionen für die Skalierbarkeit Implementierung konfigurieren. Der Dienst AAD unterstützt Skalierbarkeit basierend auf Replikate. AAD implementiert ein einzelnes primären Replikat, welche Ziehpunkte Vorgänge schreiben, und der mehrere schreibgeschützte sekundäre. AAD leitet transparent versuchten Einträge gegen sekundäre Replikate auf primäres Replikat. AAD bietet tatsächlichen Konsistenz. Alle an der primären vorgenommene Änderungen werden auf der Sekundärachse Replikate verteilt. Wie die meisten Vorgänge gegen AAD liest statt schreibt sind, kann diese Architektur gut skaliert werden.

    Weitere Informationen finden Sie unter [Azure AD: Erweiterte Einstellungen von unserem Geo redundant, hochgradig verfügbar, verteilte Cloud-Verzeichnis][aad-scalability].

- Für den Server Azure AD verbinden synchronisieren sollten Sie ermitteln, wie viele Objekte aus einem lokalen Verzeichnis wahrscheinlich synchronisiert werden. Wenn Sie weniger als 100.000 Objekte verfügen, können Sie die standardmäßigen SQL Server Express LocalDB Software mit Azure AD verbinden bereitgestellt. Wenn Sie eine größere Anzahl von Objekten verfügen, installieren Sie eine Herstellung Version von SQL Server und führen Sie eine benutzerdefinierte Installation von Azure AD verbinden angeben, dass eine vorhandene Instanz von SQL Server verwendet werden sollen.

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Wie bei Skalierbarkeit bedenken, umfasst die Verfügbarkeit der AAD-Dienst und die Konfiguration von Azure AD verbinden:

- Der Dienst AAD soll hohe Verfügbarkeit zu gewährleisten. Es gibt keine konfigurierbare Verfügbarkeitsoptionen aus. Es ist Geo verteilt, und mehrere Daten ausgeführt zentriert verteilt auf der ganzen Welt mit Automatisches Failover. Wenn Sie einem Data Center nicht mehr verfügbar ist, AAD ist sichergestellt, dass Ihre Verzeichnisdaten für Instanz von Access in mindestens zwei weitere auseinander liegenden Standorten Data Center verfügbar ist.

    >[AZURE.NOTE] Der Vereinbarung zum SERVICELEVEL für AAD Standard- und Premium Services Garantien mindestens 99,9 % Verfügbarkeit. Es gibt keine Vereinbarung zum SERVICELEVEL für die kostenlose Ebene der AAD aus. Weitere Informationen finden Sie unter [Vereinbarung zum SERVICELEVEL für Azure Active Directory][sla-aad].

- Zum Verbessern der Verfügbarkeit des Servers Azure AD verbinden synchronisieren können Sie eine zweite Instanz ausführen, in das staging Modus, wie im Abschnitt [Suchtopologie Aspekte](#topology-considerations) beschrieben. 

    Darüber hinaus, wenn Sie nicht die Instanz von SQL Server Express LocalDB, die mit Azure AD verbinden stammen verwenden, sollten dann hohen Verfügbarkeit für SQL Server Sie. Beachten Sie, dass die Lösung nur hohen Verfügbarkeit unterstützt SQL Cluster ist. Lösungen wie Spiegelung und immer auf werden von Azure AD Verbinden nicht unterstützt.

    Weitere Aspekte zum Verwalten von die Verfügbarkeit des Servers synchronisieren Azure AD verbinden, und wie Sie nach einem Fehler wiederherstellen, finden Sie unter [Azure AD verbinden synchronisieren: Aspekte - Wiederherstellung und Betriebsaufgaben][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Gesichtspunkte

Es gibt zwei Aspekte für die Verwaltung von AAD aus:

1. Verwalten von AAD in der Cloud.

2. Verwalten von den Azure AD verbinden-Servern synchronisieren.

AAD bietet die folgenden Optionen für die Verwaltung von Domänen und Verzeichnissen in der Cloud:

- [Azure-Active Directory-PowerShell-Modul][aad-powershell]. Verwenden Sie dieses Modul, wenn Sie allgemeine Azure AD-Verwaltungsaufgaben wie Benutzer Verwaltung Domain Management und für einmaliges Anmelden konfigurieren Skripts müssen.

- Azure AD Management Blade Azure-Portal. Diese Blade bietet eine interaktive Management Ansicht des Verzeichnisses, und ermöglicht Ihnen das Steuern und die meisten Aspekte der AAD konfigurieren.

    [! [10]][10]

Verbinden von Azure AD installiert die folgenden Tools, die Sie verwenden, um die Synchronisierungsdienste Azure AD-Verbinden von Ihrem lokalen Computer verwalten:

- Die Konsole Microsoft Azure Active Directory verbinden. Dieses Tool ermöglicht Ihnen, ändern Sie die Konfiguration des Servers Azure AD synchronisieren, anpassen, wie die Synchronisierung erfolgt, und aktivieren oder deaktivieren staging Modus, wechseln den Benutzer anmelden Modus (Sie können AD FS Anmeldung mit Ihrem lokalen Infrastruktur aktivieren).

- Die Synchronisierung Dienst-Manager. Verwenden Sie die Registerkarte *Vorgänge* in diesem Tool zum Verwalten der Synchronisierung und feststellen, ob alle Teile des Prozesses ausgefallen. Sie können die Synchronisierung manuell mithilfe dieses Tool auslösen. 

    [! [12]][12]

    Die Registerkarte *Connectors* ermöglicht es Ihnen, die Verbindungen für die Domänen zu steuern (lokal und in der Cloud), dem die Synchronisierung-Engine zugeordnet ist:

    [! [13]][13]

-  Die Synchronisierung Regel-Editor. Verwenden Sie dieses Tool, um die Art und Weise anpassen, in der Objekte transformiert werden, wenn sie zwischen einem lokalen Verzeichnis und in der Cloud AAD kopiert werden. Mit diesem Tool können Sie zusätzliche Attribute und Objekte zur Synchronisierung angeben und Filter zu bestimmen, welche Instanzen oder sollte nicht synchronisiert werden soll.

    Weitere Informationen finden Sie unter Abschnitt Synchronisierung Regel-Editor im Dokument [Azure AD verbinden synchronisieren: Grundlegendes zu der Standard-Konfiguration][aad-connect-sync-default-rules].

Die Seite [Azure AD verbinden synchronisieren: optimale Methoden zum Ändern der Standard-Konfigurations] [ aad-sync-best-practices] enthält weitere Informationen und Tipps für die Verwaltung von Azure AD verbinden.

## <a name="monitoring-considerations"></a>Überlegungen für die Überwachung

Überwachen des Systemzustands wird mithilfe einer Reihe von Agents installiert lokalen ausgeführt:

- Verbinden von Azure AD Installationen einen Agent, der Informationen zur Synchronisierung erfasst werden. Verwenden Sie das Blade Azure Active Directory verbinden Gesundheit Azure-Portal zum Überwachen der Gesundheit und Leistung von Azure AD verbinden. Weitere Informationen finden Sie unter [Verwenden Azure AD verbinden Gesundheit für synchronisieren][aad-health].

- Um die Integrität des AD DS-Domänen und Verzeichnissen aus Azure überwachen, installieren Sie die Azure AD verbinden Integrität für AD DS-Agent auf einem Computer innerhalb der lokalen Domäne aus. Verwenden Sie das Blade Azure Active Directory verbinden Systemzustand im Azure-Portal an Monitor AD DS aus. Weitere Informationen finden Sie unter [Verwenden von Azure AD verbinden Gesundheit in AD DS][aad-health-adds]

- Installieren Sie die Azure AD verbinden Integrität zum Überprüfen des AD FS für lokale ausgeführt, und verwenden Sie das Blade Azure Active Directory verbinden Gesundheit Azure-Portal, zum Überwachen der AD DS in AD FS-Agent an. Weitere Informationen finden Sie unter [Verwenden von Azure AD verbinden Gesundheit mit AD FS][aad-health-adfs]

Weitere Informationen zum Installieren von Agents AD verbinden Gesundheit und ihren Anforderungen, finden Sie unter [Azure AD verbinden Health Agent Installation][aad-agent-installation].

## <a name="sample-solution"></a>Beispiel-Lösung

Dieser Abschnitt beschreibt die Schritte zum Erstellen einer Stichprobe-Lösung, die Sie verwenden können, um die Konfiguration des AAD testen. Die Lösung umfasst die folgenden Elemente:

- Eine mehrstufige Webanwendung, die Struktur von [Virtuellen ausgeführt Computern für eine mehrstufige Architektur auf Azure]beschriebenen folgen[implementing-a-multi-tier-architecture-on-Azure].

- Client-Testcomputer. Wir empfehlen virtuellen von einem anderen Computer für diesen Computer verwenden. Die Konfiguration von diesem virtuellen Computer ist nicht von Bedeutung, solange Sie über Administratorzugriff verfügen, und sie können die Verbindung zu der Webanwendung mehrstufige.

- Ein simuliertes lokalen Umgebung, die die eigenen Domäne mit AD DS erstellt enthält.

Sind Szenarien, bei denen führen Sie folgendermaßen vor:

- Zugriff auf die mehrstufige Webanwendung unter in der Cloud für externe Benutzer mit AAD Kennwortauthentifizierung bereitstellen.

- Zugriff auf die mehrstufige Webanwendung, die in der Cloud auf Benutzer, die innerhalb der Organisation mit Kennwortauthentifizierung bereitstellen AAD ausführen ausgeführt.

### <a name="prerequisites"></a>Erforderliche Komponenten

Welche Schritte ausführen angenommen, die folgenden Komponenten:

- Sie haben ein vorhandenes Azure Abonnement, in dem Sie eine Ressourcengruppen erstellen können.

- Die [Azure Line Benutzeroberfläche]installiert haben[azure-cli].

- Sie haben die heruntergeladen und installiert die neueste erstellen. Finden Sie [hier] [ azure-powershell-download] Anweisungen.

- Sie haben eine Kopie der [Makecert] installiert[ makecert] Utility auf dem Testclientcomputer.

- Sie haben Zugriff auf eine Entwicklungscomputer, der Visual Studio installiert ist.

### <a name="create-the-n-tier-web-application-architecture"></a>Erstellen der Web mehrstufige Anwendungsarchitektur

1. Erstellen Sie einen Ordner mit dem Namen `Scripts` , enthält einen Unterordner namens `Parameters`.

2. Herunterladen der [Bereitstellen-ReferenceArchitecture.ps1] [ solution-script] PowerShell-Skript in den Ordner Skripts.

3. Erstellen Sie einen weiteren Unterordner mit dem Namen Windows im Ordner Parameter.

4. Laden Sie die folgenden Dateien auf Parameter/Windows-Ordner an:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Bearbeiten Sie die Datei **Bereitstellen-ReferenceArchitecture.ps**1 in den Ordner Skripts, und ändern Sie die folgende Zeile aus, um die Ressourcengruppe angeben, die erstellt oder verwendet, um die virtuellen Computer und Ressourcen erstellt, das Skript enthalten sein sollte:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Bearbeiten Sie die folgenden Dateien im Ordner s **Parameter/Fenster**, und legen Sie die `resourceGroup` Wert in der `virtualNetworkSettings` Abschnitt in jede dieser Dateien identisch sein, die Sie in der Skriptdatei **Bereitstellen-ReferenceArchitecture.ps1** angegeben haben.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Öffnen Sie ein Azure PowerShell-Fenster zu, wechseln Sie zu dem Ordner Skripts und führen Sie den folgenden Befehl aus:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Ersetzen Sie **<subscription id>** mit Ihrer Azure-Abonnement-ID an.

    Für **<location>**, eine Azure Region, z. B. *Eastus* oder *Westus*angeben.

8. Wenn das Skript ausgeführt wurde, verwenden Sie zum Abrufen der öffentlichen IP-Adresse des Web-Ebenen Lastenausgleich (*RAS Aad-Ntier-Web-Projektor*) Azure-Portal:

    [! [18]][18]

9. Melden Sie sich bei Ihrem Test Client Computer (oder virtuellen Computer), und stellen Sie sicher, dass Sie die Web-Ebene zugreifen können, mithilfe von Internet Explorer, um die öffentliche IP-Adresse des Web-Ebenen Lastenausgleich zu suchen. Die IIS-Standardseite sollte angezeigt werden.

### <a name="simulate-configuration-of-a-public-web-site"></a>Konfiguration von einer öffentlichen Website zu reproduzieren

>[AZURE.NOTE] Die folgenden Schritte simulieren des DNS-www.contoso.com durch Ändern der Hosts-Datei auf dem Testclientcomputer die Web-Ebene zuordnen. Darüber hinaus werden die Web-Ebenen virtuellen Computern mit selbstsignierten Zertifikaten und ein Fake Hostinganbieter Zertifizierungsstelle konfiguriert. Dies ist zum Testen der ausschließlich und **Diese Techniken in einer Umgebung für die Herstellung nicht verwendet werden sollen**.

1. Klicken Sie auf dem Clientcomputer bearbeiten Sie die Datei **C:\Windows\System32\drivers\etc\hosts** mit dem Editor, und fügen Sie einen Eintrag aus, der die öffentliche IP-Adresse des Web-Ebenen Lastenausgleich des www.contoso.com zuordnet:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Stellen Sie sicher, dass Sie jetzt mit www.contoso.com vom Testclientcomputer durchsuchen können. Die gleiche IIS-Standardseite sollte wie zuvor angezeigt werden.

3. Klicken Sie auf dem Testclientcomputer öffnen Sie ein Eingabeaufforderungsfenster als Administrator, und verwenden Sie das Programm Makecert bis c
4. Erstellen ein Zertifikat gefälschte Stammzertifizierungsstelle:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Stellen Sie sicher, dass die folgenden Dateien erstellt werden:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Führen Sie zum Installieren der Test-Stammzertifizierungsstelle den folgenden Befehl ein:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Verwenden Sie Zertifizierungsstelle testen, um ein Zertifikat für www.contoso.com generieren:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. Führen Sie den Befehl **Mmc** , und fügen Sie das Zertifikate-Snap-in für das Computerkonto für den lokalen Computer.

7. In der *Zertifikatinhaber (lokaler Computer) / persönlich/Zertifikat/* speichern, exportieren Sie das Zertifikat www.contoso.com mit dem zugehörigen privaten Schlüssel in einer Datei namens www.contoso.com.pfx:

    [! [20]][20]

8. Kehren Sie zu der Azure-Portal zurück, und Verbinden mit der Ebene Management virtueller Computer (*RAS-Aad-Ntier-Management-vm1*). Der Standard-Benutzername ist *Testbenutzer* mit Kennwort *AweS0me@PW*:

    [! [21]][21]
    
9. Schließen Sie von der Ebene Management virtueller Computer an jeden der Web-Stufe virtuellen Computern wiederum mit Benutzername *Testbenutzer* und-Kennwort *AweS0me@PW*, und führen Sie die folgenden Aufgaben. Beachten Sie, dass der virtuelle Computer die privaten Adressen IP-Adresse 10.0.1.4, 10.0.1.5 und 10.0.1.6 haben:

    >[AZURE.NOTE] Die Webebene virtuellen Computern nur verfügen über private IP-Adressen aus. Sie können nur mithilfe der Ebene Management virtueller Computer zu verbinden.

    1. Kopieren Sie die Dateien *www.contoso.com.pfx* und *MyFakeRootCertificateAuthority.cer* aus dem Testclientcomputer.
    
    2. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator, wechseln Sie zu dem Ordner, halten die Dateien, die Sie kopiert haben, und führen Sie die folgenden Befehle:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Führen Sie die `mmc` Befehl, fügen Sie das Zertifikate-Snap-in für das Computerkonto für den lokalen Computer hinzu, und stellen Sie sicher, dass die folgenden Zertifikate installiert wurden:

        - \Certificates (lokaler Computer) \Personal\Certificates\ www.contoso.com

        - \Certificates (lokaler Computer) \Trusted Stamm-Zertifizierung Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Starten Sie die Internet Information Services (IIS) Manager-Verwaltungskonsole, und navigieren Sie zu *Sites\Default-Website* , auf dem Computer.

    5. Im Bereich *Aktionen* auf *Bindungen*und Hinzufügen einer Https-Bindung mit dem www.contoso.com SSL-Zertifikat:

        [! [22]][22]

10. Kehren Sie zu dem Testclientcomputer zurück, und stellen Sie sicher, dass Sie jetzt zu https://www.contoso.com wechseln können.

### <a name="create-an-azure-active-directory-tenant"></a>Erstellen einer Azure Active Directory-Mandanten

1. Erstellen Sie mithilfe des Azure-Portals einem Azure Active Directory-Mandanten, wie z. B. *Myaadname*. onmicrosoft.com, wobei *Myaadname* der Name eines Verzeichnisses von Ihnen ausgewählten ist.

2. Hinzufügen eines Benutzers mit dem Namen *Administrator* mit der Rolle GlobalAdmin ein Verzeichnis. Notieren Sie sich das neu generierte Kennwort.

3. Navigieren Sie zu https://account.activedirectory.windowsazure.com/ mithilfe von Internet Explorer, und melden Sie sich als admin@ *Myaadname*. onmicrosoft.com. Ändern Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.

### <a name="create-and-deploy-a-test-web-application"></a>Erstellen und Bereitstellen einer Webanwendung testen

1. Verwenden Visual Studio auf dem Entwicklungscomputer eine ASP.NET Web-Anwendung, die mit dem Namen ContosoWebApp1 erstellen (mithilfe von .NET Framework 4.5.2):

    [! [23]][23]

2. Wählen Sie die Vorlage *MVC* , ändern Sie die Authentifizierung in *Arbeit und Schule Konten*, und geben Sie den Namen Ihrer AAD Mandanten. Erstellen Sie eine App-Verwaltungsdienst nicht in der Cloud.

    [! [24]][24]

3. Erstellen Sie und führen Sie die Anwendung, um die Authentifizierung zu testen. Geben Sie den Namen des Kontos der Anmeldeseite admin@ *Myaadname*. onmicrosoft.com, geben Sie das Kennwort ein, und klicken Sie dann auf *Anmelden*:

    [! [25]][25]

4. Stellen Sie sicher, dass AAD gefragt, um die Zugriffsberechtigung werden für Sie anmelden, und Lesen Sie Ihr Profil, und klicken Sie dann startet Ausführen der Webanwendung mit der Identität des Administrator.

5. Schließen Sie Internet Explorer, und kehren Sie zu Visual Studio zurück.

6. Öffnen Sie die Datei web.config und in der `<appSettings>` Abschnitt, ändern Sie den Wert des Schlüssels *Ida: PostLogoutRedirectUri* *Https://www.contoso.com:443 /*. Speichern Sie die Datei ein.

7. Die *Lösung Explorer* -Fenster mit der rechten Maustaste in des Projekts ContosoWebApp1, klicken Sie auf *Veröffentlichen*.

8. Klicken Sie im *Web veröffentlichen* klicken Sie auf *Benutzerdefiniert*. Erstellen Sie ein benutzerdefiniertes Profil mit dem Namen *ContosoWebApp1*.

9. Klicken Sie auf der Seite *Verbindung* legen Sie die *Methode veröffentlichen* *File System* , und geben Sie einen Ordner mit dem Namen *ContosoWebApp*, sich an einem geeigneten Ort auf Ihrem Computer befindet.

10. Legen Sie auf der Seite *Einstellungen* der *Konfiguration* auf *Release*.

11. Klicken Sie auf der Seite *Vorschau* auf *Veröffentlichen*.

12. Verbinden mit jedem Webserver wiederum (über die Verwaltungsebene virtueller Computer), und führen Sie die folgenden Aufgaben:

    1. Kopieren Sie die *ContosoWebApp* Ordner und seinen Inhalt in den Ordner *C:\inetpub* vom Entwicklungscomputer ein.

    2. Navigieren Sie mithilfe der Verwaltungskonsole für Internet Information Services (IIS), zur *Website Sites\Default* auf dem Computer.

    3. Klicken Sie im Bereich *Aktionen* klicken Sie auf *Basis-Einstellungen*, und ändern Sie den physischen Pfad der Website zu *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Veröffentlichen Sie die Anwendung durch AAD Web zu testen

1. Melden Sie sich bei der Azure-Portal an, und navigieren Sie zu Ihrem Verzeichnis AAD.

2. Klicken Sie auf die ContosoWebApp1-Anwendung, klicken Sie auf die Registerkarte *Applications* .

3. Stellen Sie sicher, dass das Verzeichnis Ihrer Anwendung erfolgreich hinzugefügt wird, und klicken Sie dann auf die Registerkarte *Konfigurieren* .

4. Ändern Sie die *Melden Sie sich auf URL* in Https://www.contoso.com:443, und legen Sie die *URL der Antwort* auf Https://www.contoso.com:443 (dieselbe URL).

5. Speichern Sie die Konfiguration.

6. Navigieren Sie auf dem Testclientcomputer zu https://www.contoso.com. Stellen Sie sicher, dass AAD für Ihre Anmeldeinformationen aufgefordert werden, und melden Sie sich an.

>[AZURE.NOTE] Dies ist der Endpunkt für das erste Szenario: Aktivieren des Zugriffs auf die mehrstufige Webanwendung unter in der Cloud für externe Benutzer mit AAD Kennwortauthentifizierung bereitstellen.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Erstellen einer simulierten lokalen Umgebung mit Active Directory

Die lokalen Umgebung enthält zwei Domänencontroller für die `contoso.com` Domäne und zwei Server zum Hosten Azure AD verbinden synchronisieren Dienst. Der Server zum Hosten Azure AD verbinden sind nicht Domäne.

1. Im Datei-Explorer zurück in den Ordner Skripts, enthält das Skript verwendet, um die mehrstufige Web-Anwendung zu erstellen.

2. Klicken Sie im Ordner Parameter Hinzufügen einer anderen Sub-Ordner mit dem Namen `Onpremise`.

3. Laden Sie die folgenden Dateien auf Parameter/lokale Edition Ordner an:

    - [hinzufügen-Fügt-Domäne-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Erstellen – addiert-Gesamtstruktur-extension.parameters.json][create-adds-forest-extension-parameters]

    - [VirtualMachines-adc.parameters.json][virtualMachines-adc-parameters]

    - [VirtualMachines-adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [VirtualMachines-adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [VirtualNetwork fügt dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Bearbeiten Sie die Datei bereitstellen-ReferenceArchitecture.ps1 in den Ordner Skripts, und ändern Sie die folgende Zeile aus, um die Ressourcengruppe angeben, die erstellt oder verwendet, um die virtuellen Computer und Ressourcen erstellt, das Skript enthalten sein sollte:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Bearbeiten Sie die folgenden Dateien im Ordner Lokale Parameter/Edition, und legen Sie die `resourceGroup` Wert in der `virtualNetworkSettings` Abschnitt in jede dieser Dateien identisch sein, die Sie in der Skriptdatei bereitstellen-ReferenceArchitecture.ps1 angegeben haben.

    - VirtualMachines-adds.parameters.json

    - VirtualMachines-adc.parameters.json

    - virtualNetwork.parameters.json

    - VirtualNetwork fügt dns.parameters.json

7. Öffnen Sie ein Azure PowerShell-Fenster zu, wechseln Sie zu dem Ordner Skripts und führen Sie den folgenden Befehl aus:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Ersetzen Sie `<subscription id>` mit Ihrer Azure-Abonnement-ID an.

    Für `<location>`, geben Sie eine Azure Region, wie `eastus` oder `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Installieren und Konfigurieren des Diensts Azure AD verbinden synchronisieren

Die Konfiguration, die in den folgenden Schritten dargestellt besteht aus zwei Instanzen des Diensts synchronisieren Azure AD verbinden. Die erste ist aktiv, während das zweite im staging Modus Schnelles Failover und hohen Verfügbarkeit bereitstellen, wenn der erste Server nicht konfiguriert ist.

1. Über das Azure-Portal an, navigieren Sie zu der Ressourcengruppe die virtuellen Computern für die Azure AD verbinden Synchronisierungsdienste (standardmäßig*RAS-Aad-lokale Edition-Rg* ) gedrückt, und Verbinden mit *RAS-Aad-lokale Edition-adc-vm1* virtueller Computer. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

2. Herunterladen von Azure AD-Verbinden von [hier][aad-connect-download].

3. Führen Sie das Installationsprogramm Azure AD verbinden, und führen Sie eine Installation mit der Option *Anpassen* .

    >[AZURE.NOTE] Sie können die Option *Express-Einstellungen* nicht verwenden, da der virtuellen Computer der Hostingdienst Azure AD verbinden synchronisieren nicht Domänenverbund ist.

4. Lassen Sie auf der Seite *erforderliche Komponenten installieren* die Einstellungen optionale Konfiguration leer, um die Standardoptionen zu übernehmen.

5. Wählen Sie auf der Seite *Benutzer anmelden* *Synchronisierung von Kennwörtern*aus.

6. Geben Sie auf der Seite *mit Azure AD* admin@ *Myaadname*. onmicrosoft.com, wobei *Myaadname* den Namen der Ihrem Mandanten AAD ist. Geben Sie das Kennwort für das Admin-Konto an.

7. Geben Sie auf der Seite *Ihre Verzeichnisse verbinden* "contoso.com" für die Gesamtstruktur (Geben Sie den Wert in, da es nicht in der Dropdown-Liste angezeigt wird), geben Sie CONTOSO\testuser für den Benutzernamen, geben Sie AweS0me@PW für das Kennwort ein, und klicken Sie dann auf *Verzeichnis hinzufügen*.

8. Übernehmen Sie den Standardwert für den Benutzerprinzipalnamen, auf der Seite *Azure AD-Anmeldung konfigurieren* . Aktivieren Sie *Weiter ohne überprüften Domänen*.

    >[AZURE.NOTE] Verzeichnis "contoso.com" werden als *Nicht überprüft*aufgeführt. Um einen Domänennamen überprüfen möchten, müssen Sie einen TXT-Eintrag für Ihre Domänennamen Registrierungsstelle erstellen. In diesem Beispiel ist die lokale Domäne nicht extern registriert. Weitere Informationen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamen zu Azure Active Directory][aad-custom-directory].

9. Wählen Sie auf der Seite *Domäne und Organisationseinheit Filtern* *Synchronisieren aller Domänen und Organisationseinheiten* (die Standardeinstellung).

10. Klicken Sie auf der Seite *eindeutig identifizieren die Benutzer* übernehmen Sie die Standardwerte.

11. Wählen Sie auf der Seite *Benutzer und Geräte Filtern* *alle Benutzer und Geräte synchronisieren* (die Standardeinstellung).

12. Wählen Sie auf der Seite *optionale Features* *Kennwort abgeschlossenen writebackvorgängen*ein.

13. Klicken Sie auf der Seite *bereit zum Konfigurieren* wählen Sie *nach Abschluss der Konfiguration der Synchronisierung starten*, aber lassen Sie diese Option deaktiviert *staging-Modus aktivieren* .

14. Stellen Sie sicher, dass der Konfigurationsprozess ohne Fehler abgeschlossen wird, und beenden Sie dann das Installationsprogramm aus.

15. Schließen Sie aus dem Azure-Portal an *RAS-Aad-lokale Edition-adc-vm2* virtueller Computer. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

16. Laden Sie Azure AD-verbinden, und führen Sie das Installationsprogramm aus.

17. Schritt im Assistenten, und Antworten in die Schritte 3 bis 12 beschriebenen.

18. Wählen Sie auf der Seite *bereit zum Konfigurieren* *Starten Sie den Synchronisierungsprozess nach Abschluss der Konfiguration*, und **Wählen Sie auch** *staging-Modus aktivieren*aus. Dies bewirkt, dass des Azure AD verbinden synchronisieren Diensts Details Konten und Objekte aus der Domäne "contoso.com" abgerufen und in der Datenbank zu speichern, aber es nicht diese Details zu Ihrem Mandanten AAD übertragen.

14. Stellen Sie sicher, dass der Konfigurationsprozess ohne Fehler abgeschlossen wird, und beenden Sie dann das Installationsprogramm aus.

### <a name="test-the-aad-configuration"></a>Testen Sie die Konfiguration AAD

1. Verwenden des Azure-Portals an, wechseln Sie zu Ihrem Verzeichnis AAD, öffnen Sie das Blade Azure Active Directory, klicken Sie auf *Benutzer und Gruppen*, und klicken Sie dann auf *alle Benutzer* , um die Liste der Benutzer und Gruppen, die mit dem Verzeichnis synchronisiert anzuzeigen. Sie sollten Benutzer für die folgenden Konten finden Sie unter:

    - Administrator (admin@ *Myaadname*. onmicrosoft.com)

    - Lokalen Verzeichnis Synchronisierung Dienstkontos (Sync_ADC1_*Nnnnnnnnnnnn*@*Myaadname*. onmicrosoft.com)

    - Lokalen Verzeichnis Synchronisierung Dienstkontos (Sync_ADC2_*Nnnnnnnnnnnn*@*Myaadname*. onmicrosoft.com)

2. Klicken Sie im Azure-Portal navigieren Sie zu der Ressourcengruppe die virtuellen Computern für die AD DS-Domäne-Controller (standardmäßig*RAS-Aad-lokale Edition-Rg* ) gedrückt, und Verbinden mit *RAS-Aad-lokale Edition-Ad-vm1* virtueller Computer. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

3. Mithilfe der Verwaltungskonsole *Active Directory-Benutzer und Computer* mit dem Namen Diane Tibbot, mit dem Anmeldenamen einen neuer Domänenbenutzer hinzufügen dianet@contoso.com. Geben Sie ein Kennwort Ihrer Wahl. Stellen Sie dem Benutzer ein Mitglied der Gruppe Administratoren (Dies ist, so dass Sie lokal als dieser Benutzer später anmelden können – die nur Computer in der Domäne DCs sind).

4. Wechseln Sie zu der *RAS-Aad-lokale Edition-adc-vm1* virtueller Computer, öffnen Sie ein PowerShell-Fenster, und führen Sie die folgenden Befehle:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Stellen Sie sicher, dass der Befehl *Erfolg*zurückgibt.

    >[AZURE.NOTE] Dieser Schritt ist erforderlich, da standardmäßig die Synchronisierung in 30 Minuten Abständen ausführen geplant wurde. Diese Befehle erzwingen, dass eine Synchronisierung sofort erfolgt.

5. Zurückkehren Sie zur Azure-Portal, wechseln Sie zu Ihrem Verzeichnis AAD, Öffnen des Azure-Active Directory-Blades, klicken Sie auf *Benutzer und Gruppen*, und klicken Sie dann auf *alle Benutzer*. Diane Tibbot sollte nun angezeigt werden (dianet@ *Myaadname*. onmicrosoft.com) in der Liste der Benutzer angezeigt werden.

6. Klicken Sie in das Blade Azure Active Directory auf *Enterprise-Anwendungen*, und klicken Sie dann auf *Alle Programme*. Klicken Sie auf die *ContosoWebApp1* -Anwendung, und klicken Sie dann auf *Benutzer und Gruppen*. Klicken Sie in der Symbolleiste auf *Hinzufügen*. Klicken Sie auf *Benutzer und Gruppen*, und wählen Sie *Diane Tibbot*aus. Klicken Sie auf *zuweisen*.

7. Navigieren Sie mithilfe von Internet Explorer zu der Website https://account.activedirectory.windowsazure.com. Melden Sie sich als dianet@ *Myaadname*. onmicrosoft.com mit dem Kennwort, die Sie zuvor angegeben haben.

    >[AZURE.NOTE] Klicken Sie auf das Symbol ContosoWebApp in der Liste der Anwendungen nicht. AAD sucht nach der Anwendung an der öffentlich aufgeführten DNS-Adresse für www.contoso.com, unterscheidet sich von der Adresse des Ihrer Lastenausgleich Web-Ebenen.

8. Klicken Sie auf die Registerkarte *Profil* . Die Details des Benutzers (Wenn Sie eine angegeben, wenn er erstellt wurde) sollte angezeigt werden.

    >[AZURE.NOTE] Wenn Sie klicken Sie auf *Kennwort ändern*, sind nicht zulässig, um diese Aufgabe auszuführen, wie dieser Vorgang zuerst von einem Administrator aktiviert werden muss. Weitere Informationen finden Sie unter [Erste Schritte mit dem Kennwort Management][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Aktivieren der lokale Benutzer zum Ausführen der Anwendung mithilfe der Authentifizierung über AAD

1. Kehren Sie zu der *RAS-Aad-lokale Edition-Ad-vm1* Domänencontrollercomputer zurück.

2. Öffnen Sie die *DNS-Manager* -Verwaltungskonsole, und fügen Sie einen neuen Hosteintrag für www.contoso.com. Geben Sie die öffentliche IP-Adresse des Web-Ebenen Lastenausgleich.

3. Kopieren Sie die Datei *MyFakeRootCertificateAuthority.cer* vom Testclientcomputer (Sie erstellt diese Dateien in der Prozedur [simulieren die Konfiguration von einer öffentlichen Website](#simulate-configuration-of-a-public-web-site)
    
4. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator, wechseln Sie zu dem Ordner, halten die Datei, die Sie soeben kopiert haben, und führen Sie den folgenden Befehl aus:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Navigieren Sie mithilfe von Internet Explorer zu https://www.contoso.com. Stellen Sie sicher, dass die AAD Anmeldeseite für die Webanwendung ContosoWebApp1 angezeigt wird.

6. Melden Sie sich als dianet@ *Myaadname*. onmicrosoft.com. Die Anwendung sollte ausführen und Sie ordnungsgemäß anmelden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, die bewährte Methoden für die [Erweiterung Ihrer lokalen ADDS-Domäne zu Azure][adds-extend-domain]

- Erfahren Sie, die bewährte Methoden zum [Erstellen einer ADDS Ressourcengesamtstruktur] [ adds-resource-forest] in Azure

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Cloud Identität Architektur mit Azure Active Directory"
[1]: ./media/guidance-identity-aad/figure2.png "Einzelne Gesamtstruktur, einzelner Sie AAD Directory Suchtopologie"
[2]: ./media/guidance-identity-aad/figure3.png "Mehrere Gesamtstrukturen, einzelnen AAD Directory Suchtopologie"
[4]: ./media/guidance-identity-aad/figure5.png "Staging Server Suchtopologie"
[5]: ./media/guidance-identity-aad/figure6.png "Mehrere AAD Verzeichnisse Suchtopologie"
[6]: ./media/guidance-identity-aad/figure7.png "Markieren eine benutzerdefinierte Installation von Azure AD verbinden synchronisieren mit einer bestimmten Instanz von SQL Server"
[7]: ./media/guidance-identity-aad/figure8.png "Angeben der SSO-Methode für Benutzer anmelden"
[8]: ./media/guidance-identity-aad/figure9.png "Angeben der Domäne und Organisationseinheit Filteroptionen"
[9]: ./media/guidance-identity-aad/figure10.png "Aktivieren das Kennwort schreiben-back"
[10]: ./media/guidance-identity-aad/figure11.png "Azure AD-Management vorher in das portal"
[11]: ./media/guidance-identity-aad/figure12.png "Verbinden von Azure AD-Konsole"
[12]: ./media/guidance-identity-aad/figure13.png "Die Registerkarte Vorgänge in der Synchronisierung Dienst-Manager"
[13]: ./media/guidance-identity-aad/figure14.png "Der Verbinder Registerkarte Synchronisierung Dienst-Manager"
[14]: ./media/guidance-identity-aad/figure15.png "Die Synchronisierung Regel-Editor"
[15]: ./media/guidance-identity-aad/figure16.png "Vorher Azure Active Directory verbinden Dienststatus in die Synchronisierung Dienststatus mit Azure-portal"
[16]: ./media/guidance-identity-aad/figure17.png "Vorher Azure Active Directory verbinden Dienststatus in der Azure-Portal mit AD DS-Dienststatus"
[17]: ./media/guidance-identity-aad/figure18.png "Berichte zur Sicherheit Azure-Portal zur Verfügung."
[18]: ./media/guidance-identity-aad/figure19.png "Die Hervorhebung der öffentlichen IP-Adresse des Web-Ebenen Lastenausgleich Azure-portal"
[19]: ./media/guidance-identity-aad/figure20.png "Verwenden von Internet Explorer, um die öffentliche IP-Adresse des Web-Ebenen Lastenausgleich suchen"
[20]: ./media/guidance-identity-aad/figure21.png "Die Zertifikate-Snap-in das Zertifikat www.contoso.com mit"
[21]: ./media/guidance-identity-aad/figure22.png "Herstellen einer Verbindung mit der Ebene Management virtueller Computer"
[22]: ./media/guidance-identity-aad/figure23.png "Erstellen die HTTPS-Bindung für die Standardwebsite"
[23]: ./media/guidance-identity-aad/figure24.png "Erstellen der ContosoWebApp1 Web-Anwendung"
[24]: ./media/guidance-identity-aad/figure25.png "Festlegen der Authentifizierungseigenschaften der Anwendung Web ContosoWebApp1"
[25]: ./media/guidance-identity-aad/figure26.png "Anmelden bei Azure AAD aus der Web-Anwendung ContosoWebApp1"
[26]: ./media/guidance-identity-aad/figure27.png "Ändern den Ordner für die Standard-Website"