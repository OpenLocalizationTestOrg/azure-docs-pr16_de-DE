<properties
    pageTitle="Erstellen Sie einen virtuellen Windows-Computer mit für die Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage | Microsoft Azure"
    description="Verwenden einer Ressource Azure-Manager-Vorlage zum Erstellen eines neuen virtuellen Computers für Windows mit der Erweiterung Azure-Diagnose."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Erstellen Sie einen virtuellen Windows-Computer mit für die Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage

Diese Azure-Diagnose-Erweiterung bietet die Überwachung und Diagnose-Funktionen, die auf einem Windows-basierten Azure-virtuellen Computern. Sie können diese Funktionen auf dem virtuellen Computer aktivieren, indem Sie einschließlich Erweiterung als Teil der Vorlage Azure Ressource-Manager. Weitere Informationen zum Hinzufügen von einer beliebigen anderen Erweiterung als Teil einer Vorlage virtuellen Computern finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen mit virtueller Computer Erweiterungen](virtual-machines-windows-extensions-authoring-templates.md) . Dieser Artikel beschreibt, wie Sie Vorlage virtuellen Computern Windows Azure-Diagnose-Erweiterung hinzufügen können.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Hinzufügen der Azure-Diagnose Erweiterung für die Definition der Ressource virtueller Computer 

Um die Erweiterung Diagnose auf einem Windows-Computer zu aktivieren, müssen Sie die Erweiterung als Ressource virtueller Computer in der Ressource-Manager Vorlage hinzufügen.

Für eine einfache Ressourcenmanager basierend virtuellen Computern hinzufügen die Erweiterung Konfiguration zu der Matrix *Ressourcen* für den virtuellen Computer: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Eine andere allgemeine Messe ist die Erweiterung Konfiguration fügen Sie bei der Stammwebsite Ressourcenknoten der Vorlage statt ihn unter des virtuellen Computers Ressourcenknoten definieren. Bei diesem Ansatz müssen Sie eine hierarchische Beziehung zwischen der Erweiterung und des virtuellen Computers explizit mit den Werten *Name* und *Typ* angeben. Beispiel: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Die Erweiterung immer des virtuellen Computers zugeordnet ist, können Sie entweder direkt direkt unter des virtuellen Computers Ressourcenknoten definieren oder auf der Basis Ebene definieren und verwenden Sie die hierarchische Benennungskonvention des virtuellen Computers zuordnen.

Die Erweiterungskonfiguration für virtuellen Computern skalieren Mengen in der Eigenschaft *ExtensionProfile* der *VirtualMachineProfile*angegeben.
   
Die Eigenschaft *Publisher* mit dem Wert der **Microsoft.Azure.Diagnostics** und *die Eigenschaft mit dem Wert der **IaaSDiagnostics** * identifizieren eindeutig die Erweiterung Azure-Diagnose.

Der Wert der *Name* -Eigenschaft kann verwendet werden, auf die Erweiterung in der Ressourcengruppe verweisen. Speziell **Microsoft.Insights.VMDiagnosticsSettings** Einstellung aktivieren sie leicht identifiziert werden, indem der Azure klassischen Portal Portal sichergestellt wird, die die Überwachung Diagramme in der klassischen Azure-Portal ordnungsgemäß angezeigt.

Die *TypeHandlerVersion* gibt die Version der Erweiterung aus, die Sie verwenden möchten. Nebenversion **Wahr** festlegen *AutoUpgradeMinorVersion* sichergestellt werden, dass Sie die letzte Nebenversion der Erweiterung erhalten werden, die zur Verfügung. Es wird dringend empfohlen, dass Sie immer *AutoUpgradeMinorVersion* immer **true** ist festgelegt, damit Sie immer erhalten Sie die neueste verfügbaren Diagnose Erweiterung mit den neuen Features und Updates verwenden. 

