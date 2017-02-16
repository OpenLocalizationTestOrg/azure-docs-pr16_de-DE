<properties 
    pageTitle="Azure WebJobs Dokumentationsressourcen" 
    description="Empfohlene Ressourcen für den Umgang mit Azure WebJobs und Azure WebJobs SDK." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs Dokumentationsressourcen

## <a name="overview"></a>(Übersicht)

Dieses Thema enthält Links zu Dokumentationsressourcen zur Verwendung von Azure WebJobs und Azure WebJobs SDK. Azure WebJobs bieten eine einfache Möglichkeit zum Ausführen von Skripts oder Programme als Hintergrundprozesse im Kontext einer [App-Dienst Web app, API-app oder mobile-app](../app-service/app-service-value-prop-what-is.md). Hochladen, und führen Sie eine ausführbare Datei wie als Cmd, Bat, EXE-Datei (.NET), ps1, sh, Php, hierhin, Js und Jar. Diese Programme führen Sie nach einem Zeitplan (Cron) oder permanent als WebJobs.

Der Zweck des [WebJobs SDK](websites-webjobs-resources.md) ist der Code, den Sie für häufige Aufgaben schreiben, dass eine WebJob ausführen, wie z. B. Image-Verarbeitung, Warteschlange Verarbeitung können, RSS-Aggregation, Wartung und Senden von e-Mails zu vereinfachen. Das WebJobs SDK weist integrierte Features für die Arbeit mit Azure-Speicher und Dienstbus, für die Planung von Vorgängen und Behandeln von Fehlern und viele andere allgemeine Szenarien. Darüber hinaus dieses Konzept erweiterbar sein, und es ist [Quellrepository für Erweiterungen zu öffnen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure-Funktionen](../azure-functions/functions-overview.md) (derzeit in der Seitenansicht) eine Version des WebJobs SDK basiert auf, die mit C#-Skript, Node.js und anderen Sprachen passt. 

Erstellen, bereitstellen und Verwalten von WebJobs ist nahtlos mit integrierten Tools in Visual Studio. Sie können WebJobs auf Grundlage von Vorlagen erstellen, veröffentlichen und (Ausführen/beenden/Monitor/Debuggen) verwalten können. 

Das WebJobs Dashboard im Portal Azure bietet leistungsfähige Management-Funktionen, die Ihnen Vollzugriff auf die Ausführung von WebJobs, einschließlich der Möglichkeit zum Aufrufen der einzelner Funktionen innerhalb WebJobs bieten. Das Dashboard zeigt auch Laufzeiten (Funktion) und die Ausgabe der Protokollierung. 

##<a name="a-namegetstartedagetting-started-with-webjobs-and-the-webjobs-sdk"></a><a name="getstarted"></a>Erste Schritte mit WebJobs und das WebJobs SDK

