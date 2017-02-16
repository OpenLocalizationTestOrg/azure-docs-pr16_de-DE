<properties
    pageTitle="Automatische Skalieren Cloud-Dienst im Portal | Microsoft Azure"
    description="(klassisch) Erfahren Sie, wie in der klassischen Portal verwenden, um Regeln für automatische Skalierung für einen Cloud-Dienst Webrolle oder Worker-Rolle in Azure konfigurieren."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Automatisches Skalieren Cloud-Dienst

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-scale-portal.md)
- [Azure klassischen-portal](cloud-services-how-to-scale.md)

Auf der Seite skalieren des klassischen Azure Portals können Sie Ihre Webrolle oder Worker-Rolle manuell skalieren oder können Sie Aktivieren der automatischen Skalierung basierend auf CPU-Auslastung oder einer Warteschlange.

>[AZURE.NOTE] Dieser Artikel befasst sich Cloud-Dienst Web- und Worker Rollen. Wenn Sie direkt einen virtuellen Computern (klassisch) erstellen, wird es in einen Cloud-Dienst gehostet werden. Einige dieser Informationen gilt für diese Typen von virtuellen Computern. Eine Sammlung Verfügbarkeit von virtuellen Computern Skalierung ist eigentlich nur diese aktiviert und deaktiviert herunter basierend auf den Maßstab Regeln, die Sie konfigurieren. Weitere Informationen zu virtuellen Computern und Verfügbarkeit Mengen angezeigt werden finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Bevor Sie die Skalierung für eine Anwendung konfigurieren, berücksichtigen Sie die folgende Informationen:

- Skalierung ist durch Verwendung von Core betroffen. Verwenden Sie größere Rolleninstanzen mehr Kerne aus. Sie können eine Anwendung nur im Rahmen der Kerne für Ihr Abonnement skalieren. Angenommen, wenn Ihr Abonnement maximal zwanzig Adern und Ausführen eine Anwendung mit zwei mittlerer Größe aufweist Cloud services (insgesamt vier Kerne) und Sie können nur Dezimalstellen von anderen Cloud-Service-Bereitstellungen in Ihr Abonnement von 16 Kerne. Weitere Informationen zu Größen finden Sie unter [Größe der Cloud-Dienst](cloud-services-sizes-specs.md) .

- Sie müssen eine Warteschlange erstellen und mit einer Rolle verbinden, bevor eine Anwendung basierend auf einer Nachricht Schwellenwert skaliert werden kann. Weitere Informationen finden Sie unter [der Warteschlange-Speicherdienst verwenden](../storage/storage-dotnet-how-to-use-queues.md).

