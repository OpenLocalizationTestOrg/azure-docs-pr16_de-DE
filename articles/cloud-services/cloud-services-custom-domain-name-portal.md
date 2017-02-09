<properties
    pageTitle="Konfigurieren Sie einen benutzerdefinierten Domänennamen in der Cloud Services | Microsoft Azure"
    description="Erfahren Sie, wie Sie Ihre Azure-Anwendung oder Daten mit dem Internet in einer benutzerdefinierten Domäne verfügbar machen, indem Sie die DNS-Einstellungen konfigurieren.  In diesen Beispielen wird das Azure-Portal."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurieren einen benutzerdefinierten Domänennamen für einen Azure-Cloud-Dienst

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-custom-domain-name-portal.md)
- [Azure klassischen-portal](cloud-services-custom-domain-name.md)

Beim Erstellen einer Cloud-Dienst von Azure an Unterdomäne des **cloudapp.net**weist. Beispielsweise, wenn Ihre Cloud-Dienst "Contoso" heißt, werden Ihre Benutzer auf Ihrer Anwendung auf eine URL wie http://contoso.cloudapp.net zugreifen. Azure weist auch eine virtuelle IP-Adresse.

Sie können jedoch auch Ihrer Anwendung auf Ihren eigenen Domänennamen, wie etwa **"contoso.com"**verfügbar machen. In diesem Artikel wird erläutert, wie reservieren oder einen benutzerdefinierten Domänennamen für Cloud-Dienst Webrollen konfigurieren.