Das Element *Einstellungen* enthält Konfigurationseigenschaften für die Erweiterung, die festlegen und wieder die Erweiterung (auch als öffentliche Konfiguration bezeichnet) gelesen werden kann. Die Eigenschaft *Xmlcfg* enthält XML-basierten Konfiguration für die Protokolle Diagnose-Datenquellen usw., die vom Agent Diagnose gesammelt werden sollen. Weitere Informationen zu den XML-Schema selbst finden Sie unter [Diagnose Konfigurationsschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Üblich ist, können Sie die ist-XML-Konfiguration als Variable in der Ressourcenmanager Azure-Vorlage speichern und dann verketten und base64 codiert werden, um den Wert für *Xmlcfg*festzulegen. Finden Sie im Abschnitt auf [Diagnose Konfigurationsvariablen](#diagnostics-configuration-variables) zu verstehen, Informationen dazu, wie Sie die XML-Daten in einer Variablen zu speichern. Die Eigenschaft *StorageAccount* gibt den Namen des Speicherkontos der Diagnosedaten übertragen werden sollen. 
 
Die Eigenschaften in *ProtectedSettings* (manchmal als "Privat" Konfiguration genannt) können festgelegt werden jedoch nicht wieder nach der Festlegung gelesen werden kann. Nur Schreiben Art des *ProtectedSettings* macht ihn für die Speicherung von geheimen Informationen, wie die kontoschlüssel Speicher, in dem die Diagnosedaten geschrieben werden können.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Angeben von Diagnose Speicher-Konto als Parameter 

Der Diagnose Erweiterung Json-Codeausschnitt oben wird davon ausgegangen zwei Parameter *ExistingdiagnosticsStorageAccountName* und *ExistingdiagnosticsStorageResourceGroup* das Diagnose Speicherkonto angeben, wo die Diagnosedaten gespeichert werden. Das Diagnose Speicher-Konto angeben, wie Parameter Dies ist einfach, ändern das Diagnose Speicher-Konto in verschiedenen Umgebungen z. B. möglicherweise eine andere Diagnose Speicher-Konto zum Testen und eine andere für die Herstellung Bereitstellung verwenden möchten.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Bewährte Methode ein Diagnose Speicher-Konto in einer anderen Ressourcengruppe als der Ressourcengruppe des virtuellen Computers angegeben ist. Eine Ressourcengruppe eine Bereitstellungseinheit mit einem eigenen Lebensdauer sein betrachtet, ein virtuellen Computers erneut bereitgestellt neue Konfigurationen Updates sind darauf vorgenommen, doch Sie weiterhin die Diagnosedaten in demselben Speicherkonto über diese Bereitstellungen virtuellen Computern speichern möchten und bereitgestellt werden kann. Über die Speicherkonto in einer anderen Ressource ermöglicht das Speicherkonto annehmen von Daten aus verschiedenen virtuellen Computern Bereitstellungen über die verschiedenen Versionen von Problemen zu erleichtern.

>[AZURE.NOTE] Wenn Sie eine Windows-virtuellen Computern-Vorlage von Visual Studio erstellen möglicherweise Speicher Standardkonto dasselbe Speicherkonto verwenden, in dem die virtuellen Computern virtuelle Festplatte hochgeladen wird festgelegt werden. Dies ist in der ersten Einrichtung von den virtuellen Computer zu vereinfachen. Sie sollten gestalten Sie die Vorlage, um eine andere Speicher-Konto verwenden, die als Parameter übergeben werden kann. 

## <a name="diagnostics-configuration-variables"></a>Diagnose Konfigurationsvariablen
 
Der Diagnose Erweiterung Json-Codeausschnitt oben definiert eine Variable *Accountid* zur Vereinfachung der erste im Speicher kontoschlüssel für die Speicherung Diagnose:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Die *Xmlcfg* -Eigenschaft für die Erweiterung Diagnose definiert ist, mit mehreren Variablen, die miteinander verkettet sind. Die Werte dieser Variablen werden in XML-Datei, sodass sie richtig Escapezeichen umgewandelt werden, wenn die Json-Variablen festlegen müssen.

Im folgenden wird beschrieben, das Diagnose Konfigurations-Xml, das standard-System Ebene Leistungsindikatoren sowie einige Windows-Ereignisprotokollen und Diagnose Infrastrukturprotokolle erfasst. Es wurde Escapezeichen und richtig formatiert werden, damit die Konfiguration direkt in der Variablen-Abschnitt der Vorlage eingefügt werden kann. Finden Sie die [Diagnose Konfigurationsschema](https://msdn.microsoft.com/library/azure/dn782207.aspx) für eine weitere personenbezogenen lesbare Beispiel der XML-Konfiguration aus.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Der Kennzahlen Definition XML-Knoten in der Konfiguration oben ist ein wichtiger Konfigurationselement aus, definiert werden, wie die zuvor in die XML-Daten in *PerformanceCounter* Knoten definierten Leistungsindikatoren aggregiert und gespeichert werden. 

> [AZURE.IMPORTANT] Diese Kennzahlen Laufwerk Überwachung Diagramme und Benachrichtigungen Azure-Portal an.  Wenn die überwachen virtueller Computer-Daten in der Azure-Portal angezeigt werden sollen, muss der **Kennzahlen** Knoten mit dem *ResourceID* und **MetricAggregation** in der Diagnosekonfiguration für Ihre virtuellen Computer enthalten sein. 

Im folgenden finden ein Beispiel für die XML-Daten für Kennzahlen Definitionen: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Das Attribut *ResourceID* identifiziert eindeutig des virtuellen Computers im Rahmen Ihres Abonnements. Vergewissern Sie sich mit den Funktionen subscription() und resourceGroup(), damit die Vorlage automatisch aktualisiert diese Werte basierend auf dem Abonnement und Ressourcengruppe, die Sie bereitstellen.

Wenn Sie mehrere virtuelle Computer in einer Schleife erstellen, müssen Sie den Wert *ResourceID* mit einer copyIndex()-Funktion, um jede einzelne virtueller Computer ordnungsgemäß zu unterscheiden zu füllen. Der Wert *XmlCfg* kann aktualisiert werden, damit dies unterstützt wird wie folgt:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Der Wert MetricAggregation *PT1H* und *PT1M* eine Aggregation über eine Minute und eine Aggregation mehr als eine Stunde zu kennzeichnen.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics Tabellen im Speicher

Die Kennzahlen Konfiguration oben generiert Tabellen in Ihrem Diagnose Speicher-Konto für die folgenden Benennungskonventionen:

- **WADMetrics** : Standardpräfix für alle WADMetrics Tabellen
- **PT1H** oder **PT1M** : Gibt an, dass die Tabelle über 1 Stunde oder einer Minute Aggregieren von Daten enthält.
- **P10D** : Gibt an, in der Tabelle werden Daten für 10 Tagen ab, wenn die Tabelle gestartet Sammeln von Daten enthalten
- **V2S** : Zeichenfolgenkonstante
- **JJJJMMTT** : das Datum, an dem die Tabelle gestartet Sammeln von Daten,

Beispiel: *WADMetricsPT1HP10DV2S20151108* wird Kennzahlen Daten über eine Stunde für 10 Tagen starten auf 11-Nov-2015 aggregiert enthalten.    

Jede Tabelle WADMetrics enthält die folgenden Spalten:

- **PartitionKey**: die Partitionkey basierend auf dem Wert *ResourceID* zur eindeutigen Identifizierung die Ressource virtueller Computer erstellt wird. für z. B.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : entspricht dem Format <Descending time tick>:<Performance Counter Name>. Die Berechnung der absteigender Zeit Teilstriche ist maximal Zeit Teilstriche minus die Anzeigedauer Anfang einer Periode Aggregation an. Z. B. Wenn der Stichprobe Punkt auf 10-Nov-2015 gestartet und 00:00Hrs UTC und der Berechnung wäre: DateTime.MaxValue.Ticks - (neue DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Teilstriche). Für der Speicher Verfügbare Bytes Leistung Zähler, der die Zeile Taste aussieht wie: 2519551871999999999__:005CMemory:005CAvailable:0020 Bytes
- **CounterName** : ist der Name der Leistung Zähler. Bei der Suche der *CounterSpecifier* in XML-Config definiert.
- **Maximale** : den größten Wert für den Leistungsindikator über den Zeitraum Aggregation.
- **Minimale** : den kleinsten Wert, der die Leistung Zähler über den Zeitraum Aggregation.
- **Summe** : die Summe aller Werte des Performance-Zähler über den Zeitraum Aggregation gemeldet.
- **Zählen** : die Gesamtzahl der Werte für die Leistung Zähler gemeldet.
- **Durchschnittliche** : den Mittelwert (Gesamtanzahl)-Wert, der die Leistung Zähler über den Zeitraum Aggregation.


## <a name="next-steps"></a>Nächste Schritte

- Eine vollständige Beispiel-Vorlage mit einem Windows-Computer mit der Erweiterung Diagnose finden Sie unter [201-virtueller Computer-Überwachung-Diagnose-Erweiterung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Bereitstellen der Ressource-Manager-Vorlage mit [Azure PowerShell](virtual-machines-windows-ps-manage.md) oder [Azure Befehlszeile](virtual-machines-linux-cli-deploy-templates.md)
- Erfahren Sie mehr über [authoring Ressourcenmanager Azure-Vorlagen](../resource-group-authoring-templates.md)







