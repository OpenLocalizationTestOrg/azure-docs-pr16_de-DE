<properties
   pageTitle="Standardgröße TEMP-Ordner ist zu klein für eine Rolle | Microsoft Azure"
   description="Eine Rolle der Cloud-Dienst verfügt über eine begrenzte Menge an Speicherplatz für den Ordner "TEMP". In diesem Artikel finden Sie einige Vorschläge zum werdenden Speicherplatz zu vermeiden."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standardgröße TEMP-Ordner ist in einen Cloud-Dienst Web/Worker-Rolle zu klein

Das standardmäßige temporäre Verzeichnis einer Cloud-Dienst Worker oder Web Rolle weist eine maximale Größe von 100 MB, die an einem bestimmten Punkt voll möglicherweise. Dieser Artikel beschreibt, wie Sie zu vermeiden, dass Speicherplatz für das temporäre Verzeichnis.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Warum ausführen kann ich Speicherplatz?

Die standardmäßigen Windows-Umgebung, die Variablen TEMP und TMP stehen Code, der in der Anwendung ausgeführt wird. Sowohl TEMP und TMP zeigen Sie auf ein einzelnes Verzeichnis, das eine maximale Größe von 100 MB hat. Alle Daten, die in diesem Verzeichnis gespeichert ist, werden nicht über den Lebenszyklus von Cloud-Dienst beibehalten; Wenn die Rolleninstanzen in einen Cloud-Dienst wiederverwendet werden, ist diese Verzeichnis entfernt.

## <a name="suggestion-to-fix-the-problem"></a>Vorschläge zur Behebung des Problems

Implementieren Sie eine der folgenden alternativen aus:

- Konfigurieren einer lokalen Speicherressource, und greifen Sie sie direkt anstatt TEMP oder TMP. Rufen Sie die [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) -Methode, um eine lokale Speicherressource Code zugreifen, die innerhalb der Anwendung ausgeführt wird. 

- Konfigurieren einer lokalen Speicherressource, und zeigen Sie die TEMP und TMP-Verzeichnisse auf den Pfad der lokalen Speicherressource verweisen. Diese Änderung sollte innerhalb der Methode [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) ausgeführt werden.

Im folgenden Code wird gezeigt, wie so ändern Sie die Zielverzeichnisse für TEMP und TMP aus innerhalb der OnStart-Methode:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Einen Blog lesen beschrieben, die [den Azure Rolle ASP.NET temporäre Webordner zu vergrößern](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Weitere [Artikel zur Fehlerbehebung](/?tag=top-support-issue&product=cloud-services) für Cloud Services anzeigen

Behebung von Cloud-Dienst Rolle mithilfe von Azure PaaS Computer Diagnosedaten finden zeigen Sie [Kevin Williamsons Blog Reihe an](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)
