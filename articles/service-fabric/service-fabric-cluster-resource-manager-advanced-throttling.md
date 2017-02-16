<properties
   pageTitle="Im Dienst Fabric Cluster Ressourcenmanager begrenzungsebene | Microsoft Azure"
   description="Sie lernen, wie Drosselungen bereitgestellten vom Dienst Fabric Cluster Ressource-Manager konfigurieren."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Das Verhalten der Dienst Fabric Cluster Ressourcenmanager begrenzungsebene
Auch wenn Sie die Ressourcenmanager Cluster ordnungsgemäß konfiguriert haben, kann der Cluster gestört abrufen. Hierfür kann beispielsweise Gleichzeitiges Knoten oder Fehlerstrukturanalyse Domäne Fehler bei der – was passiert, wenn ein, die bei einer Aktualisierung aufgetreten ist? Ressourcenmanager wird am besten versuchen, um alles zu beheben, aber in Zeiten wie folgt möchten Sie möglicherweise eine Abfangfunktion berücksichtigen, damit der Cluster selbst eine Möglichkeit konstant hat (die Knoten aus, die zu gelangen, führen Sie wieder vertraut sind, die netzwerkbedingungen selbst zu lösen, korrigierte Bits abrufen bereitgestellt). Um mit der folgenden Arten von Situationen, der Dienst Fabric Cluster Ressourcenmanager mehrere Drosselungen einschließen. Beachten Sie, dass diese Drosselungen ziemlich Unterbrechung sind und im Allgemeinen sollten nicht verwendet werden, wenn es wurde einige sorgfältige Mathematik fertig, um den Umfang der parallele Arbeit, die in der Cluster sowie eine häufig verwendete tatsächlich ausgeführt werden, kann diese beantworten müssen sortiert der (vielen) ungeplanten Phänomene neu konfiguriert Ereignisse (Alias: "Sehr schlechten Tage").

Im Allgemeinen empfehlen wir sehr schlecht Tage durch die anderen Optionen (wie normale Code Updates und zur Vermeidung von Cluster zunächst overscheduling) zu vermeiden, anstatt begrenzungsebene Ihren Cluster, um zu verhindern, dass Sie Ressourcen verwenden, wenn sie versucht, sich selbst zu beheben). Drosselungen habe Standardwerte, die wir durch Erfahrung ok Standardeinstellungen werden gefunden haben, aber Sie wahrscheinlich sehen und zu einer erwarteten tatsächlichen Belastung zu optimieren. Während der nicht zu beschränken oder laden, die der Cluster empfohlen, die Sie möglicherweise feststellen wird, ob die Fälle (bis Sie diese beheben können) dauert, in denen Sie müssen ein paar Drosselungen eingerichtet haben, auch wenn dies bedeutet, dass den Cluster konstant länger.

##<a name="configuring-the-throttles"></a>Konfigurieren der Drosselungen
Die standardmäßig enthaltenen Drosselungen sind:

-   GlobalMovementThrottleThreshold – steuert dies die Gesamtzahl der abgestimmt im Cluster über einige Zeit (definiert als die GlobalMovementThrottleCountingInterval, Wert in Sekunden)
-   MovementPerPartitionThrottleThreshold – dies die Gesamtzahl der abgestimmt für alle Servicepartition Zeitverlauf einige Steuerelemente (die MovementPerPartitionThrottleCountingInterval, Wert in Sekunden)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Achten Sie darauf, dass in den meisten Fällen Kunden diese Drosselungen verwenden, die er wurde, erwähnt, da sie bereits in einer Umgebung für die Ressource eingeschränkt wurden (z. B. begrenzte Bandbreite in einzelne Knoten oder Festplatten, der auf den Anforderungen parallele Replikation waren nicht erstellt, die diesen platziert wird wurden) die darauf, dass solche Vorgänge an würde nicht erfolgreich ist oder wäre trotzdem langsam.  In diesen Fällen wurden Kunden bequeme kennen, die sie die Zeitdauer potenziell erweitern wurden, die Servers dauert Cluster zum Erreichen eines unveränderliche Zustands, einschließlich kennen, die sie am unteren Zuverlässigkeit für insgesamt ausgeführt werden, während sie gedrosselt wurden einhandeln konnte.

## <a name="next-steps"></a>Nächste Schritte
- Um herauszufinden, darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster, schauen Sie sich im Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md)
- Ressourcenmanager Cluster verfügt über zahlreiche Optionen für die Beschreibung des Clusters. Um herauszufinden Auschecken Weitere Informationen zu diesen Artikels zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
