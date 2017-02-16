<properties
    pageTitle="Stellen Sie das Laufwerk D: eines virtuellen Computers einen Datenträger | Microsoft Azure"
    description="Beschreibt, wie Laufwerkbuchstaben für einen Windows-virtuellen ändern, sodass Sie das Laufwerk D: als Datenlaufwerk verwenden können."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Verwenden Sie das Laufwerk D: als Datenlaufwerk auf einen Windows-virtuellen 

Wenn die Anwendung muss sich auf Laufwerk D zum Speichern von Daten verwenden, befolgen Sie diese Anweisungen mit einem anderen Laufwerkbuchstaben für den temporären Datenträger aus. Verwenden Sie nie den temporären Datenträger zum Speichern von Daten, die Sie beibehalten müssen.

Wenn Sie die Größe oder **Beenden (Deallocate)** eines virtuellen Computers, kann dies Platzierung des virtuellen Computers zu einer neuen Hypervisor auslösen. Ein Ereignis geplanten oder nicht geplanten Wartung möglicherweise auch diese Platzierung auslösen. In diesem Szenario wird der temporäre Datenträger des ersten Buchstabens der verfügbaren Laufwerk zugewiesen. Wenn Sie eine Anwendung, die speziell Laufwerk D: erforderlich sind haben, müssen Sie Sie wie folgt vor, um vorübergehend pagefile.sys verschieben, fügen Sie einen neuen Datenträger und Zuweisen des Buchstabens D und dann pagefile.sys zurück in das temporäre Laufwerk verschieben. Sobald Sie fertig sind, dauert Azure nicht wieder die D: Wenn der virtuellen Computer auf einen anderen Hypervisor verschoben wird.

Weitere Informationen darüber, wie Azure der temporären Datenträger verwendet finden Sie unter [Grundlegendes zu den temporären Laufwerk auf Microsoft Azure virtuellen Computern](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Den Datenträger Daten anfügen

Zuerst müssen Sie den Daten Datenträger des virtuellen Computers installieren. 

- Wenn das Portal verwenden möchten, finden Sie unter [So fügen Sie einen Datenträger Azure-Portal](virtual-machines-windows-attach-disk-portal.md)
- Wenn das klassische Portal verwenden möchten, finden Sie unter [einen Datenträger Daten auf einem Windows-Computer installieren](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Verschieben Sie pagefile.sys vorübergehend zu Laufwerk C

1. Verbinden Sie mit des virtuellen Computers. 

2. Mit der rechten Maustaste im Menüs **Start** , und wählen Sie **System**.

3. Wählen Sie im linken Menü **Erweiterte Systemeinstellungen**aus.

4. Wählen Sie im Abschnitt **Leistung** **Einstellungen**aus.

5. Wählen Sie die Registerkarte **Erweitert** .

5. Wählen Sie im Abschnitt **virtueller Speicher** **Ändern**.

6. Wählen Sie das Laufwerk **C** , und klicken Sie dann auf **Größe wird vom System verwaltet** , und klicken Sie dann auf **festlegen**.

7. Wählen Sie das Laufwerk **D** , und klicken Sie dann auf **keine Seitennavigation-Datei** , und klicken Sie dann auf **festlegen**.

8. Klicken Sie auf Übernehmen. Sie erhalten eine Warnung, dass der Computer neu gestartet werden, damit die Änderungen wirksam muss.

9. Starten Sie den virtuellen Computer neu.




## <a name="change-the-drive-letters"></a>Ändern Sie die Laufwerkbuchstaben 

1. Nachdem Sie der virtuellen Computer neu gestartet wird, melden Sie sich wieder an, um den virtuellen Computer.

2. Klicken Sie im Menü **Start** auf, und geben Sie **diskmgmt.msc** , und drücken Sie die EINGABETASTE. Datenträger Verwaltung wird gestartet.

3. Mit der rechten Maustaste auf **D**, das Laufwerk temporäre Speicher, und wählen Sie **Ändern Laufwerkbuchstaben und Pfade**.

4. Klicken Sie unter Laufwerkbuchstaben wählen Sie Laufwerk **G** aus, und klicken Sie dann auf **OK**. 

5. Mit der rechten Maustaste auf dem Datenträger Daten, und wählen Sie **Ändern Laufwerksbuchstaben und Pfade**.

6. Klicken Sie unter Laufwerkbuchstaben wählen Sie Laufwerk **D** aus, und klicken Sie dann auf **OK**. 

7. Mit der rechten Maustaste auf **G**, das Laufwerk temporäre Speicher, und wählen Sie **Ändern Laufwerkbuchstaben und Pfade**.

8. Klicken Sie unter Laufwerkbuchstaben wählen Sie Laufwerk **E** aus, und klicken Sie dann auf **OK**. 

> [AZURE.NOTE] Wenn Ihre virtuellen Computer andere Datenträger oder Laufwerke ist, weisen die Laufwerkbuchstaben anderen Datenträger und Laufwerke mithilfe der gleichen Methode. Die Datenträgerkonfiguration werden soll:  
>- C: OS Datenträger  
>- D: Datenträger  
>- E:) temporärer Speicherplatz



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Verschieben von pagefile.sys wieder in das Speicherlaufwerk temporäre 

1. Mit der rechten Maustaste im Menüs **Start** , und wählen Sie **System**

2. Wählen Sie im linken Menü **Erweiterte Systemeinstellungen**aus.

3. Wählen Sie im Abschnitt **Leistung** **Einstellungen**aus.

4. Wählen Sie die Registerkarte **Erweitert** .

5. Wählen Sie im Abschnitt **virtueller Speicher** **Ändern**.

6. Wählen Sie das OS-Laufwerk **C** , und klicken Sie auf **die Datei keine Seitennavigation** , und klicken Sie dann auf **festlegen**.

7. Wählen Sie das temporäre Speicherung Laufwerk **E** , und klicken Sie dann auf **Größe wird vom System verwaltet** , und klicken Sie dann auf **festlegen**.

8. Klicken Sie auf **Übernehmen**. Sie erhalten eine Warnung, dass der Computer neu gestartet werden, damit die Änderungen wirksam muss.

9. Starten Sie den virtuellen Computer neu.




## <a name="next-steps"></a>Nächste Schritte
- Sie können verfügbarer Festplattenspeicher zu Ihrer virtuellen Computern durch [einen zusätzlichen Datenträger anfügen](virtual-machines-windows-attach-disk-portal.md)erhöhen.



