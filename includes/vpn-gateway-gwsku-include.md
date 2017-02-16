Beim Erstellen eines Gateways virtuelles Netzwerk müssen Sie das Gateway SKU angeben, die Sie verwenden möchten. Wenn Sie einen höheren Gateway SKU auswählen, weitere CPUs und Netzwerk-Bandbreite mit dem Gateway zugeordnet sind und das Gateway kann daher höhere Netzwerkdurchsatz an das virtuelle Netzwerk unterstützen.

VPN-Gateway können die folgenden SKUs verwenden:

- Grundlegende
- Standard
- HighPerformance

Wenn Sie eine SKU ausgewählt haben, beachten Sie Folgendes:

- Wenn Sie ein anderes PolicyBased VPN verwenden möchten, müssen Sie die grundlegenden SKU verwenden. Richtlinien VPN (vormals statisches Routing bezeichnet) werden auf alle anderen SKU nicht unterstützt.
- BGP wird auf dem grundlegende SKU nicht unterstützt.
- ExpressRoute VPN-Gateway parallel Konfigurationen werden nicht unterstützt, klicken Sie auf die grundlegende SKU.
- Klicken Sie auf die HighPerformance-SKU können aktive S2S VPN-Gateway Verbindungen konfiguriert werden.
