
# <a name="azure-and-internet-of-things"></a>Azure und das Internet der Dinge

Willkommen bei Microsoft Azure und im Internet Aktionen (IoT). In diesem Artikel wird eine IoT Lösungsarchitektur, die die gemeinsamen Merkmale einer Lösung IoT beschrieben, die Sie bereitstellen möglicherweise Azure Services verwenden. IoT Lösungen Sie sicherer, bidirektionale Kommunikation zwischen Geräten oftmals in Millionen und eine Lösung Back-End Nummerierung. Angenommen, können eine Lösung Back-End automatisierte, Vorhersage Analytics Sie Folgendes für eine Steigerung Einsichten aus Ihrem Gerät-Cloud-Ereignisstream.

Azure IoT Hub ist einer der Hauptbausteine beim implementieren diese IoT Lösungsarchitektur Azure Services verwenden. IoT-Suite bietet abgeschlossen, End-to-End-Implementierungen dieser Architektur für bestimmte IoT Szenarien. Beispiel: 

- Die Lösung *remote-Überwachung* können Sie den Status von Geräten wie Verkaufsautomaten zu überwachen. 
- Die Lösung *Vorhersage Wartung* hilft Ihnen die Wartung Anforderungen Geräten wie Pumpen in remote entsprechend Sender im Vorfeld definieren und zur Vermeidung von ungeplanten Ausfallzeiten.

## <a name="iot-solution-architecture"></a>IoT Lösungsarchitektur

Das folgende Diagramm zeigt eine normale IoT Lösungsarchitektur. Das Diagramm nicht die Namen von bestimmter Azure Dienste enthält, aber enthält die wichtigsten Elemente in einer allgemeinen IoT Lösungsarchitektur. In dieser Architektur sammeln IoT Geräte von Daten, die sie zu einem Cloud Gateway zu senden. Cloud Gateways bereitgestellt, die Daten für die Verarbeitung von anderen Back-End-Diensten aus, in dem Daten in anderen Programmen Line-of-Business oder personenbezogenen Operatoren über ein Dashboard oder ein anderes Gerät Präsentation übermittelt werden.

![IoT Lösungsarchitektur][img-solution-architecture]

> [AZURE.NOTE] Eine ausführliche Erläuterung von IoT Architektur finden Sie unter der [Microsoft Azure IoT Bezug Architektur][lnk-refarch].

### <a name="device-connectivity"></a>Device-Konnektivität

In dieser Architektur IoT Lösung senden Geräte werden, wie z. B. Sensorwerte aus einer Konsole pumping an einen Endpunkt Cloud gespeichert und verarbeitet. In einem Szenario Vorhersage Wartung möglicherweise die Back-End Streams von Sensordaten verwenden, um festzustellen, wann eine bestimmte Pumpe Wartung erfordert. Geräte können auch erhalten und reagieren auf Befehle Cloud-zu-Gerät Nachrichten in einen Cloud-Endpunkt gelesen werden. Möglicherweise im Szenario Vorhersage Wartung der Lösung Back-End senden Sie beispielsweise Befehle an andere Pumpen in der pumping benötigt beginnen erneutes Zahlungen, unmittelbar vor dem Wartung, um sicherzustellen, dass die Wartung Engineering loslegen kann, wenn Anna ankommt begonnen.

Zu den wichtigsten Aufgaben gegenüberliegende IoT Projekte Weise zuverlässig und sicher Geräte mit der Lösung Back-End verbinden. IoT Geräte haben unterschiedliche Merkmalen im Vergleich zu anderen-Clients wie Browser und mobile-apps. IoT Geräte:

- Werden häufig eingebettete Systeme mit keine personenbezogenen Operator.
- Kann in remote-Standorten bereitgestellt werden, wo der physische Zugriff teurer ist.
- Kann nur über die Back-End Lösung erreichbar sein. Es gibt keine andere Möglichkeit zur Interaktion mit dem Gerät aus.
- Möglicherweise Power und Verarbeitung Ressourcen zugänglich gemacht.
- Möglicherweise müssen Sie bei unterbrochener, langsam oder teure Netzwerkkonnektivität.
- Möglicherweise müssen Sie vertrauliche, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
- Können mit einem großen Satz von beliebte Hard- und Software Plattformen erstellt werden.

