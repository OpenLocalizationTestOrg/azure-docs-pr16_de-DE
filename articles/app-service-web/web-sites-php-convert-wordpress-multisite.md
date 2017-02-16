<properties 
    pageTitle="Konvertieren von WordPress in mehreren Sites in Azure App-Verwaltungsdienst" 
    description="Erfahren Sie, wie Sie eine vorhandene WordPress Web-app durch den Katalog in Azure erstellt wird, und wandeln Sie es in WordPress mehreren Sites" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Konvertieren von WordPress in mehreren Sites in Azure App-Verwaltungsdienst

## <a name="overview"></a>(Übersicht)

*Von [Robert Lobaugh][ben-lobaugh], [Microsoft öffnen Technologies Inc.][ms-open-tech]*

In diesem Lernprogramm erfahren Sie, wie Übernehmen einer vorhandenen WordPress Web-app durch den Katalog in Azure erstellt und diese in eine Installation WordPress mehreren Sites konvertiert. Darüber hinaus erfahren Sie, wie Sie jedem der Unterwebsites innerhalb Ihrer Installation eine benutzerdefinierte Domäne zuweisen.

Es wird angenommen, dass Sie eine vorhandene Installation von WordPress haben. Wenn Sie nicht der Fall ist, führen Sie die Anleitung im [Erstellen einer Website aus dem Katalog in Azure WordPress][website-from-gallery].

