<properties
    pageTitle="WebJobs in Azure App-Verwaltungsdienst"
    description="Informationen Sie zum Erstellen WebJobs zum Hintergrund Tests ausführen und interagieren mit den Diensten wie Speicher und Dienstbus geplante Vorgängen erstellen."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Verwenden von WebJobs in Azure App-Verwaltungsdienst

Dieser Artikel enthält Links zu Dokumentationsressourcen zur Verwendung von Azure WebJobs und Azure WebJobs SDK. Azure WebJobs bieten eine einfache Möglichkeit, Programme oder Skripts auf [App Dienst Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)als Hintergrundprozesse ausgeführt werden. Hochladen, und führen Sie eine ausführbare Datei wie als Cmd, Bat, EXE-Datei (.NET), ps1, sh, Php, hierhin, Js und Jar. Diese Programme führen Sie nach einem Zeitplan (Cron) oder permanent als WebJobs.

Das WebJobs SDK vereinfacht Azure-Speicher verwenden. Das WebJobs SDK verfügt über eine Bindung und Trigger-System, das mit Microsoft Azure-Speicher Blobs, Warteschlangen und Tabellen sowie Bus Servicewarteschlangen funktioniert.

Erstellen, bereitstellen und Verwalten von WebJobs ist nahtlos mit integrierten Tools in Visual Studio. Sie können WebJobs auf Grundlage von Vorlagen erstellen, veröffentlichen und (Ausführen/beenden/Monitor/Debuggen) verwalten können.

Das WebJobs Dashboard im Portal Azure bietet leistungsfähige Management-Funktionen, die Ihnen Vollzugriff auf die Ausführung von WebJobs, einschließlich der Möglichkeit zum Aufrufen der einzelner Funktionen innerhalb WebJobs bieten. Das Dashboard zeigt auch Laufzeiten (Funktion) und die Ausgabe der Protokollierung.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
