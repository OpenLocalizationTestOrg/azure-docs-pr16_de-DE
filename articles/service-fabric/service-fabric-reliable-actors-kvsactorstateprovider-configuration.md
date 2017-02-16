<properties
   pageTitle="Übersicht über die Konfiguration von Azure Service Fabric zuverlässigen Akteuren KVSActorStateProvider | Microsoft Azure"
   description="Lernen Sie Azure Service Fabric dynamische Akteuren vom Typ KVSActorStateProvider konfigurieren."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfigurieren von zuverlässigen Akteuren – KVSActorStateProvider
Sie können die standardmäßige Konfiguration der KVSActorStateProvider ändern, durch Ändern der settings.xml-Datei, die für den angegebenen Akteur im Stammverzeichnis Microsoft Visual Studio-Paket unter dem Ordner Config generiert wird.

Die Laufzeit Azure Service Fabric für vordefinierte Abschnittsnamen in der Datei settings.xml sucht und es verbraucht die Konfigurationswerte beim Erstellen der zugrunde liegenden Runtime-Komponenten.

>[AZURE.NOTE] Führen Sie **nicht** die Abschnittsnamen der folgenden Konfigurationen settings.xml Datei, die in Visual Studio-Lösung generiert wird, ändern oder löschen.

## <a name="replicator-security-configuration"></a>Die Konfiguration der Replicator-Sicherheit
Replicator Sicherheitskonfigurationen werden verwendet, um die Kommunikationskanal zu sichern, der während der Replikation verwendet wird. Dies bedeutet, dass Dienste gegenseitig Replikationsdatenverkehr, um sicherzustellen, dass die Daten, die hochgradig zur Verfügung gestellt werden auch sicher sind sehen können.
Standardmäßig wird verhindert, dass ein leere Sicherheit Konfigurationsabschnitt Replikation Sicherheit.

### <a name="section-name"></a>Namen des Abschnitts
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicator Konfiguration
Replicator Konfigurationen Konfigurieren der Replikations-Dienstes, die für die Herstellung des Akteur State Provider Zustands hochgradig zuverlässigen zuständig ist.
Die Konfiguration wird von der Visual Studio-Vorlage generiert und in den meisten Fällen. In diesem Abschnitt spricht über zusätzliche Konfigurationen beschrieben, die zum Optimieren der Replikations-Dienstes verfügbar sind.

### <a name="section-name"></a>Namen des Abschnitts
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Von Konfigurationsnamen

|Namen|Einheit|Standardwert|Hinweise|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0.015|Zeitraum, für den Hintergrund der Replikations-Dienstes bei der sekundäre wartet nach Erhalt eines Vorgangs vor dem Senden eine Bestätigung mit dem primären. Alle anderen Empfangsbestätigungen für Vorgänge, die innerhalb dieses Zeitraums verarbeitet gesendet werden, werden als eine Antwort gesendet.|
|ReplicatorEndpoint|N/V|Keine standardmäßigen--erforderliche parameter|Legen Sie die IP-Adresse und den Port, die der primären/sekundären Replikations-Dienstes zur Kommunikation mit anderen Replikatoren in der Replikation verwendet wird. Dies sollte einen TCP-Ressource Endpunkt in Servicemanifests verwiesen werden. Finden Sie in [Service Manifestressourcen](service-fabric-service-manifest-resources.md) , lesen Weitere Informationen zum Definieren von Endpunkt Ressourcen in Servicemanifests. |
|RetryInterval|Sekunden|5|Zeitraum, nach dem der Replikations-Dienstes eine Nachricht erneut übermittelt, wenn sie nicht über eine Bestätigung für einen Vorgang erhält.|
|MaxReplicationMessageSize|Bytes|50 MB|Maximale Größe von Replikationsdaten, die in einer einzelnen Nachricht übertragen werden können.|
|MaxPrimaryReplicationQueueSize|Anzahl von Vorgängen|1024|Maximale Anzahl von Vorgängen in der primären Warteschlange. Ein Vorgang ist frei, nachdem der primären Replikations-Dienstes von allen der sekundären Vervielfältigern eine Bestätigung erhalten. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl von Vorgängen|2048|Maximale Anzahl von Vorgängen in der Warteschlange sekundäre. Ein Vorgang ist nach dem vornehmen Zustand hochgradig verfügbar bis Beibehaltung freigegeben. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|

## <a name="store-configuration"></a>Konfiguration zum Speichern von
Store-Konfigurationen werden verwendet, um die im lokalen Speicher konfiguriert werden, der verwendet wird, um den Zustand beizubehalten, der repliziert werden.
Die Konfiguration wird von der Visual Studio-Vorlage generiert und in den meisten Fällen. In diesem Abschnitt spricht über zusätzliche Konfigurationen beschrieben, die zum Optimieren der lokalen Speichers verfügbar sind.

### <a name="section-name"></a>Namen des Abschnitts
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Von Konfigurationsnamen

|Namen|Einheit|Standardwert|Hinweise|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Millisekunden|200|Legt die maximale Batchverarbeitung Intervall für dauerhaften lokalen Speicher Commit an.|
|MaxVerPages|Anzahl der Seiten|16384|Speichern Sie die maximale Anzahl von Version Seiten in die lokale Datenbank ein. Es bestimmt die maximale Anzahl von ausstehenden Transaktionen.|

## <a name="sample-configuration-file"></a>Beispiel-Konfigurationsdatei

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Hinweise

Der Parameter BatchAcknowledgementInterval steuert Replikationswartezeit. Der Wert "0" angegeben, ergibt sich die niedrigste mögliche Wartezeit, aber Durchsatz (wie weitere Bestätigungsnachrichten gesendet und verarbeitet, jede mit weniger Empfangsbestätigungen werden müssen).
Der Wert für BatchAcknowledgementInterval größer, je höher der Replikation Gesamtdurchsatz, aber höhere Operation Wartezeit. Dies übersetzt direkt in die Wartezeit der Transaktion übermittelt.