Konvertieren einer vorhandenen WordPress einzelnen Website installieren zu mehreren Sites ist im Allgemeinen recht einfach und zahlreiche hier die ersten Schritte kommen direkt aus dem [A-Netzwerk erstellen] [ wordpress-codex-create-a-network] Seite auf die [WordPress Codex](http://codex.wordpress.org).

Lassen Sie uns beginnen.

## <a name="allow-multisite"></a>Zulassen von mehreren Sites

Müssen Sie zuerst Aktivieren von mehreren Sites, bis die `wp-config.php` Datei mit der **WP\_zulassen\_mehreren Sites** Konstante. Es gibt zwei Methoden zum Bearbeiten von Web app-Dateien: der erste wird über FTP und das zweite bis Git. Wenn Sie mit einer der folgenden Methoden für die Einrichtung wie nicht vertraut sind, finden Sie die folgenden Lernprogramme:

* [PHP mit MySQL und FTP-Website][website-w-mysql-and-ftp-ftp-setup]

* [PHP-Website mit MySQL und Git][website-w-mysql-and-git-git-setup]

Öffnen der `wp-config.php` Datei mit dem Editor Ihrer Wahl, und fügen Sie den folgenden oben im `/* That's all, stop editing! Happy blogging. */` Linie.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Achten Sie darauf, dass die Datei zu speichern und wieder auf dem Server hochladen!

## <a name="network-setup"></a>Netzwerk-Setup

Melden Sie sich in den Bereich *wp-Administrator* Ihres Web app und Sie sollte angezeigt werden ein neues Element unter im Menü **Extras** aufgerufen **Netzwerk einrichten**. Klicken Sie auf **Netzwerk-Setup** , und füllen Sie die Details des Netzwerks.

![Netzwerk-Setup-Bildschirm][wordpress-network-setup]

In diesem Lernprogramm verwendet das *Unterverzeichnisse* Website-Schema aus, da es immer arbeiten soll, und wir von benutzerdefinierten Domänen für jede Unterwebsite später im Lernprogramm einrichten. Jedoch sollte es möglich, eine Unterdomäne zu installieren, wenn Sie eine Domäne über das Einrichten und [Azure-Portal](https://portal.azure.com) Platzhalterzeichen DNS-ordnungsgemäß zuordnen, einrichten.

Weitere Informationen zu Unterdomäne im Vergleich mit einer Unterverzeichnis Setups finden Sie unter den [Typen von Netzwerkarchitektur mit] [ wordpress-codex-types-of-networks] Artikel über die WordPress Codex.

## <a name="enable-the-network"></a>Aktivieren Sie im Netzwerk

Das Netzwerk ist jetzt in der Datenbank konfiguriert, doch es gibt eine verbleibende Schritt die Netzwerkfunktionalität zu aktivieren. Fertigstellen der `wp-config.php` Einstellungen und vergewissern Sie sich `web.config` jedem Standort ordnungsgemäß weiterleitet.


Nach dem Klicken auf die Schaltfläche " **Installieren** " auf der Seite *Netzwerk einrichten* , versucht WordPress Aktualisieren der `wp-config.php` und `web.config` Dateien. Jedoch sollten Sie immer überprüfen die Dateien, um sicherzustellen, dass die Updates erfolgreich waren. Wenn dies nicht der Fall ist, wird dieser Bildschirm präsentieren Sie mit den erforderlichen Updates. Bearbeiten Sie und speichern Sie die Dateien.


Sichern nach Ausführung dieser Updates, die Sie benötigen, melden Sie sich ab und melden Sie sich in dem Dashboard wp-Administrator.

Klicken Sie auf der Administrator Balken beschriftete **Meine Websites**sollte nun ein zusätzliches Menü werden. In diesem Menü können Sie durch das **Netzwerkadministrator** Dashboard des neuen Netzwerks steuern.

## <a name="adding-custom-domains"></a>Hinzufügen von benutzerdefinierten Domänen

Die [Zuordnung zu WordPress MU Domäne] [ wordpress-plugin-wordpress-mu-domain-mapping] -Plug-in ist es ein Kinderspiel benutzerdefinierte Domänen auf einer beliebigen Website in Ihrem Netzwerk hinzufügen. In der Reihenfolge für das Plug-in ordnungsgemäß funktioniert müssen Sie einige zusätzliche Einrichtungsschritte auf dem Portal und bei Ihrer domänenregistrierungsstelle ausführen.

## <a name="enable-domain-mapping-to-the-web-app"></a>Aktivieren Sie die Domäne Zuordnung zu Web app

Hinzufügen von benutzerdefinierten Domänen bei Web Apps unterstützt der **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Plan-Modus nicht. Sie müssen in **freigegeben** oder **Standard** -Modus zu schalten. Zweck

* Melden Sie sich bei der Azure-Portal, und suchen Sie die Web app. 
* Klicken Sie auf der Registerkarte **Skalieren von** in **-Einstellungen**.
* Wählen Sie unter **Allgemein** *freigegeben* oder *STANDARD*
* Klicken Sie auf **Speichern**

Möglicherweise wird eine Meldung mit der Frage, überprüfen die Änderung, und bestätigen Sie, dass Web app jetzt Kosten, anfallen kann je nach Verwendung und die anderen Konfiguration, die Sie festlegen.

Es dauert ein paar Sekunden Verarbeitungszeit die neuen Einstellungen, also jetzt ist ein guter Zeitpunkt für die Einrichtung Ihrer Domäne zu starten.

## <a name="verify-your-domain"></a>Überprüfen Sie Ihrer Domäne

Bevor Azure Web Apps zuordnen eine Domäne zu der Website zulassen wird, müssen Sie zuerst stellen Sie sicher, dass Sie die Autorisierung zuordnen die Domäne besitzen. Dazu müssen Sie einen neuen CNAME-Eintrag zu Ihrem DNS-Eintrag hinzufügen.

* Melden Sie sich bei Ihrer Domäne DNS-manager
* Erstellen Sie einen neuen CNAME- *awverify*
* Zeigen Sie auf *Awverify* *Awverify. YOUR_DOMAIN.azurewebsites.NET*

Es dauert einige Zeit für die DNS-Änderungen vollständige wirksam, wenn die folgenden Schritte nicht sofort, funktionieren daher wechseln, stellen Sie eine Kaffeetasse, und klicken Sie dann wieder, und versuchen Sie es erneut.

## <a name="add-the-domain-to-the-web-app"></a>Hinzufügen der Domäne bei der Web-app

Kehren Sie zu Ihrer Online über das Azure-Portal zurück, klicken Sie auf **Einstellungen**, und klicken Sie dann auf **benutzerdefinierte Domänen und SSL**.

Wenn die *SSL-Einstellungen* angezeigt werden, sehen Sie die Felder, in dem Sie alle Domänen Eingabemethoden werden die Web app zugewiesen werden soll. Wenn Sie eine Domäne hier nicht aufgelistet ist, wird es nicht zur Verfügung für die Zuordnung innerhalb WordPress, unabhängig davon, wie die DNS-Domäne einrichten ist.

![Verwalten von benutzerdefinierten Domänen Dialogfeld][wordpress-manage-domains]

Nach der Eingabe Ihrer Domäne in das Textfeld, wird Azure des CNAME-Eintrags überprüfen, die Sie zuvor erstellt haben. Wenn Sie die DNS-Einträge nicht vollständig verteilt wurde, wird ein roter Indikator angezeigt. Wenn sie erfolgreich war, wird ein grünes Häkchen angezeigt. 

Achten Sie darauf, die am unteren Rand des Dialogfelds aufgeführte IP-Adresse ein. Sie benötigen diese Option, um den A-Eintrag für Ihre Domäne einrichten.

## <a name="setup-the-domain-a-record"></a>Einrichten der Domäne einen Datensatz

Wenn die anderen Schritte erfolgreich waren, können Sie jetzt Azure Web app durch DNS A-Datensatz die Domäne zuweisen. 

Es ist wichtig, dass Azure Web apps sowohl CNAME- und A-Einträge, akzeptiert hier zu beachten, jedoch *müssen* Sie einen A-Eintrag verwenden, um entsprechende Domäne Zuordnung zu ermöglichen. Ein CNAME kann weitergeleitet werden in einem anderen CNAME, welche ist, was mit YOUR_DOMAIN.azurewebsites.net Azure für Sie erstellt.

Verwenden die IP-Adresse aus dem vorherigen Schritt, zurück zu Ihrer DNS-Manager und verweisen auf diese IP-A-Eintrag für die Einrichtung.


## <a name="install-and-setup-the-plugin"></a>Installieren und Einrichten des Plug-Ins

WordPress mehreren Sites haben derzeit keine integrierte Methode, um benutzerdefinierte Domänen zuordnen. Es ist jedoch ein Plug-in aufgerufen [WordPress MU Domäne Zuordnung] [ wordpress-plugin-wordpress-mu-domain-mapping] , die Funktionalität für Sie hinzufügt. Melden Sie sich an den Administrator Netzwerk Teil Ihrer Website, und installieren Sie das Plug-in **WordPress MU Domäne zuordnen** .

Nach dem Installieren und aktivieren das Plug-in, finden Sie auf **Einstellungen** > so konfigurieren Sie das Plug-in**Domäne zuordnen** . Geben Sie in das erste Textfeld, *IP-Adresse des Servers*, die IP-Adresse, die Sie verwendet, um den A-Eintrag für die Domäne einrichten. Legen Sie die gewünschte *Domänenoptionen* fest (die Standardeinstellungen sind oft fein), und klicken Sie auf **Speichern**.

## <a name="map-the-domain"></a>Zuordnen der Domäne

Besuchen Sie das **Dashboard** für die Website, die, der Sie die Domäne zu zuordnen möchten. Klicken Sie auf **Extras** > **Zuordnung Domäne** , und geben Sie die neue Domäne in das Textfeld ein, und klicken Sie auf **Hinzufügen**.

Standardmäßig wird die neue Domäne zu der Website-Domäne automatisch generierte neu geschrieben werden. Wenn Sie den gesamten Verkehr in die neue Domäne gesendet haben möchten, aktivieren Sie im Feld *primäre Domäne für diesen Blog* vor dem Speichern. Sie können eine unbegrenzte Anzahl von Domänen zu einer Website hinzufügen, aber nur ein Primärschlüssel sein kann.

## <a name="do-it-again"></a>Führen Sie es erneut

Azure Web Apps können Sie eine unbegrenzte Anzahl von Domänen in einem Web-app hinzufügen. Zum Hinzufügen weiterer Domänen müssen Sie in den Abschnitten **Überprüfen Ihrer Domäne** und **die Domäne einen Eintrag für die Einrichtung** für jede Domäne ausführen.  

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
