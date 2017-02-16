<properties
   pageTitle="Ausführen der automatisierten Elasticsearch Stabilität Tests | Microsoft Azure"
   description="Beschreibung der, wie Sie die Tests Stabilität in Ihrer eigenen Umgebung ausgeführt werden können."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="running-the-automated-elasticsearch-resiliency-tests"></a>Ausführen der automatisierten Elasticsearch Stabilität tests

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie](guidance-elasticsearch.md).

[Konfigurieren von Stabilität]und Wiederherstellung auf Elasticsearch auf Azure[elasticsearch-resilience-recovery], wir eine Reihe von Tests, die ausgeführt wurden, anhand einer Stichprobe Elasticsearch Cluster zu bestimmen, wie gut das System auf einige allgemeine Formulare des Fehlers geantwortet, und wie gut wiederhergestellt beschrieben. Die Tests wurden Skripts verwendet, damit sie automatisierte Verfahren ausgeführt werden können. Dieses Dokument beschreibt, wie Sie die Tests in Ihrer eigenen Umgebung wiederholen können. 

Die folgenden Szenarien wurden getestet:

- **Ausfall eines und Neustart mit keine Daten verloren**. Ein Datenknoten beendet und nach 5 Minuten neu gestartet.
Elasticsearch wurde nicht neu zuordnen fehlende mehrere Shards hinweg in diesem Intervall, sodass keine zusätzliche e/a in mehrere Shards hinweg bewegen anfallen konfiguriert. Wenn Sie der Knoten neu gestartet wurde, bringt des Wiederherstellungsvorgangs der mehrere Shards hinweg auf diesem Knoten wieder auf dem neuesten Stand.

- **Ausfall eines Knotens mit schwerwiegenden Datenverlust**. Ein Datenknoten beendet ist und die Daten, die ihn enthält ist zum Simulieren von schwerwiegenden Datenträgerfehler gelöscht wird. Der Knoten dann neu gestartet wird (nach 5 Minuten), effektiv fungiert als Ersatz für den ursprünglichen Knoten. Des Wiederherstellungsvorgangs muss neu erstellt die fehlenden Daten für diesen Knoten und kann mehrere Shards hinweg frei, die auf anderen Knoten verschieben umfassen.

- **Ausfall eines und Neustart ohne Datenverlust, jedoch mit Shard erneuten Reservierung**. Ein Datenknoten angehalten, und die mehrere Shards hinweg, die ihn enthält werden an andere Knoten zugewiesen. Der Knoten neu gestartet wird, und weitere erneuten Reservierung tritt auf, um den Cluster neu zu verteilen.

- **Paralleles Updates**. Jeder Knoten im Cluster beendet und neu gestartet nach kurzer um Maschinen, die gerade neu gestartet nach einer softwareaktualisierung zu reproduzieren. Nur ein Knoten wird auf einem beliebigen Zeitpunkt gestoppt.
Mehrere Shards hinweg werden nicht erneut zugeordnet, bevor ein Knoten verfügbar ist.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die automatisierten Tests erfordern die folgenden Elemente:

- Ein Elasticsearch Cluster.

- Eine JMeter Umgebung eingerichtet wurde wie in den [Leitfaden zur Leistung testen]beschrieben. 

- Die folgenden Besonderheiten des JMeter master virtuellen Computers nur installiert.

    - Java Runtime 7.

    - Nodejs 4.x.x oder höher.

    - Die Git Befehlszeilentools.

## <a name="how-the-scripts-work"></a>Wie funktionieren die Skripts

Die Testskripts sollen auf die JMeter Master VM ausgeführt werden. Bei der Auswahl eines Tests ausführen führen Sie die Skripts den folgenden Ablauf:

1.  Starten Sie einen JMeter Testplan übergibt die Parameter, die Sie angegeben haben.

2.  Kopieren eines Skripts, das die vom Test zu eines angegebenen virtuellen Computers im Cluster erforderlichen Datenoperationen ausführt. Dies kann eine beliebige virtuellen Computers, die eine öffentliche IP-Adresse enthält, oder den *Jumpbox* virtuellen Computer sein, wenn Sie den Cluster mithilfe der [Azure Elasticsearch Schnellstart Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch)erstellt haben.

