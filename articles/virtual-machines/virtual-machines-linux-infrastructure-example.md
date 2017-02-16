<properties
    pageTitle="Beispiel für Infrastruktur Exemplarische Vorgehensweise | Microsoft Azure"
    description="Lernen Sie die Key Entwurf und Implementierung von Richtlinien für die Bereitstellung von eine Beispiel-Infrastruktur in Azure aus."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Beispiel für Azure Infrastruktur Exemplarische Vorgehensweise

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

In diesem Artikel durchläuft Baustein ein Beispiel Anwendung Infrastruktur aus. Entwerfen einer Infrastruktur für einen einfachen Online Store, der die Richtlinien und Entscheidungen um Benennungskonventionen, Verfügbarkeit Mengen, virtuelle Netzwerke und Lastenausgleich vereint und tatsächlichen Bereitstellung Ihrer virtuellen Computern (virtuellen Computern) erläutert.


## <a name="example-workload"></a>Beispiel für Arbeitsbelastung

Adventure Works Cycles möchte Anwendung Online Store in Azure zu erstellen, die der besteht aus:

- Zwei Nginx-Servern des Front-End-in eine Stufe Web Clients
- Verarbeitung von Daten und Bestellungen in einer Schicht Anwendung zwei Nginx-servers
- Zwei MongoDB Server Teil einer sharded Cluster zum Speichern von Produktdaten und Bestellungen in einer Datenbankebene
- Zwei Active Directory-Domänencontroller für Kundenkonten und Lieferanten in eine Ebene Authentifizierung
- Alle Server befinden sich zwei Subnetzen:
    - Ein Front-End-Subnetz für die Webserver 
    - ein Back-End-Subnetz für die Anwendungsserver Cluster MongoDB und Domänencontroller

![Diagramm der verschiedenen Ebenen für die Anwendungsinfrastruktur](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Eingehende secure Web-Verkehr muss Lastenausgleich möglich zwischen den Webservern wie Kunden den Online Store durchsuchen. Reihenfolge verarbeitet den Datenverkehr in Form von HTTP anfordert, aus dem Internet, die Servern zwischen die Anwendungsserver Lastenausgleich müssen werden. Darüber hinaus muss die Infrastruktur für eine hohe Verfügbarkeit entworfen werden.

Das sich daraus ergebende Design zu integrieren.

- Zuweisen einer Azure-Abonnement und Konto
- Eine einzelne Ressourcengruppe
- Speicherkonten
- Ein virtuelles Netzwerk mit unterschiedlichen zwei
- Verfügbarkeit legt für die virtueller Computer mit einer ähnlichen Rolle
- Virtuellen Computern

Alle genannten führen Sie die folgenden Benennungskonventionen:

- Adventure Works Cycles verwendet **[IT Arbeitsbelastung]-[Ort]-[Azure Ressource]** als Präfix
    - In diesem Beispiel "**Azos**" (Azure Online Store) der IT Arbeitsbelastung Name und "**Verwenden von**" (ostasiatische US 2) ist der Speicherort
- Verwenden von Speicherkonten Adventureazosusesa**[Beschreibung]**
    - 'Adventure' wurde hinzugefügt, um das Präfix für die Eindeutigkeit, und die Verwendung von Bindestriche Speicher Kontonamen nicht unterstützt.
- Virtuelle Netzwerke verwenden AZOS-verwenden-VN**[Anzahl]**
- Verfügbarkeit Sätze verwenden Azos-verwenden-als-**[Rolle]**
- Namen von virtuellen Computern verwenden Azos-verwenden – virtueller Computer -**[Vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Konten und Azure-Abonnements

Adventure Works Cycles ist mit dem Namen Adventure Works-Konzern-Abonnement, deren Enterprise-Abonnement verwenden, Rechnung für diese IT Arbeitsbelastung bereitstellen.


## <a name="storage-accounts"></a>Speicherkonten

Adventure Works Cycles bestimmt, dass sie zwei Speicherkonten benötigt:

- **Adventureazosusesawebapp** für die standardmäßige Speicherung Webserver, Anwendungsserver, und Domänencontroller und ihre Datenlaufwerke.
- **Adventureazosusesadbclust** für die Speicherung Premium die MongoDB sharded Cluster-Server und ihre Datenfestplatten.


## <a name="virtual-network-and-subnets"></a>Virtuelles Netzwerk und Subnetze

Da das virtuelle Netzwerk nicht mit dem Netzwerk des Adventure Arbeit Cycles lokale laufenden Connectivity benötigt, entschieden Sie sich in einem Cloud nur virtuelle Netzwerk.

Er erstellt ein virtuelles Netzwerk Cloud nur mit den folgenden Einstellungen mit Azure-Portal an:

- Name: AZOS-verwenden – VN01
- Standort: Ostasiatischen USA 2
- Virtuelle Netzwerk Adressbereichs: 10.0.0.0/8
- Erste Subnetz:
    - Name: Front-End
    - Leerzeichen zu beheben: 10.0.1.0/24
- Zweite Subnetz:
    - Name: Back-End-
    - Leerzeichen zu beheben: 10.0.2.0/24


## <a name="availability-sets"></a>Verfügbarkeit von Gruppen

Um hohe Verfügbarkeit der alle vier Ebenen von deren Online Store beibehalten möchten, entschieden Adventure Works Cycles für vier Verfügbarkeit Datenmengen aus:

- **Azos verwenden als Web** für die Webserver
- **Azos verwenden als app** für die Anwendungsserver
- **Azos verwenden als Db** für die Server in der MongoDB sharded cluster
- **Azos verwenden als dc** für die Domänencontroller


## <a name="virtual-machines"></a>Virtuellen Computern

Adventure Works Cycles entschlossen auf die folgenden Namen für ihre Azure-virtuellen Computern:

- **Azos-verwenden – virtueller Computer-web01** für den ersten Webserver
- **Azos-verwenden – virtueller Computer-web02** für den zweiten Webserver
- **Azos-verwenden – virtueller Computer-app01** für den ersten Anwendungsserver
- **Azos-verwenden – virtueller Computer-app02** für den zweiten Anwendungsserver
- **Azos-verwenden – virtueller Computer-db01** für den ersten MongoDB Server im cluster
- **Azos-verwenden – virtueller Computer-db02** für den zweiten MongoDB Server im cluster
- **Azos-verwenden – virtueller Computer-dc01** für den ersten Domänencontroller
- **Azos-verwenden – virtueller Computer-dc02** für den zweiten Domänencontroller

So sieht die sich daraus ergebende Konfiguration aus.

![Endgültige Anwendungsinfrastruktur in Azure bereitgestellt](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Dieser Konfiguration übernimmt:

- Ein Cloud nur virtuelles Netzwerk mit unterschiedlichen zwei (Front-End- und Back-End)
- Zwei Speicherkonten
- Vier Verfügbarkeit Sätze für jede Ebene des Online-Speichers
- Die virtuellen Computer für die vier Ebenen
- Eine Sammlung externen Lastenausgleich für HTTPS-basierten Webdatenverkehr aus dem Internet an den Webserver
- Eine interne Lastenausgleich für unverschlüsselter Web-Verkehr von den Webservern die Anwendungsserver festlegen
- Eine einzelne Ressourcengruppe


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 