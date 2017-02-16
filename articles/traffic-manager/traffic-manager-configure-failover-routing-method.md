<properties
   pageTitle="Konfigurieren Sie den Datenverkehr Manager Failover Datenverkehr routing Methode | Microsoft Azure"
   description="In diesem Artikel helfen Ihnen Failover Datenverkehr routing Methode in den Datenverkehr Manager konfigurieren"
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

# <a name="configure-failover-routing-method"></a>Konfigurieren von Failover routing Methode

Eine Organisation möchte häufig Zuverlässigkeit für seine Dienste bereitzustellen. Dies geschieht, um zusätzliche Dienste für den Fall, dass ihre primäre Service-fällt aus. Ein gemeinsames Muster für Service-Failover besteht darin bieten eine Reihe von identische Dienste und den Datenverkehr einer primären Dienst, bei weitgehender einer konfigurierten Liste ein oder mehrere zusätzliche Dienste senden. Sie können diese Art der Sicherung mit Azure-Cloud-Diensten und Websites anhand der folgenden Verfahren konfigurieren.

Beachten Sie, dass Azure Websites bereits Failover Datenverkehr routing Methode Funktionalität für Websites innerhalb eines Datencenters (auch bekannt als Bereich), unabhängig von den Website-Modus bietet. Datenverkehr-Manager können Sie Failover Datenverkehr routing Methode für Websites in verschiedenen Rechenzentren angeben.

## <a name="to-configure-failover-traffic-routing-method"></a>So konfigurieren Sie Failover Datenverkehr routing Methode

1. Im klassischen Azure Portal im linken Bereich klicken Sie auf das Symbol **Datenverkehr Manager** zum Öffnen des Bereichs für den Datenverkehr-Manager. Wenn Sie Ihr Profil Datenverkehr Manager noch nicht erstellt haben, finden Sie unter [Den Datenverkehr Manager Profile verwalten](traffic-manager-manage-profiles.md) für Schritte zum Erstellen eines grundlegenden Datenverkehr Manager Profils.
2. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach dem Datenverkehr-Manager-Profil, das die Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
3. Auf Ihrer Profilseite, klicken Sie auf die **Endpunkte** am oberen Rand der Seite, und stellen Sie sicher, dass die beiden-Dienste Cloud und Websites (Endpunkte), die in der Konfiguration enthalten sein sollen vorhanden sind. Schritte zum Hinzufügen oder Entfernen von Endpunkten finden Sie unter [Verwalten von Endpunkten in den Datenverkehr Manager](traffic-manager-endpoints.md).
4. Klicken Sie auf Ihrer Profilseite klicken Sie auf " **Konfigurieren** " nach oben, um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie für **den Datenverkehr routing Methode Einstellungen**, die Datenverkehr routing Methode **Failover**aus. Wenn es nicht der Fall ist, klicken Sie auf **Failover** in der Dropdownliste aus.
6. **Priorität Failoverliste**passen Sie die Reihenfolge Ihrer Endpunkte Failover aus. Wenn Sie die **Failover** Datenverkehr routing Methode auswählen, ist die Reihenfolge der ausgewählten Endpunkte wichtig. Der primäre Endpunkt ist ganz oben. Verwenden Sie den Pfeil nach oben und unten weisenden Sie Pfeile, um die Reihenfolge zu ändern, je nach Bedarf. Informationen dazu, wie Sie die Failoverpriorität festzulegen, mithilfe von Windows PowerShell finden Sie unter [Festlegen-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Stellen Sie sicher, dass die **Einstellungen für die Überwachung** ordnungsgemäß konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind nicht den Datenverkehr gesendet werden. Um die Endpunkte zu überwachen, müssen Sie einen Pfad und Dateinamen angeben. Notiz, die eine Weiterleitung Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und setzt voraus, dass die Datei im Stammverzeichnis (Standard) ist. Weitere Informationen zum Überwachen finden Sie unter [Den Datenverkehr Manager überwachen](traffic-manager-monitoring.md).
8. Nachdem Sie die Konfiguration Änderungen abgeschlossen haben, klicken Sie am unteren Rand der Seite **zu speichern** .
9. Testen Sie die Änderungen in der Konfiguration aus. Weitere Informationen finden Sie unter [Testen Datenverkehr Manager-Einstellungen](traffic-manager-testing-settings.md) .
10. Nachdem Ihr Profil Datenverkehr Manager einrichten und zur Arbeit ist, bearbeiten Sie den DNS-Eintrag auf dem autorisierenden DNS-Server den Namen Ihres Unternehmens auf den Domänennamen Datenverkehr Manager verweisen. Weitere Informationen hierzu finden Sie unter [Punkt einer Unternehmen Internet-Domäne zu einer Domäne den Datenverkehr-Manager](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Nächste Schritte

[Zeigen Sie eine Firma Internet-Domäne auf eine Domäne Datenverkehr-Manager](traffic-manager-point-internet-domain.md)

[Weiterleitung Methoden Datenverkehr-Manager](traffic-manager-routing-methods.md)

[Konfigurieren der Funktion RUNDEN Robert routing Methode](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurieren der Leistung routing Methode](traffic-manager-configure-performance-routing-method.md)

[Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)

[Datenverkehr-Manager – deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)

[Datenverkehr Manager – deaktivieren oder Aktivieren von außen liegenden Tabellenblättern](disable-or-enable-an-endpoint.md)

