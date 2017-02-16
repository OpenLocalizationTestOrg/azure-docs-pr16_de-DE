<properties
    pageTitle="Stellen Sie erneut bereit Azure Stapel | Microsoft Azure"
    description="Erneut bereitstellen Sie Azure Stapel."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Stellen Sie erneut bereit Azure Stapel

Um Azure Stapel erneut bereitstellen möchten, müssen Sie über ganz neu starten wie unten beschrieben.

## <a name="steps-to-redeploy-azure-stack"></a>Schritte zum Azure Stapel erneut bereitstellen.

1. Starten Sie den Host in das ursprüngliche Betriebssystem (um direkt installiert). Dies ist nicht die Standardeinstellung für das Startmenü, sodass Sie KVM oder lokale Konsole verwenden müssen, um es auszuwählen, während der Neustart (während der Installation sollten Sie die "Boot aus virtuelle Festplatte" OS zu "AzureStack TP2" benannt, dies kann Ihnen helfen, identifizieren die OS welche ist).

    Müssen Sie nicht entfernen den vorhandenen Boot-Eintrag (das neue Support Skript, die "PrepareBootFromVHD.ps1" die für die für Sie zuständig ist.)

2. Wenn Sie keinen KVM oder das Betriebssystem Boot vor dem Neustart auswählen möchten:
    
    1. Suchen Sie das Skript.\BootMenuNoKVM.ps1 aus. Diese Datei ist mit anderen Support bereitgestellten Skripts zusammen mit diesem Build verfügbar.
    
    2. Führen Sie das Skript mit erhöhten. Wählen Sie den Namen der ursprünglichen Host OS. Dies wird den Host in der ursprünglichen Host OS starten, ohne KVM-Zugriff.
    
    3. Wenn das Skript abgeschlossen ist, werden Sie aufgefordert, den Neustart zu bestätigen.

    - Wenn andere Benutzer angemeldet sind, schlägt dieser Befehl fehl.

    - Führen Sie den folgenden Befehl einfach: Computer neu starten-erzwingen 
 
3. Löschen Sie die CloudBuilder.vhdx-Datei, die als Teil der vorherigen Bereitstellung verwendet wurde.

    Sie benötigen keine vorhandenen Speicherpool aus der vorherigen TP2 Bereitstellung löschen. Das Bereitstellungsskript erkennt die vorhandene bereinigt, und dann neu erstellt werden.

5. Kopieren Sie eine neue Kopie der CloudBuilder.vhdx, starten Sie ihn, usw. erneut.

## <a name="next-steps"></a>Nächste Schritte

[Verbinden mit Azure Stapel](azure-stack-connect-azure-stack.md)
