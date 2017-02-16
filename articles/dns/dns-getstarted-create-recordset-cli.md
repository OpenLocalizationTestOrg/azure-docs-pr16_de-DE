<properties
   pageTitle="Erstellen einer Datensatzgruppe und Einträge für eine DNS-Zone mit CLI | Microsoft Azure"
   description="Informationen zum Erstellen von Hosteinträge für Azure DNS. Einrichten von Datensatz legt und Einträge mit CLI"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>Erstellen von DNS-Datensätze und Datensätze mithilfe von CLI

> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)


In diesem Artikel führt Sie durch die Vorgehensweise zum Erstellen von Datensätzen und Einträge Datensätze mithilfe von CLI. Nach dem Erstellen der DNS-Zone, müssen Sie die DNS-Einträge für Ihre Domäne hinzufügen. Dazu müssen Sie zuerst DNS-Einträge und Datensätze zu verstehen.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="create-a-record-set-and-record"></a>Erstellen Sie einen Datensatz festlegen und Datensatz

In diesem Abschnitt zeigen wir Ihnen zum Erstellen einer Datensatzgruppe und Datensätze. In diesem Beispiel erstellen Sie einen Eintrag festlegen, die den relative Name "Www" in der DNS-Zone "contoso.com" enthält. Der vollqualifizierten Namen der Datensätze ist "www.contoso.com". Der Datensatztyp ist "A", und die Time to live (TTL) ist 60 Sekunden. Nach Abschluss dieses Schritts, wird eine leere Datensatzgruppe erstellt haben.

Zum Erstellen eines Datensatzes festlegen in der Spitze der Zone (in diesem Fall "contoso.com"), verwenden Sie den Namen des Datensatzes "@", einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Messe.

### <a name="1-create-a-record-set"></a>1. Erstellen einer Datensatzgruppe

Verwenden Sie zum Erstellen der Anzahl von Datensätzen `azure network dns record-set create`. Geben Sie die Ressourcengruppe Zonennamen festlegen Datensatz relative Name, den Datensatztyp und den TTL-Wert. Wenn die `--ttl` Parameter nicht definiert ist, ist der Standardwert vier (in Sekunden). Nach Abschluss dieses Schritts, müssen Sie eine leere "Www" Datensatz festlegen.

*Syntax: Netzwerk-Dns-Eintrag-Set erstellen < Ressourcengruppe >< Dns-Zone-Name > <name> <type><ttl>*

    azure network dns record-set create myresourcegroup  contoso.com  www A  60

### <a name="2-add-records"></a>2 Datensätze hinzufügen

Wenn die neu erstellten "Www" Datensatzgruppe verwenden möchten, müssen Sie Datensätze hinzufügen. Hinzufügen von Einträgen, um Datensätze mithilfe von `azure network dns record-set add-record`.

Der Parameter für das Hinzufügen von Datensätzen zu einer variieren je nach Typ der Datensatzgruppe. Beispielsweise, wenn Sie einen Satz Datensatz vom Typ "A" verwenden, Sie werden nur Datensätze mit dem Parameter angeben können `-a <IPv4 address>`.

Sie können IPv4 hinzufügen *A* -Datensätze, die den Eintrag "Www" festlegen, indem Sie mit den folgenden Befehl aus:

*Syntax: Netzwerk DNS-Eintrag-Set hinzufügen-Eintrag < Ressourcengruppe >< Dns-Zone-Name >< Datensatz-SetName ><type>*

    azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46

## <a name="additional-record-type-examples"></a>Zusätzliche Datensatztyp Beispiele

So erstellen Sie einen Eintrag Festlegen der einzelnen Datensatztypen Sie in den folgenden Beispielen. Jeder Datensatzgruppe enthält einen einzelnen Datensatz.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Zum Verwalten der Anzahl von Datensätzen und Datensätzen finden Sie unter [Verwalten von DNS-Einträge und Datensatz legt mithilfe von CLI](dns-operations-recordsets-portal.md).

Weitere Informationen zu Azure DNS finden Sie unter der [Azure DNS (Übersicht)](dns-overview.md).
