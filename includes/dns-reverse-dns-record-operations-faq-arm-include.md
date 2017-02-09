<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Häufig gestellte Fragen - Reverse DNS für Ihre Azure zugewiesene IP-Adresse

### <a name="how-much-do-reverse-dns-records-cost"></a>Wie viel führen Sie DNS-Einträge Kosten umkehren?
Sie sind kostenlos!  Es ist keine zusätzliche Kosten für reverse-DNS-Einträge oder Abfragen.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Werden die reverse DNS-Einträge für meine Azure zugewiesene öffentliche IP-Adresse aus dem Internet beheben?
Ja. Nach dem Festlegen der reverse-DNS-Eigenschaft für die IP-Adresse Ihrer öffentlichen verwaltet Azure an alle DNS-Stellvertretungen und DNS-Zonen erforderlich, um sicherzustellen, dass reverse-DNS-Eintrag für alle Benutzer der Internet aufgelöst.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Wird ein standardmäßige reverse DNS-Eintrag für meine öffentliche IP-Adressen werden erstellt?
Nein. Reverse-DNS-ist ein Feature Anmeldung. Wenn Sie nicht so konfigurieren Sie diese auswählen, werden keine standardmäßigen reverse DNS-Einträge erstellt.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Was ist das Format für den vollqualifizierten Domänennamen (FULLY)?
Fully werden in der Reihenfolge angegeben, und müssen abgeschlossen sein, durch einen Punkt (z. B. "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Was passiert, wenn ein Fehler auftreten, die Überprüfung wird für die reverse-DNS-ich angegeben haben?
Fehl, in dem die Überprüfung für reverse-DNS-überprüft, der Dienst Verwaltungsvorgang schlägt fehl. Korrigieren Sie den reverse DNS-Wert nach Bedarf, und versuchen Sie erneut.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kann ich für Meine Website Azure reverse-DNS-Einträge verwalten?
Reverse-DNS-wird Azure Websites nicht unterstützt. Reverse-DNS-wird für Azure virtuellen Computern unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Kann ich für meine öffentliche IP-Adresse mehrere reverse-DNS-Einträge konfigurieren?
Nein. Azure unterstützt ein einzelnes reverse DNS-Eintrags für jede öffentliche IP-Adresse ein. Jede öffentliche IP-Adresse kann jedoch eigene reverse-DNS-Eintrag verfügen.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Kann ich reverse-DNS-Einträge für eine öffentliche IP-Adresse von IPv6 konfigurieren?
Nein.  Zu diesem Zeitpunkt werden reverse-DNS-Einträge nur für IPv4 öffentliche IP-Adressen unterstützt.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Kann ich einen reverse-DNS-Eintrag für meine öffentliche IP-Adresse konfigurieren, ohne dass eine DomainNameLabel angegeben?
Nein. Um reverse-DNS-Einträge für Ihre öffentliche IP-Adressen nutzen zu können, müssen Sie die Eigenschaft DomainNameLabel angeben.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich e-Mails an externen Domänen aus Dienste Azure berechnen senden?
Nein. [Berechnen von Azure Services beim Senden von e-Mails an externe Domänen nicht unterstützt](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
