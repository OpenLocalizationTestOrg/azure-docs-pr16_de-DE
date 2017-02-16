<properties 
   pageTitle="Learning PowerShell-Workflow"
   description="In diesem Artikel wird als eine schnelle Lektion Autoren PowerShell vertraut gelten zu verstehen, die Unterschiede zwischen der PowerShell und PowerShell-Workflow."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Learning Windows PowerShell-Workflow

Runbooks in Azure Automatisierung werden als Windows PowerShell-Workflows implementiert.  Ein Windows PowerShell-Workflow ähnelt ein Windows PowerShell-Skript jedoch einige wesentliche Unterschiede, die an einen neuen Benutzer mitunter schwer hat.  Dieser Artikel richtet sich an Benutzer, die bereits mit PowerShell vertraut sind und kurz erläutert Konzepte, mit denen Sie benötigen, wenn Sie ein PowerShell-Skript zu einem PowerShell-Workflow für die Verwendung in einem Runbooks konvertieren.  

Ein Workflow ist eine Abfolge von programmierten, verbundenen Schritte, die Aufgaben langer oder Koordinierung von mehreren Schritten in mehreren Geräten oder verwaltete Knoten erforderlich. Die Vorteile eines Workflows über ein normales Skript sind die Möglichkeit, eine Aktion anhand mehrerer Geräte gleichzeitig durchzuführen und die Möglichkeit, die automatisch von Fehlern wiederherstellen. Ein Windows PowerShell-Workflow ist ein Windows PowerShell-Skript, das Windows Workflow Foundation nutzt. Während der Workflow mit Windows PowerShell-Syntax geschrieben und von Windows PowerShell gestartet wird, wird es von Windows Workflow Foundation verarbeitet werden.

