<properties
    pageTitle="Azure Stapel Übersicht über die Features für Entwickler | Microsoft Azure"
    description="Erfahren Sie, die Features von den Stapel-Dienst und seine APIs aus Sicht der Entwicklung."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Übersicht über die Stapel Features für Entwickler

In diesen Überblick über die Hauptkomponenten des Diensts Azure Stapel besprochen, die primäre Service-Features und Ressourcen, die Stapel Entwickler verwenden können, um umfangreiche Parallel erstellen Lösungen zu berechnen.

Gibt an, ob Sie entwickeln eine verteilte berechnete Anwendung oder dem Dienst Probleme direkte [REST-API] [ batch_rest_api] Anrufe oder Sie verwenden eine [Stapel SDKs](batch-technical-overview.md#batch-development-apis), erhalten Sie zahlreiche Ressourcen und in diesem Artikel beschriebenen Features verwenden.

> [AZURE.TIP] Einer höheren Ebene Einführung in den Stapel Dienst finden Sie unter [Grundlagen von Azure Stapel](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Stapel Dienst workflow

Der folgende allgemeine Workflow wird der Darstellung fast aller Anwendungen und Dienste, die den Stapel-Dienst für die Verarbeitung parallel Auslastung verwenden standardmäßig angezeigt:

1. Hochladen der **Datendateien** , die Sie in einer [Azure-Speicher] verarbeiten möchten[ azure_storage] Konto. Blatt schließt eine integrierte Unterstützung für den Zugriff auf Azure Blob-Speicher, und Aufgaben können diese Dateien zum [Berechnen von Knoten](#compute-node) herunterladen, wenn die Aufgaben ausgeführt werden.

2. Hochladen von **Dateien der Anwendung** , die Ihre Aufgaben ausgeführt werden. Diese Dateien Binärdateien oder Skripts und deren abhängigen Dateien werden können, und durch die Aufgaben in Ihre Aufträge ausgeführt werden. Ihre Vorgänge können diese Dateien herunterladen, über Ihr Konto Speicher, oder Sie können das Feature [Anwendungspakete](#application-packages) des Blatts für anwendungsverwaltung und Bereitstellung.

3. Erstellen Sie einen [Ressourcenpool](#pool) von Computeknoten. Wenn Sie einem Ressourcenpool erstellen, geben Sie die Anzahl der berechnen Knoten für den Ressourcenpool, deren Größe und das Betriebssystem an. Wenn der jeweiligen Aufgabe in Ihrem Auftrag ausgeführt wird, hat es auf einem der Knoten in der Ressourcenpool ausführen zugewiesen.

4. Erstellen eines [Auftrags](#job). Ein Projekt verwaltet eine Auflistung von Aufgaben an. Sie verbinden jeden Auftrag, der einen bestimmten Pool der Vorgänge des Projekts Ausführungsort.

5. Hinzufügen von [Vorgängen](#task) im Projekt. Jede Aufgabe wird ausgeführt, die Anwendung oder das Skript, die Sie zum Verarbeiten der Datendateien, die sie von Ihrem Konto Speicher downloads hochgeladen. Wie jeden Vorgang abgeschlossen ist, können sie deren Ausgabe in Azure-Speicher hochladen.

6. Überwachung des Projektstatus und Abrufen der Aufgabe Ausgabe aus Azure-Speicher.

Den folgenden Abschnitten werden diese und andere Ressourcen des Blatts, mit denen Ihre verteilte berechnete Szenario.

> [AZURE.NOTE] Sie benötigen ein [Stapel-Konto](batch-account-create-portal.md) , um den Stapel-Dienst verwenden. Verwenden einer [Azure-Speicher] auch fast alle Lösungen[ azure_storage] Konto für die Datei speichern und abrufen. Stapelverarbeitung aktuell unterstützt nur **Allgemeine** Speicher Kontotyp, wie in Schritt 5 des [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) in [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben.

## <a name="batch-service-resources"></a>Stapel Service-Ressourcen

Einige der folgenden Ressourcen – Konten zu berechnen, dass Knoten, Pools, Projekten und Aufgaben – alle Lösungen erforderlich sind, die den Stapel-Dienst verwenden. Andere, werden wie Projektpläne und Anwendungspaketen hilfreich, aber optional Features.

- [Konto](#account)
- [Berechnen von Knoten](#compute-node)
- [Ressourcenpool](#pool)
- [Position](#job)

  - [Projektpläne](#scheduled-jobs)

- [Aufgabe](#task)

  - [Start-Aufgabe](#start-task)
  - [Position Manager Aufgabe](#job-manager-task)
  - [Vorbereitung und Release Projektaufgaben](#job-preparation-and-release-tasks)
  - [Mit mehreren Instanzen Vorgang (MPI)](#multi-instance-tasks)
  - [Anordnungsbeziehungen](#task-dependencies)

- [Anwendungspaketen](#application-packages)

## <a name="account"></a>Konto

Eine eindeutig bestimmten Entität innerhalb des Diensts Stapel ist ein Stapel-Konto. Alle Verarbeitung ist ein Stapel-Konto zugeordnet. Wenn Sie mit dem Dienst Stapel Operationen durchführen, benötigen Sie den Namen des Kontos und eine ihrer Schlüssel Konto an. Sie können [ein Stapel Azure-Konto über das Azure-Portal erstellen](batch-account-create-portal.md).

## <a name="compute-node"></a>Berechnen von Knoten

Ein berechnen-Knoten ist ein Azure-virtuellen Computern (virtueller Computer), die für die Verarbeitung von eines Teils Ihrer Anwendung Arbeitsbelastung festgelegt ist. Die Größe eines Knotens bestimmt die Anzahl der CPU-Kerne, Speicherkapazität und die Größe des lokale Dateien, die dem Knoten zugewiesen wird. Sie können Windows oder Linux Knoten Speicherpools mithilfe von entweder Azure Cloud Services oder virtuellen Computern Marketplace Bilder erstellen. Finden Sie im folgenden Abschnitt [Pool](#pool) für Weitere Informationen zu diesen Optionen aus.

Knoten ausgeführt werden können eine ausführbare Datei oder das Skript, das von der Umgebung Betriebssystem des Knotens unterstützt wird. Dies umfasst \*.exe, \*.cmd, \*bat- und PowerShell-Skripts für Windows – und Binärdateien, Gerüst und Python Skripts für Linux.

Alle zu berechnen, dass Knoten im Stapel ebenfalls sind:

- Eine standard- [Ordnerstruktur](#files-and-directories) und zugehörige [Umgebungsvariablen](#environment-settings-for-tasks) , die für den Bezug von Aufgaben zur Verfügung stehen.
- **Firewall** -Einstellungen, die zum Steuern des Zugriffs konfiguriert sind.
- [Remotezugriff](#connecting-to-compute-nodes) auf Windows ((Remotedesktopprotokoll)) und Knoten Linux (Secure Shell (SSH)).

## <a name="pool"></a>Ressourcenpool

Ein Ressourcenpool ist eine Auflistung von Knoten, die die Anwendung ausgeführt wird, klicken Sie auf. Der Ressourcenpool kann manuell erstellt werden von Ihnen oder automatisch durch den Stapel Dienst bei der Angabe der zu erledigenden Arbeit. Sie können erstellen und Verwalten von einem Ressourcenpool, die die Ressourcen der Anwendung erfüllt. Ein Ressourcenpool kann nur durch den Stapel-Konto verwendet werden, in dem es erstellt wurde. Eine Stapel-Konto besitzen, kann mehr als einem Ressourcenpool.

Erstellen von Azure Stapel Pools auf Hauptplattform Azure berechnen. Sie bieten umfangreiche Zuteilung, Anwendungsinstallation, Verteilung der Daten, für die Überwachung Gesundheit und flexible Anpassung der Anzahl von Computeknoten in einem Ressourcenpool ([Skalierung](#scaling-compute-resources)).

Jeder Knoten, der einem Ressourcenpool hinzugefügt wird, wird einen eindeutigen Namen und die IP-Adresse zugewiesen. Wenn ein Knoten aus einem Ressourcenpool entfernt wird, die an das Betriebssystem oder Dateien vorgenommenen Änderungen verloren, und deren Namen und die IP-Adresse werden für eine zukünftige Verwendung freigegeben. Wenn ein Knoten einem Ressourcenpool verlässt, geht ihre Lebensdauer über.

Wenn Sie einem Ressourcenpool erstellen, können Sie die folgenden Attribute angeben:

- Berechnen von Knoten **Betriebssystem** und **version**

    Wenn Sie ein Betriebssystem für die Knoten in der Ressourcenpool auswählen, haben Sie zwei Optionen: **Konfiguration des virtuellen Computers** und **Cloud Services-Konfiguration**.

    **Konfiguration des virtuellen Computers** bietet sowohl Linux und Windows-Bildern berechnen Knoten aus dem [Azure-virtuellen Computern Marketplace][vm_marketplace].
    Wenn Sie einem Ressourcenpool, die Konfiguration des virtuellen Computers Knoten enthält erstellen, müssen Sie nicht nur die Größe von Knoten, aber auch **virtuellen Computern Bild Bezug** und der Stapel **Knoten Agent SKU** installiert werden, auf den Knoten angeben. Weitere Informationen zu diesen Pooleigenschaften festlegen finden Sie unter [Bereitstellen von Linux Knoten in Azure Stapel Pools zu berechnen](batch-linux-nodes.md).

    **Cloud Services-Konfiguration** bietet Windows Knoten *nur*zu berechnen. Verfügbaren Betriebssysteme für Cloud Services-Konfiguration Pools werden in [Azure Gast-BS-Versionen und SDK Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md)aufgeführt. Wenn Sie einem Ressourcenpool, die Cloud Services-Knoten enthält erstellen, müssen Sie nur die Knotengröße und der *Familie OS*angeben. Bei der Erstellung Speicherpools Windows Knoten zu berechnen, Sie am häufigsten Cloud Services verwenden.

    - Die *Familie OS* bestimmt auch an, welche Versionen von .NET mit dem Betriebssystem installiert werden.
    - Wie mit Worker-Rollen in der Cloud Services, Sie eine *Version des Betriebssystems* angeben können (Weitere Informationen zu Workerrollen, finden Sie im Abschnitt [Weitere Informationen über Cloud-Dienste](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) in der [Cloud Services Overview](../cloud-services/cloud-services-choose-me.md)).
    - Wie bei Worker-Rollen, wir empfehlen, dass Sie angeben `*` für die *Version des Betriebssystems* , damit die Knoten werden automatisch aktualisiert, und es keine erforderlich steht, um neu zu ausstatten Versionen veröffentlicht. Die primäre Anwendungsfall-zum Auswählen einer bestimmten Betriebssystemversion ist zur Sicherstellung der Anwendungskompatibilität, wodurch Abwärtskompatibilität testen, um die ausgeführt werden, bevor Sie die Version aktualisiert werden. Nach der Überprüfung der *Version des Betriebssystems* für den Ressourcenpool aktualisiert werden kann und das neue OS Bild kann installiert – alle laufenden Vorgänge unterbrochen und werden hat, weitergeleitet.

- **Die Größe der Knoten**

    **Cloud Services-Konfiguration** berechnen Knoten Größen werden in [Größen für Cloud Services](../cloud-services/cloud-services-sizes-specs.md)aufgeführt. Stapel unterstützt alle Cloud Services-Größen außer `ExtraSmall`.

    **Konfiguration des virtuellen Computers** berechnen Knoten, die Größen in [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) und [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) aufgelistet werden. Stapel unterstützt alle Azure-virtuellen Computer Größen außer `STANDARD_A0` sowie mit Premium-Speicher (`STANDARD_GS`, `STANDARD_DS`, und `STANDARD_DSV2` Reihe).

    Wenn Sie die Größe eines berechnen Knoten auswählen zu können, erwägen Sie die Merkmale und den Anforderungen der Programme, die Sie auf den Knoten ausgeführt werden. Aspekte wie gibt an, ob die Anwendung multithreaded ist und wie viel Speicher sie es verbraucht helfen bestimmt die Knotengröße optimal geeignet und kostengünstiger. Es ist üblich, wählen Sie einen Knoten Größe unter der Voraussetzung, dass eine Aufgabe jeweils auf einem Knoten ausgeführt wird. Es ist jedoch möglich, dass mehrere Vorgänge (und daher mehrere Anwendungsinstanzen) [parallel ausgeführt werden](batch-parallel-node-tasks.md) , klicken Sie auf Knoten während der Ausführung des Auftrags berechnen. In diesem Fall ist es üblich, wählen Sie einen größeren Knoten, die höhere Nachfrage der Ausführung der Aufgabe parallele gerecht werden kann. Weitere Informationen finden Sie unter [Planen Richtlinie Vorgangs](#task-scheduling-policy) .

    Alle Knoten in einem Ressourcenpool haben die gleiche Größe. Wenn beabsichtigen, Ausführen von Applications mit unterschiedlichen Systemanforderungen und/oder laden Ebenen, es empfiehlt sich, dass Sie separate Pools verwenden.

- **Ziel der Anzahl von Knoten**

    Dies ist die Anzahl der Knoten berechnen, die Sie in dem Pool bereitstellen möchten. Dies wird als *Ziel* bezeichnet, da der Ressourcenpool in einigen Fällen nicht die gewünschte Anzahl von Knoten erreichen möglicherweise. Ein Ressourcenpool erreichen die gewünschte Anzahl von Knoten möglicherweise nicht, wenn er [Core Kontingent](batch-quota-limit.md#batch-account-quotas) für Ihr Konto Stapel – erreicht oder es ist eine automatische Skalierung Formel, die Sie dem Pool angewendet haben, die die maximale Anzahl von Knoten beschränkt (siehe Abschnitt "Skalierung Richtlinie").

- **Anpassungsbereich für Richtlinie**

    Zusätzlich zur Angabe einer statischen Anzahl Knoten, können Sie stattdessen schreiben und Anwenden einer [Formel automatische Skalierung](#scaling-compute-resources) zu einem Ressourcenpool. Stapel Dienst regelmäßig wertet die Formel und die Anzahl der Knoten innerhalb der basierend auf verschiedenen Ressourcenpool, Position und Vorgang Parameter, die Sie angeben können Pool passt.

- **Planen der Richtlinie von Tasks**

    Die Option [max Vorgänge pro Knoten](batch-parallel-node-tasks.md) Konfiguration bestimmt die maximale Anzahl von Aufgaben, die auf den einzelnen Knoten berechnen im Pool parallel ausgeführt werden können.

    Die Konfiguration ist, dass jeweils eine Aufgabe, die auf einem Knoten ausgeführt wird, es gibt jedoch einige Szenarien, in denen sie positiv auf mehr als eine Aufgabe auf einem Knoten gleichzeitig ausgeführt haben. Finden Sie unter dem [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) im Artikel [gleichzeitige Knoten Aufgaben](batch-parallel-node-tasks.md) , um anzuzeigen, wie Sie von mehreren Vorgängen pro Knoten profitieren können.

    Sie können auch einen *Fülleffekt Typ* angeben, die bestimmt, ob Stapels Aufgaben gleichmäßig über alle Knoten in einem Ressourcenpool verteilt oder jeder Knoten die maximale Anzahl von Aufgaben Sprachpakete vor dem Zuweisen von Aufgaben an einen anderen Knoten.

- **Status der Kommunikation** von Computeknoten

    In den meisten Szenarien Aufgaben funktionieren unabhängig und müssen nicht miteinander kommunizieren. Es gibt jedoch einige Applikationen, in denen Aufgaben wie [MPI Szenarien](batch-mpi.md)kommunizieren müssen.

    Sie können einem Ressourcenpool zum Zulassen der Kommunikation zwischen den Knoten innerhalb It –**Symbol Kommunikation**konfigurieren. Wenn Netzwerk-Kommunikation aktiviert ist, Knoten in der Cloud Services-Konfiguration Pools miteinander kommunizieren können, auf größer als 1100 Ports und Konfiguration des virtuellen Computers Pools Datenverkehr an allen Ports nicht einschränken.

    Beachten Sie, dass auch wirkt sich auf die Position der einzelnen Knoten im Cluster und möglicherweise die maximale Anzahl von Knoten in einem Ressourcenpool, da Bereitstellung Einschränkungen beschränken Symbol Kommunikation ermöglichen. Wenn eine Anwendung keine Kommunikation zwischen Knoten erforderlich sind, wird der Stapel-Dienst kann eine große Anzahl von Knoten mit dem Pool aus viele verschiedene Cluster zuweisen, und Rechenzentren zu aktivieren größerer parallele Verarbeitung Power.

- **Starten des Vorgangs** für berechnen Knoten

    Der *Vorgang beginnen* optional führt auf den einzelnen Knoten wie dieser Knoten Pool Beitritt und jedes Mal ein Knoten neu gestartet oder einem neuen Image versehen. Die Start-Aufgabe ist besonders hilfreich, bei der Vorbereitung Datenverarbeitungsknoten für die Ausführung von Aufgaben, wie das Installieren der Anwendung, die Ihre Aufgaben auf den Knoten berechnen ausgeführt werden.

- **Anwendungspaketen**

    Sie können die [Anwendungspakete](#application-packages) für die Bereitstellung auf die Datenverarbeitungsknoten im Pool angeben. Anwendungspakete bieten vereinfachte Bereitstellung "und" versionsverwaltung-Anwendung, die Ihre Aufgaben ausgeführt werden. Anwendungspaketen, die Sie für einen Pool angeben auf den einzelnen Knoten, die diesem Pool verknüpft installiert sind, und jedes Mal ein Knoten neu gestartet oder einem neuen Image versehen ist. Anwendungspakete werden derzeit auf Linux Datenverarbeitungsknoten nicht unterstützt.

- **Netzwerkkonfiguration**

    Sie können angeben, auf die ID eines Azure [virtuelles Netzwerk (VNet)](../virtual-network/virtual-networks-overview.md) in dem Pool des Knoten berechnen erstellt werden soll. Anforderungen für eine VNet angeben, für Ihre Ressourcenpool Sie unter [Hinzufügen einer Ressourcenpool mit einer Firma finden] [ vnet] im Stapel REST-API Bezug.

> [AZURE.IMPORTANT] Alle Stapel Konten verfügen über ein **Kontingent** , die die Anzahl der **Kerne** (und daher berechnen Knoten) in einem Stapel Konto beschränkt. Sie finden die standardmäßigen Kontingente und Anweisungen zum [Vergrößern eine Vorgabe](batch-quota-limit.md#increase-a-quota) (beispielsweise die maximale Anzahl der Kerne in Ihr Konto Stapel) in [Kontingente und Grenzwerte für den Stapel Azure-Dienst](batch-quota-limit.md). Wenn Sie feststellen, sich fragt, "Warum wird nicht Mein Ressourcenpool erreichen von mehreren X-Knoten?" Dieses Kontingent Core möglicherweise die Ursache.

## <a name="job"></a>Position

Ein Projekt ist eine Auflistung von Aufgaben an. Es verwaltet werden, wie die Berechnung von ihrer Aufgaben auf den Knoten berechnen in einem Ressourcenpool ausgeführt wird.

- Der Auftrag gibt dem **Ressourcenpool** in dem ist die Arbeit ausgeführt werden soll. Sie können einen neuen Pool für jedes Projekt erstellen oder verwenden Sie einen Ressourcenpool für viele Aufträge. Sie können einem Ressourcenpool für jedes Projekt, das mit einem Projektplan zugeordnet ist, oder für alle Projekte, die einem Projektplan zugeordnet sind, erstellen.

- Sie können eine optionale **Priorität**angeben. Wenn ein Projekt mit höherer Priorität als Aufträge, die derzeit ausgeführt werden gesendet wird, sind die Aufgaben für das Projekt höherer Priorität in der Warteschlange vor Aufgaben für die Aufträge geringerer Priorität eingefügt. Aufgaben in geringerer Priorität Aufträge, die bereits ausgeführt werden, sind nicht gestört.

- Auftrag **Einschränkungen** können Sie bestimmte Grenzwerte für Ihre Aufträge angeben:

    Sie können eine **maximale Wallclock Zeit**, festlegen, dass, wenn ein Projekt länger als die maximale Wallclock Zeit, der angegeben wird läuft, den Auftrag und alle Vorgänge abgeschlossen werden.

    Stapel kann erkennen, und wiederholen Sie dann auf fehlgeschlagene Aufgaben. Sie können die **maximale Anzahl Wiederholungsversuche Aufgabe** als Einschränkung, einschließlich, ob ein Vorgang *immer* oder *nie* wiederholt ist angeben. Wiederholen einer Aufgabe bedeutet, dass die Aufgabe hat, weitergeleitet wird erneut ausgeführt werden.

- Die Clientanwendung kann Aufgaben mit einem Projekt hinzuzufügen, oder Sie können eines [Auftrags-Manager Vorgang](#job-manager-task)angeben. Eine Position Manager Aufgabe enthält die Informationen, die erforderlich ist, erstellen die erforderlichen Aufgaben für ein Projekt – mit dem Auftrag Manager Vorgang auf einem der Knoten berechnen im Pool ausgeführt wird. Wird die Position Manager Aufgabe behandelt speziell durch den Stapel – ist es in der Warteschlange, sobald der Auftrag wird erstellt, und neu gestartet wird, wenn sie fehlschlägt. Ein Projekt-Manager ist *erforderlich* für Projekte, die über einen [Zeitplan für Aufträge](#scheduled-jobs) erstellt werden, weil es ist die einzige Möglichkeit, die Vorgänge zu definieren, bevor Sie der Auftrag instanziiert wird.

- Standardmäßig bleiben Aufträge im aktiven Zustand ein, wenn alle Vorgänge innerhalb des Projekts abgeschlossen sind. Sie können dieses Verhalten ändern, sodass die Position automatisch beendet wird, wenn alle Aufgaben im Auftrag abgeschlossen sind. Festlegen des Projekts **OnAllTasksComplete** Eigenschaft ([OnAllTasksComplete] [ net_onalltaskscomplete] in .NET Stapel) zu *Terminatejob* , den Auftrag automatisch zu beenden, wenn alle Vorgänge der Status "abgeschlossen" enthält.

    Beachten Sie, dass der Dienst Stapel, ein Projekt mit *keine* Aufgaben berücksichtigt für alle Vorgänge abgeschlossen haben. Diese Option ist daher am häufigsten mit einer [Position Manager Vorgang](#job-manager-task)verwendet werden. Wenn Sie automatische Auftrag Beendigung ohne eines Auftrags-Manager verwenden möchten, sollten Sie Anfangs legen Sie einen neuen Auftrag **OnAllTasksComplete** Eigenschaft auf *Noaction*dann legen Sie dafür den *Terminatejob* nur, nachdem Sie alle Vorgänge dem Projekt hinzugefügt haben.

### <a name="job-priority"></a>Priorität

Sie können eine Priorität stellen zuweisen, die Sie in Stapel erstellen. Der Stapel-Dienst verwendet den Wert für Priorität des Projekts bestimmt die Reihenfolge der Planung von Aufträgen in einem Konto (Dies ist nicht zu mit einem [geplanten Auftrag](#scheduled-jobs)verwechselt werden). Die Priorität Wertebereich von-1000 und 1000, wobei-1000 wird die niedrigste Priorität und 1000 die höchste. Sie können die Priorität eines Projekts mithilfe der [Aktualisieren Sie die Eigenschaften eines Auftrags] aktualisieren[ rest_update_job] Vorgang (Stapel REST) oder durch Ändern der [CloudJob.Priority] [ net_cloudjob_priority] Eigenschaft (Stapel .NET).

Haben innerhalb der gleichen Konto höherer Priorität Aufträge Planung der Rangfolge über Aufträge geringerer Priorität. Ein Projekt mit einem Wert höherer Priorität in einem Konto verfügt nicht über Planung der Rangfolge über eine andere Position mit einem Wert geringerer Priorität in ein anderes Konto.

Planen von Pools Auftrag ist unabhängig. Zwischen verschiedenen Pools ist es nicht sichergestellt, indem er höherer Priorität zuerst geplant ist, deren zugeordneten Ressourcenpool kleiner als im Leerlauf Knoten ist. Im gleichen Pool haben Aufträge mit der gleichen Prioritätsstufe einer gleich Wahrscheinlichkeit berechnet wird.

### <a name="scheduled-jobs"></a>Geplante Aufträge

[Zeitpläne der beruflichen Position] [ rest_job_schedules] ermöglichen es Ihnen, sich wiederholende Aufträge innerhalb des Diensts Stapel zu erstellen. Ein Zeitplan für Aufträge gibt an, wann Aufträge ausgeführt und umfasst die Angaben für die Einzelvorgänge ausgeführt werden soll. Sie können angeben, dass die Dauer des Terminplans – wie lange und wenn der Terminplan aktiv – ist und wie oft während dieses Zeitraums, die Aufträge erstellt werden soll.

## <a name="task"></a>Aufgabe

Eine Aufgabe ist eine Einheit der Berechnung, die mit einem Projekt verknüpft ist. Es wird ein Knoten ausgeführt. Aufgaben, die zu einem Knoten zur Ausführung zugeordnet sind, oder Sie sind in der Warteschlange, bis ein Knoten frei wird. Einfach gesagt, führt eine Aufgabe eine oder mehrere Programme oder Skripts auf einem Knoten berechnen zum Ausführen der Arbeit, die Sie benötigen.

Wenn Sie einen Vorgang erstellen, können Sie Folgendes angeben:

- Die **Befehlszeile** des Vorgangs. Dies ist die Befehlszeile, die die Anwendung oder das Skript auf dem Berechnungsknoten ausgeführt wird.

    Es ist wichtig, beachten Sie, dass die Befehlszeile tatsächlich nicht unter einer Shell ausgeführt wird. Daher, es kann nicht systembedingt Shell Features nutzen wie [Umgebungsvariable](#environment-settings-for-tasks) erläuterten (Dies umfasst die `PATH`). Um diese Features nutzen zu können, rufen Sie die Verwaltungsshell in die Befehlszeile – beispielsweise durch Starten `cmd.exe` auf Windows-Knoten oder `/bin/sh` unter Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Wenn Ihre Aufgaben Ausführen einer Anwendung oder das Skript, der nicht in des Knotens des müssen `PATH` oder rufen Sie die Verwaltungsshell explizit in die Befehlszeile Vorgang, Umgebungsvariablen verweisen.

- **Ressourcen-Dateien** , die die zu verarbeitenden Daten enthalten. Diese Dateien werden vor der Ausführung des Vorgangs Befehlszeile von BLOB-Speicher in einem **allgemeinen Zweck** Azure-Speicher Konto automatisch an den Knoten kopiert. Weitere Informationen finden Sie in den Abschnitten [Vorgang beginnen](#start-task) und [Dateien und Verzeichnissen](#files-and-directories).

- Die **Umgebungsvariablen** , die von der Anwendung erforderlich sind. Weitere Informationen finden Sie im Abschnitt [-Umgebung, die Einstellungen für Vorgänge](#environment-settings-for-tasks) .

- Die **Einschränkungen** , unter dem die Aufgabe ausgeführt werden soll. Für Beispiel die maximale Zeit, die die Aufgabe ausgeführt werden darf, wie oft die maximale Anzahl, die eine fehlgeschlagene Aufgabe wiederholt werden soll, und die maximale dieser Dateien in den Vorgang geöffneten Verzeichnis Uhrzeit aufbewahrt werden.

- **Anwendung-Paketen** zum Berechnungsknoten bereitstellen, an dem die Aufgabe ausführen geplant ist. [Anwendung-Pakete](#application-packages) bieten vereinfachte Bereitstellung "und" versionsverwaltung-Anwendung, die Ihre Aufgaben ausgeführt werden. Auf Vorgangsebene Anwendungspakete sind besonders hilfreich in freigegebenen Pool Umgebungen, wobei unterschiedliche Stellen auf einem Ressourcenpool ausgeführt werden und der Pool wird nicht gelöscht werden, wenn ein Projekt abgeschlossen ist. Wenn der Auftrag als Knoten weniger Aufgaben im Pool aufweist, können Vorgang Anwendungspakete Datenübertragung minimiert werden, da die Anwendung nur auf Knoten bereitgestellt wird, die Vorgänge ausgeführt werden.

Neben zum Ausführen der Berechnung auf einem Knoten, Aufgaben, die Sie definieren werden die folgenden Inhalten Aufgaben auch durch den Stapel-Dienst bereitgestellt:

- [Start-Aufgabe](#start-task)
- [Position Manager Aufgabe](#job-manager-task)
- [Vorbereitung und Release Projektaufgaben](#job-preparation-and-release-tasks)
- [Mehrere Instanz Aufgaben (MPI)](#multi-instance-tasks)
- [Anordnungsbeziehungen](#task-dependencies)

### <a name="start-task"></a>Start-Aufgabe

Indem Sie mit einem Ressourcenpool zuordnen einer **Aufgabe zu starten** , können Sie die betriebssystemumgebung der Knoten vorbereiten. Beispielsweise können Sie Aktionen wie die Programme, die Ihre Aufgaben ausführen installieren und Starten von Hintergrundprozessen ausführen. Die Start-Aufgabe ausgeführt wird bei jedem Start von ein Knoten, solange sie in dem Pool--befindet einschließlich, wenn der Knoten zum ersten Mal hinzugefügt, dem Pool und wann sie neu gestartet oder einem neuen Image versehen werden.

Ein wesentlicher Vorteil von der Start-Aufgabe ist, dass sie alle Informationen, die erforderlich sind enthalten kann, um einen Knoten berechnen konfigurieren und installieren die Anwendung, die für die Ausführung der Aufgabe erforderlich sind. Daher erhöhen die Anzahl der Knoten in einem Ressourcenpool ist so einfach wie das neue Ziel Knoten zählen – bereits Stapel angeben weist die Informationen, die erforderlich ist, um die neuen Knoten konfigurieren und bereiten sie für das Annehmen von Aufgaben.

Wie bei allen Aufgaben Azure Stapel Sie eine Liste der **Ressourcendateien** in [Azure-Speicher]angeben können[azure_storage], über die **Befehlszeile** ausgeführt werden. Stapel zuerst die Ressourcendateien auf den Knoten aus Azure-Speicher kopiert und dann die Befehlszeile ausführt. Für einen Vorgang Ressourcenpool starten enthält die Dateiliste in der Regel die Anwendung Vorgang und die zugehörigen Dateien an.

Konnte beziehen sie auch Bezug Daten durch alle Aufgaben verwendet werden, die auf dem Berechnungsknoten ausgeführt werden. Zum beispielsweise Befehlszeile einer Start-Aufgabe durchführen einer `robocopy` Vorgang an Anwendungsdateien (die wurden festgelegte Ressourcendateien, und klicken Sie auf den Knoten heruntergeladen) aus der Start-Aufgabe [arbeiten Verzeichnis](#files-and-directories) in den [freigegebenen Ordner](#files-and-directories)zu kopieren, und führen Sie eine MSI-Datei oder `setup.exe`.

> [AZURE.IMPORTANT] Stapelverarbeitung aktuell unterstützt *nur* **Allgemeine** Speicher Kontotyp, wie in Schritt 5 des [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) in [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben. Stapelverarbeitungsaufgaben (einschließlich standard Aufgaben, Start Aufgaben, Vorbereitung Projektaufgaben und Projektaufgaben für die veröffentlichte Version) müssen Ressourcendateien angeben, die *nur* in **Allgemeine** Speicherkonten befinden.

Es empfiehlt sich in der Regel für den Dienst Stapel zu warten, bis der Start-Vorgang abgeschlossen ist, bevor er den Knoten bereit sind, die Aufgaben zugewiesen werden, jedoch können Sie dies konfigurieren.

Wenn eine Startaufgabe auf einem Knoten berechnen fehlschlägt, dann wird der Status des Knotens aktualisiert, und der Fehler an, und der Knoten wird keine Aufgaben zugewiesen. Eine Startaufgabe kann fehl, es ist ein Problem mit dem Kopieren der Ressourcendateien aus dem Speicher oder der Prozess ausgeführt, indem Sie ihre Befehlszeile einen Exitcode zurück.

Wenn Sie hinzufügen oder aktualisieren die erste Aufgabe für einen *vorhandenen* Pool, müssen Sie dessen berechnen Knoten für den Start-Vorgang auf Knoten angewendet werden neu starten.

### <a name="job-manager-task"></a>Auftrag Manager Aufgabe

Sie normalerweise verwenden eines **Auftrags-Manager Vorgang** zum Steuerelement und/oder Ausführung Auftrags – beispielsweise zum Erstellen und übermitteln die Aufgaben für ein Projekt überwachen, ermitteln zusätzliche Aufgaben ausführen, und wann die Arbeit abgeschlossen ist. Eine Aufgabe-Manager ist jedoch nicht auf diese Aktivitäten beschränkt. Es ist eine vollständigen Aufgabe, die Aktionen ausführen kann, die für das Projekt erforderlich sind. Beispielsweise möglicherweise ein Auftrag Manager Vorgang herunterladen eine Datei, die als Parameter angegeben ist, den Inhalt dieser Datei analysieren und zusätzliche Vorgänge basierend auf diesen Inhalt senden.

Eine Aufgabe-Manager wird gestartet, bevor Sie alle anderen Vorgänge. Es bietet die folgenden Features:

- Es wird automatisch übermitteltes als Aufgaben durch den Stapel Dienst der Auftrag erstellt wird.

- Es ist geplant, bevor Sie die anderen Aufgaben eines Projekts auszuführen.

- Der zugeordnete Knoten ist die letzte aus einem Ressourcenpool entfernt werden soll, wenn der Ressourcenpool ist downsized wird.

- Seine Beendigung kann mit der Terminierung von alle Aufgaben in den Auftrag verknüpft werden.

- Eine Position Manager Aufgabe wird die höchste Priorität angegeben werden, wenn er muss neu gestartet werden. Wenn ein Knoten im Leerlauf nicht verfügbar ist, möglicherweise der Stapel-Dienst eine der anderen laufenden Aufgaben im Pool zu schaffen für den Task-Manager Auftrag ausführen beenden.

- Eine Aufgabe für die Manager im Auftrag einer verfügt über den Aufgaben von anderen Einzelvorgänge nicht Priorität. Für Aufträge werden nur die Prioritäten Position auf Vorgangsebene beobachtet.

### <a name="job-preparation-and-release-tasks"></a>Vorbereitung und Release Projektaufgaben

Stapel bietet Auftrag vorbereitende Aufgaben für die Ausführung vor dem Auftrags einrichten. Release Projektaufgaben gelten für Wartung nach der Position oder zum Aufräumen.

- **Auftrag zur Vorbereitung Aufgabe**: ein Auftrag zur Vorbereitung Vorgang ausgeführt wird auf allen berechnen Knoten, die zum Ausführen von Aufgaben, bevor eine der anderen Projektaufgaben ausgeführt werden angesetzt werden. Einen Position Vorbereitung Vorgang Daten kopieren können, die alle Vorgänge freigegeben ist, aber ist nur für den Auftrag, beispielsweise.
- **Task freigeben Auftrag**: Wenn ein Projekt abgeschlossen hat, Task Freigeben eines Auftrags ausgeführt wird, auf den einzelnen Knoten im Pool, die mindestens einen Vorgang ausgeführt. Task Freigeben eines Auftrags können zum Löschen von Daten, die durch den Auftrag zur Vorbereitung Vorgang kopiert werden oder zum Komprimieren und diagnostic Log-Daten, beispielsweise hochladen.

Beide job Vorbereitung und Release-Aufgaben können Sie an einer Befehlszeile ausführen, wenn der Vorgang aufgerufen wird. Sie bieten Features wie Datei herunterladen, erhöhten Ausführung, benutzerdefinierten Umgebungsvariablen, Dauer der maximalen Ausführung, "Wiederholen" zählen und Datei Aufbewahrungszeit.

Weitere Informationen zur Vorbereitung und Release Projektaufgaben finden Sie unter [Ausführen Vorbereitung und Fertigstellung Projektaufgaben Azure Blattnamen Knoten zu berechnen](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Vorgang mit mehreren Instanzen

Eine [Aufgabe mit mehreren Instanzen](batch-mpi.md) ist eine Aufgabe, die gleichzeitig auf mehreren Computeknoten Ausführung konfiguriert ist. Bei mit mehreren Instanzen Aufgaben können Sie High Performance computing Szenarien aktivieren, die eine Gruppe von Computeknoten erfordern, die gemeinsam zum Verarbeiten von einer einzelnen Arbeitsbelastung (wie Nachricht übergeben Interface (MPI)) zugeordnet sind.

Ausführliche Informationen zum Ausführen von MPI Aufträgen in Stapel mithilfe der Objektbibliothek Stapel .NET checken Sie [Verwenden mit mehreren Instanzen von Aufgaben zur Ausführung Nachricht übergeben Interface (MPI) Anwendung in Azure Stapel](batch-mpi.md)aus.

### <a name="task-dependencies"></a>Anordnungsbeziehungen

[Anordnungsbeziehungen](batch-task-dependencies.md), wie der Name sagt, ermöglichen es Ihnen, die angeben, dass der Abschluss anderer Aufgaben, bevor Sie seine Ausführung ein Vorgangs abhängt. Dieses Feature bietet Unterstützung für Situationen, in denen ein Vorgang "untergeordneten" verbraucht die Ausgabe einer "übergeordneten" Aufgabe – oder eine übergeordneten Aufgabe einige Initialisierung ausführt, die durch eine untergeordnete Aufgabe erforderlich ist. Wenn Sie dieses Feature verwenden zu können, müssen Sie zuerst anordnungsbeziehungen auf Ihre Stapelverarbeitung aktivieren. Klicken Sie dann für jeden Vorgang, die von einem anderen abhängt (oder andere), geben Sie die Aufgaben, die von diesen Vorgang abhängig ist.

Anordnungsbeziehungen können Sie wie in den folgenden Szenarien konfigurieren:

* *TaskB* hängt *TaskA* (*TaskB* wird nicht erst beginnen, Ausführung *TaskA* durchgeführt wurde).
* *TaskC* hängt davon ab, sowohl *TaskA* und *TaskB*.
* *TaskD* hängt von einen Bereich von Aufgaben, wie etwa Aufgaben *1* bis *10*, bevor er ausgeführt wird.

Schauen Sie sich [anordnungsbeziehungen in Azure Stapel](batch-task-dependencies.md) und die [TaskDependencies] [ github_sample_taskdeps] Code Stichprobe [Azure-Stapel-Beispiele] [ github_samples] GitHub Repository für weitere ausführliche Details zu diesem Feature.

## <a name="environment-settings-for-tasks"></a>Umgebungseinstellungen für Vorgänge

Jeden Vorgang durch den Stapel Dienst ausgeführt hat Zugriff auf Umgebungsvariablen, die es für Datenverarbeitungsknoten fest. Dies umfasst Umgebungsvariablen vom Dienst Stapel definiert ([Service definiert][msdn_env_vars]) und Umgebungsvariablen benutzerdefinierte-, die Sie für Ihre Vorgänge definieren können. Die Applikationen und Skripts, die Ihre Aufgaben ausführen haben Zugriff auf diese Umgebungsvariablen-, die während der Ausführung aus.

Sie können benutzerdefinierte Umgebungsvariablen Ebene der Vorgang oder eine Position Auffüllen von der *umgebungseinstellungen* -Eigenschaft für diese Personen festlegen. Beispielsweise finden Sie unter der [Aufgabe Hinzufügen eines mit einem Projekt] [ rest_add_task] Vorgang (Stapel REST-API) oder die [CloudTask.EnvironmentSettings] [ net_cloudtask_env] und [CloudJob.CommonEnvironmentSettings] [ net_job_env] Eigenschaften in .NET Stapel.

Der Clientanwendung oder einen bestimmten Dienst erhalten eines Vorgangs Umgebungsvariablen Dienst definiert und benutzerdefinierte, [erhalten Sie Informationen zu einem Vorgang] mit[ rest_get_task_info] Vorgang (Stapel REST) oder indem Sie den Zugriff auf die [CloudTask.EnvironmentSettings] [ net_cloudtask_env] Eigenschaft (Stapel .NET). Prozesse auf einem Knoten berechnen ausführen können Zugriff auf diese und andere Umgebungsvariablen-, die auf dem Knoten, z. B. mithilfe der bekannten `%VARIABLE_NAME%` (Windows) oder `$VARIABLE_NAME` Syntax (Linux).

Finden Sie eine vollständige Liste aller Dienst definiert Umgebung Variablen im [Knoten Umgebungsvariablen berechnen][msdn_env_vars].

## <a name="files-and-directories"></a>Dateien und Verzeichnisse durchsuchen

Jede Aufgabe hat ein *Verzeichnis arbeiten* unter dem er 0 (null) oder mehrere Dateien und Verzeichnisse erstellt. Dieses Arbeitsverzeichnis kann zum Speichern des Programms, das ausgeführt wird, indem Sie die Aufgabe, die Daten, die verarbeitet und die Ausgabe der Verarbeitung ausgeführte verwendet werden. Alle Dateien und Verzeichnisse eines Vorgangs den Aufgabenbereich Benutzer gehören.

Der Dienst Stapel stellt einen Teil des Dateisystems auf einem Knoten als *Stammverzeichnis*zur Verfügung. Aufgaben können des Stammverzeichnisses zugreifen, indem Sie die Bezüge auf die `AZ_BATCH_NODE_ROOT_DIR` Umgebungsvariable. Weitere Informationen zur Verwendung von Umgebungsvariablen finden Sie unter [umgebungseinstellungen für Vorgänge](#environment-settings-for-tasks).

Das Stammverzeichnis enthält die folgenden Directory-Struktur:

![Berechnen von Knoten Directory-Struktur][1]

- **freigegebene**: Dieses Verzeichnis bietet Lese-und Schreibzugriff auf *Alle* Aufgaben, die auf einem Knoten ausgeführt werden. Alle Aufgaben, die auf den Knoten ausgeführt werden kann erstellen, lesen, aktualisieren und Löschen von Dateien in diesem Verzeichnis. Aufgaben können dieses Verzeichnis zugreifen, indem Sie die Bezüge auf die `AZ_BATCH_NODE_SHARED_DIR` Umgebungsvariable.

- **Start**: Dieses Verzeichnis wird durch einen Vorgang Start als seine Arbeitsverzeichnis verwendet. Alle Dateien, die auf den Knoten, indem Sie die Start-Aufgabe heruntergeladen werden werden hier gespeichert. Die Start-Aufgabe kann erstellen, lesen, aktualisieren und Löschen von Dateien unterhalb dieses Verzeichnisses. Aufgaben können dieses Verzeichnis zugreifen, indem Sie die Bezüge auf die `AZ_BATCH_NODE_STARTUP_DIR` Umgebungsvariable.

- **Aufgaben**: ein Verzeichnis wird erstellt, für jeden Vorgang, die auf dem Knoten ausgeführt wird. Er erfolgt über eine verweisen auf die `AZ_BATCH_TASK_DIR` Umgebungsvariable.

    Datenverzeichnis jede Aufgabe der Stapel Dienst erstellt ein Arbeitsverzeichnis (`wd`), deren eindeutiger Pfad angegeben ist, indem Sie die `AZ_BATCH_TASK_WORKING_DIR` Umgebungsvariable. Dieses Verzeichnis bietet Lese-und Schreibzugriff, die dem Vorgang an. Der Vorgang kann erstellen, lesen, aktualisieren und Löschen von Dateien unterhalb dieses Verzeichnisses. Dieses Verzeichnis wird beibehalten, basierend auf *RetentionTime* Nebenbedingung, die für den Vorgang angegeben ist.

    `stdout.txt`und `stderr.txt`: Diese Dateien werden in den Ordner geschrieben, während der Ausführung des Vorgangs.

>[AZURE.IMPORTANT] Wenn ein Knoten aus dem Pool entfernt wird, werden *Alle* Dateien, die auf den Knoten gespeicherte entfernt.

## <a name="application-packages"></a>Anwendungspaketen

Das [Anwendungspakete](batch-application-packages.md) -Feature stellt einfache Verwaltung und Bereitstellung von Applications zu berechnen Knoten in der Pools. Sie können hochladen und Verwalten mehrerer Versionen der Programme ausführen Ihrer Aufgaben, einschließlich ihrer Binärdateien und Support-Dateien. Dann können Sie eine oder mehrere der folgenden Anwendungen zu den Knoten berechnen im Pool automatisch bereitstellen.

Sie können die Anwendungspakete Ebene Ressourcenpool und der Aufgabe angeben. Wenn Sie die Anwendungspakete Ressourcenpool angeben, wird die Anwendung für jeden Knoten im Pool bereitgestellt. Wenn Sie die Aufgabe Anwendungspakete angeben, wird die Anwendung bereitgestellt, nur für Knoten, die zum Ausführen angesetzt werden Sie mindestens eines der des Projekts Aufgaben, einfach, bevor Sie den Vorgang Befehlszeile ausgeführt wird.

Stapel übernimmt die Details über das Arbeiten mit Azure-Speicher zum Speichern Ihrer Anwendungspaketen und bereitstellen, um den Knoten, zu berechnen, damit der Code und die Verwaltung Verwaltungsaufwand vereinfacht werden kann.

Checken Sie mehr über die Anwendung Paket-Funktion finden Sie [Bereitstellung der Anwendung mit Azure Stapel Anwendungspakete](batch-application-packages.md)aus.

>[AZURE.NOTE] Wenn Sie die Anwendungspakete Ressourcenpool zu einem *vorhandenen* Pool hinzufügen, müssen Sie dessen berechnen Knoten für die Anwendungspakete zu den Knoten bereitgestellt werden neu starten.

## <a name="pool-and-compute-node-lifetime"></a>Ressourcenpool und berechnen Knoten Lebensdauer

Beim Entwerfen Ihrer Lösung Azure Stapel, müssen Sie eine Entscheidung Entwurf Informationen und vornehmen, wenn Pools erstellt werden, und wie lange zu berechnen, dass die Knoten innerhalb dieser Pools zur Verfügung gehalten werden.

Klicken Sie auf eine Seite des Spektrums können Sie einem Ressourcenpool für jedes Projekt, die Sie senden erstellen und den Pool löschen, sobald die zugehörigen Aufgaben Ausführung Fertig stellen. Dies maximiert Auslastung, da die Knoten nur zugewiesen werden, wenn benötigt, und fahren Sie, sobald sie im Leerlauf befinden. Während dies bedeutet, dass die Knoten zuzuweisende der Auftrag warten muss, ist es wichtig, beachten Sie, dass für die Ausführung Vorgänge geplant sind, sobald Knoten einzeln stehen, belegt, und die Start-Aufgabe abgeschlossen. Stapel wird *nicht* warten, bis alle Knoten in einem Pool verfügbar sind, bevor Sie die Knoten Aufgaben zuweisen. Dadurch wird die maximale Auslastung aller verfügbaren Knoten sichergestellt.

Am anderen Ende des Spektrums ist Probleme sofort starten Einzelvorgänge die höchste Priorität können einem Ressourcenpool im Voraus erstellen und seine Knoten verfügbar machen, damit Aufträge gesendet werden. In diesem Szenario Aufgaben können sofort beginnen, aber Knoten und darauf warten, bis zugewiesen werden möglicherweise CPU.

Ein kombinierter Ansatz wird in der Regel verwendet, für den Umgang mit einer Variablen, aber laufenden, laden. Sie können einen, die mehrere Aufträge an übermittelt, aber skalieren können Pool haben die Anzahl der Knoten nach oben oder unten entsprechend den Auftrag laden (siehe die folgenden Abschnitte enthalten [Skalierung berechnen Ressourcen](#scaling-compute-resources) ). Hierzu können Sie reaktiv, basierend auf aktuelle laden oder die vorausschauende, wenn laden regressionsgleichung werden kann.

## <a name="scaling-compute-resources"></a>Anpassungsbereich für berechnen Ressourcen

Bei einer [automatischen Skalierung](batch-automatic-scaling.md)können Sie den Stapel Dienst dynamisch anpassen, die Anzahl der berechnen-Knoten in einem Ressourcenpool entsprechend der aktuellen Arbeitsbelastung und Ressourcen Nutzung von Ihrem Szenario berechnen lassen. So können Sie senken die Gesamtkosten für die Anwendung durch Drücken der benötigten Ressourcen und freigeben, die nicht benötigte ausführen.

Sie Aktivieren der automatischen Skalierung durch eine [Automatische Skalierung Formel](batch-automatic-scaling.md#automatic-scaling-formulas) schreiben, und verbinden die Formel mit einem Ressourcenpool. Der Dienst Stapel verwendet die Formel bestimmt die Ziel-Anzahl der Knoten im Pool für den nächsten Anpassungsbereich für Intervall (ein Intervall, die Sie konfigurieren können). Sie können die Einstellungen für automatischen Skalierung für einen Pool beim Erstellen Sie ihn später auf einem Ressourcenpool Skalierung aktivieren oder angeben. Sie können auch die Skalierung Einstellungen auf einem Ressourcenpool Skalierung aktiviert aktualisieren.

Beispielsweise muss vielleicht ein Auftrags übermitteln Sie eine große Anzahl von Aufgaben, die ausgeführt werden soll. Sie können eine Formel Anpassungsbereich für den Ressourcenpool zuweisen, die die Anzahl der Knoten im Pool basierend auf die aktuelle Anzahl der in der Warteschlange Aufgaben und der Kostensatz Abschluss der Aufgaben in den Auftrag passt. Der Dienst Stapel regelmäßig wertet die Formel und wird die Größe der Ressourcenpool, basierend auf Arbeitsbelastung an (Knoten für viele verzögerte Vorgänge hinzufügen und Entfernen von Knoten für keine Aufgaben in der Warteschlangen oder laufenden) und Ihre Formel andere Einstellungen.

Anpassungsbereich für Formel kann auf die folgende Metrik basieren:

- **Zeit Kennzahlen** basieren auf Statistik erfassten fünf Minuten in die angegebene Anzahl von Stunden.

- **Ressource Kennzahlen** basieren auf CPU-Verwendung, Bandbreite Verwendung, arbeitsspeicherauslastung und Anzahl der Knoten.

- **Aufgabe Kennzahlen** basieren auf Aufgabenstatus, beispielsweise *aktiver* (in der Warteschlange), *ausgeführt*oder *abgeschlossen*.

Wenn die Anzahl der in einem Ressourcenpool Datenverarbeitungsknoten automatische Skalierung verringert werden, müssen Sie berücksichtigen Umgang mit Aufgaben, die zum Zeitpunkt des Vorgangs verringern ausgeführt werden. Um dies zu unterstützen, bietet Stapel eine *Knoten zur Freigabe Option* , die in Formeln enthalten sein können. Beispielsweise können Sie angeben, dass ausgeführte Aufgaben werden sofort beendet, sofort beendet und dann hat, für die Ausführung auf einem anderen Knoten weitergeleitet oder darf abgeschlossen sein, bevor der Knoten aus dem Pool entfernt wird.

Weitere Informationen zum Skalieren automatisch einer Anwendungs finden Sie unter [automatisch skalieren Knoten in einem Stapel Azure-Pool zu berechnen](batch-automatic-scaling.md).

> [AZURE.TIP] Legen Sie zum Berechnen Ressource Auslastung zu maximieren, die Ziel-Anzahl der Knoten 0 (null) am Ende eines Auftrags, aber zulassen der Ausführung von Aufgaben auf Fertig stellen.

## <a name="security-with-certificates"></a>Sicherheit mit Zertifikaten

Sie müssen in der Regel Zertifikate verwenden Sie beim Verschlüsseln oder entschlüsseln vertrauliche Informationen für Aufgaben, wie die Taste für ein [Konto Azure-Speicher][azure_storage]. Um dies zu unterstützen, können Sie die Zertifikate auf Knoten installieren. Verschlüsselte Kennwörter Aufgaben über Befehlszeilenparameter übergeben oder in einem Vorgang Ressourcen eingebettet werden, und die installierten Zertifikate können verwendet werden, um diese entschlüsseln.

Verwenden Sie das [Zertifikat hinzufügen] [ rest_add_cert] Vorgang (Stapel REST) oder [CertificateOperations.CreateCertificate] [ net_create_cert] Methode (Stapel .NET) ein Zertifikat mit einem Stapel-Konto hinzufügen. Sie können dann das Zertifikat in einem neuen oder vorhandenen Ressourcenpool zuordnen. Wenn Sie ein Zertifikat mit einem Ressourcenpool verbunden ist, installiert den Stapel-Dienst das Zertifikat auf den einzelnen Knoten im Pool. Der Stapel installiert die entsprechenden Zertifikate beim Start von des Knotens nach oben, vor dem Start alle Vorgänge (einschließlich der Start-Aufgabe Projekt und Manager).

Wenn Sie zu einem *vorhandenen* Pool Zertifikate hinzufügen, müssen Sie dessen berechnen Knoten für die Zertifikate auf Knoten angewendet werden neu starten.

## <a name="error-handling"></a>Fehlerbehandlung

Möglicherweise finden Sie es zur Behandlung von sowohl Vorgangs- und Anwendung Fehlern innerhalb Ihrer Lösung Stapel erforderlich.

### <a name="task-failure-handling"></a>Aufgabe die Fehlerbehandlung
Aufgabe Fehlern gibt folgenden Kategorien:

- **Planen von Fehlern**

    Wenn die Übertragung von Dateien angegeben sind, die für einen Vorgang aus irgendeinem Grund fehlschlägt, wird ein Fehler"Planung" für den Vorgang festgelegt.

    Fehler bei der Planung kann auftreten, wenn des Vorgangs Ressourcendateien wurden verschoben, das Konto Speicher ist nicht mehr verfügbar oder ein anderes Problem aufgetreten, die die erfolgreiche Kopieren von Dateien auf den Knoten verhindert.

- **Anwendungsfehler**

    Der Prozess, der über die Befehlszeile des Vorgangs angegeben ist, kann auch fehl. Der Vorgang gilt als fehlgeschlagen ist, wenn Sie ein Exitcode vom Prozess zurückgegeben wird, die durch den Vorgang ausgeführt wird (siehe *Task beenden Codes* im nächsten Abschnitt).

    Für Anwendungsfehler können Sie den Stapel, um den Vorgang auf eine bestimmte Anzahl von Zeiten automatisch erneut zu konfigurieren.

- **Einschränkung Fehlern**

    Sie können eine Einschränkung festgelegt, die die maximale Ausführung Dauer für ein Projekt oder eine Aufgabe, die *MaxWallClockTime*angibt. Dies kann zum Beenden von "hängen geblieben" Aufgaben hilfreich sein.

    Wenn die maximale Zeitdauer überschritten wurde, die Aufgabe als *abgeschlossen*gekennzeichnet ist, aber der Beendigungscode, dass festgelegt wird `0xC000013A` und markiert das Feld *SchedulingError* `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Für das Debuggen Anwendungsfehler

- `stderr`und`stdout`

    Während der Ausführung möglicherweise eine Anwendung diagnostic Ausgabe erzeugen, die Sie verwenden können, um Probleme zu beheben. Wie aus dem vorherigen Abschnitt [Dateien und Verzeichnissen](#files-and-directories)erwähnt, schreibt der Stapel Dienst standard Ausgabe und Standardfehlerausgabe an `stdout.txt` und `stderr.txt` Dateien im Verzeichnis Vorgang auf dem Berechnungsknoten. Azure-Portal oder eine Stapel SDKs können diese Dateien herunterladen. Sie können beispielsweise abrufen, diese und andere Dateien für die Problembehandlung mithilfe von [ComputeNode.GetNodeFile] [ net_getfile_node] und [CloudTask.GetNodeFile] [ net_getfile_task] in der Bibliothek Stapel .NET.

- **Codes zum Beenden der Aufgabe**

    Wie zuvor schon erwähnt, wird eine Aufgabe markiert, als fehlerhaft durch den Stapel-Dienst, der Prozess, der durch den Vorgang ausgeführt wird, einen Exitcode zurück. Wenn eine Aufgabe ein Prozesses ausgeführt wird, füllt Stapel Code-Eigenschaft für den Vorgang beenden mit *Code des Prozesses zurück*. Es ist wichtig, beachten Sie, dass Code zum Beenden des Vorgangs ist **nicht** durch den Stapel-Dienst bestimmt festgestellt vom Prozess selbst oder das Betriebssystem auf dem der Prozess ausgeführt wird.

### <a name="accounting-for-task-failures-or-interruptions"></a>Buchhaltung für Aufgabe Fehlern oder interruptions

Vorgänge möglicherweise gelegentlich fehlschlägt oder unterbrochen werden. Die Vorgang Anwendung selbst kann ein Fehler auftreten, der Knoten, in dem die Aufgabe ausgeführt wird, möglicherweise neu gestartet werden, oder der Knoten möglicherweise entfernt werden aus dem Pool während eines Vorgangs Größenänderungs-Wenn Knoten sofort zu entfernen, ohne warten auf Aufgaben, die zum Abschließen des Pool zur Freigabe Richtlinie festgelegt ist. In allen Fällen kann die Aufgabe automatisch durch den Stapel für die Ausführung auf einem anderen Knoten hat, weitergeleitet werden.

Es ist es möglich, dass ein wiederkehrender Problem verursachen einen Vorgang bis zum hängen oder ausführen sehr lange dauert. Sie können die Ausführung Höchstdauer für einen Vorgang festlegen. Wenn es überschritten wird, wird Stapel die Anwendung Vorgang unterbrochen.

### <a name="connecting-to-compute-nodes"></a>Herstellen einer Verbindung zum Berechnen von Knoten

Sie können weitere für das Debuggen aus, und melden Sie sich mit einem Knoten berechnen Remote Problembehandlung ausführen. Azure-Portal können zum Herunterladen einer Datei (Remotedesktopprotokoll) für Windows-Knoten und Verbindungsinformationen für Linux Knoten Secure Shell (SSH) zu erhalten. Sie können auch dazu mithilfe der Stapel-APIs – beispielsweise mit [Stapel .NET] [ net_rdpfile] oder [Stapel Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Informationen zum Verbinden mit einem Knoten über RDP oder SSH müssen Sie zuerst einen Benutzer auf den Knoten erstellen. Hierzu können die Azure Portals, [Hinzufügen eines Benutzerkontos zu einem Knoten] [ rest_create_user] mithilfe der Stapel REST-API, rufen Sie die [ComputeNode.CreateComputeNodeUser] [ net_create_user] Methode in .NET Stapel, oder rufen Sie die [Add_user] [ py_add_user] Methode im Modul Stapel Python.

### <a name="troubleshooting-bad-compute-nodes"></a>Zur Problembehandlung "Bad" berechnen Knoten

In Situationen, in dem einige Ihrer Aufgaben weiß nicht, kann der Clientanwendung Stapel oder einen bestimmten Dienst, die Metadaten Fehlgeschlagene Aufgaben zur Identifizierung eines fehlerhafte Knotens untersuchen. Jeder Knoten in einem Ressourcenpool erhält eine eindeutige ID, und der Knoten, auf dem ein Vorgang ausgeführt wird, in den Vorgang Metadaten enthalten ist. Nachdem Sie einen Problem Knoten festgestellt haben, können Sie verschiedene Aktionen ausführen:

- **Starten Sie den Knoten** ([REST][rest_reboot] | [.NET][net_reboot])

    Einen Neustart von den Knoten kann manchmal von latente Probleme wie hängen oder abgestürzten Prozesse deaktivieren. Beachten Sie, dass, wenn Ihre Ressourcenpool eine Start-Aufgabe oder Ihre Position eine Position Vorbereitung Aufgabe verwendet, diese ausgeführt werden beim Neustart des Knotens.

- **Stellen Sie den Knoten** ([REST][rest_reimage] | [.NET][net_reimage])

    Dies wird das Betriebssystem auf dem Knoten neu installiert. Wie bei einem Neustart einen Knoten aus, starten Sie Vorgänge aus, und führen Vorbereitung Projektaufgaben werden erneut aus, nach einem neuen Image der Knoten versehen wurden weist.

- **Entfernen Sie den Knoten aus dem pool** ([REST][rest_remove] | [.NET][net_remove])

    Manchmal ist es erforderlich, vor den Knoten vollständig aus dem Pool zu entfernen.

- **Deaktivieren Sie auf dem Knoten Planen von Tasks** ([REST][rest_offline] | [.NET][net_offline])

    Dies effektiv erhält den Wert der Knoten "offline" aus, damit keine weiteren Aufgaben zugewiesen sind, sondern ermöglicht es den Knoten weiterhin ausgeführt wird und in dem Pool. Dies können Sie eine weitere Untersuchung der Ursachen der Fehler durchführen ohne Verlust von Daten für die fehlgeschlagene Aufgabe – und ohne den Knoten bewirken, dass zusätzlicher Vorgangsinformationen Fehlern. Beispielsweise können Sie Planung auf den Knoten, und [Melden Sie sich Remote](#connecting-to-compute-nodes) Untersuchen des Knotens Ereignisprotokollen oder andere Probleme durch einen Vorgang deaktivieren. Nachdem Sie Ihre Untersuchung abgeschlossen haben, können Sie ihn wieder online dann übertragen, durch das Aktivieren der Berechnung von Vorgängen ([REST][rest_online] | [.NET][net_online]), oder führen Sie eine der oben beschriebenen Aktionen.

> [AZURE.IMPORTANT] Mit jeder Aktion, die in diesem Abschnitt – beschrieben wird neu zu starten, neu abbilden, entfernen und deaktivieren Vorgang Planung – können Sie angeben, wie Vorgänge, die gerade ausgeführt wird, klicken Sie auf den Knoten verarbeitet werden beim Ausführen der Aktion. Beispielsweise, wenn Sie die Berechnung von Vorgängen auf einem Knoten mithilfe der Stapel .NET Client-Bibliothek deaktivieren, können Sie angeben einer [DisableComputeNodeSchedulingOption] [ net_offline_option] Enumerationswert angeben, ob zum **Abschluss** **Requeue** Aufgaben ausführen Sie für die Planung auf anderen Knoten, oder zulassen der Ausführung von Aufgaben ausführen, bevor die Aktion (**TaskCompletion**).

## <a name="next-steps"></a>Nächste Schritte

- Gehen Sie durch eine Stichprobe Stapel Anwendung schrittweise in [Erste Schritte mit der Bibliothek Azure Stapel für .NET](batch-dotnet-get-started.md). Es gibt auch eine [Python Version](batch-python-tutorial.md) des Lernprogramms, die eine Arbeitsbelastung für Linux berechnen Knoten ausgeführt wird.

- Herunterladen und erstellen Sie den [Stapel Explorer] [ github_batchexplorer] Beispielprojekt zur Verwendung während die Stapel Lösungen zu entwickeln. Mithilfe des Stapels-Explorers können Sie die folgenden und vieles mehr ausführen:
  - Überwachen und Pools, Projekte und Vorgänge in Ihrem Konto Stapel bearbeiten
  - Herunterladen von `stdout.txt`, `stderr.txt`, und andere Dateien von Knoten
  - Erstellen von Benutzern auf Knoten, und Laden Sie RDP-Dateien für den remote-Anmeldung

- Erfahren Sie, wie [Linux berechnen Hierarchieknoten zu erstellen](batch-linux-nodes.md).

- Besuchen Sie das [Forum Azure Stapel] [ batch_forum] auf MSDN. Im Forum ist ein guter, Fragen zu stellen, ob Sie nur learning sind oder Rat bei der Verwendung von Stapel sind.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
