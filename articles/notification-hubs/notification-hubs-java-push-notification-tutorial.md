<properties 
    pageTitle="Verwenden Sie die Benachrichtigung Hubs mit Java" 
    description="Informationen Sie zum Verwenden von Azure Benachrichtigung Hubs aus einem Java-Back-End." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="java" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-java"></a>So verwenden Sie die Benachrichtigung Hubs von Java
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
In diesem Thema werden die wichtigsten Features von den neuen vollständig unterstützte offizielle Azure Benachrichtigung Hub Java SDK. Dies ist ein open Source-Projekt, und Sie können den gesamten SDK Code bei [Java SDK]anzeigen. 

Im Allgemeinen können Sie ein Java/PHP/Python/Ruby Back-End beschriebenen im MSDN-Thema [Benachrichtigung Hubs REST-APIs](http://msdn.microsoft.com/library/dn223264.aspx)der Benachrichtigung Hub REST Schnittstelle alle Benachrichtigung Hubs Features zugreifen. Diese Java SDK stellt einen dünnen Wrapper über diese REST-Schnittstellen in Java. 

Das SDK unterstützt derzeit aus:

- Klicken Sie auf Benachrichtigung Hubs CRUD 
- Klicken Sie auf Registrierungen CRUD
- Installation Management
- Import/Export-Registrierungen
- Normale sendet
- Geplante sendet
- Asynchrone Operationen über Java NIO
- Unterstützte Plattformen: APNS (iOS), GCM (Android), WNS (Windows Store-apps), MPNS (Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android ohne Google-Diensten) 

## <a name="sdk-usage"></a>SDK Verwendung

### <a name="compile-and-build"></a>Kompilieren und erstellen

Verwenden von [Maven]

So erstellen Sie:

    mvn package

## <a name="code"></a>Code

### <a name="notification-hub-cruds"></a>Benachrichtigung Hub CRUDs

**Erstellen Sie einen NamespaceManager ein:**
    
    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**Benachrichtigung Hub zu erstellen:**
    
    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);
    
 ODER

    hub = new NotificationHub("connection string", "hubname");

**Erste Benachrichtigung-Hub an:**
    
    hub = namespaceManager.getNotificationHub("hubname");

**Benachrichtigung Hub zu aktualisieren:**
    
    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Benachrichtigung Hub zu löschen:**
    
    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Registrierung CRUDs
**Erstellen eines Benachrichtigung Hub Clients an:**

    hub = new NotificationHub("connection string", "hubname");

**Windows-Registrierung zu erstellen:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**IOS-Registrierung zu erstellen:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

Sie können auf ähnliche Weise Registrierungen für Android (GCM), Windows Phone (MPNS) und Kindle Fire (ADM) erstellen.

**Vorlage Registrierungen zu erstellen:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**Erstellen von Registrierungen mit Attribute + Upsert erstellen Muster**

Entfernt Duplikate aufgrund einer beliebigen verloren Antworten, wenn Registrierung Ids auf dem Gerät gespeichert:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Registrierungen zu aktualisieren:**
    
    hub.updateRegistration(reg);

**Registrierungen zu löschen:**
    
    hub.deleteRegistration(regid);

**Abfrage Registrierungen:**

*   **Abrufen von einzelnen Registrierung:**
    
        hub.getRegistration(regid);
    
*   **Erhalten Sie alle Registrierungen Hub an:**
    
        hub.getRegistrations();
    
*   **Abrufen von Registrierungen mit Tag:**
    
        hub.getRegistrationsByTag("myTag");
    
*   **Abrufen von Registrierungen von Kanal:**
    
        hub.getRegistrationsByChannel("devicetoken");

Alle Websitesammlung Abfragen unterstützen $top und Fortsetzung Token.

### <a name="installation-api-usage"></a>Verwendung der Installation API
Installation API ist eine alternative Methode für die Verwaltung der Registrierung. Statt zu verwalten mehrerer Registrierungen ist keine Kleinigkeit und möglicherweise falsch oder ineffizient einfach durchgeführt werden, ist es möglich, eine Einzelinstallation Objekt zu verwenden. Installation enthält alles, was Sie benötigen: Pushbenachrichtigungen Kanal (Gerät Token), Kategorien, Vorlagen, sekundäre Kacheln (für WNS und APNS). Sie benötigen keine rufen Sie den Dienst aus, um mehr erhalten Id – einfach GUID oder ein anderes Kennzeichen generieren, auf Gerät beibehalten und an Ihre Back-End-zusammen mit Pushbenachrichtigungen Kanal (Gerät Token). Klicken Sie auf die Back-End-sollte nur einen einzelnen Anruf führen: CreateOrUpdateInstallation, es ist vollständig Idempotent, also gerne wiederholen Sie bei Bedarf.

Als Beispiel für Amazon Kindle Fire sieht er wie folgt aus:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Wenn Sie es aktualisieren möchten: 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