3.  Führen Sie das Skript auf virtuellen Computers (oder Jumpbox).

Die folgende Abbildung zeigt die Struktur der testumgebung und Elasticsearch Cluster. Beachten Sie, dass die Testskripts secure Shell (SSH) verwenden Sie Verbindung zu den einzelnen Knoten im Cluster verschiedene Elasticsearch Operationen wie das Beenden oder Neustarten eines Knotens ausführen.

![](./media/guidance-elasticsearch/resilience-testing1.png)

## <a name="setting-up-the-jmeter-tests"></a>Einrichten der JMeter überprüft.

Vor dem Ausführen der Stabilität überprüft sollten kompilieren und die JUnit Tests befindet sich im Ordner Stabilität/Jmeter/Tests bereitstellen. Diese Tests werden von den JMeter Testplan verwiesen werden. Weitere Informationen finden Sie unter dem Verfahren "Importieren ein vorhandenes JUnit Testprojekt in" Ellipse "ein" in [einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch bereitstellen][].

Es gibt zwei Versionen der JUnit Tests frei, die in den folgenden Ordnern:

- **Elasticsearch17.** Das Projekt in diesem Ordner wird die Datei Elasticsearch17.jar generiert. Verwenden Sie diese JAR Testzwecken Elasticsearch Versionen 1.7.x haben

- **Elasticsearch20**. Das Projekt in diesem Ordner wird die Datei Elasticsearch20.jar generiert. Verwenden Sie diese JAR Testzwecken Elasticsearch Version 2.0.0 und höher

Kopieren Sie die entsprechende JAR-Datei mit den restlichen die Abhängigkeiten auf Ihren Computern JMeter. Der Vorgang wird durch das Verfahren "Bereitstellen von eine Prüfung JUnit JMeter" bei der [Bereitstellung von einer JMeter JUnit Demo testen Elasticsearch Leistung]beschrieben.

## <a name="configuring-vm-security-for-each-node"></a>Konfigurieren der Sicherheit von virtuellen Computer für die einzelnen Knoten

Die Testskripts erfordern eine Authentifizierungszertifikat auf den einzelnen Knoten Elasticsearch im Cluster installiert sein. Dadurch wird die Skripts, die automatisch abläuft, ohne dass eine Benutzernamen oder Kennwort beim Herstellen der Verbindung mit den verschiedenen virtuellen Computern.

Beginnen Sie mit der Anmeldung bei einem Knoten im Cluster Elasticsearch (oder die Jumpbox VM), und führen Sie den folgenden Befehl Authentifizierungsschlüssel erzeugt:

```Shell
ssh-keygen -t rsa
```

