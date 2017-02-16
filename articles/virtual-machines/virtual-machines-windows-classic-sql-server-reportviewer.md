<properties 
    pageTitle="Verwenden Sie in einer Website ReportViewer | Microsoft Azure"
    description="In diesem Thema beschrieben, wie eine Microsoft Azure-Website mit dem Visual Studio ReportViewer-Steuerelement zu erstellen, die einen auf einer Microsoft Azure virtuellen Computern gespeicherten Bericht angezeigt werden."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Verwenden von ReportViewer in einer Website in einem Azure gehostet wird

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Sie können eine Microsoft Azure-Website mit dem Visual Studio ReportViewer-Steuerelement erstellen, die ein Berichts auf einer Microsoft Azure virtuellen Computern gespeichert werden angezeigt. Das ReportViewer-Steuerelement wird in einer Webanwendung, verwenden die Vorlage für ASP.NET Web-Anwendung zu erstellen.

>[AZURE.IMPORTANT] Das ReportViewer-Steuerelement unterstützt ASP.NET-MVC-Webanwendung Vorlagen nicht.

Um ReportViewer in Ihrer Microsoft Azure-Website zu integrieren, müssen Sie die folgenden Aufgaben ausführen.

- **Hinzufügen** Assemblys, dem Bereitstellungspaket

- **Konfigurieren** Authentifizierung und Autorisierung

- **Veröffentlichen** der ASP.NET Webanwendung mit Azure

## <a name="prerequisites"></a>Erforderliche Komponenten

Überprüfen Sie im Abschnitt "Allgemein empfohlen sowie optimale Methoden" in [SQL Server Business Intelligence in Azure virtuellen Computern](virtual-machines-windows-classic-ps-sql-bi.md)an.

