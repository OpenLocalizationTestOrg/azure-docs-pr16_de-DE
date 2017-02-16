#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Konfigurieren einen benutzerdefinierten Domänennamen für eine Website zu Azure

Beim Erstellen einer Websites Azure bietet eine Anzeige Unterdomäne auf die Domäne azurewebsites.net, damit die Benutzer Ihrer Website mithilfe einer URL wie http:// zugreifen können&lt;MeineWebsite >. azurewebsites.net. Wenn Sie Ihren Websites für Sie freigegeben oder Standardmodus konfiguriert haben, können Sie Ihrer Website zu Ihren eigenen Domänennamen zuordnen.

Optional können Sie Azure Datenverkehr Manager um Saldo eingehenden Datenverkehr zu Ihrer Website zu laden. Weitere Informationen zur Funktionsweise von Datenverkehr Manager mit Websites finden Sie unter [Steuern Azure Websites Verkehr mit Azure Datenverkehr Manager][trafficmanager].

> [AZURE.NOTE] Die Verfahren in dieser Aufgabe gelten für Azure-Websites. Cloud Services finden Sie unter <a href="/develop/net/common-tasks/custom-dns/">einen benutzerdefinierten Domänennamen in Azure konfigurieren</a>.

> [AZURE.NOTE] Die Schritte in dieser Aufgabe ist es erforderlich, zum Konfigurieren Ihrer Websites für Sie freigegeben oder Standardmodus, der möglicherweise ändern, wie viel Sie für Ihr Abonnement in Rechnung gestellt werden. Weitere Informationen finden Sie unter <a href="/pricing/details/web-sites/">Websites Preise Details</a> .

Inhalt dieses Artikels:

