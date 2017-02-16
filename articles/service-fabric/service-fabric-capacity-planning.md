<properties
   pageTitle="Planen der Dienst Fabric apps Kapazität | Microsoft Azure"
   description="Beschreibt, wie die Anzahl der Datenverarbeitungsknoten erforderlich für eine Fabric Service-Anwendung zu identifizieren"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Planen von Applications Dienst Fabric Kapazität


Dieses Dokument erläutert, wie Sie den Umfang des Ressourcen (CPUs, RAM, Festplattenspeicher) schätzen Sie zur Ausführung der Anwendung Azure Service Fabric müssen. Es ist üblich für Ihren Anforderungen Ressource über einen Zeitraum ändern. Normalerweise müssen Sie einige Ressourcen des Diensts entwickeln/testen, und klicken Sie dann weitere Ressourcen erfordern, wie Sie in Betrieb wechseln und dann und Ihrer Anwendung Beliebtheit. Beim Entwerfen der Anwendungs, durch die langfristigen Anforderungen vorstellen Sie, und nehmen Sie Optionen, mit denen Ihren Dienst für Kunden hohe Nachfrage zu skalieren.

 Wenn Sie einen Dienst Fabric Cluster erstellen, entscheiden Sie, was Cluster Arten von virtuellen Computern (virtuelle Computer) zusammensetzt. Jeder virtueller Computer verfügt über eine begrenzte Menge von Ressourcen in Form von CPUs (Kerne und Geschwindigkeit), Netzwerk-Bandbreite, RAM und Festplattenspeicher. Während des Diensts über einen Zeitraum immer umfangreicher wird, können Sie auf virtuellen Computern aktualisieren, die größere Ressourcen bieten und/oder weitere virtuellen Computern zum Cluster hinzufügen. Um letztere durchzuführen, müssen Sie Ihrem Dienst Anfangs entwerfen, damit sie neue virtuellen Computern nutzen kann, die mit dem Cluster dynamisch hinzugefügt werden.

Einige Dienste zu wenig keine Daten auf den virtuellen Computern selbst verwalten. Daher sollten Kapazität, Planung für diese Dienste hauptsächlich auf die Leistung, konzentrieren, d. h., markieren die entsprechenden CPUs (Kerne und Geschwindigkeit) der virtuellen Computern. Darüber hinaus sollten Sie die Bandbreite, einschließlich Netzwerk übertragen wie häufig auftretende und wie viele Daten übertragen werden. Wenn Sie der Dienst muss auch als Service-Verwendung erhöht ausführen, können Sie weitere virtuellen Computern mit dem Saldo Cluster und Laden hinzufügen im Netzwerk über alle virtuellen Computern anfordert.

Planen der Kapazität sollten hauptsächlich auf Größe für Dienste, die große Mengen von Daten auf den virtuellen Computern verwalten, konzentrieren. Daher sollten Sie die Kapazität des virtuellen Computers RAM und Festplattenspeicher sorgfältig. Die virtuellen Speicher in Windows nimmt Speicherplatz RAM Anwendung Code aussehen. Darüber hinaus stellt die Laufzeit Dienst Fabric smart Seitennavigation planmäßigen nur wichtiges Daten im Speicher und die kalten Daten auf einem Datenträger verschieben. Applikationen können daher mehr Speicher als des virtuellen Computers physisch verfügbar ist. Haben Sie mehr Arbeitsspeicher erhöht einfach Leistung, da der virtuellen Computer mehr Speicherplatz im RAM überwachen kann. Der virtuellen Computer, die Sie auswählen sollten einen Datenträger ausreicht, um die gewünschten Daten des virtuellen Computers gespeichert haben. Der virtuellen Computer sollte ähnlich ausreichend RAM für die Leistung beschleunigen gewünschte haben. Wenn des Diensts Daten über einen Zeitraum vergrößert wird, können Sie weitere virtuelle Computer auf den Cluster und Partition Daten über alle virtuellen Computern hinzufügen.

## <a name="determine-how-many-nodes-you-need"></a>Bestimmen Sie, wie viele benötigten Knoten

Partitionierung des Diensts, können Sie Daten des Diensts zu skalieren. Weitere Informationen zu Partitionierung finden Sie unter [Partitionierung Dienst Fabric](service-fabric-concepts-partitioning.md). Jede Partition innerhalb eines einzelnen virtuellen Computers anpassen muss, aber mehrere (kleine) Partitionen eines einzelnen virtuellen Computers platziert werden können. Ja, eine weitere kleinen Partitionen Probleme größere Flexibilität als ein paar größere Partitionen Probleme. Das Verhältnis ist, gibt es viele Partitionen Dienst Fabric Aufwand erhöht, und Sie keine durchgeführte Vorgänge, partitionsübergreifend ausführen. Es gibt auch weitere mögliche Netzwerkverkehr, wenn Ihr Service-Code häufig Datenelementen zugreifen, die in anderen Partitionen live muss. Beim Entwerfen von Ihrem Dienst, sollten Sie sorgfältig diese vor- und Nachteile zu einer effektiven Strategie des vorherigen zu gelangen.

