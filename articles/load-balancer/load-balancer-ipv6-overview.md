<properties
    pageTitle="Übersicht über IPv6 für Azure Lastenausgleich | Microsoft Azure"
    description="Grundlegendes zu IPv6-Unterstützung für Lastenausgleich Azure und Lastenausgleich virtuellen Computern aus."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, Azure Lastenausgleich, zwei Stapel, öffentliche IP-Adresse, native ipv6, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Übersicht über IPv6 für Azure Lastenausgleich

Mit einer IPv6-Adresse können Internet zugänglichen Lastenausgleich bereitgestellt werden. Zusätzlich zu IPv4-Konnektivität dadurch die folgenden Funktionen:

* Systemeigene End-to-End-IPv6-Konnektivität zwischen öffentlichen Internet-Clients und Azure-virtuellen Computern (virtuelle Computer) über den Lastenausgleich.
* Systemeigene End-to-End-IPv6 ausgehende Verbindungen zwischen virtuellen Computern und öffentlichen Internet IPv6-fähige Clients.

Die folgende Abbildung zeigt die IPv6-Funktionalität für Lastenausgleich Azure.

![Azure Lastenausgleich mit IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Nach der Bereitstellung kann ein IPv4 oder IPv6 aktiviert Internet-Client mit der öffentlichen IPv4 oder IPv6-Adressen (oder Hostnamen) des Lastenausgleich Azure internetfähigen kommunizieren. Lastenausgleich leitet IPv6-Pakete an die privaten IPv6-Adressen der virtuellen Computern Netzwerk-Adresse (Netzwerkadressübersetzung) verwenden. Der IPv6-Internet-Client kann nicht direkt mit der IPv6-Adresse der virtuellen Computern kommunizieren.

## <a name="features"></a>Features

Systemeigene IPv6-Unterstützung für virtuelle Computer befinden, die über Azure Ressourcenmanager bietet:

1. Lastenausgleich IPv6 Services für IPv6-Clients im Internet
2. Systemeigener IPv6 und IPv4 Endpunkte auf virtuellen Computern ("zwei gestapelte")
3. Eingehende und ausgehende initiiert systemeigenen IPv6-Verbindungen
4. Unterstützte Protokolle wie etwa TCP, UDP und HTTP (S) aktivieren eine Vielzahl von Service Architekturen

## <a name="benefits"></a>Vorteile

Dieses Feature ermöglicht die folgenden Hauptvorteile:

* Entsprechen Sie die gesetzlichen Vorschriften mit Anforderung, dass neue Applikationen mit reinen IPv6-Clients zugänglich
* Mobile aktivieren und das Internet der Dinge (IOT) Entwickler mit zwei gestapelte (IPv4 + IPv6) Azure virtuellen Computern der wachsende Mobile & IOT Märkten behoben werden?

## <a name="details-and-limitations"></a>Details und Einschränkungen

Details

* Der Azure DNS-Dienst sowohl A IPv4 und IPv6-AAAA Namensdatensätze enthält und reagiert mit beide Datensätze für den Lastenausgleich. Der Client wählt die Adresse (IPv4 oder IPv6) zur Kommunikation mit.
* Wenn ein virtueller Computer eine Verbindung mit einem öffentlichen Internet IPv6-Gerät eingestellt, führt des virtuellen Computers IPv6-Adresse Netzwerkadresse übersetzt (NAT) in den öffentlichen IPv6-Adresse des Lastenausgleich.
* Virtuellen Computern unter dem Betriebssystem Linux müssen konfiguriert sein, um IPv6-IP-Adresse über DHCP erhalten. Zahlreiche Linux Bilder im Katalog Azure sind bereits konfiguriert um IPv6 ohne Änderung zu unterstützen. Weitere Informationen finden Sie unter [Konfigurieren von DHCPv6 für Linux virtuellen Computern](load-balancer-ipv6-for-linux.md)
* Wenn Sie einen Prüfpunkt Dienststatus mit Ihrer Lastenausgleich verwenden, erstellen Sie eine IPv4-Prüfpunkt und verwenden Sie es mit der IPv4 und IPv6 Endpunkte. Wenn der Dienst Ihre virtuellen Computers nach unten wechselt, werden die IPv4 und IPv6 Endpunkte aus Drehung übernommen.

Einschränkungen

* Sie können keine IPv6 laden Lastenausgleich Regeln Azure-Portal hinzugefügt. Die Regeln können nur über die Vorlage CLI, PowerShell erstellt werden.
* Sie können vorhandene virtuelle Computer, um IPv6-Adressen verwenden nicht aktualisieren. Sie müssen die neue virtuellen Computern bereitstellen.
* Eine einzelne IPv6-Adresse kann ein einzelnes Netzwerk Benutzeroberflächen in jeder virtuellen Computer zugewiesen werden.
* Die öffentlichen IPv6-Adressen können ein virtueller Computer zugewiesen werden. Sie können nur ein Lastenausgleich zugewiesen werden.
* Sie können nicht die umgekehrte DNS-Suche für Ihre öffentlichen IPv6-Adressen konfigurieren.
* Die virtueller Computer mit IPv6-Adressen können nicht Azure-Cloud-Dienst angehören. Sie können eine Azure-virtuellen Netzwerk (VNet) verbunden sein und kommunizieren miteinander über deren IPv4-Adressen.
* Privat IPv6-Adressen auf einzelne virtuellen Computern in einer Ressourcengruppe bereitgestellt werden können, aber können nicht in einer Ressourcengruppe über Maßstab Sätze bereitgestellt werden.
* Azure-virtuellen Computern können keine Verbindung über IPv6 zu anderen virtuellen Computern, andere Azure Services oder lokale Geräte herstellen. Sie können nur mit einem Azure Lastenausgleich über IPv6 kommunizieren. Sie können jedoch mit dieser Weitere Ressourcen, die Verwendung von IPv4 kommunizieren.
* Netzwerk-Sicherheitsgruppe (NSG) Schutz für IPv4 wird in zwei-Stapel (IPv4 + IPv6) Bereitstellungen unterstützt. NSGs gelten nicht für die Endpunkte IPv6.
* Der IPv6-Endpunkt des virtuellen Computers wird nicht direkt mit dem Internet verfügbar gemacht. Es ist hinter einem Lastenausgleich. Nur die in den Laden Lastenausgleich Regeln angegebenen Ports werden über IPv6 zugegriffen.
* Ändern den Parameter IdleTimeout für IPv6 wird **derzeit nicht unterstützt**. Die Standardeinstellung ist vier Minuten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie ein Lastenausgleich mit IPv6 bereitstellen.

* [Verfügbarkeit von IPv6 nach region](https://go.microsoft.com/fwlink/?linkid=828357)
* [Bereitstellen einer Lastenausgleich mit IPv6 mithilfe einer Vorlage](load-balancer-ipv6-internet-template.md)
* [Bereitstellen einer Lastenausgleich mit IPv6 Azure PowerShell](load-balancer-ipv6-internet-ps.md)
* [Bereitstellen Sie ein Lastenausgleich mit IPv6 Azure CLI](load-balancer-ipv6-internet-cli.md)
