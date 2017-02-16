<properties
   pageTitle="Protokoll speichern mithilfe von Linux Azure Diagnose | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Azure-Diagnose zum Sammeln von Protokollen aus einem Dienst Fabric Linux Cluster in Azure ausgeführt einrichten."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Protokoll speichern mithilfe von Azure-Diagnose

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Wenn Sie einen Azure Service Fabric Cluster ausführen, ist es eine gute Idee, die Protokolle aus allen Knoten an einer zentralen Stelle zu sammeln. Haben Sie die Protokolle an einem zentralen Ort erleichtert zu analysieren und Behandeln von Problemen, ob sie in Ihrer Dienste, die Anwendung oder den Cluster selbst sind. Eine Möglichkeit zum Hochladen und Protokoll speichern besteht darin, die Erweiterung Azure-Diagnose verwenden, die Protokolle zu Azure-Speicher hochgeladen wird. Können Sie die Ereignisse aus Speicher gelesen und in einem Produkt wie [Flexible suchen](service-fabric-diagnostic-how-to-use-elasticsearch.md) oder eine andere Log analysieren Lösung zu platzieren.

## <a name="log-sources-that-you-might-want-to-collect"></a>Log-Datenquellen, die Sie sammeln möchten möglicherweise
- **Dienst Fabric Protokolle**: von der Plattform über [LTTng](http://lttng.org) ausgegeben und Ihr Speicherkonto geladen. Protokolle können sein, Betrieb oder Laufzeitereignisse, die die Plattform gibt aus. Diese Protokolle werden in den Speicherort gespeichert, die das Manifest Cluster gibt an. (Um die Details des Speicher-Konto zu gelangen, suchen Sie nach der Kategorie **AzureTableWinFabETWQueryable** , und suchen Sie nach **StoreConnectionString**.)
- **Anwendungsereignisse**: von des Diensts Code ausgegeben. Alle Protokollierung Lösung können, die textbasierten Protokolldateien – beispielsweise LTTng schreibt. Weitere Informationen finden Sie in der Dokumentation LTTng auf Spur Ihrer Anwendung.  


## <a name="deploy-the-diagnostics-extension"></a>Bereitstellen der Diagnose-Erweiterung
Dieser erste Schritt in Erfassung von Protokollen besteht die Erweiterung Diagnose auf jeder der den virtuellen Computern in den Dienst Fabric Cluster bereitstellen. Die Erweiterung Diagnose sammelt Protokolle jedes virtuellen Computers und hochlädt, mit dem Speicherkonto aus, dem Sie angeben. Die Schritte variieren, je nachdem, ob Sie die Azure-Portal oder Ressourcenmanager Azure verwenden.

Legen Sie zum Bereitstellen von der Diagnose Erweiterung mit den virtuellen Computern im Cluster als Teil der Clustererstellung, **Diagnose** auf **auf**ein. Nach dem Erstellen des Clusters können Sie diese Einstellung mithilfe von im Portal ändern.

Klicken Sie dann konfigurieren Sie Linux Azure Diagnose (LAD), um die Dateien zu sammeln und in Ihr Speicherkonto zu platzieren. Dieses Verfahren wird erläutert, wie Szenario 3 ("Hochladen Ihrer eigenen Protokolldateien") im Artikel [LAD zu überwachen und diagnostizieren Linux virtuellen Computern zu verwenden](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Bei dieser Vorgehensweise wird Ihnen den Zugriff auf die Spuren. Sie können die Spuren in eine Schnellansicht Ihrer Wahl hochladen.

Sie können auch die Erweiterung Diagnose mithilfe von Azure Ressourcenmanager bereitstellen. Der Prozess ähnelt für Windows und Linux und für Windows Cluster in [zum Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md)beschrieben ist.

Vorgänge Management Suite, können Sie auch in [Vorgänge Management Suite Log Analytics mit Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/)beschriebenen.

Nachdem Sie diese Konfiguration fertig sind, überwacht der Agent LAD die angegebenen Protokolldateien an. Immer, wenn eine neue Zeile zu der Datei angefügt wird, erstellt er einen Syslog-Eintrag, der auf den Speicher gesendet wird, die Sie angegeben haben.


## <a name="next-steps"></a>Nächste Schritte
Weitere Details zum welche Ereignisse Sie beim Beheben von Problemen untersuchen sollten finden Sie unter [LTTng Dokumentation](http://lttng.org/docs) und [LAD verwenden](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
