## <a name="scenario"></a>Szenario

Um zum Erstellen NSGs besser zu veranschaulichen, wird dieses Dokument folgenden Szenario verwenden.

![VNet Szenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In diesem Szenario erstellen Sie eine NSG für jedes Subnetz im **TestVNet** virtuelle Netzwerk, wie unten beschrieben: 

- **NSG-Front-End**. Der front-End-NSG mit dem *Front-End* -Subnetz angewendet werden, und zwei Regeln enthalten:  
    - **Rdp-Regel**. Mit dieser Regel wird RDP Datenverkehr der *Front-End* -Subnetz zulassen.
    - **Web-Regel**. Mit dieser Regel wird HTTP-Verkehr auf dem *Front-End* -Subnetz zulassen.
- **NSG-Back-End**. Die Back-End-NSG mit dem *Back-End-* Subnetz angewendet werden, und zwei Regeln enthalten: 
    - **Sql-Regel**. Diese Regel ermöglicht SQL-Datenverkehr nur aus dem *Front-End* -Subnetz an.
    - **Web-Regel**. Mit dieser Regel wird, dass alle Internet Datenverkehr aus dem Subnetz *Back-End-* gebunden verweigert.

Die Kombination dieser Regeln erstellen ein DMZ-ähnliche Szenario, wobei das Back-End-Subnetz eingehenden Datenverkehr kann nur für SQL von der front-End-Subnetz empfangen und verfügt über keinen Zugriff auf das Internet, während das front-End-Subnetz mit dem Internet kommunizieren kann, und erhalten nur eingehende HTTP-Anfragen.
 
