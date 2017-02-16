<properties
   pageTitle="Lebenszyklus Dienst Struktur Anwendung | Microsoft Azure"
   description="Beschreibt Entwicklung, bereitstellen, testen, Upgrade, Verwaltung und Entfernen von Applications Dienst Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Lebenszyklus Fabric Service-Anwendung
Wie mit anderen Plattformen Anwendung auf Azure Service Fabric normalerweise die folgenden Phasen durchläuft: Entwurf, Entwicklung, testen, Bereitstellung, Upgrade, Wartung und entfernen. Fabric-Dienst bietet herausragende Unterstützung für die vollständige Anwendungslebenszyklus Cloud-Anwendung, von der Entwicklung über Bereitstellung, tägliche Verwaltung und Wartung zu tatsächlichen außer Betrieb. Das Service-Modell kann verschiedene Rollen im Lebenszyklus Anwendung unabhängig voneinander Teilnahme an. Dieser Artikel enthält eine Übersicht über die APIs und wie sie durch die anderen Rollen in der gesamten die Phasen des Lebenszyklus Fabric Service-Anwendung verwendet werden.

## <a name="service-model-roles"></a>Rollen für Service-Modell
Die Service-Modell Rollen sind:

- **Dienst Entwicklertools**: entwickelt modular und generische Dienste, die unterschiedliche erneut und in der gleichen Typ oder verschiedene Typen mehrere Clientanwendungen verwendet werden können. Beispielsweise kann ein Warteschlangendienst verwendet werden, für die Erstellung einer zur Anwendung (Helpdesk) oder e-Commerce-Anwendung (Warenkorb).

- **Anwendung Entwicklertools**: erstellt von Applications durch die Integration von eine Sammlung von Diensten, um bestimmte spezifische Voraussetzungen oder Szenarien erfüllen. Beispielsweise eine e-Commerce-Website möglicherweise integrieren, "JSON statusfreien Front-End-Dienst", "Angebot Stateful Service" und "Warteschlange Stateful Service" zum Erstellen einer auctioning Lösung.

- **Anwendungsadministrator**: macht Entscheidungen auf Anwendungskonfiguration (die Konfiguration Vorlagenparameter ausfüllen), Bereitstellung (Zuordnung zu den verfügbaren Ressourcen) und die Qualität des Diensts. Ein Anwendungsadministrator beschließt beispielsweise das Gebietsschema (in Englisch für Deutschland) oder Japanisch für Japan, beispielsweise der Anwendung. Eine andere bereitgestellte Anwendung kann unterschiedliche Einstellungen haben.

- **Operator**: bereitstellt, basierend auf der Anwendungskonfiguration und Anforderungen, die vom Anwendungsadministrator angegeben. Beispielsweise ein Operator Vorschriften stellt die Anwendung bereit und stellt sicher, dass es in Azure ausgeführt wird. Operatoren Anwendungsinformationen Gesundheit und Leistung überwachen und verwalten die physische Infrastruktur, je nach Bedarf.


## <a name="develop"></a>Entwickeln
1. Ein *Entwickler* entwickelt verschiedene Typen von Diensten Modell Programmierung [Zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md) oder [Zuverlässigen Services](service-fabric-reliable-services-introduction.md) verwenden.
2. Ein *Entwickler* werden deklarativ entwickeltes Diensttypen in einer Dienstmanifestdatei ein oder mehrere Code, Konfiguration und Daten Pakete aus.
3. Ein *Anwendungsentwickler* erstellt dann eine Anwendung mit anderen Diensttypen aus.
4. Ein *Anwendungsentwickler* beschreibt deklarativ den Anwendungstyp in einem Anwendungsmanifest verweisen auf den Dienst Manifeste enthaltenen Dienste und ordnungsgemäß überschreiben und andere Einstellungen für Konfiguration und Bereitstellung der einzelnen Dienste parametrisieren.

Beispiele finden Sie unter [Erste Schritte mit zuverlässigen Akteuren](service-fabric-reliable-actors-get-started.md) und [Erste Schritte mit zuverlässigen Services](service-fabric-reliable-services-quick-start.md) .

## <a name="deploy"></a>Bereitstellen
1. Ein *Anwendungsadministrator* führt Anpassung, damit den Anwendungstyp zu einer bestimmten Anwendung zu einem Dienst Fabric Cluster bereitgestellt werden, indem Sie die entsprechenden Parameter des Elements **Anwendungstyp für** im Anwendungsmanifest angeben.

