
<properties 
   pageTitle="Azure SDK für .NET 2,8 – Versionsinformationen" 
   description="Azure SDK für .NET 2,8 – Versionsinformationen" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK für .NET 2,8, 2.8.1 und 2.8.2

##<a name="overview"></a>(Übersicht)
 
Dieser Artikel enthält die Versionsinformationen (die bekannte Probleme und abgeschnitten werden Änderungen enthält) für das Azure SDK für .NET 2,8, 2.8.1 und 2.8.2 frei. 

Vollständige Liste der neuen Features und Updates, die in dieser Version vorgenommen finden Sie unter der Ankündigung [Azure SDK 2,8 für Visual Studio 2013 und Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) . 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK für .NET 2,8

### <a name="download-azure-sdk-for-net-28"></a>Herunterladen von Azure SDK für .NET 2,8

[Azure SDK für .NET 2,8 für Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK für .NET 2,8 für Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>Unterstützung für .NET 4.5.2 

####<a name="known-issues"></a>Bekannte Probleme

Azure .NET SDK 2,8 ermöglicht es Ihnen, .NET 4.5.2 erstellen Cloud-Dienst Pakete. Jedoch 4.5.2-.NET Framework nicht installiert werden standardmäßig Gast-BS Bilder bis Januar 2016 Gast-BS lassen Sie wieder los. Vor diesem, 4.5.2 von .NET Framework werden durch eine separate Gast-BS Veröffentlichungsversion – November 2015-02 zur Verfügung. Finden Sie unter der Seite [Azure Gast OS Versionen und SDK Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md) nachverfolgt, wenn das Bild freigegeben wird.  Nachdem das Bild des November 2015-02 freigegeben ist, können Sie auswählen, die diesem Bild durch Aktualisieren der Cloud-Dienst Konfigurationsdatei (.cscfg) verwenden. Die Dienstkonfiguration Datei legen Sie das Attribut OsVersion des Elements ServiceConfiguration auf die Zeichenfolge "WA-Gast-OS-4.26_201511-02". Wenn Sie auswählen, zu nutzen, verwenden Sie diese Abbildung erhalten Sie nicht mehr automatische Updates an den Gast-BS. Um die automatische Updates OsVersion erhalten müssen auf festgelegt sein "*" und .NET 4.5.2 stehen nur zur Verfügung, bis die automatischen Updates in Januar 2016.

###<a name="azure-data-factory"></a>Factory Azure-Daten

####<a name="known-issues"></a>Bekannte Probleme 

Während der Erstellung eines **Data Factory-Vorlage** Projekt im Zusammenhang mit Beispieldaten möglicherweise Azure Power-Shellskript Azure Power Shell-Version auf dem Computer installiert ist nach 0.9.8.

Damit diese Art von Project erfolgreich erstellt haben, müssen Sie [Azure Power Shell Version 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)installieren.


### <a name="azure-resource-manager-tools"></a>Azure Ressourcenmanager Tools 

####<a name="breaking-changes"></a>Unterbrechen der Änderungen

Das PowerShell-Skript bereitgestellt, die Ressourcengruppe Azure-Projekt wurde in dieser Version für die Arbeit mit den neuen Azure PowerShell-Cmdlets Version 1.0 aktualisiert.  Diese neue Skript funktioniert aus mit Visual Studio nicht, wenn eine Version des SDK vor 2,8 verwenden.  

Skripts von in früheren Versionen von SDK erstellte Projekte werden nicht in Visual Studio ausgeführt werden bei Verwendung des 2,8 SDK.  Alle Skripts weiterhin außerhalb von Visual Studio mit der entsprechenden Version der Azure-PowerShell-Cmdlets arbeiten.  

Das 2,8 SDK erfordert Version 1.0 der Azure-PowerShell-Cmdlets.  Alle anderen Versionen von SDK erfordern Version 0.9.8 der Azure-PowerShell-Cmdlets.  Weitere Informationen finden Sie in [diesem](http://go.microsoft.com/fwlink/?LinkID=623011) Blog.

###<a name="web-tools-extensions"></a>Web Tools Extensions

####<a name="known-issues"></a>Bekannte Probleme

Die folgenden bekannten Probleme werden in der folgenden Version behoben sein.

- App-Dienst verknüpften Cloud und Server-Explorers Bewegung für nicht Herstellung Umgebungen (wie China Azure oder Azure Stapel Kunden) funktioniert nicht. Für Kunden in diese Betroffene Bereiche wird das Veröffentlichungsprofil aus dem Azure-Portal herunterladen publishing Möglichkeit aktivieren. Zukünftige Versionen repariert Gesten, wie etwa "Debugger anfügen" und "Streaming Protokolle anzeigen" Azure China und Stapel Kunden. 
- Kunden möglicherweise Fehler bei der App-Dienst finden Sie unter erstellen, wenn die App Einsichten Instanz fest, zu denen bereitgestellt werden in einem Bereich als ostasiatische US ist. In diesen Fällen werden eine App-Verwaltungsdienst im Portal erstellen und das Veröffentlichungsprofil herunterladen Szenarien für die Veröffentlichung aktivieren. 

###<a name="azure-hdinsight-tools"></a>Azure HDInsight Tools

####<a name="new-updates"></a>Neue updates

- Sie können die Struktur Abfrage im Cluster über HiveServer2 mit fast keine Verwaltungsaufwand ausführen und finden Sie unter der Auftrag Echtzeit protokolliert.
- Verwenden der neuen Struktur Ausführung Vorgangsansicht Sie in Ihrem Auftrag tieferen einsteigen können, finden Sie weitere Details und erkennen Sie potenzielle Probleme.

Informationen finden Sie unter [Azure SDK 2,8 für Visual Studio 2013 und Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK für .NET 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Bekannte Probleme in Visual Studio 2013 und Visual Studio 2015
 
1. Ausgelöste WebJob veröffentlicht wird Steckplätze anzeigen und Fehlermeldungen und wird keinen Zeitplan, aber es wird zu und drücken Sie die WebJob Azure. Kunden, benötigt eine Stelle planmäßige sind, können Sie das Azure-Portal, um den Zeitplan für die WebJob einzurichten. 
2. Python Kunden können Debugger Probleme auftreten. Service-Team ist eine Lösung für diese einführen, aber wenn Kunden betroffen sind, lassen Sie Microsoft Informationen in den Foren oder Ankündigung Blog oder Notizen Kommentarbereich lassen. 
3. Kunden in bestimmte Bereiche (z. B. Süd Indien) werden App-Verwaltungsdienst provisioning Fehler auftreten. Dies ist mit dem Portal konsistent, und Kunden, bei die dieses Problem auftritt, können Azure-Portal verwenden, um Zugriff auf diese Geo-Regionen veröffentlichen anfordern. Nachdem das Anfordern des Zugriffs auf diese Bereiche mit sollte der Azure Portals provisioning funktionieren. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK für .NET 2.8.2

Nach der Installation von der 2.8.2 Tools Kunden können das folgende Problem auftreten.         

- Wenn Sie 10 für Windows und Internet Explorer nicht installiert haben, können Sie ein Fehler "InternetExplorer konnte nicht gefunden werden" angezeigt.
Um das Problem zu beheben, installieren Sie Internet Explorer verwenden im Dialogfeld Windows-Komponenten hinzufügen/entfernen aus.

Wenn Sie dieses Problem beobachten, verwenden Sie das Senden eines Lächeln Feature um zu melden.

Weitere Informationen finden Sie unter [diesen](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) Posten.
##<a name="other-updates"></a>Andere updates

Andere Updates finden Sie unter [Azure SDK 2,8 Ankündigung bereitstellen](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Siehe auch

[Azure SDK 2,8 Ankündigung Beitrag](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Support und Rente Informationen zum Microsoft Azure SDK für .NET und APIs](https://msdn.microsoft.com/library/azure/dn479282.aspx)

