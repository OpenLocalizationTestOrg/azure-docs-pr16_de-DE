



Details über die virtuellen Computer-Agents und deren Funktionsweise virtueller Computer Extensions unterstützt finden Sie unter [virtueller Computer-Agents und Erweiterungen – Überblick über](../articles/virtual-machines/virtual-machines-windows-classic-manage-extensions.md).

##<a name="azure-vm-extensions"></a>Azure-virtuellen Computer Erweiterungen

Virtueller Computer Erweiterungen können die meisten der kritische Funktionen, die Sie mit Ihrem virtuelle Computer, einschließlich Grundfunktionen wie das Zurücksetzen von Kennwörtern, RDP, konfigurieren verwenden möchten, und viele, viele andere implementieren. Da neue Erweiterungen immer hinzugefügt haben, wird die Anzahl der möglichen Funktionen, die Ihre virtuellen Computer in Azure unterstützen weiterhin vergrößern. Standardmäßig werden mehrere grundlegende virtueller Computer Extensions installiert, wenn Sie Ihre virtuellen Computer aus der Bildergalerie, einschließlich **IaaSDiagnostics** (aktuell Windows virtuellen Computern nur), **VMAccess**und **BGInfo** (auch aktuell nur Windows) erstellen. Jedoch sind nicht alle Erweiterungen zu einem bestimmten Zeitpunkt, aufgrund des beständigen Flusses von Feature Updates und neue Erweiterungen auf Windows und Linux implementiert.

##<a name="connectivity-and-basic-management"></a>Konnektivität und einfache Verwaltung

Die folgenden Erweiterungen sind entscheidend, aktivieren, wieder aktivieren, oder deaktivieren grundlegende Konnektivität mit Ihrer virtuellen Computer aus, nach dem Erstellen und ausführen.

|Name des virtuellen Computers Erweiterung|Beschreibung der Features|Weitere Informationen
|---|---|---|
|VMAccessAgent (Windows)|Erstellen, aktualisieren und Benutzerinformationen und RDP Verbindung Konfigurationen zurücksetzen.|[Windows](../articles/virtual-machines/virtual-machines-windows-classic-extensions-customscript.md)
|VMAccessForLinux (Linux)|Erstellen, aktualisieren und Benutzerinformationen und SSH Verbindung Konfigurationen zurücksetzen.|[Linux](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess)

##<a name="deployment-and-configuration-management"></a>Implementierungs- und Konfiguration

Die folgenden Erweiterungen unterstützt verschiedene Arten von Bereitstellung und Konfiguration Management Szenarien und Features. Finden Sie auch im Abschnitt virtueller Computer Vorgänge und Verwaltung unter.

