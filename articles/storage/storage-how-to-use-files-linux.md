<properties
    pageTitle="Verwenden von Azure-Dateien mit Linux | Microsoft Azure"
        description="Erstellen Sie eine Dateifreigabe Azure in der Cloud mit diesem schrittweises Lernprogramm aus. Verwalten der Inhalt Ihrer Datei freigeben, und stellen Sie eine Dateifreigabe aus einer Azure-virtuellen Computern (virtueller Computer) unter Linux oder eine lokale-Anwendung, die SMB 3.0 unterstützt."
        services="storage"
        documentationCenter="na"
        authors="mine-msft"
        manager="aungoo"
        editor="tysonn" />

<tags ms.service="storage"
      ms.workload="storage"
      ms.tgt_pltfrm="na"
      ms.devlang="na"
      ms.topic="article"
      ms.date="10/18/2016"
      ms.author="minet" />


# <a name="how-to-use-azure-file-storage-with-linux"></a>Verwenden von Azure Dateispeicher mit Linux

## <a name="overview"></a>(Übersicht)

Azure Dateispeicher bietet Dateifreigaben in der Cloud, mit dem standardmäßigen SMB-Protokoll. Mit Azure-Dateien können Sie Enterprise Applications migrieren, die auf Dateiserver an Azure aufsetzen. In Azure ausgeführt Applications können einfach Dateifreigaben aus Azure-virtuellen Computern mit Linux bereitstellen. Und mit der neuesten Version von Dateispeicher, können Sie auch eine Dateifreigabe aus einer lokalen-Anwendung, die SMB 3.0 unterstützt, bereitstellen.

Sie können mithilfe der [Azure-Portal](https://portal.azure.com), den Azure-Speicher PowerShell-Cmdlets, Azure-Speicher-Client-Bibliotheken oder die Azure Storage REST-API Azure Dateifreigaben erstellen. Darüber hinaus da Dateifreigaben SMB-Freigaben sind, können Sie über standard-Dateisystem-APIs zugreifen.

Dateispeicher basiert auf derselben Technologie wie Warteschlange, Tabelle und Blob-Speicher, damit Dateispeicher bietet die Verfügbarkeit, Zuverlässigkeit, Skalierbarkeit und Geo Redundanz, die in der Azure-Speicher-Plattform integriert ist. Details zur Datei Speicher Leistungsziele und Grenzwerte finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md).

