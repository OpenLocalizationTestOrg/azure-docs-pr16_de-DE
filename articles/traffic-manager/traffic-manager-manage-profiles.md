<properties
    pageTitle="Azure Datenverkehr Manager Profile verwalten | Microsoft Azure"
    description="In diesem Artikel können Sie die erstellen, deaktivieren, zu aktivieren, löschen und Anzeigen des Verlaufs eines Profils Azure Datenverkehr-Manager."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Verwalten eines Profils Azure Datenverkehr Manager

Datenverkehr-Manager-Profilen verwenden den Datenverkehr-routing Methoden, um die Verteilung der Datenverkehr in der Cloud Services oder Website-Endpunkte steuern. In diesem Artikel wird erläutert, wie erstellen und diese Profile verwalten.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Erstellen eines Profils Datenverkehr-Managers, die mit schnellen Erstellen

Sie können schnell ein Profil Datenverkehr Manager mithilfe von schnellen Erstellen in der klassischen Azure-Portal erstellen. Schnelle erstellen können Sie zum Erstellen von Profilen mit grundlegenden Konfiguration Einstellungen. Allerdings können keine Sie schnellen Erstellen für Einstellungen wie den Satz von Endpunkten (Cloud-Diensten und Websites), die Reihenfolge der Failover für die Weiterleitung Failover Datenverkehr-Methode oder Einstellungen für die Überwachung verwenden. Nach dem Erstellen Ihres Profils an, können Sie diese Einstellungen in der klassischen Azure-Portal konfigurieren. Datenverkehr Manager unterstützt bis zu 200 Endpunkte pro Profil. Die meisten Verwendungsszenarien erfordern jedoch nur wenige Endpunkte.

### <a name="to-create-a-traffic-manager-profile"></a>Zum Erstellen eines Profils Datenverkehr-Manager

