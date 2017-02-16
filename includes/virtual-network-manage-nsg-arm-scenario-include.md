## <a name="sample-scenario"></a>Beispielszenario

Zum Verwalten von NSGs besser zu veranschaulichen, wird in diesem Artikel folgenden Szenario verwendet.

![VNet Szenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In diesem Szenario erstellen Sie eine NSG für jedes Subnetz im **TestVNet** virtuelle Netzwerk, wie unten beschrieben: 

- **NSG-Front-End**. Der front-End-NSG mit dem *Front-End* -Subnetz angewendet werden, und zwei Regeln enthalten:  
    - **Rdp-Regel**. Mit dieser Regel wird RDP Datenverkehr der *Front-End* -Subnetz zulassen.
    - **Web-Regel**. Mit dieser Regel wird HTTP-Verkehr auf dem *Front-End* -Subnetz zulassen.
- **NSG-Back-End**. Die Back-End-NSG mit dem *Back-End-* Subnetz angewendet werden, und zwei Regeln enthalten: 
    - **Sql-Regel**. Diese Regel ermöglicht SQL-Datenverkehr nur aus dem *Front-End* -Subnetz an.
    - **Web-Regel**. Mit dieser Regel wird, dass alle Internet Datenverkehr aus dem Subnetz *Back-End-* gebunden verweigert.

Die Kombination dieser Regeln erstellen ein DMZ-ähnliche Szenario, wobei das Back-End-Subnetz eingehenden Datenverkehr für SQL-Datenverkehr kann nur von der front-End-Subnetz empfangen und verfügt über keinen Zugriff auf das Internet, während das front-End-Subnetz mit dem Internet kommunizieren kann, und erhalten nur eingehende HTTP-Anfragen.

Um die oben beschriebenen Szenario bereitzustellen, führen Sie [diesen Link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klicken Sie auf **Bereitstellen in Azure**, ersetzen Sie den Parameter Standardwerte bei Bedarf und folgen Sie den Anweisungen im Portal. In Beispiel aufgeführten Schritte ausführen wurde die Vorlage verwendet, um eine Gruppe Ressourcennamen **RG-NSG**bereitzustellen. 
 