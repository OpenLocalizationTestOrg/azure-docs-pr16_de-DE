<properties
   pageTitle="Szenarien und Beispiele für das Abonnement Governance | Microsoft Azure"
   description="Enthält Beispiele zum Azure-Abonnement Governance für häufige Szenarien implementieren."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Beispiele für die Implementierung von Azure Enterprise scaffold

Dieses Thema enthält Beispiele für wie ein Unternehmen die Empfehlungen für eine [Azure Enterprise Scaffold](resource-manager-subscription-governance.md)implementieren kann. Ein fiktives Unternehmen, die mit dem Namen "Contoso" verwendet, um bewährte Verfahren für häufige Szenarien veranschaulichen. 

## <a name="background"></a>Hintergrund

Contoso ist ein weltweit Unternehmen, Lieferung Kette Lösungen für Kunden in alle Elemente aus einem Modell "Software als Dienst" zu einem Datenmodell in verpackt ermöglicht, lokal bereitgestellt.  Diese entwickeln Software in der ganzen Welt mit erheblichen Entwicklung Centers in Indien, den USA und Kanada an. 

Der ISV Teil des Unternehmens ist in mehrere unabhängige Business Einheiten unterteilt, die Produkte in einem erheblichen Unternehmen verwalten. Jede Einheit Business verfügt über eine eigene Entwickler, Produktmanager und Architekten. 

Die Enterprise-Technologie-Dienste (legt) Business Unit stellt zentralisierte IT-Funktion und verwaltet mehrere Data Center, wo Business Einheiten deren Applikationen hosten. Zusammen mit der Verwaltung von der Data Center, die Organisation legt bietet und zentralisierte Zusammenarbeit (beispielsweise e-Mail und Websites) und Netzwerk/Telefonie Services verwaltet. Sie verwalten auch unsere Kunden Auslastung für kleinere Unternehmenseinheiten Berechtigung Betrieb Mitarbeiter nicht an. 

In diesem Thema werden die folgenden Rollen verwendet:

- Dieters ist der legt Azure-Administrator.
- Alice ist Contoso Leiter der Entwicklung in der Kette Business an. 

Contoso benötigt, um eine Linie-of-Business-app und einer app für unsere Kunden zu erstellen. Es entschieden hat die apps auf Azure ausgeführt werden. Dieters liest das Thema [Nachschlagewerke Abonnement Governance](resource-manager-subscription-governance.md) und kann nun die Empfehlungen implementieren. 

## <a name="scenario-1-line-of-business-application"></a>Szenario 1: Line-of-Business-Anwendung

Contoso ist ein Quelle Code Management-System (BitBucket) von Entwicklern verwendet werden, auf der ganzen Welt erstellen.  Die Anwendung verwendet Infrastruktur als Service (IaaS) für hostet und Webservern und einem Datenbankserver besteht aus. Entwickler Zugriff auf Servern in ihrer Umgebung Entwicklung, aber nicht benötigten Zugriff auf die Server in Azure. Contoso ETS möchte der Besitzer der Anwendung und Team die Anwendung verwalten können. Die Anwendung steht nur während Contoso Unternehmensnetzwerk. Dieters muss sich das Abonnement für diese Anwendung einrichten. Das Abonnement wird auch in der Zukunft andere Software Developer-bezogene hosten.  

### <a name="naming-standards--resource-groups"></a>Benennen Standards und Ressourcengruppen

Dieters erstellt ein Abonnement Entwicklertools unterstützt, die für alle Business Einheiten sind. Er muss aussagekräftige Namen für die Gruppen Abonnement und Ressourcen (für die Anwendung und die Netzwerke) zu erstellen. Er erstellt die folgenden Gruppen für Abonnement und Ressourcen:

| Element | Namen | Beschreibung |
| ---- | ---- | ----------- |
| Abonnement | Contoso legt DeveloperTools Herstellung | Allgemeine Entwicklertools unterstützt |
| Ressourcengruppe | rgBitBucket | Enthält die Anwendung Webserver und Datenbankserver |
| Ressourcengruppe | rgCoreNetworks | Enthält das virtuelle Netzwerke und die Verbindung zwischen Standorten gateway |


### <a name="role-based-access-control"></a>Rollenbasierte Access-Steuerelement

Dieters möchte nach dem Erstellen Ihrer Abonnements, stellen Sie sicher, dass die entsprechenden Teams und Anwendungsbesitzer ihre Ressourcen zugreifen können. Dieters erkennt, dass jedes Team unterschiedliche Anforderungen verfügt. Er verwendet die Gruppen, die von Contoso lokalen Active Directory (AD) zu Azure Active Directory synchronisiert wurden und die richtige Zugriffsebene für die Teams bietet. 

Dieters weist die folgenden Rollen für das Abonnement: 

