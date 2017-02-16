<properties 
    pageTitle="Bereitstellen eine Cloud-Service-Bereitstellung (Node.js) | Microsoft Azure" 
    description="Informationen Sie zum Bereitstellen der Azure-Anwendung auf einer staging-Umgebung und dann auf einer Herstellung-VIP (Virtual IP) austauschen." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Das Staging von einer Anwendungs in Azure

Um das staging-Umgebung in Azure getestet werden, bevor Sie es in dieser Umgebung verschieben in denen die Anwendung im Internet zugänglich ist, kann eine Anwendung bereitgestellt werden. Die staging-Umgebung ist genau wie dieser Umgebung, mit der Ausnahme, dass Sie nur die bereitgestellte Anwendung mit einer verborgene URL zugreifen können, die von Azure generiert wird. Nachdem Sie überprüft haben, dass die Anwendung ordnungsgemäß funktioniert, können sie in dieser Umgebung bereitgestellt werden, nach Durchführung einer VIP (Virtual IP) austauschen.

> [AZURE.NOTE] Die Schritte in diesem Artikel gelten nur für Knoten Applikationen als Azure-Cloud-Dienst gehostet.

## <a name="step-1-stage-an-application"></a>Schritt 1: Phaseneigenschaften Sie Anwendung

Diese Aufgabe umfasst, wie Sie eine Anwendung mithilfe des **Microsoft Azure PowerShell**Phaseneigenschaften.

1.  Übergeben, wenn Sie einen Dienst veröffentlichen, einfach die **-Slot** mit dem **Veröffentlichen-AzureServiceProject** -Cmdlet-Parameter.

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Melden Sie sich bei der [Azure klassischen Portal] , und wählen Sie die **Cloud-Dienste**. Nach der Cloud-Dienst erstellt wird, und der **Staging** Spalte Status für die **Ausführung von**aktualisiert wurde, klicken Sie auf den Dienstnamen.

    ![Portal zur Anzeige eines ausgeführten Diensts][cloud-service]

3.  Wählen Sie das **Dashboard**, und wählen Sie dann das **Staging**aus.

    ![Cloud Service-dashboard][cloud-service-dashboard]

4. Notieren Sie den Wert in den Eintrag **URL-Website** nach rechts. Der DNS-Name ist eine verborgene internen ID, Azure generiert.

    ![Website-url][cloud-service-staging-url]

Nun können Sie überprüfen, dass die Anwendung in das staging-Umgebung ordnungsgemäß arbeitet, mit dem staging Website-URL.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Schritt 2: Aktualisieren einer Anwendung Herstellung durch Auslagern VIPs

Nachdem Sie die aktualisierte Version der Anwendung in das staging-Umgebung überprüft haben, können Sie schnell in Herstellung zurücklegen durch die virtuelle IP-Adressen (VIPs) für das Staging und Herstellung Umgebungen austauschen.

> [AZURE.NOTE] Dieser Schritt setzt voraus, dass Sie bereits eine Anwendung Herstellung bereitgestellt und die aktualisierte Version der Anwendung bereitgestellt haben.

1.  Melden Sie sich bei der [Azure klassischen Portal]auf **Cloud Services** und wählen Sie den Dienstnamen.

2.  Aus dem **Dashboard** **Staging**wählen Sie aus, und klicken Sie dann auf am unteren Rand der Seite **austauschen** . Daraufhin wird das Dialogfeld VIP austauschen.

    ![VIP austauschen Dialogfeld][vip-swap-dialog]

3.  Überprüfen Sie die Informationen ein, und klicken Sie dann auf **OK**. Die zwei Bereitstellungen beginnen, während die Bereitstellung staging zum Herstellung wechselt und die Herstellung Bereitstellung zum Staging wechselt aktualisieren.

Sie haben erfolgreich eine Bereitstellung bereitgestellt und eine Bereitstellung Herstellung durch Austauschen VIPs mit für die Bereitstellung Staging aktualisiert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [So Herstellung durch Austauschen VIPs in Azure ein Dienstupgrades bereitstellen]

[Azure klassischen-portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[So Herstellung durch Austauschen VIPs in Azure ein Dienstupgrades bereitstellen]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