Führen Sie bereits Undestand welche CNAME- und A-Einträge sind? [Sprung ältere der Erklärung](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Die Verfahren in dieser Aufgabe gelten für Azure Cloud Services. App-Dienste finden Sie unter [diese](../app-service-web/web-sites-custom-domain-name.md). Für Speicherkonten finden Sie unter [diese](../storage/storage-custom-domain-name.md).

<p/>

> [AZURE.TIP]
> Jetzt geht's los schneller – verwenden der neuen Azure [schrittweise Anleitung](http://support.microsoft.com/kb/2990804)!  Zuordnen eines benutzerdefinierten Domänennamens und zum Schutz der Kommunikation (SSL) ist mit Azure Cloud Services oder Azure Websites ein Kinderspiel.

## <a name="understand-cname-and-a-records"></a>Grundlegendes zu CNAME und A-Einträge

CNAME (oder Alias Datensätze) und eine Datensätzen ermöglichen es Ihnen, einen Domänennamen mit einem bestimmten Server zuordnen (oder in diesem Fall service) jedoch anders funktionieren. Es gibt auch einige bestimmte Aspekte bei Verwendung von A-Einträge mit Azure-Cloud-Diensten, die Sie berücksichtigen sollten, bevor Sie sich entscheiden, welche verwenden.

### <a name="cname-or-alias-record"></a>CNAME oder Alias-Eintrag

Ein CNAME-Eintrag weist eine *bestimmte* Domäne, beispielsweise **"contoso.com"** oder **www.contoso.com**, eine kanonische Domänennamen. In diesem Fall der kanonische Domänenname ist die **[Anwendung] .cloudapp .net** Domänennamen von Ihrer Azure gehostet Anwendung. Nach dem Erstellen, wird der CNAME-Eintrag erstellt einen Alias für die **[Anwendung] .cloudapp .net**. Der CNAME-Eintrag wird die IP-Adresse des lösen Ihre **[Anwendung] .cloudapp .net** service automatisch, wenn die IP-Adresse des Cloud-Dienst verwandelt hat, haben Sie keine Aktionen durchführen.

> [AZURE.NOTE]
> Einige domänenregistrierungsstellen können Sie nur Unterdomänen zuordnen, wenn Sie einen CNAME-Eintrag, beispielsweise www.contoso.com und nicht Stammverzeichnis Namen, wie etwa "contoso.com" verwenden. Weitere Informationen zu den CNAME-Einträge finden Sie unter der Dokumentation Ihrer Registrierungsstelle, [den Eintrag Wikipedia auf CNAME-Eintrag](http://en.wikipedia.org/wiki/CNAME_record)oder das Dokument [IETF Domain Names - Implementierung und Spezifikation](http://tools.ietf.org/html/rfc1035) .

### <a name="a-record"></a>Einen Datensatz

Ein *A* -Datensatzes eine Domäne, beispielsweise **"contoso.com"** oder **www.contoso.com**, *oder eine Domäne Platzhalterzeichen* wie maps ** \*. "contoso.com"**, um eine IP-Adresse. Wenn eine Azure-Cloud-Dienst, der virtuelle IP-Adresse des Diensts. Daher ist der wichtigste Vorteil der eines A-Datensatzes über einen CNAME-Eintrag, dass Sie einen Eintrag enthalten können, die einen Platzhalter, wie verwendet \* **. "contoso.com"**, würde die Anfragen für mehrere Unterdomänen wie **mail.contoso.com**, **login.contoso.com**oder **www.contso.com**behandelt.

> [AZURE.NOTE]
> Da eine statische IP-Adresse ein A-Datensatzes zugeordnet ist, kann es automatisch Änderungen in der Cloud-Dienst die IP-Adresse nicht auflösen. Die IP-Adresse von Ihrem Cloud-Dienst verwendet wird beim ersten reserviert, wenn Sie auf einen leeren Slot (Herstellung oder Staging.) bereitstellen. Wenn Sie die Bereitstellung für den Slot löschen, wird die IP-Adresse von Azure freigegeben, und alle zukünftigen Bereitstellungen zu den Slot möglicherweise eine neue IP-Adresse angegeben werden.
>
> Einfache, ist die IP-Adresse ein Slot gegebene Bereitstellung (Herstellung oder Staging) beibehalten, wenn zwischen Staging und Herstellung Bereitstellungen oder einen Compliance-Upgrade von einer vorhandenen Bereitstellung austauschen. Weitere Informationen dazu, wie Sie diese Aktionen durchführen finden Sie unter [So Cloud-Dienste verwalten](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Fügen Sie einen CNAME-Eintrag für Ihre benutzerdefinierte Domäne hinzu.

Um einen CNAME-Eintrag zu erstellen, müssen Sie einen neuen Eintrag in der Tabelle DNS für Ihre benutzerdefinierte Domäne hinzufügen mithilfe der Tools zur Verfügung gestellt, indem Sie die Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche, aber weicht Methode einen CNAME-Eintrag festlegen, aber die Konzepte sind gleich.

1. Verwenden Sie eine der folgenden Methoden zum Suchen der **. cloudapp.net** Domänennamen an den Clouddienst zugewiesen.

    * Melden Sie sich bei der [Azure-Portal], wählen Sie Ihre Cloud-Dienst, prüfen Sie im Abschnitt **Essentials** , und suchen Sie dann den Eintrag **URL der Website** an.

        ![den ersten Blick Abschnitt mit den Website-URL][csurl]
            
        **ODER**
  
    * Installieren und Konfigurieren von [Azure Powershell](../powershell-install-configure.md), und verwenden Sie den folgenden Befehl aus:

        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Speichern Sie den Namen der Domäne, in der beiden Methoden zurückgegebene URL verwendet werden, da Sie sie benötigen, wenn Sie einen CNAME-Eintrag zu erstellen.

1.  Melden Sie sich bei Ihrer DNS-Registrierungsstelle Website, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit **Domänennamen**, **DNS**oder **Name Server Management**bezeichnet.

2.  Suchen Sie jetzt, wählen Sie oder geben Sie den CNAME. Möglicherweise müssen Sie den Datensatztyp aus einer Dropdownliste auswählen, nach unten, oder wechseln Sie zu einer Seite Erweiterte Einstellungen. Sie sollten die Wörter **CNAME**, **Alias**oder **Unterdomänen**Suchen nach.

3.  Sie müssen außerdem den Domäne oder Unterdomäne Alias für den CNAME-Eintrag, beispielsweise **"www"** bereitstellen, wenn Sie einen Alias für **www.customdomain.com**erstellen möchten. Wenn Sie einen Alias für die Stammdomäne erstellen möchten, kann es als geführt die '**@**' Symbol in der sich die Registrierungsstelle DNS-Tools.

4. Klicken Sie dann müssen Sie einen kanonische Hostnamen Bereitstellen Ihrer Anwendung **cloudapp.net** Domäne in diesem Fall ist.

Angenommen, der folgende CNAME-Eintrag alle an weiterleitet Datenverkehr aus **www.contoso.com** **contoso.cloudapp.net**, den benutzerdefinierten Domänennamen der bereitgestellten Anwendung:

| Alias/Host Name/Unterdomäne | Kanonische Domäne     |
| ------------------------- | -------------------- |
| "www"                       | Contoso.cloudapp.NET |

> [AZURE.NOTE]
Besucher ein **www.contoso.com** werden nie finden Sie unter der WAHR Host (contoso.cloudapp.net), damit der Prozess zum Weiterleiten an den Endbenutzer nicht sichtbar ist.

> Im Beispiel oben gilt nur für den Datenverkehr in die Unterdomäne **"www"** . Da Sie Platzhalter mit CNAME-Einträge verwenden können, müssen Sie einen CNAME für jede Domäne-Unterdomäne erstellen. Gewünschte Datenverkehr von Unterdomänen, wie direkte *. contoso.com an Ihre Adresse cloudapp.net können Sie Konfigurieren einer * *URL umleiten* * oder * *URL weiterleiten** Eintrag in Ihrem DNS-Einstellungen, oder erstellen ein A-Datensatzes.


## <a name="add-an-a-record-for-your-custom-domain"></a>Hinzufügen eines A-Eintrags für Ihre benutzerdefinierte Domäne

Um einen A-Eintrag zu erstellen, müssen Sie zuerst die virtuelle IP-Adresse Ihre Cloud-Dienst finden. Klicken Sie dann fügen Sie einen neuen Eintrag in der Tabelle DNS für Ihre benutzerdefinierte Domäne mit den Tools von Ihrer Registrierungsstelle hinzu. Jeder Registrierungsstelle hat eine ähnliche aber weicht Methode zum Angeben eines A-Datensatzes, aber die Konzepte sind gleich.

1. Verwenden Sie eine der folgenden Methoden an, um die IP-Adresse Ihre Cloud-Dienst erhalten.

    * Login [Azure-Portal], wählen Sie aus der Cloud-Dienst, prüfen Sie im Abschnitt **Essentials** , und suchen Sie dann den Eintrag **öffentliche IP-Adressen** .

        ![den ersten Blick Abschnitt mit der VIP][vip]

        **ODER**

    * Installieren und Konfigurieren von [Azure Powershell](../powershell-install-configure.md), und verwenden Sie den folgenden Befehl aus:

        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Speichern Sie die IP-Adresse, wie Sie diesen benötigen, wenn Sie einen A-Eintrag zu erstellen.

1.  Melden Sie sich bei Ihrer DNS-Registrierungsstelle Website, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit **Domänennamen**, **DNS**oder **Name Server Management**bezeichnet.

2.  Finden Sie jetzt, wo Sie aktivieren oder eines Eintrags eingeben können. Möglicherweise müssen Sie den Datensatztyp aus einer Dropdownliste auswählen, nach unten, oder wechseln Sie zu einer Seite Erweiterte Einstellungen.

3. Wählen Sie aus, oder geben Sie die Domäne oder die Unterdomäne, die diesen A-Datensatz verwendet wird. Wählen Sie beispielsweise **"www"** Erstellen eines Alias für **www.customdomain.com**werden soll. Wenn Sie einen Platzhaltereintrag alle Unterdomänen erstellen möchten, geben Sie '__*__'. Dies umfasst alle Unterdomänen wie **mail.customdomain.com**, **login.customdomain.com**und **www.customdomain.com**.

    Wenn Sie einen A-Eintrag für die Stammdomäne erstellen möchten, kann es als geführt werden die '**@**' Symbol in der sich die Registrierungsstelle DNS-Tools.

4. Geben Sie die IP-Adresse Ihre Cloud-Dienst in das bereitgestellte Feld ein. Ordnet den Domäneneintrag in den A-Eintrag für die IP-Adresse der Cloudbereitstellung-Dienst verwendet werden.

Beispielsweise die folgende, die ein Eintrag gesamten Verkehr von **contoso.com** in **137.135.70.239**, die IP-Adresse der bereitgestellten Anwendung weiterleitet:

| Host Name/Unterdomäne | IP-Adresse     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |


In diesem Beispiel veranschaulicht das Erstellen eines A-Datensatzes für die Domäne aus. Wenn Sie einen Platzhalterzeichen-Eintrag, um alle Unterdomänen Deckblatt erstellen möchten, geben Sie '__*__' als die Unterdomäne.

>[AZURE.WARNING]
>IP-Adressen in Azure sind standardmäßig dynamisch. Wahrscheinlich möchten Sie werden eine [reservierte IP-Adresse](../virtual-network/virtual-networks-reserved-public-ip.md) verwenden, um sicherzustellen, dass Ihre IP-Adresse nicht geändert wird.

## <a name="next-steps"></a>Nächste Schritte

* [Zum Verwalten von Cloud-Diensten](cloud-services-how-to-manage.md)
* [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne](../cdn/cdn-map-content-to-custom-domain.md)
* [Allgemeine Konfiguration von Ihrem Cloud-Dienst](cloud-services-how-to-configure-portal.md).
* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy-portal.md).
* Konfigurieren von [Ssl-Zertifikate](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure-portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
 