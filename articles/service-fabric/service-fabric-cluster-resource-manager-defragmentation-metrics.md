<properties
   pageTitle="Defragmentierung der Kennzahlen Azure Service Struktur | Microsoft Azure"
   description="Übersicht über die Defragmentierung verwenden oder Packen als eine Strategie für Kennzahlen Dienst Struktur"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentierung der Metrik und Laden Dienst Struktur
Der Dienst Fabric Cluster Ressourcenmanager hauptsächlich mit Lastenausgleich im Hinblick auf die Last verteilen – und stellen Sie sicher, dass alle Knoten im Cluster gleichmäßig ausgelastet sind befasst. Dies ist normalerweise die sicherste und smartest Layout im Hinblick auf funktionierenden Fehlern, da sichergestellt wird, dass sich einige Großteil eine bestimmte Auslastung jeder angegebenen Fehler treffen nicht. Der Dienst Fabric Cluster Ressourcenmanager unterstützt eine andere Strategie sowie, welche Defragmentierung ist. Defragmentierung weist normalerweise darauf hin, dass anstelle von versuchen, die Nutzung einer Metrik über den Cluster verteilen, wir tatsächlich versuchen, sie konsolidieren. Hierbei handelt es sich um eine zu bündeln Umkehrung der unserer normalen Strategie – statt basierten auf die durchschnittliche Standardabweichung metrischen Auslastung für eine bestimmte Metrik minimieren Cluster optimieren, zunächst für erhöhten Standardabweichung optimieren. Aber warum ist es sinnvoll, diese Strategie?

Tja, wenn Sie die Last gleichmäßig zwischen den Knoten im Cluster ausdehnen haben haben klicken Sie dann Sie einen Teil der Ressourcen bestimmt, die die Knoten bieten. Normalerweise ist dies kein Problem, aber in manchen Fällen einige Auslastung Services sind besonders große und nutzen die meisten eines Knotens erstellen – sagen 75 % bis 95 % des Knotens Ressourcen würde einhandeln dedizierten zu einem einzelnen Dienstinstanz oder Replikat. Dies kein Problem aufgetreten ist, wird der Cluster Ressourcenmanager zum Zeitpunkt der Erstellung Dienst, für die es Neuorganisieren Cluster akzeptieren, um Platz für diese großen Arbeitsbelastung, und legen Sie darum, die es erfolgen muss erkennen, aber in der Zwischenzeit die Arbeitsbelastung warten, bis er im Cluster geplant werden muss.

Vorausgesetzt, dass die Planung des neuen Auslastung normalerweise mindestens ein wenig Wartezeit vertrauliche, wenn sich etwas anders vorgehen werden nicht können wir manchmal Schlag rechts, indem Sie diese SLAs, wenn es zahlreiche Dienste und Bundesstaat gibt versehen, verschieben, insbesondere dann, wenn diese im Cluster im Allgemeinen groß sind (und daher dauert länger zum Navigieren im Cluster). Tatsächlich um, wenn wir Erstellung Uhrzeiten in Simulationen basierend auf den richtigen Clusterdaten gemessen, gesehen haben wir die Wenn Services groß genug wurden und Cluster wurde Recht, dass die Erstellung einer großen Dienste verlangsamt werden möchten, und wir dies verbessern konnte, indem Sie die Einführung in die Richtlinie der Defragmentierung Kennzahlen genutzt.

Wie Datei konnte Erstellung oder Access verlangsamt erhalten Wenn Festplatte des Computers fragmentiert wurde und konnte durch das Laufwerk defragmentieren, damit es größere zusammenhängende Blöcke verfügbar wurden beschleunigt, Sie konfigurieren können, Defragmentierung Kennzahlen, damit der Cluster Ressourcenmanager vorausschauende versuchen, die Laden der Dienste in weniger Knoten verkleinern, sodass vorhanden (fast) immer Platz für selbst umfangreiche Services ist , aktivieren sie schnell erstellt werden. Die meisten Personen nicht erforderlich, da Dienste sollten in der Regel kleine und daher ist es nicht schwer zu Platz für sie zu finden, aber wenn Sie benötigt diese schnell erstellt und haben große Services (und Sie bereit sind, akzeptieren die anderen vor-und Nachteile wie höhere Impactfulness von Fehlern und einige Tipps, die genutzte überlassen bleibt, während sie warten, bis die Auslastung geplant werden) und dann die Defragmentierung Strategie ist es für Sie.

Im nachstehenden Diagramm bietet eine visuelle Darstellung der zwei verschiedene Cluster, eine der defragmentiert wird und eine also nicht. Erwägen Sie in der angeglichene Groß-/Kleinschreibung die abgestimmt wäre erforderlich, vor Ort eines der größten Serviceobjekte, wenn eine neue wurden erstellt werden, im Vergleich zum defragmentierten Cluster, wo diese sofort auf Knoten 4 oder 5 platziert werden konnte.

![Vergleichen von Lastenausgleich und defragmentiert Cluster][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentierung vor- und Nachteile
Was sind die anderen konzeptionelle Kompromisse? Wir empfehlen sorgfältige Maße Ihrer Auslastung bevor Defragmentierung Kennzahlen einschalten. Hier ist eine schnelle Tabelle enthält wichtige anzustellen:

| Defragmentierung finden IT-Experten  | Defragmentierung aufzulisten |
|----------------------|----------------------|
|Ermöglicht die Erstellung schneller großen services | Konzentriert sich auf weniger Knoten, zunehmender Konflikte laden
|Ermöglicht niedrigere Verschieben von Daten während der Erstellung    | Fehler können weitere Dienste auswirken und dazu führen, dass weitere Änderung
|Ermöglicht umfassende Beschreibung der Anforderungen und Freigeben von Speicherplatz | Komplexere Konfiguration für insgesamt Ressourcenverwaltung

Können Sie im selben Cluster-Kriterien defragmentierte und normale mischen und Ressourcenmanager kann es am besten, um sicherzustellen, dass Sie ein Layout, die so viele der Defragmentierung Metrik konsolidiert werden erhalten, wenn sie versuchen, den Rest zu wobei. Die Anzahl der Lastenausgleich im Vergleich zu die Anzahl der Defragmentierung Kennzahlen und-Stärken von aktuellen lädt, usw. Metrik ist die genauen Ergebnisse, die Sie erzielen abhängig.

## <a name="configuring-defragmentation-metrics"></a>Konfigurieren der Defragmentierung Kennzahlen
Konfigurieren der Defragmentierung Metrik ist eine globale Entscheidung im Cluster, und einzelne Metrik für die Defragmentierung ausgewählt werden können:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Nächste Schritte
- Ressourcenmanager Cluster verfügt über zahlreiche Optionen für die Beschreibung des Clusters. Um herauszufinden Auschecken Weitere Informationen zu diesen Artikels zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
- Kennzahlen gibt an, wie der Dienst Fabric Cluster Ressource-Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie, lesen Sie weitere Informationen zu diesen und so konfigurieren sie [in diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
