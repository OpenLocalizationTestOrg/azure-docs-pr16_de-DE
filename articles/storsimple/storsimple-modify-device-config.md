<properties 
   pageTitle="Ändern Sie die Konfiguration der StorSimple Gerät | Microsoft Azure" 
   description="Beschreibt, wie den StorSimple-Manager-Dienst verwenden, um ein StorSimple Gerät neu zu konfigurieren, die bereits bereitgestellt wurde." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Verwenden des StorSimple Manager-Diensts so ändern Sie die Konfiguration der StorSimple-Gerät

## <a name="overview"></a>(Übersicht) 

Das Azure klassische Portal **Konfigurieren** Seite enthält alle Geräteparameter, die Sie auf einem Gerät StorSimple neu konfigurieren können, die von einem Dienst StorSimple Manager verwaltet wird. In diesem Lernprogramm wird erläutert, wie Sie die Seite **Konfigurieren** verwenden können, für die folgenden Geräte-Level Aufgaben:

- Ändern des Audiogeräts 
- Ändern von Einstellungen für Zeit 
- Ändern der DNS-Einstellungen 
- Ändern der Netzwerk-Schnittstellen
- Austauschen oder IP-Adressen zuzuweisen

## <a name="modify-device-settings"></a>Ändern des Audiogeräts

Die Einstellungen für Audiogeräte enthalten den Anzeigenamen des Geräts und das Gerät Beschreibung an.

Ein StorSimple Gerät, das mit der StorSimple Manager-Dienst verbunden ist, wird ein Standardname zugewiesen. Der Standardname spiegeln normalerweise die fortlaufende Zahl des Geräts. Beispielsweise gibt ein Standardnamen für das Gerät, der 15 Zeichen lang sein, wie z. B. 8600-SHX0991003G44HT, ist Folgendes:

- **8600** – zeigt das Gerätemodell an.
- **SHX** – zeigt die Fertigung Website an.
- **0991003** - zeigt ein bestimmtes Produkt an.
- **G44HT**- werden die letzten 5 Ziffern zum Erstellen von eindeutigen fortlaufenden Zahlen erhöht. Dies möglicherweise nicht sequenzielle Reihe.

Im klassische Azure-Portal können Sie den Namen des Geräts zu ändern, und weisen sie einen eindeutigen Namen Ihrer Wahl. Der angezeigte Name beliebige Zeichen enthalten und kann maximal 64 Zeichen lang sein.

Sie können auch eine Beschreibung des Geräts angeben. Eine Beschreibung des Geräts können Sie normalerweise den Besitzer und den Standort des Geräts zu ermitteln. Das Beschreibungsfeld muss weniger als 256 Zeichen enthalten.
 
## <a name="modify-time-settings"></a>Ändern von Einstellungen für Zeit

Ihr Gerät muss Zeit mit der Cloud-Service-Anbieter für die Authentifizierung zu synchronisieren. Wählen Sie Ihre Zeitzone aus der Dropdownliste aus, und geben Sie bis zu zwei Network Time Protocol (NTP)-Servern. Der primäre NTP-Server ist erforderlich, und es wird angegeben, wenn Sie Windows PowerShell für StorSimple verwenden, um Ihr Gerät zu konfigurieren. Sie können die standardmäßigen Windows Server **time.windows.com** als dem NTP-Server angeben. Sie können die primäre NTP-Server-Konfiguration über das Azure klassischen Portal anzeigen, aber Sie müssen die Windows PowerShell-Benutzeroberfläche verwenden, um ihn zu ändern.

Die sekundäre NTP-Server-Konfiguration ist optional. Das klassische Portal können Sie um einen sekundären NTP-Server zu konfigurieren. 

Wenn Sie den NTP-Server zu konfigurieren, stellen Sie sicher, dass Ihr Netzwerk den Datenverkehr NTP Übergabe aus Datencenters mit dem Internet zulässt. Wenn Sie einen öffentlichen NTP-Server angeben möchten, müssen Sie sicherstellen, dass Ihr Netzwerk-Firewalls und anderen Sicherheitsgeräten konfiguriert sind, um NTP Datenverkehr an und von externen Netzwerk zulassen. Wenn bidirektionaler NTP Verkehr nicht zulässig ist, müssen Sie einen internen NTP-Server verwenden (ein Windows-Domänencontroller bietet diese Funktion). Wenn Ihr Gerät Zeit nicht synchronisieren kann, es mit der Cloud-Anbieter kommunizieren möglicherweise nicht.