- Sie können Ressourcen skalieren, die in der Cloud-Service verknüpft sind. Weitere Informationen zum Verknüpfen von Ressourcen finden Sie unter [wie: verknüpfen eine Ressource in einen Cloud-Service](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Um hohe Verfügbarkeit der Anwendung zu aktivieren, sollten Sie sicherstellen, dass es mit zwei oder mehr Rolleninstanzen bereitgestellt wird. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Planen der Skalierung

Standardmäßig führen alle Rollen nicht auf einen bestimmten Zeitplan zu. Daher gelten keine geändert Einstellungen für alle Uhrzeiten und alle Wochentage im Verlauf des Jahres. Wenn Sie möchten, können Sie die manuelle oder automatische Skalierung für einrichten:

- Wochentage
- Wochenenden
- Nächte Woche
- Morgens Woche
- Bestimmte Datumsangaben
- Bestimmte Datumsbereiche

Dies ist Conigured im [Azure klassischen-Portal](https://manage.windowsazure.com/) an, klicken Sie auf der  
**Cloud Services** > **\[der Cloud-Dienst\]** > **Maßstab** > **\[Herstellung oder Staging\] ** Seite.

Klicken Sie auf die Schaltfläche **Einrichten von Planungszeiten** für jede Rolle, die Sie ändern möchten.

![Cloud Service automatische Skalierung basierend auf einem Zeitplan][scale_schedules]



## <a name="manual-scale"></a>Manuelle Skalierung

Klicken Sie auf der Seite **Skalieren** können Sie manuell erhöhen oder verringern die Anzahl der Instanzen in einen Cloud-Dienst ausgeführt. Dies wird für jeden von Ihnen erstellten Zeitplan oder die gesamte Zeit konfiguriert, wenn Sie einen Zeitplan nicht erstellt haben.

1. Im [Azure klassischen Portal](https://manage.windowsazure.com/) **Cloud Services**klicken Sie auf, und klicken Sie dann auf den Namen des Cloud-Dienst auf das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihre Cloud-Dienst angezeigt werden, müssen Sie von der **Herstellung** **Staging** oder umgekehrt ändern.

2. Klicken Sie auf **Skalieren**.

3. Wählen Sie den Zeitplan für Skalierungsoptionen ändern möchten. Standardmäßig *keine geplanten Zeiten* Wenn Sie keine Zeitpläne definiert haben.

4. Suchen Sie den Abschnitt **nach Metrisch skalieren** , und wählen Sie ****. Dies ist die Standardeinstellung für alle Rollen.

5. Jede Rolle in der Cloud-Dienst weist einen Schieberegler für das Ändern der Anzahl der Instanzen zu verwenden.

    ![Manuelles Skalieren einer Rolle der Cloud-Dienst][manual_scale]

    Wenn Sie weitere Instanzen benötigen, müssen Sie die [Größe des Cloud-Dienst virtuellen Computers](cloud-services-sizes-specs.md)zu ändern.

6. Klicken Sie auf **Speichern**.  
Rolleninstanzen werden hinzugefügt oder entfernt basierend auf Ihrer Auswahl geändert.

>[AZURE.TIP] Wenn Sie finden Sie unter ![][tip_icon] bewegen Sie die Maus darauf, und Sie können Hilfe zu welche eine bestimmte Einstellung enthält.


## <a name="automatic-scale---cpu"></a>Automatische Skalierung - CPU

Wenn der durchschnittliche Prozentsatz der CPU-Auslastung über oder unter dem angegebenen Schwellenwerte wechselt skaliert; Rolleninstanzen werden erstellt oder gelöscht.

1. Im [Azure klassischen Portal](https://manage.windowsazure.com/) **Cloud Services**klicken Sie auf, und klicken Sie dann auf den Namen des Cloud-Dienst auf das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihre Cloud-Dienst angezeigt werden, müssen Sie von der **Herstellung** **Staging** oder umgekehrt ändern.

2. Klicken Sie auf **Skalieren**.

3. Wählen Sie den Zeitplan für Skalierungsoptionen ändern möchten. Standardmäßig *keine geplanten Zeiten* Wenn Sie keine Zeitpläne definiert haben.

4. Suchen Sie den Abschnitt **nach Metrisch skalieren** , und wählen Sie die **CPU**.

5. Jetzt können Sie einen Mindest- und Höchstwerten Zellbereich Rolleninstanzen, das Ziel CPU-Auslastung (auf eine Skala von auslösen), und wie viele Instanzen nach oben und unten Skalieren konfigurieren.

![Skalieren einer Rolle der Cloud-Dienst durch hohe CPU-Auslastung][cpu_scale]

>[AZURE.TIP] Wenn Sie finden Sie unter ![][tip_icon] bewegen Sie die Maus darauf, und Sie können Hilfe zu welche eine bestimmte Einstellung enthält.





## <a name="automatic-scale---queue"></a>Automatische Skalierung - Warteschlange

Dies wird automatisch skaliert, wenn die Anzahl der Nachrichten in einer Warteschlange Ober- oder unterhalb eines bestimmten Schwellenwert überschreitet; Rolleninstanzen werden erstellt oder gelöscht.

1. Im [Azure klassischen Portal](https://manage.windowsazure.com/) **Cloud Services**klicken Sie auf, und klicken Sie dann auf den Namen des Cloud-Dienst auf das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihre Cloud-Dienst angezeigt werden, müssen Sie von der **Herstellung** **Staging** oder umgekehrt ändern.

2. Klicken Sie auf **Skalieren**.

3. Suchen Sie den Abschnitt **nach Metrisch skalieren** , und wählen Sie die **CPU**.

4. Jetzt können Sie einen Bereich Mindest- und Höchstwerten der Rolleninstanzen, die Warteschlange und den Umfang der Nachrichten in Warteschlange für jede Instanz und wie viele Instanzen nach oben und unten skalieren Verarbeitungszeit konfigurieren.

![Skalieren einer Rolle der Cloud-Dienst, indem Sie eine Nachrichtenwarteschlange][queue_scale]

>[AZURE.TIP] Wenn Sie finden Sie unter ![][tip_icon] bewegen Sie die Maus darauf, und Sie können Hilfe zu welche eine bestimmte Einstellung enthält.


## <a name="scale-linked-resources"></a>Verknüpfte Ressourcen skalieren

Häufig, wenn Sie eine Rolle skalieren, ist es von Vorteil, die Datenbank zu skalieren, die auch die Anwendung verwendet wird. Wenn Sie die Datenbank mit der Cloud-Dienst verknüpfen, können Sie die Skalierung Einstellungen für die betreffende Ressource, indem Sie auf die entsprechende Verknüpfung zugreifen.

1. Im [Azure klassischen Portal](https://manage.windowsazure.com/) **Cloud Services**klicken Sie auf, und klicken Sie dann auf den Namen des Cloud-Dienst auf das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihre Cloud-Dienst angezeigt werden, müssen Sie von der **Herstellung** **Staging** oder umgekehrt ändern.

2. Klicken Sie auf **Skalieren**.

3. Suchen Sie im Abschnitt **Verknüpfte Ressourcen** und geklickt haben, klicken Sie auf **Verwalten Maßstab für diese Datenbank**.

    > [AZURE.NOTE] Wenn Sie einen Abschnitt **Verknüpfte Ressourcen** angezeigt werden, verfügen Sie alle verknüpften Ressourcen wahrscheinlich nicht.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