1. **Bringen Sie Ihre Clouddienste und Websites auf Ihrer Umgebung Herstellung ein.** Weitere Informationen zu Cloud Services finden Sie unter [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Weitere Informationen zu Websites finden Sie unter [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Melden Sie sich zum klassischen Azure-Portal an.** Klicken Sie auf **neu** , klicken Sie auf der unteren linken Ecke des Portals, klicken Sie auf **Network Services > Datenverkehr Manager**, und klicken Sie dann auf **Schnellen Erstellen** , um Ihr Profil konfigurieren zu beginnen.
3. **Konfigurieren des DNS-Präfixes an.** Geben Sie einen eindeutigen DNS-Einträge Ihrer Datenverkehr-Manager-Profil Präfixname. Sie können nur des Präfixes für den Datenverkehr Manager Domänennamen angeben.
4. **Wählen Sie das Abonnement aus.** Wählen Sie das entsprechende Azure-Abonnement aus. Jedes Profil ist ein einzelnes Abonnement zugeordnet. Wenn Sie nur ein Abonnement besitzen, wird diese Option nicht angezeigt.
5. **Wählen Sie den Datenverkehr routing Methode.** Wählen Sie den Datenverkehr routing Methode in **den Datenverkehr routing Richtlinie**ein. Weitere Informationen zu den Datenverkehr Weiterleitung Methoden finden Sie unter [Informationen zu den Datenverkehr Manager Datenverkehr Weiterleitung Methoden](traffic-manager-routing-methods.md).
6. **Klicken Sie auf "Erstellen", um das Profil erstellen**. Wenn die Konfiguration des Profils abgeschlossen ist, können Sie Ihr Profil im Datenverkehr-Manager im klassischen Azure-Portal suchen.
7. **Konfigurieren Sie Endpunkte, für die Überwachung und zusätzliche Einstellungen im klassischen Azure-Portal an.** Verwenden die Symbolleiste erstellen konfiguriert nur grundlegende Einstellungen aus. Es ist so konfigurieren Sie zusätzliche Einstellungen wie die Liste der Endpunkte und die Reihenfolge der Endpunkt Failover erforderlich.


## <a name="disable-enable-or-delete-a-profile"></a>Deaktivieren, aktivieren oder Löschen eines Profils

Sie können ein vorhandenes Profil, damit diese Datenverkehr Manager benutzeranforderungen an die Endpunkte konfiguriert bezieht sich nicht deaktivieren. Wenn Sie ein Profil Datenverkehr Manager deaktivieren, das Profil und die Informationen in das Profil bleiben erhalten und die Benutzeroberfläche Datenverkehr Manager bearbeitet werden können.  Bezüge auf fortsetzen, wenn Sie das Profil erneut aktivieren. Wenn Sie ein Profil Datenverkehr Manager im klassischen Azure-Portal erstellen, wird sie automatisch aktiviert. Wenn Sie sich, dass ein Profil nicht mehr benötigt wird entscheiden, können Sie es löschen.

### <a name="to-disable-a-profile"></a>So deaktivieren Sie ein Profil

1. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, ändern Sie den CNAME-Eintrag auf dem Internet-DNS-Server so, dass es nicht mehr zu Ihrem Profil Datenverkehr Manager zeigt.
2. Datenverkehr stoppt weitergeleitet werden die Endpunkte durch den Datenverkehr Manager einräumen.
3. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie auf der Seite Datenverkehr Manager das Profil durch Klicken auf die Spalte neben dem Profilnamen ein. Notiz auf den Namen des Profils oder den Pfeil neben dem Namen öffnet die Einstellungsseite für das Profil.
4. Klicken Sie nachdem Sie das Profil ausgewählt haben auf am unteren Rand der Seite **Deaktivieren** .

### <a name="to-enable-a-profile"></a>Aktivieren ein Profils

1. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie auf der Seite Datenverkehr Manager das Profil durch Klicken auf die Spalte neben dem Profilnamen ein. Notiz auf den Namen des Profils oder den Pfeil neben dem Namen öffnet die Einstellungsseite für das Profil.
2. Klicken Sie nachdem Sie das Profil ausgewählt haben auf am unteren Rand der Seite **Aktivieren** .
3. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, erstellen Sie einen CNAME-Eintrag auf dem Internet-DNS-Server auf den Domänennamen Ihres Profils Datenverkehr Manager verweisen.
4. Datenverkehr wird erneut auf die Endpunkte weitergeleitet.

### <a name="to-delete-a-profile"></a>Zum Löschen eines Profils

1. Stellen Sie sicher, dass der DNS-Eintrag auf dem Internet-DNS-Server nicht mehr einen CNAME-Eintrag verwendet, der auf den Domänennamen Ihres Profils Datenverkehr Manager verweist.
2. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie auf der Seite Datenverkehr Manager das Profil durch Klicken auf die Spalte neben dem Profilnamen ein. Notiz auf den Namen des Profils oder den Pfeil neben dem Namen öffnet die Einstellungsseite für das Profil.
3. Klicken Sie nachdem Sie das Profil ausgewählt haben auf am unteren Rand der Seite **Löschen** .

## <a name="view-traffic-manager-profile-change-history"></a>Ansicht Datenverkehr Manager Profil Änderungsverlauf

Sie können das Änderungsprotokoll für Ihr Profil Datenverkehr Manager im klassischen Azure-Portal in Management Services anzeigen.

### <a name="to-view-your-traffic-manager-change-history"></a>Der Änderungsverlauf Datenverkehr Manager anzeigen

1. Klicken Sie im linken Bereich des klassischen Azure Portals auf **Management Services**.
2. Klicken Sie auf der Seite Management Services auf **Vorgang Protokolle**.
3. Klicken Sie auf der Seite Vorgang Protokolle können Sie Filtern zum Anzeigen der Änderungsverlauf für Ihr Profil Datenverkehr-Manager. Klicken Sie nachdem Sie Ihre Filteroptionen ausgewählt haben auf das Häkchen, um die Ergebnisse anzuzeigen.

   - Um die Änderungen für alle Ihre Profile anzeigen möchten, wählen Sie Ihr Abonnement und Zeitraums, und klicken Sie dann im Kontextmenü **Typ** **Datenverkehr-Manager** .
   - Zum nach Profilname gefiltert werden, geben Sie den Namen des Profils in das Feld **Name der Dienst** , oder wählen Sie ihn aus dem Kontextmenü aus.
   - Um die Details für jede einzelne Änderung anzeigen möchten, markieren Sie die Zeile mit der Änderung, die Sie anzeigen möchten, und klicken Sie dann auf **Details** am unteren Rand der Seite. Im Fenster **Details Vorgangs** können Sie die XML-Darstellung des Objekts API anzeigen, die erstellt oder als Teil des Vorgangs aktualisiert wurde.

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von außen liegenden Tabellenblättern](traffic-manager-endpoints.md)
- [Konfigurieren von Failover routing Methode](traffic-manager-configure-failover-routing-method.md)
- [Konfigurieren der Funktion RUNDEN Robert routing Methode](traffic-manager-configure-round-robin-routing-method.md)
- [Konfigurieren der Leistung routing Methode](traffic-manager-configure-performance-routing-method.md)
- [Zeigen Sie eine Firma Internet-Domäne auf einen Domänennamen Datenverkehr-Manager](traffic-manager-point-internet-domain.md)
- [Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)