Wechseln Sie zum Anzeigen einer Liste von öffentlichen NTP-Servern im [Web für NTP-Servern](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Was passiert, wenn das Gerät in einer anderen Zeitzone bereitgestellt wird?

Wenn das Gerät in einer anderen Zeitzone bereitgestellt wird, wird die Zeitzone Gerät ändern. Vorausgesetzt, dass alle Sicherung Richtlinien die Zeitzone Gerät verwenden, werden die Sicherung Richtlinien gemäß der neuen Zeitzone automatisch angepasst. Es ist kein Eingriff erforderlich.

## <a name="modify-dns-settings"></a>Ändern der DNS-Einstellungen

Ein DNS-Server wird verwendet, wenn Ihr Gerät versucht, mit der Cloud-Service-Anbieter zu kommunizieren. Für eine hohe Verfügbarkeit müssen Sie während der anfänglichen Gerät Bereitstellung sowohl der primären und sekundären DNS-Server zu konfigurieren. Um die primäre DNS-Server neu zu konfigurieren, müssen Sie die Windows PowerShell-Oberfläche auf Ihrem Gerät StorSimple verwenden.

Wenn den sekundären DNS-Server ändern möchten, können Sie im klassische Azure-Portal verwenden.



## <a name="modify-network-interfaces"></a>Ändern der Netzwerk-Schnittstellen

Ihr Gerät verfügt über sechs Gerät Netzwerk-Schnittstellen, von denen vier 1 Switch und von denen zwei sind 10 Switch. Diese Schnittstellen sind als Daten 0 – 5 Daten gekennzeichnet. Daten 0, Daten 1, Daten 4 und 5 Daten sind 1 GbE, während Daten 2 und 3 von Daten 10 GbE-Netzwerk-Schnittstellen sind.

Konfigurieren von **Netzwerk Benutzeroberflächen-Einstellungen** für jede der Schnittstellen verwendet werden. Um eine hohe Verfügbarkeit sicherzustellen, empfehlen wir, dass Sie mindestens zwei iSCSI und zwei Cloud-aktivierten Schnittstellen auf Ihrem Gerät haben. Wir empfehlen jedoch nicht erforderlich ist, dass nicht verwendete Schnittstellen deaktiviert werden.

Wenn Sie eine der Netzwerkschnittstellen konfigurieren möchten, müssen Sie eine virtuelle IP-Adresse (VIP) konfigurieren.

Daten 0 ist die Cloud standardmäßig aktiviert. Wenn Sie Daten 0 konfigurieren zu können, müssen Sie zwei feste IP-Adressen, eine für jeden Controller konfigurieren. Diese festen IP-Adressen können verwendet werden, um die Gerätecontroller direkt zugreifen und sind nützlich, wenn Sie Updates installieren, auf dem Gerät oder beim Zugriff auf die Controller zum Zweck der Problembehandlung.

In StorSimple 8000 Reihe Update 1, wird die Weiterleitung Metrik Datenseite 0 festgelegt zum niedrigsten Wert; daher, wenn Ihr Gerät StorSimple 8000 Reihe Update 1 ausgeführt wird, der Cloud-den Datenverkehr durch die Daten 0 geleitet. Notieren Sie sich diesen Wenn Sie mehr als eine Cloud-fähige Netzwerkschnittstelle auf Ihrem Gerät StorSimple haben.

>[AZURE.NOTE] Die festen IP-Adressen für den Controller werden für die Wartung der Updates auf dem Gerät verwendet. Daher müssen die feste IP-Adressen geroutet und eine Verbindung mit dem Internet herstellen.

Für jedes Netzwerk-Schnittstelle werden die folgenden Parameter angezeigt:

- **Geschwindigkeit** – nicht konfigurierbare Parameter. Daten 0, Daten 1, Daten 4 und 5 Daten sind immer Switch 1, Daten 2 und 3 von Daten 10 GbE-Schnittstellen sind.

     >[AZURE.NOTE] Geschwindigkeit und Duplex werden immer automatisch-ausgehandelt. Jumbojet Rahmen werden nicht unterstützt.
 
- **Bundesstaat Schnittstelle** – eine Benutzeroberfläche aktiviert oder deaktiviert werden kann. Wenn aktiviert, versucht das Gerät, die Benutzeroberfläche verwenden. Es empfiehlt sich, dass nur die Schnittstellen, die mit dem Netzwerk verbunden sind, und verwendet aktiviert sein. Deaktivieren Sie alle Schnittstellen, die Sie nicht verwenden.

- **Schnittstelle** – für diesen Parameter, können Sie iSCSI-Verkehr aus der Cloud-Speicherdatenverkehr isolieren. Für diesen Parameter kann eine der folgenden Aktionen aus:

    - **Cloud aktiviert** – Wenn aktiviert, das Gerät diese Schnittstelle verwendet zur Kommunikation mit der Cloud.
    - **iSCSI aktiviert** – Wenn aktiviert, das Gerät diese Schnittstelle verwendet zur Kommunikation mit dem iSCSI-Host.

    Es empfiehlt sich, dass Sie iSCSI-Verkehr aus der Cloud-Speicherdatenverkehr isolieren. Beachten Sie auch, wenn Ihr Host im selben Subnetz wie Ihr Gerät enthalten ist, nicht benötigte einen Gateway zuweisen; Wenn Ihre Host in einem anderen Subnetz als Ihr Gerät ist, müssen Sie einen Gateway zuweisen.

- **IP-Adresse** – Dies kann IPv4 oder IPv6 oder beides sein. Die IPv4 und IPv6 Adressfamilien werden für das Gerät Netzwerk-Schnittstellen unterstützt. Wenn Sie IPv4 verwenden, geben Sie eine 32-Bit-IP-Adresse (*xxx.xxx.xxx.xxx*) in Punkt-dezimale Notation. Wenn IPv6 verwenden möchten, geben Sie einfach ein Präfix 4-stelliges und wird eine 128-Bit-Adresse für Ihr Gerät-Schnittstelle basierend auf das Präfix automatisch generiert werden.

- **Subnetz** – Dies bezieht sich auf die Subnetzmaske und über die Windows PowerShell-Schnittstelle konfiguriert ist.

- **Gateway** – Dies ist der Standard-Gateway, die von dieser Schnittstelle verwendet werden soll, wenn versucht wird, die Kommunikation mit Knoten, die nicht innerhalb der gleichen IP-Adressbereichs (Subnetz) sind. Das Standard-Gateway muss in der gleichen Adressbereichs (Subnetz) wie die Schnittstelle IP-Adresse, wie von der Subnetzmaske bestimmt.

- **Feste IP-Adresse** – dieses Feld steht nur, während Sie die Daten 0 konfigurieren Benutzeroberfläche. JOIN-Operationen wie Updates oder das Gerät Problembehandlung müssen Sie möglicherweise direkt Verbindung mit dem Gerätecontroller. Die feste IP-Adresse kann Zugriff auf die aktive und der passive Controller auf Ihrem Gerät verwendet werden.

Sie können über das Azure klassischen Portal neu Controller 0 und 1 Controller konfigurieren.

>[AZURE.NOTE] 
>
>- Um ordnungsgemäße Funktion sicherzustellen, überprüfen Sie die Benutzeroberfläche Geschwindigkeit und Duplex auf den Schalter, dem mit jeder Geräteschnittstelle verbunden ist. Switch-Schnittstellen sollte entweder handeln mit oder für ein Gigabitbereich konfiguriert werden (1000/s) und voll-Duplex sein. Schnittstellen mit langsamer Geschwindigkeit oder halb-Duplex Betrieb führt Leistungsprobleme.
>
>- Um Ausfallzeiten und weniger zu minimieren, wird empfohlen, Portfast auf den einzelnen Ports wechseln zu aktivieren, die an die iSCSI-Netzwerk-Benutzeroberfläche von Ihrem Gerät angeschlossen werden soll. Dadurch wird sichergestellt, dass Netzwerkkonnektivität schnell eingerichtet werden kann, fällt ein.
 
## <a name="swap-or-reassign-ips"></a>Austauschen oder IP-Adressen zuzuweisen

Aktuell, wenn alle Netzwerkschnittstelle auf dem Controller zugewiesen ist eine VIP, die verwendet wird (nach dem gleichen Gerät oder ein anderes Gerät im Netzwerk), fehl klicken Sie dann der Controller über. Daher müssen Sie das richtige Verfahren folgen, wenn Sie Austauschen von VIPs für das Gerät Netzwerk-Schnittstelle, da Sie eine doppelte IP-Situation erstellt werden.

Führen Sie die folgenden Schritte aus, um austauschen oder VIPs nach jedem der Netzwerkschnittstellen zuzuweisen:

#### <a name="to-reassign-ips"></a>Zuweisen von IP-Adressen

1. Deaktivieren Sie die IP-Adresse für beide Schnittstellen.

2. Nachdem die IP-Adressen deaktiviert sind, weisen Sie die neuen IP-Adressen zu den jeweiligen Schnittstellen aus.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [MPIO für Ihr Gerät StorSimple konfigurieren](storsimple-configure-mpio-windows-server.md).

- Erfahren Sie, wie der Dienst StorSimple Manager zum Verwalten von Ihrem Geräts StorSimple zu [verwenden](storsimple-manager-service-administration.md).
     
