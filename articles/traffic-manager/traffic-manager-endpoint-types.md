<properties
    pageTitle="Typen von Datenverkehr Manager Endpunkt | Microsoft Azure"
    description="Dieser Artikel erläutert die verschiedene Arten von Endpunkten, die mit Azure Datenverkehr Manager verwendet werden können"
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
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Datenverkehr Manager Endpunkte

Microsoft Azure Datenverkehr-Manager können Sie steuern, wie Netzwerkdatenverkehr an Anwendung Bereitstellungen in verschiedenen Rechenzentren ausgeführt verteilt ist. Sie konfigurieren jede Anwendung Bereitstellung als 'Endpunkt' in den Datenverkehr Manager. Wenn Datenverkehr Manager eine DNS-eingeht, wählt es einen Endpunkt in der DNS-Antwort zurückgegeben. Datenverkehr Manager basiert die Auswahl auf den aktuellen Status der Endpunkt und den Datenverkehr-routing Methode. Weitere Informationen finden Sie unter [Funktionsweise von Datenverkehr-Manager](traffic-manager-how-traffic-manager-works.md).

Es gibt drei Arten von Endpunkt von Datenverkehr Manager unterstützt:

- **Azure Endpunkte** werden für in Azure gehostete Dienste verwendet.
- **Externe Endpunkte** werden für außerhalb Azure, entweder lokal oder mit einem anderen Hostinganbieter gehostet Dienste verwendet.
- **Endpunkte geschachtelt** werden verwendet, um die Datenverkehr Manager Profile zum Erstellen von flexibler Datenverkehr-routing Phishingversuchen zur Unterstützung von der Anforderungen größere und komplexere Bereitstellungen kombinieren.

Es ist keine Einschränkung wie die Endpunkte des verschiedene Typen in einem einzigen Datenverkehr Manager Profil kombiniert werden. Jedes Profil kann beliebige Kombination von Endpunkttypen enthalten.

Den folgenden Abschnitten wird jeder Endpunkttyp ausführlicher beschrieben.

## <a name="azure-endpoints"></a>Azure Endpunkte

Azure Endpunkte werden zur Azure-basierte Dienste in den Datenverkehr Manager verwendet. Die folgenden Azure Ressourcentypen werden unterstützt:

- 'Klassisch' IaaS virtuellen Computern und PaaS cloud Services.
- Web Apps
- Öffentl.IP Ressourcen (die mit virtuellen Computern verbunden werden direkt oder über ein Lastenausgleich Azure). Die Öffentl.IP müssen einen DNS-Namen zugewiesen, in einem Manager Datenverkehr Profil verwendet werden soll.

Öffentl.IP Ressourcen sind Azure Ressourcenmanager Ressourcen. Sie sind nicht in der klassischen Bereitstellungsmodell vorhanden. Daher sind nur in unterstützten Datenverkehr Vorgesetzten Azure Ressourcenmanager Erfahrung. Andere Endpunkttypen werden per Ressourcenmanager, und das Bereitstellungsmodell klassischen unterstützt.

