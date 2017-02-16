<properties
    pageTitle=" Erstellen Sie ein Azure Media Services-Konto mit dem Portal Azure | Microsoft Azure"
    description="In diesem Lernprogramm führt Sie durch die Schritte zum Erstellen eines Azure Media Services-Kontos mit Azure-Portal an."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen Sie ein Azure Media Services-Konto über das Azure-portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Azure-Portal bietet eine Möglichkeit, um schnell ein Konto Azure Media Services (AMS) zu erstellen. Ihr Konto können Sie Media-Dienste zugreifen, mit denen Sie zu speichern, verschlüsseln, codieren, verwalten und Medieninhalten in Azure übertragen. Zur Erstellung eines Media Services-Kontos Zeit, Sie auch erstellen Sie ein Speicherkonto zugeordneten (oder verwenden Sie eine vorhandene) in der gleichen geografische Region als das Konto Media-Dienste.

In diesem Artikel wird erläutert, gängige Konzepte und gezeigt, wie ein Konto Media-Dienste mit dem Azure-Portal zu erstellen.

## <a name="concepts"></a>Konzepte

Zugreifen auf Medienservices erfordert zwei verknüpften Konten:

- Ein Konto Media-Dienste. Ihr Konto erhalten Sie Zugriff auf eine Reihe von cloudbasierten Media-Dienste, die in Azure verfügbar sind. Ein Konto Media-Dienste werden keine tatsächlichen Medieninhalten gespeichert. Stattdessen werden Metadaten zu Medieninhalten und Medien Verarbeitungsaufträge in Ihrem Konto gespeichert. Zum Zeitpunkt der Erstellung des Kontos, wählen Sie eine verfügbare Media-Dienste Region aus. Das von Ihnen ausgewählte Region ist einem Data Center, in dem die Einträge Metadaten für Ihr Konto gespeichert.

    Verfügbare Medien Services (AMS) Regionen zählen folgende: North Europe, Westen Europe, Westen US, ostasiatischen US, oder Asien, Ostasien, Japan Westen, Japan Osten. Media-Dienste werden keine Gruppen verwendet.
    
    AMS ist jetzt auch in den folgenden Daten Centers: Brasilien Süd, Indien "Westen", Indien Süd und Indien zentralen. Sie können nun mithilfe das Azure-Portal Medien Dienstkonten erstellen und Ausführen von verschiedenen Aufgaben, die hier beschriebenen. Live-Codierung ist jedoch in diese Data Center nicht aktiviert. Darüber hinaus sind nicht alle Arten von reservierte Einheiten Codierung in diese Data Center zur Verfügung.
    
    - Brasilien Süd: Nur Standard- und grundlegende Codierung reservierte Einheiten verfügbar sind.
    - Westen, Indien Indien Süd: 

- Ein Konto Azure-Speicher. Speicherkonten müssen in der gleichen geografische Region als das Konto Media-Dienste befinden. Wenn Sie ein Konto Media-Dienste zu erstellen, Sie können entweder ein vorhandenes Speicherkonto in derselben Region auswählen, oder Sie können ein neues Speicherkonto in derselben Region erstellen. Wenn Sie ein Konto Media-Dienste löschen, werden die Blobs in Ihr Speicherkonto verknüpften nicht gelöscht.

## <a name="create-an-ams-account"></a>Erstellen Sie ein Konto AMS

Die Schritte in diesem Abschnitt zeigen, wie ein Konto AMS zu erstellen.

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **+ neue** > **Web + Mobile** > **Media-Dienste**.

    ![Media-Dienste zu erstellen.](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Kontos zu erstellen** .

    ![Media-Dienste zu erstellen.](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie in das Feld **Kontoname**den Namen des neuen AMS-Kontos ein. Ein Kontonamen Media-Dienste ist alle Kleinbuchstaben Ziffern oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement zwischen den verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. Wählen Sie in der **Ressourcengruppe**der neuen oder vorhandenen Ressource ein.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien gemeinsam nutzen. Weitere finden Sie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Wählen Sie **Speicherort**das geografische Region, das zum Speichern der Datensätze Medien und Metadaten für Ihr Konto Media-Dienste verwendet werden. Diese Region wird zum Verarbeiten und Streamen von Medien verwendet werden. Die verfügbaren Media-Dienste Regionen werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie in **Speicher-Konto**ein Speicherkonto, um Blob-Speicher des Inhalts Medien über Ihr Konto Media-Dienste bereitzustellen. Sie können ein vorhandenes Speicherkonto in der gleichen geografische Region als Ihr Konto Media-Dienste auswählen, oder Sie können ein Speicherkonto erstellen. Ein neues Speicherkonto wird in der gleichen Region erstellt. Die Regeln für Speicher Kontonamen sind die gleichen wie bei Medien Dienstkonten.

        Weitere Informationen zu Speicher [hier](storage-introduction.md).

    4. Wählen Sie die **Pin zum Dashboard** , um den Fortschritt der Bereitstellung Konto finden Sie unter.
    
7. Klicken Sie auf **Erstellen** am unteren Rand des Formulars.

    Nachdem das Konto erfolgreich erstellt wurde, ändert sich der Status für die **Ausführung von**. 

    ![Media-Dienste-Einstellungen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Verwalten Sie Ihr Konto AMS (z. B. Hochladen von Videos, Codieren von Anlagen, Überwachung des Projektstatus) verwenden Sie das Fenster **Einstellungen** .

## <a name="manage-keys"></a>Verwalten von Tasten

Sie benötigen den Namen des Kontos und die Informationen zu Primärschlüsseln der Media Services-Kontos programmgesteuert Zugriff auf.

1. Wählen Sie im Portal Azure Ihr Konto ein. 

    Das Fenster **Einstellungen** wird auf der rechten Seite angezeigt. 

2. Wählen Sie im Fenster **Einstellungen** **Schlüssel**aus. 

    Windows **Verwalten Tasten** zeigt den Kontonamen und der primären und sekundären Schlüssel wird angezeigt. 
3. Drücken Sie die Schaltfläche Kopieren, um die Werte zu kopieren.
    
    ![Tasten der Media-Dienste](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Nächste Schritte

Sie können nun Dateien in Ihr Konto AMS hochladen. Weitere Informationen finden Sie unter [Hochladen von Dateien](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


