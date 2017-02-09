## <a name="load-balancer-differences"></a>Laden Sie Lastenausgleich Unterschiede

Es gibt verschiedene Optionen zum Verteilen mit Microsoft Azure Netzwerkdatenverkehr aus. Diese Optionen funktionieren anders voneinander, müssen ein anderes Feature festlegen und die verschiedenen Szenarios unterstützen. Sie können jede in Grad der Isolation verwendet werden, oder sie kombinieren.

- **Azure Lastenausgleich** funktioniert bei den Transport Layer (Ebene 4 in den OSI-Netzwerk Bezug Stapel). Darüber Netzwerk Ebene Verteilung der Verkehr über Instanzen einer Anwendung in derselben Azure Data Center ausgeführt.

- **Application Gateway** funktioniert bei der Anwendung Layer (Ebene 7 in den OSI-Netzwerk Bezug Stapel). Es fungiert als Reverse Proxy-Dienst, die Clientverbindung getrennt wird und Weiterleiten von Besprechungsanfragen an Back-End-Endpunkte.

- **Datenverkehr-Manager** Ebene der DNS-Einträge zu erhalten.  DNS-Antworten verwendet, um die direkte Endbenutzer den Datenverkehr in Global verteilten Endpunkte. Clients schließen klicken Sie dann direkt an diese Endpunkte.

In der folgenden Tabelle werden die Features von jedem Dienst angeboten zusammengefasst:

| Dienst | Azure Lastenausgleich | Application Gateway | Datenverkehr-Manager |
|---|---|---|---|
|Technologie| Transportebene (Ebene 4) | Anwendungsebene (Layer 7) | DNS-Ebene |
| Unterstützte Anwendungsprotokolle | Alle | HTTP und HTTPS |  Alle (ein HTTP-Endpunkt ist für die Überwachung Endpunkt erforderlich) |
| Endpunkte | Azure-virtuellen Computern und Cloud Services Rolleninstanzen | Alle Azure interne IP-Adresse oder öffentlichen Internet IP-Adresse | Azure-virtuellen Computern, Cloud Services, Azure Web Apps und externen Endpunkte |
| Vnet-Unterstützung | Kann für beide Internet zugänglichen und internen (Vnet) Applications verwendet werden | Kann für beide Internet zugänglichen und internen (Vnet) Applications verwendet werden |    Unterstützt nur Internet zugänglichen Applikationen |
Endpunkt für die Überwachung | Über Prüfpunkte unterstützt | Über Prüfpunkte unterstützt | Über das HTTP-/HTTPS-GET unterstützt | 

Azure Lastenausgleich und Application Gateway Routing Netzwerk Datenverkehr an Endpunkte, aber andere Verwendungsszenarien zu welcher Datenverkehr verarbeitet haben. In der folgenden Tabelle wird der Unterschied zwischen den beiden Lastenausgleich:

| Typ | Azure Lastenausgleich | Application Gateway |
|---|---|---|
| Protokolle | UDP/TCP | HTTP / HTTPS |
| IP-Reservierung | Unterstützt | Nicht unterstützt | 
| Laden Sie Lastenausgleich Modus | 5-tuple(source IP, source port, destination IP, destination port, protocol type) | Round Robert<br>Routing basierend auf URL | 
| Modus für den Lastenausgleich (Datenquelle IP / Kurznotizen Sitzungen) |  2-Tupel (Quelle IP- und IP-Zieladresse), 3-Tupel (Quelle IP-IP-Zieladresse und Port). Können nach oben oder unten basierend auf der Anzahl von virtuellen Computern skalieren | Cookie-basierten Zugehörigkeit<br>Routing basierend auf URL |
| Gesundheit Prüfpunkte | Standard: Prüfpunkt Intervall - 15 Sekunden. Aus der Rotation ausgenommen: 2 fortlaufender Fehlern. Unterstützt User defined untersucht | Im Leerlauf Prüfpunkt Intervall 30 Sekunden. Erfasste nach dem 5 aufeinander folgenden live Datenverkehr Fehlern oder ein einzelnes Prüfpunkt Fehler im Leerlauf Modus. Unterstützt User defined untersucht | 
| SSL-Verschiebung | Nicht unterstützt | Unterstützt | 
  