<properties
   pageTitle="Konfigurieren Sie ein Azure-Cloud-Service-Projekt mit Visual Studio | Microsoft Azure"
   description="Erfahren Sie, wie ein Azure-Cloud-Service-Projekt in Visual Studio, je nach Ihren Anforderungen für dieses Projekt zu konfigurieren."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurieren Sie ein Azure-Cloud-Service-Projekt mit Visual Studio

Sie können ein Azure-Cloud-Service-Projekt, je nach Ihren Anforderungen für dieses Projekt konfigurieren. Sie können die Eigenschaften für das Projekt für die folgenden Kategorien festlegen:

- **Veröffentlichen eines Cloud-Diensts in Azure**

  Sie können festlegen, dass eine Eigenschaft aus, um sicherzustellen, dass ein vorhandene Cloud-Dienst auf Azure bereitgestellt, die nicht versehentlich gelöscht wird.

- **Führen Sie aus oder Debuggen Sie einen Clouddienst auf dem lokalen computer**

  Sie können auswählen, eine Dienstkonfiguration zu verwenden, und geben Sie an, ob Azure Speicheremulator beginnen soll.

- **Überprüfen Sie bei der Erstellung einer Cloud-Service-Paket**

  Sie können entscheiden, um alle Warnungen als Fehler behandelt, sodass Sie sicherstellen, dass das Cloud-Service-Paket ohne Probleme bereitgestellt wird. Dies reduziert Wartezeit, wenn Sie beim Bereitstellen und erfahren Sie, dass ein Fehler aufgetreten.

Die folgende Abbildung zeigt so zu verwenden, wenn Sie ausführen oder dem Clouddienst lokal Debuggen Konfiguration auswählen. Sie können eine der Projekteigenschaften, die Sie in diesem Fenster erfordern festlegen, wie in der Abbildung dargestellt.

![Konfigurieren eines Microsoft Azure-Projekts](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>So konfigurieren Sie ein Azure-Cloud-Service-Projekt

1. Um ein Projekt aus der **Lösung Explorer**Cloud-Dienst konfigurieren möchten, öffnen Sie das Kontextmenü für das Projekt für den Cloud-Dienst, und wählen Sie dann auf **Eigenschaften**.

  Eine Seite mit dem Namen des Projekts Cloud-Dienst wird im Visual Studio-Editor.

1. Wählen Sie die Registerkarte **Entwicklung** aus.

1. Wählen Sie **True**aus, um sicherzustellen, dass Sie eine vorhandene Bereitstellung in Azure, bei der Aufforderung, vor dem Löschen einer vorhandenen Liste für die Bereitstellung versehentlich löschen nicht.

1. Zum auswählen die Dienstkonfiguration, die Sie beim Ausführen oder lokal, Ihre Cloud-Dienst in der Liste **Dienstkonfiguration Debuggen** verwenden möchten, wählen Sie die Dienstkonfiguration.

  >[AZURE.NOTE] Wenn Sie eine Dienstkonfiguration verwenden, finden Sie unter erstellen möchten so: Verwalten von Dienstkonfigurationen und Profile. Wenn Sie eine Dienstkonfiguration für eine Rolle ändern möchten, finden Sie unter [So konfigurieren Sie die Rollen für einen Azure-Cloud-Dienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Wählen Sie zum Azure Speicheremulator starten, wenn Sie ausführen oder lokal, Ihre Cloud-Dienst in der **Speicheremulator Azure starten Debuggen**, **Wahr**.

1. Um sicherzustellen, dass Sie veröffentlicht werden können, wenn in **behandeln Warnungen als fehlerhaft**Überprüfungsfehler Paket, vorhanden sind, wählen Sie **True**.

1. Wählen Sie **True**aus, um sicherzustellen, dass Ihre Webrolle denselben Port jedes Mal verwendet, die ihn lokal in IIS Express **verwenden Web Project Ports**, gestartet. Um einen bestimmten Port für ein bestimmtes Webprojekt verwenden möchten, öffnen Sie das Kontextmenü für das Webprojekt, wählen Sie die Registerkarte **Eigenschaften** aus, wählen Sie die Registerkarte **Web** und ändern Sie die Port-Nummer in die **Project-Url** -Einstellung im Abschnitt **IIS Express** . Geben Sie zum Beispiel `http://localhost:14020` als Project-URL.

1. Um alle Änderungen zu speichern, die Sie in den Eigenschaften des Projekts Cloud-Dienst vorgenommen haben, wählen Sie auf die Symbolleiste auf die Schaltfläche **Speichern** .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konfigurieren der Azure-Cloud-Dienstprojekte in Visual Studio finden Sie unter [Konfigurieren Ihrer Azure-Projekt mit mehreren Dienstkonfigurationen](vs-azure-tools-multiple-services-project-configurations.md).
