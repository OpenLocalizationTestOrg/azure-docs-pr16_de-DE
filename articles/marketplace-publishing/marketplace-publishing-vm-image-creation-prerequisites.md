<properties
   pageTitle="Technische Voraussetzung für die Erstellung von Abbild eines virtuellen Computers von Azure Marketplace | Microsoft Azure"
   description="Grundlegendes zu den Anforderungen für das Erstellen und Bereitstellen von Abbild eines virtuellen Computers zu Azure Marketplace für andere Benutzer erwerben."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technische Voraussetzung für die Erstellung von Abbild eines virtuellen Computers von Azure Marketplace
Lesen Sie den Prozess sorgfältig vor Beginn und zu verstehen Sie, wo und warum die einzelnen Schritte durchgeführt werden. So weit wie möglich, Sie sollten Vorbereiten der Firmeninformationen und anderen Daten, erforderlichen Tools herunterladen und/oder technische Komponenten vor Beginn des Erstellungsprozess Angebot erstellen. Diese Elemente sollten aus Durchsicht dieses Artikels deutlich zu sagen.  

## <a name="download-needed-tools--applications"></a>Herunterladen der benötigten Tools und Anwendungen
Sie sollten Sie Folgendes bereit, bevor Sie beginnen die haben:

- Je nach Betriebssystem Sie verwenden möchten, installieren Sie die [Azure PowerShell-Cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) oder [Linux Line Benutzeroberflächen-Tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) aus [Azure](https://azure.microsoft.com/downloads/) -Downloadseite.
- Installieren Sie von CodePlex Azure-Speicher-Explorer.
- Herunterladen Sie und installieren Sie das Zertifizierung Test-Tool für Azure zertifiziert:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Sie benötigen einen Windows-basierten Computer das Tool Zertifizierung ausführen. Wenn Sie nicht über die Windows-basierten Computer verfügen, können Sie das Tool mithilfe eines Windows-basierten virtuellen Computers in Azure ausführen.

## <a name="platforms-supported"></a>Unterstützte Plattformen
Entwickeln Sie Azure-basierten virtuellen Computer werden unter Windows oder Linux. Einige Elemente des Veröffentlichungsprozesses – beispielsweise eine Azure-kompatiblen virtuelle Festplatte (virtuelle Festplatte) – verschiedene Tools verwenden und Schritte, je nachdem, welches Betriebssystem, erstellen:  

- Wenn Sie Linux verwenden, finden Sie im Abschnitt "Erstellen einer Azure-kompatiblen virtuelle Festplatte (Linux-basiert)" der [Anleitung zum virtuellen Computern Bild veröffentlichen](marketplace-publishing-vm-image-creation.md).
- Wenn Sie Windows verwenden, finden Sie im Abschnitt "Erstellen einer Azure-kompatiblen virtuelle Festplatte (Windows-basiertem)" des [virtuellen Computers Bild für Veröffentlichung Leitfaden](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] Sie benötigen Zugriff auf einen Windows-basierten Computer zu:
- Führen Sie das Tool Zertifizierung Überprüfung.
- Erstellen der virtuellen Festplatte freigegeben Access Signatur-URL für die virtuelle Festplatte Zertifizierung Einreichung an.

## <a name="develop-your-vhd"></a>Entwickeln Sie Ihre virtuelle Festplatte
Sie können Azure-virtuellen Festplatten in der Cloud oder lokalen entwickeln:

- Cloud-basierte Entwicklung bedeutet, dass alle Entwicklungsschritte per Remotezugriff auf einer virtuellen Festplatte auf Azure ausgeführt werden.
- Lokale Entwicklung erfordert eine virtuelle Festplatte herunterladen und es mit lokalen Infrastruktur entwickeln. Auch wenn dies möglich ist, es nicht zu empfehlen. Beachten Sie, dass für Windows oder SQL Entwicklung lokalen erfordert, dass Sie die entsprechenden lokalen Lizenzschlüssel besitzen. Sie können nicht ein- oder SQL Server nach dem Erstellen eines virtuellen Computers zu installieren. Sie müssen Ihr Angebot auch auf ein genehmigten SQL-Bild aus dem Azure-Portal basieren. Wenn Sie lokale entwickeln möchten, müssen Sie einige Schritte ausführen anders als wurden Wenn Sie in der Cloud entwickeln. Relevante Informationen finden Sie unter [Erstellen eines lokalen virtuellen Computer Bilds](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie die erforderlichen Komponenten überprüft und die erforderlichen Vorgänge abgeschlossen, können Sie bei der Erstellung Ihrer virtuellen Computern Bild Angebots als die [Anleitung zum virtuellen Computern Bild veröffentlichen](marketplace-publishing-vm-image-creation.md)ausführliche nach vorne verschieben.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: So veröffentlichen ein Angebots zu Azure Marketplace](marketplace-publishing-getting-started.md)
- [Erstellen Sie einen virtuellen Computer mit Windows Azure Preview-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
