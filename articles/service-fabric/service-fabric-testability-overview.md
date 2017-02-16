<properties
   pageTitle="Fehlerstrukturanalyse-Analysis Services-Übersicht | Microsoft Azure"
   description="In diesem Artikel werden die Fehlerstrukturanalyse Analysis Services-Dienst Struktur für veranlassen der Fehlern und Testszenarien in Ihrer Dienste ausführen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/06/2016"
   ms.author="rsinha"/>

# <a name="introduction-to-the-fault-analysis-service"></a>Einführung in die Fehlerstrukturanalyse Analysis Services

Fehlerstrukturanalyse Analyse Dienst dient zum Testen von Diensten, die auf Microsoft Azure Service Fabric aufbauen. Mit dem Fehlerstrukturanalyse Analysis-Dienst können Sie aussagekräftige Fehlern auslösen und abgeschlossen Testszenarien für Ihre Programme ausgeführt. Diesen Fehlern und Szenarien Übung, und überprüfen Sie die zahlreichen Zustände und Übergänge, bei denen ein Dienst in der Audiodatei, alle in einer Weise gesteuert, sicherer und konsistente auftreten wird.

Aktionen sind die einzelnen Fehler verwendet einen Dienst zum Testen es. Ein Dienstentwickler kann diese als Bausteine zum Schreiben von komplexen Szenarien verwenden. Beispiel:

  * Starten Sie einen Knoten, um eine beliebige Anzahl von Situationen zu reproduzieren, einen Computer oder virtuellen Computer neu gestartet wird.

  * Verschieben Sie eine Kopie Ihrer dynamische Dienst Lastenausgleich, Failover oder Anwendungsupgrade simulieren.

  * Rufen Sie Quorum Verlust auf eine dynamische Dienst eine Situation erstellen, in dem Schreibvorgänge fortfahren kann, da es keine genügend Replikate "Sichern" oder "sekundäre", um neue Daten zu akzeptieren.

  * Rufen Sie Datenverlust auf eine dynamische Dienst eine Situation erstellen, in dem alle in-Memory-Zustand vollständig bereinigt wird, ein.

Szenarien werden komplexe Vorgänge erstellter eine oder mehrere Aktionen. Fehlerstrukturanalyse-Analysis-Dienst bietet zwei integrierte abgeschlossen Szenarien:

  * Szenario Chaos
  * Failover-Szenario

## <a name="testing-as-a-service"></a>Als Dienst testen

Fehlerstrukturanalyse Analyse Dienst ist ein Dienst Fabric Systemdienst, der mit einem Dienst Fabric Cluster automatisch gestartet wird. Dies ist Dienst fungiert als Host für Fehlerinjektion testen Szenario Ausführung und Gesundheit Analyse aus. 

![Fehlerstrukturanalyse Analysis Services][0]

Wenn ein Fehlerstrukturanalyse Aktion oder Test-Szenario initiiert wird, ist ein Befehls an den Fehlerstrukturanalyse Analysis-Dienst gesendet, das Szenario Fehlerstrukturanalyse, Aktion oder Test ausgeführt wird. Fehlerstrukturanalyse Analyse Dienst ist dynamische, sodass es zuverlässigen kann Fehlern und Szenarien ausführen, und überprüfen Sie die Ergebnisse. Beispielsweise kann ein langer Testszenario vom Dienst Analyse Fehlerstrukturanalyse zuverlässig ausgeführt werden. Und da Tests innerhalb der Cluster ausgeführten befinden, kann der Dienst den Status der Cluster und Ihrer Dienste zu bieten weitere ausführliche Informationen zu Fehlern untersuchen.

## <a name="testing-distributed-systems"></a>Verteilte Systeme testen

Dienst Fabric erleichtert schreiben und Verwalten von verteilten skalierbare Applikationen erheblich. Fehlerstrukturanalyse Analyse Dienst vereinfacht das Testen einer verteilten Anwendungs auf ähnliche Weise einfacher. Es gibt drei wichtigsten Probleme, die beim Testen gelöst werden müssen:

1. Simulieren/Generieren von Fehlern, die in realen Szenarios auftreten können: eine der wichtigsten Aspekte Dienst Stoffbahn liegt in der verteilten Applications aus verschiedenen Fehlern wiederherstellen. Klicken Sie zum Testen, dass die Anwendung nicht aus diesen Fehlern benötigen wir jedoch ein Verfahren simulieren/generiert diese praktisches Fehler in einer Umgebung für gesteuert.

