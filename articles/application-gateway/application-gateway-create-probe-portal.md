<properties
   pageTitle="Erstellen Sie einen benutzerdefinierten Prüfpunkt für ein Gateway mit dem Portal | Microsoft Azure"
   description="Erfahren Sie, wie eine benutzerdefinierte Probe für Application Gateway zu erstellen, mit dem portal"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Erstellen Sie einen benutzerdefinierten Prüfpunkt für Application Gateway mithilfe des Portals

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-probe-ps.md)
- [Azure klassischen PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Szenario

Erstellen einen benutzerdefinierten Gesundheit Prüfpunkt in einer vorhandenen Anwendung Gateways, das folgende Szenario durchläuft.
Das Szenario setzt voraus, dass Sie die Schritte zum [Erstellen eines Gateways Anwendung](application-gateway-create-gateway-portal.md)bereits befolgt haben.

## <a name="a-namecreateprobeacreate-the-probe"></a><a name="createprobe"></a>Erstellen des Prüfpunkts

Prüfpunkte werden in zwei Schritten über das Portal konfiguriert. Der erste Schritt besteht im Erstellen des Prüfpunkts, fügen Sie des Prüfpunkts an den Back-End-http-Einstellungen des Gateways Anwendung.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu http://portal.azure.com, und wählen Sie eine vorhandene Anwendungsgateway.

![Übersicht über die Anwendung Gateway][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Suchen** , und klicken Sie auf die Schaltfläche **Hinzufügen** , um einen neuen Prüfpunkt hinzuzufügen.

![Hinzufügen von Prüfpunkt Blade mit Informationen ausgefüllt][2]

### <a name="step-3"></a>Schritt 3

Füllen Sie die erforderlichen Informationen für den Prüfpunkt aus, und klicken Sie auf **OK**, wenn Sie fertig sind.

- **Name** – Dies ist einen Anzeigenamen für den Prüfpunkt, die im Portal zugegriffen werden kann.
- **Host** – Dies ist der Hostname, der für den Prüfpunkt verwendet wird. Geltendes nur, wenn mehrere Website ist so konfiguriert, dass auf Gateway-Anwendung, andernfalls "127.0.0.1" verwenden. Dies unterscheidet sich von Hostname des virtuellen Computers.
- **Pfad** – den Rest der vollständige Url für den benutzerdefinierten Prüfpunkt. Ein gültiger Pfad beginnt mit "/"
- **Intervall (Sekunden)** - wie oft der Prüfpunkt zum Überprüfen der Integrität ausgeführt wird. Es empfiehlt sich nicht in der unteren festlegen als 30 Sekunden.
- **Timeout (Sekunden)** - Zeit, die der Prüfpunkt, bevor ein Timeout erreicht ist wartet. Das Timeoutintervall muss hoch genug sein, dass ein http-Anruf bereitgestellt werden kann, um sicherzustellen, dass die Back-End-Dienststatus Seite verfügbar ist.
- **Fehlerhafte Schwellenwert** - Anzahl fehlgeschlagener Versuche fehlerhaften berücksichtigt werden. Ein Schwellenwert von 0 bedeutet, dass, wenn eine Gesundheit Kontrollkästchen fehlschlägt die Back-End fehlerhaften sofort bestimmt werden.

> [AZURE.IMPORTANT] der Hostname ist nicht den Servernamen. Dies ist der Name des virtuellen Hosts auf dem Anwendungsserver ausgeführt. Der Prüfpunkt wird an Http://(host name):(port from httpsetting)/UrlPath gesendet.

![Prüfpunkt Konfiguration Einstellungen][3]

## <a name="add-probe-to-the-gateway"></a>Prüfpunkt mit dem Gateway hinzufügen

Nachdem der Prüfpunkt erstellt wurde, ist das Gateway hinzugefügt werden. Prüfpunkt Einstellungen sind für die Back-End-http-Einstellungen des Gateways Anwendung festlegen.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf die **HTTP-Einstellungen** des Gateways Anwendung, und klicken Sie dann auf der aktuellen Back-End-HTTP-Einstellungen im Fenster, um die Konfiguration Blade anzuzeigen.

![HTTPS-Einstellungen-Fenster][4]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf das Blade **AppGatewayBackEndHttp** Einstellungen klicken Sie auf **benutzerdefinierte Probe verwenden** , und wählen Sie den Prüfpunkt im Abschnitt [Erstellen der Prüfpunkt](#createprobe) erstellt.
Wenn Sie fertig sind, klicken Sie auf **OK** und die Einstellungen angewendet werden.

![Appgatewaybackend Einstellungen blade][5]

Der Standard-Prüfpunkt überprüft den Standardzugriff auf die Webanwendung. Nun ein benutzerdefinierter Prüfpunkt erstellt wurde, verwendet das Anwendungsgateway den benutzerdefinierten Pfad zum Überwachen der Dienststatus für die Back-End-ausgewählt definiert. Basierend auf den Kriterien, die definiert wurde, überprüft das Anwendungsgateway in der Prüfpunkt angegebene Datei. Wenn der Anruf an Host: Port / Pfad eine Http 200 OK Statusantwort nicht zurück, der Server ist von Drehung, nach der fehlerhafte Schwellenwert erreicht wird geöffnet. Überprüfung weiterhin auf den fehlerhaften Instanz ermitteln, zu welchem wieder fehlerfrei ist. Nachdem die Instanz wieder hinzugefügt wurde fehlerfrei Server Pool Datenverkehr beginnt parallelen darauf erneut und Überprüfung auf die Instanz Benutzer angegebenen Intervall wie gewohnt.


## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von SSL Verschiebung mit Azure Application Gateway finden Sie unter [Konfigurieren von SSL Auslagern](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png