| Rolle | Zugewiesen ist | Beschreibung |
| ---- | ----------- | ----------- |
| [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) | Verwaltete ID von Contoso AD | Diese ID wird gesteuert, einfach in Time (JIT) Zugriff über Contoso Identität Verwaltungstool und stellt sicher, dass das Abonnement Besitzer Zugriff vollständig überwacht wird. |
| [Sicherheits-Manager](./active-directory/role-based-access-built-in-roles.md#security-manager) | Sicherheit und Risiken Management Abteilung | Diese Rolle ermöglicht Benutzern das Sicherheitscenter Azure und den Status der Ressourcen anzuzeigen. |
| [Netzwerk Mitwirkender](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Netzwerkteam | Diese Rolle ermöglicht Contosos Network Team das VPN zu anderen Websites und die virtuellen Netzwerke verwalten. |
| *Benutzerdefinierte Rolle* | Besitzer der Anwendung | Dieters erstellt eine Rolle aus, die die Möglichkeit zum Ändern von Ressourcen innerhalb der Ressourcengruppe gewährt. Weitere Informationen finden Sie unter [Benutzerdefinierte Rollen in Azure RBAC](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Richtlinien

Dieters weist die folgenden Anforderungen für die Verwaltung von Ressourcen in das Abonnement:

- Da die Entwicklungstools Entwickler in der ganzen Welt unterstützt, wird nicht er Benutzer erstellen von Ressourcen in einem beliebigen Bereich blockieren möchten. Allerdings muss er wissen, wo die Ressourcen erstellt werden. 
- Er ist betreffenden mit Kosten. Möchte er daher, um zu verhindern, dass die Anwendungsbesitzer unnötigerweise teure virtuellen Computern erstellen.  
- Da diese Anwendung Entwickler in viele Business Einheiten dient, möchte er jeder Ressource mit den Besitzer des Unternehmens Einheit und Anwendung zu markieren. Mithilfe dieser Kategorien können legt die entsprechenden Teams zurück.

Er erstellt die folgenden [Richtlinien Ressourcenmanager](resource-manager-policy.md): 

| Feld | Effekt | Beschreibung |
| ----- | ------ | ----------- |
| Speicherort | Audit | Überwachen Sie die Erstellung von Ressourcen in einer beliebigen region |
| Typ | Verweigern | Verweigern Sie Entstehung G-Serie virtuellen Computern |
| Kategorien | Verweigern | Anwendung Besitzer Kategorie erforderlich |
| Kategorien | Verweigern | Kosten Center Kategorie erforderlich |
| Kategorien | Anfügen | Anfügen von Tagname **Geschäftseinheit** und Tagwert **legt** für alle Ressourcen |


### <a name="resource-tags"></a>Ressourcen-Kategorien

Dieters weiß, dass er auf der Rechnung die Kostenstelle zur Durchführung BitBucket identifizieren müssen bestimmte Informationen verfügbar sind muss. Darüber hinaus möchte Dieters alle Ressourcen kennen, die legt besitzt. 

Er fügt die folgenden [Kategorien](resource-group-using-tags.md) auf die Ressourcengruppen und Ressourcen. 
 
| Tagname | Wert der Kategorie |
| -------- | --------- |
| ApplicationOwner | Der Name der Person, die diese Anwendung verwaltet. |
| CostCenter | Die Kostenstelle der Gruppe, die für die Azure Ernährung bezahlt. |
| Geschäftseinheit | **Legt** (der Business Unit mit dem Abonnement verknüpft ist) |

### <a name="core-network"></a>Core Netzwerk

Das Contoso ETS Informationen Sicherheits- und Risiko-Team prüft Dieterss vorgeschlagenen Plan die Azure-Anwendung zu verschieben. Sie möchten sicherstellen, dass die Anwendung nicht mit dem Internet verbunden ist.  Dieters, weist ebenfalls Entwicklertools apps, die in der Zukunft in Azure verschoben werden. Diese apps erfordern öffentliche Schnittstellen.  Wenn Sie diese Anforderungen erfüllt, stellt er internen und externen virtuelle Netzwerke und eine Netzwerksicherheitsgruppe zum Einschränken des Zugriffs.

Er erstellt die folgenden Ressourcen:

| Ressourcenart | Namen | Beschreibung |
| ------------- | ---- | ----------- |
| Virtuelles Netzwerk | vnInternal | Mit der Anwendung BitBucket verwendet und über ExpressRoute mit Contoso Unternehmensnetzwerk verbunden ist.  Ein Subnetz (SbBitBucket) enthält die Anwendung mit einem bestimmten IP-Adresse Leerzeichen. |
| Virtuelles Netzwerk | vnExternal | Verfügbar für zukünftige Applikationen, die Endpunkte öffentlich zugänglichen erfordern. |
| Netzwerk-Sicherheitsgruppe | nsgBitBucket | Stellt sicher, dass die Angriffsfläche von diesem Arbeitsbelastung minimiert ist, indem Sie Verbindungen nur auf Port 443 für das Subnetz, in dem die Anwendung auf (SbBitBucket) verfügbar ist. |

### <a name="resource-locks"></a>Sperren für Ressourcen 

Dieters erkennt, dass die Verbindung zwischen Contoso Unternehmensnetzwerk und dem internen virtuelle Netzwerk aus unbeabsichtigtes löschen oder fehlerhaften Skripts geschützt werden muss. 

Er erstellt die folgenden [Ressourcen Sperren](resource-group-lock-resources.md):

| Sperrtyp | Ressource | Beschreibung |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Verhindert, dass Benutzer die virtuelle Netzwerk oder Subnets löschen, aber nicht verhindert, dass das Hinzufügen neuer Subnetze. |

### <a name="azure-automation"></a>Azure Automatisierung 

Dieters hat nichts für diese Anwendung zu automatisieren. Obwohl er ein Automatisierung Azure-Konto erstellt haben, wird nicht er Anfangs verwenden. 

### <a name="azure-security-center"></a>Azure-Sicherheitscenter 

Contoso IT-Servicemanagement muss schnell erkennen und Risiken zu behandeln. Darüber hinaus soll zu verstehen, welche Probleme auftreten können.  

Damit diese Anforderungen erfüllt, Dieters im [Sicherheitscenter Azure](./security-center/security-center-intro.md)ermöglicht, und ermöglicht den Zugriff auf die Rolle Sicherheits-Manager. 

## <a name="scenario-2-customer-facing-app"></a>Szenario 2: unsere Kunden-app

Business Leiter in der Kette Business weist verschiedene Verkaufschancen Engagement mit Contoso Kunden erhöhen mithilfe einer Treuekarte identifiziert. Annas Team muss diese Anwendung erstellen und feststellen, dass Azure die Möglichkeit, die geschäftliche Notwendigkeit erfüllen vergrößert wird. Alice funktioniert mit Dieters aus legt zwei Abonnements für die Entwicklung und Betrieb dieser Anwendung konfigurieren.

### <a name="azure-subscriptions"></a>Azure-Abonnements 

Dieters bei Azure Enterprise Portal angemeldet und erkennt, dass die Lieferung Kette Abteilung bereits vorhanden ist.  Wie dieses Projekt das erste Entwicklungsprojekt für das Angebot Kette Team in Azure ist, erkennt Dieters jedoch die Notwendigkeit für Annas Entwicklungsteam ein neues Konto an.  Er erstellt das Konto "R & D" für das Team und weist Access an Alice. Alice über das Konto-Portal anmeldet und erstellt zwei Abonnements: eine, die die Development Server und eine für die Herstellung Server enthalten soll.  Anna umfasst die zuvor definierte naming-Standards beim Erstellen der folgenden Abonnements: 

| Abonnement verwenden | Namen |
| ---------------- | ---- |
| Entwicklung | SupplyChain ResearchDevelopment LoyaltyCard Entwicklung |
| Herstellung | SupplyChain Operationen LoyaltyCard Herstellung |

### <a name="policies"></a>Richtlinien

Dieters und Alice erläutert die Anwendung, und erkennen, dass diese Anwendung nur Kunden in der Region North American dient.  Alice und ihre Mitarbeiter Planen des Azure Service-Umgebung Anwendung und SQL Azure verwenden, um die Anwendung zu erstellen. Möglicherweise müssen sie das Erstellen von virtuellen Computern während der Entwicklung.  Stellen Sie sicher, dass ihre Entwickler die Ressourcen zu durchsuchen und untersuchen Sie Probleme ohne legt herausziehen benötigten ist Heike möchte. 

Für die **Entwicklung Abonnement**erstellen sie die folgenden Richtlinien:

| Feld | Effekt | Beschreibung |
| ----- | ------ | ----------- |
| Speicherort | Audit | Überwachen Sie die Erstellung von Ressourcen in einer beliebigen Region ein. |

Nicht beschränken sie den Typ der Sku, die ein Benutzer in der Entwicklung erstellen kann, und sie keine Kategorien für alle Ressourcengruppen oder Ressourcen erforderlich.

Für die **Herstellung Abonnement**erstellen sie die folgenden Richtlinien:

| Feld | Effekt | Beschreibung |
| ----- | ------ | ----------- |
| Speicherort | Verweigern | Die Erstellung von Ressourcen außerhalb der USA Data Center zu verweigern. |
| Kategorien | Verweigern | Anwendung Besitzer Kategorie erforderlich |
| Kategorien | Verweigern | Erfordern Sie Abteilung Kategorie. | 
| Kategorien | Anfügen | Anfügen von Kategorie zu jeder Ressourcengruppe, der Herstellung Umgebung angibt. |

Nicht beschränken sie den Typ der Sku, die ein Benutzer in der Herstellung erstellen kann.

### <a name="resource-tags"></a>Ressourcen-Kategorien 

Dieters weiß, dass er bestimmte Informationen, um die richtige Business Gruppen für Abrechnung und Besitz zu ermitteln muss. Er definiert Ressource Tags für die Ressourcengruppen und Ressourcen. 
 
Tagname | Wert der Kategorie |
| -------- | --------- |
| ApplicationOwner | Der Name der Person, die diese Anwendung verwaltet. |
| Abteilung | Die Kostenstelle der Gruppe, die für die Azure Ernährung bezahlt. |
| EnvironmentType | **Herstellung** (Obwohl das Abonnement **Herstellung** im Namen enthält, kann dieses Tag einschließlich Identifizierung zu erleichtern beim Ressourcen im Portal oder auf der Rechnung betrachten.) |

### <a name="core-networks"></a>Core Netzwerken

Das Team Contoso ETS Informationen Sicherheit und Risiken prüft Dieterss vorgeschlagenen Plan die Azure-Anwendung zu verschieben. Sie möchten sicherstellen, dass die Anwendung Treuekarte ordnungsgemäß isoliert und in einem DMZ-Netzwerk geschützt ist.  Um diese Anforderung zu erfüllen, Erstellen von Dieters und Alice ein externes virtuelles Netzwerk und eine Netzwerksicherheitsgruppe, die Anwendung Treuekarte aus dem Firmennetzwerk Contoso zu finden.  

Erstellen für die **Entwicklung Abonnement**diese aus:

| Ressourcenart | Namen | Beschreibung |
| ------------- | ---- | ----------- |
| Virtuelles Netzwerk | vnInternal | Die Contoso-Treuekarte Entwicklungsumgebung fungiert und über ExpressRoute mit Contoso Unternehmensnetzwerk verbunden ist. |

Erstellen für die **Herstellung Abonnement**diese aus:

| Ressourcenart | Namen | Beschreibung |
| ------------- | ---- | ----------- |
| Virtuelles Netzwerk | vnExternal | Die Anwendung Treuekarte gehostet und nicht direkt mit Contoso ExpressRoute verbunden ist. Code wird direkt auf die Dienste PaaS über deren Quellcode System abgelegt. |
| Netzwerk-Sicherheitsgruppe | nsgBitBucket | Damit ist sichergestellt, dass die Angriffsfläche von diesem Arbeitsbelastung von Dokumenten, indem nur in Grenze Kommunikation auf TCP 443 minimiert ist.  Contoso ist auch Untersuchung läuft eine Web-Anwendung-Firewall für zusätzlichen Schutz verwenden. |  

### <a name="resource-locks"></a>Sperren für Ressourcen 

Dieters und Alice gewähren möchten, und entscheiden Sie zum Hinzufügen von Ressourcen Sperren auf einige der wichtigsten Ressourcen in der Umgebung auf unbeabsichtigtes löschen während einer Pushbenachrichtigungen fehlerhaften Code zu verhindern. 

Sie erstellen die folgenden sperren:

| Sperrtyp | Ressource | Beschreibung |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Um zu verhindern, dass Personen die virtuelle Netzwerk oder Subnets löschen. Die Sperre verhindert nicht das Hinzufügen neuer Subnetze. |

### <a name="azure-automation"></a>Azure Automatisierung 

Alice und dem Entwicklungsteam haben umfassende Runbooks zum Verwalten der Umgebung für diese Anwendung. Die Runbooks für Addition/löschen Knoten für die Anwendung und andere DevOps Aufgaben ermöglichen. 

Wenn diese Runbooks verwenden möchten, aktivieren sie [Automatisierung](./automation/automation-intro.md)aus.

### <a name="azure-security-center"></a>Azure-Sicherheitscenter 

Contoso-IT-Servicemanagement muss schnell erkennen und Risiken zu behandeln. Darüber hinaus soll zu verstehen, welche Probleme auftreten können.  

Damit diese Anforderungen erfüllt werden können, ermöglicht Dieters Azure Sicherheitscenter. Er wird sichergestellt, dass das Sicherheitscenter Azure die Ressourcen überwachen ist und ermöglicht den Zugriff auf die Teams DevOps und Sicherheit. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen von Vorlagen für Ressourcenmanager, finden Sie unter [bewährte Methoden zum Erstellen von Azure Ressourcenmanager Vorlagen](resource-manager-template-best-practices.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) beigetragen zu diesem Thema.*