Dateispeicher ist jetzt in der Regel verfügbar und sowohl die SMB 2.1 SMB 3.0 unterstützt. Weitere Details für den Dateispeicher finden Sie unter den [Datei-Dienst REST-API](https://msdn.microsoft.com/library/azure/dn167006.aspx).

>[AZURE.NOTE] Linux SMB-Client unterstützt Verschlüsselung, sodass Bereitstellen einer Dateifreigabe von Linux weiterhin erforderlich ist, dass der Client in der gleichen Azure Region als die Dateifreigabe noch nicht. Ist jedoch Verschlüsselung Unterstützung für Linux auf den Plan Linux Entwickler SMB Funktionalität verantwortlich an. Linux-Versionen, die in der Zukunft Verschlüsselung unterstützen werden kann eine Azure Dateifreigabe überall ebenfalls bereitgestellt.

## <a name="video-using-azure-file-storage-with-linux"></a>Video: Verwenden von Azure Dateispeicher mit Linux

Hier ist ein Video, das zum Erstellen und Verwenden von Azure Dateifreigaben auf Linux veranschaulicht.

> [AZURE.VIDEO azure-file-storage-with-linux]

## <a name="choose-a-linux-distribution-to-use"></a>Wählen Sie eine Linux Verteilung verwenden ##

Beim Erstellen eines Linux virtuellen Computers Azure können Sie ein Bild Linux angeben der SMB 2.1 oder höher aus der Bildergalerie Azure unterstützt. Es folgt eine Liste mit empfohlenen Linux Bilder:

- Ubuntu Server 14.04 +
- RHEL 7 +
- CentOS 7 +
- Debian 8
- OpenSUSE 13.2 +
- SUSE Linux Enterprise Server 12
- SUSE Linux Enterprise Server 12 (Premium Bild)

## <a name="mount-the-file-share"></a>Laden Sie die Dateifreigabe ##

Um die Dateifreigabe anhand eines virtuellen Computers mit Linux bereitzustellen, müssen Sie möglicherweise einen SMB/CIFS-Client installieren, wenn die Verteilung, die Sie verwenden einen integrierten Client aufweist. Hierbei handelt es sich um den Befehl aus Ubuntu, eine Auswahl Cifs-Utils zu installieren:

    sudo apt-get install cifs-utils

Als Nächstes müssen Sie stellen eine Zuordnung zeigen (Mkdir Mymountpoint), und klicken Sie dann den Bereitstellungsbefehl, der folgendermaßen aussieht:

     sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename ./mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777

Sie können auch die Einstellungen in der/etc/fstab, um die Freigabe bereitstellen hinzufügen.

Beachten Sie, dass 0777 hier einen Directory-Datei über die Berechtigung Code dargestellt werden, der Ausführung/Lese-und Schreibberechtigungen für alle Benutzer zu erhalten. Sie können es mit anderen Datei Berechtigung Code Linux Datei Berechtigung Dokument folgen ersetzen.

Wenn Sie eine Dateifreigabe bereitgestellt, die nach dem Neustart beibehalten möchten, können Sie auch eine Einstellung wie unten in der/etc/fstab hinzufügen:

    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777

Beispielsweise, wenn Sie einen Azure-virtuellen erstellt können Linux Bild Ubuntu Server 15.04 (das aus der Bildergalerie Azure zur Verfügung) verwenden, Sie die Datei wie folgt bereitstellen:

    azureuser@azureconubuntu:~$ sudo apt-get install cifs-utils
    azureuser@azureconubuntu:~$ sudo mkdir /mnt/mountpoint
    azureuser@azureconubuntu:~$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    azureuser@azureconubuntu:~$ df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

Wenn Sie CentOS 7.1 verwenden, können Sie die Datei wie folgt bereitstellen:

    [azureuser@AzureconCent ~]$ sudo yum install samba-client samba-common cifs-utils
    [azureuser@AzureconCent ~]$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    [azureuser@AzureconCent ~]$ df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

Wenn Sie öffnen SUSE 13.2 verwenden, können Sie die Datei wie folgt bereitstellen:

    azureuser@AzureconSuse:~> sudo zypper install samba*  
    azureuser@AzureconSuse:~> sudo mkdir /mnt/mountpoint
    azureuser@AzureconSuse:~> sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    azureuser@AzureconSuse:~> df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

## <a name="manage-the-file-share"></a>Verwalten der Dateifreigabe ##

Der [Azure-Portal](https://portal.azure.com) stellt eine Benutzeroberfläche für die Verwaltung von Dateispeicher Azure bereit. Sie können über den Webbrowser die folgenden Aktionen ausführen:

- Hochladen und Herunterladen von Dateien an und von der Dateifreigabe.
- Überwachen der tatsächlichen Verwendung der einzelnen Dateifreigabe.
- Passen Sie das Datei freigeben Größenkontingent.
- Kopieren der `net use` Befehl zu verwenden, um Ihre Dateifreigabe aus einem Windows-Client bereitzustellen.

Sie können auch der Azure Plattformen Line Interface (CLI Azure) von Linux verwenden, die Dateifreigabe verwalten. Azure CLI bietet eine Reihe von Quelle Plattform-Befehle für die Arbeit mit Azure-Speicher, einschließlich Dateispeicher öffnen. Es bietet im Wesentlichen die gleiche Funktionalität in Azure-Portal als auch Rich-Daten-Access-Funktionalität gefunden. Beispiele finden Sie unter [Verwendung der CLI Azure mit Azure-Speicher](storage-azure-cli.md).

## <a name="develop-with-file-storage"></a>Entwickeln mit Speicherung von Dateien ##

Als Entwickler können Sie die Anwendung mit Dateispeicher mithilfe der [Azure-Speicher-Client-Bibliothek für Java](https://github.com/azure/azure-storage-java)erstellen. Codebeispielen finden Sie unter [Verwenden von Java Dateispeicher](storage-java-how-to-use-file-storage.md).

Der [Azure-Speicher-Client-Bibliothek für Node.js](https://github.com/Azure/azure-storage-node) können Sie auch gegen Dateispeicher entwickeln.

## <a name="feedback-and-more-information"></a>Feedback und Weitere Informationen ##

Linux Benutzer, wir möchten von Ihnen hören!

Die Azure Dateispeicher für Linux-Benutzer-Gruppe bietet a-Forum für Sie Feedback geben Dateispeicher auf Linux übernehmen und ausgewertet werden soll. E-Mail- [Azure Datei Speicher Linux Benutzer](mailto:azurefileslinuxusers@microsoft.com) , die Benutzer Gruppe beizutreten.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter folgenden Links, um weitere Informationen zu Azure Dateispeicher.

### <a name="conceptual-articles-and-videos"></a>Konzeptionelle Artikeln und videos

- [Azure Dateien Speicher: eine frictionless Cloud SMB Dateisystem für Windows und Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
- [Erste Schritte mit auf Windows Azure-Datei-Speicher](storage-dotnet-how-to-use-files.md)

### <a name="tooling-support-for-file-storage"></a>Unterstützung für Dateispeicher Tools

- [Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)
- [Erstellen und Verwalten von Dateifreigaben](storage-azure-cli.md#create-and-manage-file-shares) mithilfe der Azure-CLI

### <a name="reference"></a>Bezug

- [Datei Service-REST-API-Referenz](http://msdn.microsoft.com/library/azure/dn167006.aspx)
- [Azure Dateien Artikel zur Problembehandlung](storage-troubleshoot-file-connection-problems.md)

### <a name="blog-posts"></a>Von Blogbeiträgen

- [Azure Dateispeicher ist jetzt in der Regel verfügbar](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
- [Innere Azure Dateispeicher](https://azure.microsoft.com/blog/inside-azure-file-storage/)
- [Einführung in Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
- [Beibehalten von Verbindungen mit Microsoft Azure-Dateien](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
