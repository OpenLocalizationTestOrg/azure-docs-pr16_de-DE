<properties
    pageTitle="Migrieren von mobilen Diensten in einer App-Dienst Mobile-App"
    description="Erfahren Sie, wie Sie ganz einfach Ihre Mobile Services-Anwendung zu einer App-Dienst Mobile-App migrieren"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="a-namearticle-topamigrate-your-existing-azure-mobile-service-to-azure-app-service"></a><a name="article-top"></a>Migrieren von Ihrer vorhandenen Azure Mobile Service zu Azure-App-Verwaltungsdienst

Mit der [allgemeinen Verfügbarkeit der App-Verwaltungsdienst Azure]können Azure Mobile Dienste Websites einfach zu nutzen, der alle Features von der App-Verwaltungsdienst Azure in-situ migriert werden.  Dieses Dokument wird erläutert, was bei Ihrer Website aus Azure Mobile-Dienste zu Azure-App-Verwaltungsdienst Migrieren zu erwarten.

## <a name="a-namewhat-does-migration-doawhat-does-migration-do-to-your-site"></a><a name="what-does-migration-do"></a>Welche Funktion hat die Migration – zu Ihrer Website?

Migration von Ihrer Azure Mobile Service verwandelt Ihre Mobile Service in einer [App-Verwaltungsdienst Azure] -app, ohne den Code.  Ihre Benachrichtigung Hubs, SQL-Datenverbindung, Authentifizierungseinstellungen, geplanter Aufträge und Domänennamen bleiben unverändert.  Mobile Clients mithilfe Ihrer Azure Mobile Service weiterhin normal ausgeführt werden.  Migration den Dienst gestartet, sobald sie an der App-Verwaltungsdienst Azure übertragen werden.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="a-namewhy-migrateawhy-you-should-migrate-your-site"></a><a name="why-migrate"></a>Warum sollten Sie Ihre Website migrieren.

Microsoft ist empfehlen daher Migrieren von Ihrer Azure Mobile Service zum Nutzen der Features von Azure App-Verwaltungsdienst, einschließlich:

  *  Neue Host-Funktionen, einschließlich [WebJobs] und [benutzerdefinierten Domänennamen].
  *  Die Verbindung zu Ressourcen lokal [VNet] zusätzlich [Hybrid-Verbindungen]verwenden.
  *  Für die Überwachung und Behandlung von Problemen mit neuen Relic oder [Anwendung Einsichten].
  *  Integrierte DevOps-Tools, einschließlich [staging Steckplätze], Rollup-zurück, und in der Herstellung zu testen.
  *  [Automatische Skalierung], den Lastenausgleich und [Leistung zu überwachen].

Weitere Informationen zu den Vorteilen von App-Verwaltungsdienst Azure finden Sie im [Mobile im Vergleich zu App Service] -Thema.

## <a name="a-namebefore-you-beginabefore-you-begin"></a><a name="before-you-begin"></a>Vorbemerkung

Bevor Sie beginnen etwaige wichtigsten Änderungen auf Ihrer Website, sollten Sie [Ihre Mobile Service sichern] Skripts und SQL-Datenbank.

## <a name="a-namemigrating-siteamigrating-your-sites"></a><a name="migrating-site"></a>Migrieren Ihrer Websites

Des Migrationsvorgangs migriert werden alle Websites in einem einzigen Azure-Bereich.

Migrieren von Ihrer Website:

  1.  Melden Sie sich bei der [Azure klassischen Portal].
  2.  Wählen Sie einen Mobile-Dienst in der Region, die, das Sie migrieren möchten.
  3.  Klicken Sie auf die Schaltfläche **Migrieren App-Dienst** .

    ![Die Schaltfläche "migrieren"][0]

  4.  Informationen zum Migrieren in die im Dialogfeld App-Dienst an.
  5.  Geben Sie den Namen Ihres Mobile-Diensts in das bereitgestellte Feld ein.  Beispielsweise ist Ihren Domänennamen ein contoso.azure-mobile.net, geben Sie dann _"Contoso"_ in das bereitgestellte Feld ein.
  6.  Klicken Sie auf die Schaltfläche mit dem Häkchen.

