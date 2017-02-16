<properties
   pageTitle="Verwalten von mehreren Umgebungen Dienst Struktur | Microsoft Azure"
   description="Dienst Fabric Applikationen können auf Cluster ausgeführt werden, die in der Größe von einem Computer Tausende von Autos liegen. In einigen Fällen werden Sie die Anwendung für diese verfügt über Umgebungen unterschiedlich konfigurieren möchten. In diesem Artikel beschrieben, wie andere Anwendungsparameter pro Umgebung definieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Verwalten der Anwendungsparameter für mehrere Umgebungen

Sie können mithilfe von überall aus mehrwertiges Nachschlagefeld zu viele Tausende von Autos Azure Service Fabric Cluster erstellen. Während der Anwendungsbinärdateien ohne Änderung über diese Breite Spektrum der Umgebungen ausgeführt werden können, sollten Sie häufig zum Konfigurieren der Anwendung unterschiedlich, je nach Anzahl von Computern, die Sie bereitstellen möchten.

Ein einfaches Beispiel, sollten Sie `InstanceCount` für einen statusfreie Dienst. Wenn Sie eine Anwendung in Azure ausgeführt werden, werden Sie in der Regel für diesen Parameter den speziellen Wert "1" festlegen möchten. Dadurch wird sichergestellt, dass es sich bei Ihrem Dienst auf den einzelnen Knoten im Cluster ausgeführt wird. Diese Konfiguration ist jedoch nicht für einen Cluster mit einem einzelnen Computer geeignet, da Sie mehrere Prozesse Abhören dieselbe IP-Adresse auf einem einzigen Computer haben, können. Sie werden in der Regel legen Sie stattdessen `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Umgebung-spezifische Parameter angeben

Die Lösung für dieses Konfigurationsproblem ist eine Reihe von parametrisierte Standarddienste und Anwendung Parameterdateien, die für eine angegebene-Umgebung diese Parameterwerte ausgefüllt. Standarddiensten und Anwendungsparameter werden in der Anwendung und Service-Manifeste konfiguriert. Die Schemadefinition für die Dateien ServiceManifest.xml und ApplicationManifest.xml wird mit dem Dienst Fabric SDK und Tools *C:\Programme c:\Programme\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*installiert.

### <a name="default-services"></a>Standarddienste

Dienst Fabric Applikationen bestehen aus eine Auflistung von Dienstinstanzen. Es ist, zwar möglich, dass Sie eine leere Anwendung erstellen und dann alle Dienstinstanzen dynamisch erstellen haben die meisten Applikationen eine Reihe von Core Services, die immer erstellt werden soll, wenn die Anwendung instanziiert wird. Diese werden als "Standarddienste" bezeichnet. Sie werden im Anwendungsmanifest mit Platzhaltern für die Konfiguration pro-Umgebung in eckigen Klammern eingeschlossen angegeben:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Jeder der benannten Parameter muss innerhalb des Anwendungsmanifests der Parameter-Element definiert werden:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Das Attribut der Standardwert gibt den Wert in das Fehlen eines Parameters spezifischere für eine angegebene-Umgebung verwendet werden.

>[AZURE.NOTE] Nicht alle Dienst Instanzparameter eignen sich für die Konfiguration-Umgebung. Im Beispiel oben werden die Werte LowKey und HighKey für Partitionierungsschema des Diensts explizit für alle Instanzen des Diensts definiert, da der Partition Bereich eine Funktion der Datendomäne, nicht der Umgebung ist.


### <a name="per-environment-service-configuration-settings"></a>Konfigurations-diensteinstellungen-Umgebung

Der [Dienst Fabric Anwendungsmodell](service-fabric-application-model.md) können Dienste Konfigurationspaketen enthalten, die benutzerdefinierte Schlüssel-Wert-Paare enthalten, die zur Laufzeit gelesen werden. Die Werte für diese Einstellungen können auch unterscheiden sich durch-Umgebung, die durch die Angabe einer `ConfigOverride` im Anwendungsmanifest.

Angenommen, Sie die folgende Einstellung in der Datei Config\Settings.xml für haben die `Stateful1` Dienst:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Wenn Sie diesen Wert für eine bestimmte Anwendung-Umgebung Paar außer Kraft setzen, Erstellen einer `ConfigOverride` beim Importieren von Servicemanifests im Anwendungsmanifest.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Für diesen Parameter kann dann durch Umgebung konfiguriert werden, wie oben gezeigt. Dies ist möglich, indem sie im Abschnitt Parameter des Anwendungsmanifests deklarieren und Umgebung-spezifische Werte in die Anwendung Parameter-Dateien angeben.

>[AZURE.NOTE] Bei Konfiguration diensteinstellungen, es gibt drei Stellen, in dem der Wert eines Schlüssels festgelegt werden kann: das Service-Konfigurations-Paket, das Anwendungsmanifest und die Anwendung Parameter-Datei. Dienst Fabric wird immer aus der Anwendung Parameterdatei auswählen ersten (sofern angegeben), klicken Sie dann auf das Anwendungsmanifest und schließlich auf das Konfigurationspaket.


### <a name="application-parameter-files"></a>Parameter-Dateien der Anwendung

Projekt für die Fabric Service-Anwendung kann eine oder mehrere Anwendung Parameterdateien einbeziehen. Jede von ihnen definiert die spezifischen Werte für die Parameter, die im Anwendungsmanifest definiert sind:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Standardmäßig umfasst eine neue Anwendung zwei Anwendung Parameterdateien mit der Bezeichnung Local.xml und Cloud.xml:

![Parameter-Dateien in Lösung Explorer Anwendung][app-parameters-solution-explorer]

Zum Erstellen einer neuen Parameterdatei einfach kopieren und Einfügen einer vorhandenes und geben sie einen neuen Namen.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identifizieren von Umgebung-spezifischen Parametern während der Bereitstellung

Zum Zeitpunkt der Bereitstellung müssen Sie die entsprechenden Parameterdatei mit der Anwendung übernehmen auswählen. Sie können über das Dialogfeld Veröffentlichen in Visual Studio oder über PowerShell ausführen.

### <a name="deploy-from-visual-studio"></a>Bereitstellen von Visual Studio

Sie können aus der Liste der verfügbaren Parameterdateien auswählen, wenn Sie eine Anwendung in Visual Studio veröffentlichen.

![Wählen Sie eine Parameterdatei in das Dialogfeld ' veröffentlichen '][publishdialog]

### <a name="deploy-from-powershell"></a>Bereitstellen von PowerShell

Die `Deploy-FabricApplication.ps1` PowerShell-Skript enthalten, in der Anwendung Projektvorlage akzeptiert ein Veröffentlichungsprofil als Parameter und der PublishProfile enthält einen Verweis auf die Datei der Anwendung.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über einige der grundlegenden Konzepte, die in diesem Thema behandelt werden, finden Sie unter der [Dienst Fabric – technische Übersicht](service-fabric-technical-overview.md). Informationen zu anderen app Management-Funktionen, die in Visual Studio verfügbar sind, finden Sie unter [Verwalten von Ihrem Dienst Fabric Applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
