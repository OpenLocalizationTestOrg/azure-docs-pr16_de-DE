<properties
    pageTitle="Weiterleitung Methoden Datenverkehr Manager konfigurieren | Microsoft Azure"
    description="In diesem Artikel wird erläutert, wie konfiguriert unterschiedliche Weiterleitung Methoden in den Datenverkehr Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Konfigurieren der Weiterleitung Methoden Datenverkehr-Manager

Azure Datenverkehr Manager bietet drei routing Methoden, die steuern, wie Datenverkehr zum verfügbaren Service Endpunkten weitergeleitet wird. Die Datenverkehr-routing Methode wird angewendet, jede DNS-Abfrage empfangen, um zu bestimmen, welche Endpunkt in der DNS-Antwort zurückgegeben werden soll.

Es gibt drei routing Datenverkehr-Methoden in den Datenverkehr Manager verfügbar:

- **Priorität:** Wählen Sie 'Priorität', wenn Sie verwenden möchten, Verwenden einer primären Service-Endpunkts an, und geben Sicherungskopien aus, für den Fall, dass die primäre nicht verfügbar ist.
- **Gewichteten:** Option 'gewichteten' Wenn Sie den Datenverkehr auf eine Reihe von Endpunkten verteilen möchten, definieren entweder gleichmäßig oder entsprechend Breiten, die Sie.
- **Leistung:** Wählen Sie "Leistung", wenn Sie die Endpunkte an unterschiedlichen geografischen Standorten haben und Endbenutzer den "nächstliegende" Endpunkt im Hinblick auf den niedrigsten Netzwerkwartezeit verwendet werden soll.

## <a name="configure-priority-routing-method"></a>Konfigurieren von routing Methode Priorität

Unabhängig von den Website-Modus bereitstellen Azure Websites bereits Failoverfunktionalität für Websites innerhalb eines Datencenters (Bereich). Datenverkehr-Manager bietet Failover für Websites in verschiedenen Rechenzentren.

Ein gemeinsames Muster Failoververarbeitung Dienst ist Datenverkehr an einer primären Dienst senden und bieten eine Reihe von identische zusätzliche Dienste für Failover. Die folgenden Schritte erläutert, wie diese mit Prioritätsstufe Failover mit Azure-Cloud-Diensten und Websites zu konfigurieren:

