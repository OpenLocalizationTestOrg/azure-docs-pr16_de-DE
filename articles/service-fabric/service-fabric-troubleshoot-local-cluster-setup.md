<properties
   pageTitle="Behandeln von Problemen mit Ihrem lokalen Dienst Fabric Cluster Setup | Microsoft Azure"
   description="Dieser Artikel behandelt das eine Reihe von Vorschlägen zur Behandlung dieses Problems Ihren Cluster lokale Entwicklung"
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
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Behandeln von Problemen mit Ihrem lokalen Entwicklung Cluster-setup

Wenn Sie ein Problem bei der Interaktion mit Ihrem lokalen Azure Service Fabric Entwicklung Cluster auftreten, überprüfen Sie die folgenden Vorschläge für mögliche Lösungen.

## <a name="cluster-setup-failures"></a>Setupfehler Cluster

### <a name="cannot-clean-up-service-fabric-logs"></a>Kann nicht Dienst Fabric Protokolle bereinigen.

#### <a name="problem"></a>Problem

Während der Ausführung des Skripts DevClusterSetup, wird einen Fehler wie folgt:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Lösung

Schließen Sie das aktuelle PowerShell-Fenster, und öffnen Sie ein neues Fenster der PowerShell als Administrator. Sie sollten jetzt das Skript erfolgreich ausgeführt werden.

## <a name="cluster-connection-failures"></a>Cluster Verbindungsfehlern

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Dienst Fabric PowerShell-Cmdlets sind in PowerShell Azure nicht erkannt.

#### <a name="problem"></a>Problem

Wenn Sie versuchen, den Dienst Fabric PowerShell-Cmdlets wie ausführen `Connect-ServiceFabricCluster` in einem Fenster Azure PowerShell es fehlschlägt, sagen, dass das Cmdlet nicht erkannt wird. Der Grund dafür ist, dass Azure PowerShell die 32-Bit-Version von Windows PowerShell (auch in der 64-Bit-Betriebssystemversionen) verwendet, während der Dienst Fabric Cmdlets nur in 64-Bit-Umgebungen funktionieren.

#### <a name="solution"></a>Lösung

Führen Sie immer Dienst Fabric Cmdlets direkt von Windows PowerShell ein.

>[AZURE.NOTE] Die neueste Version von Azure PowerShell erstellt eine spezielle Verknüpfung, keine, sodass diese nicht mehr ausgeführt werden sollen.

### <a name="type-initialization-exception"></a>Geben Sie Initialization-Ausnahme

#### <a name="problem"></a>Problem

Wenn Sie eine Verbindung mit dem Cluster in PowerShell sind, finden Sie den Fehler TypeInitializationException für System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Lösung

Die Path-Variable wurde während der Installation nicht ordnungsgemäß festgelegt. Melden Sie sich bei Windows ab, und melden Sie sich wieder an. Dadurch wird den Pfad vollständig aktualisiert.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Cluster datenbankverbindung mit "ist Objekt geschlossen"

#### <a name="problem"></a>Problem

Anruf an verbinden-ServiceFabricCluster tritt ein Fehler wie folgt:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Lösung

Schließen Sie das aktuelle PowerShell-Fenster, und öffnen Sie ein neues Fenster der PowerShell als Administrator. Sie sollten jetzt hergestellt sein.

### <a name="fabric-connection-denied-exception"></a>Verbindung verweigert Fabric-Ausnahme

#### <a name="problem"></a>Problem

Beim Debuggen von Visual Studio wird eine Fehlermeldung FabricConnectionDeniedException angezeigt.

#### <a name="solution"></a>Lösung

Dieser Fehler tritt normalerweise auf, wenn Sie versuchen zu testen So starten Sie einen Diensthost manuell, verarbeiten, anstatt durch die Runtime Fabric Service für Sie zu starten.

Stellen Sie sicher, dass Sie keine Dienstprojekte festlegen als beim Startprojekte in der Lösung verfügen. Nur Fabric Service-Anwendungsprojekte sollte als Startprojekte festgelegt werden.

>[AZURE.TIP] Wenn Setup folgen, Ihrer lokale Cluster nicht ordnungsgemäß Verhalten beginnt, Sie können es mithilfe der lokalen Cluster Manager-System über Taskleiste Anwendungs zurücksetzen. Entfernt den vorhandenen Cluster und Einrichten eines neuen Kontos. Bitte beachten Sie, dass alle Applikationen bereitgestellt und zugeordnete Daten entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Verstehen Sie und Behandeln von Problemen mit der Cluster mit Integritätsberichte system](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Visualisieren Sie Ihre Cluster mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)
