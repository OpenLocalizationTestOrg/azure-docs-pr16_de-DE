<properties
    pageTitle="Synchronisieren von Azure AD verbinden: verstehen und Anpassen der Synchronisierung | Microsoft Azure"
    description="Erläutert, wie Azure AD verbinden synchronisieren funktioniert und so anpassen."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Synchronisieren von Azure AD verbinden: verstehen und Anpassen der Synchronisierung
Die Synchronisierungsdienste Azure Active Directory verbinden (Azure AD verbinden synchronisieren) ist eine zentrale Komponente von Azure AD verbinden. Es sorgt für alle Vorgänge, die für die Synchronisierung Identitätsdaten zwischen Ihrem lokalen Umgebung und Azure AD-verwandt sind. Azure AD verbinden synchronisieren ist der Nachfolger von DirSync, Azure AD synchronisieren und Forefront Identität Manager mit dem Azure Active Directory Connector konfiguriert.

In diesem Thema ist die Homepage für **Azure AD verbinden synchronisieren** (auch **Synchronisieren-Engine**genannt) und enthält Links zu allen anderen Themen im Zusammenhang mit es. Links zu Azure AD verbinden finden Sie unter [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).

Der Synchronisierungsdienst besteht aus zwei Komponenten, die lokal **Azure AD verbinden synchronisieren** Komponente und der Dienstseite in Azure AD **Azure AD verbinden Synchronisierungsdienst**aufgerufen. Der Dienst ist allgemeine für DirSync, Azure AD synchronisieren und Azure AD verbinden.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD verbinden synchronisieren Themen

Thema | Was es behandelt und wann lesen
----- | -----
**Grundlagen von Azure AD verbinden synchronisieren** |
[Grundlegendes zu der Architektur](active-directory-aadconnectsync-understanding-architecture.md) | Für diejenigen, die noch neu bei der Synchronisierung-Engine sind und die Architektur und die verwendeten Begriffe erfahren möchten.
[Technische Konzepte](active-directory-aadconnectsync-technical-concepts.md) | Eine kurze Version des Themas Architektur und kurz erläutert die Konditionen verwendet.
[Topologien für Azure AD verbinden](active-directory-aadconnect-topologies.md) | Beschreibt die verschiedenen Topologien und Szenarien, die die Synchronisierung-Engine unterstützt.
**Benutzerdefinierte Konfiguration** |
[Den Installationsassistenten ausführen erneut](active-directory-aadconnectsync-installation-wizard.md) | Erläutert, welche Optionen zur Verfügung haben, wenn Sie den Assistenten zum Installieren von Azure AD verbinden erneut ausführen.
[Grundlegendes zu deklarative bereitgestellt.](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Beschreibt die Konfigurationsmodell deklarative provisioning bezeichnet.
[Grundlegendes zu deklarative Provisioning Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Beschreibt die Syntax für Ausdruckssprache in deklarativen provisioning verwendet werden.
[Grundlegendes zu der Standard-Konfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)| Beschreibt die Out-of-Box-Regeln und der Standardkonfiguration. Auch beschrieben, wie die Regeln für die Out-of-Box-Szenarien entwickelt zusammenarbeiten.
[Grundlegendes zu Benutzern und Kontakten](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Klicken Sie auf das vorherige Thema weiterhin und beschreibt, wie die Konfiguration für Benutzer und Kontakte zusammen, insbesondere in einer Umgebung mit mehreren Gesamtstrukturen funktioniert.
[Zum Ändern der Standard-Konfigurations](active-directory-aadconnectsync-change-the-configuration.md) | Führt Sie durch So stellen Sie eine allgemeine Konfiguration in Attribut Zahlungen zu ändern.
[Bewährte Methoden zum Ändern der Standard-Konfigurations](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Eingeschränkte Unterstützung und für die Änderung an der Konfiguration Out-of-Box.
[Konfigurieren von Filtern](active-directory-aadconnectsync-configure-filtering.md) | Beschreibt die verschiedenen Optionen Anleitung zum Einschränken der Objekte mit Azure Active Directory synchronisiert und schrittweise so konfigurieren Sie diese Optionen werden sind.
**Features und Szenarios** |
[Unbeabsichtigte Löschen sperren](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Beschreibt das Feature *unbeabsichtigte Löschen sperren* und konfigurieren.
[Scheduler](active-directory-aadconnectsync-feature-scheduler.md) | Beschreibt die integrierten Scheduler, d. h. importieren, synchronisieren, und Exportieren von Daten.
[Implementieren der Synchronisierung von Kennwörtern](active-directory-aadconnectsync-implement-password-synchronization.md) | Beschreibt Funktionsweise der Synchronisierung von Kennwörtern, implementieren, und die Steuerung und Problembehandlung.
[Gerät abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-device-writeback.md) | Beschreibt die Funktionsweise von Gerät abgeschlossenen writebackvorgängen in Azure AD-Verbindung herstellen.
[Directory-Erweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md) | Beschreibt, wie das Schema Azure AD-mit Ihrer eigenen benutzerdefinierten Attributen zu erweitern.
**Sync-Diensts** |
[Azure AD verbinden synchronisieren Service-features](active-directory-aadconnectsyncservice-features.md) | Beschreibt die Synchronisierung Service-Seite und zum Ändern der synchronisierungseinstellungen in Azure AD.
[Doppeltes Attribut Stabilität](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Beschreibt das Aktivieren und **UserPrincipalName** und **ProxyAddresses** Attribut doppelte Werte Stabilität zu verwenden.
**Vorgänge und Benutzeroberfläche** |
[Synchronisation Dienst-Manager](active-directory-aadconnectsync-service-manager-ui.md) | Beschreibt die Synchronisierung Dienst-Manager UI, einschließlich [Vorgänge](active-directory-aadconnectsync-service-manager-ui-operations.md), [Verbinder](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)und [Metaverse Suche](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) Registerkarten an.
[Aspekte und Betriebsaufgaben](active-directory-aadconnectsync-operations.md) | Beschreibt Betrieb bedenken, wie z. B. Wiederherstellung an.
**So wird es gemacht...** |
[Zurücksetzen der Azure AD-Konto](active-directory-aadconnectsync-howto-azureadaccount.md) | So setzen Sie die Anmeldeinformationen des Dienstkontos zum Verbinden von Azure AD verbinden synchronisieren mit Azure AD.
**Weitere Informationen und Verweise** |
[Ports](active-directory-aadconnect-ports.md) | Listet die Ports Sie zwischen der Synchronisierungs-Engine Ihren lokalen Verzeichnissen und Azure AD-zu öffnen müssen.
[Attribute mit Azure Active Directory synchronisiert.](active-directory-aadconnectsync-attributes-synchronized.md) | Enthält alle Attribute, die zwischen der lokalen AD und Azure AD-synchronisiert werden.
[Funktionen Bezug](active-directory-aadconnectsync-functions-reference.md) | Enthält alle Funktionen in deklarativen provisioning verfügbar.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