Überwachen Sie den Status der Migration in die Aktivität überwachen. Ihre Website wird als im klassischen Azure-Portal *Migrieren* aufgeführt.

  ![Migration Aktivitätsmonitor][1]

Jeder Migration kann zwischen 3 und pro mobile Service migrierende 15 Minuten dauern.  Ihre Website bleibt während der Migration verfügbar.
Ihre Website wird am Ende des Migrationsvorgangs neu gestartet.  Die Website ist beim Starten, die ein paar Sekunden dauern, möglicherweise nicht verfügbar.

## <a name="a-namefinalizing-migrationafinalizing-the-migration"></a><a name="finalizing-migration"></a>Abschließen der Migrations

So testen Sie Ihre Website aus einem mobilen Client nach Abschluss des Migrationsvorgangs planen.  Stellen Sie sicher, dass Sie alle ohne Änderungen an den mobilen Client allgemeine Clientaktionen ausführen können.  

### <a name="a-nameupdate-app-service-tieraselect-an-appropriate-app-service-pricing-tier"></a><a name="update-app-service-tier"></a>Wählen Sie einen entsprechenden Preise Ebene App-Dienst

Sie haben eine größere Flexibilität beim Preise, nachdem Sie in der App-Verwaltungsdienst Azure migriert.

  1.  Melden Sie sich mit dem [Azure-Portal]an.
  2.  Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3.  Standardmäßig wird das Blade Einstellungen geöffnet.
  4.  Klicken Sie im Menü Einstellungen auf **App Dienst planen** .
  5.  Klicken Sie auf die Kachel **Preise Ebene** .
  6.  Klicken Sie auf die Kachel angebracht, die Ihren Anforderungen, und klicken Sie auf **auswählen**.  Möglicherweise müssen Sie auf **Alle anzeigen** , um die verfügbaren Preisgestaltung finden Sie unter leisten.

Als Ausgangspunkt empfehlen wir die folgenden Ebenen aus:

| Mobile Service Preise Ebene | App-Dienst Preise Ebene |
| :-------------------------- | :----------------------- |
| Kostenlose                        | F1 kostenlose                  |
| Grundlegende                       | B1 Basic                 |
| Standard                    | S1 Standard              |

Es gibt dazu Flexibilität in die richtigen Preise Ebene für eine Anwendung auszuwählen.  Alle Details für die Preise von den neuen App-Dienst finden Sie unter [App Preisen] .

> [AZURE.TIP]Die App-Dienst Standard Ebene enthält Zugriff auf viele Funktionen, die Sie verwenden möchten, einschließlich [staging Steckplätze], automatischen Sicherung und automatische Skalierung.  Sehen Sie sich die neuen Funktionen, während Sie es sind!

### <a name="a-namereview-migration-scheduler-jobsareview-the-migrated-scheduler-jobs"></a><a name="review-migration-scheduler-jobs"></a>Überprüfen Sie die migrierte Scheduler Aufträge

Scheduler Aufträge ist erst ungefähr 30 Minuten nach der Migration werden nicht angezeigt.  Geplante Aufträge weiterhin im Hintergrund ausgeführt.
Geplanter Aufträge anzeigen, nachdem sie erneut sichtbar sind:

  1.  Melden Sie sich mit dem [Azure-Portal]an.
  2.  Wählen Sie **Durchsuchen >**, geben Sie in das Feld _Filter_ ein **Terminplan** ein, und wählen Sie dann **Scheduler Websitesammlungen**.

Es gibt eine begrenzte Anzahl von kostenlosen Scheduler Aufträge verfügbar nach der Migration.  Überprüfen Sie Ihre Verwendung und die [Azure Scheduler Pläne].

### <a name="a-nameconfigure-corsaconfigure-cors-if-needed"></a><a name="configure-cors"></a>Konfigurieren von CORS, sofern erforderlich

Gemeinsames Nutzen von Ressourcen Cross-Origin ist ein Verfahren für eine Website für den Zugriff auf eine andere Domäne eine Web-API auf zulassen.  Wenn Sie mit der entsprechenden Website Azure Mobile-Dienste verwenden, müssen Sie CORS als Teil der Migration zu konfigurieren.  Wenn Sie Azure Mobile Dienste ausschließlich von mobilen Geräten aus zugreifen möchten, klicken Sie dann muss CORS nicht außer in Ausnahmefällen konfiguriert sein.

