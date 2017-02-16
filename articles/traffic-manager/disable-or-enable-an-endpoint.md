<properties
   pageTitle="Einen Endpunkt Datenverkehr Manager aktivieren oder Deaktivieren von | Microsoft Azure"
   description="Dieser Artikel hilft Endpunkte Profil Datenverkehr Manager aktivieren oder deaktivieren."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Einen Endpunkt Datenverkehr Manager aktivieren oder deaktivieren

Sie können auch einzelne Endpunkte deaktivieren, die Teil eines Profils Datenverkehr Manager sind. Endpunkte umfassen sowohl Cloud Services-Websites. Deaktivieren von außen liegenden Tabellenblättern belässt es als Teil des Profils, aber das Profil fungiert, als wäre der Endpunkt nicht darin enthalten ist. Diese Aktion ist dann sehr hilfreich, einen Endpunkt, der Wartungsmodus oder, die erneut bereitgestellt ist vorübergehend entfernt. Wenn Sie der Endpunkt wieder einsatzbereit ist, können sie aktiviert, werden

>[AZURE.NOTE] **Deaktivieren von außen liegenden Tabellenblättern hat nichts mit Bereitstellungszustand in Azure. Ein Endpunkt fehlerfreier bleibt nach oben und zum Empfang von Datenverkehr, auch wenn in den Datenverkehr Manager deaktiviert. Deaktivieren von außen liegenden Tabellenblättern in einem Profil wirkt sich darüber hinaus nicht auf deren Status in einem anderen Profil aus.**

## <a name="to-disable-an-endpoint"></a>Deaktivieren von außen liegenden Tabellenblättern

1. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach den Datenverkehr-Manager-Profil, das die Endpunkt Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
1. Klicken Sie am oberen Rand der Seite auf die **Endpunkte** um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
1. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und klicken Sie dann auf am unteren Rand der Seite **Deaktivieren** .
1. Datenverkehr wird gestoppt parallelen an den Endpunkt basierend auf der DNS-Time-to-Live (TTL) für den Datenverkehr Manager Domänennamen konfiguriert. Sie können die Gültigkeitsdauer auf der Seite Konfiguration des Profils Datenverkehr Manager ändern.

## <a name="to-enable-an-endpoint"></a>Aktivieren von außen liegenden Tabellenblättern


1. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach den Datenverkehr-Manager-Profil, das die Endpunkt Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
1. Klicken Sie am oberen Rand der Seite auf die **Endpunkte** um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
1. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und klicken Sie dann auf am unteren Rand der Seite **Aktivieren** .
1. Datenverkehr wird zum Dienst erneut als vom Profil diktierten parallelen gestartet.

## <a name="next-steps"></a>Nächste Schritte

[Datenverkehr-Manager – deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)

[Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)

[Datenverkehr Manager Leistungsaspekte](traffic-manager-performance-considerations.md)