>[AZURE.NOTE] ReportViewer-Steuerelemente werden mit Visual Studio Standard Edition oder höher ausgeliefert. Wenn Sie Web Developer Express Edition verwenden, müssen Sie die [MICROSOFT Berichts-VIEWER 2012 Laufzeit](https://www.microsoft.com/download/details.aspx?id=35747) zum Verwenden der Features ReportViewer Runtime installieren.
>
>ReportViewer im Modus für lokale Verarbeitung konfiguriert wird in Microsoft Azure nicht unterstützt.

Überprüfen Sie im Whitepaper [basierend auf Berichtsservern, Reporting Services-Bericht-Viewer-Steuerelement und Microsoft Azure-virtuellen Computern](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)an.

## <a name="adding-assemblies-to-the-deployment-package"></a>Hinzufügen von Assemblys zum das Bereitstellungspaket

Wenn Sie Ihre ASP.NET Anwendung lokal gehostet, die ReportViewer-Assemblys normalerweise direkt im globalen Assemblycache (GAC) des IIS-Servers während der Installation von Visual Studio installiert werden, und direkt von der Anwendung zugegriffen werden können. Wenn Sie Ihrer Anwendung ASP.NET in der Cloud gehostet, lässt Microsoft Azure nicht, etwas im GAC installiert sein, damit Sie sicherstellen müssen, dass die ReportViewer-Assemblys lokal für eine Anwendung verfügbar sind. Sie können dazu fügen im Projekt Verweise auf diese und so konfiguriert werden, die lokal kopiert werden.

Im Verarbeitungsmodus remote verwendet das ReportViewer-Steuerelement die folgenden Assemblys:

- **Microsoft.ReportViewer.WebForms.dll**: enthält den ReportViewer-Code, die Sie in Ihrer Seite ReportViewer verwenden müssen. Beim Ablegen eines ReportViewer-Steuerelements auf einer ASP.NET-Seite im Projekt, wird ein Verweis für diese Assembly zum Projekt hinzugefügt.

- **Microsoft.ReportViewer.Common.dll**: enthält Klassen, die vom ReportViewer-Steuerelement zur Laufzeit verwendet. Es ist nicht automatisch zum Projekt hinzugefügt.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Einen Verweis auf Microsoft.ReportViewer.Common hinzufügen

- Mit der rechten Maustaste Knoten **Verweise** des Projekts und wählen Sie **Verweis hinzufügen**, wählen Sie auf der Registerkarte .NET die Assembly aus, und klicken Sie auf **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Die Assemblys lokal nach Ihrer Anwendung ASP.NET Gestaltung von

1. Klicken Sie im Ordner " **Verweise** " auf die Assembly Microsoft.ReportViewer.Common, damit deren Eigenschaften im Bereich Eigenschaften angezeigt werden.

1. Legen Sie im Bereich Eigenschaften die Option **Lokale Kopie** auf True.

1. Wiederholen Sie die Schritte 1 und 2 für Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>ReportViewer ein Sprachpaket erwerben

1. Installieren Sie das entsprechende redistributable Microsoft Berichts-Viewer 2012 Runtime-Paket vom [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).

1. Wählen Sie die Sprache in der Dropdownliste aus, und die Seite mit der entsprechenden Download Center Seite weitergeleitet.

1. Klicken Sie auf **herunterladen** , um das Herunterladen von ReportViewerLP.exe starten.

1. Nachdem Sie ReportViewerLP.exe heruntergeladen haben, klicken Sie auf **Ausführen** , um sofort zu installieren, oder klicken Sie auf **Speichern** , um ihn auf Ihrem Computer zu speichern. Wenn Sie auf **Speichern**klicken, beachten Sie den Namen des Ordners, in dem Sie die Datei speichern.

1. Suchen Sie den Ordner, in dem Sie die Datei gespeichert, werden soll. Mit der rechten Maustaste ReportViewerLP.exe, klicken Sie auf **als Administrator ausführen**, und klicken Sie dann auf **Ja**.

1. Nach ReportViewerLP.exe wird ausgeführt, sehen Sie die c:\windows\assembly weist die Ressource **Microsoft.ReportViewer.Webforms.Resources** und **Microsoft.ReportViewer.Common.Resources**-Dateien.

### <a name="to-configure-for-localized-reportviewer-control"></a>So konfigurieren Sie die lokalisierten ReportViewer-Steuerelement

1. Herunterladen Sie und installieren Sie das Microsoft-Berichts-Viewer 2012 Runtime redistributable Package anhand der oben angegebenen Anweisungen.

1. Erstellen von <language> Ordner im Projekt und kopieren die zugeordneten Ressourcenassembly Dateien vorhanden. Sind die zu kopierenden Ressourcenassemblydateien: **Microsoft.ReportViewer.Webforms.Resources.dll** und **Microsoft.ReportViewer.Common.Resources.dll**. Wählen Sie die Ressourcenassemblydateien aus, und legen Sie im Eigenschaftenbereich **Copy to Output Directory** **immer kopieren"**.

1. Festlegen der Kultur und UICulture für das Webprojekt. Weitere Informationen zum Festlegen der Culture and UI Culture für eine ASP.NET-Webseite, finden Sie unter [wie: Festlegen der Kultur und die Kultur der Benutzeroberfläche für ASP.NET-Webseitenglobalisierung](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfigurieren der Authentifizierung und Autorisierung

ReportViewer richtige Anmeldeinformationen verwenden, um mit dem Berichtsserver authentifizieren muss, und die Anmeldeinformationen müssen vom Berichtsserver autorisiert sein, auf die Berichte zuzugreifen, werden soll. Informationen zur Authentifizierung finden Sie im Whitepaper [basierend auf Berichtsservern, Reporting Services-Bericht-Viewer-Steuerelement und Microsoft Azure-virtuellen Computern](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Veröffentlichen Sie die Anwendung ASP.NET Web in Azure

Veröffentlichen einer ASP.NET Web-Anwendung in Azure Anweisungen, finden Sie unter [wie: Migrieren und Veröffentlichen einer Web-Anwendung in Azure von Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) und [Erste Schritte mit Web Apps und ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Wenn Sie der Befehl Azure-Bereitstellungsprojekt hinzufügen oder Azure Cloud-Dienstprojekt hinzufügen im Kontextmenü in Lösung Explorer nicht angezeigt wird, müssen Sie das Ziel-Framework für das Projekt in .NET Framework 4 zu ändern.
>
>Die beiden Befehle bieten im Wesentlichen die gleiche Funktionalität. Eine "oder" den Befehl andere erscheinen auf das Kontextmenü je nachdem, welche Version von Microsoft Azure SDK installiert haben.

## <a name="resources"></a>Ressourcen

[Microsoft-Berichte](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence in Azure-virtuellen Computern](virtual-machines-windows-classic-ps-sql-bi.md)

[Verwenden Sie zum Erstellen einer Azure-virtuellen Computer mit einer einheitlichen Modus Berichtsserver PowerShell](virtual-machines-windows-classic-ps-sql-report.md)

[Reporting Services-Bericht-Viewer-Steuerelement und Microsoft Azure-virtuellen Computern Grundlage Berichtsservern](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