2. Die Möglichkeit, korrelierte Fehler generieren: grundlegende Fehlern im System, wie z. B. Netzwerkfehlern und Computer-Fehlern sind einfach einzeln berechnet werden. Generieren einer größeren Anzahl von Szenarien, die in der Praxis als Ergebnis Interaktionen der diese einzelne Fehler auftreten können, ist wichtige.

3. Unified Erfahrung auf verschiedenen Ebenen Entwicklung und Bereitstellung: Es gibt viele Fehlerstrukturanalyse einfügen Systeme, die verschiedenen Arten von Fehlern ausführen können. Beim Verschieben von Entwicklertools Chi-Box-Szenarien, für die Ausführung von dieselben Tests in großen Test-Umgebungen, bei der Verwendung von für Tests in Herstellung ist mit der alle der folgenden jedoch beeinträchtigt.

Während es viele Verfahren zur Lösung dieser Probleme, eines Systems, mit denen das gleiche mit erforderlichen Garantien – ganz aus einer ein-Feld Entwicklertools-Umgebung gibt, in der Herstellung Zuordnungseinheiten, d.h. Testen unterhält fehlt. Fehlerstrukturanalyse Analyse Dienst hilft Entwickler konzentrieren auf ihre eigene Geschäftslogik testen. Fehlerstrukturanalyse-Analysis-Dienst bietet die Funktionen, die zum Testen der Interaktion mit dem zugrunde liegenden verteilten System des Diensts erforderlich sind.



### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Simulieren zu generieren von praktisches Fehlerszenarien

Klicken Sie zum Testen der Stabilität eines verteilten Systems gegen Fehler benötigen wir ein Verfahren zum Generieren von Fehlern. In der Theorie startet Generieren eines Fehlers wie ein Knoten unten einfach, scheint sie demselben Satz von Konsistenzprobleme, die Dienst Fabric zu lösen versucht drücken. Wenn wir einen Knoten, beenden möchten, ist der entsprechende Workflow beispielhaft Folgendes:

1. Stellen Sie im Desktopclient die Anforderung einer war(en) Knoten aus.

2. Senden Sie die Anfrage in den rechten Knoten aus.

    ein. Wenn der Knoten nicht gefunden wird, sollte ein Fehler auf.

    b. Wenn der Knoten gefunden wird, sollten sie nur der Knoten beendet wird zurück.

Zum Überprüfen des Fehlers im Hinblick auf testen, muss der Test wissen, wenn dieser Fehler ausgelöst wird, der Fehler tatsächlich passiert. Die Garantie, die Dienst Fabric enthält, die entweder ist der Knoten gehen nach unten oder wurde bereits ab, wenn der Befehl den Knoten erreicht. In beiden Fällen sollte der Test ordnungsgemäß Grund über den Status und erfolgreich ist oder fehlschlägt ordnungsgemäß in bei der Validierung können können. Ein System implementiert außerhalb Service-Struktur bis demselben Satz von Fehlern führen konnte drücken Sie viele Netzwerk, Hardware und Softwareprobleme, die die vorherigen Garantien bietet verhindern möchten. Wenn die Probleme, die bereits zuvor erwähnt, Fabric Service wird umkonfigurieren im Cluster Zustand, um die Probleme zu beheben, und daher die Fehlerstrukturanalyse Analysis Services können weiterhin rechts Dokumentensatz Garantien.

### <a name="generating-required-events-and-scenarios"></a>Generieren von erforderlichen Ereignisse und Szenarien

Während ein Fehlers praktisches konsistente simulieren nicht einfach zunächst ist, ist die Option zum Generieren korrelierter Fehlern auch komplexere. Beispielsweise passiert Verlust von Daten in eine dynamische dauerhaften Dienst geschieht Folgendes:

1. Nur ein Schreiben Quorum Replikate sind auf dem neuesten Stand auf Replikation. Alle sekundären Replikate positiven hinter dem primären.

2. Das Schreiben Quorum fällt aus, da die Replikate abwärts (aufgrund von Code Paket oder Knoten wechseln nach unten).

