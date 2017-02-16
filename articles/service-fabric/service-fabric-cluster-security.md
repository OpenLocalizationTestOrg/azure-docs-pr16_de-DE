<properties
   pageTitle="Einen Dienst Fabric Cluster Secure | Microsoft Azure"
   description="Beschreibt die Sicherheitsszenarien für einen Dienst Fabric Cluster und die anderen Technologien verwendet, um diese Szenarios implementieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Dienst Fabric Cluster Sicherheitsszenarien

Ein Dienst Fabric-Cluster ist eine Ressource, die Sie besitzen. Cluster sollten immer gesichert werden, um zu verhindern, dass nicht autorisierte Benutzer die Verbindung zum Cluster, besonders, wenn sie Produktionsarbeitslasten daran ausgeführt hat. Obwohl es so erstellen Sie einen ungeschützte Cluster möglich ist, wird dieser Vorgang also zulassen alle anonyme Benutzer eine Verbindung herstellen, wenn er Management Endpunkte mit dem öffentlichen Internet verfügbar macht. 

Dieser Artikel enthält eine Übersicht über die Sicherheitsszenarien für Cluster unter Azure oder eigenständigen und die verschiedenen Technologien verwendet, um diese Szenarios implementieren. Die Cluster Sicherheitsszenarien sind:

- Knoten Sicherheit
- Client-zu-Knoten-Sicherheit
- Rollenbasierte Access-Steuerelements (RBAC)

## <a name="node-to-node-security"></a>Knoten Sicherheit
Sichert die Kommunikation zwischen den virtuellen Computern oder Computern im Cluster. Dadurch wird sichergestellt, dass nur Computer, die autorisiert sind, dem Cluster beizutreten in Hostinganbieter Anwendungen und Dienste im Cluster teilnehmen können.

![Diagramm der Kommunikation zwischen Knoten][Node-to-Node]

Cluster auf Azure oder eigenständigen-Cluster unter Windows ausführen können entweder [Zertifikat Sicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) für Windows Server-Computer verwenden.
### <a name="node-to-node-certificate-security"></a>Zertifikat Knoten Sicherheit
Dienst Fabric verwendet x. 509-Serverzertifikate, die Sie als Teil der Typ Knotens Konfigurationen angeben, wenn Sie einen Cluster erstellen. Ein schnellen Überblick über was diese Zertifikate sind und wie Sie erfassen oder dort zu erstellen können, wird am Ende dieses Artikels bereitgestellt.