Angenommen, die Anwendung einen einzelnen dynamische Dienst verfügt, der eine Store festgelegt, die Sie nicht Vorhaben ist, wächst auf DB_Size GB pro Jahr. Sie bereit sind, fügen Sie weitere Applikationen (und Partitionen) wie Variation neben diesem Jahr auftreten.  Der Replikation Faktor (RF), der die Anzahl der Replikate des Diensts bestimmt wirkt sich auf die gesamte DB_Size. Die Summe DB_Size in allen Replikaten ist der Replikation Faktor DB_Size multipliziert.  Node_Size stellt den Datenträger Speicherplatz/RAM pro Knoten, die Sie für den Dienst verwenden möchten. Zur Optimierung der Systemleistung die DB_Size sollte in den Speicher passt, über den Cluster, und eine Node_Size, die um das RAM für den virtuellen Computer ist ausgewählt werden soll. Durch das Zuordnen eines Node_Size, der größer als die RAM-Kapazität ist, werden Sie auf die von der Dienst Fabric Runtime bereitgestellten Seitennavigation verlassen. Auf diese Weise Ihre Veranstaltung möglicherweise nicht optimale, wenn Ihre gesamten Daten als wichtiges gilt (seit dann die Daten ist seitenweise in/out). Für viele Dienste, in dem nur ein Bruch Daten wichtiges ist, ist es jedoch kostengünstiger.

Die Anzahl der maximalen Leistung erforderlich Knoten kann wie folgt berechnet werden:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Konto für Wachstum

Sie möchten die Anzahl der Knoten basierend auf den DB_Size, die Sie Ihrem Dienst, sowie die DB_Size wachsen, die Sie mit begonnen haben erwarten zu berechnen. Klicken Sie dann wächst die Anzahl der Knoten während des Diensts immer umfangreicher wird, damit Sie nicht die Anzahl der Knoten zu viel bereitgestellt werden. Aber die Anzahl der Partitionen sollte die Anzahl der Knoten, die erforderlich sind, wenn Sie den Dienst am maximale geometrischen ausführen basieren.

Es ist sinnvoll, einige zusätzliche Autos jederzeit verfügbar sein, damit Sie alle unerwartete Spitzen oder Rechtsmitteln behandeln können (z. B., wenn ein paar virtuellen Computern nach unten wechseln).  Während die zusätzliche Kapazität mithilfe der erwarteten Spitzen bestimmt werden soll, wird diese als Ausgangsbasis ein paar zusätzliche virtuellen Computern reservieren (5 bis 10 Prozent zusätzliche).

Das vorangehende wird davon ausgegangen einen einzelnen dynamische Dienst aus. Wenn Sie mehr als eine dynamische Dienst haben, müssen Sie die DB_Size zugeordnet der anderen Diensten in der Formel hinzufügen. Alternativ können Sie die Anzahl der Knoten separat für jeden Dienst dynamische berechnen.  Der Dienst möglicherweise Replikate oder Partitionen, die Lastenausgleich nicht zur Verfügung. Beachten Sie, dass Partitionen auch weitere Daten als andere eventuell beibehalten. Weitere Informationen zu Partitionierung finden Sie unter [Partitionierung Artikel über bewährte Methoden](service-fabric-concepts-partitioning.md). Die Gleichung oben ist jedoch Partition und Kopie unabhängig, da Fabric Service wird sichergestellt, dass die Replikate in optimierter Weise zwischen den Knoten ausdehnen sind.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Verwenden Sie für die Berechnung eines Arbeitsblatts

Jetzt sehen wir uns einige reeller Zahlen in der Formel. Ein [Beispiel Tabelle](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) zeigt, wie zum Planen der Kapazität für eine Anwendung, die drei Typen von Datenobjekten enthält. Für jedes Objekt ungefähre wir ihre Größe und wie viele Objekte, dass wir erwartet haben. Wählen Sie auch wie viele Replikate von jedem Objekttyp sollen. Die Kalkulationstabelle berechnet die Gesamtmenge des Arbeitsspeichers im Cluster gespeichert werden.

Wir geben Sie dann einen virtueller Speicher und die monatlichen Kosten. Je nach Größe virtueller Computer, wird die Kalkulationstabelle die minimale Anzahl von Partitionen, die Sie verwenden, um Ihre Daten physisch anpassen auf den Knoten zu teilen. Sie möglicherweise eine größere Anzahl von Partitionen, die bestimmten Berechnung Ihrer Anwendung wünschen und Netzwerkverkehr muss. Die Tabelle zeigt die Anzahl der Partitionen, die die Benutzer Profilobjekte verwalten sollen sechs erhöht hat.

Basierend auf alle diese Informationen, wird nun die Kalkulationstabelle, dass Sie alle Daten mit den gewünschten Partitionen und Replikate physisch in einem Cluster 26-Knoten zugreifen können. Jedoch würde diese Cluster dicht verpackt sein sollten Sie einige zusätzlichen Knoten ausgefallener Knoten und Upgrades gerecht werden kann. Die Tabelle wird aufgezeigt, haben Sie mehr als 57 Knoten keinen zusätzlichen Schutz bietet, da Sie leere Knoten müssten. In diesem Fall soll trotzdem über 57 Knoten wechseln Sie um zu ausgefallener Knoten und Upgrades aufnehmen zu können. Sie können das Arbeitsblatt entsprechend Ihrer Anwendung Bedürfnisse gibt.   

![Tabelle für die Kostenberechnung][Image1]



## <a name="next-steps"></a>Nächste Schritte

Auschecken [Partitionierung Dienst Fabric Services] [ 10] so erfahren mehr über das Aufteilen von Ihrem Diensts.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