Für erweiterte Szenarien können wir teilweise Aktualisieren der nur bestimmte Eigenschaften des Objekts Installation ändern können. Teilweise Update ist im Grunde Teilmenge der JSON-Patch Vorgänge, die Sie für die Installation Objekt ausführen können.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Installation zu löschen:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, Patch und löschen stimmen sich später mit abrufen. Der Vorgang, die während des Anrufs an die Systemwarteschlange geht einfach und im Hintergrund ausgeführt wird. Beachten Sie, dass Get dient nicht für Hauptfenster Laufzeit Szenario jedoch nur bei Debuggen und Problembehandlung, es eng vom Dienst beschränkt.

Senden von Fluss für Installationen entspricht der Registrierungen. Wir haben nur eine Option zum Adressieren Benachrichtigung spezielle Installation – einfach verwenden Kategorie eingeführt "InstallationId: {gewünscht-Id}". Für die oben genannten Fall würden sie wie folgt aussehen:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

Für eine mehrere Vorlagen:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>Planen von Benachrichtigungen (für STANDARD Ebene verfügbar)

Dasselbe wie normale senden, aber mit einem zusätzlichen Parameter - ScheduledTime, die besagt, wann Benachrichtigung übermittelt werden sollen. Dienst akzeptiert jeder Stelle der Zeitraum zwischen jetzt + 5 Minuten und jetzt + 7 Tage.

**Planen einer systemeigenen Windows-Benachrichtigung an:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>Importieren/Exportieren (für STANDARD Ebene verfügbar)
Manchmal ist es erforderlich, um Masse Operation gegen Registrierungen ausführen. Normalerweise ist für die Integration mit einem anderen System, oder nur ein großer Fix zu sagen Aktualisieren der Kategorien. Es wird dringend empfohlen, nicht Get-Update-Verkehr zu verwenden, wenn wir Tausende von Registrierungen sprechen empfohlen. Import/Export-Funktion dient dazu das Szenario bedecken. Im Wesentlichen bieten Ihnen eine Zugriff auf einige Blob-Container unter Ihrem Speicherkonto als Quelle für eingehende Daten und einen Speicherort für die Ausgabe ein.

**Senden Sie exportieren Position:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**Übermitteln Sie Importvorgang an:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**Warten Sie, bis das Projekt abgeschlossen ist:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**Abrufen Sie aller Projekte:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**URI mit SAS Signatur:** Dies ist die URL des einige BLOB-Datei oder Blob-Container plus Satz von Parametern wie Berechtigungen und Ablauf Uhrzeit plus Signatur alles versucht, mithilfe des Kontos SAS-Taste. Rich-Funktionen, einschließlich der Erstellung von solche Arten von URIs verfügt Azure-Speicher Java SDK Einfache Alternative können Sie wollen ImportExportE2E Test-Klasse (aus dem Github-Speicherort) verwenden die sehr einfache und compact Implementierung Signieren Algorithmus hat.

###<a name="send-notifications"></a>Benachrichtigungen senden
Das Objekt Benachrichtigung ist einfach ein Textkörper mit Überschriften, einige Methoden Hilfe bei der Erstellung der einheitlichen und Vorlage Benachrichtigungen Objekte.

* **Windows Store und Windows Phone 8.1 (nicht-Silverlight)**

        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);

* **iOS**

        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);

* **Android**

        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);

* **Windows Phone 8.0 und 8.1 Silverlight**

        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);

* **Kindle Fire**

        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);

* **Senden an Kategorien**

        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);

* **Senden an den Tag-Ausdruck**       

        hub.sendNotification(n, "foo && ! bar");

* **Vorlage Benachrichtigung senden**

        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

Ausführen von Java-Code sollte jetzt eine Benachrichtigung, die auf Ihrem Zielgerät angezeigte erzeugen.

##<a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Nächste Schritte
In diesem Thema werden so erstellen Sie einen einfachen REST Java-Client für Benachrichtigung Hubs vorgestellt. Von hier aus können Sie folgende Aktionen ausführen:

* Laden Sie die vollständige [Java SDK], die den gesamten SDK Code enthält. 
* Wiedergegeben Sie mit den Beispielen werden:
    - [Erste Schritte mit Benachrichtigung Hubs]
    - [Senden Sie dem neusten Stand]
    - [Auf dem neusten lokalisiert senden]
    - [Senden von Benachrichtigungen für authentifizierte Benutzer]
    - [Plattform-Benachrichtigungen für authentifizierte Benutzer senden]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Erste Schritte mit Benachrichtigung Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Senden Sie dem neusten Stand]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Auf dem neusten lokalisiert senden]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Senden von Benachrichtigungen für authentifizierte Benutzer]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Senden von Benachrichtigungen Plattformen für authentifizierte Benutzer]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/
 
