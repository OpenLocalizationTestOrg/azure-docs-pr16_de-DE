<properties
   pageTitle="Problembehandlung bei Fehlern bei der Anwendung Gateway Ungültiger Gateway (502) | Microsoft Azure"
   description="Informationen Sie zum Behandeln von Problemen mit der Anwendung Gateway 502-Fehlern"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Problembehandlung von Fehlern bei ungültiger Gateway in Application Gateway

## <a name="overview"></a>(Übersicht)

Nachdem Sie ein Gateway Azure konfiguriert haben, ist eine der treten möglicherweise Fehler "Serverfehler: 502 - Webserver hat eine ungültige Antwort beim fungieren als ein Gateway oder Proxy Server". Dies kann aus folgenden Hauptfenster Gründen auftreten:

- Back-End-Ressourcenpool Azure Application Gateway ist nicht konfiguriert oder leer.
- Keiner der virtuellen Computern oder Instanzen in virtuellen Computer Skalierung festlegen ist fehlerfrei.
- Back-End-virtuellen Computern oder Instanzen von virtuellen Computer Skalierung festlegen sind zum Standard Gesundheit Prüfpunkt nicht reagiert.
- Ungültige oder falscher Konfiguration der benutzerdefinierten Gesundheit überprüfen zu können.
- Fordern Sie Timeout oder Verbindungsprobleme mit benutzeranforderungen an.

## <a name="empty-backendaddresspool"></a>Leere BackendAddressPool

### <a name="cause"></a>Ursache

Wenn das Anwendungsgateway keine virtuellen Computern oder die virtuellen Computer Skalierung festlegen so konfiguriert ist, in dem Pool Back-End-Adressen enthält, konnten keine Kundenanfrage weitergeleitet werden und löst einen Ungültiger Gateway-Fehler.

### <a name="solution"></a>Lösung

Stellen Sie sicher, dass die Back-End-Adresse Ressourcenpool nicht leer ist. Dies kann entweder über PowerShell, CLI oder Portal erfolgen.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Die Ausgabe aus dem vorherigen Cmdlet sollte Pool nicht leere Back-End-Adressen enthalten. Im folgenden Beispiel, in dem sind zwei Pools der zurückgegeben wird, die mit den vollqualifizierten Domänennamen oder die IP-Adressen für die Back-End-virtuellen Computern konfiguriert sind. Der Bereitstellung Zustand des der BackendAddressPool muss 'wurde erfolgreich abgeschlossen werden".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Fehlerhafte Instanzen in BackendAddressPool

### <a name="cause"></a>Ursache

Wenn Sie alle Instanzen von BackendAddressPool Probleme aufweisen, müssten Application Gateway keine Back-End für Benutzer Anforderung an weiterleiten. Dies kann auch der Fall sein, wenn Back-End-Instanzen fehlerfrei sind aber verfügen nicht über die erforderliche Anwendung bereitgestellt.

### <a name="solution"></a>Lösung

Stellen Sie sicher, dass die Instanzen fehlerfrei sind und die Anwendung ordnungsgemäß konfiguriert ist. Überprüfen Sie, ob die Back-End-Instanzen von einem anderen virtuellen Computer in der gleichen VNet auf einen Ping reagieren können. Wenn mit einer öffentlichen Endpunkt konfiguriert, sicherstellen Sie, dass die Browseranforderung einer zur Webanwendung gewartet werden müssen.

## <a name="problems-with-default-health-probe"></a>Probleme mit der standardmäßigen Gesundheit Prüfpunkt

### <a name="cause"></a>Ursache

Fehler vom Typ 502 auch möglich häufig verwendeten Indikatoren, dass der Standard-Dienststatus Prüfpunkt nicht Back-End-virtuellen Computern erreichen kann. Wenn eine Anwendungsgateway-Instanz bereitgestellt wird, wird automatisch einen standardmäßigen Gesundheit Prüfpunkt zu jeder BackendAddressPool mit den Eigenschaften des der BackendHttpSetting konfiguriert. Dieser Prüfpunkt festgelegt ist keine Benutzereingabe erforderlich. Wenn Sie eine Regel für den Lastenausgleich konfiguriert ist, wird eine Zuordnung insbesondere zwischen einem BackendHttpSetting und BackendAddressPool geführt. Ein Prüfpunkt Standard für jeden der folgenden Zuordnungen konfiguriert ist, und Application Gateway initiiert eine regelmäßige Health Kontrollkästchen Verbindung jede Instanz in der BackendAddressPool am Port im Element BackendHttpSetting angegeben ist. Folgende Tabelle enthält die Werte der Standard-Dienststatus Prüfpunkt zugeordnet.


|Prüfpunkt-Eigenschaft | Wert | Beschreibung|
|---|---|---|
| Prüfpunkt-URL| http://127.0.0.1/ | URL-Pfad |
| Intervall | 30 | Prüfpunkt Intervall in Sekunden |
| Timeout für  | 30 | Prüfpunkt Timeout in Sekunden |
| Fehlerhafte Schwellenwert | 3 | Prüfpunkt "Wiederholen" zählen. Back-End-Server wird unten markiert, nachdem die Anzahl der aufeinander folgenden Prüfpunkt Fehler den fehlerhaften Schwellenwert erreicht. |

### <a name="solution"></a>Lösung

