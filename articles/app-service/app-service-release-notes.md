<properties 
   pageTitle="Azure SDK für .NET 2.5.1 – Versionsinformationen" 
   description="Azure SDK für .NET 2.5.1 – Versionsinformationen" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK für .NET 2.5.1 – Versionsinformationen

Dieses Dokument enthält die Versionsinformationen für das Azure SDK für .NET 2.5.1 lassen Sie wieder los. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK für .NET 2.5.1 Freigeben von Notizen

Die folgenden sind neuen Features und Updates im Azure SDK für .NET 2.5.1.

- Neue Features\scenarios im Zusammenhang mit der **Web-Tools-Erweiterungen**. 

    - Azure Websites wurde umbenannt in Azure-App-Verwaltungsdienst. Weitere Informationen finden Sie unter [Azure App-Dienst und vorhandenen Azure-Dienste](app-service-changes-existing-services.md).
    - Unterstützung für Azure-API Apps (Preview) wurde hinzugefügt, damit Kunden ASP.NET-Projekte als API Apps veröffentlichen können, und verwenden Sie dann auf Hinzufügen > Azure-API App Client Bewegung in C#-Projekten, um basierend auf der Struktur der App bereitgestellten API Code generieren. 
    - Der Knoten Websites im Server-Explorer ist anstelle der App-Verwaltungsdienst Azure-Knoten veraltet, die Unterstützung für Ressourcen, die Gruppierung von Azure-API Apps, Mobile-Apps und Web Apps basierende Gruppe enthält.
    - Azure Mobile-Apps (Preview) Support hinzugefügt wurde, damit Kunden neue Mobile-Apps Projekte erstellen können Mobile-Apps Controller hinzufügen, veröffentlichen Sie die Projekte und Remote Debuggen von Applications.
    - Hinzufügen > Azure-API App Client Bewegung unterstützt jetzt lokale Swagger JSON-Dateien, sodass Web-API Entwickler Swagger generieren, oder erstellen Sie sie manuell von Drittanbietern NuGets wie Swashbuckle verwenden können. Auf diese Weise können die-Client-Entwickler der Code Generation Features verwenden, um alle Swagger Endpunkt in C#-Projekten nutzen. 
    - Web App und API-App für die Veröffentlichung Dialogfelder wurden zur Unterstützung des Konzepts Azure-Portal der Ressource gruppieren weiterentwickelt und Auswahl/Erstellung von Azure Ressourcengruppen und App Service-Pläne im neuen provisioning Dialogfeld Web App und API-App dargestellt werden. 
    - Azure-API App-Server-Explorer-Knoten finden Sie Links zu den Apps-API Tiefe Link in der Azure-Portal als auch andere Features wie Log Streaming und Remotedebuggen.

    Bekannte Probleme und aktuelle Einschränkungen Azure SDK .NET 2.5.1 [in diesem](app-service-release-notes.md#known_issues_2_5_1) Abschnitt unten.


- Neue Features\scenarios im Zusammenhang mit **HDInsight Tools** in Visual Studio sind in dieser Version aktiviert. 
    - Lokale Überprüfung von Skripts Struktur. Klicken Sie auf die Schaltfläche bestätigen Skript, in der Symbolleiste, um festzustellen, ob Fehler in Ihrem Skript vorhanden sind. 
    - Verbesserte für das Debuggen von Aufträgen Struktur. Sie können jetzt Struktur Aufträge Debuggen, indem Sie den Zugriff auf aus Protokolle in Visual Studio. Wenn die Anwendung Leistungsprobleme verfügt, wird Untersuchung läuft aus Protokolle nützliche Informationen...
    - (Public Preview) Das automatische Vervollständigen von Schlüsselwort und IntelliSense-support für Struktur. Helfen Sie Struktur Skripts verfassen, HDInsight Tools für Visual Studio hinzugefügt Schlüsselwort automatischen Vervollständigung und IntelliSense-support für Struktur.
    - Storm-Support. HDInsight Tools für Visual Studio können jetzt Storm Topologien/Spouts/Schrauben in c# entwickeln. Dann können Sie die entwickeltes Suchtopologie zu einem Cluster Storm übermitteln und finden Sie unter der Suchtopologie/herstellt/Schnauze Status. Systemprotokolle und Kunden Protokolle können Sie um Ihre Storm Topologien/Schrauben/Spouts zu behandeln. Vorhandene JAVA-Anlagen können in Storm auf HDInsight Sie auch.
    
    Weitere Informationen finden Sie unter [Erste Schritte mit HDInsight Hadoop-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a name="a-idknownissues251aazure-sdk-for-net-251-known-issues-and-limitations"></a><a id="known_issues_2_5_1"></a>Azure SDK für .NET 2.5.1 bekannte Probleme und Einschränkungen

- Azure-API Apps wird als Bereitstellungsziel für Mobile-Apps angezeigt. Web Apps sollte das Ziel des nur für Mobile-Apps, bis eine nachfolgende Version sein. 
- Azure API App bereitgestellt kann dazu führen, dass Erfolg, doch zeitweise fehlschlagen den Fortschritt im Fenster Azure App Dienst Aktivität aktualisieren. Problemumgehung besteht darin, den Status der neuen Azure-API App Azure-Portal zu überprüfen. 
- Datei > Neues Projekt > API App > F5 Experience führt zu einem HTTP-Fehler auf, da es keine default/index.html ist. Navigieren Sie zur /api/values URL manuell werden umgangen. 
- Server-Explorer Symbole angezeigt zeitweise, flache. Einen Neustart im Vergleich mit einer wird dieses Problem behoben. 
- Wenn eine Ausnahme ausgelöst wird, während der Bereitstellung von Web App oder API-App (beispielsweise Kontingent überschritten Fehler oder doppelte Azure-API App gatewayname), Anzeigen der Fehler JSON unformatierten Text ein. 
- Wiederkehrender Texterstellung Probleme bei der Anwendung Einsichten zum Zeitpunkt der Erstellung Projekt aktiviert ist.
- Es kann vorkommen, generierte Azure-API App Client Code fehlt Namespaces, benötigten manuell enthalten (oder automatisch über Visual Studio als Ersatz importiert werden) für Code kompiliert. 
- Mobile-App Projekte Web apps veröffentlicht werden soll, aber Sie müssen eine da Mobile-App Projekte, eine Datenbank erfordern als Mobile App Azure-Portal erstellten Website auswählen. 
- Die Startseite für Mobile-Apps verwendet den Begriff "mobile Service" statt "mobile apps" 
- Erstellen eines Projekts von Mobile-App kann in eine Minute um bis zu dauern erstellen. 
- Während API App stammen provisioning (in einigen Fällen) einen Fehler wieder aus der Azure-API über die entsprechenden, dass die Berechtigungen nicht ordnungsgemäß festgelegt werden konnten, während die App API ordnungsgemäß bereitgestellt und einsatzbereit ist. Sie können manuell mithilfe der Azure-Portal Berechtigungen festlegen.
- Anwendung Einsichten wird auf API App-Vorlagen und Mobile-App-Vorlagen nicht unterstützt.
- API App Projekte können in Verbindung mit der Cloud-Dienst Projekte verwendet werden.
- Project-Vorlagen API App sind nur in c# verfügbar.
- API App Verbrauch über das Kontextmenü "Azure-API App Client hinzufügen" wird nur in c# unterstützt.

 
