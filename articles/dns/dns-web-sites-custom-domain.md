<properties
   pageTitle="Erstellen von benutzerdefinierten DNS-Einträge für eine Web app | Microsoft Azure  "
   description="Informationen zum Erstellen von benutzerdefinierten Domäne DNS-Einträge für Web app mit Azure DNS."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Erstellen von DNS-Einträge für eine Web app in einer benutzerdefinierten Domäne

Azure DNS können Sie um eine benutzerdefinierte Domäne für Ihre Web apps zu hosten. Angenommen, Sie Erstellen einer Azure Web app und Sie möchten Ihre Benutzer darauf zugreifen, indem Sie entweder mit contoso.com oder www.contoso.com als FQDN.

Dazu müssen Sie zwei Datensätze zu erstellen:

- Ein "A"-Stammdatensatz auf "contoso.com" zeigen
- Ein Datensatz "CNAME" für den "www"-Namen, der auf den A-Eintrag verweist

Denken Sie daran, dass Sie einen A-Eintrag für eine Web-app in Azure erstellen, der A-Eintrag manuell sein muss aktualisiert werden, wenn die zugrunde liegenden IP-für das Web app Änderungen Adresse.

## <a name="before-you-begin"></a>Vorbemerkung

Bevor Sie beginnen, müssen Sie zuerst eine DNS-Zone in Azure DNS erstellen und delegieren die Zone in Ihrer Registrierungsstelle bei Azure DNS.

1. Führen Sie die Schritte in [Erstellen eines DNS-Zone](dns-getstarted-create-dnszone.md), zum Erstellen einer DNS-Zone.
2. Führen Sie die Schritte in der [DNS-Domäne Delegierung](dns-domain-delegation.md), zum Delegieren der DNS-Einträge bei Azure DNS.

Nach dem Erstellen einer Zone und deren Delegierung an Azure DNS-Einträge, können Sie dann Einträge für Ihre benutzerdefinierte Domäne erstellen.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Erstellen eines A-Datensatzes für Ihre benutzerdefinierte Domäne

Ein A-Eintrag wird verwendet, um die Zuordnung eines Namens zu seiner IP-Adresse. Im folgenden Beispiel wird zuweisen @ als eines A-Datensatzes zu einer IPv4-Adresse:

### <a name="step-1"></a>Schritt 1

Erstellen eines A-Datensatzes, und weisen Sie einer Variablen $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Schritt 2

Fügen Sie den Wert für die IPv4, zu die zuvor erstellte Datensatzgruppe "@" mithilfe der $rs Variable zugewiesen. Zugewiesene IPv4-Wert wird die IP-Adresse für Ihre Web-app sein.

Folgen Sie den Schritten [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure-App-Dienst](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address), um die IP-Adresse für eine Web app finden Sie.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Schritt 3

Die Änderungen in der Datensatzgruppe zu übernehmen. Verwenden Sie `Set-AzureRMDnsRecordSet` zum Hochladen von Änderungen an den festlegen bei Azure DNS-Eintrag:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. erstellen Sie einen CNAME-Eintrag für Ihre benutzerdefinierte Domäne

Wenn Ihre Domäne bereits von Azure DNS verwaltet wird (finden Sie unter [DNS-Domäne Delegierung](dns-domain-delegation.md), können Sie Folgendes verwenden das Beispiel in einen CNAME-Eintrag für contoso.azurewebsites.net erstellen.

### <a name="step-1"></a>Schritt 1

Öffnen Sie PowerShell und erstellen Sie einen neuen CNAME-Datensatz Satz und einer Variablen $rs zuweisen. In diesem Beispiel wird eine Datensatzgruppe CNAME mit "Zeit für eine live" von 600 Sekunden im DNS-Zone mit dem Namen "contoso.com" erstellen.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Schritt 2

Nachdem die CNAME-Datensatz Menge erstellt wurde, müssen Sie den Aliaswert einer beträchtlich erhöhen, zeigen Sie auf der Web-app wird.

Verwenden die zuvor zugewiesene Variable "$rs" können den folgenden PowerShell-Befehl Sie den Alias für das Web app contoso.azurewebsites.net erstellen.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Schritt 3

Übernehmen Sie die Änderungen, die mit der `Set-AzureRMDnsRecordSet` Cmdlet aus:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Sie können den Eintrag überprüfen ordnungsgemäß erstellt wurde, indem Sie Abfragen mithilfe von Nslookup, "www.contoso.com" aus, wie unten dargestellt:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Erstellen Sie einen "Awverify"-Eintrag für Web apps


Wenn Sie einen A-Eintrag für Ihre Web app verwenden möchten, müssen Sie wechseln durch eine Überprüfung, um sicherzustellen, dass Sie der Besitzer die benutzerdefinierte Domäne sind. Dieser Überprüfungsschritt erfolgt durch einen besonderen CNAME-Eintrag mit dem Namen "Awverify" erstellen. Dieser Abschnitt gilt für nur eine Datensätze.


### <a name="step-1"></a>Schritt 1

Erstellen Sie den Eintrag "Awverify" ein. Im folgenden Beispiel wird den Eintrag "Aweverify" für contoso.com Besitzrechte für die benutzerdefinierte Domäne überprüft erstellt.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Schritt 2

Nachdem die Datensatzgruppe "Awverify" erstellt wurde, weisen Sie den CNAME-Eintrag alias-set aus. Im folgenden Beispiel werden wir den CNAMe-Eintrag zu awverify.contoso.azurewebsites.net alias-set zuweisen.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Schritt 3

Übernehmen Sie die Änderungen, die mit den `Set-AzureRMDnsRecordSet cmdlet`, wie im folgenden Befehl dargestellt.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Nächste Schritte

Folgen Sie den Schritten zum Konfigurieren der Web-app, um eine benutzerdefinierte Domäne verwendet [einen benutzerdefinierten Domänennamen für App-Dienst konfigurieren](../app-service-web/web-sites-custom-domain-name.md) .








