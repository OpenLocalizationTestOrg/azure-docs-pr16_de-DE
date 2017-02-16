


## <a name="advantages-of-integrating-compute-network-and-storage-under-the-azure-resource-manager-deployment-model"></a>Vorteile der Integration von Datenverarbeitung, Netzwerk und Speicher unter dem Modell zur Bereitstellung von Azure Ressourcenmanager

Das Modell zur Bereitstellung von Azure Ressourcenmanager bietet die Möglichkeit, einfach nutzen Sie vorgefertigte Anwendungsvorlagen oder eine Anwendungsvorlage zum Bereitstellen und Verwalten von Ressourcen Azure Datenverarbeitung, Netzwerk und Speicher zu erstellen. In diesem Abschnitt werden wir die Vorteile der Bereitstellung von Ressourcen über das Modell zur Bereitstellung von Azure Ressourcenmanager durchzuführen.

-   Problemloses – Komplexität erstellen, integrieren und komplizierte Applications, die der gesamte Farbskala Azure Ressourcen (z. B. Websites, SQL-Datenbanken, virtuellen Computern oder virtuelle Netzwerke) enthalten können aus einer Vorlagendatei freigegeben gemeinsames Bearbeiten
-   Flexibilität wiederholbare Bereitstellungen für Entwicklung, DevOps und System-Administratoren haben, wenn Sie die gleichen Vorlagendatei verwenden
-   Tiefe Integration von virtuellen Computer-Erweiterungen (benutzerdefinierte Skripts, DSC, Verwaltungsangestellte, Marionette usw.) mit Azure-Manager in einer Vorlagendatei sind einfache Planung der-VM-Setup-Konfiguration
-   Definieren von Tags und die Abrechnung Weitergabe dieser Tags für berechnen, Netzwerk und Speicher-Ressourcen
-   Einfache und präzise organisationsinterne Ressource Access Management mit Azure Role-Based Access Steuerelement (RBAC)
-   Vereinfachte Aktualisierung oder Textabschnitt durch die ursprüngliche Vorlage ändern und erneutes ihn dann


## <a name="advancements-of-the-compute-network-and-storage-apis-under-azure-resource-manager"></a>Fortschritten der berechnen, Netzwerk und Speicher-APIs unter Azure Ressourcenmanager

Neben den Vorteilen, weiter oben erwähnten gibt es einige Leistung erheblich Fortschritten in den APIs zur Verfügung:

