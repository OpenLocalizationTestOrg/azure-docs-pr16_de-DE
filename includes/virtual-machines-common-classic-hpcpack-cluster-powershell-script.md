



Abhängig von Ihrer Umgebung und Auswahlmöglichkeiten kann das Skript alle Cluster Infrastruktur einschließlich Azure virtuelles Netzwerk, Speicherkonten, Cloud Services, Domain Controller, Remotecomputer oder einem lokalen SQL-Datenbanken, am Knoten und zusätzliche Cluster-Knoten erstellen. Sie können auch das Skript mit bereits vorhandenen Azure-Infrastruktur und erstellen nur die HPC Cluster-Knoten.


Hintergrundinformationen zum Planen eines HPC Pack Clusters finden Sie unter den Inhalt [einer Testversion des Produkts und Planung](https://technet.microsoft.com/library/jj899596.aspx) und [Erste Schritte](https://technet.microsoft.com/library/jj899590.aspx) in der HPC Pack TechNet-Bibliothek.



## <a name="prerequisites"></a>Erforderliche Komponenten

* **Azure-Abonnement** – Sie können ein Abonnement in der globalen Azure oder China Azure-Dienst verwenden. Ihr Abonnement Einschränkungen wirkt sich die Anzahl und Typ der Clusterknoten, die Sie bereitstellen können. Informationen finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../articles/azure-subscription-service-limits.md).


* **Windows-Clientcomputer mit Azure PowerShell 0.8.7 oder höher installiert und konfiguriert** -finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../articles/powershell-install-configure.md) für Installations- und Schritte für die Verbindung zu Ihrem Azure-Abonnement.


* **Bereitstellungsskript HPC Pack IaaS** -, und Entpacken Sie die neueste Version des Skripts vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949)herunterladen. Überprüfen Sie die Version des Skripts durch Ausführen `New-HPCIaaSCluster.ps1 –Version`. In diesem Artikel basiert auf Version 4.4.1 des Skripts.

* **Konfigurations-Skriptdatei** – Sie müssen eine XML-Datei zu erstellen, die das Skript verwendet, um den HPC Cluster konfigurieren. Informationen und Beispiele hierzu finden Sie unter Abschnitten weiter unten in diesem Artikel und die Datei Manual.rtf, der das Bereitstellungsskript begleitet.


## <a name="syntax"></a>Syntax

```
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
>[AZURE.NOTE]Sie müssen das Skript als Administrator ausführen.

### <a name="parameters"></a>Parameter

* **ConfigFile** - gibt den Dateipfad der Konfigurationsdatei HPC-Cluster zu beschreiben. Weitere Informationen zu den Konfigurationsdatei in diesem Thema finden Sie unter oder in der Datei Manual.rtf in den Ordner, der das Skript enthält.

* **AdminUserName** - gibt den Benutzernamen an. Wenn der Domänengesamtstruktur durch das Skript erstellt wurde, wird diese lokale Administratorbenutzernamen für alle virtuellen Computern und der Administrator den Namen der Domäne. Wenn der Domänengesamtstruktur bereits vorhanden ist, gibt dies den Benutzer der Domäne als die lokale Administratorbenutzernamen HPC Pack installieren.

* **AdminPassword** - gibt das Kennwort des Administrators an. Nicht in der Befehlszeile angegeben, werden Sie das Skript aufgefordert, das Kennwort einzugeben.

* **HPCImageName** (optional) – gibt den HPC Pack virtuellen Computer Bildnamen verwendet, um den HPC Cluster bereitstellen. Sie müssen ein Bild Microsoft bereitgestellten HPC Pack aus dem Azure Marketplace. Wenn nicht angegeben (in den meisten Fällen empfohlen), das Skript wählt die letzte Veröffentlichung [HPC Pack Bild](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). Das neueste Bild basiert auf Windows Server 2012 R2 Datacenter mit HPC Pack 2012 R2 Update 3 installiert.

    >[AZURE.NOTE] Bereitstellung schlägt fehl, wenn Sie nicht, dass ein gültiges HPC Pack Bild angeben.

* **Protokolldatei** (optional) – gibt den Pfad der Bereitstellung Protokolldatei an. Wenn nicht angegeben, wird das Skript eine Protokolldatei in das temporäre Verzeichnis des Computers Ausführen der Skripts erstellen.

* **Erzwingen, dass** (optional) – Unterdrückt alle die Bestätigung Fragen.

* **NoCleanOnFailure** (optional) – gibt an, dass der Azure-virtuellen Computern, die nicht erfolgreich bereitgestellt werden nicht entfernt werden. Sie müssen diese virtuellen Computern manuell entfernen, bevor Sie erneut ausführen das Skript, um die Bereitstellung fortzusetzen, oder die Bereitstellung kann ein Fehler auftreten.

* **PSSessionSkipCACheck** (optional) – für jede Cloud-Dienst mit virtuellen Computern bereitgestellt, die von diesem Skript, ein selbst signiertes Zertifikat wird automatisch generiert, indem Azure, und alle virtuellen Computern in der Cloud-Dienst verwenden dieses Zertifikat als das standardmäßige Windows Remote Management (WinRM) Zertifikat. Um HPC-Features in dieser Azure-virtuellen Computern bereitstellen, das Skript standardmäßig vorübergehend installiert diese Zertifikate hinzufügen in den lokalen Computer\\vertrauenswürdigen Serverzertifikat des Client-Computers wird den Fehler "nicht vertrauenswürdigen Zertifizierungsstelle" Sicherheit während der Ausführung der Skripts; die Zertifikate werden entfernt, wenn das Skript endet. Wenn dieser Parameter angegeben wird, die Zertifikate nicht im Clientcomputer installiert sind, und die sicherheitswarnung wird unterdrückt.

    >[AZURE.IMPORTANT] Für diesen Parameter ist nicht für die Herstellung Bereitstellungen empfohlen.

### <a name="example"></a>Beispiel

Im folgende Beispiel erstellt einen neuen HPC Pack Cluster mithilfe der Konfigurationsdatei *MyConfigFile.xml*, und gibt die Administratorberechtigungen für die Installation von Cluster an.

```
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Weitere Aspekte



* Das Skript kann optional Senden des Auftrags über das Web-Portal HPC Pack oder die HPC Pack REST-API aktivieren.

* Das Skript kann optional benutzerdefinierte Skripts für vor und nach der Konfiguration auf dem am Knoten ausgeführt, wenn Sie zusätzlichen Software installieren oder andere Einstellungen konfigurieren möchten.


## <a name="configuration-file"></a>Konfigurationsdatei

Konfigurationsdatei für das Bereitstellungsskript ist eine XML-Datei. Die Schemadatei ist HPCIaaSClusterConfig.xsd in den Ordner HPC Pack IaaS Bereitstellung Skript. **IaaSClusterConfig** ist das Element aus der Konfigurationsdatei, die enthält die untergeordneten Elemente in der Datei Manual.rtf im Ordner Bereitstellung Skript ausführlich beschrieben.





