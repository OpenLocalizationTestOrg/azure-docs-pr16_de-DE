
<properties
   pageTitle="Internet gegenüberliegende laden Lastenausgleich Übersicht | Microsoft Azure "
   description="Übersicht über das Internet gegenüberliegende Lastenausgleich und der zugehörigen Funktionen. Wie funktioniert ein Lastenausgleich für Azure-virtuellen Computern und Cloud-Diensten verwenden."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Internet zugänglichen laden Lastenausgleich (Übersicht)

Azure Lastenausgleich ordnet die öffentliche IP-Adresse und den Port Anzahl von eingehendem der privaten IP-Adresse und den Port Anzahl des virtuellen Computers und umgekehrt für den Datenverkehr Antwort des virtuellen Computers. Laden Lastenausgleich Regeln können Sie bestimmte Arten von Datenverkehr zwischen mehreren virtuellen Computern oder Diensten verteilen. Beispielsweise können Sie die Laden der Web-Anforderung Verkehr über mehrere Webservern oder Webrollen zuweisen.

Für einen Clouddienst, der Instanzen von Webrollen und Worker-Rollen enthält, können Sie einen öffentlichen Endpunkt in der Definition (.csdef) Dienstdatei definieren.

Die _servicedefinition.csdef_ Datei enthält die Endpunktkonfiguration, und wenn Sie mehrere Rolleninstanzen für eine Website oder Arbeitskollegen Rolle Bereitstellung haben, werden der Lastenausgleich dafür einrichten. Die Methode zum Hinzufügen von Instanzen mit der Cloudbereitstellung besteht darin, die Anzahl der Instanzen der Konfiguration Dienstdatei (.csfg) ändern.

Die folgende Abbildung zeigt einen Endpunkt Lastenausgleich für verschlüsselte Webdatenverkehr, der zwischen drei virtuellen Computern öffentlichen und privaten-Standardanschluss 443 freigegeben werden. Diese drei virtuellen Computern sind in einem Satz mit Lastenausgleich.

![Beispiel für Öffentliche laden Lastenausgleich](./media/load-balancer-internet-overview/IC727496.png))

Abbildung 1: Lastenausgleich Endpunkt für verschlüsselte Web-Verkehr

Beim Senden von Internet-Clients Webseite Anfragen an die öffentliche IP-Adresse des Cloud-Dienst auf TCP-Port 443, verteilt der Lastenausgleich Azure die Anfragen zwischen den drei virtuellen Computern mit Lastenausgleich festlegen. Weitere Informationen zu laden Lastenausgleich Algorithmen finden Sie unter den [Lastenausgleich Übersichtsseite zu laden](load-balancer-overview.md#load-balancer-features).

Standardmäßig verteilt Azure Lastenausgleich Netzwerkverkehr gleichmäßig auf mehrere Instanzen von virtuellen Computern. Sie können auch die Sitzung Zugehörigkeit, Weitere Informationen finden Sie unter [Laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Internal Lastenausgleich](load-balancer-internal-overview.md) , um besser zu verstehen, welche Lastenausgleich eine bessere Anpassung für die Cloudbereitstellung ist.

Sie können auch [Erste Schritte beim Erstellen einer gegenüberliegende Lastenausgleich Internet](load-balancer-get-started-internet-arm-ps.md) und welche Art von [Verteilung Modus](load-balancer-distribution-mode.md) für eine bestimmte Auslastung Lastenausgleich Netzwerk Datenverkehr Verhalten konfigurieren.

Wenn die Anwendung benötigt Verbindungen für Server hinter einem Lastenausgleich beibehalten werden soll, können Sie weitere Informationen zu [im Leerlauf TCP Timeouteinstellungen für ein Lastenausgleich](load-balancer-tcp-idle-timeout.md)verstehen. Es wird hilfreich Verbindung im Leerlauf Verhalten wissen möchten, wenn Sie Lastenausgleich Azure verwenden.
