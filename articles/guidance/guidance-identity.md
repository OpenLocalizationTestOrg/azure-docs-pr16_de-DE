<properties
   pageTitle="Verwalten von Identität in Azure | Microsoft Azure"
   description="Erläutert und vergleicht die unterschiedlichen Methoden für die Verwaltung von Identität in Hybrid Systeme, die die lokale/Cloud Begrenzungslinie mit Azure umfassen."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Verwalten von Identität in Azure

In den meisten Enterprise-Systemen, basierend auf Windows verwenden Sie Active Directory (AD) Identität Rechteverwaltungsdienste den Clientanwendungen bereitstellen. AD funktioniert auch in einer lokalen Umgebung, wenn Sie Ihr Netzwerkinfrastruktur in der Cloud erweitern Sie allerdings einige wichtigen Informationen zum Verwalten der Identität im Hinblick vornehmen. Domänen lokalen zum Einbinden von virtuellen Computern in der Cloud erweitert werden soll? Sollten Sie neue Domänen in der Cloud erstellen, und wenn Ja, wie? Sollten Sie Ihre eigenen Gesamtstruktur in der Cloud implementieren oder sollten Sie sicherstellen, der Azure Active Directory (AAD) verwenden?

In diesem Artikel werden einige allgemeinen Optionen für die Probleme, die diesem Szenario vorhandene Besprechung und können, die Sie ermitteln, welche Lösung optimale, wird, Ihre Bedürfnisse Ihren Anforderungen entsprechend.

## <a name="using-azure-active-directory"></a>Verwenden von Azure Active Directory

Sie können AAD verwenden, um eine AD-Domäne in der Cloud erstellen und verknüpfen Sie es mit einer lokalen AD-Domäne. AAD ermöglicht es Ihnen so konfigurieren Sie einmaliges Anmelden (SSO) für Applikationen Zugriff über die Cloud-Benutzer.

[! [0]][0]

AAD ist eine einfache Möglichkeit, eine Sicherheitsdomäne in der Cloud implementiert wird. Es ist von vielen Microsoft-Programmen, z. B. Microsoft Office 365 genutzt werden. 

Vorteile der Verwendung von AAD:

- Es gibt keine müssen Active Directory-Infrastruktur in der Cloud zu verwalten. AAD wird vollständig verwaltet und von Microsoft verwaltet.

- AAD bietet dieselben Identitätsinformationen, die lokale verfügbar ist.

- In Azure, verringern die Notwendigkeit der externen Anwendungen und Benutzer, die lokale Domäne Kontakt mit Ihnen aufnehmen kann Authentifizierung erfolgen.

Aspekte, die bei der Verwendung von AAD beachten:

- Identität Services sind auf Benutzer und Gruppen beschränkt. Es gibt keine Möglichkeit, den Dienst und Computer-Konten authentifizieren.

- Sie müssen mit Ihrer Domäne lokalen Verzeichnis AAD synchronisiert bleiben Connectivity konfigurieren. 

- Sie sind verantwortlich für die Veröffentlichung von Applications, die Benutzer in der Cloud bis AAD zugreifen können.

