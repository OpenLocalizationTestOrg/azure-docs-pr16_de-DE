<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Häufig gestellte Fragen - Ihrer ARPA Zone in Azure DNS-Hostinganbieter

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Können ARPA Zonen für mein Internetdienstanbieter zugewiesene IP-Blöcke auf Azure DNS werden hosten?
Ja. Zonen ARPA für eigene IP-Adressbereiche in Azure DNS-Hostinganbieter ist vollständig unterstützt.

Einfach [in Azure DNS Zone erstellen](dns-getstarted-create-dnszone.md), und klicken Sie dann arbeiten mit Ihren Internetdienstanbieter [die Zone](dns-domain-delegation.md)delegieren.  Sie können die PTR-Einträge für jede reverse-Lookup klicken Sie dann auf die gleiche Weise wie andere Datensatztypen verwalten.

Sie können auch [mithilfe der Azure CLI eine vorhandenen reverse-Lookup-Zone importieren](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Was kostet Hostinganbieter Meine ARPA Zone Kosten?
Die Zone ARPA für Ihren Internetdienstanbieter zugewiesene IP-Hosting wird blockieren in Azure DNS zu [standard Azure DNS Sätzen](https://azure.microsoft.com/pricing/details/dns/)belastet.

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kann ich ARPA Zonen für IPv4 und IPv6-Adressen in Azure DNS hosten?
Ja.