Zusätzlich zu den oben genannten Vorschriften muss alle IoT Lösung auch Maßstab, Sicherheit und Zuverlässigkeit vorführen. Die resultierende Datensatzgruppe Connectivity Anforderungen ist schwer und zeitaufwändiger traditionelle Technologien wie Web Container verwenden und Makler messaging implementiert werden. Azure IoT Hub und den Geräte-SDKs IoT erleichtern Lösungen implementiert wird, die diese Anforderungen entsprechen.

Ein Gerät direkt mit einem Cloud Gateway-Endpunkt kommunizieren kann, oder wenn das Gerät eines der Kommunikationsprotokolle, die das Cloud Gateway unterstützt verwenden können, können Sie über eine mittlere Gateway verbinden. Beispielsweise [Azure IoT Protokollgateway] [ lnk-protocol-gateway] Protokoll Übersetzung ausführen können, wenn Geräte eines der Protokolle nicht verwenden können, die IoT Hub unterstützt.

### <a name="data-processing-and-analytics"></a>Datenverarbeitung und analytics

In der Cloud ist eine Lösung wieder beenden IoT, in dem meisten der Datenverarbeitung auftritt, z. B. Filtern und aggregieren werden und das mit anderen Diensten weiterleiten. Beenden die IoT Lösung zurück:

- Werden bei aus Ihren Geräten empfängt und bestimmt, wie die Daten verarbeiten und speichern. 
- Können Sie Befehle aus der Cloud zu bestimmten Gerät zu senden.
- Stellt Gerät Registrierung-Funktionen, mit denen Sie Geräte bereitstellen und welche Geräte ausgewählt werden, die Verbindung zu Ihrer Infrastruktur Steuerelement an.
- Können Sie den Status Ihrer Geräte nachverfolgen und deren Aktivitäten zu überwachen.

In dem Szenario Vorhersage Wartung speichert die Lösung Back-End zurückliegenden werden Daten. Die Back-End können diese Daten verwenden, um Muster zu identifizieren, die darauf hinweisen, dass die Wartung auf einer bestimmten Pumpe fällig ist.

IoT-Lösungen können automatische Feedback Schleifen sind. Beispielsweise kann ein Modul Analytics in die Back-End aus werden identifizieren möchten, die die Temperatur eines bestimmten Geräts über normalen Geschäftsjahre Ebenen liegt. Die Lösung kann ein Befehls klicken Sie dann auf das Gerät, anweisen, er Berichtigung bezüglich senden.

### <a name="presentation-and-business-connectivity"></a>Präsentation "und" Business connectivity

Die Präsentation und Business Connectivity Ebene kann Endbenutzer Interaktion mit der Lösung IoT und die Geräte. Es ermöglicht Benutzern das Anzeigen und Analysieren von ihren Geräten gesammelten Daten. Diese Ansichten dauert das Formular oder in der Nähe Echtzeitdaten Dashboards oder BI-Berichte, die beide zurückliegenden Daten angezeigt werden können. Beispielsweise kann ein Operator zum Status von bestimmten entsprechend Rechner überprüfen und finden Sie unter alle Benachrichtigungen, durch das System ausgelöst. Diese Ebene kann auch die Integration von IoT Lösung Back-End mit vorhandenen Branchen Programme in Enterprise-Geschäftsprozessen oder Workflows einbinden. Beispielsweise kann die Lösung Vorhersage Wartung mit einem Terminplan System integrieren, die Bücher ein Engineering um einen pumping Sender besuchen, wenn die Lösung eine Unterdruckpumpe benötigen Wartung bezeichnet.

![IoT Lösung dashboard][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
