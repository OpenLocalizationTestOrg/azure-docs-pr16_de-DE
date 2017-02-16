<properties 
   pageTitle="Für das Debuggen Azure Cloud Services | Microsoft Azure"
   description="Debuggen von Azure-Cloud-Diensten"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Debuggen von Clouddiensten

Verschiedene Ansätze können Sie die Anwendung Azure Debuggen mithilfe der Azure-Tools für Microsoft Visual Studio und das SDK Azure:

- Sie können eine Azure-Anwendung von Visual Studio bei der Entwicklung, Debuggen, wie Sie eine beliebige Visual c# oder Visual Basic-Anwendung. Weitere Informationen finden Sie unter [Debuggen von Ihrem Cloud-Dienst auf Ihrem lokalen Computer](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Azure-Diagnose können Sie detaillierten Informationen Code ausgeführt werden, in Rollen, melden Sie sich, ob die Rollen in der Entwicklungsumgebung oder in Azure ausgeführt werden. Weitere Informationen finden Sie unter [Sammeln Protokollierung Daten mithilfe von Azure-Diagnose](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Wenn Sie zum Schreiben von Rollen an .NET Framework 4 oder .NET Framework 4.5 gerichtet sind Visual Studio Enterprise verwenden, können Sie IntelliTrace Zeitpunkt aktivieren, dass Sie einen Azure-Cloud-Dienst in Visual Studio bereitstellen. IntelliTrace stellt ein Protokoll aus, die Sie mit Visual Studio verwenden können, die Anwendung debuggen, als ob es in Azure ausgeführt wurden. Weitere Informationen finden Sie unter [einem veröffentlichten Cloud-Dienst mit IntelliTrace und Visual Studio debuggen]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Sie können die remote-Debuggen auf Ihrer Clouddienste zu dem Zeitpunkt an, wenn Sie den Cloud-Dienst von Visual Studio bereitstellen, aktivieren. Wenn Sie remote Debuggen für eine Bereitstellung zu aktivieren, werden remote Debuggen Services auf den virtuellen Computern installiert, die jede Instanz der Rolle ausgeführt werden. Diese Dienste, wie z. B. msvsmon.exe, keinen Einfluss auf die Leistung oder dazu führen, dass zusätzliche Kosten. Weitere Informationen finden Sie unter [Debuggen einen Clouddienst in Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



