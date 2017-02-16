<properties
   pageTitle="So behalten Sie eine Konstante virtuelle IP-Adresse für einen Clouddienst | Microsoft Azure"
   description="Erfahren Sie, wie Sie sicherstellen, dass die virtuelle IP-Adresse von Ihrem Azure-Cloud-Dienst nicht geändert wird."
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

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>So behalten Sie eine Konstante virtuelle IP-Adresse für einen Clouddienst

Wenn Sie einen Clouddienst, der in Azure gehostet wird aktualisieren, müssen Sie sicherstellen, dass die virtuelle IP-Adresse des Diensts nicht geändert wird. Viele Management-Domänendienste verwenden Domain Name System (DNS) für die Registrierung von Domänennamen ein. DNS funktioniert nur, wenn die VIP unverändert bleibt. Sie können des **Veröffentlichen-Assistenten** in Azure-Tools verwenden, um sicherzustellen, dass die VIP von Ihrem Cloud-Dienst geändert wird, wenn Sie ihn zu aktualisieren. Weitere Informationen dazu, wie Sie DNS Domain Management für Clouddienste verwenden finden Sie unter [Konfigurieren Sie einen benutzerdefinierten Domänennamen für einen Azure-Cloud-Dienst](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Veröffentlichen eines Cloud-Diensts ohne seine VIP ändern

Wenn Sie zuerst auf Azure in einer bestimmten Umgebung, wie dieser Umgebung bereitstellen, wird die VIP des Cloud-Dienst zugewiesen. Die VIP ändern nicht, es sei denn, die Bereitstellung explizit löschen oder implizit durch die Bereitstellung Aktualisierungsvorgang gelöscht wird. Wenn die VIP beibehalten möchten, müssen Sie nicht die Bereitstellung löschen, und Sie müssen außerdem sicherstellen, dass die Bereitstellung von Visual Studio automatisch löschen nicht. Sie können das Verhalten steuern, indem Sie die Einstellungen für die Bereitstellung des **Veröffentlichen-Assistenten**, dem unterstützt mehrere Bereitstellungsoptionen angeben. Sie können angeben, einen frisch oder eine Update-Bereitstellung, die inkrementell oder gleichzeitiges sein kann, und beider Arten von Update-Bereitstellungen beibehalten die VIP. Definitionen der folgenden Arten von Bereitstellung finden Sie unter [Veröffentlichen Azure-Anwendung-Assistenten](vs-azure-tools-publish-azure-application-wizard.md).  Darüber hinaus können Sie steuern, ob die vorherige Bereitstellung von Cloud-Dienst gelöscht wird, wenn ein Fehler auftritt. Die VIP möglicherweise unerwartet ändern, wenn Sie diese Option nicht ordnungsgemäß festlegen.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Cloud-Dienst aktualisieren, ohne deren VIP ändern

1. Nachdem Sie mindestens einmal Ihre Cloud-Dienst bereitstellen, öffnen Sie das Kontextmenü für den Knoten für Ihr Projekt Azure, und wählen Sie dann auf **Veröffentlichen**. Der **Azure-Anwendung veröffentlichen** -Assistent wird angezeigt.

1. Klicken Sie in der Liste der Abonnements wählen Sie aus, die Sie bereitstellen möchten, und wählen Sie dann auf die Schaltfläche **Weiter** . Die Seite " **Einstellungen** " des Assistenten wird angezeigt.

1. Stellen Sie sicher, dass der Name des Cloud-Dienst, der Sie bereitstellen, der **Umgebung**, die **Konfiguration erstellen**und die **Dienstkonfiguration** alles richtig sind, klicken Sie auf der Registerkarte **Allgemeine Einstellungen** .

1. Stellen Sie sicher, dass das Speicherkonto und die Bezeichnung Bereitstellung, dass das Kontrollkästchen **Bereitstellung bei einem Fehler löschen** deaktiviert ist und das Kontrollkästchen Update **Bereitstellung** ausgewählt ist richtig sind, klicken Sie auf der Registerkarte **Erweiterte Einstellungen** . Indem Sie das Kontrollkästchen aktualisieren **Bereitstellung** auswählen, stellen Sie sicher, dass Ihre Bereitstellung wird nicht gelöscht und Ihre VIP nicht verloren werden, wenn Sie die Anwendung erneut veröffentlichen. Wenn Sie das **Kontrollkästchen bei Fehler Bereitstellung löschen**deaktivieren, stellen Sie sicher, dass Ihre VIP wird nicht gelöscht werden, falls ein Fehler, während der Bereitstellung auftritt.

1. Um weitere festzulegen, wie Sie die Rollen aktualisiert werden sollen, wählen Sie den Link " **Einstellungen** " neben dem Feld **Bereitstellung aktualisieren** aus, und wählen Sie dann entweder auf die inkrementell aktualisieren oder gleichzeitiges Aktualisierungsoption in das Dialogfeld Einstellungen **Bereitstellung aktualisieren** . Jede Instanz wird aktualisiert, wenn Sie inkrementell aktualisieren auswählen, einzeln nacheinander, damit die Anwendung immer verfügbar ist. Wenn Sie gleichzeitiges Aktualisieren auswählen, werden alle Instanzen zur gleichen Zeit aktualisiert. Gleichzeitiges aktualisieren schneller, aber der Dienst möglicherweise nicht zur Verfügung für die Aktualisierung auswählen.

1. Wenn Sie Ihre Einstellungen sind, wählen Sie die Schaltfläche **Weiter** .

1. Klicken Sie auf der Seite **Zusammenfassung** des Assistenten überprüfen Sie Ihre Einstellungen, und wählen Sie dann auf die Schaltfläche **Veröffentlichen** .

  >[AZURE.WARNING] Wenn die Bereitstellung fehlschlägt, sollten Sie Adresse, warum es fehlgeschlagen ist und umgehend, erneut, zur Vermeidung von Ihrem Cloud-Dienst in einem beschädigten Zustand verlassen.

## <a name="next-steps"></a>Nächste Schritte

Zur Veröffentlichung in Azure von Visual Studio finden Sie unter [Veröffentlichen Azure-Anwendung-Assistenten](vs-azure-tools-publish-azure-application-wizard.md).