Ausführliche Informationen finden Sie unter [Implementieren Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Verwenden von Active Directory in der Cloud verknüpft zu einer lokalen Struktur

Sie können Active Directory-Verzeichnisdienst (AD DS) lokal gehostet, aber die in einem Hybriden Szenario, in dem Elemente einer Anwendung in Azure ansässig sind, es effizienter repliziert diese Funktionalität und dem AD Repository in der Cloud. Dieser Ansatz kann die Verzögerung durch Senden Authentifizierung verringern und lokale Autorisierungsanfragen aus der Cloud in AD DS ausgeführt lokalen zurück. 

[! [1]][1]

Dieser Ansatz erfordert, dass Sie Ihre eigene Domäne in der Cloud erstellen und verknüpfen Sie ihn mit der lokalen Gesamtstruktur. Erstellen Sie virtuelle Computer, um die AD DS-Dienste zu hosten.

Vorteile der Verwendung einer eigenen Domänennamens in der Cloud:

- Bietet die Möglichkeit zum Authentifizieren des Benutzers, Service und Computer Konten lokale und in der Cloud.

- Ermöglicht den Zugriff auf die gleichen Identitätsinformationen, die lokale verfügbar ist.

- Es ist nicht erforderlich zum Verwalten von einer separaten Active Directory-Struktur ein. die Domäne in der Cloud kann der lokalen Gesamtstruktur angehören.

- Sie können vom lokalen Gruppenrichtlinienobjekt Objekte die Domäne in der Cloud definierten Gruppenrichtlinien anwenden.

Überlegungen für die Verwendung einer eigenen Domänennamens in der Cloud:

- Müssen Sie über das Erstellen und Verwalten Ihrer eigenen AD DS-Server und die Domäne in der Cloud.

- Möglicherweise gibt es einige Wartezeit Synchronisierung zwischen den Domänenservern in der Cloud und der lokalen-Servern.

Informationen zum Konfigurieren dieser Architektur finden Sie unter [Erweitern von Active Directory Directory Services (ADDS) in Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Verwenden von Active Directory mit einer separaten

Eine Organisation, die Active Directory (AD) lokal ausgeführt wird möglicherweise eine Gesamtstruktur mit zahlreichen Domänen. Verwenden von Domänen Isolationsgrad zwischen Funktionsbereichen bereitstellen, die separate, oftmals aus Sicherheitsgründen aufbewahrt werden müssen, aber Sie können Informationen zwischen Domänen, indem Sie Beziehungen Trust freigeben.

[! [2]][2]

Eine Organisation, die separate Domänen nutzt kann Azure nutzen, indem Sie eine oder mehrere der folgenden Domänen in einer separaten Gesamtstruktur in der Cloud verschieben. Sie können auch möglicherweise eine Organisation möchten, bleiben alle Cloudressourcen aus diesen belegten lokalen logisch verschiedenen, und Informationen zu Cloudressourcen im eigenen Verzeichnis, als Teil einer Struktur frei, die auch in der Cloud speichern.

Vorteile der Verwendung einer separaten Gesamtstruktur in der Cloud:

- Lokale Identitäten implementieren können und trennen Azure nur Identitäten.

- Es ist nicht erforderlich, repliziert aus der lokalen Active Directory-Struktur in Azure, die Auswirkungen der Netzwerkwartezeit verringern.

Aspekte:

- Authentifizierung für lokale Identitäten in der Cloud führt zusätzliche Netzwerk *Abschnitte* in der lokalen AD-Servern.

- Müssen Sie über eine eigene AD DS-Server und implementieren Gesamtstruktur in der Cloud sowie die entsprechenden Trust Beziehungen zwischen Gesamtstrukturen.

[Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure] Dokument[ adds-forest-in-azure] Implementieren dieser Ansatz ausführlicher beschrieben.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Verwenden von Active Directory Federation Services-(ADFS) mit Azure

ADFS lokal ausgeführt werden kann, aber die in einem Hybriden Szenario, in dem Applications in Azure ansässig sind, es effizienter, diese Funktionalität implementieren, in der Cloud, wie unten dargestellt.

[! [3]][3]

Diese Architektur ist besonders hilfreich:

- Lösungen, die partnerverbundkontakte Autorisierung Webanwendungen zum Partnerorganisationen verfügbar machen nutzen.

- Systeme, die von außerhalb der Organisation Firewalls ausgeführt Webbrowsern unterstützen.

- Systeme, die Benutzern der Zugriff auf Webanwendungen durch Herstellen einer Verbindung von autorisierten externen Geräten wie Remotecomputern, Notizbücher und andere mobile Geräte zu ermöglichen. 

Vorteile der Verwendung von ADFS mit Azure:

- Sie können Ansprüche unterstützende Applikationen nutzen.

- Es bietet die Möglichkeit, externe Partner für die Authentifizierung zu vertrauen.

- Es sorgt für Kompatibilität mit großen Satz von Protokolle für die Authentifizierung.

Überlegungen für die Verwendung von ADFS mit Azure:

- Sie müssen Sie Ihre eigenen addiert, ADFS und ADFS-Web-Anwendungsproxy-Servern in der Cloud zu implementieren.

- Diese Architektur kann leichter zu konfigurieren.

Ausführliche Informationen finden Sie unter [Implementierung von Active Directory Federation Services (ADFS) in Azure][adfs-in-azure].

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Ressourcen wird erläutert, wie die in diesem Artikel beschriebenen Architekturen implementiert wird.

- [Implementieren der Azure-Active Directory][implementing-aad]
- [Erweitern Sie in Azure Active Directory-Verzeichnisdienst (ADDS)][extending-adds]
- [Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure][adds-forest-in-azure]
- [Implementierung von Active Directory Federation Services (ADFS) in Azure][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Cloud Identität Architektur mit Azure Active Directory"
[1]: ./media/guidance-identity/figure2.png "Secure Hybrid Netzwerkarchitektur mit Active Directory"
[2]: ./media/guidance-identity/figure3.png "Secure Hybrid Netzwerkarchitektur mit separaten AD-Domänen und Gesamtstrukturen"
[3]: ./media/guidance-identity/figure4.png "Secure Hybrid Netzwerkarchitektur mit ADFS"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md