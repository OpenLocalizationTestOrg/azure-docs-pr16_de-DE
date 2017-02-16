<properties
   pageTitle="Anwendungsupgrade: Erweiterte Themen | Microsoft Azure"
   description="Dieser Artikel behandelt einige erweiterten Themen zur Aktualisierung einer Fabric Service-Anwendungs."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Dienst Fabric Anwendungsupgrade: Erweiterte Themen

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Hinzufügen oder Entfernen von Diensten bei einem Anwendungsupgrade

Wenn Sie ein neuer Dienst zur Anwendung hinzugefügt wurde, die bereits bereitgestellt wird, und als Aktualisierung veröffentlicht, wird der neue Dienst bereitgestellte Anwendung hinzugefügt.  Eine solche Aktualisierung wirkt beliebige dieser Dienste sich nicht, die bereits Teil der Anwendung wurden. Für den neuen Dienst aktiv sein muss jedoch eine Instanz des Diensts, das hinzugefügt wurde gestartet werden (mit den `New-ServiceFabricService` Cmdlet).

Services können auch über eine Anwendung als Teil einer Aktualisierung entfernt werden. Bevor Sie mit dem Upgrade müssen jedoch alle aktuelle Instanzen des Diensts zu be-gelöschte beendet werden (mit den `Remove-ServiceFabricService` Cmdlet). 

## <a name="manual-upgrade-mode"></a>Manuelle Upgrade-Modus

> [AZURE.NOTE]  Der nicht überwachte manuelle Modus sollten nur für ein Upgrade Fehler beim oder angehalten berücksichtigt werden. Der überwachte Modus ist die empfohlene Upgrade für Applikationen Dienst Fabric.

Azure Service-Struktur bietet mehrere Upgrade Modi Entwicklung und Herstellung Cluster unterstützt. Optionen für die Bereitstellung ausgewählt möglicherweise bei anderen Umgebungen unterschiedlich.

Überwachten paralleles die Aktualisierung der Anwendung ist der am häufigsten verwendete Aktualisierung in Herstellung verwenden. Wenn die Richtlinie Upgrade angegeben wird, ist Service Fabric sichergestellt, dass die Anwendung fehlerfrei ist, bevor das Upgrade fortgesetzt wird.

 Den manuelle paralleles Anwendung Upgrade Modus können der Anwendungsadministrator total Kontrolle über den Fortschritt der Aktualisierung durch den verschiedenen Upgrade Domänen haben. Dieser Modus ist nützlich, wenn eine angepasste oder komplexe Gesundheit Auswertung Richtlinie erforderlich ist, oder ein nicht konventionelle Upgrades geschieht (beispielsweise die Anwendung ist bereits Datenverlust).

Schließlich empfiehlt sich die automatische Aktualisierung im Wechsel Anwendung für die Entwicklung oder Tests Umgebungen einen schnellen Iterationszyklus während der Dienstentwicklung bereitstellen.

## <a name="change-to-manual-upgrade-mode"></a>Wechseln Sie zum manuellen Upgrade-Modus
**Manueller**– die Aktualisierung der Anwendung bei der aktuellen UD beenden und den Upgrade Modus auf nicht überwacht manuell ändern. Der Administrator muss manuell aufrufen **MoveNextApplicationUpgradeDomainAsync** , um das Upgrade fortzusetzen oder auslösen zurücksetzen, indem Sie ein neues Upgrade initiieren. Nachdem das Upgrade in einen manuellen Modus eingibt, bleibt er im manuellen Modus, bis eine neue Aktualisierung gestartet wird. Der Befehl **GetApplicationUpgradeProgressAsync** gibt FABRIC\_Anwendung\_UPGRADE\_Zustand\_PARALLELEN\_weiterleiten\_ausstehend.

## <a name="upgrade-with-a-diff-package"></a>Mit einem Vergleich Paket aktualisieren

Eine Fabric Service-Anwendung kann durch provisioning mit eine vollständige und unabhängige Anwendungspaket aktualisiert werden. Eine Anwendung kann auch mithilfe eines Unterschiede Pakets, das nur die aktualisierten Anwendungsdateien, das aktualisierte Anwendungsmanifest und der Dienst Manifestdateien enthält aktualisiert werden.

Eine vollständige Anwendungspaket enthält alle Dateien zum Starten und Ausführen einer Fabric Service-Anwendungs. Ein Vergleich-Paket enthält nur die Dateien, die zwischen der letzten bereitstellen und der aktuellen Aktualisierung geändert sowie das vollständige Anwendungsmanifest und den Dienst Manifestdateien. Jeder Verweis in das Anwendungsmanifest oder Servicemanifest, die im Layout erstellen nicht gefunden werden kann, wird im Bild Store für durchsucht.

Vollständige Anwendungspakete sind für die erste Installation einer Anwendung zum Cluster erforderlich. Aktualisierungen können entweder eine vollständige Anwendung oder Unterschiede Paket sein.

Jede Gelegenheit bei Verwendung eines Pakets Vergleich eine gute Wahl werden:

* Ein Vergleich Paket ist bevorzugten, wenn Sie eine umfangreiche Anwendungspaket haben, die mehrere Dienst Manifestdateien und/oder mehrere Code-Paketen, Config-Paketen oder Paketen Daten verweist.

* Ein Paket Unterschiede ist bevorzugten, wenn Sie ein Bereitstellung verwenden, der das Layout erstellen direkt aus Ihrer Anwendung erstellen Prozess generiert. In diesem Fall, obwohl der Code nicht geändert, erhalten neu erstellten Assemblys zu eine andere Prüfsumme. Verwenden eines Anwendungspakets vollständige benötigen Sie die Version auf alle Code-Paketen aktualisieren. Verwenden eines Pakets Unterschiede, geben Sie nur die Dateien, die geändert und der Manifestdateien, wenn sich die Version geändert hat.

Wenn eine Anwendung mit Visual Studio aktualisiert wird, wird das Paket Unterschiede automatisch veröffentlicht. Um ein Paket Unterschiede manuell zu erstellen, das Anwendungsmanifest und die Manifeste Dienst aktualisiert werden müssen, aber nur die geänderten Pakete in das Paket endgültige Anwendung berücksichtigt werden sollen. 

Beispielsweise beginnen wir mit der folgenden Anwendung (Versionsnummern für erleichterte Bedienung Grundlegendes zu bereitgestellten):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Nun angenommen, Sie nur das Code Paket von service1 mithilfe eines Unterschiede Pakets mithilfe der PowerShell aktualisieren möchten. Die aktualisierte Anwendung verfügt jetzt über die folgende Ordnerstruktur:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

In diesem Fall aktualisieren Sie das Anwendungsmanifest 2.0.0 und Servicemanifests für service1 entsprechend das Code Paket aktualisieren. Der Ordner für Ihr Anwendungspaket hätte die folgende Struktur:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Ihrer Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch eine Anwendung Aktualisierung mit Visual Studio.

[Aktualisieren Ihrer Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine Anwendung Aktualisierung mithilfe der PowerShell.

Steuern Sie, wie eine Anwendung upgrades mithilfe von [Parametern zu aktualisieren](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Upgrades der Anwendung kompatibel, indem Sie lernen, wie Sie [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung von Verweisen auf die Schritte in der [Anwendungsupgrades Problembehandlung](service-fabric-application-upgrade-troubleshooting.md).
 