* [Einführung in Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs sind großartig, und starten Sie sie sofort mit!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blogbeitrag von mehr Troy.)
* [Azure WebJobs Features](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Was ist das WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Hintergrund Aufträge Leitfäden nach Microsoft Muster und Methoden](/documentation/articles/best-practices-background-jobs/)
* [Ankündigung von der 1.1.0 RTM des Microsoft Azure WebJobs SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Erste Schritte mit dem Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Verwenden von Azure Warteschlangenspeicher mit der WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Verwenden von Azure Blob-Speicher mit dem WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Verwenden von Azure Table Storage mit der WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Verwenden von Azure Service Bus mit der WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [So verwenden Sie WebHooks mit dem WebJobs SDK, zusammen mit Beispielen für GitHub, IFTTT und HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK Kurzübersicht (PDF-Download)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [WebJobs Einstellungen Dokumentation in GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videos
    * [WebJobs und das WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs Videoreihe auf Kanal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Einführung in WebJobs Tools für Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tools und Remote Debuggen](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure WebJobs Update mit Pranav Rastogi - Erweiterbarkeit in Version 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Siehe auch auf [WebJobs bereitstellen](#deploy) und [Testen und Debuggen von WebJobs](#debug)in den folgenden Abschnitten.

##<a name="a-namedeployadeploying-webjobs"></a><a name="deploy"></a>Bereitstellen von WebJobs

* [So verwenden Visual Studio bereitstellen Azure WebJobs](websites-dotnet-deploy-webjobs.md)
* [Zum Verwenden des Portals Azure WebJobs bereitstellen](web-sites-create-web-jobs.md)
* [Aktivieren der Befehlszeile oder über ein fortlaufender Übermittlung Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Bereitstellen einer .NET Console-app in Azure mit WebJobs Git](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Bereitstellen einer F-WebJob in Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Bereitstellen von benutzerdefinierten Diensten als Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videos
    * [Einführung in WebJobs Tools für Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tools und Remote Debuggen](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="a-namescheduleascheduling-webjobs"></a><a name="schedule"></a>Planen von WebJobs

* [Die Azure WebJob Dialogfeld "hinzufügen"](websites-dotnet-deploy-webjobs.md#configure)
* [Erstellen einer geplanten WebJob Azure-Portal](web-sites-create-web-jobs.md#CreateScheduled)
* [Einbinden eines Auftrags Scheduler zu einer WebJob](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Planen von Azure WebJobs mit Cron Ausdrücke](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Einzelne WebJob Funktionen mithilfe der WebJobs SDK TimerTrigger Planung](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="a-namedebugatesting-and-debugging-webjobs"></a><a name="debug"></a>Testen und Debuggen WebJobs

* [Neuer Entwickler und für das Debuggen von Features für Azure WebJobs in Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Anzeigen der WebJobs-Dashboards](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [So schreiben mithilfe des SDK WebJobs Protokolle, und zeigen Sie sie im Dashboard](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Remote Debuggen WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Wer hat diesen Blob geschrieben?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Hosten interaktive Code in der Cloud](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Spur Azure WebJobs hinzufügen](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Überwachen, diagnose und Problembehandlung bei Microsoft Azure-Speicher](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videos
    * [WebJobs Tools und Remote Debuggen](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="a-namescaleascaling-webjobs"></a><a name="scale"></a>Anpassungsbereich für WebJobs

* [Skalierung der Webanwendung mit Azure Websites](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure App-Verwaltungsdienst: Architektur großer Maßstab sofort For Business Web Apps](https://channel9.msdn.com/Events/Build/2014/3-626). Behandelt die Skalierung der Web apps mit WebJobs, einschließlich der WebJobs SDK.
* Videos
    * [Skalierung WebJobs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="a-nameadditionalaadditional-webjobs-resources"></a><a name="additional"></a>Zusätzliche Ressourcen für WebJobs

* [Azure WebJobs GA-Blogbeitrag von Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Aufträge des Powershell-Web App-Verwaltungsdienst Azure ausgeführt](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Schließt erste benachrichtigt, wenn Ihre Azure WebJobs ausgelöst wurde.](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Einfache Web App Sicherung Aufbewahrungsrichtlinie mit WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure Web Apps und Cloud Services bei der ersten Anforderung langsam](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Zeigt, wie WebJobs verwenden, um das Feature "AlwaysOn" zu reproduzieren, das nur für die standardmäßige Preisgestaltung Ebene verfügbar ist.
* [Sicheres war(en) WebJobs](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Sicheres war(en) für WebJobs SDK, finden Sie unter [sicheres war(en)](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Automatisieren von Sicherungskopien mit Azure WebJobs & AzCopy](http://markjbrown.com/azure-webjobs-azcopy/)
* Videos
    * [Azure WebJobs Videos durch Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs Videoreihe auf Kanal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="a-nameadditionalsdkaadditional-webjobs-sdk-resources"></a><a name="additionalsdk"></a>Zusätzliche WebJobs SDK-Ressourcen

* [WebJobs SDK-Version von Notizen](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK-Quellcode](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK Erweiterungen Quellcode](https://github.com/Azure/azure-webjobs-sdk-extensions), mit [ausführlicher Leitfaden zum Erweiterbarkeitsmodell](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [WebJobs SDK Skript-Quellcode](https://github.com/Azure/azure-webjobs-sdk-script/) (für [Azure-Funktionen](../azure-functions/functions-overview.md)verwendet)
* [Hochladen von Dateien FREB in Azure-Speicher mit dem SDK WebJobs WebJob](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Mit der Protokollierung Vorteile von einer Azure Webjob gehostet Hostinganbieter Azure Webjobs außerhalb Azure,](http://bypassion.dk/?p=510)
* [Erstellen ein Tool zum Importieren von Daten mit Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Übersicht über Azure-Funktionen](../azure-functions/functions-overview.md)
* Videos
    * [Azure WebJobs Videoreihe auf Kanal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="a-namesamplesasample-webjob-applications"></a><a name="samples"></a>Beispiel für WebJob Applikationen

* [Beispiel für Applikationen zur Verfügung gestellt, durch das Team WebJobs GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Einfache Azure Web App mit WebJobs Backend mit dem SDK WebJobs](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Veranschaulicht die Verwendung der geplanten und ereignisgesteuerten WebJobs. Finden Sie im Blogbeitrag [Neuerstellen der SiteMonitR mit Azure WebJobs SDK](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs)aus.

##<a name="a-nameblogsablogs"></a><a name="blogs"></a>Blogs

* [Azure-blog](/blog)
* [Amit Apple Blog](http://blog.amitapple.com/). Schwerpunkt auf WebJobs (nicht im SDK).
* [Magnus Mårtenssons-blog](http://magnusmartensson.com/)

##<a name="a-namegethelpagetting-help-with-webjobs"></a><a name="gethelp"></a>Aufrufen der Hilfe mit WebJobs

* [StackOverflow für WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow für das WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow für Azure-Funktionen](http://stackoverflow.com/questions/tagged/azure-functions)
* [Forum Azure und ASP.NET](http://forums.asp.net/1247.aspx)
* [Azure App Dienst Web Apps-forum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps Benutzer VoIP-Website](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Verwenden Sie die hashtags #AzureWebJobs.
* [WebJobs Fehler oder Problem melden](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

