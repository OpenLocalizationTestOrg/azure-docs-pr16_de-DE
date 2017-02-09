#### <a name="create-an-a-record-set-with-single-record"></a>Erstellen eines A-Datensatzes mit einer einzigen Datensatz festlegen

Verwenden Sie zum Erstellen einer `azure network dns record-set create`. Angeben die Ressourcengruppe Zonennamen festlegen Eintrag relative Name, Datensatztyp und Zeit zu live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Nach dem Erstellen von A aufzeichnen festlegen, fügen Sie die IPv4-Adresse auf den Datensatz mit festlegen `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Erstellen einer AAAA-Eintrag mit einem einzelnen Datensatz festlegen

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Erstellen Sie einen CNAME-Eintrag mit einem einzelnen Datensatz festlegen

CNAME-Einträge können Sie nur ein einzelnes Zeichenfolgenwert.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Erstellen Sie einen MX-Eintrag mit einem einzelnen Datensatz festlegen

In diesem Beispiel verwenden wir die aufzeichnen SetName "@" zum Erstellen von MX-Eintrag in der Zone Spitze (in diesem Fall "contoso.com"). Dies ist im Allgemeinen MX-Einträge.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Erstellen Sie ein NS-Eintrag mit einem einzelnen Datensatz festlegen

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Erstellen Sie einen PTR-Eintrag mit einem einzelnen Datensatz festlegen  
In diesem Fall ' Meine-Arpa-Zone.com' ARPA Zeitzone darstellt IP-Bereichs darstellt.  Jede PTR-Eintrag festlegen in dieser Zone entspricht eine IP-Adresse in diesem Bereich IP.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Erstellen Sie einen SRV-Eintrag mit einem einzelnen Datensatz festlegen

Wenn Sie einen SRV-Eintrag im Stammverzeichnis der Zone erstellen, können Sie "_service" und "_protocol" in den Namen des Datensatzes angeben. Es ist nicht erforderlich, um einzuschließen "@" in den Namen des Datensatzes.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Erstellen Sie einen TXT-Eintrag mit einer einzigen Datensatz festlegen

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
