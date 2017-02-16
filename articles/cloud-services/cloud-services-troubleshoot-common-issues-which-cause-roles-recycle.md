<properties
   pageTitle="Häufigsten Ursachen für Cloud-Dienst Rollen Wiederverwendung | Microsoft Azure"
   description="Eine Rolle der Cloud-Dienst, die plötzlich-Freigabe kann längere Downtime verursachen. Hier sind einige häufige Probleme, die Rollen wiederverwendet, werden Anlass geben Ihnen Ausfallzeiten reduzieren möglicherweise helfen können."
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
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Häufige Probleme, die Rollen freizugebenden verursachen

In diesem Artikel werden einige der häufigsten Ursachen für Bereitstellungsprobleme behandelt und stellt Tipps zur Problembehandlung helfen, diese Probleme zu lösen. Zeigt an, dass ein Problem, mit einer Anwendung vorliegt ist, wenn die Instanz der Rolle kann nicht gestartet werden, oder es zwischen den Staaten Initialisierung, beschäftigt und beenden wechselt.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Fehlende Laufzeit Abhängigkeiten

Wenn eine Rolle in Ihrer Anwendung eine erforderlich ist, der nicht Teil des .NET Framework oder der Azure verwaltete Bibliothek, müssen Sie diese Assembly explizit in das Anwendungspaket aufnehmen. Beachten Sie, dass andere Microsoft-Framework nicht Azure standardmäßig zur Verfügung stehen beibehalten. Wenn Ihre Rolle solche ein Framework erforderlich ist, müssen Sie diese Assemblys in das Anwendungspaket hinzufügen.

Bevor Sie erstellen und zum Packen der Anwendung, überprüfen Sie Folgendes:

- Bei Verwendung von Visual Studio, stellen Sie sicher, dass die Eigenschaft **Lokale Kopie** auf **True** für jede referenzierte Anordnung in Ihrem Projekt festgelegt ist, die nicht Bestandteil der Azure SDK oder .NET Framework ist.

- Stellen Sie sicher, dass die Verbindungszeichenfolgeneigenschaft keine nicht verwendete Assemblys im Element Kompilierung verweist.

- Die **Aktion erstellen** für jede cshtml-Datei wird auf **Inhalt**festgelegt. Dadurch wird sichergestellt, dass die Dateien in das Paket ordnungsgemäß angezeigt wird, und ermöglicht es anderen Dateien verwiesen wird, in das Paket angezeigt werden.

## <a name="assembly-targets-wrong-platform"></a>Falsche Assembly Ziele-Plattform

Azure ist eine 64-Bit-Umgebung. Daher funktioniert nicht für ein 32-Bit-Ziel kompilierte .NET-Assemblys auf Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rolle löst Ausnahmefehler beim Initialisierung oder beenden aus.

Alle Ausnahmen, die von den Methoden der [RoleEntryPoint] -Klasse, die wozu auch die [OnStart], [OnStop]und Methoden [Ausführen ausgelöst werden] , sind Ausnahmefehler. Wenn Ausnahmefehler in einem der folgenden Methoden auftritt, wird die Rolle Papierkorb. Wenn die Rolle mehrmals wiederverwendet, kann es Ausnahmefehler jedes Mal auslösen werden, die sie versucht haben, starten.

## <a name="role-returns-from-run-method"></a>Rolle gibt von Run-Methode

Die Methode [Ausführen] soll endlos ausgeführt. Wenn der Code die Methode [Ausführen] überschreibt, sollten sie endlos warten. Wenn die Methode [Ausführen] zurückgibt, Wiederverwendung die Rolle.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Falsche DiagnosticsConnectionString-Einstellung

Wenn Anwendung Azure-Diagnose verwendet, muss Ihre Konfiguration Dienstdatei angeben der `DiagnosticsConnectionString` Einstellung für die Konfiguration. Diese Einstellung sollte eine HTTPS-Verbindung zu Ihrem Speicherkonto in Azure fest.

Um sicherzustellen, dass Ihre `DiagnosticsConnectionString` Einstellung korrekt ist, bevor Sie Ihre Anwendungspaket Azure bereitstellen, überprüfen Sie Folgendes:  

- Die `DiagnosticsConnectionString` Punkte mit einem gültigen Speicherkonto in Azure festlegen.  
  Standardmäßig verweist diese Einstellung mit dem Speicherkonto emulierten, damit diese Einstellung muss vor der Bereitstellung Ihrer Anwendungspakets explizit ändern. Wenn Sie diese Einstellung nicht ändern, wird eine Ausnahme ausgelöst, wenn die Instanz der Rolle versucht, um den Diagnoseprotokollen Monitor zu starten. Dadurch kann die Instanz der Rolle endlos Papierkorb.

- Die Verbindungszeichenfolge wird in folgendem [Format](../storage/storage-configure-connection-string.md)angegeben. (Das Protokoll muss als HTTPS angegeben werden). Ersetzen Sie *MyAccountName* durch den Namen Ihrer Speicher-Konto und *MyAccountKey* zusammen mit der Zugriffstaste:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Wenn Sie eine Anwendung mithilfe von Azure-Tools für Microsoft Visual Studio entwickeln, können Sie die [Eigenschaftenseiten](https://msdn.microsoft.com/library/ee405486) verwenden, um diesen Wert festzulegen.

## <a name="exported-certificate-does-not-include-private-key"></a>Exportierte Zertifikat enthält keinen privaten Schlüssel

Um eine Webrolle unter SSL ausführen zu können, müssen Sie sicherstellen, dass Ihre exportierte Management Zertifikat den privaten Schlüssel enthält. Wenn Sie die *Windows-Zertifikat-Manager* verwenden, um das Zertifikat zu exportieren, müssen Sie unbedingt **Ja** auswählen die Option **privaten Schlüssel exportieren** . Das Zertifikat muss im PFX-Format exportiert werden nur Standarddateiformat aktuell nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere [Artikel zur Fehlerbehebung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Cloud Services anzeigen

Zeigen Sie weitere Rolle Wiederverwendung Szenarien bei [Kevin Williamsons Blog Reihe an](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Ausführen]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
