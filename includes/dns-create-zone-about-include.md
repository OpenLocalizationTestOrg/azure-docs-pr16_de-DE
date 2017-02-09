Eine DNS-Zone wird verwendet, um die DNS-Einträge für eine bestimmte Domäne gehostet wird. Um Ihre Domäne Hostinganbieter zu starten, müssen Sie zum Erstellen einer DNS Zone. Alle DNS-Einträge für eine bestimmte Domäne erstellt werden innerhalb einer Zone DNS-Einträge für die Domäne. 

Beispielsweise kann die Domäne "contoso.com" eine Reihe von DNS-Einträgen, wie etwa "mail.contoso.com" (für eine e-Mail-Server) und "www.contoso.com" (für eine Website) enthalten. 


## <a name="a-namenamesaabout-dns-zone-names"></a><a name="names"></a>Informationen zu DNS Zonennamen
 
- Der Name der Zone muss innerhalb der Ressourcengruppe eindeutig sein, und die Zone darf nicht bereits vorhanden sein. Andernfalls wird der Vorgang fehl.

- Zone gleichnamigen kann wieder in eine andere Ressourcengruppe oder ein anderes Azure-Abonnement verwendet werden. 

- In mehreren Zonen denselben Namen haben, wird jede Instanz anderen Namen Serveradressen zugewiesen werden, und nur eine Instanz von der übergeordneten Domäne delegiert werden kann. Weitere Informationen finden Sie unter [Stellvertretung einer Domäne zu Azure DNS](../articles/dns/dns-domain-delegation.md).