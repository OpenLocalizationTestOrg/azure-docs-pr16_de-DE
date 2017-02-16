<properties
   pageTitle="Behandeln von Problemen mit langsam Sichern von Dateien und Ordnern in Azure Sicherung | Microsoft Azure"
   description="Bietet Hinweise zur Problembehandlung, um Ihnen die Ursache des Azure Sicherung Leistungsprobleme zu diagnostizieren"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Behandeln von Problemen mit langsam Sichern von Dateien und Ordnern in Azure Sicherung

Dieser Artikel bietet Hinweise zur Problembehandlung, um Ihnen bei der diagnose der Ursache des Sicherung langsam für Dateien und Ordner aus, wenn Sie zusätzliche Azure verwenden, zu erleichtern. Wenn Sie die Sicherung Azure-Agent zum Sichern von Dateien verwenden, kann der Sicherungsdatei Prozess mehr Zeit als erwartet dauern. Diese Verzögerung möglicherweise eine oder mehrere der folgenden Ursachen haben:

-   [Es gibt Leistungsengpässe auf dem Computer, der gerade gesichert wird.](#cause1)
-   [Einem anderen Prozess oder Antivirensoftware ist die Sicherung Azure-Prozess stören.](#cause2)
-   [Der Sicherung Agent läuft auf einer Azure-virtuellen Computern (virtueller Computer).](#cause3)  
-   [Sie haben eine große Anzahl (Millionen) von Dateien sichern.](#cause4)

Bevor Sie beginnen, Behandeln von Problemen, empfehlen wir, dass Sie herunterladen und der [neuesten Sicherung Azure-Agent installieren](http://aka.ms/azurebackup_agent). Wir stellen häufige Updates an den Sicherung Agent zum Beheben von Problemen mit verschiedenen Features hinzufügen und Verbessern der Leistung.

Wir empfehlen auch, um sicherzustellen, dass Sie keine der Konfigurationsprobleme auftreten sind die [Sicherung Azure Service häufig gestellte Fragen](backup-azure-backup-faq.md) zu überprüfen.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Ursache: Leistungsengpässe auf dem computer

Der verlangsamen können Engpässe auf dem Computer, der gerade gesichert wird. Beispielsweise kann des Computers Möglichkeit Lese-oder Schreibzugriff auf Festplatte oder verfügbare Bandbreite zum Senden von Daten über das Netzwerk, Engpässe verursachen.

Windows bietet ein integriertes Tool, das [Performance Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon), um diese Engpässe erkennen aufgerufen hat.

Hier sind einige Datenquellen und Bereiche, die bei der Diagnose von Engpässen für optimale Sicherungskopien hilfreich sein können.

| Indikator  | Status  |
|---|---|
|Logische Datenträger (physische Datenträger) – im Leerlauf %   | • 100 % auf 50 % im Leerlauf Leerlauf = fehlerfrei</br>• 49 % 20 % im Leerlauf Leerlauf = Warnung oder eines Monitors</br>• 19 % im Leerlauf auf 0 % im Leerlauf = kritisch oder abwesend Spezifikation|
|  Logische Datenträger (physische Datenträger) – % durchschnittliche Sec Lese- oder Schreibvorgang Datenträger |  • 0,001 ms zum 0.015 ms = fehlerfrei</br>• 0.015 ms zum 0.025 ms = Warnung oder eines Monitors</br>• 0.026 ms oder mehr = kritisch oder abwesend Spezifikation|
|  Logische Datenträger (physische Datenträger) – aktuelle Warteschlange (für alle Instanzen) | 80 Anforderung von mehr als 6 Minuten |
| Arbeitsspeicher – Ressourcenpool nicht seitenweise Bytes|• Weniger als 60 % der Pool verbraucht = fehlerfrei<br>• 61 80 % verbraucht Ressourcenpool = Warnung oder eines Monitors</br>• Größer als 80 % Ressourcenpool verbraucht = kritisch oder abwesend Spezifikation|
| Arbeitsspeicher – seitenweise Ressourcenpool Bytes |• Weniger als 60 % der Pool verbraucht = fehlerfrei</br>• 61 80 % verbraucht Ressourcenpool = Warnung oder eines Monitors</br>• Größer als 80 % Ressourcenpool verbraucht = kritisch oder abwesend Spezifikation|
| Arbeitsspeicher und verfügbare MB| • 50 % des freien Speichers verfügbar oder mehr = fehlerfrei</br>• 25 % des freien Speichers verfügbar = Monitor</br>• 10 % des freien Speichers verfügbar = Warnung</br>• Weniger als 100 MB oder 5 % des freien Speichers verfügbar = kritisch oder abwesend Spezifikation|
|Prozessor –\%CPU-Zeit (alle Instanzen)|• Weniger als 60 % verbraucht = fehlerfrei</br>• 61 bis 90 % verbraucht = Monitor oder vorsichtig</br>• 91 % und 100 % verbraucht = kritisch|


> [AZURE.NOTE] Wenn Sie feststellen, dass die Infrastruktur Ursache, empfehlen wir, dass Sie die Datenträger regelmäßig für eine bessere Leistung defragmentieren.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Ursache: Mit einem anderen Prozess oder Antivirensoftware in Konflikt mit Azure Sicherung

Wir haben mehrere Instanzen gesehen, wo andere Prozesse im Windows-System Leistung von der Sicherung von Azure-Agent-Prozess beeinträchtigt haben. Beispielsweise, wenn Sie sowohl die Sicherung Azure-Agent und einem anderen Programm, um Daten zu sichern, oder wenn Antivirensoftware ausgeführt wird und verfügt über eine Sperre auf Dateien gesichert werden müssen, sperrt das Vielfache auf führen Dateien möglicherweise Konflikte. In diesem Fall die Sicherung kann ein Fehler auftreten, oder der Auftrag mehr Zeit in Anspruch als erwartet.

Die beste in diesem Szenario wird empfohlen, um festzustellen, ob die Sicherung Zeit für die Sicherungsdatei Azure-Agent ändert sich das andere Sicherung Programm deaktivieren. Normalerweise ist sicherzustellen, dass mehrere zusätzliche Einzelvorgänge nicht gleichzeitig arbeiten ausreichend, um zu verhindern, dass Auswirkungen miteinander.

Für Antivirensoftware Programme empfehlen wir, dass Sie die folgenden Dateien und Speicherorte ausschließen:

- C:\Programme\Gemeinsame Dateien\Microsoft Azure Wiederherstellung Services Agent\bin\cbengine.exe als Prozess
- C:\Programme\Microsoft c:\Programme\Microsoft Azure Wiederherstellung Services Agent\ Ordner
- Streichen Sie Speicherort (Wenn Sie nicht am Standardspeicherort verwenden)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Ursache: Sicherung Agent für eine Azure-virtuellen Computern ausgeführt

Wenn Sie den Sicherung Agent auf einem virtuellen Computer ausführen, werden langsamer, wenn Sie es auf einem physischen Computer ausführen. Dies ist aufgrund von IOPS Einschränkungen erwartet.  Sie können jedoch die Leistung optimieren, indem Sie wechseln der Datenlaufwerke, die in Azure Premium Speicher gesichert werden. Wir arbeiten an dieses Problem zu beheben, und Fix werden in zukünftigen Versionen zur Verfügung.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Ursache: Sichern eine große Anzahl (Millionen) von Dateien

Verschieben eines große Datenmengen dauert länger als eine kleinere Menge der Daten zu verschieben. In einigen Fällen bezieht sich zusätzliche Zeit auf nicht nur die Größe der Daten, sondern auch die Anzahl der Dateien oder Ordner. Dies gilt besonders wenn Millionen von kleinen Dateien (ein paar Bytes in wenigen KB) gesichert werden.

Dieses Verhalten tritt auf, da gedrückt, während Sie die Daten sichern und es in Azure verschieben, Azure gleichzeitig Ihre Dateien Katalogisierung ist. In einigen Szenarien seltenen möglicherweise den Katalog als erwartet dauert länger.

Mithilfe die folgenden Indikatoren hilft Ihnen den Engpass verstehen und entsprechend arbeiten, klicken Sie auf den nächsten Schritten fort:

- **Benutzeroberfläche den Fortschritt für die Datenübertragung angezeigt wird**. Die Daten ist immer noch übertragen. Der Netzwerk-Bandbreite oder die Größe der Daten möglicherweise verzögerter führen.

- **Benutzeroberfläche wird den Fortschritt für die Datenübertragung nicht angezeigt**. Öffnen Sie die Protokolle C:\Microsoft Azure Wiederherstellung Services Agent\Temp am, und aktivieren Sie dann für den Eintrag FileProvider::EndData in den Protokollen. Dieser Eintrag gibt an, dass die Datenübertragung abgeschlossen und der Katalog Vorgang geschieht. Abbrechen Sie, die Sicherung Einzelvorgänge nicht. In diesem Fall etwas mehr für zum Abschluss des Vorgangs Katalog warten. Wenn das Problem weiterhin besteht, wenden Sie sich an [Azure unterstützen](https://portal.azure.com/#create/Microsoft.Support).
