<properties
   pageTitle="Aktivieren von SSL-Richtlinie und Ende zum SSL auf Application Gateway | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die Anwendungsgateway durchgehende SSL unterstützen."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Aktivieren von SSL-Richtlinie und Ende zum SSL auf Gateway-Anwendung

## <a name="overview"></a>(Übersicht)

Anwendungsgateway unterstützt SSL-Beendigung auf dem Gateway, nach welcher Datenverkehr in der Regel fließt an die Back-End-Server entschlüsselt. Dadurch wird die Webservern aus teure Verschlüsselung/entschlüsseln Verwaltungsaufwand überflüssigen sein. Für einige Kunden ist unverschlüsselter Kommunikation auf die Back-End-Server jedoch keine zulässige Option. Dies wird möglicherweise durch Sicherheits-Compliance-Anforderungen oder die Anwendung möglicherweise nur sichere Verbindung akzeptieren. Solche Anwendungsmöglichkeiten unterstützt Anwendungsgateway jetzt durchgehende SSL-Verschlüsselung.

Ende an das Ende SSL ermöglicht es Ihnen, vertrauliche Daten in die Back-End-verschlüsselte weiterhin nutzen Sie die Vorteile von Features für Lastenausgleich Layer 7 sicher übertragen das Anwendungsgateway, beispielsweise Cookies Zugehörigkeit, URL-basierten routing, unterstützt das routing auf Websites oder die Möglichkeit zum Einfügen von X basierend-weitergeleiteten-* Header.

Wenn mit einem Ende zum SSL-Kommunikationsmodus konfiguriert ist, Anwendungsgateway Benutzer SSL-Sitzungen auf dem Gateway beendet, und Benutzerdatenverkehr entschlüsselt. Anschließend werden die konfigurierten Regeln zum Auswählen einer geeigneten Back-End-Ressourcenpool Instanz für die Weiterleitung Verkehr zu angewendet. Anwendungsgateway danach initiiert eine neue SSL-Verbindung mit dem Back-End-Server und verschlüsselt wieder Daten mithilfe des Back-End-Servers Zertifikat für öffentliche Schlüssel vor der Anforderung an die Back-End-Übertragung. Ende, um SSL beenden ist durch Festlegen der Einstellung für Protokoll in BackendHTTPSetting zu Https, die dann auf einem Back-End-Ressourcenpool angewendet wird aktiviert. Jede Back-End-Server in die Back-End-Pool mit einem Ende zum SSL aktiviert muss mit einem Zertifikat zum Zulassen sicheren Kommunikation konfiguriert sein.

![Ende zum Ssl-Szenario][1]

In diesem Beispiel werden Anfragen mit TLS1.2 an Back-End-Server in Pool1 unter Verwendung von SSL durchgehende weitergeleitet.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Ende, um SSL und mithilfe von Zertifikaten zu beenden.

Anwendungsgateway kommuniziert nur mit bekannten Back-End-Instanzen, die auf der weißen Liste ihres Zertifikats mit dem Anwendungsgateway aufweisen. Um mithilfe von Zertifikaten zu aktivieren, müssen Sie den öffentlichen Schlüssel Back-End-Serverzertifikate mit dem Anwendungsgateway (nicht im Stammzertifikat) hochladen. Nur Verbindungen mit bekannte und auf weißer Liste Downloadzeit sind dann zulässig. Die verbleibende Downloadzeit führt zu einem Gateway-Fehler auf. Selbstsignierte Zertifikaten sind Testzwecken nur und nicht für Produktionsarbeitslasten empfohlen. Solche Zertifikate muss auch auf weißer Liste mit dem Anwendungsgateway wie in den vorherigen Schritten beschrieben, bevor sie verwendet werden können.

## <a name="application-gateway-ssl-policy"></a>Anwendung Gateway SSL-Richtlinien

Anwendungsgateway unterstützt Benutzer konfigurierbare SSL Aushandlungsrichtlinien, die Kunden mehr Kontrolle über SSL-Verbindungen auf dem Anwendungsgateway ermöglichen.

1. SSL 2.0 und 3.0 für alle Anwendungsgateways standardmäßig deaktiviert. Sie sind nicht konfigurierbare überhaupt.
2. SSL Richtlinie Definition bietet Sie option So deaktivieren Sie eines der folgenden Protokolle 3 - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Wenn keine SSL-Richtlinie definiert ist alle drei (TLSv1\_0, TLSv1\_1, TLSv1_2) aktiviert werden.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Kennenlernen Ende, um Ende SSL und SSL-Richtlinie, wechseln Sie zu [Ende zum SSL auf Anwendungsgateway zu aktivieren](application-gateway-end-to-end-ssl-powershell.md) aus, um ein Gateway mit Möglichkeit zum Senden von Datenverkehr an Downloadzeit in verschlüsselter Form zu erstellen.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png