- Stellen Sie sicher, dass eine Standardwebsite konfiguriert ist und bei 127.0.0.1 hört.
- Wenn BackendHttpSetting einen anderen Port als 80 angegeben ist, sollte die Standard-Website bei diesem Port Abhören konfiguriert werden.
- Der Anruf an Http://127.0.0.1:port sollte einen HTTP-Ergebniscode 200 zurück. Dies sollte der 30 Sekunden Timeout zurückgegeben werden.
- Stellen Sie sicher, dass konfigurierten Port geöffnet ist und es sind keine Firewall-Regeln oder Azure Netzwerk-Sicherheitsgruppen, wodurch eingehenden oder ausgehenden Datenverkehr für den Port konfiguriert blockiert.
- Wenn Azure klassischen virtuellen Computern oder Cloud-Dienst mit FQDN oder öffentliche IP-Adresse verwendet wird, stellen Sie sicher, dass den entsprechenden [Endpunkt](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) geöffnet ist.
- Wenn der virtuellen Computer über Azure Ressourcenmanager konfiguriert ist und liegt außerhalb der VNet, wo Application Gateway bereitgestellt wird, muss [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) zum Zugreifen auf den gewünschten Port dürfen konfiguriert sein.


## <a name="problems-with-custom-health-probe"></a>Probleme mit benutzerdefinierten Gesundheit Prüfpunkt

### <a name="cause"></a>Ursache

Benutzerdefinierte Integrität Prüfpunkte zulassen zusätzliche Flexibilität bei der Überprüfung Verhalten Standard. Wenn Sie Benutzerdefinierte Probes verwenden, können Benutzer das Intervall Prüfpunkt, die URL, und Pfad zum Testen und wie viele Fehler beim Antworten auf annehmen, bevor Sie die Back-End-Ressourcenpool Instanz als fehlerhaft konfigurieren. Die folgenden zusätzlichen Eigenschaften hinzugefügt werden.

|Prüfpunkt-Eigenschaft| Beschreibung|
|---|---|
| Namen | Name der der Prüfpunkt. Dieser Name wird verwendet, verweisen Sie dem Prüfpunkt in die Back-End-HTTP-Einstellungen. |
| Protokoll | Protokoll, mit den Prüfpunkt senden. Der Prüfpunkt wird das Protokoll definiert, in die Back-End-HTTP-Einstellungen verwenden. |
| Host |  Hostname den Prüfpunkt senden. Nur anwendbar, wenn mehrere Website Application Gateway konfiguriert ist. Dies unterscheidet sich von Hostname des virtuellen Computers.  |
| Pfad | Relative Pfad der der Prüfpunkt. Der gültige Pfad beginnt von "/". Der Prüfpunkt wird gesendet, um \<Protokoll\>://\<Host\>:\<Port\>\<Pfad\> |
| Intervall | Prüfpunkt Intervall in Sekunden an. Dies ist das Zeitintervall zwischen zwei aufeinander folgende Stichproben aus.|
| Timeout für | Prüfpunkt Timeout in Sekunden an. Als fehlerhaft, wenn eine gültige Antwort nicht innerhalb dieses Zeitlimit eingeht, wird der Prüfpunkt markiert. |
| Fehlerhafte Schwellenwert | Prüfpunkt "Wiederholen" zählen. Back-End-Server wird unten markiert, nachdem die Anzahl der aufeinander folgenden Prüfpunkt Fehler den fehlerhaften Schwellenwert erreicht. |


### <a name="solution"></a>Lösung

Überprüfen Sie, ob die benutzerdefinierte Integrität Prüfpunkt richtig als der obigen Tabelle konfiguriert ist. Zusätzlich zu den vorherigen Schritten zur Problembehandlung auch Stellen Sie Folgendes sicher:

- Stellen Sie sicher, dass der Prüfpunkt gemäß des [Leitfadens](application-gateway-create-probe-ps.md)richtig angegeben ist.
- Wenn Application Gateway für eine einzelne Website so konfiguriert ist, sollte standardmäßig die Host Name als "127.0.0.1", angegeben werden, wenn andernfalls in benutzerdefinierte Probe konfiguriert.
- Stellen Sie sicher, dass einen Anruf nach http://\<Host\>:\<Port\>\<Pfad\> gibt einen HTTP-Ergebniscode 200.
- Stellen Sie sicher, dass das Intervall, Timeout und UnhealtyThreshold innerhalb der zulässigen Bereiche sind.

## <a name="request-time-out"></a>Timeout anfordern

### <a name="cause"></a>Ursache

Wenn eine Benutzer Anforderung eingeht, wird Application Gateway gilt für die Anforderung die konfigurierten Regeln und leitet sie an eine Instanz der Back-End-Pool. Es wird eine konfigurierbare Zeitspanne für eine Antwort aus der Back-End-Instanz wartet. Standardmäßig ist dieses Intervall **30 Sekunden**. Wenn Application Gateway keine Antwort von Back-End-Anwendung in diesem Intervall erhalten, würde Anforderung des Benutzers finden Sie unter Fehler 502.

### <a name="solution"></a>Lösung

Application Gateway ermöglicht Benutzern, so konfigurieren Sie diese Einstellung über BackendHttpSetting, die dann auf verschiedenen Pools angewendet werden können. Verschiedenen Back-End-Pools können verschiedene BackendHttpSetting und daher andere Anforderung Timeout konfiguriert haben.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Nächste Schritte

Wenn die vorherigen Schritte das Problem nicht beheben, öffnen Sie ein [Ticket unterstützen](https://azure.microsoft.com/support/options/).
