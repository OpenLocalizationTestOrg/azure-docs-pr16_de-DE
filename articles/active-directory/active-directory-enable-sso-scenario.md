<properties
    pageTitle="Verwalten von Applications with Azure-Active Directory | Microsoft Azure"
    description="Dieser Artikel die Vorteile der Integration von Azure Active Directory mit Ihrem lokalen, Cloud und SaaS Applications."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Verwalten von Applications with Azure-Active Directory

Phasen der ist-Workflow Inhalt haben Unternehmen zwei grundlegende Anforderungen für alle Programme auf:

1. Um die Produktivität zu steigern, sollten Applikationen einfach zu ermitteln und Zugriff

2. So aktivieren Sie Sicherheit und Governance sollte das Unternehmen die Kontrolle und Aufsicht auf, wer tatsächlich greift jede Anwendung und kann

In der Welt der Cloud Applikationen kann dies am besten erreicht werden Identität zu steuern, "*Wer darf Möglichkeiten, die*" verwenden.

Bei der Berechnung Terminologie an:

- *Wer* wird als *Identität* – die Verwaltung von Benutzern und Gruppen bezeichnet.

- *Was* ist unter *Access Management* – die Verwaltung des Zugriffs auf geschützte Ressourcen bekannt

Beide Komponenten werden zusammen als *Identität und Access Management (IAM)*, die von der [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) Group als definiert ist bekannt "*die Sicherheit Disziplin, die die richtigen Personen Zugriff auf die richtigen Ressourcen rechts ermöglicht Zeiten aus den richtigen Gründen*".

OK, was ist das Problem? Wenn IAM an einem Ort mit einer integrierten Lösung *nicht verwaltet* wird:

- Identität Administratoren müssen einzeln erstellen und Aktualisieren von Benutzerkonten in allen Programmen separat, eine Aktivität redundante und Zeit in Anspruch nehmen.

- Benutzer müssen mehrere Anmeldeinformationen ein, um die Applikationen Zugriff für die Arbeit mit benötigten merken. Daher meist Benutzer ihre Kennwörter notieren oder verwenden Sie andere Kennwort Management-Lösungen anderen Risiken Daten vorgestellt.

- Redundant, Zeit in Anspruch nehmen Aktivitäten reduzieren die Menge der Zeit Benutzer und Administratoren auf geschäftliche Aktivitäten, die untere Zeile Ihres Unternehmens erhöhen arbeiten.

Ja, was verhindert im Allgemeinen Organisationen von Lösungen für integrierte IAM eingeführt?

- Die meisten technische Lösungen basieren auf Softwareplattformen, die bereitgestellt und von jeder Organisation für eigene Applikationen angepasst werden müssen.

- Cloudanwendungen werden häufig mit einer höheren Rate als IT-Abteilung mit vorhandenen IAM Lösungen integrieren kann übernommen.

- Sicherheit und Tools für die Überwachung erfordern weitere Anpassung und Integration umfassende E2E Szenarien zu erzielen.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure-Active Directory integriert

Azure-Active Directory ist Microsoft umfassende Identität as a Service (IDaaS) Lösung, die:

- Ermöglicht IAM als Cloud-Dienst 

- Ermöglicht die Verwaltung der zentralen Zugriff, einmalige Anmelden (SSO) und Berichtstools 

