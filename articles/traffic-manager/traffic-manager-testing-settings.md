<properties
    pageTitle="Testen der Einstellungen für den Datenverkehr Manager | Microsoft Azure"
    description="In diesem Artikel helfen Ihnen Datenverkehr Manager-Einstellungen zu testen"
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

# <a name="test-your-traffic-manager-settings"></a>Testen Sie Ihre Datenverkehr-Manager

Klicken Sie zum Testen der Einstellungen für den Datenverkehr Manager müssen Sie an verschiedenen Stellen mehrere Clients haben aus dem Sie die Tests ausführen können. Zeigen Sie dann die Endpunkte in Ihrem Profil Datenverkehr Manager nach unten einzeln ein.

* Legen Sie den Wert für TTL DNS niedrig, sodass schnell (z. B. 30 Sekunden) weiterleiten.
* Kennen Sie die IP-Adressen Ihrer Azure-Cloud-Dienste und Websites in das Profil, das Sie testen möchten.
* Verwenden Sie die Tools, mit denen Sie einen Namen für die DNS-Einträge in eine IP-Adresse aufzulösen, und zeigen Sie die Adresse.

Sie prüfen, um festzustellen, ob die DNS-Namen in IP-Adressen der Endpunkte in Ihrem Profil beheben. Die Namen sollten in Übereinstimmung mit den Datenverkehr routing Methode im Profil Datenverkehr-Manager definierte lösen. Sie können die Tools wie **Nslookup** oder **einsteigen** zum Auflösen von DNS-Namen verwenden.

Die folgenden Beispiele helfen Sie Ihr Profil Datenverkehr Manager zu testen.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Überprüfen Sie den Datenverkehr Manager Profil mithilfe von Nslookup und Ipconfig in Windows

1. Öffnen Sie einen Befehl oder ein Windows PowerShell-Eingabeaufforderung als Administrator an.
2. Typ `ipconfig /flushdns` DNS-Auflösungscache zu leeren.
3. Typ `nslookup <your Traffic Manager domain name>`. Prüft der folgende Befehl beispielsweise den Domänennamen mit dem Präfix *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Eine typische Ergebnis zeigt die folgende Informationen:

    * Die DNS-Namen und die IP-Adresse des DNS-Servers zugegriffen wird, um diesen Datenverkehr Manager Domänennamen zu beheben.
    * Der Domänenname Datenverkehr Manager eingegebenen auf die Befehlszeile nach "Nslookup" und die IP-Adresse, die die Datenverkehr Manager Domäne aufgelöst. Die zweite IP-Adresse ist das wichtige zu überprüfen. Es sollte eine öffentliche virtuelle IP-Adresse (VIP) Adresse für eine der Clouddienste oder Websites im Datenverkehr-Manager-Profil, die Sie testen möchten übereinstimmen.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Zum Testen der Failover Datenverkehr routing Methode

1. Lassen Sie alle Endpunkte nach oben.
2. Mit einem einzelnen Client, fordern Sie DNS-Auflösung für Ihr Unternehmen Domänennamen mit Nslookup oder einem ähnlichen Programm.
3. Stellen Sie sicher, dass die gelöst IP-Adresse den primären Endpunkt entspricht.
4. Zeigen Sie Ihre primäre Endpunkt oder entfernen Sie die Überwachung Datei, damit diese Datenverkehr Manager geht davon aus, dass die Anwendung nach unten.
5. Warten Sie die DNS-Time-to-Live (TTL) des das Profil Datenverkehr Manager sowie eine zusätzliche zwei Minuten. Wenn Ihre DNS-TTL 300 Sekunden (5 Minuten) ist, müssen Sie beispielsweise sieben Minuten warten.
6. Leeren Sie Ihre DNS-Client-Cache und Anforderung DNS-Auflösung mithilfe von Nslookup. In Windows können Sie Ihre DNS-Cache mit dem Befehl Ipconfig/flushdns leeren.
7. Stellen Sie sicher, dass die gelöst IP-Adresse den sekundären Endpunkt entspricht.
8. Wiederholen Sie den Vorgang, die wiederum Deaktivierung des jeder Endpunkt aus. Stellen Sie sicher, dass die DNS-Einträge in der Liste die IP-Adresse des nächsten Endpunkts zurückgibt. Wenn alle Endpunkte unten sind, sollten Sie die IP-Adresse des primären Endpunkts erneut beziehen.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>So testen Sie die gewichteten Datenverkehr routing Methode

1. Lassen Sie alle Endpunkte nach oben.
2. Mit einem einzelnen Client, fordern Sie DNS-Auflösung für Ihr Unternehmen Domänennamen mit Nslookup oder einem ähnlichen Programm.
3. Stellen Sie sicher, dass die gelöst IP-Adresse stimmt mit einem Ihrer Endpunkte.
4. Leeren Sie Ihr DNS-Client-Cache zu, und wiederholen Sie die Schritte 2 und 3 für jeden Endpunkt. Sie sollten unterschiedliche IP-Adressen für jeden Ihrer Endpunkte zurückgegeben sehen.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>So testen Sie die Leistung Datenverkehr routing Methode

Um eine Leistung Datenverkehr routing Methode effektiv zu testen, müssen Sie den Kunden in verschiedene Teile der Welt verfügen. Sie können Clients in unterschiedlichen Azure Regionen erstellen, die verwendet werden können, um Ihre Dienste zu testen. Wenn Sie ein globales Netzwerk verfügen, können Sie Remote melden Sie sich bei Clients in andere Teile der Welt und führen Sie die Tests von dort aus.

Alternativ sind frei webbasierten DNS-Suche und einsteigen verfügbar. Einige dieser Tools bieten Ihnen der Möglichkeit, die Auflösung von DNS-Namen aus verschiedenen Speicherorten auf der ganzen Welt zu überprüfen. Führen Sie eine Suche nach "DNS-Suche" Beispiele für ein. Drittanbieter-Dienste wie Gomez oder Leitgedanke können verwendet werden, zu bestätigen, dass Ihre Profile Datenverkehr verteilen sind, wie erwartet.

## <a name="next-steps"></a>Nächste Schritte

* [Zu den Datenverkehr Manager Datenverkehr Weiterleitung Methoden](traffic-manager-routing-methods.md)
* [Datenverkehr Manager Leistungsaspekte](traffic-manager-performance-considerations.md)
* [Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)




