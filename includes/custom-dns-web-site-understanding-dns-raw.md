Domain Name System (DNS) wird verwendet, um Ressourcen im Internet zu suchen. Angenommen, wenn Sie eine Webadresse für die app in Ihrem Browser eingeben, oder klicken Sie auf einen Link auf einer Webseite, verwendet es DNS die Domäne in eine IP-Adresse übersetzen. Die IP-Adresse weist Ähnlichkeit mit der eine Anschrift, aber es ist nicht sehr personenbezogenen freundlichen. Beispielsweise ist es viel einfacher, einen DNS-Namen wie **"contoso.com"** als es ist, eine IP-Adresse wie 192.168.1.88 oder 2001:0:4137:1f67:24a2:3888:9cce:fea3 Denken Sie daran denken Sie daran.

Das DNS-System basiert auf *Einträge*. Einträge ordnen Sie einen bestimmten *Namen*, wie etwa **"contoso.com"**entweder eine IP-Adresse oder einen anderen DNS-Namen. Wenn eine Anwendung wie einem Webbrowser einen Namen in DNS nachgeschlagen werden, findet den Eintrag, und welchen es als Adresse auf zeigt verwendet. Wenn der Wert, den, dem Sie auf zeigt, eine IP-Adresse ist, wird im Browser diesen Wert verwenden. Wenn sie auf einen anderen DNS-Namen verweist, muss die Anwendung Auflösung erneut ausführen. Schließlich werden alle mit einer namensauflösung von in eine IP-Adresse beenden.

Bei der Erstellung einer Web app im App-Dienst wird automatisch ein DNS-Name des Web app zugewiesen. Dieser Name hat die Form eines ** &lt;Yourwebappname&gt;. azurewebsites.net**. Es steht auch eine virtuelle IP-Adresse für die Verwendung beim Erstellen von DNS-Einträge, sodass Sie entweder Datensätze erstellen können, die es auf der **. azurewebsites.net**, oder Sie können auf die IP-Adresse zeigen.

> [AZURE.NOTE] Wenn Sie löschen und neu Web app erstellen oder ändern Sie den App-Dienst Plan-Modus auf **frei** , nachdem es **einfacher**, **freigegeben**oder **Standard**festgelegt wurde, wird die IP-Adresse der Web app ändern.

Es gibt auch mehrere Typen von Datensätzen, jede mit ihren eigenen Funktionen und Einschränkungen, aber von Web apps wir nur interessieren zwei, *A* und *CNAME* -Einträge.

###<a name="address-record-a-record"></a>Adresseintrag (einen Datensatz)

Ein A-Datensatzes eine Domäne, beispielsweise **"contoso.com"** oder **www.contoso.com**, *oder eine Domäne Platzhalterzeichen* wie maps ** \*. "contoso.com"**, um eine IP-Adresse. Im Falle einer Web app im App-Dienst Adresse entweder die virtuelle IP-Adresse des Diensts oder eine bestimmte IP-, die Sie für Ihre Web app erworben haben.

Die Hauptvorteile von eines A-Datensatzes über einen CNAME-Eintrag sind:

* Sie können eine IP-Adresse eine Stammdomäne wie **contoso.com** zuordnen; Viele Registrierungsstelle zulassen nur diese mit einer Datensätze

* Sie können einen Eintrag, der einen Platzhalter, wie verwendet haben ** \*. "contoso.com"**, würde die Anfragen für mehrere Unterdomänen wie **mail.contoso.com**, **blogs.contoso.com**oder **www.contso.com**behandelt.

> [AZURE.NOTE] Da eine statische IP-Adresse ein A-Datensatzes zugeordnet ist, kann es automatisch Änderungen an die IP-Adresse der Web app nicht auflösen. Beim Konfigurieren von Einstellungen für den benutzerdefinierten Domänennamen für Ihre Web app, ist eine IP-Adresse für die Verwendung mit einem Einträge enthalten. Dieser Wert möglicherweise jedoch ändern, wenn löschen und neu Web app erstellen oder ändern Sie den App-Dienst Plan Modus, in den Hintergrund auf **frei**.

