## <a name="about-records"></a>Informationen zu Datensätzen

Jede DNS-Eintrag hat einen Namen und einen Typ. Einträge sind in verschiedenen Typen entsprechend den Daten organisiert darin enthaltenen. Der am häufigsten verwendete Typ ist ein "A"-Eintrag, der einen Namen zu einer IPv4-Adresse zugeordnet ist. Ein anderer ist ein "MX"-Eintrag, der einen Namen mit einer e-Mail-Server zugeordnet ist.

Azure DNS unterstützt alle allgemeine DNS-Eintragstypen, einschließlich A, AAAA, CNAME, MX, NS, PTR, SOA, SRV und TXT. Beachten Sie Folgendes:
- SOA Datensätze werden automatisch mit den einzelnen Zonen erstellt, sie können nicht einzeln erstellt werden.
- SPF-Einträge sollten mithilfe des Typs TXT-Eintrag erstellt werden. Weitere Informationen finden Sie auf [dieser Seite](http://tools.ietf.org/html/rfc7208#section-3.1).

In Azure DNS werden Datensätze mit relativen Namen angegeben. "Vollqualifizierte" Domänennamen (FULLY) enthält den Zonennamen, während "relativer" Name nicht der Fall ist. Beispielsweise bietet relativ Datensatz namens "Www" in der Zone "contoso.com" des www.contoso.com vollqualifizierten aufzeichnen.

## <a name="about-record-sets"></a>Informationen zu Datensätze

Manchmal müssen Sie mehrere DNS-Eintrag mit einem angegebenen Namen und Typ zu erstellen. Nehmen Sie beispielsweise an, dass die Website "www.contoso.com" auf zwei verschiedene IP-Adressen gehostet wird. Die Website erfordert zwei verschiedenen A-Einträge, eine für jede IP-Adresse. Dies ist ein Beispiel für einen Datensatz festlegen:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS verwaltet die DNS-Einträge mithilfe von Recordsets. Ein Datensatz ist die Sammlung von DNS-Einträgen in einer Zone, die denselben Namen haben und die gleichen Typ. Die meisten Datensätze enthalten einen einzelnen Datensatz, wie dieser, in denen einer mehr als ein Datensatz enthält Beispiele sind jedoch nicht selten.

SOA und CNAME-Datensätze werden Ausnahmen. Die DNS-Standards ermöglichen nicht mehrere Datensätze mit demselben Namen für diese Dateitypen.

Die Gültigkeitsdauer oder die Gültigkeitsdauer (TTL) gibt an, wie lange jeden Datensatz von Clients zwischengespeichert werden, bevor Sie erneut abgefragt wird. In diesem Beispiel wird die Gültigkeitsdauer 3.600 Sekunden oder 1 Stunde. Die Gültigkeitsdauer ist für den Datensatz ein, nicht für jeden Datensatz angegeben, daher der gleiche Wert für alle Datensätze innerhalb dieser Anzahl von Datensätzen verwendet wird.

#### <a name="wildcard-record-sets"></a>Platzhalterzeichen Datensatz Datensätze

Azure DNS unterstützt [Platzhalterzeichen Datensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Diese werden für jede Abfrage, die mit einem übereinstimmenden Namen zurückgegeben (es sei denn, eine näher Übereinstimmung in einem anderen als Datensatz Satz). Für alle Datensatztypen außer NS und SOA werden Platzhalterzeichen Datensatz-Gruppen unterstützt.  

Verwenden Sie zum Erstellen einer Datensatzgruppe Platzhalterzeichen der Datensatz setName "\*". Oder verwenden Sie einen Namen mit der Bezeichnung "\*", beispielsweise"\*.foo".

#### <a name="cname-record-sets"></a>CNAME-Datensätze

CNAME-Datensätze können nicht mit anderen Datensatz mit demselben Namen verwendet werden. Sie können keine beispielsweise einen CNAME-Eintrag mit dem relativ Namen "Www" festlegen und eines A-Datensatzes mit relativ namens "Www" zur gleichen Zeit erstellen. Da der Zone Spitze (Name = ‘@’) enthält immer die NS und SOA aufzeichnen Mengen, die erstellt wurden, wenn die Zone erstellt wurde, Sie können nicht in der Zone Spitze festlegen einen CNAME-Eintrag erstellen. Dieser Einschränkungen entstehen durch den DNS-Standards und Schwächen Azure DNS nicht zur Verfügung.
