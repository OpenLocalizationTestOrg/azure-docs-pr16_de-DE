<properties
   pageTitle="Checkliste Stabilität | Microsoft Azure"
   description="Checkliste, die Anleitungen für Stabilität Bedenken während des Entwurfs enthält."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure Stabilität Anleitungen: Stabilität Checkliste

Entwurf Ihrer Anwendung für Stabilität erfordert Planung und Minimieren von einer Vielzahl von Fehlermodi, die auftreten können. Überprüfen Sie die Elemente in dieser Prüfliste gegen den Entwurf der Anwendung, schlechter wird.

## <a name="requirements"></a>Anforderungen

- **Definieren des Kunden Verfügbarkeit Anforderungen an.** Ihr Kunde muss Verfügbarkeit Anforderungen für die Komponenten in Ihrer Anwendung, und dies wirkt sich auf Entwurf Ihrer Anwendung. Abrufen von Vertrag aus Ihren Kunden für die Verfügbarkeitsziele des jeweiligen Textabschnitts Ihrer Anwendung, andernfalls der Entwurf möglicherweise nicht gerecht der vom Kunden. Weitere Informationen finden Sie im Abschnitt [Ihren Anforderungen Stabilität definieren](guidance-resiliency-overview.md#defining-your-resiliency-requirements) , der das Dokument [für Azure robuste Anwendungen entwerfen](guidance-resiliency-overview.md) .

## <a name="failure-mode-analysis"></a>Fehler bei der Modus Analyse

- **Führen Sie eine Fehler Modus Analyse (FMA) für die Anwendung.** FMA umfasst zum Erstellen der Stabilität in eine Anwendung frühzeitig in der Entwurfsphase. Die Ziele von einer FMA umfassen:  

    - Bestimmen Sie, welche Arten von Fehlern Anwendung auftreten kann. 
    - Erfassen Sie potenzielle Effekte und Einfluss der einzelnen Fehlertyp über die Anwendung.
    - Strategien für die Wiederherstellung zu identifizieren. 

    Weitere Informationen finden Sie unter [für Azure robuste Anwendungen entwerfen: Fehler Modus Analyse][fma].  

## <a name="application"></a>Anwendung

- **Bereitstellen Sie mehrerer Instanzen von Services an.** Dienste zwangsläufig fehl, und wenn die Anwendung auf einer einzelnen Instanz von einem Dienst abhängig ist zwangsläufig schlägt auch. Um mehrere Instanzen für [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md)bereitstellen möchten, wählen Sie eine [App Dienst planen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) , die mehrere Instanzen bietet ein. Konfigurieren Sie für Azure Cloud Services jeder Ihrer Rollen verwenden [mehrerer Instanzen](../cloud-services/cloud-services-choose-me.md#scaling-and-management)aus. Für [Azure-virtuellen Computern (virtuelle Computer)](../virtual-machines/virtual-machines-windows-about.md), stellen Sie sicher, dass Ihre virtuellen Computer Architektur virtueller mehr als einem Computer enthält, und eine [Verfügbarkeit festlegen]jeder virtueller Computer zu bieten hat[availability-sets].   

- **Verwenden Sie ein Lastenausgleich, um Besprechungsanfragen verteilen.** Ein Lastenausgleich verteilt Ihrer Anwendung Anfragen zu fehlerfrei Dienstinstanzen durch Drehung fehlerhafte Instanzen entfernen. Wenn Ihr Dienst Azure-App-Verwaltungsdienst oder Azure-Cloud-Diensten verwendet, ist es bereits Lastenausgleich für Sie ein. Wenn die Anwendung Azure-virtuellen Computern verwendet, müssen Sie ein Lastenausgleich bereitstellen. Übersicht über die [Azure Lastenausgleich](../load-balancer/load-balancer-overview.md) Weitere Informationen hierzu finden Sie unter. 

- **Konfigurieren von Azure Anwendungsgateways, um mehrere Instanzen verwenden.** Je nach Ihrer Anwendung Anforderungen ein [Azure Application Gateway](../application-gateway/application-gateway-introduction.md) besser geeignet zum Verteilen von Besprechungsanfragen in Ihrer Anwendung Services möglicherweise. Jedoch sind einzelne Instanzen des Diensts Application Gateway keine Vereinbarung zum Servicelevel gewährleistet, sodass es möglich ist, dass die Anwendung nicht könnten, wenn die Anwendungsgateway-Instanz fehlschlägt. Bereitstellung von mehr als einem Medium oder größere Application Gateway Instanz, um die Verfügbarkeit des Diensts im Rahmen der [Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)sichergestellt ist.

- **Use Verfügbarkeit Sets für jede Anwendungsebene**. Platzieren Ihre Instanzen in einer [Verfügbarkeit festlegen] [ availability-sets] garantiert Konnektivität mit mindestens eine Instanz der virtueller Computer im Sinne der [Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/). Wenn in Ihrer virtuellen Computern sind keine Sammlung von Verfügbarkeit, die Sie besitzen garantiert schützen können, und ist es möglich, dass er alle fehlschlagen können oder gleichzeitig aktualisiert werden. 

- **Erwägen Sie die Bereitstellung Ihrer Anwendungs über mehrere Bereiche.** Wenn eine Anwendung auf eine einzelne Region bereitgestellt wird, der gesamte Bereich in das seltene Ereignis ist nicht mehr verfügbar, Ihrer Anwendung auch nicht verfügbar ist. Dies kann im Rahmen Ihrer Anwendung Vereinbarung zum SERVICELEVEL inakzeptabel sein. Ist dies der Fall ist, sollten Sie die Bereitstellung von Ihrer Anwendung und deren Diensten über mehrere Bereiche. Eine Bereitstellung mit mehreren Region können eine aktive (verteilen Anfragen über mehrere aktive Instanzen) oder Muster einer Aktiv-Passiv (eine Instanz "warme" wird in reservieren, aktualisieren, für den Fall, dass Sie die erste Instanz fehlschlägt). Es empfiehlt sich, dass Sie mehrere Instanzen von Ihrer Anwendung Services über regionale-Paare bereitstellen. Weitere Informationen finden Sie unter [Business Continuity- und Disaster Wiederherstellung (BCDR): Azure gepaart Regionen](../best-practices-availability-paired-regions.md).

- **Gegebenenfalls Stabilität Muster für remote-Vorgänge zu implementieren.** Wenn der Kommunikation zwischen remote Services eine Anwendung abhängt, tritt der Kommunikationspfad zwangsläufig. Treten mehrere Fehler, könnte die verbleibenden fehlerfreien Instanzen von Ihrer Anwendung Services mit Anforderungen überfordert werden. Es gibt mehrere Muster nützlich für den Umgang mit häufig auftretenden Fehler, einschließlich des Timeout Musters, die [Muster wiederholen][retry-pattern], die [Sicherung] [ circuit-breaker] Muster und andere. Weitere Informationen finden Sie unter [für Azure robuste Anwendungen entwerfen](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Verwenden Sie automatische Skalierung reagieren auf Erhöhung laden.** Wenn die Anwendung nicht zum Zweck einer Skalierung automatisch zunehmender Auslastung konfiguriert ist, ist es möglich, die Dienste Ihrer Anwendung nicht funktioniert, wenn er mit benutzeranforderungen ausgelastet werden. Weitere Informationen hierzu finden Sie unter:

    - Allgemeine: [Skalierbarkeit Checkliste](../best-practices-scalability-checklist.md) 
    - App-Verwaltungsdienst Azure: [Anzahl der Instanzen manuell oder automatisch zu skalieren][app-service-autoscale]
    - Cloud Services: [So automatisch skalieren Cloud-Dienst][cloud-service-autoscale]
    - Virtuellen Computern: [Automatische Skalierung und virtuellen Computern Maßstab legt fest][vmss-autoscale]


- **Implementieren der asynchrone Operationen, wann immer möglich.** Synchroner Vorgänge können Ressourcen des und andere Vorgänge blockieren, während der Anrufer wartet, bis der Vorgang abgeschlossen ist. Entwerfen Sie jeden Teil Ihrer Anwendung dürfen für asynchrone Vorgänge, wann immer möglich. Weitere Informationen zum Implementieren asynchrone Programmierung in c# finden Sie unter [asynchrone Programmierung mit asynchronen und wartet auf][asynchronous-c-sharp].

- **Verwenden Sie den Azure Datenverkehr-Manager für Ihre Anwendung Datenverkehr an verschiedener Regionen.**  [Azure Datenverkehr Manager] [ traffic-manager] führt Ebene der DNS-Einträge für den Lastenausgleich und können Datenverkehr auf die verschiedenen Bereiche basierend auf dem [Datenverkehrs-routing] [ traffic-manager-routing] Methode, die Sie angeben und die Integrität des Ihrer Anwendung Endpunkte. 

- **Konfigurieren Sie und Testen Sie der Systemzustand führt eine Überprüfung auf Ihre Lastenausgleich und den Datenverkehr-Manager.** Stellen Sie sicher, dass Ihre Gesundheit Logik der kritische Teile des Systems überprüft und ordnungsgemäß auf Systemzustand Stichproben reagiert.

    - Die Integrität ist, sucht nach [Azure Datenverkehr Manager] [ traffic-manager] und [Azure Lastenausgleich] [ load-balancer] eine bestimmte Funktion dienen. Für den Datenverkehr Manager der Dienststatus Prüfpunkt bestimmt, ob in eine andere Region über fehl. Für ein Lastenausgleich bestimmt, ob Sie die Drehung ein virtuellen Computers aufheben.      

    - Für einen Prüfpunkt Datenverkehr Manager sollte Ihre Gesundheit Endpunkt prüfen Abhängigkeiten kritischen, die innerhalb der gleichen Region bereitgestellt werden und, deren Ausfall einen Failover auf eine andere Region auslösen soll.  

    - Für ein Lastenausgleich sollten der Dienststatus Endpunkt die Integrität des den virtuellen Computer melden. Setzen Sie nicht andere Ebenen oder externe Dienste. Andernfalls wird ein Fehler, der außerhalb der virtuellen Computer Lastenausgleich So entfernen Sie den virtuellen Computer aus Drehung führen.

    - Leitfaden zum Dienststatus in Ihrer Anwendung Überwachung implementieren finden Sie unter [Dienststatus Endpunkt Überwachung Muster](https://msdn.microsoft.com/library/dn589789.aspx).

- **Überwachen von Drittanbietern Services an.** Wenn eine Anwendung von Drittanbietern Services abhängig ist, legen fest, wo und wie diese Dienste von Drittanbietern können ein Fehler auftreten, und was diesen Fehlern Effekt auf Ihrer Anwendung haben. Ein Drittanbieter-Dienst möglicherweise nicht gehören, Überwachung und Diagnose, damit es ist wichtig, melden Sie sich Ihre Aufrufe hiervon und zu koordinieren sie mit der Anwendung Gesundheit und Diagnoseprotokoll mit einem eindeutigen Bezeichner. Weitere Informationen über bewährte Methoden für die Überwachung und Diagnose finden Sie unter die [Überwachung und Diagnose Anleitungen] [ monitoring-and-diagnostics-guidance] Dokument.

- **Stellen Sie sicher, dass alle Drittanbieter-Dienst, den Sie nutzen eine Vereinbarung zum SERVICELEVEL bereitstellt.** Wenn eine Anwendung abhängig von einem Drittanbieter-Dienst, aber Drittanbieter keine Garantie für die Verfügbarkeit in Form einer Vereinbarung zum SERVICELEVEL bietet, kann auch die Verfügbarkeit der Anwendung garantiert werden. Der Vereinbarung zum SERVICELEVEL ist nur so gut wie die kleinsten verfügbaren Komponente der Anwendung.


## <a name="data-management"></a>Datenverwaltung

- **Verstehen der Replikationsmethoden für Datenquellen Ihrer Anwendung.** Die Anwendungsdaten werden in unterschiedlichen Datenquellen gespeichert und anderen Verfügbarkeit Anforderungen haben. Die Replikationsmethoden für jede Art von Datenspeicher in Azure auswerten, einschließlich [Azure Speicherreplikation](../storage/storage-redundancy.md) und [SQL-aktive Geo Datenbankreplikation](../sql-database/sql-database-geo-replication-overview.md) , um sicherzustellen, dass Ihrer Anwendung Daten Anforderungen erfüllt sind.

- **Stellen Sie sicher, dass keine einzelnen Benutzerkontos auf sowohl Herstellung und Sichern von Daten zugreifen.** Ihre Daten Sicherungskopien gefährdet sind, wenn ein einzelnes Benutzerkonto Schreibberechtigung sowohl Herstellung und zusätzliche Quellen besitzt. Ein bösartiger Benutzer konnte absichtlich alle Ihre Daten löschen können, während ein normale Benutzer versehentlich gelöscht werden konnte. Entwerfen der Anwendungs, um die Berechtigungen eines einzelnen Benutzerkontos einschränken, sodass nur die Benutzer, die Schreibzugriff erfordern Schreibzugriff und es nur für die Herstellung oder sichern, aber nicht beide ist.

- **Dokument über fehlschlagen und wieder nicht bestanden Ihrer Datenquelle zu verarbeiten und zu testen.** Ein Mensch Operator müssen in der Groß-/Kleinschreibung, falls Ihre Datenquelle schlägt fehl, führen Sie eine Reihe von dokumentierten Anweisungen über fehlschlägt, um eine neue Datenquelle. Wenn die dokumentierten Schritte Fehler haben, wird ein Operator werden nicht erfolgreich folgen, und ein Fehler auftreten, über die Ressource. Testen Sie regelmäßig die Anweisung Schritte, um sicherzustellen, dass ein Operator folgen sie kann erfolgreich über fehlschlagen und fehlschlagen wieder der Datenquelle.

- **Überprüfen Sie Ihre Daten Sicherungskopien ein.** Regelmäßig stellen Sie sicher, dass Ihre Sicherung Daten erwartungsgemäß, indem ein Skript zum Überprüfen der Datenintegrität, Schema und Abfragen. Es ist auch keine Sicherungskopie, wenn es nicht zum Wiederherstellen der Datenquellen sinnvoll ist. Melden Sie sich an, und melden Sie alle Inkonsistenzen benachrichtigt, damit der Sicherung Dienst repariert werden kann.

- **Erwägen Sie eine Speicher Kontotyp, die Geo redundant ist.** Daten in einem Konto Azure Storage gespeichert werden immer lokal repliziert. Es gibt jedoch mehrere Replikationsstrategien zur Verfügung, um ein Konto Speicher bereitgestellt wird. Wählen Sie [Azure-Lesezugriff Geo redundante Speicherung (RAS-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) zum Schutz Ihrer Anwendungsdaten anhand der seltene aus, wenn eine ganze Region nicht mehr verfügbar ist.

    > [AZURE.NOTE] Für virtuelle Computer verlassen Sie sich nicht auf RAS-GRS Replikation zum Wiederherstellen der Datenträger virtueller Computer (virtuelle Festplatte Dateien). Verwenden Sie stattdessen die [Sicherung Azure][azure-backup].   

## <a name="operations"></a>Vorgänge

- **Überwachen und warnen bewährte Methoden in Ihrer Anwendung zu implementieren.** Ohne ordnungsgemäße Überwachung, Diagnose und warnen gibt es Möglichkeit keine zum Ermitteln von Fehlern in Ihrer Anwendung und benachrichtigen einen Operator, um sie zu reparieren. Weitere Informationen über bewährte Methoden für die [Überwachung und Diagnose Anleitungen] finden Sie unter[ monitoring-and-diagnostics-guidance] Dokument.

- **Messen Sie remote Call Statistiken und stellen Sie die Informationen für das Anwendungsteam zur Verfügung zu.**  Wenn Sie nicht nachverfolgen und liefern remote Call Statistiken in Echtzeit und eine einfache Möglichkeit bieten, diese Informationen zu überprüfen, wird das Team Vorgänge eine sofortige Ansicht in den Integritätsstatus Ihrer Anwendung keinen. Probleme in den Diensten, und Sie nur Measure Mittelwert remote Call Zeit, Sie nicht genügend Informationen bereitzustellen haben werden. Zusammenfassen Sie remote Call Kennzahlen wie Wartezeit, Durchsatz und Fehler in die Quantile zwischen 99 und 95. Ausführen von statistischen Analysen auf die Metrik zu Fehlern, die innerhalb jeder Quantil auftreten für eine Steigerung.

- **Verfolgen Sie die Anzahl der vorübergehenden Ausnahmen und Wiederholungsversuche über einem angemessenen Zeitrahmen aus.** Wenn Sie nicht nachverfolgen und vorübergehende Ausnahmen überwachen und Wiederholungsversuche über einen Zeitraum, ist es möglich, dass ein Problem oder Fehler nach Ihrer Anwendung "Wiederholen" Logik ausgeblendet werden konnte. D. h., wenn der für die Überwachung und Protokollierung nur zeigt zur erfolgreichen oder keines Vorgangs, der Fakultät, der Vorgang war aufgrund von Ausnahmen mehrmals wiederholt werden ausgeblendet. Ein Trend zunehmender Ausnahmen über einen Zeitraum gibt an, dass der Dienst ist, Probleme auftreten, und kann ein Fehler auftreten. Weitere Informationen finden Sie unter [Hinweise zu bestimmten Diensten wiederholen][retry-service-guidance].

- **Implementieren Sie ein Frühwarnsystem, die einen Operator benachrichtigt.** Benennen Sie die Key Performance Indikatoren für des Systemzustands der Anwendung, z. B. vorübergehende Ausnahmen und remote Wartezeit anrufen, und legen geeignete Schwellenwerte für jede von ihnen ein. Senden Sie eine Benachrichtigung zu Vorgängen, wenn der Schwellenwert erreicht wird. Legen Sie diese Schwellenwerte auf Ebenen, die Probleme erkennen, bevor sie kritisch und Wiederherstellung Antwort erforderlich.

- **Den Releaseprozess für eine Anwendung zu dokumentieren.** Ohne ausführliche Release Prozessdokumentation möglicherweise Operator eine fehlerhafte Aktualisierung bereitstellen oder Konfigurieren von Einstellungen für eine Anwendung nicht ordnungsgemäß. Klar definieren und Release Prozesses Dokument, und stellen Sie sicher, dass es das gesamte Vorgänge Team zur Verfügung steht. Bewährte Methoden für die Bereitstellung Ihrer Anwendung robuste sind in der [robuste Bereitstellung] detaillierte[ guidance-resilient-deployment] Abschnitt des Dokuments Stabilität Anleitungen.

- **Stellen Sie sicher, dass mehr als eine Person im Team angewiesen ist, überwachen Sie die Anwendung, und führen Sie eine manuelle Wiederherstellung Schritte aus.** Wenn Sie nur ein einzigen Operators Teammitglieder haben, können Sie die Anwendung überwachen und deaktivieren vor der Wiederherstellung Starten eines, wird diese Person einen einzelnen Punkt des Fehlers. Schulen von mehreren Personen auf Erkennung und Wiederherstellung und stellen Sie sicher, es gibt immer mindestens eine aktive zu einem beliebigen Zeitpunkt.

- **Automatisieren Sie Ihrer Anwendung Bereitstellungsprozess an.** Wenn Ihre Mitarbeiter zum manuellen Bereitstellen von Ihrer Anwendung erforderlich ist, kann personenbezogenen Fehler die Bereitstellung zum Fehlschlagen verursachen. Weitere Informationen über bewährte Methoden für die Bereitstellung der Anwendung Automatisierung finden Sie unter der [Bereitstellung robuste] [ guidance-resilient-deployment] Abschnitt des Dokuments Stabilität Anleitungen.

- **Ihre Version Entwurfsprozesses Anwendung Verfügbarkeit maximiert.** Wenn Release Prozesses während der Bereitstellung Offlinemodus-Dienste erforderlich sind, werden Ihrer Anwendung nicht verfügbar, bis sie wieder online sind. Verwenden Sie die [Blau-Grün](http://martinfowler.com/bliki/BlueGreenDeployment.html) oder [lassen Sie wieder los Kanarischen](http://martinfowler.com/bliki/CanaryRelease.html) Bereitstellung-Technik zur Bereitstellung Ihrer Anwendungs auf Herstellung ein. Beide Verfahren umfassen Bereitstellen von Ihrer Versionscode entlang der Herstellung Code, damit Benutzer Release-Code zum Herstellung Code im Fall eines Fehlers umgeleitet werden können. Weitere Informationen finden Sie unter der [Bereitstellung robuste] [ guidance-resilient-deployment] Abschnitt des Dokuments Stabilität Anleitungen.

- **Melden Sie sich und Überwachen Ihrer Anwendung Bereitstellungen.** Wenn Sie verwenden bereitgestellt Bereitstellung Techniken wie Blau-grün oder Kanarischen Versionen, die mehr als eine Version der Anwendung in Betrieb ausgeführt werden. Wenn ein Problem durchgeführt werden soll, ist es wichtig, um zu bestimmen, welche Version der Anwendung ein Problem verursacht. Implementieren einer robuste Protokollierung Strategie für viele Version-spezifische Informationen wie möglich zu erfassen. 

- **Stellen Sie sicher, dass eine Anwendung gegenüber den [beschränkt Azure-Abonnement](../azure-subscription-service-limits.md)nicht ausgeführt wird.** Bestimmte Ressourcentypen, z. B. Anzahl der Ressourcengruppen, Anzahl der Kerne und Anzahl der Speicherkonten ist Azure-Abonnements eingeschränkt.  Wenn Ihren Anforderungen Anwendung Azure-Abonnement Grenzwerte überschreiten, erstellen Sie ein anderes Azure-Abonnement und Bereitstellen von ausreichende Ressourcen vorhanden.

- **Stellen Sie sicher, dass eine Anwendung gegenüber den [beschränkt - Service](../azure-subscription-service-limits.md)nicht ausgeführt wird.** Einzelne Azure Dienste haben Verbrauch Grenzwerte &mdash; beispielsweise auf Speicher, Durchsatz, Anzahl von Verbindungen, Abfragen pro Sekunde und anderer Größen begrenzt. Die Anwendung schlägt fehl, wenn versucht wird, verwenden Ressourcen diese Grenzwerte überschreitet. Dies führt zu Fehlern Dienst Drosselung und mögliche gesamten betroffenen Benutzer. 

    Je nach den bestimmten Dienst und Ihren Anforderungen Anwendung, können Sie häufig diese Grenzwerte vermeiden, indem Sie Skalierung nach oben (z. B. durch Auswählen einer anderen Preisgestaltung Ebene) oder Skalierung (Hinzufügen von neuen Instanzen).  

- **Entwerfen Sie Ihrer Anwendung Speicher Anforderungen zu Azure-Speicher Skalierbarkeit und Leistung Ziele fallen.** Azure-Speicher soll die Funktion in den vordefinierten Skalierbarkeit und Leistung Zielen, also Entwerfen Ihrer Anwendung zu Speicher innerhalb dieser Ziele nutzen. Wenn Sie diese Ziele überschreiten treten Ihrer Anwendung Speicher begrenzungsebene. Um dieses Problem zu beheben, Bereitstellung von zusätzlichen Speicher-Konten. Wenn Sie gegenüber den Grenzwert für Speicherplatz Konto ausführen, Bereitstellung von zusätzliche Azure-Abonnements, und klicken Sie dann Bereitstellung von zusätzlichen Speicher-Konten vorhanden. Weitere Informationen finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](../storage/storage-scalability-targets.md).

- **Wählen Sie die richtige Größe der virtuellen Computer für die Anwendung.** Messen der tatsächlichen CPU, Arbeitsspeicher, Datenträger und e/a-Ihrer virtuellen Computer in der Herstellung, und stellen Sie sicher, dass die virtuellen Computer-Größe, die Sie ausgewählt haben ausreicht. Andernfalls kann Ihrer Anwendung Kapazitätsprobleme auftreten, wie die virtuellen Computern ihre Grenzen nähern. Virtueller Computer Größen werden im Dokument [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-windows-sizes.md) ausführlich beschrieben.

- **Feststellen Sie, ob der Anwendung Arbeitsbelastung unveränderliche oder veränderlichen über einen Zeitraum ist.** Wenn Ihre Arbeitsbelastung über einen Zeitraum wechselt, legt verwenden Azure-virtuellen Computer Skala mit dem Maßstab automatisch die Anzahl der Instanzen virtueller Computer. Andernfalls müssen Sie manuell vergrößern oder verkleinern die Anzahl der virtuellen Computern. Weitere Informationen finden Sie unter der [Übersicht des virtuellen Computern Umfang Sätze](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Wählen Sie die richtige Service-Ebene für Azure SQL-Datenbank.** Wenn die Anwendung Azure SQL-Datenbank verwendet wird, stellen Sie sicher, dass Sie die entsprechenden Dienstebene ausgewählt haben. Wenn Sie eine Ebene, die nicht verarbeiten Ihrer Anwendung Datenbank Transaktion Einheit (DTU) Anforderungen ist auswählen, werden Ihre Daten verwenden gedrosselt. Weitere Informationen zum Auswählen der richtigen Serviceplan finden Sie unter der [SQL-Datenbank-Optionen und Leistung: zu verstehen, was in jeder Kategorie Dienst verfügbar ist](../sql-database/sql-database-service-tiers.md) Dokument. 

- **Haben Sie einen zurücksetzen Plan für die Bereitstellung.** Es ist möglich, die Bereitstellung Ihrer Anwendung konnte fehl, und führen eine Anwendung nicht mehr verfügbar sein. Entwerfen eines Prozesses zurücksetzen, kehren Sie zum letzten bekannten guten Versionsverlaufs und Ausfallzeiten zu minimieren. Die [robuste Bereitstellung] finden Sie unter[ guidance-resilient-deployment] Abschnitt des Dokuments Stabilität Anleitungen für Weitere Informationen. 

- **Erstellen eines Prozesses für die Interaktion mit Azure Support.** Wenn der Prozess für den Kontakt zu [Azure unterstützen](https://azure.microsoft.com/support/plans/) nicht festgelegt ist, bevor Bedarf an den Support wenden, werden Ausfallzeiten verlängert werden als der Supportprozess zum ersten Mal navigiert wird. Schließen Sie den Prozess für Kontaktieren des Supports und eskalieren von Problemen als Teil Ihrer Anwendung Stabilität von Anfang an.

- **Stellen Sie sicher, dass die Anwendung nicht mehr als die maximale Anzahl von Speicherkonten pro Abonnement verwendet.** Azure ermöglicht maximal 200 Speicherkonten pro Abonnement. Wenn die Anwendung mehr Speicherkonten als aktuell im Rahmen Ihres Abonnements zur Verfügung stehen erfordert, müssen Sie ein neues Abonnement erstellen und es zusätzlichen Speicherkonten erstellen. Weitere Informationen finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md#storage-limits).

- **Stellen Sie sicher, dass die Anwendung der Skalierbarkeit Ziele für virtuellen Computern Datenträger nicht überschritten wird.** Eine Neuerung Azure unterstützt eine Anzahl von Datenlaufwerke je nach verschiedene Faktoren, einschließlich virtueller Speicher und Speicher Kontotyp anfügen. Überschreitet Ihrer Anwendung der Skalierbarkeit Ziele für virtuellen Computern Datenträger, Bereitstellung von zusätzlichen Speicher-Konten und Erstellen von virtuellen Computern Datenträger vorhanden. Weitere Informationen finden Sie unter [Azure-Speicher Skalierbarkeit und Leistungsziele] (... / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Führen Sie die Failover- und Failbackrichtlinien für eine Anwendung testen.** Wenn Sie Failover und Failback vollständig getestet nicht geschehen ist, können nicht Sie sicherstellen, dass die abhängigen Dienste in Ihrer Anwendung wieder in einer synchronisierten Weise während der Wiederherstellung im Zusammenhang. Sicherstellen Sie, dass der Anwendung abhängige Failover und wieder in der richtigen Reihenfolge Fail services.

- **Führen Sie Fehlerinjektion für eine Anwendung testen.** Die Anwendung kann verschiedene Ursachen haben, wie die Gültigkeitsdauer des Zertifikats, Erschöpfung der Systemressourcen eines virtuellen Computers oder Speicher Fehlern fehl. Testen Sie die Anwendung in einer Umgebung so nah wie möglich um Herstellung, durch Simulieren oder reale Fehlern auslösen. Beispielsweise Zertifikate löschen, künstlich Systemressourcen oder Löschen einer Speicherquelle. Überprüfen Sie Ihrer Anwendung Möglichkeit zum Wiederherstellen von aus allen Arten von Fehlern durch, und zwar alleine kombinierter an. Überprüfen Sie, Fehlern nicht weitergegeben oder Ihrem System cascading.

- **Führen Sie Tests Herstellung beide synthetischen und real Benutzerdaten verwenden.** Testen und Fertigung unterscheiden sich selten, damit es wichtig ist, Blau-grün oder eine Kanarischen-Bereitstellung verwenden, und Testen Sie die Anwendung in Betrieb. So können Sie testen Sie die Anwendung in Herstellung real Auslastung, und stellen Sie sicher, dass es funktionieren wie erwartet, wenn vollständig bereitgestellt.

## <a name="security"></a>Sicherheit

- **Implementieren Sie Anwendung Ebene Schutz vor verteilten DOS Angriffen Service (DDoS).** Azure Services sind vor DDos-Angriffen am Ebene Netzwerks geschützt. Jedoch kann nicht Azure vor Anwendung-Layer-Angriffen schützen, da es schwierig ist WAHR Benutzeranfragen bösartiger benutzeranforderungen unterscheiden. Weitere Informationen zum Schutz vor Anwendung-Layer DDoS-Angriffen finden Sie im Abschnitt "Schützen von gegen DDoS" [Microsoft Azure Network Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (PDF-Download).

- **Das Prinzip der minimalen Rechte für den Zugriff auf die Ressourcen der Anwendung zu implementieren.** Die Standardeinstellung für den Zugriff auf die Ressourcen der Anwendung sollte als eingeschränkt wie möglich sein. Erteilen von höheren Ebene Berechtigungen auf Basis Genehmigung. Erteilen von zu Berechtigungen Zugriff auf die Ressourcen der Anwendung standardmäßig kann in einer anderen Person absichtlich oder versehentlich löschen von Ressourcen zu führen. Azure ermöglicht die [Steuerung des Benutzerzugriffs rollenbasierte](../active-directory/role-based-access-built-in-roles.md) zum Verwalten von Benutzerberechtigungen, aber es ist wichtig, der minimalen Rechte Berechtigungen für andere Ressourcen überprüfen, ihre eigenen Berechtigungen Systeme, wie etwa SQL Server haben. 

## <a name="telemetry"></a>Werden

- **Melden Sie werden Daten, während Sie die Anwendung in dieser Umgebung ausgeführt wird.** Erfassen Sie robuste werden Informationen, während die Anwendung in dieser Umgebung ausgeführt oder Sie werden keine ausreichende Informationen, um die Ursache von Problemen beim aktiv von Benutzern erstellen zu diagnostizieren. Weitere Informationen finden Sie in der Protokollierung bewährte Methoden steht in die [Überwachung und Diagnose Anleitungen] [ monitoring-and-diagnostics-guidance] Dokument.

- **Implementieren Sie Protokollierung ein asynchrones Muster verwenden.** Wenn Protokollierung Vorgänge synchron sind, können sie Ihrer Anwendungscode blockieren. Stellen Sie sicher, dass Ihre Protokollierung Vorgänge als asynchrone Vorgänge implementiert werden. 

- **Zu koordinieren Log Daten über Service hinweg.** Fordern Sie ein möglicherweise mehrere Dienst Begrenzung durchlaufen, in einer normalen mehrstufige Anwendung. Angenommen, ist fordern Sie ein stammen in der Regel in der Webebene übergeben an die Schicht Business und schließlich in der Datenebene beibehalten. In komplexere Szenarien möglicherweise eine Anforderung Benutzer an viele andere Dienste und Datenspeicher verteilt werden. Stellen Sie sicher, dass es sich bei Ihrem Protokollierungssystem Anrufe über Dienst hinweg entspricht, damit Sie in Ihrer Anwendung die Anfrage verfolgen können.

##  <a name="azure-resources"></a>Azure-Ressourcen 

- **Verwenden Sie Azure Ressourcenmanager Vorlagen zum von Bereitstellungsressourcen an.** Ressourcenmanager Vorlagen erleichtern Bereitstellungen über PowerShell oder der CLI Azure, was zu einem Zuverlässigkeit Bereitstellungsprozess führt automatisieren. Weitere Informationen finden Sie unter [Übersicht Azure Ressourcenmanager][resource-manager].

- **Geben Sie Ressourcen aussagekräftige Namen zu.** Zugewiesen Ressourcen aussagekräftige Namen vereinfacht das eine bestimmte Ressource suchen und seiner Rolle zu verstehen. Weitere Informationen finden Sie unter [Benennungskonventionen für Azure Ressourcen empfohlen](guidance-naming-conventions.md) 

- **Verwenden Sie Access rollenbasierte Steuerelement (RBAC)**. Verwenden Sie den Zugriff auf den Azure Ressourcen steuern, die Sie bereitstellen RBAC. RBAC können Sie Mitgliedern Ihres Teams DevOps um unbeabsichtigtes löschen oder Änderungen an bereitgestellten Ressourcen verhindern Autorisierungsrollen zuweisen. Weitere Informationen finden Sie unter [Erste Schritte mit Access Management Azure-Portal](../active-directory/role-based-access-control-what-is.md) 

- **Verwenden Sie für wichtige Ressourcen, wie z. B. virtuellen Computern Sperren für Ressourcen ein.** Ressource Sperren verhindern, dass einen Operator versehentlich löschen einer Ressource. Weitere Informationen finden Sie unter [Sperrenressourcen Azure Ressourcenmanager](../resource-group-lock-resources.md) 

- **Landes-/ paarweise angegeben werden.** Wenn auf beiden Regionen bereitstellen, wählen Sie aus dem gleichen Landes-/ Paar Regionen aus. Bei einer allgemeinen Ausfall Wiederherstellung, der eine Region aus jeder Paar erhält. Einige Dienste, wie z. B. Geo redundante Speicherung bieten in den Bereich für die automatische Replikation. Weitere Informationen finden Sie unter [Business Continuity- und Disaster Wiederherstellung (BCDR): Azure gepaart Regionen](../best-practices-availability-paired-regions.md) 

- **Gruppe zum Organisieren von Ressourcen durch (Funktion) und Lebenszyklus**.  Im Allgemeinen sollten eine Ressourcengruppe Ressourcen enthalten, die den gleichen Lebenszyklus freigeben. Dies vereinfacht das Bereitstellungen verwalten, Test-Bereitstellungen löschen und Zuweisen von Zugriffsrechten, verringern die Wahrscheinlichkeit, dass eine Bereitstellung Herstellung versehentlich gelöscht oder geändert werden. Erstellen Sie separate Ressourcengruppen für die Entwicklung, Fertigung, und Testen Sie Umgebungen. Setzen Sie in einer Bereitstellung mit mehreren Region Ressourcen für die einzelnen Regionen in separaten Ressourcengruppen ein. Dies vereinfacht eine Region erneut bereitstellen, ohne die anderen Region. 

## <a name="azure-services"></a>Azure Services 

Die folgenden Checkliste Elemente gelten für bestimmte Dienste in Azure.

###  <a name="app-service"></a>App-Dienst 

- **Verwenden Sie Standard oder Premium Ebene an.** Diese Ebenen staging Steckplätze unterstützen und automatische Sicherungskopien. Weitere Informationen finden Sie unter [Azure-App-Verwaltungsdienst-Pläne genaue (Übersicht)](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Vermeiden Sie dieselbe Skalierung nach oben oder unten.** Wählen Sie stattdessen eine Ebene und die Instanzgröße, die Ihren Anforderungen Leistung unter typische laden, und klicken Sie dann [Skalierung](../app-service-web/web-sites-scale.md) entsprechen der Instanzen, Änderungen in den Datenverkehr Lautstärke behandeln. Skalierung möglicherweise einen Neustart der Anwendung nach oben oder unten auslösen.  

- **Konfiguration als app-Einstellungen speichern.** Verwenden Sie Einstellungen für die app, um die Konfiguration Einstellungen als app-Einstellungen zu halten. Definieren Sie die Einstellungen in Ihre Vorlagen Ressourcenmanager oder mithilfe der PowerShell, damit Sie können sie als Teil einer automatisierten Bereitstellung anwenden / Aktualisierungsvorgang mehr zuverlässig ist. Weitere Informationen finden Sie unter [Konfigurieren von Web apps in Azure-App-Dienst](../app-service-web/web-sites-configure.md). 

- **Erstellen Sie separate App Dienstpläne für Herstellung und testen.** Verwenden Sie nicht Steckplätze für die Herstellung-Bereitstellung zum Testen.  Alle apps innerhalb der gleichen App Serviceplan freigeben dieselben Instanzen virtueller Computer an. Wenn Sie in der gleichen Plan Herstellung und Test-Bereitstellungen eingefügt haben, kann es die Herstellung Bereitstellung negativ auswirken. Beispielsweise können beim Laden überprüft die Herstellung live-Website beeinträchtigen. Test-Bereitstellungen in einem separaten Plan, einfügen, isolieren Sie sie aus der Herstellung Version.  

- **Separate Web apps aus dem Web APIs**. Wenn Ihre Lösung sowohl ein Front-End-Web und ein Web-API, erwägen Sie das Zerlegen diese in separaten App-Service-apps. Dieser Entwurf vereinfacht die Lösung durch Arbeitsbelastung gliedern. Sie können die Web app und die API in separaten App-Service-Pläne ausführen, sodass sie unabhängig voneinander skaliert werden können. Wenn Sie die Ebene der Skalierbarkeit am benötigen zuerst, können Sie der apps in demselben Plan bereitstellen, und verschieben sie in separaten Plänen später bei Bedarf.

- **Vermeiden Sie das App-Dienst zusätzliche Feature SQL Azure-Datenbanken sichern.** Verwenden Sie stattdessen [SQL-Datenbank automatische Sicherungskopien][sql-backup]. App-Dienst Sicherung exportiert die Datenbank in einer SQL-.bacpac-Datei, die DTUs Kosten.  

- **Bereitstellen Sie auf einer staging Slot.** Erstellen Sie einen Slot Bereitstellung für Staging. Bereitstellen von Anwendungsupdates für das staging Slot, und überprüfen Sie die Bereitstellung, bevor Sie es in Betrieb austauschen. Dadurch wird die Wahrscheinlichkeit, dass eine fehlerhafte Aktualisierung in Herstellung verringert. Außerdem wird sichergestellt, dass alle Instanzen vor, die in Betrieb vertauscht Standardserverhardware sind. Viele Clientanwendungen verfügen über eine signifikante Aufwärmdauer und Haut-Startzeit. Weitere Informationen finden Sie unter [Einrichten von staging-Umgebungen von Web apps in Azure-App-Dienst](../app-service-web/web-sites-staged-publishing.md). 

-  **Erstellen Sie einen Slot Bereitstellung, um die Bereitstellung des letzten funktionierenden (LKG) halten.** Wenn Sie ein Update für die Herstellung bereitstellen, verschieben Sie die vorherige Herstellung Bereitstellung in den Slot LKG. Dies erleichtert die Anwendung eine schlechten Bereitstellung zurückzusetzen. Wenn Sie ein Problem später feststellen, können Sie schnell die LKG Version wiederherstellen. Weitere Informationen finden Sie unter [Azure Bezug Architektur: grundlegende Webanwendung](guidance-web-apps-basic.md). 

- **Aktivieren Sie das Diagnoseprotokoll**, einschließlich der Anwendung Protokollierung und Web-Server-Protokollierung. Protokollierung ist wichtig für die Überwachung und Diagnose. [Aktivieren Sie das Diagnoseprotokoll für Web apps in Azure-App-Dienst](../app-service-web/web-sites-enable-diagnostic-log.md) finden Sie unter

- **Log zu BLOB-Speicher**. Dies vereinfacht das Sammeln und Analysieren von Daten. 

- **Erstellen einer separaten Speicherkonto für Protokolle.** Verwenden Sie nicht das gleiche Speicherkonto für Protokolle und Anwendungsdaten. Dies hindert Protokollierung von Leistung der Anwendung verringern. 

- **Überwachen der Leistung.** Verwenden Sie einen Dienst wie [Neue Relic](http://newrelic.com/) oder [Anwendung Einsichten](../application-insights/app-insights-overview.md) Monitor Application Leistung und Auslastung Verhalten zum Überwachen der Leistung.  Überwachen der Leistung bietet Ihnen in Echtzeit einen Einblick in die Anwendung. Sie können Sie Probleme diagnostizieren Ursache Datenanalysen und der Fehler. 


###  <a name="application-gateway"></a>Application Gateway 

- **Bereitstellung von mindestens zwei Instanzen.** Bereitstellen von Application Gateway mit mindestens zwei Instanzen. Eine einzelne Instanz ist eine einzelne Punkt des Fehlers. Verwenden von zwei oder mehr Instanzen für Redundanz und Skalierbarkeit. Um die [Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)nutzen zu können, müssen Sie mindestens zwei Instanzen von Medium oder größere bereitstellen. 

### <a name="azure-search"></a>Azure suchen

- **Bereitstellung von mehr als eine Replikation.** Verwenden Sie mindestens zwei Replikate für hoher Verfügbarkeit gelesen oder drei Lese-und Schreibzugriff hohe Verfügbarkeit.

- **Konfigurieren von Indexern für mehrere Region Bereitstellungen.** Wenn Sie eine Bereitstellung mit mehreren Region haben, sollten Sie Ihre Optionen für Continuity in Indizierung.

    - Ist die Datenquelle Geo repliziert, zeigen Sie jedes Indizierungsprogramm jeder regionalen Azure Suchdiensts auf deren Quellreplikat lokaler Daten ein.  

    - Wenn die Datenquelle nicht Geo repliziert ist, zeigen Sie mehrere Indexer in der gleichen Datenquelle, sodass die Suche Azure Services in mehreren Regionen kontinuierlich und unabhängig aus der Datenquelle indizieren. Weitere Informationen finden Sie unter [Suchen Azure Leistung und Optimierung Aspekte][search-optimization].

###  <a name="azure-storage"></a>Azure-Speicher 

- **Verwenden Sie für die Anwendungsdaten Lesezugriff Geo redundante Speicher (RAS-GRS) ein.** RAS-GRS Speicher repliziert die Daten in einer sekundären Region und stellt schreibgeschützten Zugriff aus der sekundäre Region. Wenn in der primären Region ein Ausfall Speicher vorhanden ist, können die Daten aus der sekundäre Region lesen. Weitere Informationen finden Sie unter [Replikation Azure-Speicher](../storage/storage-redundancy.md).

- **Verwenden Sie für virtuellen Computer Datenträger, Premium Speicher** Weitere Informationen finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../storage/storage-premium-storage.md).

- **Erstellen Sie für Warteschlange-Speicher eine Sicherungskopie Warteschlange in eine andere Region ein.** Für Warteschlange-Speicher weist eine schreibgeschützt verwenden, eingeschränkt, da Sie nicht in die Warteschlange oder Elemente aus der Warteschlange entfernt. Erstellen Sie stattdessen eine Sicherungskopie Warteschlange in einem Speicherkonto in einer anderen Region ein. Ist ein Ausfall Speicher, kann die Anwendung die Sicherung Warteschlange verwenden, bis die primäre Region wieder verfügbar ist. Auf diese Weise kann die Anwendung weiterhin neue Anfragen bearbeitet werden.  

###  <a name="documentdb"></a>DocumentDB 

- **Reproduzieren Sie die Datenbank über hinweg Regionen.** Mit einem Konto mit mehreren Region weist die Datenbank DocumentDB eine schreiben Region und mehrere gelesen Regionen. Wenn ein Fehler in der Region schreiben vorliegt, können Sie von einem anderen Replikation lesen. Client-SDK übernimmt dies automatisch aus. Sie können auch über den Bereich Schreiben in eine andere Region fehl. Weitere Informationen finden Sie unter [Verteilen von Daten mit DocumentDB Global](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>SQL-Datenbank 

- **Verwenden Sie Standard oder Premium Ebene an.** Diese Ebenen bieten einen Zeitraum mehr Point-in-Time wiederherstellen (35 Tage). Weitere Informationen finden Sie unter [Optionen für SQL-Datenbank und Leistung](../sql-database/sql-database-service-tiers.md).

- **Aktivieren Sie die Überwachung der SQL-Datenbank.** Überwachung kann die Diagnose Angriffen oder personenbezogenen Fehler verwendet werden. Weitere Informationen finden Sie unter [Erste Schritte mit SQL-Datenbank Überwachung](../sql-database/sql-database-auditing-get-started.md). 

- **Verwenden der Active Geo-Replikation** Verwenden Sie aktiven Geo-Replikation, um einem lesbaren sekundären in einem anderen Bereich zu erstellen.  Wenn die primäre Datenbank fehlschlägt, oder einfach muss offline sein, führen Sie ein manuelles Failover auf die sekundäre Datenbank.  Bis Sie über ein Fehler auftreten, bleibt die sekundäre Datenbank schreibgeschützt.  Weitere Informationen finden Sie unter [SQL-aktive Geo Datenbankreplikation](../sql-database/sql-database-geo-replication-overview.md). 

- **Sharding verwenden**. Erwägen Sie Sharding horizontal die Datenbank aufgeteilt. Sharding kann Fehlerstrukturanalyse Isolation angeben. Weitere Informationen finden Sie unter [Skalierung heraus mit Azure SQL-Datenbank](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Verwenden Sie Point-in-Time wiederherstellen, um personenbezogenen Fehler zu beheben.**  Point-in-Time wiederherstellen gibt Ihrer Datenbank zu einem früheren Zeitpunkt. Weitere Informationen finden Sie unter [Wiederherstellen eine automatische Datenbanksicherungskopien mit Azure SQL-Datenbank][sql-restore].

- **Verwenden Sie zum Wiederherstellen eines aus einem Dienstausfall Geo-wiederherstellen.** Geo-wiederherstellen wiederhergestellt eine Datenbank aus einer Sicherung Geo redundante.  Weitere Informationen finden Sie unter [Wiederherstellen eine automatische Datenbanksicherungskopien mit Azure SQL-Datenbank][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (auf einem virtuellen Computer ausgeführt wird)

- **Die Datenbank repliziert werden.** Verwenden Sie SQL Server immer auf Verfügbarkeit Gruppen, um die Datenbank repliziert. Hohe Verfügbarkeit bereitgestellt, wenn eine SQL Server-Instanz fehlschlägt. Weitere Informationen finden Sie unter [Weitere Informationen...](guidance-compute-n-tier-vm.md) 

- **Sichern der Datenbank**. Wenn Sie bereits [Azure Sicherung](https://azure.microsoft.com/documentation/services/backup/) Sichern Ihrer virtuellen Computer verwenden, sollten erwägen Sie, [Azure Sicherung für SQL Server-Auslastung mit DPM](../backup/backup-azure-backup-sql.md)zu verwenden. Bei diesem Ansatz ist es eine Sicherung Administratorrolle für die Organisation und einer einheitlichen Wiederherstellungsvorgangs für virtuellen Computern und SQL Server. Verwenden Sie andernfalls [SQL Server verwaltete Sicherung in Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)ein. 


###  <a name="traffic-manager"></a>Datenverkehr-Manager 

- **Durchführen Sie manuelle Failback.** Nach einem Failover Datenverkehr Manager manuelle Failback durchführen, anstatt weiß nicht automatisch zurück. Bevor wieder fehlschlägt, stellen Sie sicher, dass alle Anwendung Teilsystemen fehlerfrei sind.  Andernfalls können Sie eine Situation erstellen, in dem die Anwendung vorwärts und rückwärts zwischen Data Center spiegelt. Weitere Informationen finden Sie unter [Ausführen von virtuellen Computern in mehreren Bereichen](guidance-compute-multiple-datacenters.md).

- **Erstellen einer Gesundheit Prüfpunkt Endpunkt**. Erstellen Sie einen benutzerdefinierten Endpunkt, der für den allgemeinen Zustand der Anwendung meldet. Dadurch Datenverkehr-Manager über auftreten, wenn alle Kritischer Weg fehlschlägt, nicht nur die front-End. Der Endpunkt sollte einen HTTP-Fehlercode zurück, wenn alle wichtigen Abhängigkeit fehlerhaften oder nicht erreichbar ist. Melden Sie Fehler in nicht kritischen Dienste, jedoch nicht. Andernfalls möglicherweise der Dienststatus Prüfpunkt Failover ausgelöst, wenn er nicht erforderlich ist "false Positives" erstellen. Weitere Informationen finden Sie unter [den Datenverkehr Manager Endpunkt Überwachung und Failover](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtuellen Computern 

- **Vermeiden Sie einen Arbeitsbelastung Herstellung eines einzelnen virtuellen Computers ausgeführt.** Eine einzelne Bereitstellung ist nicht flexibel in Bezug auf geplanten oder nicht geplanten Wartung. Setzen Sie stattdessen mehrere virtuellen Computern in einer Verfügbarkeit festlegen oder virtueller Computer skalieren zurück, die mit einem Lastenausgleich im Vordergrund. Damit der Vereinbarung zum SERVICELEVEL nutzen zu können, muss mindestens zwei virtuellen Computern innerhalb der gleichen Verfügbarkeit bereitgestellt werden. Weitere Informationen finden Sie unter [Übersicht des virtuellen Computern Umfang Sätze](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Geben Sie die Verfügbarkeit festgelegt, wenn Sie den virtuellen Computer bereitstellen.** Derzeit gibt es Möglichkeit keine zum Hinzufügen einer VM Ressourcenmanager zu einer Verfügbarkeit festgelegt, nachdem Sie der virtuellen Computer bereitgestellt wird. Beim Hinzufügen ein neues virtuellen Computers zu einer vorhandenen Verfügbarkeit festlegen, vergewissern Sie sich einen Netzwerkadapter für den virtuellen Computer zu erstellen, und fügen die NIC an den Adresspool Back-End-auf dem Lastenausgleich. Andernfalls wird nicht Lastenausgleich Netzwerkdatenverkehr an diesen virtuellen Computer weiterzuleiten. 

- **Setzen Sie jede Anwendungsebene in einer separaten Verfügbarkeit festzulegen.** In einer Anwendung mehrstufige nicht setzen virtuellen Computern aus unterschiedlichen Ebenen in der gleichen Verfügbarkeit festlegen. Virtuelle Computer in einem Satz Verfügbarkeit über Fehlerstrukturanalyse Domänen (FDs) gesetzt und Aktualisieren von Domänen (UD). Um die Vorteile der Redundanz FDs und UDs zu gelangen, muss jeder virtuellen Computer Verfügbarkeit festlegen jedoch können die gleichen Clientanfragen zu behandeln sein. 

- **Wählen Sie die richtigen virtueller Speicher Leistung Anforderungen entsprechend aus.** Beim Verschieben von einer bestehenden Arbeitsbelastung in Azure, beginnen Sie mit der virtueller Speicher, die auf Ihre Server lokal am nächsten kommt. Klicken Sie dann messen Sie die Leistung von Ihrer tatsächlichen Arbeitsbelastung in Bezug auf CPU, Arbeitsspeicher und Laufwerks-IOPS, und passen Sie bei Bedarf die Größe. Dadurch wird sichergestellt, dass die Anwendung verhält sich wie erwartet in einen Cloud-Umgebung. Wenn Sie mehrere Netzwerkkarten benötigen, achten Sie zudem die NIC Grenzwert für jede Größe. 

- **Verwenden Sie Premium Speicherung für virtuelle Festplatten.** Azure Premium-Speicher unterstützt leistungsfähige und niedrig Wartezeiten Datenträger. Weitere Informationen finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../storage/storage-premium-storage.md) wählen Sie die Größe virtueller Computer, der Premium-Speicher unterstützt. 

- **Erstellen einer separaten Speicher-Konto für jeden virtuellen Computer an.** Platzieren Sie die virtuellen Festplatten für einen virtuellen Computer in einem separaten Speicherkonto an. Auf diese Weise können, um zu vermeiden, drücken die IOPS Grenzwerte für Speicherkonten. Weitere Informationen finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](../storage/storage-scalability-targets.md). Wenn Sie eine große Anzahl von virtuellen Computern bereitstellen, Bedenken Sie jedoch von der Beschränkung pro Abonnement für Speicherkonten. Anzeigen [von Speichergrenzwerten](../azure-subscription-service-limits.md#storage-limits).

- **Erstellen einer separaten Speicher-Konto für Diagnoseprotokolle**. Schreiben Sie Diagnoseprotokolle nicht mit dem Speicherkonto als die virtuellen Festplatten zu verhindern, dass der diagnoseprotokollierung wirkt die IOPS für die virtuellen Computer Datenträger aus. Ein Standardkonto Lokales redundante Speicher (LRS) ist für Diagnoseprotokolle ausreichend. 

- **Installieren von Applications auf einen Datenträger, nicht auf dem Datenträger OS.** Andernfalls können Sie das Größenlimit Datenträger erreicht haben. 

- **Verwenden Sie Azure Sicherung virtuellen Computern sichern.** Sicherungskopien Schutz vor Datenverlust unbeabsichtigtes. Weitere Informationen finden Sie unter [Schützen des Azure virtuellen Computern mit einer Wiederherstellung Services Tresor](../backup/backup-azure-vms-first-look-arm.md).

- **Aktivieren von Diagnoseprotokollen**, einschließlich grundlegende Gesundheit Kennzahlen, Infrastrukturprotokolle und [Boot Diagnose][boot-diagnostics]. Boot Diagnose hilft Ihnen ein Boot-Fehler diagnostizieren, wenn Ihre virtuellen Computer in einem nicht startfähige Zustand erhält. Weitere Informationen finden Sie unter [Übersicht der Azure Diagnoseprotokolle][diagnostics-logs].

- **Verwenden Sie die Erweiterung AzureLogCollector**. (Windows virtuellen Computern nur.) Diese Erweiterung aggregiert Azure-Plattform Protokolle und lädt diese in Azure-Speicher, ohne den Operator Remote-Anmeldung in den virtuellen Computer hoch. Weitere Informationen finden Sie unter [AzureLogCollector Erweiterung](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Virtuelles Netzwerk 

- **Weißen oder blockieren hinzufügen öffentliche IP-Adressen einer NSG mit dem Subnetz.** Blockieren Sie bösartiger Benutzer Zugriff oder zulassen Sie des Zugriffs nur von Benutzern, die Berechtigung zum Zugriff auf die Anwendung.  

- **Erstellen Sie einen benutzerdefinierten Gesundheit Prüfpunkt.** Laden Lastenausgleich Gesundheit untersucht können entweder HTTP oder TCP testen. Wenn Sie ein virtuellen HTTP-Server ausgeführt wird, ist der HTTP-Prüfpunkt ein besserer Indikator des Integritätsstatus als einen Prüfpunkt TCP aus. Verwenden Sie für eine HTTP-Prüfpunkt einen benutzerdefinierten Endpunkt, der den allgemeinen Zustand der Anwendung, einschließlich aller wichtige Abhängigkeiten Berichte aus. Weitere Informationen finden Sie unter [Übersicht Azure Lastenausgleich](../load-balancer/load-balancer-overview.md).

- **Blockieren Sie nicht durch den Dienststatus Prüfpunkt.** Der Prüfpunkt laden Lastenausgleich Gesundheit wird von einer bekannten IP-Adresse 168.63.129.16 gesendet. Blockieren Sie nicht Datenverkehr in den oder aus diese IP-Adresse in einem beliebigen Firewall-Richtlinien oder Netzwerk Sicherheit Gruppe (NSG) Regeln. Blockieren des Dienststatus Prüfpunkts würde So entfernen Sie den virtuellen Computer aus Drehung Lastenausgleich. 

- **Aktivieren Sie Lastenausgleich Protokollierung.** Die Protokolle der anzeigen wie viele virtuelle Computer auf die Back-End-Netzwerkverkehr aufgrund von fehlerhaften Prüfpunkt Antworten werden nicht empfangen. Weitere Informationen finden Sie unter [Log Analytics für Lastenausgleich Azure](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