###<a name="alias-record-cname-record"></a>Aliaseintrag (CNAME)

Ein CNAME-Eintrag ordnet einen *bestimmten* DNS-Namen, wie z. B. **mail.contoso.com** oder **www.contoso.com**, ein anderer Domänenname wird (kanonische). Bei der App-Dienst Web Apps, die kanonische Domänenname ist die ** &lt;Yourwebappname >. azurewebsites.net** Domänennamen der Web app. Nach dem Erstellen, erstellt der CNAME-Eintrag ein Alias für die ** &lt;Yourwebappname >. azurewebsites.net** Domänennamen ein. Der CNAME-Eintrag wird die IP-Adresse des beheben Ihrer ** &lt;Yourwebappname >. azurewebsites.net** Domänennamens automatisch, wenn die IP-Adresse des Web app verwandelt hat, haben Sie keine Aktionen durchführen.

> [AZURE.NOTE] Einige domänenregistrierungsstellen können Sie nur Unterdomänen zuordnen, wenn Sie einen CNAME-Eintrag, beispielsweise **www.contoso.com**und nicht Stamm Namen, wie etwa **"contoso.com"**verwenden. Weitere Informationen zu den CNAME-Einträge finden Sie unter der Dokumentation Ihrer Registrierungsstelle, <a href="http://en.wikipedia.org/wiki/CNAME_record">den Eintrag Wikipedia auf CNAME-Eintrag</a>oder das Dokument <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - Implementierung und Spezifikation</a> .

###<a name="web-app-dns-specifics"></a>Web app DNS-Besonderheiten

Verwenden eines A-Datensatzes mit Web Apps, müssen Sie zuerst eine der folgenden TXT-Einträge erstellen:

* **Für die Stammdomäne** - A DNS TXT-Eintrag der **@** auf ** &lt;Yourwebappname&gt;. azurewebsites.net**.

* **Für eine bestimmte Unterdomäne** - A DNS-Namen des ** &lt;Unterdomäne >** zu ** &lt;Yourwebappname&gt;. azurewebsites.net**. Beispielsweise **Blogs** der A-Eintrag für **blogs.contoso.com**ist.

* **Für die Platzhalterzeichen Sub-Dodmains** - A DNS TXT-Eintrag der *** zu ** &lt;Yourwebappname&gt;. azurewebsites.net**.

Diese TXT-Eintrag wird verwendet, um sicherzustellen, dass Sie die Domäne besitzen, die Sie verwenden möchten. Dies gilt zusätzlich zum Erstellen eines A-Datensatzes auf die virtuelle IP-Adresse der Web app zeigen.

Sie können die IP-Adresse finden und **. azurewebsites.net** Namen für Ihre Web-app, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie in Ihrem Browser das [Azure-Portal](https://portal.azure.com)aus.

2. Klicken Sie auf den Namen der Web app in das Blade **Web Apps** , und wählen Sie **benutzerdefinierte Domänen** aus den unteren Rand der Seite.

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. In das **benutzerdefinierte Domänen** Blade wird die virtuelle IP-Adresse angezeigt. Speichern Sie diese Informationen, wie er beim Erstellen von DNS-Datensätzen verwendet werden

    ![](./media/custom-dns-web-site/virtual-ip-address.png)

    > [AZURE.NOTE] Sie können keine benutzerdefinierten Domänennamen mit einer **Free** Web app und müssen ein upgrade der App-Serviceplan auf **freigegeben**, **grundlegende**, **Standard**oder **Premium** -Leiste. Weitere Informationen zu der App-Serviceplan Preise Ebenen, einschließlich zum Ändern der Preisgestaltung Ebene der Web app finden Sie unter [So skalieren Web apps](../articles/web-sites-scale.md).
