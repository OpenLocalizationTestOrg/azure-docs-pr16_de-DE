<properties
    pageTitle="Verwalten von Endpunkten in Azure Datenverkehr Manager | Microsoft Azure"
    description="In diesem Artikel helfen Ihnen, hinzufügen, entfernen, aktivieren und deaktivieren die Endpunkte aus Azure Datenverkehr-Manager."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Hinzufügen, deaktivieren, aktivieren oder Löschen von Endpunkten

Das Feature Web Apps in Azure-App-Dienst bietet bereits Failover und Round-Robert Datenverkehr Weiterleitung Funktionalität für Websites innerhalb eines Datencenters, unabhängig von den Website-Modus. Azure Datenverkehr-Manager können Sie Failover- und Round-Robert Datenverkehr für Websites und Cloud-Dienste in verschiedenen Rechenzentren routing angeben. Im ersten Schritt zu sorgen, dass die Funktionalität ist den Cloud-Dienst oder Website-Endpunkt nach den Datenverkehr Manager hinzufügen.

>[AZURE.NOTE]  In diesem Artikel wird erläutert, wie das klassische Portal verwenden. Das Azure klassische Portal unterstützt nur das Erstellen und die Zuordnung Clouddienste und der Web apps als Endpunkte. Das neue [Azure-Portal](https://portal.azure.com) ist die bevorzugte Schnittstelle an.

Sie können auch einzelne Endpunkte deaktivieren, die Teil eines Profils Datenverkehr Manager sind. Deaktivieren von außen liegenden Tabellenblättern belässt es als Teil des Profils, aber das Profil fungiert, als wäre der Endpunkt nicht darin enthalten ist. Diese Aktion eignet sich für einen Endpunkt, der Wartungsmodus oder, die erneut bereitgestellt ist vorübergehend zu entfernen. Nachdem Sie der Endpunkt wieder einsatzbereit ist, können sie aktiviert werden.

>[AZURE.NOTE] Deaktivieren von außen liegenden Tabellenblättern hat nichts mit Bereitstellungszustand in Azure. Ein Endpunkt fehlerfreier bleibt nach oben und zum Empfang von Datenverkehr, auch wenn in den Datenverkehr Manager deaktiviert. Deaktivieren von außen liegenden Tabellenblättern in einem Profil wirkt sich darüber hinaus nicht auf deren Status in einem anderen Profil aus.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Um einen Cloud-Dienst oder Website-Endpunkt hinzuzufügen.

1. Suchen Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal das Datenverkehr Manager-Profil, das die Einstellungen Endpunkt enthält, die Sie ändern möchten. Klicken Sie zum Öffnen der Einstellungsseite auf den Pfeil rechts neben den Namen des Profils.
2. Klicken Sie am oberen Rand der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die bereits Bestandteil der Konfiguration sind.
3. Klicken Sie am unteren Rand der Seite auf **Hinzufügen** , um die **Endpunkte hinzufügen** Seite zu öffnen. Standardmäßig werden die Seite der Cloud Services unter **Endpunkte**aufgeführt.
4. Wählen Sie für Clouddienste die Clouddienste in der Liste, um sie als Endpunkte für dieses Profil hinzuzufügen. Das Deaktivieren des Namens der Cloud-Dienst entfernt in der Liste der Endpunkte.
5. Klicken Sie auf die Dropdown-Liste **Diensttyp** für Websites und wählen Sie dann auf **Web app**.
6. Wählen Sie die Websites in der Liste, um sie als Endpunkte für dieses Profil hinzuzufügen. Das Deaktivieren des Namens der Website entfernt in der Liste der Endpunkte. Sie können nur eine Website pro Azure Datacenter (Bereich) auswählen. Wenn Sie die erste Website auswählen, kann die anderen Websites im gleichen Datencenter nicht mehr ausgewählt. Beachten Sie auch, dass nur Standard Websites aufgeführt sind.
7. Nachdem Sie die Endpunkte für dieses Profil ausgewählt haben, klicken Sie auf das Auswahlfeld unten rechts, um die Änderungen zu speichern.

>[AZURE.NOTE] Nach dem Hinzufügen oder Entfernen von außen liegenden Tabellenblättern aus einem Profil mithilfe der *Failover* Datenverkehr routing Methode, die Priorität Failoverliste möglicherweise nicht sortiert werden diese wie gewünscht. Sie können die Reihenfolge der Liste Priorität Failover auf der Seite Konfiguration anpassen. Weitere Informationen finden Sie unter [Konfigurieren von Failover Datenverkehr routing](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Deaktivieren von außen liegenden Tabellenblättern

1. Suchen Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal das Datenverkehr Manager-Profil, das die Einstellungen Endpunkt enthält, die Sie ändern möchten. Klicken Sie zum Öffnen der Einstellungsseite auf den Pfeil rechts neben den Namen des Profils.
2. Klicken Sie am oberen Rand der Seite auf die **Endpunkte** um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und klicken Sie dann auf am unteren Rand der Seite **Deaktivieren** .
4. Clients weiterhin den Datenverkehr an den Endpunkt für die Dauer der Time-to-Live (TTL) zu senden. Sie können die Gültigkeitsdauer auf der Seite Konfiguration des Profils Datenverkehr Manager ändern.

## <a name="to-enable-an-endpoint"></a>Aktivieren von außen liegenden Tabellenblättern

1. Suchen Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal das Datenverkehr Manager-Profil, das die Einstellungen Endpunkt enthält, die Sie ändern möchten. Klicken Sie zum Öffnen der Einstellungsseite auf den Pfeil rechts neben den Namen des Profils.
2. Klicken Sie am oberen Rand der Seite auf die **Endpunkte** um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und klicken Sie dann auf am unteren Rand der Seite **Aktivieren** .
4. Clients werden an den Endpunkt aktiviert geleitet, gemäß dem Profil.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>So löschen Sie einen Cloud-Dienst oder Website-Endpunkt

1. Suchen Sie im Bereich den Datenverkehr Manager im klassischen Azure-Portal das Datenverkehr Manager-Profil, das die Einstellungen Endpunkt enthält, die Sie ändern möchten. Klicken Sie zum Öffnen der Einstellungsseite auf den Pfeil rechts neben den Namen des Profils.
2. Klicken Sie am oberen Rand der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die bereits Bestandteil der Konfiguration sind.
3. Klicken Sie auf den Namen des Endpunkts an, die Sie aus dem Profil löschen möchten, klicken Sie auf der Seite Endpunkte.
4. Klicken Sie am unteren Rand der Seite auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Profilen Datenverkehr-Managers](traffic-manager-manage-profiles.md)
* [Konfigurieren der Weiterleitung Methoden](traffic-manager-configure-routing-method.md)
* [Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)
* [Datenverkehr Manager Leistungsaspekte](traffic-manager-performance-considerations.md)
* [Vorgänge auf den Datenverkehr Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
