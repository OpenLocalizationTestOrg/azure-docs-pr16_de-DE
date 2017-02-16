<properties
   pageTitle="Konfigurieren der Leistung Datenverkehr routing Methode | Microsoft Azure"
   description="In diesem Artikel helfen Ihnen die Leistung Datenverkehr routing Methode in den Datenverkehr Manager konfigurieren"
   services="traffic-manager"
   documentationCenter=""
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

# <a name="configure-performance-traffic-routing-method"></a>Konfigurieren der Leistung Datenverkehr routing Methode

Um den Datenverkehr für Cloud-Diensten und Websites (Endpunkte), die sich in verschiedenen Rechenzentren auf der ganzen Welt (auch bekannt als Regionen) befinden, können Sie eingehenden Datenverkehr an den Endpunkt mit den niedrigsten Wartezeit aus den anfragenden Client weiterleiten. Normalerweise Datencenter mit den niedrigsten Wartezeit entspricht der nächsten in geografische Entfernung. Die Leistung Datenverkehr routing Methode können Sie verteilen, basierend auf den niedrigsten Wartezeit jedoch in Konto in Echtzeit Änderungen in Netzwerkkonfiguration oder Laden nicht möglich. Weitere Informationen zu anderen Datenverkehr Weiterleitung Methoden, die Azure Datenverkehr Manager bietet, finden Sie unter [Informationen zu den Datenverkehr Manager Datenverkehr routing Methoden](traffic-manager-routing-methods.md).

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Routing Datenverkehr auf Grundlage der niedrigsten Wartezeit für eine Reihe von Endpunkten:

1. Im klassischen Azure Portal im linken Bereich klicken Sie auf das Symbol **Datenverkehr Manager** zum Öffnen des Bereichs für den Datenverkehr-Manager. Wenn Sie Ihr Profil Datenverkehr Manager noch nicht erstellt haben, finden Sie unter [Den Datenverkehr Manager Profile verwalten](traffic-manager-manage-profiles.md) für die Schritte zum Erstellen eines grundlegenden Datenverkehr Manager Profils.
2. Im Bereich Datenverkehr Manager im Azure klassischen Portal suchen Sie nach dem Datenverkehr-Manager-Profil, das die Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
3. Klicken Sie auf der Seite für Ihr Profil klicken Sie auf die **Endpunkte** am oberen Rand der Seite, und stellen Sie sicher, dass die Endpunkte, die in der Konfiguration enthalten sein sollen vorhanden sind. Schritte zum Hinzufügen oder Entfernen von Endpunkten aus Ihrem Profil finden Sie unter [Verwalten von Endpunkten in den Datenverkehr Manager](traffic-manager-endpoints.md).
4. Klicken Sie auf der Seite für Ihr Profil klicken Sie auf " **Konfigurieren** " nach oben, um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie für **den Datenverkehr routing Methode Einstellungen**, die Datenverkehr routing Methode * *Leistung*aus. Wenn sie nicht der Fall ist, klicken Sie auf * *Leistung** in der Dropdownliste aus.
6. Stellen Sie sicher, dass die **Einstellungen für die Überwachung** ordnungsgemäß konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind nicht den Datenverkehr gesendet werden. Um die Endpunkte zu überwachen, müssen Sie einen Pfad und Dateinamen angeben. Notiz, die eine Weiterleitung Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und setzt voraus, dass die Datei im Stammverzeichnis (Standard) ist. Weitere Informationen zum Überwachen finden Sie unter [Informationen zu den Datenverkehr Manager überwachen](traffic-manager-monitoring.md).
7. Nachdem Sie die Konfiguration Änderungen abgeschlossen haben, klicken Sie am unteren Rand der Seite **zu speichern** .
8. Testen Sie die Änderungen in der Konfiguration aus. Weitere Informationen finden Sie unter [Einstellungen für den Datenverkehr Manager testen](traffic-manager-testing-settings.md).
9. Nachdem Ihr Profil Datenverkehr Manager einrichten und zur Arbeit ist, bearbeiten Sie den DNS-Eintrag auf dem autorisierenden DNS-Server den Namen Ihres Unternehmens auf den Domänennamen Datenverkehr Manager verweisen. Weitere Informationen hierzu finden Sie unter [Punkt einer Unternehmen Internet-Domäne zu einer Domäne den Datenverkehr-Manager](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Nächste Schritte


[Zeigen Sie eine Firma Internet-Domäne auf eine Domäne Datenverkehr-Manager](traffic-manager-point-internet-domain.md)

[Weiterleitung Methoden Datenverkehr-Manager](traffic-manager-routing-methods.md)

[Konfigurieren von Failover routing Methode](traffic-manager-configure-failover-routing-method.md)

[Konfigurieren der Funktion RUNDEN Robert routing Methode](traffic-manager-configure-round-robin-routing-method.md)

[Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)

[Datenverkehr-Manager – deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)

[Datenverkehr Manager – deaktivieren oder Aktivieren von außen liegenden Tabellenblättern](disable-or-enable-an-endpoint.md)