Bei der Verwendung von Azure Endpunkte erkennt Datenverkehr Manager, wenn eine 'Klassisch' IaaS VM, Cloud-Dienst oder eine Web App angehalten und gestartet wird. Dieser Status wird in den Endpunkt Status angezeigt. Details finden Sie unter [den Datenverkehr Manager Endpunkt überwachen](traffic-manager-monitoring.md#endpoint-and-profile-status) . Der zugrunde liegenden angehalten ist, führt den Datenverkehr Manager keine integritätsprüfungen Endpunkt oder direkte Datenverkehr an den Endpunkt aus. Kein Datenverkehr-Manager der angehaltenen Instanz Abrechnung Ereignisse auftreten. Wenn der Dienst neu gestartet wird, ist Abrechnung Lebensläufe und den Endpunkt zum Empfangen von Datenverkehr berechtigt. Diese Erkennung gilt nicht für Öffentl.IP Endpunkte.

## <a name="external-endpoints"></a>Externe Endpunkte

Externe Endpunkte sind für Dienste außerhalb Azure verwendet. Beispielsweise ein Dienst lokal gehostet oder mit einem anderen Anbieter. Externe Endpunkte können einzeln verwendet oder mit Azure Endpunkten in demselben Datenverkehr Manager Profil kombiniert werden. Kombinieren von Azure Endpunkte mit externen Endpunkten ermöglicht verschiedenen Szenarios:

- Verwenden Sie entweder eine aktive oder Aktiv / Passiv Failover-Modell Azure höhere Redundanz für eine vorhandene Anwendung lokal bereitgestellt.
- Erweitern Sie zum Verringern der Anwendung Wartezeit für Benutzer auf der ganzen Welt eine vorhandene lokale Anwendung zu zusätzlichen geografischen Standorten in Azure ein. Weitere Informationen finden Sie unter [Datenverkehr Manager "Leistung" Daten-routing](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Verwenden von Azure Bereitstellung von zusätzlicher Kapazität für eine vorhandene lokale Anwendung, fortlaufend oder als 'Burst Cloud' Lösung für eine Sammlung Nachfrage entsprechen.

In bestimmten Fällen, es ist sinnvoll, externe Endpunkte verwenden, um Azure Dienste verweisen (Beispiele finden Sie in den [häufig gestellte Fragen](#faq)). In diesem Fall sind integritätsprüfungen bei der Azure Endpunkte Tarifs nicht externe Endpunkte in Rechnung gestellt. Jedoch im Gegensatz zu Azure Endpunkte, wenn Sie beenden oder löschen den zugrunde liegenden Dienst Gesundheit überprüfen Abrechnung verwendet, bis Sie deaktivieren oder löschen Sie den Endpunkt im Datenverkehr-Manager.

## <a name="nested-endpoints"></a>Geschachtelte Endpunkte

Geschachtelte Endpunkte Kombinieren mehrerer Datenverkehr Manager Profile um flexible Datenverkehr-routing Phishingversuchen erstellen und die Anforderungen größeren, komplexen Bereitstellungen unterstützen. Mit Endpunkten geschachtelt wird ein Profil 'Kind' als Endpunkt eines Profils 'Parent' hinzugefügt. Das untergeordnete und das übergeordnete Profile können andere Endpunkte eines beliebigen Typs, einschließlich andere geschachtelte Profile enthalten. Weitere Informationen finden Sie unter [geschachtelte Datenverkehr-Manager-Profilen](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps als Endpunkte

Einige weitere Aspekte sollten bei Web Apps als Endpunkte in den Datenverkehr Manager konfigurieren:

1. Nur Web Apps auf "Standard" SKU oder höher, die für die Verwendung mit den Datenverkehr Manager berechtigt sind. Hinzufügen einer Web-App von einer niedrigere SKU Versuche fehl. Herunterstufen von der SKUs einer vorhandenen Web App führt in den Datenverkehr Manager Datenverkehr in die Web App nicht mehr zu senden.

2. Wenn Sie ein Endpunkt eine HTTP-Anforderung empfängt, wird 'Host' Header in der Besprechungsanfrage um zu bestimmen, welche Web App die Anfrage service sollte. Der Host-Header enthält den DNS-Namen verwendet, um die Anfrage, beispielsweise 'contosoapp.azurewebsites.net' einleiten. Wenn Sie einen anderen Namen für die DNS-Web-App verwenden zu können, muss der DNS-Name als einen benutzerdefinierten Domänennamen für die App registriert sein. Wenn Sie einen Endpunkt Web App als Azure Endpunkt hinzufügen, wird der DNS-Manager Datenverkehr Profilname automatisch für die App registriert. Diese Registrierung wird automatisch entfernt, wenn Sie der Endpunkt gelöscht wird.

3. Jedes Profil Datenverkehr Manager kann jeder Region Azure höchstens eine Web App-Endpunkt aufweisen. Wenn Sie für diese Einschränkung umgehen können Sie eine Web App als externe Endpunkt konfigurieren. Weitere Informationen finden Sie unter den [häufig gestellte Fragen](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Aktivieren und Deaktivieren von Endpunkten

Deaktivieren von außen liegenden Tabellenblättern in den Datenverkehr Manager kann nützlich sein, den Datenverkehr vorübergehend von außen liegenden Tabellenblättern zu entfernen, die Wartungsmodus oder, die erneut bereitgestellt wird. Nachdem Sie der Endpunkt erneut ausgeführt wird, kann es wieder aktiviert werden.

Endpunkte können aktiviert und deaktiviert, über den Datenverkehr Manager-Portal PowerShell, CLI oder REST-API, die sowohl Ressourcenmanager und das Bereitstellungsmodell klassischen unterstützt werden.

>[AZURE.NOTE] Deaktivieren einen Endpunkt Azure hat nichts mit Bereitstellungszustand in Azure. Ein Azure Service (z. B. bleibt eines virtuellen Computers oder Web App ausgeführt wird und zum Empfang von Datenverkehr, auch wenn in den Datenverkehr Manager deaktiviert. Datenverkehr kann direkt an die Instanz des Diensts und nicht über die DNS-Manager Datenverkehr Profilname berücksichtigt werden. Weitere Informationen finden Sie unter [Funktionsweise der Datenverkehr-Manager](traffic-manager-how-traffic-manager-works.md).

Die aktuelle Berechtigung jedes Endpunkts Datenverkehr empfangen hängt die folgenden Faktoren aus:

- Das Profil Status (aktiviert/deaktiviert)
- Der Endpunkt Status (aktiviert/deaktiviert)
- Die Ergebnisse der Gesundheit für diesen Endpunkt überprüft

Details finden Sie unter [den Datenverkehr Manager Endpunkt überwachen](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Da Ebene der DNS-Manager Datenverkehr funktioniert, kann es nicht vorhandene Verbindungen an einem beliebigen Endpunkt beeinflussen. Wenn Sie ein Endpunkt nicht verfügbar ist, weist Datenverkehr Manager neue Verbindungen mit einem anderen Endpunkt aus. Der Host hinter den Endpunkt deaktiviert oder fehlerhaft kann jedoch weiterhin Verkehr über vorhandene Verbindungen erhalten, bis Sie diese Sitzungen beendet werden. Applikationen sollte die Sitzungsdauer, um den Datenverkehr in abzuleiten aus vorhandene Verbindungen zulassen beschränken.

Wenn alle Endpunkte in einem Profil deaktiviert sind, oder wenn das Profil selbst deaktiviert ist, klicken Sie dann den Datenverkehr Manager eine 'NXDOMAIN' Antwort an eine neue DNS-Abfrage sendet.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kann ich den Datenverkehr Manager mit Endpunkten aus mehrere Abonnements verwenden?

Es ist nicht möglich, mit Azure Web Apps, Endpunkte aus mehrere Abonnements verwenden. Azure Web Apps erfordert, dass alle benutzerdefinierten Domänennamen mit Web Apps verwendet nur innerhalb eines einzelnen Abonnements verwendet wird. Es ist nicht möglich, Web Apps aus mehreren Abonnements mit dem gleichen Domänennamen zu verwenden.

Für andere Endpunkttypen ist es möglich, den Datenverkehr Manager mit Endpunkten aus mehr als einem Abonnement verwenden. Wie Sie den Datenverkehr Manager konfigurieren, hängt davon ab, ob Sie das Bereitstellungsmodell klassischen oder die Ressourcenmanager Oberfläche verwenden.

- In der Ressourcenmanager Endpunkte aus einem beliebigen Abonnement können als nach den Datenverkehr Manager hinzugefügt werden, solange die Person, die Konfiguration des Profils Datenverkehr Manager Lesezugriff auf den Endpunkt verfügt. Diese Berechtigungen können mit [Azure Ressourcenmanager rollenbasierte Access-Steuerelements (RBAC)](../active-directory/role-based-access-control-configure.md)erteilt werden.
- In der klassischen Bereitstellung Modell-Schnittstelle erfordert Datenverkehr Manager an, dass Cloud Services oder Web Apps konfiguriert als Azure Endpunkte im selben Abonnement als den Datenverkehr-Manager-Profil befinden. Cloud-Dienstendpunkte in anderen Abonnements können mit den Datenverkehr Manager als 'externe' Endpunkte hinzugefügt werden. Diese externen Endpunkte als Azure Endpunkte, anstatt die externe Rate in Rechnung gestellt werden.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kann ich den Datenverkehr Manager mit der Cloud-Dienst 'Staging' verwenden?

Ja. Cloud-Dienst staging' Steckplätze kann in den Datenverkehr Manager als externe Endpunkte konfiguriert sein. Integritätsprüfungen werden weiterhin Rate Azure Endpunkte belastet. Da der externe Endpunkttyp verwendet wird, werden Änderungen an der zugrunde liegenden Dienst nicht automatisch übernommen. Mit externen Endpunkten kann nicht Datenverkehr Manager erkennen, wenn die Cloud-Dienst beendet oder gelöscht wird. Daher weiterhin den Datenverkehr-Manager für integritätsprüfungen Abrechnung bis der Endpunkt deaktiviert oder gelöscht wird.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Unterstützt den Datenverkehr Manager IPv6 Endpunkte?

Datenverkehr Manager bietet aktuell keine IPv6 addressible Namenserver. Datenverkehr Manager kann jedoch weiterhin von IPv6-Clients Herstellen einer Verbindung mit IPv6 Endpunkte verwendet werden. Ein Client werden keine DNS-Anfragen direkt auf den Datenverkehr Manager gestellt. In diesem Fall verwendet der Client einen rekursive DNS-Dienst an. Ein reine IPv6-Client sendet Anfragen an die rekursive DNS-Dienst mit IPv6. Klicken Sie dann sollte der Dienst rekursive wenden Sie sich an den Datenverkehr Manager Namenserver IPv4 verwenden können.

Datenverkehr Manager reagiert mit der DNS-Name des Endpunkts an. Um einen Endpunkt IPv6 unterstützen, muss der Endpunkt DNS-Name auf die IPv6-Adresse zeigen DNS AAAA Datensatz vorhanden. Integritätsprüfungen Datenverkehr Manager unterstützt nur IPv4-Adressen. Der Dienst muss einen Endpunkt IPv4 auf dem gleichen DNS-Namen verfügbar zu machen.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kann ich den Datenverkehr Manager mit mehr als einem Web-App in der gleichen Region verwenden?

Normalerweise ist Datenverkehr Manager verwendet, um den Datenverkehr in die Anwendung bereitgestellt, die in unterschiedlichen Regionen direkte. Allerdings können sie auch verwendet werden, wo eine Anwendung mehrere Bereitstellung in derselben Region hat. Die Endpunkte Datenverkehr Manager Azure lässt nicht mehr als eine Web App-Endpunkt aus derselben Azure Region das gleiche Datenverkehr Manager Profil hinzugefügt werden.

Die folgenden Schritte bieten für diese Einschränkung umgehen:

1. Überprüfen Sie, dass Ihre Endpunkte in anderen Web app'skalieren Einheiten' sind. Ein Domänenname muss einer einzelnen Website in einem angegebenen Mengen Einheit zuordnen. Daher freigeben nicht zwei Web Apps in derselben Skala Einheit Datenverkehr Manager Profil.
2. Fügen Sie Ihres Domänennamens Eitelkeit als eine benutzerdefinierte Hostname bei jeder Web App an hinzu. Jede Web-App muss sich in einer anderen Maßstab Einheit. Alle Web Apps müssen den gleichen Abonnement gehören.
3. Hinzufügen einer (und nur eine) Web App-Endpunkt zu Ihrem Profil Datenverkehr Manager als Azure Endpunkt.
4. Hinzufügen von jede zusätzliche Web App-Endpunkt zu Ihrem Profil Datenverkehr Manager als externe Endpunkt. Externe Endpunkte können nur in das Modell zur Bereitstellung von Ressourcenmanager hinzugefügt werden.
5. Erstellen Sie einen DNS-CNAME-Eintrag in Ihrer Eitelkeit-Domäne, die auf Ihre Datenverkehr Manager DNS-Profilname verweist (<>.... trafficmanager.net).
6. Zugriff auf Ihre Website über den Eitelkeit Domänennamen nicht den Datenverkehr Manager DNS-Profilnamen ein.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, [den Datenverkehr Manager wie funktioniert](traffic-manager-how-traffic-manager-works.md).
- Informationen Sie zu den Datenverkehr Manager [Endpunkt Überwachung und automatisches Failover](traffic-manager-monitoring.md).
- Informationen Sie zu den Datenverkehr Manager [Datenverkehr Weiterleitung Methoden](traffic-manager-routing-methods.md).
