<properties
    pageTitle="Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-app"
    description="Erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen (Eitelkeit Domäne) zu Ihrer Anwendung in Azure-App-Verwaltungsdienst zuordnen."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="cephalin"/>

# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-app

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

In diesem Artikel wird gezeigt, wie Sie manuell einen benutzerdefinierten Domänennamen zur Web app, mobile-app Back-End- oder API-app im [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md)zuordnen. 

Ihre app Lieferumfang bereits eine eigene Unterdomäne azurewebsites.net. Angenommen, wenn der Name der app **Contoso**ist, ist der Domänenname **contoso.azurewebsites.net**. Sie können jedoch eine benutzerdefinierte Domäne zuordnen benennen App daher, die entsprechende URL ein, wie `www.contoso.com`, Ihrer Marke widerspiegeln.

>[AZURE.NOTE] Abrufen von Hilfe bei Azure-Experten in den [Azure-Foren](https://azure.microsoft.com/support/forums/). Wechseln Sie zu der [Website Azure unterstützen](https://azure.microsoft.com/support/options/) noch höheren Grad der Unterstützung und klicken Sie auf **Erste unterstützen**.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## <a name="buy-a-new-custom-domain-in-azure-portal"></a>Kaufen Sie eine neue benutzerdefinierte Domäne Azure-Portal

Wenn Sie einen benutzerdefinierten Domänennamen bereits erworben nicht geschehen ist, können Sie kaufen und direkt in Ihrem app Einstellungen im [Azure-Portal](https://portal.azure.com)zu verwalten. Diese Option erleichtert das Zuordnen eine benutzerdefinierte Domäne zu Ihrer Anwendung, ob Ihre app [Azure Datenverkehr Manager](web-sites-traffic-manager-custom-domain-name.md) oder nicht verwendet. 

Anweisungen finden Sie unter [kaufen Sie einen benutzerdefinierten Domänennamen für App-Dienst](custom-dns-web-site-buydomains-web-app.md).

## <a name="map-a-custom-domain-you-purchased-externally"></a>Zuordnen einer benutzerdefinierten Domänennamens, die Sie extern erworben haben.

Wenn Sie bereits eine benutzerdefinierte Domäne aus [Azure DNS](https://azure.microsoft.com/services/dns/) oder von einem Drittanbieter-Anbieter erworben haben, gibt es drei Hauptschritte zuordnen die benutzerdefinierte Domäne zu Ihrer Anwendung:

1. [Die IP-Adresse *(nur einen Datensatz)* Get-app](#vip).
2. [Erstellen der DNS-Einträge, die Ihre Domäne zu Ihrer Anwendung zuordnen](#createdns). 
    - **Wo**: Ihre Domäne die Registrierungsstelle-Verwaltungstool (z. B. Azure DNS, GoDaddy usw.).
    - **Warum**:, damit Ihre domänenregistrierungsstelle Quelldatentabelle bekannt die gewünschte benutzerdefinierte Domäne zu Ihrer Azure-Anwendung ist.
1. [Aktivieren des benutzerdefinierten Domänennamens für Ihre app Azure](#enable).
    - **Wo**: der [Azure-Portal](https://portal.azure.com).
    - **Warum**: damit Ihre app weiß auf Befehle in den benutzerdefinierten Domänennamen zu reagieren.
3. [Überprüfen der DNS-Verteilung](#verify).

### <a name="types-of-domains-you-can-map"></a>Typen von Domänen, die zugeordnet werden kann

Azure App-Verwaltungsdienst können Sie die folgenden Kategorien von benutzerdefinierten Domänen zu Ihrer Anwendung zuordnen.

- **Root Domain** - den Namen der Domäne, die Sie bei der domänenregistrierungsstelle reserviert (dargestellt durch die `@` Datensatz in der Regel hosten). Beispielsweise **"contoso.com"**.
- **Unterdomäne** - jeder Domäne, die unter Ihrer Stammdomäne befindet. Beispielsweise **www.contoso.com** (dargestellt durch die `www` Eintrag host).  Sie können anderen apps in Azure verschiedene Unterdomänen derselben Stamm Domäne zuordnen.
- **Platzhalterzeichen Domäne** - [Alle Unterdomäne, dessen ganz links DNS-Bezeichnung ist `*` ](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (z. B. hosten Einträge `*` und `*.blogs`). Beispielsweise ** \*. "contoso.com"**.

### <a name="types-of-dns-records-you-can-use"></a>Typen von DNS-Einträge, die Sie verwenden können

Je nach Bedarf können Sie zwei verschiedene Arten von standard-DNS-Einträge Ihrer benutzerdefinierte Domäne zuordnen: 

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) - Karten, die Ihren benutzerdefinierten Domänennamen zu der Azure-app virtuelle IP-Adresse direkt. 
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record) - maps Ihren benutzerdefinierten Domänennamen in Ihrer app Azure Domänennamen, * *&lt;*Appname*>. azurewebsites.net**. 

Der Vorteil von CNAME ist, dass sie über die IP-Adressänderungen beibehalten. Wenn Sie löschen und Ihre app erstellen oder Ändern von eine höhere Preisgestaltung Stufe wieder in die Ebene **freigegeben** , kann Ihrer app virtuelle IP-Adresse ändern. Durch eine Änderung ein CNAME-Eintrag immer noch gültig ist, während ein A-Datensatzes ein Update erforderlich ist. 

Das Lernprogramm erfahren Sie die Schritte für die Verwendung des A-Eintrags und auch für die Verwendung des CNAME-Eintrags.

>[AZURE.IMPORTANT] Erstellen Sie einen CNAME-Eintrag für Ihre Stammdomäne (d. h. der "Stammdatensatz"). Weitere Informationen finden Sie unter [Warum kann ein CNAME-Eintrag nicht verwendet werden bei der Domäne aus](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Zum Zuordnen einer Stammdomäne zu Ihrer Azure-Anwendung, verwenden Sie stattdessen einen A-Eintrag.

<a name="vip"></a>
## <a name="step-1-a-record-only-get-apps-ip-address"></a>Schritt 1. *(Nur einen Datensatz)* Abrufen von app IP-Adresse
Um einen benutzerdefinierten Domänennamen verwenden eines A-Datensatzes zuordnen möchten, benötigen Sie Ihre Azure app IP-Adresse ein. Wenn Sie stattdessen einen CNAME-Eintrag mit zugeordnet sind, diesen Schritt überspringen und auf im nächsten Abschnitt verschieben.

1.  Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)an.

2.  Klicken Sie im Menü links auf **App-Dienste** .

4.  Klicken Sie auf die app, und klicken Sie auf **benutzerdefinierte Domänen**.

6.  Notieren Sie sich die IP-Adresse über Hostnamen Abschnitt...

    ![Karte benutzerdefinierten Domänennamen mit einem Datensatz: erste IP-Adresse für Ihre App-Verwaltungsdienst Azure-app](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  Lassen Sie dieses Portal Blade geöffnet. Sie werden wieder zu gelangen, nachdem Sie die DNS-Einträge erstellen.

<a name="createdns"></a>
## <a name="step-2-create-the-dns-records"></a>Schritt 2. Erstellen Sie die DNS-Datensätze

Melden Sie sich bei Ihrer domänenregistrierungsstelle, und verwenden Sie deren Tool zum Hinzufügen eines A-Eintrags oder CNAME-Eintrag. Jeder Registrierungsstelle Benutzeroberfläche unterscheidet sich leicht, damit der Dokumentation Ihres Anbieters wenden Sie sich an sollte. Es gibt jedoch einige allgemeinen Richtlinien.

1.  Suchen Sie die Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit der Bezeichnung **Domänennamen**, **DNS**oder **Name Server-Verwaltung**. Häufig finden Sie den Link, indem Sie Ihre Kontoinformationen anzeigen und suchen Sie nach einem Link wie **My Domains**.
2.  Suchen Sie nach einem Link, mit dem Sie hinzufügen oder Bearbeiten von DNS-Einträge. Dies möglicherweise eine **Zonendatei** oder **DNS-Einträge** Link oder einer Verknüpfung mit Konfiguration **Erweitert** .
3.  Erstellen Sie den Eintrag, und speichern Sie die Änderungen zu.
    - [Hinweise für eines A-Datensatzes finden Sie hier](#a).
    - [Anweisungen für einen CNAME-Eintrag hier sind](#cname).

<a name="a"></a>
### <a name="create-an-a-record"></a>Erstellen eines A-Datensatzes

Wenn Ihre Azure app IP-Adresse zuordnen ein A-Datensatzes verwenden möchten, müssen Sie tatsächlich sowohl eines A-Datensatzes und einen TXT-Eintrag zu erstellen. A-Eintrag für die Auflösung von DNS-Einträge selbst ist, und der TXT-Eintrag ist für Azure, um sicherzustellen, dass Sie der Besitzer des benutzerdefinierten Domänennamens sind. 

Konfigurieren Sie Ihren A-Eintrag wie folgt (@ steht normalerweise auf die Stammdomäne):
 
<table cellspacing="0" border="1">
  <tr>
    <th>Beispiel für FQDN</th>
    <th>Ein Host</th>
    <th>Einen Wert</th>
  </tr>
  <tr>
    <td>"contoso.com" (Stamm)</td>
    <td>@</td>
    <td>IP-Adresse von <a href="#vip">Schritt 1</a></td>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>"www"</td>
    <td>IP-Adresse von <a href="#vip">Schritt 1</a></td>
  </tr>
  <tr>
    <td>*. contoso.com (Platzhalterzeichen)</td>
    <td>*</td>
    <td>IP-Adresse von <a href="#vip">Schritt 1</a></td>
  </tr>
</table>

Ihren zusätzliche TXT-Eintrag zu gelangen, auf der Messe, die von Zuordnungen &lt; *Unterdomäne*>. &lt; *Rootdomain*> zu &lt; *Appname*>. azurewebsites.net. Konfigurieren Sie Ihren TXT-Eintrag wie folgt ein:

<table cellspacing="0" border="1">
  <tr>
    <th>Beispiel für FQDN</th>
    <th>TXT-Host</th>
    <th>TXT-Wert</th>
  </tr>
  <tr>
    <td>"contoso.com" (Stamm)</td>
    <td>@</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>"www"</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (Platzhalterzeichen)</td>
    <td>*</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
###Erstellen Sie einen CNAME-Eintrag

Wenn Sie einen CNAME-Eintrag verwenden, um Ihre Azure app Standarddomänennamen zuordnen, benötigen Sie kein zusätzlichen TXT-Eintrags wie mit einem A-Eintrag. 

>[AZURE.IMPORTANT] Erstellen Sie einen CNAME-Eintrag für Ihre Stammdomäne (d. h. der "Stammdatensatz"). Weitere Informationen finden Sie unter [Warum kann ein CNAME-Eintrag nicht verwendet werden bei der Domäne aus](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Zum Zuordnen einer Stammdomäne zu Ihrer Azure-Anwendung, verwenden Sie stattdessen einen [A-Eintrag](#a) .

Konfigurieren der CNAME-Eintrag wie folgt (@ steht normalerweise auf die Stammdomäne):

<table cellspacing="0" border="1">
  <tr>
    <th>Beispiel für FQDN</th>
    <th>CNAME-Host</th>
    <th>CNAME-Wert</th>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>"www"</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (Platzhalterzeichen)</td>
    <td>*</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>
##Schritt 3. Aktivieren des benutzerdefinierten Domänennamens für Ihre app

Wieder im vorher **Benutzerdefinierte Domänen** in der Azure-Portal (siehe [Schritt 1](#vip)), müssen Sie den vollqualifizierten Domänennamen (FQDN) Ihrer benutzerdefinierten Domäne in der Liste hinzufügen.

1.  Wenn Sie angemeldet [Azure-Portal](https://portal.azure.com)nicht getan haben.

2.  Klicken Sie im Portal Azure im linken Menü auf auf **App-Dienste** .

3.  Klicken Sie auf die app, und klicken Sie auf **benutzerdefinierte Domänen** > **Hostname hinzufügen**.

4.  Fügen Sie den vollqualifizierten Domänennamen Ihrer benutzerdefinierten Domäne in der Liste (z. B. **www.contoso.com**) ein.

    ![Zuordnen einen benutzerdefinierten Domänennamen zu einer Azure-app: Liste der Domänennamen hinzufügen](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure versucht, überprüfen Sie den Namen der Domäne, den Sie hier verwenden. Achten Sie darauf, dass die gleichen Domänennamen ist, für den Sie in [Schritt2](#createdns)ein DNS-Eintrags erstellt. 

5.  Klicken Sie auf **Überprüfen**.

6.  Beim Klicken auf die **Gültigkeit** Azure wird deaktivieren Domäne Überprüfung Workflow Starten eines. Hiermit wird für Besitzer der Domäne als auch Hostname Verfügbarkeit und Bericht Erfolg oder detaillierte Fehler mit Nachschlagewerke Guidence zum Beheben des Fehlers überprüft.    

7.  Bei einer erfolgreichen Validierung **Hinzufügen Hostname** Schaltfläche wird aktiv und Sie können die Hostname zuweisen. 

8.  Navigieren Sie zu Ihren benutzerdefinierten Domänennamen in einem Browser, nach Beendigung Azure Ihren neuen benutzerdefinierten Domänennamen konfigurieren. Im Browser sollte Ihre Azure app zu öffnen, was bedeutet, dass Ihr benutzerdefinierter Domänenname ordnungsgemäß konfiguriert ist.

> [AZURE.NOTE] Ist der DNS-Eintrag bereits in (aktive Domäne bedienen Datenverkehr Szenario) verwenden, und Sie müssen präventiv Web app zur Überprüfung der Domäne gebunden, einfach erstellen Sie dann einen TXT-Einträge als Beispiele in der folgenden Tabelle angezeigt. Ihren zusätzliche TXT-Eintrag zu gelangen, auf der Messe, die von Zuordnungen &lt; *Unterdomäne*>. &lt; *Rootdomain*> zu &lt; *Appname*>. azurewebsites.net. 
> <table cellspacing="0" border="1">
  <tr>
    <th>Beispiel für FQDN</th>
    <th>TXT-Host</th>
    <th>TXT-Wert</th>
  </tr>
  <tr>
    <td>"contoso.com" (Stamm)</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (Sub)</td>
    <td>awverify.www.contoso.com</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
    <tr>
    <td>*. contoso.com (Sub)</td>
    <td>awverify.*.contoso.com</td>
    <td>&lt;<i>Appname</i>>. azurewebsites.net</td>
  </tr>
</table>
Nachdem diesen DNS-Eintrag erstellt wurde, kehren Sie zum Azure-Portal, und fügen Sie Ihren benutzerdefinierten Domänennamen, bei der Web-app.
 

<a name="verify"></a>
##Überprüfen der DNS-Verteilung

Nachdem Sie die Konfigurationsschritte abgeschlossen haben, kann es eine Weile dauern die Änderungen zu verteilen, abhängig von Ihrem DNS-Anbieter wird. Sie können überprüfen, dass die DNS-Übermittlung wie erwartet mithilfe von [http://digwebinterface.com/](http://digwebinterface.com/)ist. Nachdem Sie zu der Website navigieren, geben Sie die Hostnamen in das Textfeld, und klicken Sie auf, **einsteigen**. Überprüfen Sie die Ergebnisse, um sicherzustellen, dass die kürzlich vorgenommenen Änderungen wirksam sind.  

![Zuordnen einen benutzerdefinierten Domänennamen zu einer Azure-app: Überprüfen der DNS-Verteilung](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] Die Weitergabe von DNS-Einträge kann (manchmal mehr) bis zu 48 Stunden dauern. Wenn Sie alles richtig konfiguriert haben, müssen Sie warten, bis die Weitergabe erfolgreich durchgeführt werden kann.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie Ihren benutzerdefinierten Domänennamen mit HTTPS [Erwerb eines SSL-Zertifikats in Azure](web-sites-purchase-ssl-web-site.md) , oder [Verwenden Sie ein Zertifikat einer Zertifizierungsstelle an anderer Stelle](web-sites-configure-ssl-certificate.md)gesichert.

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

[Erste Schritte mit Azure DNS-Einträge](../dns/dns-getstarted-create-dnszone.md)  
[Erstellen von DNS-Einträge für eine Web app in einer benutzerdefinierten Domäne](../dns/dns-web-sites-custom-domain.md)  
[Stellvertretung Domäne zu Azure DNS](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
