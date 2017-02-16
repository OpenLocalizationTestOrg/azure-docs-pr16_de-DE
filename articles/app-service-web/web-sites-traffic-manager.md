<properties
    pageTitle="Steuern der Azure Web app-Verkehr mit Azure Datenverkehr Manager"
    description="Dieser Artikel enthält zusammenfassende Informationen für Azure Datenverkehr Manager im Zusammenhang mit Azure Web apps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Steuern der Azure Web app-Verkehr mit Azure Datenverkehr Manager

> [AZURE.NOTE] Dieser Artikel enthält zusammenfassende Informationen für Microsoft Azure Datenverkehr Manager im Zusammenhang mit Azure App Dienst Web Apps. Weitere Informationen zu Azure Datenverkehr Manager selbst kann gefunden werden, indem Sie die Links am Ende dieses Artikels.

## <a name="introduction"></a>Einführung
Azure Datenverkehr Manager können Sie steuern, wie im Webclient-Anfragen bei Web apps in Azure-App-Verwaltungsdienst verteilt werden. Wenn ein Profil Azure Datenverkehr Manager Web app Endpunkte hinzugefügt werden, werden von nachverfolgt Azure Datenverkehr Manager den Status Ihrer Web Apps (ausgeführt, beendet oder gelöschte), damit sie entscheiden kann, welche diese Endpunkte Datenverkehr erhalten soll.

## <a name="load-balancing-methods"></a>Laden Sie Lastenausgleich Methoden
Azure Datenverkehr Manager mithilfe von drei verschiedenen laden Lastenausgleich Methoden. Diese werden in der folgenden Liste beschrieben, wie sie bei Azure Web apps gehören.

* **Failover**: Wenn Sie Web app Klonen verschiedener Regionen haben, können Sie mit dieser Methode können Sie eine Web-app, um den gesamten Verkehr der Web-Client-service konfigurieren und Konfigurieren einer anderen Web-app in einem anderen Bereich zu dieser Datenverkehr service, für den Fall, dass das erste Web app nicht mehr verfügbar ist.

* **Round Robert**: Wenn Sie Web app Klonen verschiedener Regionen haben, können diese Methode zum Verteilen von Verkehr gleichmäßig auf der Web apps in unterschiedlichen Regionen.

* **Leistung**: die Leistung Methode verteilt Datenverkehr auf Grundlage kürzester Zeit auf Clients Schleife. Die Leistung Methode kann von Web apps innerhalb derselben Region oder in anderen Bereichen verwendet werden.

##<a name="web-apps-and-traffic-manager-profiles"></a>Web Apps und den Datenverkehr-Manager-Profilen
Um die Steuerung des Web app-Verkehr zu konfigurieren, erstellen Sie ein Profil in Azure Datenverkehr Manager, verwendet eine der drei Lastenausgleich zuvor beschriebene Methoden laden, und fügen Sie die Endpunkte (in diesem Fall Web apps) für die Sie den Datenverkehr in das Profil steuern möchten. Ihre Web app-Status (läuft, angehalten oder gelöscht) wird regelmäßig, damit Azure Datenverkehr Manager entsprechend den Datenverkehr steuern können das Profil übermittelt.

Wenn Azure Datenverkehr Manager mit Azure verwenden zu können, sollten beachten Sie die folgenden Punkte:

* Für Web app nur Bereitstellungen innerhalb derselben Region bietet Web Apps bereits Failover und Round-Robert Funktionalität Berücksichtigung Web app-Modus.

* Für die Bereitstellung in derselben Region, die Web Apps in Verbindung mit einem anderen Azure-Cloud-Dienst verwenden, können Sie die beiden Typen von Endpunkten in Hybrid Szenarien aktivieren kombinieren.

* Sie können nur einen Web app-Endpunkt pro Region in einem Profil angeben. Wenn Sie eine Web app als Endpunkt für eine Region auswählen, sind die verbleibende Web apps in diesem Bereich für die Auswahl für das Profil mit nicht verfügbar.

* Die Endpunkte des Web app, die Sie in einem Profil Azure Datenverkehr Manager angeben unter dem Abschnitt **Domain Names** auf der Seite "konfigurieren" für das Web app im Profil angezeigt werden, aber nicht konfiguriert werden vorhanden.

* Nachdem Sie ein Profil eine Web app hinzugefügt haben, wird die **URL der Website** auf dem Dashboard von der Web-app-Portalseite benutzerdefinierte Domänen-URL des Web app angezeigt, wenn Sie eine eingerichtet haben. Andernfalls wird es angezeigt, die Datenverkehr-Manager-Profil-URL (z. B. `contoso.trafficmgr.com`). Sowohl der direkte Domänennamen von der Web app und die URL für den Datenverkehr Manager werden angezeigt, klicken Sie auf der Web-app konfigurieren Seite unter dem Abschnitt **Domain Names** .

* Den Namen Ihrer benutzerdefinierten Domäne wie erwartet funktionieren, aber zusätzlich zum Hinzufügen von Web apps, müssen Sie auch Konfigurieren der DNS-Karte auf die URL für den Datenverkehr Manager verweisen. Informationen zum Einrichten eines benutzerdefinierten Domänennamens für eine Azure Web app finden Sie unter [Konfigurieren Sie einen benutzerdefinierten Domänennamen für eine Azure-Website](web-sites-custom-domain-name.md).

* Sie können nur Web apps hinzufügen, die im Standardmodus zu einem Profil Azure Datenverkehr Manager sind.

## <a name="next-steps"></a>Nächste Schritte

Konzeptionelle sowie technische Übersicht über Azure Datenverkehr-Manager finden Sie unter [Übersicht über den Datenverkehr-Manager](../traffic-manager/traffic-manager-overview.md).

Weitere Informationen zur Verwendung von Datenverkehr Manager mit Web Apps finden Sie unter der Blogbeiträge [Mit Azure Datenverkehr Manager Azure-Websites](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) und [Azure Datenverkehr Manager können jetzt mit Azure-Websites integrieren](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
