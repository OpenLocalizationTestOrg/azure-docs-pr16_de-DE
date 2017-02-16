

<properties
   pageTitle="Überwachung (Übersicht) für die Anwendungsgateway Azure Gesundheit | Microsoft Azure"
   description="Erfahren Sie mehr über die Überwachungsfunktionen in Azure Application Gateway"
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

# <a name="application-gateway-health-monitoring-overview"></a>Anwendung Gateway Gesundheit (Übersicht)

Azure Application Gateway standardmäßig überwacht die Integrität aller Ressourcen in ihren Back-End-Pool und automatisch alle Ressourcen, die als fehlerhaft aus dem Pool entfernt. Application Gateway befindet sich die fehlerhaften Instanzen überwachen und nachdem vorliegen, und auf Systemzustand Stichproben Antworten wieder zum fehlerfrei Back-End-Pool hinzugefügt. Anwendungsgateway sendet, dass die Integrität mit demselben Port untersucht, die in die Back-End-HTTP-Einstellungen definiert ist. Dadurch wird sichergestellt, dass der Prüfpunkt denselben Port überprüft, den Verbindung zu den Back-End-Kunden verwenden werden.

![Beispiel für Prüfpunkt gateway][1]

Zusätzlich zu Standard Gesundheit Prüfpunkt für die Überwachung verwenden, können Sie auch den Dienststatus Prüfpunkt Ihrer Anwendung Anforderungen entsprechend anpassen. In diesem Artikel werden sowohl Standard- und benutzerdefinierte Gesundheit Prüfpunkte behandelt.

## <a name="default-health-probe"></a>Prüfpunkt der Standard-Dienststatus

Ein Gateway konfiguriert automatisch einen Standardwert Gesundheit Prüfpunkt beim Einrichten einer beliebigen benutzerdefinierten Prüfpunkt Konfiguration nicht. Das Überwachung Verhalten funktioniert, wenn Sie eine HTTP-Anforderung an die IP-Adressen für die Back-End-Pool konfiguriert. Für Prüfpunkte Standard Wenn die Back-End-http-Einstellungen bei HTTPS konfiguriert sind verwenden der Prüfpunkt sowie Https Integritätsstatus der Downloadzeit testen.

