<properties
   pageTitle="Beachten Sie Gast-BS Familie 1 Rente | Microsoft Azure"
   description="Stellt Informationen zu den Azure Gast OS Familie 1 Rente bei Vorkommnissen und um festzustellen, ob Sie betroffen sind"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Beachten Sie Rente Gast OS Familie 1

Ende der Verfügbarkeit von OS Familie 1 wurde zuerst am 1. Juni 2013 vorgestellt.

**2 September 2014** Die Azure Gast-Betriebssystem (BS Gast) Familie 1.x, die auf das Betriebssystem Windows Server 2008 basiert, wurde formal eingestellt. Alle Versuche, neue Dienste bereitstellen oder aktualisieren Sie vorhandene Services mit der Familie 1 werden eine Fehlermeldung, die Sie darüber informiert, dass der Gast OS Familie 1 Deaktivieren des betreffenden fehl.

**3 November 2014** Erweiterter Support für Gast OS Familie 1 beendet und es wird vollständig gelöscht werden. Alle Dienste weiterhin auf Familie 1 werden beeinträchtigt. Wir möglicherweise Dienste jederzeit beenden. Es gibt keine Garantie, die Ihrer Dienste ausgeführt weiterhin, es sei denn, Sie manuell diesen selbst aktualisieren.

Wenn Sie weitere Fragen haben, besuchen Sie die [Cloud Services-Foren](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) oder [Azure-Support wenden](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Sind Sie betroffen?

Der Cloud Services sind betroffen, wenn eine der folgenden Situationen zutrifft:

1. Sie haben einen Wert von "OsFamily ="1"explizit für Ihre Cloud-Dienst in der Datei ServiceConfiguration.cscfg angegeben.
2. Sie verfügen nicht über einen Wert für OsFamily explizit für Ihre Cloud-Dienst in der Datei ServiceConfiguration.cscfg angegeben. Derzeit das System den Standardwert von "1" in diesem Fall verwendet.
3. Das Azure klassische Portal Ihre Familie schätzen, Gast-Betriebssystem "WindowsServer 2008" aufgeführt.

Zum Suchen, die Ihrer Cloud-Dienste die Familie OS ausgeführt werden, können Sie das folgende Skript in Azure PowerShell ausführen, obwohl [Azure PowerShell einrichten](../powershell-install-configure.md) , Sie zuerst müssen. Weitere Informationen über das Skript, finden Sie unter [Azure Gast OS Familie 1 Ende der Nutzungsdauer: Juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Ihrer Clouddienste werden durch OS Familie 1 Rente beeinträchtigt, wenn die Spalte OsFamily in der Ausgabe des Skripts leer ist oder eine "1" enthält.

## <a name="recommendations-if-you-are-affected"></a>Empfehlungen, wenn Sie betroffen sind

Es empfiehlt sich, dass Sie Ihre Cloud-Dienst Rollen auf einen der unterstützten Gast OS Familien migrieren:

**Gast-BS familiäre 4.x** -Windows Server 2012 R2 *(empfohlen)*

1. Stellen Sie sicher, dass der Anwendung SDK 2.1 oder höher mit .NET Framework 4.0, 4.5 oder 4.5.1 verwendet wird.
2. Legen Sie das Attribut OsFamily auf "4" in der Datei ServiceConfiguration.cscfg, und erneut bereitstellen Sie Ihre Cloud-Dienst.


**Gast-BS familiäre 3.x** -Windows Server 2012

1. Stellen Sie sicher, dass der Anwendung SDK 1,8 oder höher mit .NET Framework 4.0 oder 4.5 verwendet wird.
2. Legen Sie das Attribut OsFamily auf "3" in der Datei ServiceConfiguration.cscfg, und erneut bereitstellen Sie Ihre Cloud-Dienst.


**Gast-BS familiäre 2.x** -Windows Server 2008 R2

1. Stellen Sie sicher, dass der Anwendung SDK 1.3 verwendet wird und höher mit .NET Framework 3.5 oder 4.0.
2. Legen Sie das Attribut OsFamily in "2" in der Datei ServiceConfiguration.cscfg, und erneut bereitstellen Sie Ihre Cloud-Dienst.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Erweiterte Unterstützung für Gast OS Familie 1 eingestellt 3 Nov 2014
Clouddienste auf Gast-BS Familie 1 werden nicht mehr unterstützt. Deaktivieren der Familie 1 so früh wie möglich zu vermeiden dienststörung migrieren.  

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die neuesten [Gast-BS frei](cloud-services-guestos-update-matrix.md).
