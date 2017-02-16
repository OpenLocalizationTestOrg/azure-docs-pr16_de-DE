Die folgende Tabelle enthält die Anforderungen für Richtlinien und RouteBased VPN Gateways. Diese Tabelle gilt für die Ressourcenmanager und klassischen Bereitstellungsmodelle. Für die Option Klassisch PolicyBased VPN Gateways stimmen mit statischen Gateways und Routing-basierten Gateways stimmen mit den dynamischen Gateways.


|   | **Richtlinien Basic VPN-Gateway** | **RouteBased Basic VPN-Gateway** | **RouteBased Standard VPN-Gateway**   | **RouteBased High Performance VPN-Gateway** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Website-zu-Standort Connectivity (S2S)**  | PolicyBased VPN-Konfiguration        | RouteBased VPN-Konfiguration  | RouteBased VPN-Konfiguration     | RouteBased VPN-Konfiguration    |
| **Punkt-zu-Standort Connectivity (P2S**)      | Nicht unterstützt   | Unterstützt (können parallel mit S2S)  | Unterstützt (können parallel mit S2S)  | Unterstützt (können parallel mit S2S) |
| **Authentifizierungsmethode**                 |    Vorinstallierten Schlüssel  | Vorinstallierten Schlüssel für Zertifikate für P2S-Konnektivität S2S-Konnektivität | Vorinstallierten Schlüssel für Zertifikate für P2S-Konnektivität S2S-Konnektivität | Vorinstallierten Schlüssel für Zertifikate für P2S-Konnektivität S2S-Konnektivität |
| **Maximale Anzahl von Verbindungen S2S**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maximale Anzahl von Verbindungen P2S**       | Nicht unterstützt                  | 128                                                                   | 128                               | 128                              |
|**Aktive Weiterleitung Support (BGP)**           | Nicht unterstützt                  | Nicht unterstützt                                                         | Unterstützt                     | Unterstützt                   |
 