-   [Grundlegendes zu CNAME und A-Einträge](#understanding-records)
-   [Konfigurieren von Websites für freigegebene oder Standardmodus](#bkmk_configsharedmode)
-   [Hinzufügen von Websites in den Datenverkehr-Manager](#trafficmanager)
-   [Einen CNAME für Ihre benutzerdefinierte Domäne hinzufügen](#bkmk_configurecname)
-   [Hinzufügen eines A-Eintrags für Ihre benutzerdefinierte Domäne](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Grundlegendes zu CNAME und A-Einträge</h2>

CNAME (oder Alias Datensätze) und einer Datensätzen ermöglichen es Ihnen, eine Website, einen Domänennamen zuordnen jedoch sie jede anders funktionieren.

###<a name="cname-or-alias-record"></a>CNAME oder Alias-Eintrag

Ein CNAME-Eintrag weist eine *bestimmte* Domäne, beispielsweise **"contoso.com"** oder **www.contoso.com**, eine kanonische Domänennamen. In diesem Fall der kanonische Domänenname ist die entweder die ** &lt;Anwendung >. azurewebsites.net** Domänennamen Ihrer Azure-Website oder der ** &lt;Anwendung >. trafficmgr.com** Domänennamen Ihres Profils Datenverkehr-Manager. Nach dem Erstellen, wird der CNAME-Eintrag erstellt einen Alias für die ** &lt;Anwendung >. azurewebsites.net** oder ** &lt;Anwendung >. trafficmgr.com** Domänennamen ein. Der CNAME-Eintrag wird die IP-Adresse des beheben Ihrer ** &lt;Anwendung >. azurewebsites.net** oder ** &lt;Anwendung >. trafficmgr.com** Domänennamens automatisch, wenn sich die IP-Adresse der Website ändert, haben Sie keinen ergreifen.

> [AZURE.NOTE] Einige domänenregistrierungsstellen können Sie nur Unterdomänen zuordnen, wenn Sie einen CNAME-Eintrag, beispielsweise www.contoso.com und nicht Stamm Namen, wie etwa "contoso.com" verwenden. Weitere Informationen zu den CNAME-Einträge finden Sie unter der Dokumentation Ihrer Registrierungsstelle, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia-Eintrag auf CNAME-Eintrag</a>oder den <a href="http://tools.ietf.org/html/rfc1035">Domänennamen IETF - Implementierung und Spezifikation</a> Dokument.

###<a name="a-record"></a>Einen Datensatz

Ein A-Datensatzes eine Domäne, beispielsweise **"contoso.com"** oder **www.contoso.com**, *oder eine Domäne Platzhalterzeichen* wie maps ** \*. "contoso.com"**, um eine IP-Adresse. Im Falle einer Azure-Website Adresse entweder die virtuelle IP-Adresse des Diensts oder eine bestimmte IP-, die Sie für Ihre Website erworben haben. Daher ist der wichtigste Vorteil der eines A-Datensatzes über einen CNAME-Eintrag, dass Sie einen Eintrag enthalten können, die einen Platzhalter, wie verwendet * **. "contoso.com"**, welche würde Anfragen für mehrere Unterdomänen wie behandelt * *mail.contoso.com**; * *login.contoso.com**, oder * *www.contso.com**.

> [AZURE.NOTE] Da eine statische IP-Adresse ein A-Datensatzes zugeordnet ist, kann es automatisch Änderungen an die IP-Adresse Ihrer Website nicht auflösen. Wenn Sie Einstellungen für den benutzerdefinierten Domänennamen für Ihre Website konfigurieren, wird eine IP-Adresse für die Verwendung mit einem Datensätze bereitgestellt. Dieser Wert möglicherweise jedoch ändern, wenn Sie löschen und neu zu Ihrer Website erstellen oder ändern Sie den Website-Modus, in den Hintergrund zum Freigeben.

> [AZURE.NOTE] A-Einträge können nicht für den Lastenausgleich mit den Datenverkehr Manager verwendet werden. Weitere Informationen finden Sie unter [Steuern Azure Websites Verkehr mit Azure Datenverkehr Manager][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Konfigurieren der Websites für freigegebene oder Standardmodus</h2>

Festlegen eines benutzerdefinierten Domänennamens auf einer Website ist nur für freigegebene und Standard Modi für Azure Websites verfügbar. Vor dem Wechsel von einer Website aus der kostenlosen Website auf die freigegeben oder Standard Website Modus, müssen Sie zuerst direkte Feststelltaste Ausgaben für Ihr Abonnement Website entfernen. Weitere Informationen zu freigegebenen und Standard Modus Preisen, finden Sie unter [Details Preise][PricingDetails].

1. Öffnen Sie in Ihrem Browser im [Verwaltungsportal][portal].
2. Klicken Sie auf den Namen der Website, auf der Registerkarte **Websites** .

    ![][standardmode1]

3. Klicken Sie auf die Registerkarte **ZEICHNUNGSMAßSTAB** .

    ![][standardmode2]


4. Legen Sie im Abschnitt **Allgemein** den Website-Modus durch Klicken auf **freigegeben**.

    ![][standardmode3]

    > [AZURE.NOTE] Wenn Sie den Datenverkehr Manager mit dieser Website verwenden, müssen Sie anstelle von freigegebenen wählen Standard-Modus verwenden.

5. Klicken Sie auf **Speichern**.
6. Wenn Sie dazu aufgefordert werden, zu der höheren Preis gemeinsamer Modus (oder Standardmodus, falls Standard gewünscht), klicken Sie auf **Ja** , wenn Sie einverstanden sind.

    <!--![][standardmode4]-->

    **Notiz**<br />
   Wenn Sie eine Fehlermeldung "Skalieren für Website 'Websitename' Fehler beim Konfigurieren von" erhalten, können Sie die Schaltfläche "Details", um weitere Informationen zu erhalten.

<a name="trafficmanager"></a><h2>(Optional) Hinzufügen der Websites in den Datenverkehr-Manager</h2>

Wenn Sie Ihre Website mit den Datenverkehr Manager verwenden möchten, führen Sie die folgenden Schritte aus.

1. Wenn Sie noch nicht über ein Profil Datenverkehr Manager verfügen, verwenden Sie die Informationen in [den Datenverkehr Manager Profil mit schnellen Erstellen erstellen] [ createprofile] zu erstellen. Hinweis Die **. trafficmgr.com** Domänennamen Ihres Profils Datenverkehr Manager zugeordnet. Dies wird in einem späteren Schritt verwendet werden.

2. Verwenden Sie die Informationen in [Hinzufügen oder Löschen von Endpunkten] [ addendpoint] Ihrer Website als einen Endpunkt in Ihrem Profil Datenverkehr Manager hinzufügen.

    > [AZURE.NOTE] Wenn Ihre Website beim Hinzufügen von außen liegenden Tabellenblättern nicht aufgeführt ist, stellen Sie sicher, dass sie für Standard-Modus konfiguriert ist. Sie müssen Standardmodus für Ihre Website verwenden, um mit den Datenverkehr Manager arbeiten.

3. Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit **Domänennamen**, **DNS**oder **Name Server Management**bezeichnet.

4. Suchen Sie jetzt, wo Sie aktivieren oder CNAME-Einträge eingeben können. Möglicherweise müssen Sie den Datensatztyp aus einer Dropdownliste auswählen, nach unten, oder wechseln Sie zu einer Seite Erweiterte Einstellungen. Sie sollten die Wörter **CNAME**, **Alias**oder **Unterdomänen**Suchen nach.

5. Sie müssen auch den Domäne oder Unterdomäne Alias für den CNAME-Eintrag angeben. Beispielsweise **"www"** Erstellen eines Alias für **www.customdomain.com**werden soll.

5. Sie müssen außerdem einen Hostnamen angeben, der die kanonische Domänennamen für diesen CNAME Alias ist. Dies ist der **. trafficmgr.com** Namen für Ihre Website.

Beispielsweise leitet der folgende CNAME-Eintrag gesamten Verkehr aus **www.contoso.com** zu **contoso.trafficmgr.com**, der Domänenname einer Website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Host Name/Unterdomäne</strong></td>
<td><strong>Kanonische Domäne</strong></td>
</tr>
<tr>
<td>"www"</td>
<td>Contoso.trafficmgr.com</td>
</tr>
</table>

Besucher ein **www.contoso.com** werden nie finden Sie unter der WAHR Host (contoso.azurewebsite.net), damit der Prozess zum Weiterleiten an den Endbenutzer nicht sichtbar ist.

> [AZURE.NOTE] Wenn Sie den Datenverkehr Manager mit einer Website verwenden, müssen Sie nicht die Schritte in den folgenden Abschnitten, '**Hinzufügen eines CNAME für Ihre benutzerdefinierte Domäne**' und '**Hinzufügen eines A-Datensatzes für Ihre benutzerdefinierte Domäne**'. Der CNAME-Eintrag in den vorherigen Schritten erstellt werden eingehenden Datenverkehr nach den Datenverkehr Manager leiten, klicken Sie dann den Datenverkehr an die Endpunkte Website weiterleitet.

<a name="bkmk_configurecname"></a><h2>Einen CNAME für Ihre benutzerdefinierte Domäne hinzufügen</h2>

Um einen CNAME-Eintrag zu erstellen, müssen Sie einen neuen Eintrag in der Tabelle DNS für Ihre benutzerdefinierte Domäne hinzufügen mithilfe von Tools zur Verfügung gestellt, indem Sie die Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche, aber weicht Methode einen CNAME-Eintrag festlegen, aber die Konzepte sind gleich.

1. Verwenden Sie eine der folgenden Methoden zum Suchen der **. azurewebsite.net** Domänennamen zu Ihrer Website zugewiesen.

    * Melden Sie sich im [Verwaltungsportal Azure][portal], wählen Sie Ihre Website aus, wählen Sie **Dashboard**und suchen Sie dann den Eintrag **URL-Website** im Abschnitt **den ersten Blick** .

    * Installieren und Konfigurieren von [Azure Powershell](/manage/install-and-configure-windows-powershell/), und verwenden Sie den folgenden Befehl aus:

            get-azurewebsite yoursitename | select hostnames

    * Installieren Sie und konfigurieren Sie der [Azure Command Line Interface](/manage/install-and-configure-cli/), und verwenden Sie den folgenden Befehl aus:

            azure site domain list yoursitename

    Speichern Sie diese **. azurewebsite.net** name auswählen, wie sie in den folgenden Schritten verwendet wird.

3. Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit **Domänennamen**, **DNS**oder **Name Server Management**bezeichnet.

4. Suchen Sie jetzt, wo Sie aktivieren oder CNAME-Einträge eingeben können. Möglicherweise müssen Sie den Datensatztyp aus einer Dropdownliste auswählen, nach unten, oder wechseln Sie zu einer Seite Erweiterte Einstellungen. Sie sollten die Wörter **CNAME**, **Alias**oder **Unterdomänen**Suchen nach.

5. Sie müssen auch den Domäne oder Unterdomäne Alias für den CNAME-Eintrag angeben. Beispielsweise **"www"** Erstellen eines Alias für **www.customdomain.com**werden soll. Wenn Sie einen Alias für die Stammdomäne erstellen möchten, kann es als geführt die '**@**' Symbol in der sich die Registrierungsstelle DNS-Tools.

5. Sie müssen außerdem einen Hostnamen angeben, der die kanonische Domänennamen für diesen CNAME Alias ist. Dies ist der **. azurewebsite.net** Namen für Ihre Website.

Beispielsweise leitet der folgende CNAME-Eintrag gesamten Verkehr aus **www.contoso.com** zu **contoso.azurewebsite.net**, der Domänenname einer Website:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Host Name/Unterdomäne</strong></td>
<td><strong>Kanonische Domäne</strong></td>
</tr>
<tr>
<td>"www"</td>
<td>Contoso.azurewebsite.NET</td>
</tr>
</table>

Besucher ein **www.contoso.com** werden nie finden Sie unter der WAHR Host (contoso.azurewebsite.net), damit der Prozess zum Weiterleiten an den Endbenutzer nicht sichtbar ist.

> [AZURE.NOTE] Im Beispiel oben gilt nur für den Datenverkehr in die Unterdomäne __"www"__ . Da Sie Platzhalter mit CNAME-Einträge verwenden können, müssen Sie einen CNAME für jede Domäne-Unterdomäne erstellen. Wenn Sie Datenverkehr von Unterdomänen, wie verbinden möchten *. contoso.com an Ihre Adresse azurewebsite.net können Sie einen Eintrag __URL umleiten__ vorwärts oder __Rückwärts URL__ in Ihre DNS-Einstellungen konfigurieren, oder erstellen ein A-Datensatzes.

> [AZURE.NOTE] Es dauert einige Zeit für Ihre CNAME im DNS-System übertragen. Sie können den CNAME-Eintrag für die Website keine festlegen, bis der CNAME-Eintrag verteilt wurde. Einen Dienst, wie z. B. <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der CNAME-Eintrag verfügbar ist.

###<a name="add-the-domain-name-to-your-website"></a>Fügen Sie den Namen der Domäne zu Ihrer website

Nachdem Sie der CNAME-Eintrag für den Domänennamen verteilt wurde, müssen Sie es mit Ihrer Website zuordnen. Sie können den benutzerdefinierten Domänennamen definiert durch den CNAME-Eintrag zu Ihrer Website mithilfe eines der Azure Line Interface (Azure CLI) oder mithilfe der Azure-Verwaltungsportal hinzufügen.

**Hinzufügen ein Domänennamens über die Befehlszeile tools**

Installieren Sie und konfigurieren Sie der [Azure Line Benutzeroberflächen](/manage/install-and-configure-cli/), und verwenden Sie den folgenden Befehl aus:

    azure site domain add customdomain yoursitename

Beispielsweise wird Folgendes einen benutzerdefinierten Domänennamen der **www.contoso.com** auf die Website **contoso.azurewebsite.net** hinzufügen:

    azure site domain add www.contoso.com contoso

Sie können bestätigen, dass der benutzerdefinierten Domänennamen zur Website hinzugefügt wurde, mit den folgenden Befehl aus:

    azure site domain list yoursitename

Die von diesem Befehl zurückgegebenen sollte zu erstellende Liste der benutzerdefinierten Domänennamen als auch die Standardeinstellung **. azurewebsite.net** Eintrag.

**Verwenden der Azure-Verwaltungsportal Domänennamen hinzufügen**

1. Öffnen Sie in Ihrem Browser das [Azure-Verwaltungsportal][portal].

2. Klicken Sie auf der Registerkarte **Websites** auf den Namen der Website, **Dashboard aus**, und wählen Sie dann aus den unteren Rand der Seite **Manage Domains** .

    ![][setcname2]

6. Geben Sie in das Textfeld **DOMÄNENNAMEN** den Namen der Domäne, den Sie konfiguriert haben.

    ![][setcname3]

6. Klicken Sie auf das Häkchen, um den Domänennamen zu akzeptieren.

Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Domänennamen** Seitenrand **Konfigurieren** Ihrer Website aufgeführt sein.

<a name="bkmk_configurearecord"></a><h2>Hinzufügen eines A-Eintrags für Ihre benutzerdefinierte Domäne</h2>

Um einen A-Eintrag zu erstellen, müssen Sie zuerst die IP-Adresse der Website suchen. Fügen Sie dann einen Eintrag in der Tabelle DNS für Ihre benutzerdefinierte Domäne mithilfe der Tools zur Verfügung gestellt, indem Sie die Registrierungsstelle. Jeder Registrierungsstelle hat eine ähnliche aber weicht Methode zum Angeben eines A-Datensatzes, aber die Konzepte sind gleich. Zusätzlich zum Erstellen eines A-Datensatzes, müssen Sie auch einen CNAME-Eintrag erstellen, den Azure wird verwendet, um den A-Eintrag zu überprüfen.

1. Öffnen Sie in Ihrem Browser das [Azure-Verwaltungsportal][portal].

2. Klicken Sie auf der Registerkarte **Websites** auf den Namen der Website, wählen Sie **Dashboard**, und wählen Sie dann aus den unteren Rand des Bildschirms **Manage Domains** .

    ![][setcname2]

5. Suchen nach **Der IP-Adresse zu verwenden, wenn eine Datensätze zu konfigurieren**, klicken Sie im Dialogfeld **benutzerdefinierte Domänen verwalten** . Kopieren Sie die IP-Adresse ein. Dies wird verwendet, wenn den A-Eintrag zu erstellen.

5. Beachten Sie Awverify Domänennamen am Ende des Texts am oberen Rand des Dialogfelds, klicken Sie im Dialogfeld **benutzerdefinierte Domänen verwalten** . Sie sollten **awverify.mysite.azurewebsites.net** darin **MeineWebsite** den Namen Ihrer Website. Kopieren Sie diese, aus, wie es ist, dass der Domänenname verwendet wird, wenn die Überprüfung CNAME-Eintrag zu erstellen.

6. Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit **Domänennamen**, **DNS**oder **Name Server Management**bezeichnet.

6. Suchen Sie in dem auswählen oder eingeben A und CNAME-Einträge. Möglicherweise müssen Sie den Datensatztyp aus einer Dropdownliste auswählen, nach unten, oder wechseln Sie zu einer Seite Erweiterte Einstellungen.

7. Führen Sie die folgenden Schritte aus, um den A-Eintrag zu erstellen:

    1. Wählen Sie aus, oder geben Sie die Domäne oder die Unterdomäne, die den A-Eintrag verwendet wird. Wählen Sie beispielsweise **"www"** Erstellen eines Alias für **www.customdomain.com**werden soll. Wenn Sie einen Platzhaltereintrag alle Unterdomänen erstellen möchten, geben Sie '__*__'. Dies umfasst alle Unterdomänen wie **mail.customdomain.com**, **login.customdomain.com**und **www.customdomain.com**.

        Wenn Sie einen A-Eintrag für die Stammdomäne erstellen möchten, kann es als geführt die '**@**' Symbol in der sich die Registrierungsstelle DNS-Tools.

    2. Geben Sie die IP-Adresse Ihre Cloud-Dienst in das bereitgestellte Feld ein. Ordnet den Domäneneintrag in den A-Eintrag für die IP-Adresse der Cloudbereitstellung-Dienst verwendet werden.

        Beispielsweise die folgende, die ein Eintrag gesamten Verkehr von **contoso.com** in **137.135.70.239**, die IP-Adresse von unserem bereitgestellte Anwendung weiterleitet:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Host Name/Unterdomäne</strong></td>
        <td><strong>IP-Adresse</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        In diesem Beispiel veranschaulicht das Erstellen eines A-Datensatzes für die Domäne aus. Wenn Sie einen Platzhalterzeichen-Eintrag, um alle Unterdomänen Deckblatt erstellen möchten, geben Sie '__*__' als die Unterdomäne.

7. Erstellen Sie anschließend einen CNAME-Eintrag, bei dem ein Alias von **Awverify**und eine kanonische Domäne der **awverify.mysite.azurewebsites.net** , die Sie zuvor für Ihren Kunden.

    > [AZURE.NOTE] Zwar ein Alias von Awverify für einige Registrierungsstelle verwenden kann, möglicherweise andere den vollständigen Alias Domänennamen Awverify.www.customdomainname.com oder awverify.customdomainname.com erforderlich.

    Beispielsweise erstellt die folgenden CNAME-Eintrag, mit denen Azure überprüfen die Konfiguration der A-Eintrag.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias/Host Name/Unterdomäne</strong></td>
    <td><strong>Kanonische Domäne</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Es dauert einige Zeit für die Awverify CNAME im DNS-System übertragen. Sie können keine Festlegen des benutzerdefinierten Domänennamens durch den A-Eintrag für die Website definiert werden, bis die Awverify CNAME verteilt wurde. Einen Dienst, wie z. B. <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der CNAME-Eintrag verfügbar ist.

###<a name="add-the-domain-name-to-your-website"></a>Fügen Sie den Namen der Domäne zu Ihrer website

Nachdem der **Awverify** CNAME-Eintrag für den Domänennamen verteilt wurde, können Sie dann die benutzerdefinierte Domäne definiert werden, indem Sie den A-Eintrag für Ihre Website zuordnen. Sie können den benutzerdefinierten Domänennamen definiert durch den A-Eintrag zu Ihrer Website mithilfe von CLI Azure oder mithilfe der Azure-Verwaltungsportal hinzufügen.

**Hinzufügen ein Domänennamens Schnittstelle der Azure Line (Azure CLI)**

Installieren Sie und konfigurieren Sie die [Azure CLI](/manage/install-and-configure-cli/), und verwenden Sie den folgenden Befehl aus:

    azure site domain add customdomain yoursitename

Beispielsweise wird Folgendes einen benutzerdefinierten Domänennamen **"contoso.com"** auf der Website **contoso.azurewebsite.net** hinzufügen:

    azure site domain add contoso.com contoso

Sie können bestätigen, dass der benutzerdefinierten Domänennamen zur Website hinzugefügt wurde, mit den folgenden Befehl aus:

    azure site domain list yoursitename

Die von diesem Befehl zurückgegebenen sollte zu erstellende Liste der benutzerdefinierten Domänennamen als auch die Standardeinstellung **. azurewebsite.net** Eintrag.

**Verwenden der Azure-Verwaltungsportal Domänennamen hinzufügen**

1. Öffnen Sie in Ihrem Browser das [Azure-Verwaltungsportal][portal].

2. Klicken Sie auf der Registerkarte **Websites** auf den Namen der Website, **Dashboard aus**, und wählen Sie dann aus den unteren Rand der Seite **Manage Domains** .

    ![][setcname2]

6. Geben Sie in das Textfeld **DOMÄNENNAMEN** den Namen der Domäne, den Sie konfiguriert haben.

    ![][setcname3]

6. Klicken Sie auf das Häkchen, um den Domänennamen zu akzeptieren.

Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Domänennamen** Seitenrand **Konfigurieren** Ihrer Website aufgeführt sein.

> [AZURE.NOTE] Nachdem Sie den benutzerdefinierten Domänennamen definiert durch den A-Eintrag zu Ihrer Website hinzugefügt haben, können Sie den Awverify CNAME-Eintrag mit den Tools von Ihrer Registrierungsstelle entfernen. Jedoch, wenn Sie eine andere A hinzufügen möchten Datensatz in Zukunft, müssen Sie die Awverify neu erstellen Datensatz, bevor Sie den neuen Domänennamen zuordnen können definiert durch den neuen Datensatz mit der Website.

## <a name="next-steps"></a>Nächste Schritte

-   [Zum Verwalten von Websites](/manage/services/web-sites/how-to-manage-websites/)

-   [Konfigurieren Sie ein SSL-Zertifikat für Websites](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