Zertifikat ist so konfiguriert, dass beim Erstellen des Clusters entweder durch die Azure-Portal, Azure Ressourcenmanager Vorlagen oder einer eigenständigen JSON-Vorlage. Sie können angeben, ein Zertifikat einer primären und eine optionale sekundäre Zertifikat, das für Zertifikat Rollovers verwendet wird. Die primären und sekundären Zertifikate angegebenen sollte als Administrator-Client und schreibgeschützt Client-Zertifikate für [Client-zu-Knoten Sicherheit](#client-to-node-security)angegebenen unterschiedlich sein.

Azure finden Sie unter [Einrichten von einem Cluster mithilfe einer Vorlage Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md) Informationen zum Zertifikat Sicherheit in einem Cluster konfigurieren.

Eigenständige finden Sie unter Windows Server [Sichern von einem eigenständigen Cluster mit x. 509-Zertifikate unter Windows](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Windows Knoten-Sicherheit
Eigenständige finden Sie unter Windows Server [Sichern von einem eigenständigen Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Client-zu-Knoten-Sicherheit
Authentifiziert Clients und sichert die Kommunikation zwischen einem Client und der einzelnen Knoten im Cluster. Diese Art der Sicherheit authentifiziert und sichert die Clientkommunikation, damit ist sichergestellt, dass nur autorisierte Benutzer die Cluster und der Anwendung, die auf dem Cluster bereitgestellt zugreifen können. Clients werden durch ihre Windows Sicherheitsanmeldeinformationen oder deren Sicherheitsanmeldeinformationen Zertifikat eindeutig identifiziert.

![Diagramm-Client-zu-Knoten-Kommunikation][Client-to-Node]

Cluster auf Azure oder eigenständigen-Cluster unter Windows ausführen können entweder [Zertifikat Sicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Client-zu-Knoten Zertifikat Sicherheit
 Client-zu-Knoten Zertifikat ist so konfiguriert, dass beim Erstellen des Clusters entweder über das Azure-Portal Ressourcenmanager Vorlagen oder einer eigenständigen JSON-Vorlage durch Angeben von einem Administrator-Client-Zertifikat und/oder ein Benutzer Client-Zertifikat.  Der Administrator-Client und Benutzer Client-Zertifikate, die Sie angeben sollte die primären und sekundären Zertifikate sich unterscheiden, die Sie für [Sicherheit Knoten](#node-to-node-security)angeben.

Herstellen einer Verbindung mit dem Cluster mithilfe des Zertifikats Administrator Clients haben Vollzugriff auf Verwaltungsfunktionen.  Herstellen einer Verbindung mit dem Cluster mit dem Client-Zertifikat schreibgeschützt Benutzer Clients haben nur Lesezugriff auf Management-Funktionen. Also werden diese Zertifikate für die Rolle Basis Access Steuerelement (RBAC) weiter unten in diesem Artikel beschriebenen verwendet.

Azure finden Sie unter [Einrichten von einem Cluster mithilfe einer Vorlage Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md) Informationen zum Zertifikat Sicherheit in einem Cluster konfigurieren.

Eigenständige finden Sie unter Windows Server [Sichern von einem eigenständigen Cluster mit x. 509-Zertifikate unter Windows](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Client-zu-Knoten Azure Active Directory (AAD) Sicherheit auf Azure
Cluster auf Azure ausführen können auch Zugriff auf die Verwaltung Endpunkte mit Azure Active Directory (AAD) sichern. Informationen zum Erstellen der erforderlichen AAD Elemente, wie sie während der Clustererstellung aufgefüllt und die Cluster danach Herstellen einer Verbindung mit finden Sie unter [Einrichten von einem Cluster, indem Sie mithilfe einer Vorlage Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md) .

## <a name="security-recommendations"></a>Sicherheit Empfehlungen
Für Azure Cluster empfiehlt es sich, mit Kunden und Zertifikate für Knoten Sicherheit authentifiziert AAD Sicherheit zu verwenden.

Für eigenständige Windows Server verwaltet Cluster es wird empfohlen, dass Sie die Windows-Sicherheit mit Gruppe verwenden Konten (GMA) aus, wenn Sie Windows Server 2012 R2 und Active Directory verfügen. Verwenden Sie andernfalls weiterhin Windows-Sicherheit mit Windows-Konten.

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (RBAC)
Access-Steuerelement ermöglicht den Cluster-Administrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern, Verbesserung der Sicherheit von Cluster begrenzen. Zwei Arten von anderen Access-Steuerelement werden für Clients Herstellen einer Verbindung mit einem Cluster unterstützt: Administratorrolle und Benutzerrolle.

Administratoren haben Vollzugriff auf Verwaltungsfunktionen (/ Lese-Funktionen, einschließlich). Benutzer haben standardmäßig nur Lesezugriff Verwaltungsfunktionen (z. B.-Abfragefunktionen, die), und die Möglichkeit, Anwendungen und Dienste zu beheben.

Sie angeben die Administrator- und Clientrollen zum Zeitpunkt der Clustererstellung getrennte Identitäten (Zertifikate, AAD usw.) stellen, indem Sie für jede. Weitere Informationen über die Standardeinstellungen für Access-Steuerelement und zum Ändern der Standardeinstellungen finden Sie unter [rollenbasierte Zugriffssteuerung für Dienst Fabric-Clients](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>X. 509-Zertifikate und Fabric Service
X. 509-digitale Zertifikate werden häufig zum Authentifizieren von Clients und Servern und verschlüsseln und digitales Signieren von Nachrichten. Weitere Details zu diesen Zertifikaten finden Sie unter [Arbeiten mit Zertifikaten](http://msdn.microsoft.com/library/ms731899.aspx).

Einige wichtige Dinge zu beachten:

- Mithilfe von Zertifikaten in Cluster unter Produktionsarbeitslasten sollte mit einem ordnungsgemäß konfigurierten Windows Server Zertifikat Dienst erstellt oder aus einer genehmigten [Zertifizierungsstelle (Certificate Authority, CA)](https://en.wikipedia.org/wiki/Certificate_authority)abgerufen werden.
- Nie verwenden Sie eine temporäre oder testen Sie Zertifikate in der Herstellung, die mit Tools wie MakeCert.exe erstellt werden.
- Sie können ein selbst signiertes Zertifikat, aber sollten nur tun für Test Cluster und nicht in der Herstellung.

### <a name="server-x509-certificates"></a>Server x. 509-Zertifikate

Serverzertifikate sollen primären einen Server (Knoten) mit Clients authentifizieren oder Authentifizieren von einem Server (Knoten) auf einem Server (Knoten). Eines der anfänglichen Prüfungen bei einem Kunden oder Knoten einen Knoten authentifiziert ist so überprüfen Sie den Wert aus dem allgemeinen Namen in das Feld Betreff. Diese allgemeine Name oder eine alternative Namen für die Zertifikate Betreff muss in der Liste der zulässigen Allgemeine Namen vorhanden sein.

Im folgende Artikel beschrieben, wie Sie Zertifikate mit Betreff alternativen Namen (SAN) erstellen: [zum Hinzufügen eines alternativen Namens zu einem sicheren LDAP-Zertifikat](http://support.microsoft.com/kb/931351).

Das Feld Betreff kann mehrere Werte enthalten, jede mit dem Präfix einer Initialisierung, um den Wert anzugeben. In den meisten Fällen ist die Initialisierung "CN" Gemeinsamer Name; beispielsweise "CN = www.contoso.com". Es kann auch für das Feld Betreff leer zu sein. Wenn das optionale Feld mit Alternativnamen ausgefüllt wird, müssen sie sowohl den allgemeinen Namen des Zertifikats und einem Eintrag pro alternativen Namen enthalten. Diese werden als DNS-Name Werte eingegeben.

Der Wert im Feld Beabsichtigte Zwecke des Zertifikats sollte einen geeigneten Wert, wie etwa "Serverauthentifizierung" oder "Client-Authentifizierung" enthalten.

### <a name="client-x509-certificates"></a>Client x. 509-Zertifikate

Client-Zertifikate werden nicht in der Regel von einer Drittanbieter-Zertifizierungsstelle ausgestellt. Stattdessen enthält persönliche Speicher des der aktuellen Position des Benutzers in der Regel Client-Zertifikate platziert es von einer Stammzertifizierungsstelle, mit einem beabsichtigten Zweck der "Client-Authentifizierung" aus. Beim gemeinsamen Authentifizierung erforderlich ist, kann der Client einem solchen Zertifikat verwenden.

>[AZURE.NOTE] Alle Management-Vorgänge in einem Dienst Fabric Cluster erfordern Serverzertifikate. Client-Zertifikate können für die Verwaltung verwendet werden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel enthält grundlegende Informationen zur Clustersicherheit. Nächste, [Erstellen Sie einen Cluster in Azure mithilfe einer Vorlage Ressourcenmanager](service-fabric-cluster-creation-via-arm.md) oder über das [Azure-Portal](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