|Name des virtuellen Computers Erweiterung|Beschreibung der Features|Weitere Informationen|
|---|---|---|
|**MSEnterpriseApplication**|Implementiert Features für die Unterstützung von Windows System Center.|[System Center 2012 R2 virtuellen Computern Rollen](http://social.technet.microsoft.com/wiki/contents/articles/18274.system-center-2012-r2-virtual-machine-role-authoring-guide-resource-extension-package.aspx)|
|**Krake bereitstellen** (DSC Erweiterung-basiert)|Unterstützt automatisierte Bereitstellung von Webanwendungen ASP.NET und Windows-Dienste in Entwicklung, testen und Herstellung Umgebungen.|[Erste Schritte mit Krake bereitstellen](http://docs.octopusdeploy.com/display/OD/Getting%20started)|
|**Visual Studio Release-Manager** (DSC Erweiterung-basiert)|Unterstützt die kontinuierliche Bereitstellung mit Visual Studio.|[Automatisieren von Bereitstellungen mit Release Management](https://msdn.microsoft.com/Library/vs/alm/Release/overview)|
|**CentosChefClient**|||
|**ChefClient**|Erstellt einen Verwaltungsangestellte Client Windows. (Können auch die Erweiterung DSC unter verwenden.)|[Verwaltungsangestellte und Microsoft Azure](https://www.getchef.com/solutions/azure/)|
|**LinuxChefClient**|||
|**DockerExtension**|Installiert den Docker Daemon remote Docker Befehle unterstützt.|[So verwenden Sie die Erweiterung Docker virtuellen Computern](../articles/virtual-machines/virtual-machines-linux-dockerextension.md) Eine ausführlichere Informationen finden Sie unter der [Docker virtueller Computer Erweiterung Benutzerhandbuch](https://github.com/Azure/azure-docker-extension/blob/master/README.md)|
|**DSC**|PowerShell DSC (gewünschte staatliche Konfiguration) Erweiterung.|[Azure PowerShell DSC (gewünscht Zustand Konfiguration) Erweiterung](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx)|
|**PuppetEnterpriseAgent**|Implementiert die Funktionen von Marionette Enterprise an. |[Klicken Sie auf Azure Marionette](http://puppetlabs.com/solutions/microsoft)|
|**CustomScriptExtension** (Windows) **CustomScriptForLinux** (Linux)|Benutzerdefinierte Skripts des virtuellen Computers zu einem beliebigen Zeitpunkt ruft: Start oder beim Lebensdauer.|[Benutzerdefiniertes Skript-Erweiterung](../articles/virtual-machines/virtual-machines-windows-classic-extensions-customscript.md) | [Linux](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript)|
|**AzureCATExtensionHandler**|Es verbraucht Diagnose von **IaaSDiagnostics** und einige andere Datenquellen wie [Azure Speicher Analytics Metrik](https://msdn.microsoft.com/library/azure/hh343270.aspx) gesammelten Daten und wandelt sie in eine aggregierte Daten Sammlung entsprechenden für SAP-Host Steuerelement Prozess zu nutzen|[Erweiterte Azure Überwachung für SAP](https://azure.microsoft.com/blog/2014/06/04/azure-enhanced-monitoring-for-sap/)|

##<a name="security-and-protection"></a>Sicherheit und Schutz

Die Erweiterungen in diesem Abschnitt bieten wichtige Sicherheitsfeatures für Ihre Azure-virtuellen Computern an.

|Name des virtuellen Computers Erweiterung|Beschreibung der Features|Weitere Informationen|
|---|---|---|
|**CloudLinkSecureVMWindowsAgent**|Stellt Microsoft Azure-Kunden die Möglichkeit, ihre Daten virtuellen Computern auf eine gemeinsame Infrastruktur mit mehreren Mandanten und vollständig Kontrolle über die Verschlüsselungsschlüssel für ihre verschlüsselten Daten zu Azure-Speicher-Infrastruktur verschlüsseln.|[Sichern von Microsoft Azure-virtuellen Computern Nutzung von BitLocker und Native OS-Verschlüsselung](http://www.cloudlinktech.com/azure)|
|**McAfeeEndpointSecurity**|Schutz Ihrer virtuellen Computer vor bösartiger Software.|[McAfee](https://www.mcafeeasap.com/MarketingContent/default.aspx)|
|**TrendMicroDSA**|Aktiviert TrendMicros Tiefe Sicherheit Plattform Unterstützung um einen unbefugten und Prevention, Firewall, Anti-Malware, Web aufbauen, Log Prüfung und Überwachung Integrität bereitzustellen.|[So installieren und Konfigurieren von Trend Micro Tiefe Security als Dienst eine Azure-virtuellen Computers](../articles/virtual-machines/virtual-machines-windows-classic-install-trend.md)|
|**PortalProtectExtension**|Schutz vor Angriffen für Ihre Microsoft SharePoint-Umgebung.|[Sichern Ihrer SharePoint-Bereitstellung auf Azure](http://blog.trendmicro.com/securing-sharepoint-deployment-azure/)|
|**IaaSAntimalware**|Microsoft Antimalware für Azure-Cloud-Diensten und virtuellen Computern ist eine Schutz in Echtzeit-Funktion, die zu identifizieren Entfernen von Viren, Spyware und anderer bösartiger Software, mit konfigurierbare Benachrichtigungen und, wenn bekannt, dass bösartige oder unerwünschte Software versucht, installiert oder auf Ihrem System ausgeführt.|[Modul für Azure-Cloud-Diensten und virtuellen Computern](../articles/azure-security-antimalware.md)|
|**SymantecEndpointProtection**|Symantec Endpunkt Schutz (12.1.4) ermöglicht die Sicherheit und Leistung über physische und virtuelle Systeme.|[So installieren und Konfigurieren von Symantec Endpunkt Schutz einer Azure-virtuellen Computers](../articles/virtual-machines/virtual-machines-windows-classic-install-symantec.md)

##<a name="vm-operations-and-management"></a>Virtueller Computer Vorgänge und Verwaltung

Unterstützt allgemeine Vorgänge Verwaltungsfunktionen und Verhalten. Außerdem finden Sie im Abschnitt Bereitstellung und Verwaltung der Konfiguration, über ein.

|**Name des virtuellen Computers Erweiterung**|Beschreibung der Features|Weitere Informationen|
|---|---|---|
|**AzureVmLogCollector**|Sie können den **AzureVMLogCollector** Erweiterung bei Bedarf einmalige durchführen-Auflistung des Protokolle aus mindestens Cloud-Dienst virtuellen Computern (von Webrollen und Worker-Rollen) verwenden und auf ein Azure-Speicher – ganz ohne Remote-Anmeldung bei der virtuellen Computern an die gesammelten Dateien übertragen. |[AzureLogCollector-Erweiterung](../articles/virtual-machines/virtual-machines-windows-log-collector-extension.md)|
|**IaaSDiagnostics**|Ermöglicht, deaktiviert, und Azure-Diagnose konfiguriert und wird auch von der **AzureCATExtensionHandler** zur Unterstützung von SAP-Überwachung verwendet.|[Microsoft Azure virtuellen Computern für die Überwachung mit der Erweiterung Azure-Diagnose](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/)|
|**OSPatchingForLinux**|Die Azure-virtuellen Computer Administratoren die OS virtueller Computer Updates mit angepassten Konfigurationen automatisieren können. Sie können die Erweiterung OSPatching zum Konfigurieren von OS Updates für Ihren virtuellen Computern, einschließlich: angeben, wie häufig, und geben Sie beim Installieren von OS patches, was patches zum Installieren und konfigurieren das Verhalten Neustart nach Updates|[OS Patch Erweiterung Blogbeitrag veröffentlichen](https://azure.microsoft.com/blog/2014/10/23/automate-linux-vm-os-updates-using-ospatching-extension/). Siehe auch die Infos zu und die Quelle auf GitHub bei [OS Patch Erweiterung](https://github.com/Azure/azure-linux-extensions).|

##<a name="developing-and-debugging"></a>Entwickeln und Debuggen

Diese virtuellen Computer Erweiterungen werden auf Vollständigkeit, hier aufgeführt, wie diese bieten Unterstützung für Visual Studio miteinander verwandt und nicht direkt verwendet werden sollen.

|Name des virtuellen Computers Erweiterung|Beschreibung der Features|Weitere Informationen|
|---|---|---|
|**VS14CTPDebugger**|Remote-Debuggen von im Vergleich mit der Azure SDK 2.4 unterstützt|[Remote Debuggen in Visual Studio](https://msdn.microsoft.com/library/y7f5zaaa.aspx)|
|**VS2013Debugger**|Remote-Debuggen von im Vergleich mit der Azure SDK 2.4 unterstützt||
|**VS2012Debugger**|Remote-Debuggen von im Vergleich mit der Azure SDK 2.4 unterstützt||
|**RemoteDebugVS14CTP**|Remote-Debuggen von im Vergleich mit der Azure SDK 2.3 unterstützt||
|**RemoteDebugVS2013**|Remote-Debuggen von im Vergleich mit der Azure SDK 2.3 unterstützt||
|**RemoteDebugVS2012**|Remote-Debuggen von im Vergleich mit der Azure SDK 2.3 unterstützt||
|**WebDeployForVSDevTest**|Installation und Konfiguration von IIS und Web bereitstellen unter Windows Server. Entfernen oder deaktivieren sie wird nicht unterstützt.|

##<a name="miscellaneous-features"></a>Sonstige Features

Diese Erweiterungen bieten Unterstützung für andere virtueller Computer-Features, die hilfreich sein können.

|Name des virtuellen Computers Erweiterung|Beschreibung der Features|Weitere Informationen|
|---|---|---|
|**BGInfo**|Bietet konsolidierte Bild hilfreiche Serverinformationen auf dem Desktop, wenn RDP verwenden.|[BGInfo Erweiterung](https://msdn.microsoft.com/library/mt589195.aspx)|
|**HpcVmDrivers**|Installiert, konfiguriert und verwaltet die Gerätetreiber auf eine Größe A8 oder A9 virtueller Computer unter Windows Server 2012 R2 oder Windows Server 2012 remote direkte Arbeitsspeicher Access (RDMA) Netzwerk. Ermöglicht gruppierte A8 oder A9 virtuellen Computern im Netzwerk RDMA zu verwenden, wenn eine parallele MPI Anwendung ausgeführt.|[Über die A8, A9, A10 und A11 rechenintensiven Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)

