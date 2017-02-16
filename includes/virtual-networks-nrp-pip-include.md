## <a name="public-ip-address"></a>Öffentliche IP-Adresse
Eine öffentliche IP-Adressenressource bietet entweder eine reservierte oder dynamische Internet, die gegenüberliegende IP-Adresse. Obwohl Sie eine öffentliche IP-Adresse als eigenständige Anwendung Objekt erstellen können, müssen Sie ihn auf ein anderes Objekt, um die Adresse tatsächlich verwenden zuzuordnen. Sie können eine öffentliche IP-Adresse ein Lastenausgleich, Anwendungsgateway oder einen anderen Netzwerkadapter Internet Zugriff auf diese Ressourcen zuordnen.  

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**publicIPAllocationMethod**|Legt fest, ob die IP-Adresse *statisch* oder *dynamisch*ist.|statisch, dynamische|
|**idleTimeoutInMinutes**|Definiert die im Leerlauf Timeout mit einem Standardwert von 4 Minuten. Wenn keine weiteren Pakete für eine bestimmte Sitzung innerhalb dieses Zeitraums empfangen wird, wird die Sitzung beendet.|Jeder Wert zwischen 4 und 30|
|**IP-Adresse**|IP-Adresse zu einem Objekt zugewiesen ist. Dies ist eine schreibgeschützte Eigenschaft.|104.42.233.77|

### <a name="dns-settings"></a>DNS-Einstellungen
Öffentliche IP-Adressen enthalten ein untergeordnetes Objekt mit dem Namen **DnsSettings** , enthält die folgenden Eigenschaften:

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**domainNameLabel**|Host benannte für die namensauflösung verwendet.|Www, ftp, vm1|
|**FQDN**|Vollqualifizierter Name für die öffentliche IP-Adresse.|www.westus.cloudapp.Azure.com|
|**reverseFqdn**|Vollqualifizierten Domänennamen ein, die aufgelöst, die IP-Adresse und im DNS als PTR-Eintrag registriert ist.|www.contoso.com.|

Beispiel für öffentliche IP-Adresse im JSON-Format:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>Zusätzliche Ressourcen

- Erhalten Sie weitere Informationen zu [öffentlichen IP-Adressen](../articles/virtual-network/virtual-networks-reserved-public-ip.md)ein.
- Informationen Sie zu [Instanz Ebene öffentliche IP-Adressen](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
- Lesen Sie die [REST-API Dokumentation zur](https://msdn.microsoft.com/library/azure/mt163638.aspx) öffentliche IP-Adressen ein.