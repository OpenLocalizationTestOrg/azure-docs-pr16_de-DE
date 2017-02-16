<properties
   pageTitle="Verwalten von Endpunkten in Azure Datenverkehr Manager | Microsoft Azure"
   description="In diesem Artikel helfen Ihnen, hinzufügen, entfernen, aktivieren und deaktivieren die Endpunkte aus Azure Datenverkehr-Manager."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Hinzufügen, deaktivieren, aktivieren oder Löschen von Endpunkten

Das Feature Web Apps in Azure-App-Dienst bietet bereits Failover und Round-Robert Datenverkehr Weiterleitung Funktionalität für Websites innerhalb eines Datencenters, unabhängig von den Website-Modus. Azure Datenverkehr-Manager können Sie Failover- und Round-Robert Datenverkehr für Websites und Cloud-Dienste in verschiedenen Rechenzentren routing angeben. Im ersten Schritt zu sorgen, dass die Funktionalität ist den Cloud-Dienst oder Website-Endpunkt nach den Datenverkehr Manager hinzufügen.

>[AZURE.NOTE] Sie können keine externe Speicherorte oder Datenverkehr-Manager-Profilen als Endpunkte über das klassische Azure-Portal hinzufügen. Sie müssen die REST-API [Definition erstellen](http://go.microsoft.com/fwlink/p/?LinkId=400772) oder Windows PowerShell- [Add-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774)verwenden.

Sie können auch einzelne Endpunkte deaktivieren, die Teil eines Profils Datenverkehr Manager sind. Endpunkte umfassen sowohl Cloud Services-Websites. Deaktivieren von außen liegenden Tabellenblättern belässt es als Teil des Profils, aber das Profil fungiert, als wäre der Endpunkt nicht darin enthalten ist. Diese Aktion ist dann sehr hilfreich, einen Endpunkt, der Wartungsmodus oder, die erneut bereitgestellt ist vorübergehend entfernt. Nachdem Sie der Endpunkt wieder einsatzbereit ist, können sie aktiviert werden.

>[AZURE.NOTE] Deaktivieren von außen liegenden Tabellenblättern hat nichts mit Bereitstellungszustand in Azure. Ein Endpunkt fehlerfreier bleibt nach oben und zum Empfang von Datenverkehr, auch wenn in den Datenverkehr Manager deaktiviert. Deaktivieren von außen liegenden Tabellenblättern in einem Profil wirkt sich darüber hinaus nicht auf deren Status in einem anderen Profil aus.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Um einen Cloud-Dienst oder Website-Endpunkt hinzuzufügen.


1. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach den Datenverkehr-Manager-Profil, das die Endpunkt Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
2. Klicken Sie am oberen Rand der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die bereits Bestandteil der Konfiguration sind.
3. Klicken Sie am unteren Rand der Seite auf **Hinzufügen** , um die **Endpunkte hinzufügen** Seite zu öffnen. Standardmäßig werden die Seite der Cloud Services unter **Endpunkte**aufgeführt.
4. Wählen Sie für Clouddienste die Clouddienste in der Liste, um diese als Endpunkte für dieses Profil zu aktivieren. Das Deaktivieren des Namens der Cloud-Dienst entfernt in der Liste der Endpunkte.
5. Klicken Sie auf die Dropdown-Liste **Diensttyp** für Websites und wählen Sie dann auf **Web app**.
6. Wählen Sie die Websites in der Liste, um sie als Endpunkte für dieses Profil hinzuzufügen. Das Deaktivieren des Namens der Website entfernt in der Liste der Endpunkte. Beachten Sie, dass Sie nur auf eine einzelne Website pro Azure Datacenter (Bereich) auswählen können. Wenn Sie eine Website in einem Datencenter, die mehrere Websites hostet auswählen, wenn Sie die erste Website auswählen, können die anderen im gleichen Datencenter nicht mehr ausgewählt. Beachten Sie auch, dass nur Standard Websites aufgeführt sind.
7. Nachdem Sie die Endpunkte für dieses Profil ausgewählt haben, klicken Sie auf das Auswahlfeld unten rechts, um die Änderungen zu speichern.

>[AZURE.NOTE] Wenn Sie nach dem Hinzufügen oder Entfernen von außen liegenden Tabellenblättern die *Failover* Datenverkehr routing Methode verwenden, achten Sie darauf, dass Sie in der Liste Priorität Failover auf der Seite Konfiguration entsprechend die Failover Reihenfolge für Ihre Konfiguration gewünschte anpassen. Weitere Informationen finden Sie unter [Konfigurieren von Failover Datenverkehr routing](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Deaktivieren von außen liegenden Tabellenblättern

1. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach den Datenverkehr-Manager-Profil, das die Endpunkt Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
2. Klicken Sie am oberen Rand der Seite auf die **Endpunkte** um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und klicken Sie dann auf am unteren Rand der Seite **Deaktivieren** .
4. Datenverkehr wird gestoppt parallelen an den Endpunkt basierend auf der DNS-Time-to-Live (TTL) für den Datenverkehr Manager Domänennamen konfiguriert. Sie können die Gültigkeitsdauer auf der Seite Konfiguration des Profils Datenverkehr Manager ändern.

## <a name="to-enable-an-endpoint"></a>Aktivieren von außen liegenden Tabellenblättern

1. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach den Datenverkehr-Manager-Profil, das die Endpunkt Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
2. Klicken Sie am oberen Rand der Seite auf die **Endpunkte** um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und klicken Sie dann auf am unteren Rand der Seite **Aktivieren** .
4. Datenverkehr wird zum Dienst erneut als vom Profil diktierten parallelen gestartet.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>So löschen Sie einen Cloud-Dienst oder Website-Endpunkt


1. Klicken Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal suchen Sie nach den Datenverkehr-Manager-Profil, das die Endpunkt Einstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben den Namen des Profils. Dadurch wird die Einstellungsseite für das Profil geöffnet.
2. Klicken Sie am oberen Rand der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die bereits Bestandteil der Konfiguration sind.
3. Klicken Sie auf den Namen des Endpunkts an, die Sie aus dem Profil löschen möchten, klicken Sie auf der Seite Endpunkte.
4. Klicken Sie am unteren Rand der Seite auf **Löschen**.

>[AZURE.NOTE] Sie löschen nicht als Endpunkte mithilfe des Azure klassischen Portals externe Speicherorte oder Datenverkehr Manager Profile. Sie müssen Windows PowerShell verwenden. Weitere Informationen finden Sie unter [Entfernen-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Failover routing Methode](traffic-manager-configure-failover-routing-method.md)
- [Konfigurieren der Funktion RUNDEN Robert routing Methode](traffic-manager-configure-round-robin-routing-method.md)
- [Konfigurieren der Leistung routing Methode](traffic-manager-configure-performance-routing-method.md)
- [Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)
- [Vorgänge auf den Datenverkehr Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