1. Im klassischen Azure Portal im linken Bereich klicken Sie auf das Symbol **Datenverkehr Manager** zum Öffnen des Bereichs für den Datenverkehr-Manager.
2. Suchen Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal das Datenverkehr-Manager-Profil, das die Einstellungen enthält, die Sie ändern möchten. Um die Einstellungen Profilseite zu öffnen, klicken Sie auf den Pfeil rechts neben den Namen des Profils.
3. Klicken Sie auf Ihrer Profilseite **Endpunkte** am oberen Rand der Seite auf. Stellen Sie sicher, dass sowohl die Websites, die in der Konfiguration enthalten sein sollen die Cloud Services vorhanden sind.
4. Klicken Sie auf **Konfigurieren** , nach oben, um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie für **den Datenverkehr routing Methode Einstellungen**, die Datenverkehr routing Methode **Failover**aus. Wenn es nicht der Fall ist, klicken Sie auf **Failover** in der Dropdownliste aus.
6. **Priorität Failoverliste**passen Sie die Reihenfolge Ihrer Endpunkte Failover aus. Wenn Sie die **Failover** Datenverkehr routing Methode auswählen, ist die Reihenfolge der ausgewählten Endpunkte wichtig. Der primäre Endpunkt ist ganz oben. Verwenden Sie den Pfeil nach oben und unten weisenden Sie Pfeile, um die Reihenfolge zu ändern, je nach Bedarf. Informationen dazu, wie Sie die Failoverpriorität festzulegen, mithilfe von Windows PowerShell finden Sie unter [Festlegen-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Stellen Sie sicher, dass die **Einstellungen für die Überwachung** ordnungsgemäß konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind nicht den Datenverkehr gesendet werden. Zum Überwachen der Endpunkte, müssen Sie einen Pfad und Dateinamen angeben. A Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und setzt voraus, dass die Datei im Stammverzeichnis (Standard) ist.
8. Nachdem Sie die Konfiguration Änderungen abgeschlossen haben, klicken Sie am unteren Rand der Seite **zu speichern** .
9. Testen Sie die Änderungen in der Konfiguration aus.
10. Nachdem Sie Ihr Profil Datenverkehr Manager arbeitet, bearbeiten Sie den DNS-Eintrag auf dem autorisierenden DNS-Server den Namen Ihres Unternehmens auf den Domänennamen Datenverkehr Manager verweisen.

## <a name="configure-weighted-routing-method"></a>Konfigurieren der gewichteten routing Methode

Ein gemeinsames Datenverkehr routing Methode Muster besteht darin bieten eine Reihe von identische Endpunkte, die Cloud-Diensten und Websites einschließen möchten, und senden den Datenverkehr an jedes Roundrobin. Die folgenden Schritte beschreiben, wie Sie diese Art von Datenverkehr routing Methode konfigurieren.

>[AZURE.NOTE] Azure Websites Lastenausgleich bereits Round-Robert Funktionalität für Websites innerhalb eines Datencenters (Bereich). Datenverkehr-Manager können Sie Round-Robert Datenverkehr routing Methode für Websites in verschiedenen Rechenzentren angeben.

1. Im klassischen Azure Portal im linken Bereich klicken Sie auf das Symbol **Datenverkehr Manager** zum Öffnen des Bereichs für den Datenverkehr-Manager.
2. Suchen Sie im Bereich den Datenverkehr Manager das Datenverkehr-Manager-Profil, das die Einstellungen enthält, die Sie ändern möchten. Um die Einstellungen Profilseite zu öffnen, klicken Sie auf den Pfeil rechts neben den Namen des Profils.
3. Klicken Sie auf der Seite für Ihr Profil klicken Sie auf die **Endpunkte** am oberen Rand der Seite, und stellen Sie sicher, dass die Endpunkte, die in der Konfiguration enthalten sein sollen vorhanden sind.
4. Klicken Sie auf Ihrer Profilseite klicken Sie auf " **Konfigurieren** " nach oben, um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie für **den Datenverkehr routing Methode Einstellungen**, die Datenverkehr routing Methode **Round Robert**aus. Wenn es nicht der Fall ist, klicken Sie in der Dropdownliste auf **Round Robert** .
6. Stellen Sie sicher, dass die **Einstellungen für die Überwachung** ordnungsgemäß konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind nicht den Datenverkehr gesendet werden. Zum Überwachen der Endpunkte, müssen Sie einen Pfad und Dateinamen angeben. A Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und setzt voraus, dass die Datei im Stammverzeichnis (Standard) ist.
7. Nachdem Sie die Konfiguration Änderungen abgeschlossen haben, klicken Sie am unteren Rand der Seite **zu speichern** .
8. Testen Sie die Änderungen in der Konfiguration aus.
9. Nachdem Ihr Profil Datenverkehr Manager arbeitet, bearbeiten Sie den DNS-Eintrag auf dem autorisierenden DNS-Server den Namen Ihres Unternehmens auf den Domänennamen Datenverkehr Manager verweisen.

## <a name="configure-performance-traffic-routing-method"></a>Konfigurieren der Leistung Datenverkehr routing Methode

Die Leistung Datenverkehr routing Methode können Sie den Datenverkehr an den Endpunkt mit den niedrigsten Wartezeit aus das Client Netzwerk direkte. Normalerweise ist das Rechenzentrum mit den niedrigsten Wartezeit in geografische Entfernung der nächsten. Diese Datenverkehr routing Methode kann nicht in Echtzeit Änderungen in Netzwerkkonfiguration Konto oder geladen.

1. Im klassischen Azure Portal im linken Bereich klicken Sie auf das Symbol **Datenverkehr Manager** zum Öffnen des Bereichs für den Datenverkehr-Manager.
2. Suchen Sie im Bereich den Datenverkehr Manager das Datenverkehr-Manager-Profil, das die Einstellungen enthält, die Sie ändern möchten. Um die Einstellungen Profilseite zu öffnen, klicken Sie auf den Pfeil rechts neben den Namen des Profils.
3. Klicken Sie auf der Seite für Ihr Profil klicken Sie auf die **Endpunkte** am oberen Rand der Seite, und stellen Sie sicher, dass die Endpunkte, die in der Konfiguration enthalten sein sollen vorhanden sind.
4. Klicken Sie auf der Seite für Ihr Profil klicken Sie auf " **Konfigurieren** " nach oben, um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie für **den Datenverkehr routing Methode Einstellungen**, die Datenverkehr routing Methode * *Leistung*aus. Wenn sie nicht der Fall ist, klicken Sie auf * *Leistung** in der Dropdownliste aus.
6. Stellen Sie sicher, dass die **Einstellungen für die Überwachung** ordnungsgemäß konfiguriert sind. Überwachung wird sichergestellt, dass die Endpunkte, die offline sind nicht den Datenverkehr gesendet werden. Zum Überwachen der Endpunkte, müssen Sie einen Pfad und Dateinamen angeben. A Schrägstrich "/" ist ein gültiger Eintrag für den relativen Pfad und setzt voraus, dass die Datei im Stammverzeichnis (Standard) ist.
7. Nachdem Sie die Konfiguration Änderungen abgeschlossen haben, klicken Sie am unteren Rand der Seite **zu speichern** .
8. Testen Sie die Änderungen in der Konfiguration aus.
9. Nachdem Ihr Profil Datenverkehr Manager arbeitet, bearbeiten Sie den DNS-Eintrag auf dem autorisierenden DNS-Server den Namen Ihres Unternehmens auf den Domänennamen Datenverkehr Manager verweisen.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Profilen Datenverkehr-Managers](traffic-manager-manage-profiles.md)
* [Weiterleitung Methoden Datenverkehr-Manager](traffic-manager-routing-methods.md)
* [Testen den Datenverkehr Manager-Einstellungen](traffic-manager-testing-settings.md)
* [Zeigen Sie eine Firma Internet-Domäne auf eine Domäne Datenverkehr-Manager](traffic-manager-point-internet-domain.md)
* [Verwalten von Datenverkehr Manager Endpunkte](traffic-manager-manage-endpoints.md)
* [Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)