- Unterstützt integrierte Access Management für [Tausende von Applications](https://azure.microsoft.com/marketplace/active-directory/) in der Galerie, einschließlich Vertrieb, Google Apps, Feld, Concur und mehr. 


Mit Azure Active Directory alle Programme, die Sie für Ihren Partnern veröffentlichen und Kunden (Business oder Consumer) weisen die gleiche Identität und Verwaltungsfunktionen zugreifen.<br> So können Sie die Betrieb Kosten erheblich reduziert.

Was passiert, wenn Sie eine Anwendung implementieren, die noch nicht in der Galerie aufgeführt wird müssen? Während sich ein wenig mehr Zeit als das Konfigurieren von SSO für Applikationen aus dem Katalog Anwendung handelt, bietet Azure AD einen Assistenten, der Sie mit der Konfiguration unterstützen.

Der Wert von Azure AD nicht ausreichend "einfach" Cloud Applications. Sie können Sie durch die Bereitstellung von sicheren Remotezugriff auch mit lokalen Anwendungen verwenden. Mit sicheren Remotezugriff, Sie können verhindern, die die Notwendigkeit der VPN oder andere herkömmliche RAS Implementierung erläutert.

Durch die Bereitstellung zentrale Verwaltung und für einmaliges Anmelden (SSO) für alle Ihre Applikationen, stellt Azure AD die Lösung für das Hauptfenster Daten Sicherheit und Produktivität Probleme.

- Benutzer können mehrere Clientanwendungen mit zugewiesen mehr Zeit Einkommen generieren oder geschäftliche Vorgänge Aktivitäten, die eine Anmeldung zugreifen.

- Identität Administratoren können Zugriff auf Programme an einem Ort verwalten.

Die Vorteile für den Benutzer und für Ihr Unternehmen ist offensichtlich. Werfen Sie die Vorteile Detail für ein Administrator Identität und der Organisation an.

## <a name="integrated-application-benefits"></a>Vorteile der integrierten Anwendung

Der SSO-Prozess umfasst zwei Schritte:

- Die Vorgehensweise zum Überprüfen der Identität des Benutzers Authentifizierung.

- Autorisierung, die Entscheidung zu aktivieren oder den Zugriff auf eine Ressource mit einer Zugriffsrichtlinie.

Bei Verwendung von Azure AD zum Verwalten von Applications und SSO aktivieren:

- Lokal (z. B. AD) oder Azure AD-Konto des Benutzers Authentifizierung durchgeführt.

- Autorisierung führt für die Azure AD-Zuordnung Schutz Richtlinie und konsistente Endbenutzer sicherstellen, und aktivieren auf eine beliebige Anwendung, unabhängig von deren internen Funktionen Zuordnung, Positionen und MFA Geschäftsbedingungen hinzufügen.

Es wichtig, zu verstehen, wie die Autorisierung, klicken Sie auf der Zielanwendung Stellvertreter ist hängt davon ab, wie die Anwendung mit Azure AD integriert wurde.

- **Applikationen vorinstalliert-Dienstanbieters** Diese Arrays sind wie Office 365 und Azure Applications direkt auf Azure AD erstellt und sich darauf verlassen, für deren umfassenden Verwaltungsfunktionen Identität und Access. Zugriff auf diese Anwendungen wird über Verzeichnisinformationen und token Emission aktiviert.

- **Anwendungen von Microsoft und benutzerdefinierte Clientanwendungen vorinstalliert** Hierbei handelt es sich um unabhängige Cloud-Programmen, die auf eine interne Anwendung Directory aufsetzen und können die Steuerung unabhängig von Azure AD. Zugriff auf diese Anwendungen aktiviert ist, indem Sie eine bestimmte Anmeldeinformationen der Anwendung einer Anwendungskonto zugeordnet. Je nach den Funktionen Anwendung möglicherweise die Anmeldeinformationen einen Partnerverbund Token oder Benutzername und Kennwort für ein Konto aus, die zuvor in die Anwendung bereitgestellt wurde.

- **Lokale Anwendungen** Applikationen veröffentlicht über den Azure AD-Anwendungsproxy hauptsächlich den Zugriff auf lokale Applikationen aktivieren. Diese Anwendungen basieren auf einem zentralen auf lokale Verzeichnis wie Windows Server Active Directory. Zugriff auf diese Programme wird durch den Proxy auslösen aktiviert, den Anwendungsinhalt an den Endbenutzer vorführen, während die lokalen Anmeldung Anforderung berücksichtigt.

Wenn ein Benutzer in Ihrer Organisation eintritt, müssen Sie beispielsweise ein Konto für den Benutzer in Azure AD für die primäre anmelden Vorgänge erstellen. Wenn diese Benutzer Zugriff auf eine verwaltete Anwendung wie z. B. Vertrieb erforderlich ist, müssen Sie auch So erstellen Sie ein Konto für diesen Benutzer in Vertrieb und verknüpfen es mit dem Azure-Konto SSO arbeiten vornehmen. Wenn der Benutzer Ihrer Organisation verlässt, es ist ratsam, Löschen des Kontos Azure AD- und alle Gegenstück Konten in die IAM speichert der Programme, die der Benutzer Zugriff auf hatte.

## <a name="access-detection"></a>Access-Erkennung

In modernen Unternehmen sind die IT-Abteilung häufig nicht bewusst die Cloudanwendungen, die verwendet werden. In Verbindung mit der Cloud-App-Suche bietet Azure AD Ihnen eine Lösung für diese Anwendungen zu erkennen.

## <a name="account-management"></a>Verwaltung von Konten

Verwalten von Benutzerkonten in die verschiedenen Anwendung eines manuellen Prozesses ausgeführte Arbeit von traditionell ist IT oder in der Organisation persönliche support. Voll automatisierter Azure AD Account-Management über alle dienstanwendungen Anbieter integriert und diese Applikationen von Microsoft unterstützen Automatisches Benutzer bereitgestellt oder SAML JIT bereits integriert.

## <a name="automated-user-provisioning"></a>Automatisches Benutzer bereitgestellt

Einige Applikationen stellen Automatisierungsschnittstellen für die Erstellung und freistellen (oder Deaktivierung) von Konten bereit. Wenn Sie ein Anbieter eine solche Schnittstelle anbietet, wird es von Azure Active Directory genutzt werden. Dies reduziert die Betrieb Kosten, da Verwaltungsaufgaben automatisch erfolgen, und die Sicherheit Ihrer Umgebung verbessert, da es das Risiko von unbefugten Zugriff wird verringert.

## <a name="access-management"></a>Access-Verwaltung

Azure AD-können Sie den Zugriff auf einzelne mithilfe von Applications oder leistungsgesteuert Zuordnungen Regel verwalten. Sie können auch delegieren an die richtigen Personen in der Organisation Sicherstellung der beste übersichtliche und die Belastung Helpdesk verringert Management zugreifen.

## <a name="on-premises-applications"></a>Lokale Anwendungen

Das integrierte Anwendung Proxy ermöglicht es Ihnen so veröffentlichen Sie Ihre lokalen Applikationen für Ihre Benutzer resultierender beide einheitliche Erfahrung mit modernen Cloud-Anwendung und die Vorteile von Azure AD-Funktionen, Überwachung, Berichterstattung und Sicherheit zugreifen.

## <a name="reporting-and-monitoring"></a>Berichterstattung und Überwachung

Azure AD bietet Ihnen vorab integrierte Berichterstattung und Überwachungsfunktionen, mit denen Sie wissen, der Zugriff auf Anwendungen und wenn sie tatsächlich verwendet hat.

## <a name="related-capabilities"></a>Verwandte Funktionen

Mit Azure AD können Sie Ihre Programme mit Richtlinien präzise und vorab integrierte MFA sichern. Informationen finden Sie unter Weitere Informationen zu Azure MFA [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Erste Schritte

Um anzufangen Integrieren von Applications in Azure AD, sehen Sie sich die [Integration von Azure Active Directory mit Applikationen erste Schritte](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Siehe auch

[Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
