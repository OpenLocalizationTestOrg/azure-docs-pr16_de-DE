<properties
    pageTitle="Was ist der Azure .NET SDK"
    description="Erfahren Sie, was in Azure .NET SDK enthalten ist."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Was ist der Azure SDK für .NET?

## <a name="overview"></a>(Übersicht)

Die Azure SDK für .NET ist eine Reihe von Tools für Visual Studio, Befehlszeile Tools, Laufzeit Binärdateien und Clientbibliotheken, die Ihnen helfen entwickeln, testen und Bereitstellen von apps, die in Azure ausgeführt werden. In diesem Artikel wird erläutert, was Sie erhalten, wenn Sie das SDK installieren. Sie können das SDK von der [Azure-Downloadseite](https://azure.microsoft.com/downloads/)herunterladen.

Die Azure SDK für .NET umfasst auch [Client-Bibliotheken für in Anspruch nehmen Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=510472). Diese Bibliotheken werden separat mit NuGet installiert.

##<a name="a-idincludedawhats-included-in-the-azure-sdk-for-net"></a><a id="included"></a>Was ist in der Azure SDK für .NET enthalten

Die Azure SDK für .NET installiert die folgenden Produkte:

- [Visual Studio Community Edition 2015](#vwd)
- [Microsoft Azure Speicheremulator](#stgemulator)
- [Microsoft Azure-Speicher-Tools](#stgtools)
- [Microsoft Azure Authoring-Tools](#auth)
- [Microsoft Azure Emulator](#emulator)
- [HDInsight Tools für Visual Studio und Struktur der Microsoft ODBC-Treiber](#hdinsight)
- [Microsoft Azure-Bibliotheken für .NET](#libraries)
- [Microsoft Azure SDK für Mobile-App](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Azure-Tools für Microsoft Visual Studio](#tools)
- [Microsoft ASP.NET und Webtools für Visual Studio](#wte)
- [Microsoft Azure-Daten Lake Tools für Visual Studio](#datalake)

###<a name="a-idvwdavisual-studio-community-edition-2015"></a><a id="vwd"></a>Visual Studio Community Edition 2015

Wenn Sie Visual Studio auf dem Computer besitzen, wird das SDK [Visual Studio Community Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs)installieren.

###<a name="a-idstgemulatoramicrosoft-azure-storage-emulator"></a><a id="stgemulator"></a>Microsoft Azure Speicheremulator

Der [Speicheremulator Azure](http://msdn.microsoft.com/library/hh403989.aspx) verwendet eine SQL Server-Instanz und im lokalen Dateisystem um Azure-Speicher (Warteschlangen, Tabellen, Blobs) zu reproduzieren, damit Sie lokal testen können.

###<a name="a-idstgtoolsamicrosoft-azure-storage-tools"></a><a id="stgtools"></a>Microsoft Azure-Speicher-Tools

So installieren Sie [AzCopy](http://aka.ms/AzCopy), eines Befehlszeile Tools, die Sie verwenden können, um Daten in die und aus einem Konto Azure-Speicher übertragen.

###<a name="a-idauthamicrosoft-azure-authoring-tools"></a><a id="auth"></a>Microsoft Azure Authoring-Tools

Dies umfasst die folgenden:

* Das [CSPack Befehlszeile Tool](http://msdn.microsoft.com/library/gg432988.aspx) für die Bereitstellungspakete erstellen.
* die [Befehlszeile CSEncrypt-Tool](http://msdn.microsoft.com/library/hh404001.aspx) für die Verschlüsselung von Kennwörtern, mit denen Instanzen von Cloud-Dienst Rolle über eine Remotedesktop-Verbindung zugreifen.
* Laufzeit-Binärdateien, die cloud-Dienstprojekte erfordern für die Kommunikation mit ihren Runtime-Umgebung und Diagnose. Diese Binärdateien sind nicht in NuGet-Pakete verfügbar.

###<a name="a-idemulatoramicrosoft-azure-emulator"></a><a id="emulator"></a>Microsoft Azure Emulator

Der [Azure Emulator](http://msdn.microsoft.com/library/dn339018.aspx) simuliert die Cloud-Service-Umgebung, damit Sie testen können Cloud-Dienstprojekte lokal auf Ihrem Computer, bevor Sie sie in Azure bereitstellen.

###<a name="a-idhdinsightahdinsight-tools-for-visual-studio-and-microsoft-hive-odbc-driver"></a><a id="hdinsight"></a>HDInsight Tools für Visual Studio und Struktur der Microsoft ODBC-Treiber

HDInsight Tools im Server-Explorer können Sie navigieren Struktur Datenbanken und verknüpfte Speicherkonten für HDInsight Cluster, Erstellen von Tabellen, und erstellen und senden Struktur Abfragen. Weitere Informationen finden Sie unter [Erste Schritte mit HDInsight Hadoop-Tools für Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a name="a-idlibrariesamicrosoft-azure-libraries-for-net"></a><a id="libraries"></a>Microsoft Azure-Bibliotheken für .NET

Dies umfasst die folgenden:

* NuGet-Paketen für Azure-Speicher, Dienstbus und zwischenspeichern, die auf Ihrem Computer gespeichert sind, damit Visual Studio neue Cloud Service beim Erstellen von Projekten können offline.
* Ein Visual Studio-Plug-in, mit dem [Cache In Rolle](http://msdn.microsoft.com/library/dn386103.aspx) Projekte auf Visual Studio lokal ausgeführt werden können.

###<a name="a-idmobileamicrosoft-azure-mobile-app-sdk"></a><a id="mobile"></a>Microsoft Azure SDK für Mobile-App

Tools für die Arbeit mit [Azure App Dienst Mobile-Apps](app-service-mobile/app-service-mobile-value-prop.md).

###<a name="a-idpsamicrosoft-azure-powershell"></a><a id="ps"></a>Microsoft Azure PowerShell

PowerShell Azure ermöglicht es Ihnen zu [automatisieren Azure-Umgebung und -Bereitstellung](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a name="a-idtoolsamicrosoft-azure-tools-for-microsoft-visual-studio"></a><a id="tools"></a>Microsoft Azure-Tools für Microsoft Visual Studio

Dies können Sie für die Arbeit mit Azure Ressourcen, die hauptsächlich Cloud-Diensten und virtuellen Computern:

* [Erstellen, öffnen, und Cloud-Dienstprojekte veröffentlichen](cloud-services/cloud-services-dotnet-get-started.md).
* [Erstellen von Bereitstellungspaketen für Projekte, die Cloud-Dienst](http://msdn.microsoft.com/library/ff683672.aspx).
* [Erstellen von Azure virtuellen Computern beim Erstellen neuer Webprojekte](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Erstellen von PowerShell-Skripts beim Erstellen neuer virtueller Maschinen](http://msdn.microsoft.com/library/dn642480.aspx).
* [Anzeigen und Verwalten von Cloud Service Project-Einstellungen in Visual Studio Projekteigenschaften Windows](http://msdn.microsoft.com/library/ee405486.aspx).
* Anzeigen und Verwalten von [Cloud Services](http://msdn.microsoft.com/library/ff683675.aspx), [virtuellen Computern](http://msdn.microsoft.com/library/jj131259.aspx)und [Dienstbus](http://msdn.microsoft.com/library/jj149828.aspx) im Server-Explorer.
* [Im Debuggen-Modus Remote für Cloud-Diensten und virtuellen Computern ausführen](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatisieren von Bereitstellung von Ressourcen mithilfe von Azure Ressource gruppieren Bereitstellung von Projekten](https://msdn.microsoft.com/library/dn872471.aspx)

###<a name="a-idwteamicrosoft-app-service-tools-for-visual-studio"></a><a id="wte"></a>Microsoft-App-Verwaltungsdienst-Tools für Visual Studio

Dies können Sie für die Arbeit mit Azure Websites:

* [Veröffentlichen von Webprojekte mit Azure-Websites](app-service-web/web-sites-dotnet-get-started.md).
* [Veröffentlichen Console-Anwendungsprojekte zu Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Azure-Website erstellen und SQL-Datenbank-Ressourcen beim Erstellen eines neuen Webprojekts oder beim Veröffentlichen eines Projekts](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Erstellen von PowerShell Bereitstellungsskripts beim Erstellen neuer Websites](http://msdn.microsoft.com/library/dn642480.aspx).
* [Verwalten und Problembehandlung bei Azure Websites im Server-Explorer](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Führen Sie im Debuggen-Modus Remote für Websites und WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Sie müssen nicht das Azure SDK für .NET zum Verwenden dieser Features installieren; Sie sind auch in Visual Studio-Updates enthalten.

##<a name="a-iddatalakeamicrosoft-azure-data-lake-tools-for-visual-studio"></a><a id="datalake"></a>Microsoft Azure-Daten Lake Tools für Visual Studio

Weitere Informationen finden Sie unter [Lernprogramm: Entwickeln U-SQL-Skripts mit dem Datentools für Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a name="a-idnotincludedawhats-not-included-when-you-install-the-azure-sdk-for-net"></a><a id="notincluded"></a>Was nicht vorhanden ist, bei der Installation der Azure SDK für .NET

Es gibt ein paar Punkte, die Sie möglicherweise für Azure-Entwicklung, die nicht, die bei der Installation des SDKS enthalten sind. Die wichtigsten gehören die folgenden:

* [Client-Bibliotheken](http://go.microsoft.com/fwlink/?LinkId=510472).

    Das Azure SDK umfasst Client-Bibliotheken für die Nutzung der Azure-Dienste, aber nicht für alle von ihnen bei der Installation des SDKS installiert sind. Wenn die Anwendung eine Clientbibliothek, die das SDK nicht installiert werden kann benötigt, können Sie es aus ["NuGet.org"](http://go.microsoft.com/fwlink/?LinkId=510472)erhalten. Wenn die Anwendung eine Clientbibliothek, die das SDK installiert wird verwendet, ist es empfiehlt sich das mit der aktuellen Version unter "NuGet.org" aktualisiert.

    **Lokale Kopien von Client-Bibliotheken.** Die Azure SDK für .NET kopiert auf Ihren Computer NuGet-Pakete für einige Azure Clientbibliotheken, z. B. Speicher, Dienstbus und Zwischenspeichern. Diese Clientbibliotheken werden automatisch in Projekten auf neue Cloud-Dienst aufgenommen, damit die lokale NuGet Pakete aktivieren Visual Studio zum Erstellen von Projekten, auch wenn Sie nicht mit dem Internet verbunden sind. Client-Bibliotheken werden, damit die Clientbibliotheken unter "NuGet.org" häufig weitere werden im Allgemeinen häufiger als neue SDK Versionen freigegeben werden, aktualisiert aktuelle als was sich mit dem SDK erzielen.

    **Project-Vorlagen, die Client-Bibliotheken enthalten.** Nur [Azure-Cloud-Dienst](cloud-services/cloud-services-dotnet-get-started.md) und Azure-App-Verwaltungsdienst [Mobile-Apps](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Projektvorlagen enthalten automatisch einige Clientbibliotheken. Installieren Sie für andere Bibliotheken oder andere Vorlagen die [Client-Bibliothek NuGet-Pakete](http://go.microsoft.com/fwlink/?LinkId=510472) , die Sie benötigen.

* [Mobile-Apps project-Vorlagen](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Vorlagen für Mobile-Apps sind nur in Visual Studio 2013 Update 2 und höher verfügbar. Sie sind nicht verfügbar, klicken Sie in Visual Studio 2012 oder einer früheren Version und nicht in Visual Studio 2013 Update 1 oder früher, auch wenn Sie das Azure SDK für .NET installieren.

##<a name="a-idfaqafrequently-asked-questions"></a><a id="faq"></a>Häufig gestellte Fragen

- [Viele Azure Features gibt es bereits in Visual Studio. Muss ich die Azure SDK für .NET installieren?](#azinvs)
- [Ich möchte eine Clientbibliothek. Habe ich das Azure SDK für .NET Bezugsarten installieren?](#clientlib)
- [Wo finde ich ältere Versionen von Azure SDK für .NET?](#olderversions)
- [Was ist der Supportlebenszyklus für Versionen von Azure SDK für .NET?](#lifecycle)
- [Welche Gast OS Versionen ist der Azure SDK für .NET Tasche?](#guestos)
- [Wie kann ich die Azure SDK für .NET deinstallieren?](#uninstall)

###<a name="a-idazinvsamany-azure-features-are-already-in-visual-studio-do-i-need-to-install-the-azure-sdk-for-net"></a><a id="azinvs"></a>Viele Azure Features gibt es bereits in Visual Studio. Muss ich die Azure SDK für .NET installieren?

Es empfiehlt sich das SDK installieren, wenn Sie für die neuesten Tools mit Azure entwickeln möchten. Wenn Sie nicht das SDK installieren möchten, können Sie vorgehen, wenn Folgendes zutrifft:

* Sie haben das neueste [Update für Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)installiert haben.
* Sie sind nur für Azure Websites oder Mobile Dienste, nicht für Cloud Services oder virtuellen Computern entwickeln.
* Die Anwendung nicht verwenden, Speicher, oder es Speicher verwendet, aber nicht benötigten Speicheremulator oder auf das Tool AzCopy.

###<a name="a-idclientlibai-want-a-client-library-do-i-have-to-install-the-azure-sdk-for-net-to-get-it"></a><a id="clientlib"></a>Ich möchte eine Clientbibliothek. Habe ich das Azure SDK für .NET Bezugsarten installieren?

Das SDK installiert Client-Bibliotheken, so dass Sie Cloud Service-Projekte erstellen können, auch wenn Sie nicht mit dem Internet verbunden sind. Aktuelle Clientbibliotheken sind in unter ["NuGet.org"](http://go.microsoft.com/fwlink/?LinkId=510472)NuGet-Pakete verfügbar. Weitere Informationen finden Sie unter [Was ist nicht enthalten, bei der Installation der Azure SDK für .NET](#notincluded) oben in diesem Dokument.

###<a name="a-idolderversionsawhere-can-i-find-older-versions-of-the-azure-sdk-for-net"></a><a id="olderversions"></a>Wo finde ich ältere Versionen von Azure SDK für .NET?

Ältere Versionen finden Sie unter downloads das [Azure SDK für .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) Seite.

###<a name="a-idlifecycleawhats-the-lifecycle-policy-for-versions-of-the-azure-sdk-for-net"></a><a id="lifecycle"></a>Was ist der Supportlebenszyklus für Versionen von Azure SDK für .NET?

Finden Sie unter [Microsoft Azure Cloud Services unterstützen Supportlebenszyklus](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a name="a-idguestosawhich-guest-os-versions-is-the-azure-sdk-for-net-compatible-with"></a><a id="guestos"></a>Welche Gast OS Versionen ist der Azure SDK für .NET Tasche?

Finden Sie unter [Azure Gast OS-Versionen und SDK Kompatibilitätsmatrix](http://msdn.microsoft.com/library/ee924680.aspx).

###<a name="a-iduninstallahow-do-i-uninstall-the-azure-sdk-for-net"></a><a id="uninstall"></a>Wie kann ich die Azure SDK für .NET deinstallieren?

Jedes Element in diesem Artikel unter [Was ist der Azure SDK für .NET bieten](#included) dargestellt ist ein separates Programm in der Liste der installierten Programme in Windows-Systemsteuerung auf **Programme und Funktionen**.  Es gibt keine Möglichkeit, die sie als Gruppe zu deinstallieren. Sie müssen einzeln jedes Programm deinstallieren.

Wenn Sie das Azure SDK für .NET bereits installiert haben und Sie eine neue Version installieren, ist im Allgemeinen nicht erforderlich, die alte Version deinstallieren. In den meisten Fällen aktualisiert die SDK-Installation ein vorhandenes Programm statt eine neue hinzufügen und das alte Konto zu verlassen.

Wenn Sie keine-mehr-erforderlich Reste von einer früheren Version entfernen möchten, jedoch deinstallieren Sie nur die Programme, die die ältere Versionsnummer angeben, und deinstallieren Sie sie nur, wenn das gleiche Programm durch eine neuere Version vorhanden ist. Angenommen, nach der Aktualisierung von 2,5 auf 2.6 möglicherweise 2,5 und 2.6 Versionen von "Microsoft Azure-Tools für Microsoft Visual Studio 2013" angezeigt, und Sie können die 2,5 Version deinstallieren. Aber nur die 2,5 Version von "Microsoft Azure Authoring Tools" wird angezeigt, und es wäre nicht so deinstallieren Sie die sichere.

Beachten Sie, dass Versionsnummern in Programm Titeln werden auf **Programme und Funktionen** irreführend sein können.  Beispielsweise enthält SDK Version 2.6 "Microsoft Azure Mobile-App SDK 1.0" aus, wenn Sie das SDK für Visual Studio 2013 und "Microsoft Azure Mobile-App SDK, Version 2.0" für Visual Studio 2015 installieren; die Version nicht in diesem Fall die SDK-Version, aber einen Indikator, von dem Visual Studio-Version des Programms gilt.

In diesem Artikel wird nicht jedes Programm aufgeführt, die jeder frühere Version des Azure SDK enthalten; Es gibt anderen Programmen, die Sie aus früheren Versionen von SDK, deinstallieren können, da frühere Versionen von SDK manchmal Programme enthalten, die von höhere Versionen weggelassen wurden. Angenommen, Version 2,5 Installationen "Azure Ressource-Manager-Tools für Visual Studio", aber das ist nicht in der Liste der in diesem Artikel, da es sich nicht mehr als ein separates Programm unter **Programme und Funktionen**angezeigt wird.  Dieser Artikel listet nur Programme, die in der Azure SDK für .NET Version 2.6 enthalten sind.

> **Hinweis:** Einiger Programme, die das SDK enthält möglicherweise auch separat installiert werden in anderen Kontexten und kann erforderlich sein, auch wenn Sie nicht das vollständige SDK benötigen. Gleich WAHR Programme möglicherweise, die in früheren SDK installiert wurden, aber von SDK höher ausgelassen wurden. Wenn Sie Programme deinstallieren, achten Sie darauf, dass Sie um zu vermeiden, entfernen ein Element, das immer noch auf Ihrem Computer erforderlich ist.



##<a name="a-idversionsaversions"></a><a id="versions"></a>Versionen

Um anzuzeigen, welche Version aktiv ist oder um ältere Versionen herunterzuladen, finden Sie unter der Seite [Azure SDK für .NET Versionsverlauf](https://azure.microsoft.com/downloads/archive-net-downloads/) .

##<a name="a-idresourcesaresources"></a><a id="resources"></a>Ressourcen

Zum Herunterladen der aktuellen Azure SDK für .NET oder eine Clientbibliothek finden Sie unter der [Azure-Downloadseite](https://azure.microsoft.com/downloads/).

Die Azure SDK für .NET Quellcode, einschließlich Client-Bibliotheken finden Sie unter [GitHub.com/Azure](https://github.com/azure/).

Azure-Client-Bibliothek Dokumentation zur finden Sie unter [Azure .NET Bezug](https://azure.microsoft.com/documentation/api/).
