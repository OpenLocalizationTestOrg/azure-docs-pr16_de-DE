## <a name="what-is-reverse-dns"></a>Was ist die reverse-DNS?

Herkömmlichen DNS-Einträge aktivieren eine Zuordnung aus einem DNS-Namen (beispielsweise "www.contoso.com") zu einer IP-Adresse (beispielsweise 64.4.6.100).  Reverse-DNS-ermöglicht die Übersetzung einer IP-Adresse (64.4.6.100) wieder in einen Namen (www.contoso.com).

Reverse-DNS-Einträge sind in vielen Situationen verwendet. Beispielsweise werden reverse-DNS-Einträge bei der Bekämpfung von e-Mail-Spam, indem Sie den Absender einer e-Mail-Nachricht bestätigen stark verwendet.  Der Mailserver empfangen abruft den reverse DNS-Eintrag der sendenden Server IP-Adresse, und überprüfen Sie, ob dieser Host berechtigt ist, aus der ursprünglichen Domäne senden. (Bitte beachten Sie jedoch die [Dienste Azure berechnen beim Senden von e-Mails an externe Domänen nicht unterstützt](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Wie reverse DNS funktioniert

Reverse-DNS-Einträge sind in spezielle DNS-Zonen, bekannt als 'ARPA' Zonen gehostet werden.  Diese Zonen bilden separate DNS-Hierarchie parallel mit im normalen Hierarchie, wie etwa "contoso.com" Domänen hosten.

Beispielsweise wird der DNS-Datensatz 'www.contoso.com' mithilfe von DNS-'A' Datensatz mit Namen "Www" in der Zone "contoso.com" implementiert.  Diesen A-Eintrag verweist auf die entsprechende IP-Adresse, in diesem Fall 64.4.6.100.  Umgekehrte Suche wird separat, implementiert mit einem 'PTR' Datensatz mit dem Namen '100' in der Zone '6.4.64.in-addr.arpa' (Beachten Sie, dass IP-Adressen im ARPA Zonen storniert werden.)  Diesen PTR-Eintrag, verweist Wenn sie ordnungsgemäß konfiguriert wurde, mit dem Namen "www.contoso.com".

Wenn eine Organisation eine IP-Adresse blockieren zugeordnet ist, reservieren sie auch das Recht, die entsprechende ARPA Zone an. Entspricht der IP-Adresse blockiert Azure untersuchten ARPA Zonen verwaltet werden, und von Microsoft verwaltet werden. Internetdienstanbieter kann die Zone ARPA für eigene IP-Adressen für Sie hosten, oder können Sie die Zone ARPA in einem DNS-Dienst Ihrer Wahl, wie z. B. Azure DNS hosten.

>[AZURE.NOTE] Weiterleiten von DNS-suchen und umgekehrte DNS-Suche werden in separaten, parallele DNS-Hierarchien implementiert. Die umgekehrte Suche für "www.contoso.com" ist **nicht** in der Zone "contoso.com" gehostet, lieber gehostet in der Zone ARPA für den entsprechenden IP-Adresse ein.

Weitere Informationen zum reverse-DNS-finden Sie unter [Umgekehrte DNS-Suche](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure reverse-DNS-Unterstützung

Azure unterstützt zwei getrennte Szenarien, die im Zusammenhang mit umgekehrte DNS:

1. Hosten, die ARPA Zone, Ihre IP-Adresse blockieren entspricht.
2. Zulassen, dass Sie den reverse DNS-Eintrag für die IP-Adresse zugewiesen an den Azure-Dienst konfigurieren.

Unterstützt die ehemalige, Azure DNS kann Ihre ARPA Zonen hosten und Verwalten der PTR-Einträge für jede umgekehrte DNS-Suche verwendet werden.  Die Vorgehensweise zum Erstellen der Zone ARPA, die Delegierung einrichten und Konfigurieren der PTR-Einträge entspricht der reguläre DNS-Zonen.  Die einzige Unterschiede sind, dass die Delegierung über Ihren Internetdienstanbieter statt Ihrer DNS-Registrierungsstelle konfiguriert werden muss, und nur der Datensatztyp PTR verwendet werden soll.

Letztere unterstützt, können mit Azure Konfigurieren der reverse Lookup für IP-Adressen zu Ihrem Dienst zugewiesen.  Reverse-Lookup wird als eine PTR-Eintrag in der entsprechenden ARPA Zone von Azure konfiguriert.  Diese ARPA Zonen, entspricht alle IP-Adressbereiche von Azure verwendet werden von Microsoft gehostet. **Der Rest der in diesem Artikel werden die dieses Szenario ausführlich beschrieben.**
