
## <a name="key-features"></a>Hauptfeatures

* **Leistungsstarke Hardware** – diese Instanzen entworfen und optimiert Anwendungsmöglichkeiten berechnen ankommt und Netzwerk ankommt, einschließlich High Performance computing (HPC) und Stapel Applications, modellieren und umfangreiche Simulationen. 

    Grundlegenden Spezifikationen Bereich der Speicherkapazität und Laufwerk-Details finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md). Details zu den Intel Xeon E5-2667 v3 (in der Serie H verwendet) und Intel Xeon E5-2670 Prozessor (in A8 - A11), einschließlich unterstützten Anweisung Set-Erweiterung, werden bei der Website von Intel. 

* **Entwickelt für HPC Cluster** – Bereitstellen mehrerer Instanzen von rechenintensiven in Azure, erstellen Sie einen eigenständigen HPC Cluster oder Kapazität zu einem lokalen Cluster hinzufügen. Wenn Sie möchten, Bereitstellen Sie Cluster Verwaltungs- und Terminplanungssoftware Position. Oder verwenden Sie die Instanzen für rechenintensiver Arbeit in einem anderen Azure Service wie Azure Stapel.

* **RDMA-Verbindung für Applikationen MPI** – eine Teilmenge der Instanzen berechnen ankommt (H16r, H16mr, A8 und A9) bereitstellen eine zweite Netzwerkschnittstelle für remote direkte Arbeitsspeicher Access (RDMA) Konnektivität. Diese Schnittstelle ist zusätzlich die Azure standard-Netzwerk-Benutzeroberfläche, die andere Größen virtueller Computer zur Verfügung. 

    Diese Schnittstelle ermöglicht RDMA-fähigen Instanzen über ein Netzwerk InfiniBand arbeitet mit FDR Sätze für H16r und H16mr-virtuellen Computern und QDR Sätzen für A8 und A9 virtuellen Computern kommunizieren. RDMA-Funktionen, die in diesen virtuellen Computern verfügbar gemacht können die Skalierbarkeit und Leistung von bestimmter Applikationen Linux und Windows Nachricht übergeben Interface (MPI) durch fördern. Finden Sie unter [Zugriff auf das Netzwerk RDMA](#access-to-the-rdma-network) in diesem Artikel für Anforderungen ein.



## <a name="deployment-considerations"></a>Aspekte beim Bereitstellen

* **Azure-Abonnement** – Wenn Sie bereitstellen möchten, die mehr als eine kleine Anzahl von Instanzen berechnen ankommt, sollten Sie ein Abonnement je nach Bedarf berechnet oder andere Optionen erwerben. Wenn Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/)verwenden, können Sie nur eine eingeschränkte Anzahl von Kernen Azure berechnen.

* **Preise und Verfügbarkeit** – die rechenintensiven virtueller Computer Größen werden Preise Ebene nur in den standardmäßigen angeboten. Verfügbarkeit in Azure Regionen prüfen Sie [Produkte nach Region verfügbar](https://azure.microsoft.com/regions/services/) . 

* **Kerne Kontingent** – müssen Sie möglicherweise das Kontingent Kerne in Ihrem Azure Abonnement aus der standardmäßigen 20 Kerne pro Abonnement (Wenn Sie das Bereitstellungsmodell klassischen verwenden) oder 20 Kerne pro Region erhöhen, (Wenn Sie das Modell zur Bereitstellung von Ressourcenmanager verwenden). Ihr Abonnement möglicherweise auch die Anzahl der Kerne einschränken, die Sie in bestimmten virtuellen Computer Größe Familien, einschließlich der Serie H bereitstellen können. Zum Anfordern einer Kontingent zu erhöhen, [Öffnen Sie eine Supportanfrage online Kunden](../articles/azure-supportability/how-to-create-azure-support-request.md) kostenlos. (Standardgrenzwerte können abhängig von Ihrem Abonnement Kategorie variieren.)

    >[AZURE.NOTE]Wenn Sie umfangreiche Kapazität Anforderungen haben, wenden Sie sich an Azure-Support. Azure Kontingente sind Kreditkarte beschränkt ist, nicht Kapazität Garantien. Unabhängig von der Kontingent Sie nur für Kerne unterliegen, zu verwenden.

* **Virtuellen Netzwerk** – ein Azure- [virtuellen Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) ist nicht erforderlich, um die rechenintensiven Instanzen verwenden. Jedoch möglicherweise benötigten mindestens ein cloudbasierten Azure virtuelles Netzwerks für viele Szenarien für die Bereitstellung oder eine Verbindung zwischen Standorten, wenn Sie benötigen für den Zugriff auf lokale Ressourcen wie eine Lizenz Anwendungsserver. Wenn eine erforderlich ist, erstellen Sie ein neues virtuelles Netzwerk aus, um die Instanzen bereitstellen. Hinzufügen von rechenintensiven virtuellen Computern zu einem virtuellen Netzwerk in einer Gruppe Zugehörigkeit wird nicht unterstützt.

* **Cloud-Dienst oder Verfügbarkeit festlegen** – Sie verwenden das Azure RDMA Netzwerk bereitstellen der RDMA-fähigen virtuellen Computern in der gleichen Cloud-Dienst (Wenn Sie das Bereitstellungsmodell klassischen verwenden) oder die gleiche Verfügbarkeit festlegen (Wenn Sie das Modell zur Bereitstellung von Azure Ressourcenmanager verwenden). Wenn Sie Stapel Azure verwenden, muss die RDMA-fähigen virtuellen Computern im gleichen Pool aus.

* **Ändern der Größe** – können aufgrund der spezielle Hardware, die in den Instanzen rechenintensiven verwendet Sie nur berechnen ankommt Instanzen innerhalb der gleichen Größe Familie (H-Serie oder berechnen ankommt A-Serie) Größe ändern. Beispielsweise können Sie nur ein H-Serie virtuellen Computer aus einem H-Serie Größe in eine andere Größe ändern. Darüber hinaus wird das Ändern der Größe von eine nicht-rechenintensiven Größe auf eine Größe berechnen ankommt nicht unterstützt.  

* **Netzwerk-Adressbereichs RDMA** - RDMA im Netzwerk in Azure reserviert die Adresse Leerzeichen 172.16.0.0/16. Stellen Sie sicher, dass der virtuelle Netzwerk Adresse Platz nicht im Netzwerk RDMA überlappt Schritte aus, um auf Instanzen in einem Azure-virtuellen Netzwerk bereitgestellt MPI-Programme ausführen zu können.