3. Das Schreiben Quorum kann nicht wieder im Zusammenhang, da die Daten für die Replikate (aufgrund beschädigte Datenträger oder Erstellen eines neuen Image der Computer) verloren.

Diese korrelierte Fehler führen Sie in der Praxis, aber nicht als häufig als einzelne Fehler auftreten. Die Möglichkeit, die für diese Szenarios testen vornherein produziert ist entscheidend. Noch wichtiger ist die Möglichkeit, diese Szenarios mit Produktionsarbeitslasten in gesteuert Umständen (in der Mitte mit allen Technikern Folienstapel Tag) zu reproduzieren. Das ist viel besser, als wird zum ersten Mal bei Herstellung 2:00 Uhr erfolgen.

### <a name="unified-experience-across-different-environments"></a>Einheitliche Erfahrung in verschiedenen Umgebungen

Die Methode wurde traditionell drei unterschiedliche Optionssätze Erfahrung, eine für die Entwicklungsumgebung, eine für überprüft und eine für die Herstellung zu erstellen. Das Modell wurde:

1. In der Entwicklungsumgebung zu erstellen, mit die Einheit Tests der einzelnen Methoden können Übergänge zwischen aus.

2. Führen Sie in der testumgebung Fehlern zu End-to-End-Tests, die verschiedenen Szenarios Übung aus.

3. Berücksichtigen Sie die Herstellung Umgebung zu verhindern, dass alle Fehler nicht natürlichen und um sicherzustellen, dass es extrem schnelle personenbezogenen Antwort auf Fehler bei der ist ursprünglichen.

Vorgeschlagene wir Dienst Struktur, über den Fehlerstrukturanalyse Analysis-Dienst deaktivieren Sie diese um, und verwenden dieselbe Methode zum Herstellung von Entwicklertools Umgebung. Es gibt zwei Möglichkeiten, dies zu erreichen:

1. Verwenden Sie zum Auslösen von Fehlern gesteuerter Fehlerstrukturanalyse Analysis Dienst APIs aus eine-Box-Umgebung ganz nach der Herstellung Cluster aus.

2. Verwenden Sie Cluster einer Fieber verleihen, die automatische Induktion der Fehler bewirkt, dass Sie die Fehlerstrukturanalyse Analysis Services automatische Fehlern generieren. Steuern der Kostensatz Fehlern durch Konfiguration aktiviert den gleichen Dienst in verschiedenen Umgebungen unterschiedlich getestet werden.

Mit Fabric Service Obwohl die Skalierung der Fehler in den verschiedenen Umgebungen unterschiedlich wäre würde die tatsächlichen Methoden identisch sein. Dies ermöglicht eine viel schneller Code bis zur Bereitstellung Verkaufspipeline und die Möglichkeit, die Dienste unter praktisches lädt testen.

## <a name="using-the-fault-analysis-service"></a>Mithilfe des Diensts für Fehlerstrukturanalyse Analyse

**C#**

Fehlerstrukturanalyse-Analysis Services-Features werden im System.Fabric-Namespace in das Paket Microsoft.ServiceFabric NuGet. Um die Fehlerstrukturanalyse Analysis Services-Features verwenden zu können, fügen Sie das Nuget-Paket als Referenz im Projekt.

**PowerShell**

Wenn PowerShell verwenden möchten, müssen Sie den Dienst Fabric SDK installieren. Nachdem das SDK installiert ist, ist das ServiceFabric PowerShell-Modul automatisch geladen für Ihre Verwendung.

## <a name="next-steps"></a>Nächste Schritte

Um wirklich Cloud-Skala Dienste zu erstellen, ist es entscheidend, beide vor und nach der Bereitstellung sicherstellen, dass Services realen auftretende Fehler verarbeiten können. In der Welt der Dienste heute, ist die Möglichkeit, schnell Innovationen und verschieben Sie Code in Herstellung schnell sehr wichtig. Fehlerstrukturanalyse Analyse Dienst hilft Dienst Entwickler präzise erledigen.

Beginnen Sie testen der Anwendungen und Dienste mithilfe der integrierten [Szenarien testen](service-fabric-testability-scenarios.md)oder erstellen Sie eigener Testszenarien mithilfe der [Fehlerstrukturanalyse-Aktionen](service-fabric-testability-actions.md) , die vom Fehlerstrukturanalyse Analysis-Dienst bereitgestellt.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
