Domain Name System (DNS) wird verwendet, um Punkte im Internet zu suchen. Angenommen, wenn Sie eine Adresse in Ihrem Browser, oder klicken Sie auf einen Link auf einer Webseite, verwendet es DNS um die Domäne in eine IP-Adresse zu übersetzen. Die IP-Adresse weist Ähnlichkeit mit der eine Anschrift, aber es ist nicht sehr personenbezogenen freundlichen. Beispielsweise ist es viel einfacher, einen DNS-Namen wie **"contoso.com"** als es ist, eine IP-Adresse wie 192.168.1.88 oder 2001:0:4137:1f67:24a2:3888:9cce:fea3 Denken Sie daran denken Sie daran.

Das DNS-System basiert auf *Einträge*. Einträge ordnen Sie einen bestimmten *Namen*, wie etwa **"contoso.com"**entweder eine IP-Adresse oder einen anderen DNS-Namen. Wenn eine Anwendung wie einem Webbrowser einen Namen in DNS nachgeschlagen werden, findet den Eintrag, und welchen es als Adresse auf zeigt verwendet. Wenn der Wert, den, dem Sie auf zeigt, eine IP-Adresse ist, wird im Browser diesen Wert verwenden. Wenn sie auf einen anderen DNS-Namen verweist, muss die Anwendung Auflösung erneut ausführen. Schließlich werden alle mit einer namensauflösung von in eine IP-Adresse beenden.

Beim Erstellen einer Website Azure erhält automatisch ein Namen für die DNS-Einträge zu der Website. Dieser Name hat die Form eines ** &lt;Yoursitename&gt;. azurewebsites.net**. Wenn Sie Ihre Website als Endpunkt Azure Datenverkehr Manager hinzufügen, Ihre Website zugegriffen werden dann über die ** &lt;Yourtrafficmanagerprofile&gt;. trafficmanager.net** Domäne.

> [AZURE.NOTE] Wenn Ihre Website als einen Endpunkt Datenverkehr Manager konfiguriert ist, verwenden Sie die **. trafficmanager.net** Adresse bei der DNS-Einträge zu erstellen.

> Sie können nur CNAME-Einträge mit den Datenverkehr Manager verwenden

Es gibt auch mehrere Typen von Datensätzen, jede mit ihren eigenen Funktionen und Einschränkungen, aber für Websites, die so konfiguriert, dass als Datenverkehr Manager Endpunkte, uns nur wichtig über einen; *CNAME* -Einträge.

###<a name="cname-or-alias-record"></a>CNAME oder Alias-Eintrag

Ein CNAME-Eintrag ordnet einen *bestimmten* DNS-Namen, wie z. B. **mail.contoso.com** oder **www.contoso.com**, ein anderer Domänenname wird (kanonische). Im Falle von Azure-Websites, die mit den Datenverkehr-Manager, der kanonische Domänenname ist die ** &lt;Anwendung >. trafficmanager.net** Domänennamen Ihres Profils Datenverkehr-Manager. Nach dem Erstellen, erstellt der CNAME-Eintrag ein Alias für die ** &lt;Anwendung >. trafficmanager.net** Domänennamen ein. Der CNAME-Eintrag wird die IP-Adresse des beheben Ihrer ** &lt;Anwendung >. trafficmanager.net** Domänennamens automatisch, wenn die IP-Adresse der Website verwandelt hat, haben Sie keine Aktionen durchführen.

Nachdem Sie den Datenverkehr an den Datenverkehr Manager ankommt, leitet sie den Datenverkehr auf Ihrer Website, die mit den Lastenausgleich Methode, die sie für konfiguriert ist. Dies ist transparent für die Besucher Ihrer Website. Sie werden nur den benutzerdefinierten Domänennamen in ihrem Browser angezeigt.

> [AZURE.NOTE] Einige domänenregistrierungsstellen können Sie nur Unterdomänen zuordnen, wenn Sie einen CNAME-Eintrag, beispielsweise **www.contoso.com**und nicht Stamm Namen, wie etwa **"contoso.com"**verwenden. Weitere Informationen zu den CNAME-Einträge finden Sie unter der Dokumentation Ihrer Registrierungsstelle, <a href="http://en.wikipedia.org/wiki/CNAME_record">den Eintrag Wikipedia auf CNAME-Eintrag</a>oder das Dokument <a href="http://tools.ietf.org/html/rfc1035">IETF Domain Names - Implementierung und Spezifikation</a> .
