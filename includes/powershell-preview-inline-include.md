Azure PowerShell steht derzeit in zwei Versionen - 1.0 und 0.9.8 aus. Wenn Sie vorhandene Skripts haben und nicht sofort ändern möchten, können Sie mithilfe der 0.9.8 weiterhin Version. Bei Verwendung die Version 1.0, sollten Sie Ihre Skripts sorgfältig in noch nicht produziert Umgebungen testen, vor der Verwendung in der Herstellung unerwartete Nachteile zu vermeiden.

um 1,0 Cmdlets folgen dem naming Muster {Verb}-AzureRm {nominale;} während die, die 0.9.8 Namen enthalten nicht **Rm** (z. B. neu-AzureRmResourceGroup statt neu-AzureResourceGroup). Wenn Azure PowerShell 0.9.8 verwenden zu können, müssen Sie zuerst den Ressourcenmanager Modus durch Ausführen des Befehls **Switch-AzureMode AzureResourceManager** aktivieren. Dieser Befehl ist nicht in 1.0 erforderlich.

Neue Features werden nur der Version 1.0 hinzugefügt werden. Informationen zu der Version 1.0, einschließlich Verfahren zum Installieren und deinstallieren Sie die Version finden Sie unter [Azure PowerShell 1.0](https://azure.microsoft.com/blog/azps-1-0/).

