<properties
    pageTitle="Enterprise-Klasse WordPress auf Azure App-Verwaltungsdienst | Microsoft Azure"
    description="Erfahren Sie, wie eine Enterprise-Klasse WordPress-Website auf App-Verwaltungsdienst Azure gehostet"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Enterprise-Klasse WordPress Azure App-Diensts

Azure-App-Dienst bietet eine skalierbare, sichere und benutzerfreundliche-Umgebung für Auftrag kritischen, großen Maßstab [WordPress] [ wordpress] Websites. Microsoft selbst ausgeführt wird, Enterprise-Klasse Websites wie [Office] [ officeblog] und [Bing] [ bingblog] Blogs. Dieses Dokument veranschaulicht die Verwendung Azure App Dienst Web Apps zum Einrichten und Verwalten eines Enterprise-Klasse, cloudbasierten WordPress-Website, die eine große Anzahl von Besucher behandelt werden kann.

## <a name="architecture-and-planning"></a>Architektur und Planung

Eine einfache Installation von WordPress gelten nur zwei systemvoraussetzungen.

* **MySQL-Datenbank** - verfügbar bis [ClearDB in dem Azure Marketplace][cdbnstore], oder Sie können eigene MySQL-Installation auf Azure virtuellen Computern mit entweder [Windows] verwalten[ mysqlwindows] oder [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB bietet mehrere MySQL-Konfigurationen für unterschiedliche Leistungsmerkmale für jede Konfiguration. Finden Sie unter der [Azure-Store] [ cdbnstore] Informationen Angebote Azure-Store oder direkt auf der [Website ClearDB](http://www.cleardb.com/pricing.view)gesehen bereitgestellt.

* **PHP 5.2.4 oder größer** -App-Verwaltungsdienst Azure derzeit [PHP Versionen 5.4, 5.5 und 5.6]bereit[phpwebsite].

    > [AZURE.NOTE] Es empfiehlt sich, die neueste Version von PHP, um sicherzustellen, dass Sie die neuesten Sicherheitsupdates haben immer ausgeführt.

### <a name="basic-deployment"></a>Einfache Bereitstellung

Nur die grundlegenden Anforderungen konnten Sie einem Azure Bereich für eine einfache Lösung erstellen.

![ein Azure Web app und MySQL-Datenbank in einem Bereich mit einem einzelnen Azure gehostet wird][basic-diagram]

Während dies Sie zu Ihrer Anwendung Skalierung durch Erstellen von mehreren Web Apps-Instanzen von der Website zulassen möchten, wird alles in die Data Centers in einer bestimmten geografischen Region gehostet wird. Besucher von außerhalb dieses Bereichs möglicherweise längeren Reaktionszeiten bei Verwendung der Website, und wechseln Sie die Data Centers in diesem Bereich nach unten, also wird Ihrer Anwendung angezeigt.


### <a name="multi-region-deployment"></a>Mehrere Region Bereitstellung

Mit Azure [Datenverkehr Manager][trafficmanager], es ist möglich, die Ihre Website WordPress über mehrere geographischen Regionen skalieren, während Sie nur eine URL für Besucher. Alle Besucher nützlich sein über den Datenverkehr-Manager, und klicken Sie dann auf einen Bereich, basierend auf den Lastenausgleich Konfiguration laden weitergeleitet werden.

![eine gehostet in mehreren Regionen mit hohen Verfügbarkeit CDBR Router zum Weiterleiten von an MySQL über Regionen Azure Web app][multi-region-diagram]

In den einzelnen Bereichen die Website WordPress würde weiterhin über mehrere Instanzen von Web Apps skaliert werden, aber diese skalieren bestimmte Region ist; hoher Auslastung Regionen können anders als geringem Datenverkehr diejenigen skaliert werden.

Replikation und die Weiterleitung an mehreren MySQL-Datenbanken können erfolgen mithilfe ClearDBs [CDBR hohen Verfügbarkeit Router] [ cleardbscale] (Siehe links) oder [MySQL Cluster CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Mehrere Region Bereitstellung mit Medien speichern und Zwischenspeichern
Wenn die Website Uploads oder Mediendateien Host akzeptiert, verwenden Sie Azure Blob-Speicher. Wenn Sie zwischenspeichern, empfiehlt sich [Cache Redis][rediscache], [Den Memcache Cloud](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)oder eine der anderen Zwischenspeichern Angebote im [Azure Store](http://azure.microsoft.com/gallery/store/).

![gehostet in mehreren Regionen mit hohen Verfügbarkeit CDBR Router für MySQL, mit Cache verwaltet, Blob-Speicher und CDN einer Azure Web-app][performance-diagram]

BLOB-Speicher ist auf Regionen standardmäßig Geo-verteilt, damit Sie Dateien über alle Websites repliziert Gedanken besitzen. Sie können auch die Azure [Content Verteilung Network (CDN)] aktivieren[ cdn] für Blob-Speicher verteilt die Dateien Knoten näher an die Besucher zu beenden.

### <a name="planning"></a>Planung

#### <a name="additional-requirements"></a>Weitere Anforderungen

Aktion... | Verwenden Sie diese Option...
------------------------|-----------
**Hochladen Sie oder speichern Sie große Dateien** | [WordPress-Plug-In für die Verwendung von Blob-Speicher][storageplugin]
**Senden von e-Mails** | [SendGrid] [ storesendgrid] und das [WordPress-Plug-In für die Verwendung von SendGrid][sendgridplugin]
**Benutzerdefinierten Domänennamen** | [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure-App-Verwaltungsdienst][customdomain]
**HTTPS** | [Aktivieren Sie HTTPS für eine Web-app in Azure-App-Verwaltungsdienst][httpscustomdomain]
**Noch nicht produziert Überprüfung** | [Einrichten von staging-Umgebungen von Web apps in Azure-App-Verwaltungsdienst][staging] <p>Umsteigen von staging zu Herstellung auch eine Web-app wird die Konfiguration WordPress verschoben. Sie sollten sicherstellen, dass alle Einstellungen auf die Anforderungen für Ihre app Herstellung aktualisiert werden, bevor Sie die bereitgestellte app in Betrieb wechseln.</p>
**Für die Überwachung und Problembehandlung** | [Aktivieren der Protokollierung der Diagnose von Web apps in Azure-App-Verwaltungsdienst] [ log] und [Überwachen der Web Apps in Azure-App-Verwaltungsdienst][monitor]
**Bereitstellen von Ihrer Website** | [Bereitstellen einer Web-app in Azure-App-Verwaltungsdienst][deploy]

#### <a name="availability-and-disaster-recovery"></a>Verfügbarkeit und Disaster Wiederherstellung

Aktion... | Verwenden Sie diese Option...
------------------------|-----------
**Laden Saldo Websites** oder **Geo-Websites verteilen** | [Weiterleitung von Verkehr mit Azure Datenverkehr Manager][trafficmanager]
**Sichern und Wiederherstellen** | [Sichern einer Web app im App-Verwaltungsdienst Azure] [ backup] und [Wiederherstellen eine Web-app in Azure-App-Verwaltungsdienst][restore]

#### <a name="performance"></a>Leistung

Leistung in der Cloud hauptsächlich über Zwischenspeichern und Skalierung erzielt; jedoch Speicher, Bandbreite und andere Attribute Web Apps Hostinganbieter berücksichtigt werden sollen.

Aktion... | Verwenden Sie diese Option...
------------------------|-----------
**Verstehen der App-Service-Instanz-Funktionen** | [Informationen zur Preisgestaltung, einschließlich der App-Dienst leisten-Funktionen][websitepricing]
**Cache-Ressourcen** | [Cache redis][rediscache], [Den Memcache Cloud](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)oder eine der anderen Zwischenspeichern Angebote im [Azure Store](/gallery/store/)
**Skalieren Sie Ihrer Anwendung** | [Eine Web app im App-Verwaltungsdienst Azure skalieren] [ websitescale] und [ClearDB hohen Verfügbarkeit Routing][cleardbscale]. Wenn Sie auswählen, um zu hosten und Verwalten Ihrer eigenen MySQL-Installation, sollten Sie [MySQL Cluster CGE] [ cge] für Skalierung

#### <a name="migration"></a>Migration

Es gibt zwei Methoden zum Migrieren von einer vorhandenen WordPress Website Azure-App-Dienst aus.

* ** [WordPress exportieren] [ export] ** -exportiert den Inhalt Ihres Blogs, die dann in eine neue Website WordPress Azure-App-Diensts mithilfe des das [WordPress Importer-Plug-in]importiert werden kann[import].

    > [AZURE.NOTE] Während dieses Verfahren zum Migrieren von Inhalten kann, migriert es keine-Plug-Ins, Designs oder weitere Anpassungen. Diese müssen erneut manuell installiert sein.

* **Manuellen Migration** - [Sichern Ihrer Website] [ wordpressbackup] und [Datenbank][wordpressdbbackup], dann manuell wiederherstellen, um eine Web app im App-Verwaltungsdienst Azure und verknüpften MySQL-Datenbank migrieren hochgradig angepasste Websites und um zu vermeiden der zeitlichen Aufwand-Plug-Ins, Designs und andere Anpassungen manuell zu installieren.

## <a name="step-by-step-instructions"></a>Eine schrittweise Anleitung

### <a name="create-a-wordpress-site"></a>Erstellen einer Website WordPress

1. Verwenden der [Azure Marketplace] [ cdbnstore] der Größe eine MySQL-Datenbank zu erstellen, die Sie im Abschnitt [Architektur und Planung](#planning) in der Region, dass Sie Ihre Website gehostet werden ermittelt.

2. Führen Sie die Schritte in [Erstellen einer WordPress Web app im App-Verwaltungsdienst Azure] [ createwordpress] zum Erstellen einer WordPress Web app. Wenn Sie die Web-app erstellen, wählen Sie **Verwenden einer vorhandenen MySQL-Datenbank** , und wählen Sie die Datenbank, die in Schritt 1 erstellt.

Wenn Sie eine vorhandene WordPress Website migrieren, finden Sie unter [Migrieren einer vorhandenen WordPress-Website zum Azure](#Migrate-an-existing-WordPress-site-to-Azure) nach dem Erstellen einer neuen Web app.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migrieren einer vorhandenen WordPress-Website zu Azure

Wie im Abschnitt [Architektur und Planung](#planning) erwähnt, gibt es zwei Methoden zum Migrieren einer Website WordPress.

* **Exportieren und importieren** - Websites ohne viel Anpassung, oder die Stelle, an der Sie nur den Inhalt verschieben möchten.

* **Sichern und Wiederherstellen** - Websites mit zahlreiche Anpassung, in dem Sie alles verschieben möchten.

Verwenden Sie eine der in den folgenden Abschnitten an, um Ihre Website migrieren.

#### <a name="the-export-and-import-method"></a>Exportieren und importieren-Methode

1. Verwenden Sie [WordPress exportieren] [ export] So exportieren Sie Ihre vorhandene Website.

2. Erstellen einer Web-app verwenden die Schritte im Abschnitt [Erstellen einer Website WordPress](#Create-a-new-WordPress-site) an.

3. Melden Sie sich bei Ihrer Website WordPress auf Web Apps, und klicken Sie auf **-Plug-Ins** -> **Neu hinzufügen**. Suchen nach, und die **WordPress Importer** -Plug-Ins zu installieren.

4. Nachdem die Importer-Plug-In installiert wurde, klicken Sie auf **Extras** -> **Importieren**, und wählen Sie dann **WordPress** der WordPress Importer-Plug-in verwenden.

5. Klicken Sie auf der Seite **Importieren WordPress** auf **Datei auswählen**. Navigieren Sie zu der WXR-Datei aus Ihrer vorhandenen WordPress Website exportiert, und wählen Sie die **Datei hochladen und importieren**.

6. Klicken Sie auf **Absenden**. Sie werden aufgefordert, dass der Import erfolgreich war.

8. Nachdem Sie alle diese Schritte ausgeführt haben, Starten Ihrer Website aus den Web app Blade im [Portal Azure][mgmtportal].

Nach dem Import der Website, müssen Sie möglicherweise zum Aktivieren der Einstellungen, die nicht in der Importdatei enthalten die folgenden Schritte ausführen.

Wenn Sie dieses Protokoll verwendet wurden... | Aktion
------------------ | ----------
**Permalinks** | Klicken Sie auf **Einstellungen**, aus dem Dashboard WordPress der neuen Website, -> **Permalinks** und die Struktur Permalinks aktualisieren
**Image-Medium links** | Verwenden Sie zum Aktualisieren Links an die neue Position der [decken Blau aktualisieren URLs-Plug-Ins][velvet], eines Tools suchen und ersetzen oder manuell in der Datenbank
**Designs** | Wechseln Sie zu **Darstellung** -> **Design** und aktualisieren Sie das Design der Site Bedarf
**Menüs** | Wenn Sie das Design Menüs unterstützt, können Links zur Startseite Ihrer alte untergeordnete Verzeichnis eingebettet in diese noch. Wechseln Sie zu **Darstellung** -> **Menüs** und aktualisieren sie

#### <a name="the-backup-and-restore-method"></a>Die Methode zum Sichern und Wiederherstellen

1. Sichern Sie Ihre vorhandene WordPress-Website unter Verwendung der Informationen in [WordPress Sicherungskopien][wordpressbackup].

2. Sichern Sie Ihre vorhandene Datenbank mit den Informationen in [Ihrer Datenbank sichern][wordpressdbbackup].

3. Erstellen einer Datenbank, und stellen Sie die Sicherung wieder her.

    1. Erwerben Sie eine neue Datenbank aus dem [Azure Marketplace][cdbnstore], oder die Einrichtung einer MySQL-Datenbank auf einem [Windows] [ mysqlwindows] oder [Linux] [ mysqllinux] virtuellen Computer.

    2. Mit einer MySQL-Client wie [MySQL Arbeitsfläche][workbench], Verbinden mit der neuen Datenbank und Ihrer WordPress-Datenbank importieren.

    3. Aktualisieren Sie die Datenbank aus, um die Domäneneinträge in die neue App-Verwaltungsdienst Azure-Domäne zu ändern. Beispielsweise mywordpress.azurewebsites.net. Verwenden Sie die [Suchen und Ersetzen von WordPress Datenbanken Skript] [ searchandreplace] sicheres alle Instanzen ändern.

4. Erstellen einer Web app im Azure-Portal und die Sicherung WordPress veröffentlichen.

    1. Erstellen eine Web app im [Portal Azure] [ mgmtportal] mit einer Datenbank mit **neu** -> **Web + Mobile** -> **Azure Marketplace** -> **Web Apps** -> **Web app + SQL** (oder **Web app + MySQL**) -> **Erstellen**. Konfigurieren Sie die erforderlichen Einstellungen zum Erstellen einer leeren Web app an.

    2. Klicken Sie in die Sicherungskopie der WordPress suchen Sie die Datei **wp-config.php** , und öffnen sie in einem Editor. Ersetzen Sie die folgenden Einträge mit den Informationen für die neue MySQL-Datenbank.

        * **DB_NAME** - den Benutzernamen, der Datenbank

        * **DB_USER** - den Benutzernamen, der Zugriff auf die Datenbank verwendet

        * **DB_PASSWORD** - das Benutzerkennwort

        Nach dem ändern die folgenden Einträge, speichern Sie und schließen Sie die Datei **wp-config.php** .

    3. Verwenden Sie das [Bereitstellen einer Web app im App-Verwaltungsdienst Azure] [ deploy] Informationen zum Aktivieren der Methode der Bereitstellung verwenden, und klicken Sie dann die Sicherungskopie der WordPress bereitstellen, für die Web-app im App-Verwaltungsdienst Azure werden soll.

5. Nachdem die WordPress Website bereitgestellt wurde, sollten Sie auf die neue Website (als einer App-Dienst Web app) mit zugreifen werden die *. azurewebsite.net-URL für die Website.

### <a name="configure-your-site"></a>Konfigurieren der Website

Nachdem die Website WordPress erstellt oder migriert wurde, verwenden Sie die folgende Informationen zum Verbessern der Leistung oder zusätzliche Funktionen aktivieren aus.

Aktion... | Verwenden Sie diese Option...
------------- | -----------
**Festlegen von App-Dienst Plan Modus, Größe und Skalierung aktivieren** | [Eine Web app im App-Verwaltungsdienst Azure skalieren][websitescale]
**Beständige Datenbankverbindungen aktivieren** | Standardmäßig verwendet WordPress nicht beständige Datenbankverbindungen, der eine Verbindung mit der Datenbank, um nach mehreren Verbindungen gedrosselt werden könnte. Installieren Sie zum beständige Verbindungen aktivieren (beständigen Verbindungen Netzwerkadapter Plug-in) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Verbessern der Leistung** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Deaktivieren das Cookie ARR</a> - können Verbessern der Leistung bei WordPress auf mehrere Instanzen von Web Apps ausgeführt</p></li><li><p>Aktivieren Sie das Zwischenspeichern. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis cache</a> (Preview) kann mit der <a href="https://wordpress.org/plugins/redis-object-cache/">Redis Objekt Cache WordPress-Plug-Ins</a>, oder verwenden Sie eine der anderen Zwischenspeichern Angebote aus dem <a href="/gallery/store/">Azure Store</a> verwendet werden</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">So stellen Sie WordPress schneller mit Wincache</a> - Wincache ist für Web Apps standardmäßig aktiviert.</p></li><li><p><a href="../web-sites-scale/">Skalieren einer Web-app in Azure-App-Dienst</a> und verwenden <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB hohen Verfügbarkeit Routing</a> oder <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a></p></li></ul>
**Verwenden von Blobs zum Speichern** | <ol><li><p><a href="../storage-create-storage-account/">Erstellen Sie ein Konto Azure-Speicher</a></p></li><li><p>Informationen zum Verwenden <a href="../cdn-how-to-use/">Der Content Verteilung Network (CDN)</a> Geo-verteilen in Blobs gespeicherte Daten.</p></li><li><p>Installieren Sie und konfigurieren Sie der <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure-Speicher für WordPress-Plug-in</a>.</p><p>Detaillierte Setup- und Konfigurationsinformationen für das Plug-in finden Sie im <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">Benutzerhandbuch</a>.</p> </li></ol>
**Aktivieren von e-Mail** | Aktivieren Sie <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> mit dem Azure-Speicher. Installieren Sie das <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid-Plug-in</a> für WordPress.
**Konfigurieren Sie einen benutzerdefinierten Domänennamen** | [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure-App-Verwaltungsdienst][customdomain]
**Aktivieren Sie HTTPS für einen benutzerdefinierten Domänennamen** | [Aktivieren Sie HTTPS für eine Web-app in Azure-App-Verwaltungsdienst][httpscustomdomain]
**Laden Sie Saldo oder Geo-verteilen Ihrer Website** | [Datenverkehr mit Azure Datenverkehr Manager][trafficmanager]. Wenn Sie eine benutzerdefinierte Domäne verwenden, finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure-App-Verwaltungsdienst] [ customdomain] Informationen zur Verwendung von Datenverkehr Manager mit benutzerdefinierten Domänennamen
**Aktivieren Sie automatische Sicherungskopien** | [Sichern einer Web-app in Azure-App-Verwaltungsdienst][backup]
**Diagnoseprotokollierung aktivieren** | [Aktivieren Sie Diagnoseprotokolle für Web apps in Azure-App-Verwaltungsdienst][log]

## <a name="next-steps"></a>Nächste Schritte

* [WordPress Optimierung](http://codex.wordpress.org/WordPress_Optimization)

* [Konvertieren von WordPress in mehreren Sites in Azure App-Verwaltungsdienst](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB upgrade-Assistent für Azure](http://www.cleardb.com/store/azure/upgrade)

* [Hostinganbieter WordPress in einem Unterordner des Web app in Azure-App-Verwaltungsdienst](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Schrittweise: Erstellen einer mit Azure WordPress-Website](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Hosten Sie Ihrer vorhandenen WordPress Blog auf Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Aktivieren der Tabellenteil Permalinks in WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [So migrieren, und führen Ihrem Blog WordPress Azure-App-Diensts](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Ausführen von WordPress kostenlos Azure-App-Diensts](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress auf Azure in zwei Minuten oder weniger](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Verschieben eines Blogs WordPress in Azure - Teil 1: Erstellen eines Blogs WordPress Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Verschieben eines Blogs WordPress in Azure - Teil 2: Übertragen von Inhalten](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Verschieben eines Blogs WordPress in Azure - Teil 3: Einrichten Ihrer benutzerdefinierten Domäne](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Verschieben eines Blogs WordPress in Azure - Teil 4: ziemlich Permalinks und Schreiben Sie URL-Regeln](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Verschieben eines Blogs WordPress in Azure - Teil 5: aus einem Unterordner werden im Stammordner verschieben](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Informationen zum Einrichten einer WordPress Web app in Ihrem Azure-Konto](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping von WordPress auf Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Tipps für WordPress Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