-   Aktivieren von umfangreichen und parallele Bereitstellung von virtuellen Computern
-   Unterstützung für 3 Fehlerstrukturanalyse Domänen gebildeten Verfügbarkeit
-   Verbesserte benutzerdefiniertes Skript-Erweiterung, die Angabe von Skripts aus einer beliebigen benutzerdefinierten öffentlich zugängliche URL können
- Integration von virtuellen Computern mit dem Azure-Taste Tresor für hochgradig sicheren Speicher und private Bereitstellung der Kennwörter aus [FIPS-überprüften](http://wikipedia.org/wiki/FIPS_140-2) [Hardware Security Module](http://wikipedia.org/wiki/Hardware_security_module)
-   Stellt die grundlegenden Bausteine Netzwerke über APIs Kunden komplizierte Applications erstellen, die Netzwerk-Schnittstellen, Lastenausgleich und virtuelle Netzwerke enthalten aktivieren
-   Netzwerk-Schnittstellen wie ein neues Objekt ermöglicht komplizierte Netzwerkkonfiguration betreffen und für virtuellen Computern wiederverwendet werden
-   Lastenausgleich als erster Klasse Ressource ermöglicht Zuordnungen IP-Adresse
-   Detaillierte virtuelle Netzwerk-APIs können Sie zur Vereinfachung der Verwaltung von einzelne virtuelle Netzwerke

## <a name="conceptual-differences-with-the-introduction-of-new-apis"></a>Konzeptionelle Unterschiede im Lieferumfang des neuen APIs

In diesem Abschnitt wir wird erläutert, wie einige der wichtigsten konzeptionelle Unterschiede zwischen der XML-Code APIs verfügbar heute und JSON basierten APIs verfügbar über Azure-Manager für berechnen, Netzwerk und Speicher.

 Element | Azure Servicemanagement (XML-basierten)    | Berechnen Sie, Netzwerk und Speicherdienstanbieter (JSON-basiert)
 ---|---|---
| Cloud-Dienst für virtuellen Computern |  Cloud-Dienst wurde ein Container zum Speichern der virtuellen Computern, die Verfügbarkeit von der Plattform und den Lastenausgleich erforderlich. | Cloud-Dienst ist nicht mehr ein Objekt für das Erstellen eines virtuellen Computers mit dem neuen Modell erforderlich ist. |
| Verfügbarkeit von Gruppen | Verfügbarkeit der Plattform erkennbar wurde die gleichen "AvailabilitySetName" auf den virtuellen Computern konfigurieren. Die maximale Anzahl der Fehlerstrukturanalyse Domänen wurde 2. | Festlegen der Verfügbarkeit ist eine Ressource, Microsoft.Compute Anbieter bereitgestellt werden. Virtuellen Computern, die eine hohen Verfügbarkeit erfordern muss in der Verfügbarkeit festlegen enthalten sein. Die maximale Anzahl der Fehlerstrukturanalyse Domänen ist jetzt 3. |
| Gruppen die | Gruppen die wurden zur Erstellung von virtuellen Netzwerken erforderlich. Jedoch war im Lieferumfang des regionalen virtuelle Netzwerke, die nicht erforderlich nicht mehr. |Um zu vereinfachen, vorhanden nicht des Konzepts Gruppen die im verfügbaren bis Azure Ressourcenmanager APIs. |
| Lastenausgleich    | Erstellung einer Cloud-Dienst bietet eine implizit Lastenausgleich für den virtuellen Computern bereitgestellt. | Lastenausgleich ist eine Ressource, die von den Microsoft.Network-Anbieter verfügbar gemacht werden. Die primäre Schnittstelle der virtuellen Computer, für die Lastenausgleich durchgeführt werden muss sollte Lastenausgleich verweisen. Lastenausgleich können internen oder externen sein. [Weitere Informationen.](../articles/resource-groups-networking.md) |
|Virtuelle IP-Adresse | Cloud Services erhalten eine Standard-VIP (virtuelle IP-Adresse), wenn ein virtueller Computer auf einen Clouddienst hinzugefügt wird. Die virtuelle IP-Adresse ist die Adresse, den Lastenausgleich implizit zugeordnet.   | Öffentliche IP-Adresse ist eine Ressource, die von den Microsoft.Network-Anbieter verfügbar gemacht werden. Öffentliche IP-Adresse kann es sich um statische (reserviert) oder dynamische sein. Ein Lastenausgleich kann dynamische öffentlichen IP-Adressen zugewiesen werden. Öffentliche IP-Adressen kann mithilfe von Sicherheitsgruppen gesichert werden. |
|Reservierte IP-Adresse|   Sie können eine IP-Adresse in Azure reservieren und Zuordnen einer Cloud-Dienst, um sicherzustellen, dass die IP-Adresse Kurznotizen ist.   | Öffentliche IP-Adresse im Modus "Statische" erstellt werden können, und es bietet dieselbe Funktion als eine "reservierte IP-Adresse". Statische öffentliche IP-Adressen können nur ein Lastenausgleich sofort zugewiesen werden. |
|Öffentliche IP-Adresse (PIP) pro virtueller Computer | Öffentliche IP-Adressen können auch über zugeordnete auf einen virtuellen Computer direkt. | Öffentliche IP-Adresse ist eine Ressource, die von der Microsoft.Network-Anbieter verfügbar gemacht werden. Öffentliche IP-Adresse kann es sich um statische (reserviert) oder dynamische sein. Jedoch können nur dynamische öffentlichen IP-Adressen zu Netzwerk-Schnittstellen zugeordnet werden, können Sie eine öffentliche IP-Adresse pro virtueller Computer sofort zu gelangen. |
|Endpunkte| Eingabe Endpunkte erforderlich konfiguriert sein, auf einem virtuellen Computer von Konnektivität für bestimmte Ports geöffnet sein. Einer der die gängigen Modi der Herstellen einer Verbindung mit virtuellen Computern, die beim Einrichten von Endpunkten fertig. | Eingehende NAT Regeln können auf Lastenausgleich erzielen dieselbe Funktion der Aktivierung Endpunkte an bestimmten Ports zum Herstellen einer Verbindung mit den virtuellen Computern konfiguriert werden. |
|DNS-Name| Cloud-Dienst würden ein implizit global eindeutigen DNS-Namen erhalten. Beispiel: `mycoffeeshop.cloudapp.net`. | DNS-Namen sind optionale Parameter, die für eine Ressource öffentliche IP-Adresse angegeben werden können. Kann ich der vollqualifizierten Domänennamen in folgendem Format - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Netzwerk-Schnittstellen | Primären und sekundären Schnittstelle und deren Eigenschaften als Netzwerkkonfiguration eines virtuellen Computers definiert wurden. | Netzwerk-Schnittstellen gehören, eine Ressource, Microsoft.Network Anbieter bereitgestellt werden. Des Lebenszyklus von der Schnittstelle ist nicht mit einer virtuellen Computern verknüpft. |

## <a name="getting-started-with-azure-templates-for-virtual-machines"></a>Erste Schritte mit Azure Vorlagen für virtuellen Computern

Sie können loslegen mit den Azure-Vorlagen durch die verschiedenen Tools, die wir haben für die Entwicklung und Bereitstellung auf der Plattform Nutzung.

### <a name="azure-portal"></a>Azure-portal

Azure-Portal weiterhin die Möglichkeit, mit dem Bereitstellungsmodell klassischen-virtuellen Computern und virtuellen Computern mit dem Ressourcenmanager Bereitstellungsmodell gleichzeitig bereitstellen. Azure-Portal können auch benutzerdefinierte Vorlage Bereitstellungen.

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell haben zwei Modi der Bereitstellung – **AzureServiceManagement** und **AzureResourceManager** Modus.  AzureResourceManager Modus enthält jetzt außerdem die Cmdlets zum Verwalten der virtuellen Maschinen, virtuelle Netzwerke und Speicher-Konten. Sie können weitere Informationen dazu [hier](../articles/powershell-azure-resource-manager.md).

### <a name="azure-cli"></a>Azure CLI

Die Azure Line Interface (CLI Azure) müssen zwei Modi der Bereitstellung – **AzureServiceManagement** und **AzureResourceManager** Modus. Der Modus AzureResourceManager enthält jetzt außerdem Befehle zum Verwalten von virtuellen Computern, virtuelle Netzwerke und Speicher-Konten. Sie können weitere Informationen zu sie [hier](../articles/xplat-cli-azure-resource-manager.md).

### <a name="visual-studio"></a>Visual Studio

Sie können mit der neuesten Version von Azure SDK für Visual Studio verfassen und virtuellen Computern und komplexe Applikationen direkt vom Visual Studio bereitstellen. Visual Studio bietet die Möglichkeit zum Bereitstellen aus einer vordefinierten Liste von Vorlagen oder einer leeren Vorlage.

### <a name="rest-apis"></a>REST-APIs

Die ausführliche REST-API-Dokumentation für das berechnen, Netzwerk und Speicher Ressourcenanbieter finden Sie [hier](https://msdn.microsoft.com/library/azure/dn790568.aspx).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Kann ich mit der neuen Azure Ressourcenmanager zur Bereitstellung von in einem virtuellen Netzwerk oder Speicher-Konto mithilfe der Azure Service Management-APIs erstellt einen virtuellen Computer erstellen?**

Dies wird zurzeit nicht unterstützt. Sie können keine bereitstellen, verwenden die neuen Azure Ressourcenmanager APIs eines virtuellen Computers in einem virtuellen Netzwerk bereitstellen, die mithilfe der Service Management-APIs erstellt wurde.

**Kann ich Erstellen einer virtuellen Computern mithilfe der neuen Azure Ressourcenmanager APIs aus einem Benutzer Bild, das mithilfe der Azure Service Management APIs erstellt wurde?**

Dies wird zurzeit nicht unterstützt. Jedoch können Sie die virtuelle Festplatte Dateien von einem Konto Speicher kopieren, die mithilfe der Service Management-APIs erstellt wurde, und kopieren Sie ihn in ein neues Konto erstellt, mit der neuen Azure Ressourcenmanager APIs verwenden.

**Was ist die Auswirkung auf das Kontingent für mein Abonnement?**

Die Kontingente für den virtuellen Computern, virtuelle Netzwerke und Speicher-Konten über die neuen Azure Ressourcenmanager APIs erstellt unterscheiden sich von der Kontingente, die Sie aktuell arbeiten. Jedes Abonnement ruft neue Kontingente, die mithilfe der neuen APIs Ressourcen zu erstellen. Sie können weitere Informationen über die zusätzliche Kontingente [hier](../articles/azure-subscription-service-limits.md).

**Kann ich weiterhin meine automatisierte Skripts für provisioning virtuelle Computer, virtuelle Netzwerke, Speicher Konten usw. über die neuen Azure Ressourcenmanager APIs verwenden?**

Die Automatisierung und Skripts, die Sie erstellt haben weiterhin für die vorhandenen virtuellen Computern arbeiten, virtuelle Netzwerke erstellt haben, klicken Sie unter den Servicemanagement Azure-Modus. Haben jedoch die Skripts aktualisiert werden, um das neue Schema zur Erstellung von der dieselben Ressourcen durch den neuen Ressourcenmanager Azure-Modus verwenden.

**Wo finde ich Beispiele für Ressourcenmanager Azure-Vorlagen?**

Eine umfassende Reihe von Starter-Vorlagen finden Sie auf [Azure Ressourcenmanager Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/).
