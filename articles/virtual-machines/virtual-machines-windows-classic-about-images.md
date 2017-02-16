<properties
    pageTitle="Informationen zu Bildern für Windows-virtuellen Computern | Microsoft Azure"
    description="Erfahren Sie, wie Bilder mit Windows-virtuellen Computern in Azure verwendet werden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Informationen zu Bildern für Windows-virtuellen Computern

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Arbeiten mit Bildern

Das Azure PowerShell-Modul können Sie um die Bilder verfügbar zu Ihrem Azure-Abonnement zu verwalten. Sie können auch im klassische Azure-Portal für einige Vorgänge Bild verwenden, aber die Befehlszeile erhalten Sie weitere Optionen.


-   **Alle Bilder zu erhalten**:`Get-AzureVMImage`gibt eine Liste aller Bilder in Ihrem aktuellen Abonnement: Ihre Bilder als auch von Azure oder Partner bereitgestellt. Dies bedeutet, dass Sie möglicherweise eine umfangreiche Liste aufrufen. In den nächsten Beispielen wird so erhalten Sie eine kürzere Liste.
-   **Erste Bild Familien**:`Get-AzureVMImage | select ImageFamily` Ruft eine Liste der Abbildung Familien durch mit Zeichenfolgen **ImageFamily** Eigenschaft.
-   **Alle Bilder in einem bestimmten Familie zu erhalten**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Virtueller Computer Bilder suchen**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` dies funktioniert, indem Sie die DataDiskConfiguration-Eigenschaft nur für Bilder virtueller Computer gilt filtern. In diesem Beispiel filtert auch die Ausgabe auf nur den Namen Bezeichnungsfeld und Bild.
-   **Speichern ein Bilds GRG**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Speichern Sie eine spezialisierte Bild**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] Der Parameter OSState ist erforderlich, wenn Sie ein Bild virtueller Computer erstellen, die Daten Datenträger als auch der Datenträger Betriebssystem umfasst möchten. Wenn Sie den Parameter nicht verwenden, wird das Cmdlet ein OS Bild erstellt. Der Wert des Parameters gibt an, ob das Bild, GRG- oder spezielle, ausgehend von der Datenträger: das Betriebssystem, ob für die Wiederverwendung vorbereitet wurde.
-   **Löschen eines Bilds**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Nächste Schritte

Sie können auch [Erstellen Sie einen Windows-Computer mithilfe des klassischen Portals](virtual-machines-windows-classic-tutorial.md)