Beispiel: Konfigurieren von Ihrer Anwendung Gateways Back-End-Server A, B und C verwenden, um HTTP-Netzwerkverkehr an Port 80 zu erhalten. Die standardmäßige Gesundheit Überwachung überprüft die drei Servern alle 30 Sekunden für eine fehlerfrei HTTP-Antwort. Eine fehlerfreie HTTP-Antwort enthält [Statuscode](https://msdn.microsoft.com/library/aa287675.aspx) zwischen 200 und 399 ein.

Wenn das Kontrollkästchen Standard Prüfpunkt für Server A fehlschlägt, das Anwendungsgateway entfernt sie aus dem Pool Back-End- und Netzwerkverkehr beendet parallelen auf diesem Server. Der Standard-Prüfpunkt weiterhin weiterhin Server ein alle 30 Sekunden überprüfen. Wenn Server A aus einer standardmäßigen Gesundheit Prüfpunkt erfolgreich auf eine Anforderung reagiert, wird es als wieder fehlerfrei an den Back-End-Pool hinzugefügt und den Datenverkehr beginnt erneut auf dem Server entdeckt.

### <a name="default-health-probe-settings"></a>Gesundheit Prüfpunkt-Standardeinstellungen

|Prüfpunkt-Eigenschaft | Wert | Beschreibung|
|---|---|---|
| Prüfpunkt-URL| http://127.0.0.1:\<Port\>/ | URL-Pfad |
| Intervall | 30 | Prüfpunkt Intervall in Sekunden |
| Timeout für  | 30 | Prüfpunkt Timeout in Sekunden |
| Fehlerhafte Schwellenwert | 3 | Prüfpunkt "Wiederholen" zählen. Back-End-Server wird unten markiert, nachdem die Anzahl der aufeinander folgenden Prüfpunkt Fehler den fehlerhaften Schwellenwert erreicht. |

> [AZURE.NOTE] Der Port werden immer denselben Port wie die Back-End-HTTP-Einstellungen.

Der Prüfpunkt Standard nur bei http://127.0.0.1 aussieht:\<Port\> Integritätsstatus zu bestimmen. Wenn Sie zum Konfigurieren des Dienststatus Prüfpunkts zum Wechseln zu einer benutzerdefinierten URL oder andere Einstellungen ändern müssen, müssen Sie Benutzerdefinierte Probes verwenden, wie in den folgenden Schritten beschrieben.

## <a name="custom-health-probe"></a>Benutzerdefinierte Integrität Prüfpunkt

Benutzerdefinierte Probes zulassen, dass Sie eine genauere Steuerung der Dienststatus Überwachung besitzen. Wenn Sie Benutzerdefinierte Probes verwenden, können Sie das Intervall Prüfpunkt, die URL und Pfad zum Testen und wie viele Fehler beim Antworten auf annehmen, bevor Sie die Back-End-Ressourcenpool Instanz als fehlerhaft konfigurieren.

### <a name="custom-health-probe-settings"></a>Benutzerdefinierte Integrität Prüfpunkt Einstellungen

Die folgende Tabelle enthält Definitionen für die Eigenschaften eines benutzerdefinierten Gesundheit Probe an.

|Prüfpunkt-Eigenschaft| Beschreibung|
|---|---|
| Namen | Name der der Prüfpunkt. Dieser Name wird verwendet, verweisen Sie dem Prüfpunkt in die Back-End-HTTP-Einstellungen. |
| Protokoll | Protokoll, mit den Prüfpunkt senden. Der Prüfpunkt wird das Protokoll definiert, in die Back-End-HTTP-Einstellungen verwenden. |
| Host |  Hostname den Prüfpunkt senden. Geltendes nur, wenn mehrere Website ist so konfiguriert, dass auf Gateway-Anwendung, andernfalls "127.0.0.1" verwenden. Dies unterscheidet sich von Hostname des virtuellen Computers. |
| Pfad | Relative Pfad der der Prüfpunkt. Der gültige Pfad beginnt von "/". |
| Intervall | Prüfpunkt Intervall in Sekunden an. Dies ist das Zeitintervall zwischen zwei aufeinander folgende Stichproben aus.|
| Timeout für | Prüfpunkt Timeout in Sekunden an. Als fehlerhaft, wenn eine gültige Antwort nicht innerhalb dieses Zeitlimit eingeht, wird der Prüfpunkt markiert. |
| Fehlerhafte Schwellenwert | Prüfpunkt "Wiederholen" zählen. Back-End-Server wird unten markiert, nachdem die Anzahl der aufeinander folgenden Prüfpunkt Fehler den fehlerhaften Schwellenwert erreicht. |

> [AZURE.IMPORTANT] Wenn Application Gateway für eine einzelne Website so konfiguriert ist, sollte standardmäßig die Host Name als "127.0.0.1", angegeben werden, wenn andernfalls in benutzerdefinierte Probe konfiguriert.
Zu Referenzzwecken ist eine benutzerdefinierte Probe an gesendet \<Protokoll\>://\<Host\>:\<Port\>\<Pfad\>. Der Port verwendet werden denselben Port aus, wie in die Back-End-HTTP-Einstellungen definiert sind.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Kennenlernen der Anwendungsgateway Gesundheit Überwachung, können Sie einen [benutzerdefinierten Gesundheit Prüfpunkt](application-gateway-create-probe-portal.md) in der Azure-Portal oder einen [benutzerdefinierten Gesundheit Prüfpunkt](application-gateway-create-probe-ps.md) mit PowerShell und das Modell zur Bereitstellung von Azure Ressourcenmanager konfigurieren.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png