Während der Elasticsearch Knoten (oder Jumpbox) angeschlossen ist, führen Sie die folgenden Befehle für jeden Knoten, in denen Elasticsearch Cluster. Ersetzen Sie `<username>` mit dem Namen eines gültigen Benutzers auf jeden virtuellen Computer, und Ersetzen `<nodename>` mit dem DNS-Namen oder die IP-Adresse des den virtuellen Computer den Knoten Elasticsearch hosten.
Beachten Sie, dass Sie das Kennwort des Benutzers aufgefordert werden, wenn diese Befehle ausgeführt.
Weitere Informationen finden Sie unter [SSH Benutzernamen ohne Kennwort](http://www.linuxproblem.org/art_9.html):

```Shell
ssh <username>@<nodename> mkdir -p .ssh (
cat .ssh/id\_rsa.pub | ssh <username>*@<nodename> 'cat &gt;&gt; .ssh/authorized\_keys'
```

## <a name="downloading-and-configuring-the-test-scripts"></a>Herunterladen und konfigurieren die Testskripts

Die Testskripts werden in einem Repository Git bereitgestellt. Verwenden Sie das folgende Verfahren zum Herunterladen und konfigurieren die Skripts aus.

Öffnen Sie auf dem Folienmaster JMeter Computer, in dem Sie die Tests ausführen möchten ein Git desktop (Git Bash) und Klonen Sie Repository der Skripts, wie folgt enthält:

```Shell
git clone https://github.com/mspnp/azure-guidance.git
```

Verschieben in den Ordner Stabilität-Tests, und führen Sie den folgenden Befehl aus, die zum Ausführen der Tests erforderlichen Abhängigkeiten zu installieren:

```Shell
npm install
```

Wenn Sie das Master-Shape JMeter unter Windows ausgeführt wird, das Herunterladen Sie [Plink](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), eine Line-Benutzeroberfläche für die kitten Telnetclient. Kopieren Sie die ausführbare Plink in den Ordner Stabilität-Tests/Bibliothek aus.

Wenn Sie das Master-Shape JMeter auf Linux ausgeführt wird, müssen Sie nicht Plink herunterladen, aber konfigurieren Kennwort zugängliche SSH zwischen JMeter Master und den Elasticsearch-Knoten oder Jumpbox Sie anhand der Schritte in der Anleitung verwendet werden sollen "Konfigurieren von virtuellen Computer für die einzelnen Knoten Sicherheit." 

Bearbeiten Sie die folgenden Konfigurationsparameter in der `config.js` -Datei mit Ihren testumgebung und Elasticsearch Cluster übereinstimmt. Diese Parameter gelten für alle Tests zur Verfügung:

| Namen | Beschreibung | Standardwert |
| ---- | ----------- | ------------- |
| `jmeterPath` | Lokaler Pfad sich JMeter befindet. | `C:/apache-jmeter-2.13` |
| `resultsPath` | Relative Verzeichnis, in dem das Skript das Ergebnis speichert. | `results` |
| `verbose` | Gibt an, ob das Skript im ausführlichen Modus oder nicht ausgegeben. | `true` |
| `remote` | Gibt an, ob die Tests JMeter lokal oder auf remote-Servern ausführen. | `true` |
| `cluster.clusterName` | Der Name des Elasticsearch Cluster. | `elasticsearch` |
| `cluster.jumpboxIp`         | Die IP-Adresse des Computers Jumpbox.                 |-|
| `cluster.username`          | Der Administratorbenutzer, die, den Sie beim Bereitstellen von Cluster erstellt haben. |-|
| `cluster.password`          | Das Kennwort für den Administratorbenutzer.                        |-|
| `cluster.loadBalancer.ip`   | Die IP-Adresse des Elasticsearch Lastenausgleich.    |-|
| `cluster.loadBalancer.url`  | Basis-URL des Lastenausgleich.                          |-|

## <a name="running-the-tests"></a>Ausführen der tests

Verschieben in den Ordner Stabilität-Tests, und führen Sie den folgenden Befehl aus:

```Shell
node app.js
```

Das folgende Menü sollte angezeigt werden:

![](./media/guidance-elasticsearch/resilience-testing2.png)

Geben Sie die Anzahl der dem Szenario, das Sie ausführen möchten: `11`, `12`, `13` oder `21`. 

Nachdem Sie ein Szenario auswählen, wird automatisch der Test ausgeführt. Die Ergebnisse werden als eine Reihe von durch Kommas getrennte Werte (CSV) Dateien in einem Ordner erstellt haben, klicken Sie unter Ergebnisverzeichnis gespeichert. Jeder Testlauf weist einen eigenen Ergebnisordner.
Sie können Excel zu analysieren und diese Daten von Aktienkursen.

[Running Elasticsearch on Azure]: guidance-elasticsearch-running-on-azure.md
[Tuning Data Ingestion Performance for Elasticsearch on Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Leitfaden zur Leistung testen]: guidance-elasticsearch-creating-performance-testing-environment.md
[JMeter guidance]: guidance-elasticsearch-implementing-jmeter.md
[Considerations for JMeter]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Query aggregation and performance]: guidance-elasticsearch-query-aggregation-performance.md
[elasticsearch-resilience-recovery]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Resilience and Recovery Testing]: guidance-elasticsearch-running-automated-resilience-tests.md
[Bereitstellen einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