2. Einen *Operator* uploads das Anwendungspaket zum Cluster Image Store mithilfe der [Methode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) oder das [Cmdlet **ServiceFabricApplicationPackage kopieren** ](https://msdn.microsoft.com/library/azure/mt125905.aspx). Das Anwendungspaket enthält das Anwendungsmanifest und die Sammlung von Service-Paketen. Dienst Fabric bereitstellt Applications aus dem Anwendungspaket gespeichert, die im Bild Store, der einen Azure Blob-Speicher oder der Systemdienst Dienst Fabric sein kann.

3. Der *Operator* stellt dann der Anwendung geben Sie den gewünschten Cluster aus dem hochgeladene Anwendungspaket mithilfe der [Methode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), das [Cmdlet **Register-ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)oder die [restlichen **Bereitstellen einer Anwendung** Vorgang](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Nach der Bereitstellung der Anwendungs, startet einen *Operator* die Anwendung mit der *Anwendungsadministrator* mithilfe der [Methode **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), das [Cmdlet " **New-ServiceFabricApplication** "](https://msdn.microsoft.com/library/azure/mt125913.aspx)oder den [REST der **Anwendung erstellen** Vorgang](https://msdn.microsoft.com/library/azure/dn707676.aspx)bereitgestellten Parameter aus.

5. Nachdem die Anwendung bereitgestellt wurde, verwendet einen *Operator* die [ **CreateServiceAsync** Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), das [Cmdlet " **New-ServiceFabricService** "](https://msdn.microsoft.com/library/azure/mt125874.aspx)oder den [ **Dienst erstellen** REST Vorgang](https://msdn.microsoft.com/library/azure/dn707657.aspx) zum Erstellen neuer Dienstinstanzen für die Typen von verfügbaren Service-Anwendung.

6. Die Anwendung wird jetzt im Dienst Fabric Cluster ausgeführt.

Beispiele finden Sie unter [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Test
1. Nach der Bereitstellung von Cluster lokale Entwicklung oder einem Testcluster, wird ein *Dienst Entwicklertools* das integrierten Failover Test-Szenario mithilfe der Klassen [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) und [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) oder das [ **Aufrufen ServiceFabricFailoverTestScenario** Cmdlet](https://msdn.microsoft.com/library/azure/mt644783.aspx)ausgeführt. Das Failover Test-Szenario führt einen angegebenen Dienst bis wichtige Übergänge und Failovers, um sicherzustellen, dass es immer noch aktiv ist.

2. Der *Dienst Entwicklertools* wird dann das integrierten Chaos Testszenario mithilfe der Klassen [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) und [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) oder das [ **Aufrufen ServiceFabricChaosTestScenario** Cmdlet](https://msdn.microsoft.com/library/azure/mt644774.aspx)ausgeführt. Das Chaos Testszenario hervorruft zufällig mehrere Knoten, Code Paket und Fehlern durch eine Replikat zum Cluster.

3. Der *Dienst Entwicklertools* [Tests Dienst - Kommunikation](service-fabric-testability-scenarios-service-communication.md) von authoring Testszenarien, die primäre Replikate im Cluster verschieben.

Weitere Informationen finden Sie unter [Einführung in die Fehlerstrukturanalyse Analysis Services](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Upgrade
1. Ein *Entwickler* so aktualisiert, dass die einzelnen Dienste der Anwendung instanziierten und/oder behebt Fehler und enthält eine neue Version des Servicemanifests.

2. Ein *Anwendungsentwickler* überschreibt die Einstellungen für Konfiguration und Bereitstellung der Dienste konsistent parametrisiert und enthält eine neue Version des Anwendungsmanifests. Entwickler der Anwendung anschließend übernimmt die neuen Versionen der Dienst Manifeste in die Anwendung und eine neue Version von den Anwendungstyp in einer aktualisierten Anwendungspaket.

3. Ein *Anwendungsadministrator* übernimmt die neue Version des Anwendungstyps in der Zielanwendung durch Aktualisieren der entsprechenden Parameter ein.

5. Einen *Operator* uploads das Anwendungspaket aktualisierte im Cluster Bild Store mithilfe der [Methode **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) oder das [Cmdlet **ServiceFabricApplicationPackage kopieren** ](https://msdn.microsoft.com/library/azure/mt125905.aspx). Das Anwendungspaket enthält das Anwendungsmanifest und die Sammlung von Service-Paketen.

6. Einen *Operator* stellt die neue Version der Anwendung im Zielcluster mithilfe der [Methode **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), das [Cmdlet **Register-ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)oder die [restlichen **Bereitstellen einer Anwendung** Vorgang](https://msdn.microsoft.com/library/azure/dn707672.aspx)bereit.

7. Einen *Operator* upgrades die Zielanwendung auf die neue Version mithilfe der [Methode **UpgradeApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), mit dem [ **Start-ServiceFabricApplicationUpgrade** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125975.aspx)oder die [restlichen **Aktualisieren einer Anwendung** Vorgang](https://msdn.microsoft.com/library/azure/dn707633.aspx)an.

8. Einen *Operator* überprüft den Fortschritt eines Upgrades mithilfe der [Methode **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), das [Cmdlet " **Get-ServiceFabricApplicationUpgrade** "](https://msdn.microsoft.com/library/azure/mt125988.aspx)oder den [REST der **Anwendung Aktualisieren des Vorgangsfortschritts** Vorgang](https://msdn.microsoft.com/library/azure/dn707631.aspx)an.

9. Falls notwendig, den *Operator* ändert, und wendet die Parameter mithilfe der [Methode **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), das [Cmdlet **Update-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt126030.aspx)oder die [restlichen **Update Anwendung ein Upgrade** Vorgang](https://msdn.microsoft.com/library/azure/mt628489.aspx)Aktualisierung der aktuellen Anwendung erneut.

10. Falls erforderlich, setzt den *Operator* zurück die Aktualisierung der aktuellen Anwendung mithilfe der [ **RollbackApplicationUpgradeAsync** Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), mit dem [ **Start-ServiceFabricApplicationRollback** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125833.aspx)oder die [restlichen **Zurücksetzen Anwendung Upgrade** -Vorgang](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Dienst Fabric upgrades die Zielanwendung im Cluster ausgeführt werden, ohne die Verfügbarkeit aller seiner enthaltenen Dienste verlieren.

Finden Sie unter Beispiele für die [Anwendung Lernprogramm zu aktualisieren](service-fabric-application-upgrade-tutorial.md) .

## <a name="maintain"></a>Verwalten
1. -Schnittstellen für Betriebssystem-Upgrades und Patches Service Fabric mit Azure-Infrastruktur auf alle Programme, die im Cluster ausgeführten Verfügbarkeit sichergestellt ist.

2. Für Upgrades und Patches zur Plattform Dienst Fabric upgrades Dienst Fabric selbst ohne Verlust der Verfügbarkeit von jeder der Anwendung auf dem Cluster ausgeführt.

3. Eine *Anwendungsadministrator* genehmigt das Hinzufügen oder Entfernen von Knoten aus einem Cluster nach analysieren zurückliegenden Kapazität Auslastung Daten und der geplanten zukünftigen Bedarf.

4. Einen *Operator* hinzufügen und Entfernen von Knoten vom *Anwendungsadministrator*angegeben.

5. Wenn neue Knoten hinzugefügt werden, oder vorhandene Knoten aus dem Cluster entfernt werden, laden-Salden Dienst Fabric automatisch die laufenden Anwendungen auf allen Knoten im Cluster durchführen, um optimale Leistung zu erzielen.

## <a name="remove"></a>Entfernen
1. Einen *Operator* kann eine bestimmte Instanz eines ausgeführten Diensts im Cluster löschen, ohne die gesamte Anwendung mithilfe der [Methode **DeleteServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), das [Cmdlet **ServiceFabricService entfernen** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)oder den [ **Dienst löschen** REST Vorgang](https://msdn.microsoft.com/library/azure/dn707687.aspx)entfernen.  

2. Einen *Operator* kann auch eine Instanz der Anwendung und alle zugehörigen Dienste mithilfe der [Methode **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), das [Cmdlet **ServiceFabricApplication entfernen** ](https://msdn.microsoft.com/library/azure/mt125914.aspx)oder den [REST der **Anwendung löschen** Vorgang](https://msdn.microsoft.com/library/azure/dn707651.aspx)löschen.

3. Nachdem Sie die Anwendung und Dienste beendet haben, kann der *Operator* den Anwendungstyp mithilfe der [Methode **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), das [Cmdlet **' Registrierung aufheben '-ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125885.aspx)oder die [restlichen **Aufheben der Bereitstellung Anwendung** Vorgang](https://msdn.microsoft.com/library/azure/dn707671.aspx)aufheben. Den Anwendungstyp bekannt wird das Anwendungspaket nicht aus der ImageStore entfernt. Sie müssen das Anwendungspaket manuell entfernen.

4. Einen *Operator* entfernt das Anwendungspaket aus der ImageStore mithilfe der [Methode **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) oder das [Cmdlet **ServiceFabricApplicationPackage entfernen** ](https://msdn.microsoft.com/library/azure/mt163532.aspx).

Beispiele finden Sie unter [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwickeln testen und Verwalten von Applications Dienst Fabric und Diensten, finden Sie unter:

- [Zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md)
- [Zuverlässigen Services](service-fabric-reliable-services-introduction.md)
- [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md)
- [Anwendungsupgrade](service-fabric-application-upgrade.md)
- [Prüfbarkeit (Übersicht)](service-fabric-testability-overview.md)
- [Beispiel für eine Anwendung REST-basierten Lebenszyklus](service-fabric-rest-based-application-lifecycle-sample.md)
