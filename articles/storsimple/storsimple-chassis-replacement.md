<properties 
   pageTitle="Ersetzen des Gehäuses auf einem Gerät StorSimple | Microsoft Azure"
   description="Beschreibt das Entfernen und Ersetzen Sie das Gehäuse für Ihre primäre Einheit StorSimple oder EBOD Einheit."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Ersetzen des Gehäuses auf Ihrem Gerät StorSimple

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird erläutert, wie entfernen und einem Gehäuse in einem StorSimple 8000 Reihe Gerät ersetzen. Das Modell StorSimple 8100 ist eine einzelne Einheit Gerät (ein Gehäuse), während das 8600 ein Gerät mit zwei Einheit (zwei Gehäuse) wird. Für ein Modell 8600 potenziell zwei Gehäuse, die in das Gerät nicht konnte vorhanden sind: das Gehäuse für die primäre Einheit oder dem Gehäuse für die Anlage EBOD.

In beiden Fällen ist die Austauschgehäuse an seinen, die von Microsoft ausgeliefert ist leer. Keine Power und Kühlmodule (PCMs), Controller Module, einfarbige Zustand Laufwerke (SSDs), Festplatten (Festplatten) oder Module EBOD werden aufgenommen.

>[AZURE.IMPORTANT] Überprüfen Sie vor dem entfernen, und ersetzen die Rahmen, die Sicherheitsinformationen in den [Austausch von StorSimple Hardware Komponenten](storsimple-hardware-component-replacement.md).

## <a name="remove-the-chassis"></a>Entfernen Sie die Rahmen

Führen Sie die folgenden Schritte aus, um das Gehäuse auf Ihrem Gerät StorSimple zu entfernen.

#### <a name="to-remove-a-chassis"></a>So entfernen Sie ein Gehäuse

1. Stellen Sie sicher, dass das Gerät StorSimple fahren und von allen Power Quellen getrennt.

2. Entfernen Sie alle Netzwerk- und SAS-Kabel, falls zutreffend.

3. Entfernen der Maßeinheit aus der Shapes für Gestelle an.

4. Entfernen Sie jedes der Laufwerke zu, und notieren Sie die Steckplätze, aus denen sie entfernt werden. Weitere Informationen finden Sie unter [dem Festplattenlaufwerk entfernen](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Klicken Sie auf die Anlage EBOD (sofern dies den Rahmen, die fehlgeschlagen ist), entfernen Sie die EBOD Controller Module. Weitere Informationen finden Sie unter [entfernen einen EBOD Controller](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Klicken Sie auf die primäre Einheit (sofern dies den Rahmen, die fehlgeschlagen ist), entfernen Sie die Controller, und notieren Sie die Steckplätzen aus dem entfernt. Weitere Informationen finden Sie unter [entfernen einen Controller](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Installieren Sie die Rahmen

Führen Sie die folgenden Schritte aus, um den Rahmen auf Ihrem Gerät StorSimple zu installieren.

#### <a name="to-install-a-chassis"></a>So installieren Sie ein Gehäuse

1. Stellen Sie in der Shapes für Gestelle Gehäuses bereit. Weitere Informationen finden Sie unter [Ihrem Gerät StorSimple 8100 Rackmontage](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) oder [Ihrem Gerät StorSimple 8600 Rackmontage](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Nachdem Sie die Rahmen in die Shapes für Gestelle bereitgestellt werden, installieren Sie die Controller-Module in der gleichen Positionen, denen sie zuvor in installiert wurden.

3. Installieren Sie die Laufwerke in der gleichen Positionen und Steckplätze, denen sie zuvor in installiert wurden.

    >[AZURE.NOTE] Es empfiehlt sich, dass Sie die SSDs zuerst in den Steckplätzen installieren, und installieren Sie die Festplatten.

2. Schließen Sie mit dem Gerät bereitgestellt, die Shapes für Gestelle und die installierten Komponenten Ihr Gerät in den entsprechenden Power Quellen, und aktivieren Sie das Gerät. Details finden Sie unter [Ihrem Gerät StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) oder [Kabel Ihrem Gerät StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den [Austausch von StorSimple Hardware Komponenten](storsimple-hardware-component-replacement.md).