Ausführliche Informationen zu den Themen in diesem Artikel finden Sie unter [Erste Schritte mit Windows PowerShell-Workflow](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Typen von Runbooks

Es gibt drei Arten von Runbooks in Azure Automatisierung, *PowerShell-Workflow*, *PowerShell* und *grafisch*an.  Sie definieren den Typ des Runbooks beim Erstellen des Runbooks, und einer Runbooks können nicht auf den anderen Typ konvertiert werden, nachdem sie erstellt wurde.

PowerShell Workflow Runbooks und PowerShell Runbooks verwenden für Benutzer, die entweder direkt mit PowerShell-Code arbeiten lieber im Text-Editor in Azure Automatisierung oder einer offline-Editor, wie z. B. PowerShell ISE. Sie sollten die Informationen in diesem Artikel verstehen, wenn Sie einen PowerShell Workflow Runbooks erstellen. 

Grafische Runbooks ermöglichen es Ihnen, eine Runbooks dieselben Aktivitäten und Cmdlets, aber eine Benutzeroberfläche, die Blendet die Komplexität der zugrunde liegenden PowerShell-Workflows erstellen.  Konzepte in diesem Artikel wie Kontrollpunkten und parallele Ausführung weiterhin anwenden, um grafisch Runbooks, aber nicht müssen Sie über die ausführliche Syntax Gedanken machen. 

## <a name="basic-structure-of-a-workflow"></a>Grundlegende Struktur eines Workflows

Der erste Schritt zum Konvertieren eines PowerShell-Skripts zu einem PowerShell-Workflow ist es mit dem Schlüsselwort **Workflow** trennen.  Ein Workflow startet mit dem **Workflow** -Schlüsselwort, gefolgt von der Textkörper des Skripts in geschweifte Klammern eingeschlossen. Der Namen des Workflows folgt das Schlüsselwort **Workflow** aus, wie in der folgenden Syntax dargestellt. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Der Namen des Workflows muss den Namen des Runbooks Automatisierung entsprechen. Wenn des Runbooks importiert wird, klicken Sie dann den Dateinamen muss den Namen des Workflows entsprechen und muss enden ps1.

Wenn Parameter mit dem Workflow hinzufügen möchten, verwenden Sie das Schlüsselwort **Parameter** genauso wie in einem Skript. 

## <a name="code-changes"></a>Ändern von Code

PowerShell-Workflow-Code sieht fast mit PowerShell Skriptcode eine Ausnahme bilden jedoch einige wesentlichen Änderungen aus.  Den folgenden Abschnitten werden die Änderungen, die Sie an einer PowerShell-Skript dafür in einem Workflow ausführen vornehmen müssen.

### <a name="activities"></a>Aktivitäten

Eine Aktivität ist eine bestimmte Aufgabe in einem Workflow an. Genauso wie ein Skript aus einem oder mehreren Befehlen besteht, ein Workflow einer oder mehreren Aktivitäten besteht, die in einer Sequenz ausgeführt werden. Windows PowerShell-Workflow konvertiert automatisch viele der Windows PowerShell-Cmdlets in Aktivitäten bei der Ausführung eines Workflows. Wenn Sie eine der folgenden Cmdlets in Ihrem Runbooks angeben, wird die entsprechende Aktivität tatsächlich von Windows Workflow Foundation ausgeführt. Für diese Cmdlets ohne eine entsprechende Aktivität Windows PowerShell-Workflow wird automatisch ausgeführt, das Cmdlet innerhalb einer [InlineScript auf](#inlinescript) Aktion. Es gibt eine Reihe von Cmdlets, die ausgeschlossen werden und kann nicht in einem Workflow verwendet werden, es sei denn, Sie explizit in einem Block InlineScript auf Einfügen. Weitere Details zu diesen Konzepten finden Sie unter [Verwenden von Aktivitäten in Skript-Workflows](http://technet.microsoft.com/library/jj574194.aspx).

Workflowaktivitäten verfügen über einen Satz gemeinsamer Parameter so konfigurieren Sie den Vorgang. Ausführliche Informationen zu den Workflow allgemeine Parameter finden Sie unter [About_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Mit Feldern fester Breite Parameter

Sie können keine Aktivitäten und Cmdlets in einem Workflow mit Feldern fester Breite Parameter verwenden.  Dies bedeutet lediglich, dass Sie Parameternamen verwenden müssen.

Betrachten Sie beispielsweise den folgenden Code ein, der alle laufenden Dienste abruft.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Wenn Sie versuchen, die gleichen Codes in einem Workflow ausführen, erhalten Sie eine Meldung wie "Parameter festlegen aufgelöst werden kann unter Verwendung des angegebenen benannte Parameter."  Um dies zu beheben, bieten Sie einfach den Namen des Parameters wie nachfolgend dargestellt.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deserialisierte Objekte

Objekte in Workflows werden deserialisiert.  Dies bedeutet, dass ihre Eigenschaften immer noch verfügbar sind, aber nicht deren Methoden.  Betrachten Sie beispielsweise den folgenden PowerShell-Code, der ein Dienst mithilfe der Methode die Option zum Beenden des Objekts Dienst beendet wird.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Wenn Sie versuchen, diese in einem Workflow auszuführen, erhalten Sie einen Fehler angezeigt, die besagt "Aufrufen von Methoden nicht in einem Windows PowerShell-Workflow unterstützt wird".  

Eine Möglichkeit ist, um diese beiden Codezeilen in einem Block [InlineScript auf](#InlineScript) in diesem Fall $Service herumfließen eines Objekts Dienst innerhalb des Zeitraums wäre. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Eine weitere Möglichkeit besteht darin, verwenden ein anderes Cmdlet, das die gleiche Funktionalität wie die Methode, führt, sofern verfügbar.  In unserem Beispiel das Cmdlet beenden-Dienst bietet die gleiche Funktionalität wie die Methode beenden, und können Sie Folgendes für einen Workflow verwenden.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript auf

Die Aktivität **InlineScript auf** ist nützlich, wenn Sie einen oder mehrere Befehle als traditionelle PowerShell-Skript statt PowerShell Workflow ausführen müssen.  Während die Befehle in einem Workflow für die Verarbeitung an Windows Workflow Foundation gesendet werden, werden in einem Block InlineScript auf Befehle von Windows PowerShell verarbeitet. 

InlineScript auf verwendet die nachfolgend aufgeführte Syntax.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Sie können die Ausgabe aus einer InlineScript auf zurückkehren, indem Sie die Ausgabe einer Variablen zuzuweisen. Im folgende Beispiel beendet einen Dienst und gibt dann den Namen aus.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Sie können Werte in einem Block InlineScript auf übergeben, aber Sie Modifizierer des Gültigkeitsbereichs **$Using** verwenden müssen.  Im folgende Beispiel wird mit dem vorherigen Beispiel identisch, außer dass der Name des Dienstes durch eine Variable bereitgestellt wird. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Zwar InlineScript auf Aktivitäten in bestimmten Workflows kritische möglicherweise, diese Konstrukte Workflow nicht unterstützt und sollte nur bei Bedarf aus den folgenden Gründen verwendet werden:

- Sie können keine [Kontrollpunkten](#Checkpoints) innerhalb eines Blocks InlineScript auf verwenden. Wenn ein Fehler innerhalb des Zeitraums auftritt, muss es ab dem Anfang des Blocks fortgesetzt werden.
- Sie können keine [parallele Ausführung](#parallel-execution) innerhalb einer InlineScriptBlock verwenden.
- InlineScript auf wirkt sich auf Skalierbarkeit des Workflows, da es für die gesamte Länge des Blocks InlineScript auf die Windows PowerShell-Sitzung enthält.

Weitere Informationen zur Verwendung von InlineScript auf finden Sie unter [Ausführen von Windows PowerShell-Befehlen in einem Workflow](http://technet.microsoft.com/library/jj574197.aspx) und [About_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Parallele Verarbeitung

Ein Vorteil der Windows PowerShell-Workflows ist die Möglichkeit, eine Reihe von Befehlen parallel statt wie mit einem normalen Skript nacheinander durchführen. 

Das Schlüsselwort **parallele** können Sie um einen Skriptblock mit mehreren Befehlen zu erstellen, die gleichzeitig ausgeführt wird. Dadurch wird die nachfolgend aufgeführte Syntax verwendet. In diesem Fall werden Activity1 aufgerufen und Activity2 zur gleichen Zeit gestartet. Activity3 beginnt erst Activity1 aufgerufen und die Activity2 ausgefüllt haben.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Betrachten Sie beispielsweise die folgenden PowerShell-Befehle, die mehrere Dateien in einem Netzwerkziel kopieren aus.  Diese Befehle sind sequenziell führen Sie damit, dass eine Datei kopieren, bevor die nächste gestartet wird abgeschlossen sein muss.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Der folgende Workflow führt die Befehle parallel, sodass beginnen alle Vorgänge zur gleichen Zeit kopieren.  Nur, nachdem sie alle wurden ist vollständig kopiert die Fertigstellung Nachricht angezeigt.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Können die **ForEach-parallele** auf Prozess Befehle für jedes Element in einer Sammlung gleichzeitig erstellen. Die Elemente in der Auflistung werden parallel ausgeführt, während die Befehle in den Skriptblock nacheinander ausgeführt. Dadurch wird die nachfolgend aufgeführte Syntax verwendet. In diesem Fall beginnt Activity1 aufgerufen zur gleichen Zeit für alle Elemente in der Auflistung. Activity2 wird für jedes Element starten Sie nach Abschluss der Activity1 aufgerufen. Activity3 beginnt erst, wenn Sie sowohl die Activity1 aufgerufen Activity2 für alle Elemente abgeschlossen haben.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Im folgende Beispiel ist ähnlich wie im vorherigen Beispiel Dateien parallel kopieren.  In diesem Fall wird eine Meldung für jede Datei angezeigt, nach dem Kopieren.  Nur, nachdem sie alle wurden ist vollständig kopiert die endgültige Abschluss Nachricht angezeigt.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Wir empfehlen nicht untergeordneten Runbooks parallel ausgeführt werden, da dies zu unzuverlässigen Ergebnissen worden ist.  Die Ausgabe der untergeordneten Runbooks manchmal nicht angezeigt wird, und Einstellungen für ein untergeordnetes Element Runbooks können beeinflussen, die andere parallele untergeordneten runbooks 


## <a name="checkpoints"></a>Kontrollpunkten

Ein *Prüfpunkt* ist eine Momentaufnahme des aktuellen Status des Workflows, die den aktuellen Wert für Variablen und alle bis zu diesem Zeitpunkt generierte Ausgabe enthält. Wenn ein Workflow Fehler endet oder unterbrochen ist, wird dann das nächste Mal ausgeführt wird aus den letzten Prüfpunkt anstelle von Anfang der Workflow gestartet.  Sie können einen Prüfpunkt in einem Workflow mit **Wissensstand-Workflow** -Aktivität festlegen.

Im folgenden Beispielcode, eine Ausnahme auftritt, nach dem Activity2 bewirken, dass des Workflows beenden. Wenn der Workflow erneut ausgeführt wird, wird es durch Activity2 ausführen, da dies unmittelbar nach dem letzte Prüfpunkt festgelegt wurde gestartet.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Sie sollten in einem Workflow Kontrollpunkten festlegen, nachdem Aktivitäten, die möglicherweise zu Ausnahme und sollten nicht wiederholt werden soll, wenn der Workflow fortgesetzt wird. Beispielsweise kann der Workflow ein virtuellen Computers erstellen. Sie können eine Wissensstand vor und nach der Befehle zum Erstellen des virtuellen Computers festlegen. Wenn die Erstellung fehlschlägt, würde die Befehle wiederholt werden, wenn der Workflow erneut gestartet wird. Wenn das der Workflow schlägt fehl, wenn die Erstellung erfolgreich hergestellt wurde, und des virtuellen Computers nicht erneut erstellt wird, wenn der Workflow fortgesetzt wird.

Im folgenden Beispiel werden mehrere Dateien an einen Netzwerkspeicherort kopiert und legt eine Wissensstand nach jeder Datei.  Wenn der Speicherort im Netzwerk verloren geht, wird der Workflow Fehler beenden.  Wenn es erneut gestartet wird, wird es Lebenslauf, bei dem letzten Prüfpunkt, was bedeutet, dass nur die Dateien, die bereits kopiert wurden übersprungen werden.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Da Username-Anmeldeinformationen nicht beibehalten werden, nachdem Sie die Aktivität [Standby-Workflow](https://technet.microsoft.com/library/jj733586.aspx) aufgerufen oder nach dem letzten Prüfpunkt, müssen Sie die Anmeldeinformationen auf null festlegen und dann abrufen können erneut aus dem Objekt Store nach **Standby-Workflow** oder Wissensstand bezeichnet wird festlegen.  Andernfalls möglicherweise die folgende Fehlermeldung angezeigt: *den Workflowauftrag kann nicht fortgesetzt werden, entweder weil Dauerhaftigkeitsdaten konnte nicht vollständig gespeichert, oder gespeichert werden Dauerhaftigkeitsdaten ist beschädigt. Den Workflow muss neu gestartet.*

Folgende derselbe Code veranschaulicht, wie dies in der PowerShell-Workflow Runbooks behandeln.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Dies ist nicht erforderlich, wenn die Authentifizierung mit einem Konto ausführen als mit einem Hauptbenutzer Dienst konfiguriert.  

Weitere Informationen zu Kontrollpunkten finden Sie unter [Kontrollpunkten an einen Workflow Skript hinzufügen](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Nächste Schritte

- Um mit PowerShell Workflow Runbooks anzufangen, finden Sie unter [Meine erste PowerShell Workflow Runbooks](automation-first-runbook-textual.md) 