Die Einstellungen für Ihr migrierten CORS sind als der App-Einstellung **MS_CrossDomainWhitelist** verfügbar.  Zu Ihrer Website zu der App-Dienst CORS Einrichtung migrierende Elemente:

  1.  Melden Sie sich mit dem [Azure-Portal]an.
  2.  Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3.  Standardmäßig wird das Blade Einstellungen geöffnet.
  4.  Klicken Sie im Menü API auf **CORS** .
  5.  Geben Sie eine zulässige Ursprung in das bereitgestellte Feld ein, nach der jeweils die EINGABETASTE drücken.
  6.  Nachdem Sie Ihre Liste der zulässige Ursprung korrekt ist, klicken Sie auf die Schaltfläche speichern.

> [AZURE.TIP]Einer der Vorteile der Verwendung einer App-Verwaltungsdienst Azure ist, dass Sie Ihre Website und mobile Service auf derselben Website ausführen können.  Weitere Informationen finden Sie im Abschnitt für die [nächsten Schritte](#next-steps) .

### <a name="a-namedownload-publish-profileadownload-a-new-publishing-profile"></a><a name="download-publish-profile"></a>Laden Sie ein neues Profil für die Veröffentlichung

Bei der Migration zu Azure-App-Verwaltungsdienst, wird das Profil für die Veröffentlichung Ihrer Website geändert.  Wenn Sie beabsichtigen, veröffentlichen Ihre Website von in Visual Studio, benötigen Sie ein neues Profil für die Veröffentlichung.  Das neue publishing Profil herunterladen zu können:

  1.  Melden Sie sich mit dem [Azure-Portal]an.
  2.  Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3.  Klicken Sie auf die **erste Profil veröffentlichen**.

Die PublishSettings-Datei wird auf Ihren Computer heruntergeladen.  Es ist normalerweise _Sitename_aufgerufen. PublishSettings.  Importieren Sie die Einstellungen Veröffentlichen in Ihr vorhandenes Projekt aus:

  1.  Öffnen Sie Visual Studio und Projekt Azure Mobile Service.
  2.  Mit der rechten Maustaste in Ihrem Projekts in der **Lösung-Explorer** , und wählen Sie **veröffentlichen...**
  3.  Klicken Sie auf **Importieren**
  4.  Klicken Sie auf **Durchsuchen** , und wählen Sie aus der heruntergeladenen Einstellungsdatei veröffentlichen.  Klicken Sie auf **OK**
  5.  Klicken Sie auf **Überprüfen Sie die Verbindung** , um die Arbeit veröffentlichen Einstellungen sicherzustellen.
  6.  Klicken Sie auf **Veröffentlichen** , um Ihre Website veröffentlichen.


## <a name="a-nameworking-with-your-siteaworking-with-your-site-post-migration"></a><a name="working-with-your-site"></a>Arbeiten mit Ihrer nach der Websitemigration

Arbeiten mit den neuen App-Dienst in der [Azure-Portal] nach der Migration zu starten.  Es folgen einige Hinweise auf bestimmte Vorgänge, die Sie in der [Klassischen Azure-Portal]zusammen mit ihren App-Dienst Entsprechung ausführen verwendet.

### <a name="a-namepublishing-your-siteadownloading-and-publishing-your-migrated-site"></a><a name="publishing-your-site"></a>Herunterladen und migrierte Veröffentlichungswebsite

Ihre Website ist über Git oder ftp verfügbar und kann mit verschiedenen anderen Verfahren, einschließlich WebDeploy, TFS, weshalb, GitHub und FTP erneut veröffentlicht werden.  Die Bereitstellung Anmeldeinformationen werden mit den Rest der Website migriert.  Wenn Sie Ihre Bereitstellung Anmeldeinformationen nicht festgelegt haben oder nicht können erinnern können, können Sie diese zurückzusetzen:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3. Standardmäßig wird das Blade Einstellungen geöffnet.
  4. Klicken Sie auf **Bereitstellung Anmeldeinformationen** in der Veröffentlichung Menü.
  5. Geben Sie die neuen Anmeldeinformationen für die Bereitstellung in die Textfelder, und klicken Sie auf die Schaltfläche speichern.

Diese Anmeldeinformationen können Sie die Website mit Git Klonen oder automatisierte Bereitstellungen von GitHub, TFS oder weshalb einrichten.  Weitere Informationen finden Sie in der [Dokumentation der App-Verwaltungsdienst Azure-Bereitstellung].

### <a name="a-nameappsettingsaapplication-settings"></a><a name="appsettings"></a>Anwendungseinstellungen

Die meisten Einstellungen für eine migrierte mobile Service sind verfügbar über App-Einstellungen.  Sie können eine Übersicht der Einstellungen für die app aus dem [Azure-Portal]erhalten.
Anzeigen oder Ändern der Appeinstellungen für die:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3. Standardmäßig wird das Blade Einstellungen geöffnet.
  4. Klicken Sie im Menü "Allgemein" auf **Einstellungen für die Anwendung** .
  5. Führen Sie einen Bildlauf zum Abschnitt Einstellungen für die App, und suchen Sie Ihre app-Einstellung.
  6. Klicken Sie auf den Wert für die app-Einstellung, um den Wert zu bearbeiten.  Klicken Sie auf **Speichern** , um den Wert zu speichern.

Sie können mehrere Einstellungen für die app zur gleichen Zeit aktualisieren.

> [AZURE.TIP]Es gibt zwei Anwendungseinstellungen mit demselben Wert aus.  Angenommen, Sie möglicherweise finden Sie unter _Anwendungsschlüssel_ und _MS\_Anwendungsschlüssel_.  Aktualisieren Sie beide Anwendungseinstellungen zur gleichen Zeit ein.

### <a name="a-nameauthenticationaauthentication"></a><a name="authentication"></a>Authentifizierung

Alle Authentifizierungseinstellungen sind als App-Einstellungen auf Ihrer Website migrierte verfügbar.  Klicken Sie zum Aktualisieren der Authentifizierungseinstellungen für die müssen Sie die entsprechenden app-Einstellungen ändern.  Die folgende Tabelle zeigt die entsprechenden app-Einstellungen für Ihren Authentifizierungsanbieter:

| Anbieter          | Client-ID                 | Geheim Client                | Andere Einstellungen             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft-Konto | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Hinweis: **MS\_AadTenants** wird als eine durch Trennzeichen getrennte Liste der Mandanten Domänen (die Felder "Zulässige Mandanten" im Portal Mobile) gespeichert.

> [AZURE.WARNING] **Verwenden Sie nicht die Authentifizierungsmechanismen im Menü Einstellungen**
>
> Azure-App-Dienst bietet ein separates "ohne Code" Authentifizierung und Autorisierung unter der _Authentifizierung / Autorisierung_ Menü Einstellungen und anschließend die (abgelehnt) _Mobile Authentifizierung_ Option im Menü "Einstellungen".  Diese Optionen sind eine migrierte Azure Mobile Service inkompatibel.  Sie können [Ihrer Website zu aktualisieren](app-service-mobile-net-upgrading-from-mobile-services.md) , um die App-Verwaltungsdienst Azure-Authentifizierung nutzen.

### <a name="a-nameeasytablesadata"></a><a name="easytables"></a>Daten

Die Registerkarte ' _Daten_ ' Mobile-Dienste wurde durch _Einfache Tabellen_ innerhalb des Portals Azure ersetzt.  Auf einfache Tabellen zugreifen zu können:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3. Standardmäßig wird das Blade Einstellungen geöffnet.
  4. Klicken Sie im Menü mobilen auf **einfache Tabellen** .

Sie können eine Tabelle hinzufügen, indem Sie auf die Schaltfläche **Hinzufügen** oder Ihrer vorhandenen Tabellen zugreifen, indem Sie auf einen Tabellennamen.  Es gibt verschiedene Vorgänge Sie aus diesem Blade ausführen können, einschließlich:

* Ändern der Tabellenberechtigungen für die
* Bearbeiten der Betrieb Skripts
* Verwalten des Tabellenschemas
* Löschen der Tabelle
* Löschen des Inhalts der Tabelle
* Löschen von bestimmter Zeilen der Tabelle

### <a name="a-nameeasyapisaapi"></a><a name="easyapis"></a>API

Die Registerkarte _API_ Mobile-Dienste wurde durch _Einfache APIs_ innerhalb des Portals Azure ersetzt.  Um einfache APIs zuzugreifen:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3. Standardmäßig wird das Blade Einstellungen geöffnet.
  4. Klicken Sie **Einfach APIs** MOBILE im Menü auf.

Ihre migrierte APIs sind bereits in der Blade aufgeführt.  Sie können auch eine API aus diesem Blade hinzufügen.  Um eine bestimmte API verwalten möchten, klicken Sie auf die-API.
Aus dem neuen Blade können Sie die Berechtigungen anpassen und bearbeiten die Skripts für die-API.

### <a name="a-nameon-demand-jobsascheduler-jobs"></a><a name="on-demand-jobs"></a>Scheduler Aufträge

Alle Scheduler Aufträge sind über Abschnitt Scheduler Auftrag Websitesammlungen verfügbar.  Um Ihre Aufträge Scheduler zuzugreifen:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **Durchsuchen >**, geben Sie in das Feld _Filter_ ein **Terminplan** ein, und wählen Sie dann **Scheduler Websitesammlungen**.
  3. Wählen Sie den Auftrag Sammlung für Ihre Website.  Es ist mit der Bezeichnung _Sitename_-Aufträge.
  4. Klicken Sie auf **Einstellungen**.
  5. Klicken Sie unter Verwalten auf **Scheduler Aufträge** .

Geplante Aufträge werden mit der Häufigkeit aufgeführt, die Sie vor der Migration angegeben haben.  Bei Bedarf Aufträge werden deaktiviert.  So führen Sie eine Position nach Bedarf:

  1. Wählen Sie den Auftrag, die, den Sie ausführen möchten.
  2. Falls erforderlich, klicken Sie auf **Aktivieren** , um den Auftrag zu aktivieren.
  3. Klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Terminplan**.
  4. Wählen Sie aus einer Serie von **einmal**, und klicken Sie auf **Speichern**

Befinden sich Ihre Aufträge bei Bedarf `App_Data/config/scripts/scheduler post-migration`.  Es empfiehlt sich, alle bei Bedarf Aufträge in [WebJobs] oder - [Funktionen]zu konvertieren.  Schreiben Sie neue Scheduler Aufträge als [WebJobs] oder - [Funktionen].

### <a name="a-namenotification-hubsanotification-hubs"></a><a name="notification-hubs"></a>Benachrichtigung Hubs

Mobile Dienste verwendet Benachrichtigung Hubs für Pushbenachrichtigungen.  Die folgenden Einstellungen für die App werden verwendet, um die Benachrichtigung-Hub an den Mobile-Dienst nach der Migration zu verknüpfen:

| Anwendungseinstellung                    | Beschreibung                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Die Benachrichtigung Hub Namespace           |
| **MS\_NotificationHubName**             | Der Name der Benachrichtigung Hub                |
| **MS\_NotificationHubConnectionString** | Die Benachrichtigung Hub Verbindungszeichenfolge   |
| **MS\_NamespaceName**                   | Ein Alias für MS_PushEntityNamespace      |

Ihre Benachrichtigung Hub wird über das [Azure-Portal]verwaltet.  Beachten Sie den Benachrichtigung Hub Namen (dies finden Sie die App-Einstellungen verwenden):

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **Durchsuchen**>, wählen Sie dann auf **Benachrichtigung Hubs**
  3. Klicken Sie auf die Benachrichtigung Hub Name des mobilen Diensts zugeordnet ist.

> [AZURE.NOTE]Wenn Ihre Benachrichtigung HUb eine "Gemischt" ist, ist es nicht sichtbar.  "Gemischten" Geben Sie die Benachrichtigung, dass Hubs sowohl Benachrichtigung Hubs und ältere Dienstbus-Funktionen nutzen.  [Konvertieren Sie Ihre gemischten Namespaces] , bevor Sie fortfahren.  Sobald die Konvertierung abgeschlossen ist, wird der Benachrichtigung Hub im [Portal Azure]angezeigt.

Weitere Informationen finden Sie auf [Benachrichtigung Hubs] .

> [AZURE.TIP]Benachrichtigung Hubs Verwaltungsfunktionen im [Portal Azure] befinden sich noch auf Vorschau.  Das [Klassische Azure-Portal] weiterhin für die Verwaltung von alle Ihre Benachrichtigung Hubs zur Verfügung.

### <a name="a-namelegacy-pushalegacy-push-settings"></a><a name="legacy-push"></a>Pushbenachrichtigungen Legacy-Einstellungen

Wenn Sie Pushbenachrichtigungen auf Ihrem mobilen Dienst vor der Einführung auf Benachrichtigung Hubs konfiguriert haben, verwenden Sie _legacy Pushbenachrichtigungen_.  Wenn Sie Pushbenachrichtigungen verwenden, und führen Sie eine Benachrichtigung Hub aufgeführt, die in der Konfiguration nicht angezeigt wird, ist es wahrscheinlich Sie _legacy Pushbenachrichtigungen_verwenden.  Dieses Feature wird mit allen anderen Funktionen migriert.  Wir empfehlen jedoch upgrade auf Benachrichtigung Hubs kurz nach Abschluss der Migration.

In der Zwischenzeit stehen die legacy Pushbenachrichtigungen Einstellungen (mit dem Unterschied des Zertifikats APNS) in den App-Einstellungen.  Aktualisieren Sie das Zertifikat APNS, indem die entsprechende Datei im Dateisystem ersetzen.

### <a name="a-nameapp-settingsaother-app-settings"></a><a name="app-settings"></a>Andere Appeinstellungen

Die folgenden zusätzlichen Appeinstellungen werden migrierte aus dem Mobile-Dienst, und klicken Sie unter *Einstellungen*verfügbar > *App-Einstellungen*:

| Anwendungseinstellung              | Beschreibung                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Den Namen der app                    |
| **MS\_MobileServiceDomainSuffix** | Das Präfix Domäne. z. B. Azure-mobile.net |
| **MS\_Anwendungsschlüssel**            | Ihre Anwendungstaste                    |
| **MS\_MasterKey**                 | Ihre app Master-Taste                     |

Der Anwendung und Master Schlüssel sind identisch mit der Anwendung Tasten aus dem ursprünglichen Mobile-Dienst.  Insbesondere wird die ANWENDUNGSTASTE von mobilen Clients gesendet, deren Verwendung der mobilen API überprüft.

### <a name="a-namecliequivalentsacommand-line-equivalents"></a><a name="cliequivalents"></a>Befehlszeilenäquivalente

Mehr können _Azure mobile_ Verwendung des Befehls zum Verwalten Ihrer Website Azure Mobile-Dienste.  Viele Funktionen wurden stattdessen mit dem Befehl _Azure Website_ ersetzt.  Verwenden Sie die folgende Tabelle, um Entsprechung für häufig verwendete Befehle zu ermitteln:

| _Azure Mobile_ Befehl                     | Entspricht _Azure Site_ (Befehl)            |
| :----------------------------------------- | :----------------------------------------- |
| Mobile Speicherorte                           | Speicherort Websiteliste                         |
| Mobile Listen                                | Websiteliste                                  |
| Anzeigen von mobilen _Namen_                         | Anzeigen der Website _Namen_                           |
| Mobile Restart _Namen_                      | Starten der Website _Namen_                        |
| die erneute Bereitstellung mobilen _Namen_                     | Website-Bereitstellung bereitstellen _CommitId_ _Namen_ |
| Mobile Schlüssel legen Sie _Namen_ _Typ_ _Wert_       | _Taste_ _Name_ der Website Appsetting löschen <br/> Website Appsetting hinzufügen _Schlüssel_=_Wert_ _Namen_ |
| Mobile Config Liste _Namen_                  | Liste der Website Appsetting _Namen_                |
| Mobile Config beziehen _Namen_ _Schlüssel_             | _Taste_ _Name_ der Website Appsetting anzeigen          |
| Mobile Config legen Sie _Namen_ _Schlüssel_             | _Taste_ _Name_ der Website Appsetting löschen <br/> Website Appsetting hinzufügen _Schlüssel_=_Wert_ _Namen_ |
| Mobile Domäne Liste _Namen_                  | Liste der Website Domain _name_                    |
| Mobile Domäne hinzufügen _Namen_ _Domäne_          | Website-Domäne hinzufügen _Domain_ _name_            |
| Mobile Domäne löschen _Namen_                | _Domain_ _Name_ der Website Domäne löschen         |
| Mobile Maßstab anzeigen _Namen_                   | Anzeigen der Website _Namen_                           |
| _Namen_ der mobilen Maßstab ändern                 | Website-Skala Modus _Modus_ _name_ <br /> Website-Skala Instanzen _Instanzen_ _Namen_ |
| Mobile Appsetting Liste _Namen_              | Liste der Website Appsetting _Namen_                |
| Mobile Appsetting hinzufügen _ _Schlüssel_ _Wert_ _ | Website Appsetting hinzufügen _Schlüssel_=_Wert_ _Namen_   |
| Mobile Appsetting ENTF _Namen_ - _Taste_      | _Taste_ _Name_ der Website Appsetting löschen        |
| Mobile Appsetting anzeigen _Namen_ _Schlüssel_        | _Taste_ _Name_ der Website Appsetting löschen        |

Aktualisieren Sie Authentifizierung oder Benachrichtigungseinstellungen für Pushbenachrichtigungen durch Aktualisieren die gewünschte Einstellung für die Anwendung ein.
Bearbeiten von Dateien und Veröffentlichen der Website über ftp oder Git.

### <a name="a-namediagnosticsadiagnostics-and-logging"></a><a name="diagnostics"></a>Diagnose und Protokollierung

Normalerweise ist in einer App-Verwaltungsdienst Azure Diagnoseprotokoll deaktiviert.  Diagnoseprotokolle zu aktivieren:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3. Standardmäßig wird das Blade Einstellungen geöffnet.
  4. Wählen Sie im Menü FEATURES **Diagnoseprotokolle** aus.
  5. Klicken Sie **auf** die folgenden Protokolle: **Anwendung Protokollierung (Dateisystem)**, **detaillierte Fehlermeldungen**und **gezeichnet, die Anfrage fehlgeschlagen**
  6. Klicken Sie auf **Dateisystem** für Web-Server-Protokollierung
  7. Klicken Sie auf **Speichern**

So zeigen Sie die Protokolle an:

  1. Melden Sie sich mit dem [Azure-Portal]an.
  2. Wählen Sie **alle Ressourcen** oder **Services App** aus, und klicken Sie dann klicken Sie auf den Namen Ihres migrierte Mobile-Diensts.
  3. Klicken Sie auf die Schaltfläche **Extras**
  4. Wählen Sie im Menü berücksichtigen **Log Stream** aus.

Protokolle werden im Fenster angezeigt, während sie generiert werden.  Sie können auch die Protokolle für die spätere Analyse mit Ihren Anmeldeinformationen für die Bereitstellung herunterladen. Weitere Informationen finden Sie in der Dokumentation [Protokollierung] .

## <a name="a-nameknown-issuesaknown-issues"></a><a name="known-issues"></a>Bekannte Probleme

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Löschen einer Datenbeschriftungsreihe Mobile-App migriert bewirkt, dass ein Ausfall der Website

Ihre migrierten mobilen Service mithilfe der PowerShell Azure klonen, und löschen die klonen, wird der DNS-Eintrag für Ihre Herstellung Dienst entfernt.  Ihre Website ist nicht mehr über das Internet zugänglich sein.  

Lösung: Wenn Sie Ihre Website klonen möchten, tun Sie dies über das Portal.

### <a name="changing-webconfig-does-not-work"></a>Ändern der Web.config funktioniert nicht

Wenn Sie eine ASP.NET-Website haben, ändert sich in der `Web.config` Datei nicht übernommen abrufen.  Die App-Verwaltungsdienst Azure erstellt eine geeignete `Web.config` Datei beim Start auf die Mobile-Dienste Runtime unterstützen.  Sie können bestimmte Einstellungen (beispielsweise benutzerdefinierte Kopfzeilen) mithilfe einer XML-Transformationsdatei überschreiben.  Erstellen Sie eine Datei in aufgerufen `applicationHost.xdt` – dieser Datei einhandeln muss die `D:\home\site` Verzeichnis Azure-Dienst.  Hochladen der `applicationHost.xdt` Datei über einen benutzerdefinierten Bereitstellungsskript oder direkt verwenden Kudu.  Die nachstehende Abbildung zeigt ein Beispieldokument aus:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Weitere Informationen finden Sie in der Dokumentation [XDT transformieren Beispiele] auf GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Migrierte Mobile-Dienste kann mit den Datenverkehr Manager hinzugefügt werden.

Beim Erstellen eines Profils Datenverkehr-Managers können nicht Sie direkt einen migrierten mobilen Dienst für das Profil auswählen.  Verwenden eines "externen Endpunkts".  Der externe Endpunkt kann nur über PowerShell hinzugefügt werden.  Weitere Informationen finden Sie im [Lernprogramm Datenverkehr-Manager](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Anwendung auf App-Dienst migriert wird, gibt es noch mehr Features, die Sie verwenden können:

  * Bereitstellung [staging Steckplätze] ermöglichen es Ihnen, Phaseneigenschaften Änderungen an Ihrer Website, und führen Sie A / B zu testen.
  * [WebJobs] bieten kein Ersatz für Aufträge bei Bedarf geplant.
  * Sie können durch Verknüpfen Ihrer Website GitHub, TFS oder weshalb Ihrer Website [kontinuierlich bereitstellen] .
  * [Anwendung Einsichten] können Sie um Ihrer Website zu überwachen.
  * Sie dienen einer Website und eine Mobile-API auf demselben Code.

### <a name="a-nameupgrading-your-siteaupgrading-your-mobile-services-site-to-azure-mobile-apps-sdk"></a><a name="upgrading-your-site"></a>Aktualisieren die Mobile-Website auf Azure Mobile Apps SDK

  * Für Projekte, Node.js-basierten Server enthält das neue [Mobile Apps Node.js SDK] mehrere neue Funktionen. Beispielsweise können Sie jetzt kann lokale Entwicklung und für das Debuggen, verwenden Sie eine beliebige Version Node.js über 0,10 und mit einem beliebigen Express.js Middleware anpassen.

  * Für. Netz-basierten Serverprojekte, die neuen [Mobile Apps SDK NuGet-Pakete](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) haben größere Flexibilität NuGet Abhängigkeiten.  Diese Pakete unterstützen die neue App-Dienst Authentifizierung, und mit einem beliebigen ASP.NET-Projekt verfassen. Weitere Informationen zum Upgrade finden Sie unter [Upgrade Ihrer vorhandenen .NET Mobile Service App-Dienst](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App-Verwaltungsdienst Preise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Anwendung Einsichten]: ../application-insights/app-insights-overview.md
[Automatische Skalierung]: ../app-service-web/web-sites-scale.md
[Azure App-Verwaltungsdienst]: ../app-service/app-service-value-prop-what-is.md
[Azure Dokumentation der App-Service-Bereitstellung]: ../app-service-web/web-sites-deploy.md
[Azure klassischen-Portal]: https://manage.windowsazure.com
[Azure-portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler-Pläne]: ../scheduler/scheduler-plans-billing.md
[kontinuierlich bereitstellen.]: ../app-service-web/app-service-continuous-deployment.md
[Konvertieren Sie Ihre gemischten namespaces]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[benutzerdefinierten Domänennamen]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[allgemeine Verfügbarkeit von Azure-App-Verwaltungsdienst]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid-Verbindungen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Protokollierung]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobile Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobile Services im Vergleich zur App-Dienst]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Benachrichtigung Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Überwachen der Leistung]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Sichern Sie Ihre Mobile Service]: ../mobile-services/mobile-services-disaster-recovery.md
[Staging Steckplätze]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT Transformation Beispiele]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funktionen]: ../azure-functions/functions-overview.md
