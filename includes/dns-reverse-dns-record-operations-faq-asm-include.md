<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Häufig gestellte Fragen - Reverse DNS für Ihre Azure zugewiesene IP-Adresse

### <a name="how-much-do-reverse-dns-records-cost"></a>Wie viel führen Sie DNS-Einträge Kosten umkehren?
Sie sind kostenlos!  Es ist keine zusätzliche Kosten für reverse-DNS-Einträge oder Abfragen.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Werden meine reverse-DNS-Einträge aus dem Internet beheben?
Ja. Nachdem Sie die reverse-DNS-Eigenschaft für Ihren Cloud-Dienst festlegen, verwaltet Azure an alle DNS-Stellvertretungen und DNS-Zonen erforderlich, um sicherzustellen, dass reverse-DNS-Eintrag für alle Benutzer der Internet aufgelöst.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Wird ein standardmäßige reverse DNS-Eintrag für meine Cloud-Dienste werden erstellt?
Nein. Reverse-DNS-ist ein Feature Anmeldung. Wenn Sie nicht so konfigurieren Sie diese auswählen, werden keine standardmäßigen reverse DNS-Einträge erstellt.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Was ist das Format für den vollqualifizierten Domänennamen (FULLY)?
Fully werden in der Reihenfolge angegeben, und müssen abgeschlossen sein, durch einen Punkt (z. B. "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Was passiert, wenn ein Fehler auftreten, die Überprüfung wird für die reverse-DNS-ich angegeben haben?
Fehl, in dem die Überprüfung für reverse-DNS-überprüft, der Dienst Verwaltungsvorgang schlägt fehl. Korrigieren Sie den reverse DNS-Wert nach Bedarf, und versuchen Sie erneut.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kann ich für Meine Website Azure reverse-DNS-Einträge verwalten?
Reverse-DNS-wird Azure Websites nicht unterstützt. Reverse-DNS-wird Azure PaaS Rollen und IaaS virtuellen Computern unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Kann ich für meine Cloud-Dienst mehrere reverse-DNS-Einträge konfigurieren?
Nein. Azure unterstützt ein einzelnes reverse DNS-Eintrags für jede Azure-Cloud-Dienst an. Jede Azure-Cloud-Dienst kann jedoch eigene reverse-DNS-Eintrag verfügen.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich e-Mails an externen Domänen aus Dienste Azure berechnen senden?
Nein. [Berechnen von Azure Services beim Senden von e-Mails an externe Domänen nicht unterstützt](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
