<properties
    pageTitle="Problembehandlung von Microsoft Azure Stapel | Microsoft Azure"
    description="Azure Stapel Problembehandlung."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Problembehandlung von Microsoft Azure Stapel

Dieses Dokument enthält häufige Problembehandlungsinformationen für Azure Stapel.

Stellen Sie für optimale Ergebnisse sicher, dass Ihre Umgebung Bereitstellung vor der Neuinstallation aller [Anforderungen](azure-stack-deploy.md) und [Vorbereitungen](azure-stack-run-powershell-script.md) entspricht. 

Die Empfehlungen zur Behandlung von Problemen, die in diesem Abschnitt beschriebenen aus verschiedenen Quellen stammen und möglicherweise oder Ihr bestimmte Problem möglicherweise nicht beheben. Wenn Sie ein Problem nicht auftreten, stellen Sie sicher, das [Azure Stapel MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) für weiteren Support und Weitere Informationen zu überprüfen.

Codebeispielen ungeändert bereitgestellt, und können nicht die erwarteten Ergebnisse garantiert werden. In diesem Abschnitt unterliegt häufige Bearbeitungen und Updates ein, wenn das Produkt Verbesserungen implementiert werden.

  

## <a name="known-issues"></a>Bekannte Probleme

 - Möglicherweise finden Sie unter den folgenden Fehler mit nicht Abbruch während der Bereitstellung die Bereitstellung Erfolg beeinflussen, gehen Sie wie folgt:
     - "Der Begriff 'C:\WinRM\Start-Logging.ps1' wird nicht erkannt"
     - "Aufrufen EceAction: index kann nicht für ein null-Array" 
     - "InvokeEceAction: Argument kann nicht auf Parameter 'Nachricht' gebunden werden, da es sich um eine leere Zeichenfolge ist."
 - Bereitstellung treten bei Schritt 60.61.93 mit einem Fehler "Application Bezeichner enthält 'URI' nicht gefunden" wird angezeigt Dieses Verhalten unterscheidet sich aufgrund der Ihrem Erwerb Applications in Azure Active Directory registriert sind.  Wenn dieser Fehler angezeigt wird, fahren Sie mit [das Installationsskript erneut ausführen](azure-stack-rerun-deploy.md) aus Schritt 60.61.93 bis zum Abschluss der Bereitstellung.
 - Sie sehen, dass die Ressource **Verfügbarkeit festlegen** auf der Marketplace, unter der Kategorie **VirtualMachine-Cloud angezeigt** – diese Darstellung nur ein gravierendes Problem ist.
 - Beim Erstellen eines neuen virtuellen Computers im Portal im Schritt **Grundlagen** standardmäßig die Speicheroption SSD.  Diese Einstellung muss auf der Festplatte oder im Schritt **Größe** der Bereitstellung geändert werden, werden keine virtuellen Computer Größen auswählen und Bereitstellung fortsetzen verfügbar angezeigt. 
 - Sehen Sie AzureRM PowerShell-Module werden nicht mehr standardmäßig des MAS-CON01 virtuellen Computers installiert (in TP1 wurde diese ClientVM bezeichnet). Dieses Verhalten unterscheidet sich von Entwurf, da es eine alternative Methode, um [diese Module installieren und verbinden wird](azure-stack-connect-powershell.md).  
 - Sie sehen, dass der **Microsoft.Insights** -Anbieter für Ressourcen nicht automatisch für den Mandanten Abonnements registriert ist. Wenn Sie, finden Sie unter Überwachen von Daten für einen virtuellen Computer als einen Mandanten bereitgestellt möchten, führen Sie den folgenden Befehl aus PowerShell (nachdem Sie [installieren, und verbinden Sie](azure-stack-connect-powershell.md) als einen Mandanten): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Sehen Sie Funktionen im Portal für Ressourcengruppen exportieren, jedoch kein Text angezeigt und Exportieren ist.      
 - Sie können eine Bereitstellung von Speicherressourcen größer als verfügbare Kontingent starten.  Diese Bereitstellung schlägt fehl, und die Konto Ressourcen angehalten.  Es gibt zwei Behebungsoptionen zur Verfügung:
     - Dienstadministrator kann das Kontingent, obwohl Änderungen nicht sofort wirksam und häufig in Anspruch nehmen in eine Stunde um verteilen zu erhöhen.
     - Dienstadministrator kann einen Add-on-Plan mit zusätzlichen Kontingent erstellen, die der Mandanten klicken Sie dann das Abonnement hinzugefügt werden kann.
 - Bei Verwendung des Portals zum Erstellen von virtuellen Computern auf Azure Stapel Umgebungen mit Identität in 'Azure - China virtueller Computer Größen verfügbar sind, wählen Sie im Schritt **Größe** der Bereitstellung wird nicht angezeigt, und Sie nicht Bereitstellung fortgesetzt werden.
 - Ein Bereitstellungsfehler im Portal möglicherweise angezeigt, wenn der virtuellen Computer tatsächlich erfolgreich bereitgestellt hat.
 - Wenn Sie einen Plan, Angebot, oder das Abonnement löschen, möglicherweise nicht virtuellen Computern gelöscht werden.
 - Sie sehen die Erweiterungen virtueller Computer auf der Marketplace.
 - Sie können kein virtuellen Computers aus einem gespeicherten virtuellen Computer Bild bereitstellen.
 - Mandanten eventuell Services angezeigt werden, die nicht in ihrem Abonnement enthalten sind.  Wenn Mandanten versuchen, diese Ressourcen bereitstellen, erhalten sie eine Fehlermeldung.  Beispiel: Mandanten Abonnement enthält nur Speicherressourcen.  Mandanten wird die Option zum Erstellen von anderen Ressourcen wie virtuellen Computern angezeigt.  In diesem Szenario bei dem Versuch, dass eines Mandantenverwaltungs bereitstellen ein virtuellen Computers, erhalten sie die Meldung, die angibt, dass der virtuellen Computer erstellt werden kann. 
 - Bei der Installation von TP2 sollten Sie nicht den Host aktivieren OS der virtuellen Festplatte bereitgestellt, in dem Sie das Setup-Skript Azure Stapel ausführen, oder einen Fehler, dass Windows messaging wird möglicherweise bald abläuft.


## <a name="deployment"></a>Bereitstellung

### <a name="deployment-failure"></a>Bereitstellungsfehler
Wenn Sie während der Installation einen Fehler auftreten, kann das Installationsprogramm Azure Stapel Sie Installationsfehler weiterhin anhand der [Führen Sie vor der Bereitstellung erneut aus](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Am Ende der Bereitstellung die PowerShell-Sitzung noch geöffnet und keine Ausgabe nicht anzeigen

Dieses Verhalten ist wahrscheinlich nur das Ergebnis des Standardverhaltens eines PowerShell-Befehl-Fensters, wenn es ausgewählt wurde. Die Bereitstellung der Prüfung des Konzepts ist tatsächlich erfolgreich war, aber das Skript wurde angehalten, wenn das Fenster auswählen. Sie können überprüfen, dass dies der Groß-/Kleinschreibung von wonach das Wort "Wählen Sie in der Titelleiste des Fensters Befehl" ist.  Drücken Sie ESC, um ihn Aufheben der Markierung, und die Nachricht Fertigstellung dahinter angezeigt werden soll.

## <a name="templates"></a>Vorlagen

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure Vorlage wird nicht für Azure Stapel bereitstellen.

Stellen Sie sicher, dass:

- Die Vorlage muss einen Microsoft Azure-Dienst verwenden, der bereits verfügbar oder in der Vorschau in Azure Stapel ist.
- Die APIs für eine bestimmte Ressource verwendet werden unterstützt, durch die lokale Azure Stapel Instanz und, dass Sie einen gültigen Speicherort ("lokal" in Azure Stapel Technical Preview (TP) 2 im Vergleich mit einer der "Ostasiatischen US" oder "Süd Indien" in Azure) entwickeln.
- Überprüfen Sie [in diesem Artikel](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) über die Cmdlets Test-AzureRmResourceGroupDeployment, die wodurch kleine Unterschiede bei Azure Ressourcenmanager Syntax abgefangen.

Bereits in der [GitHub Repository](http://aka.ms/AzureStackGitHub/) bereitgestellten Azure Stapel Vorlagen können Sie auch die Ihnen beim Einstieg helfen.

## <a name="virtual-machines"></a>Virtuellen Computern

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Nach dem Starten meine Microsoft Azure Stapel Prüfung des Konzepts ist Host, werden alle meine Mandanten virtuellen Computern also von Hyper-V-Manager und zurückkommen automatisch nach etwas warten?

Wenn das System wieder von der Speichersubsystem und RPs müssen Konsistenz festlegen, verfügbar ist. Die benötigte Zeit hängt von der Hardware und Specification verwendet wird, aber es möglicherweise einige Zeit nach einem Neustart des Hosts für den Mandanten virtuelle Computer wieder und erkannt werden.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ich einige virtuellen Computern gelöscht, aber weiterhin die virtuelle Festplatte Dateien auf dem Datenträger anzuzeigen. Ist dieses Verhalten zu erwarten?

Ja, dies ist Verhalten erwartet. Es wurde so entwickelt, da:

- Wenn Sie einen virtuellen Computer löschen, werden die virtuellen Festplatten nicht gelöscht. Datenträger sind separate Ressourcen in der Ressourcengruppe.
- Wenn Sie ein Speicherkonto gelöscht wird, der Löschvorgang sofort durch Azure Ressourcenmanager (Portal PowerShell) sichtbar ist, aber der Datenträger, der möglicherweise enthalten bleiben weiterhin im Speicher bis Garbagecollection ausgeführt wird.

Wenn Sie "Zeilen" virtuelle Festplatten angezeigt wird, ist es wichtig zu wissen, ob sie den Ordner für ein Speicherkonto gehören, die nicht gelöscht wurde. Wenn das Speicherkonto nicht gelöscht wurde, ist es normal, dass sie weiterhin angezeigt werden.

Weitere Informationen zum Konfigurieren von des oberen Schwellenwerts der Aufbewahrungsrichtlinien in [Verwalten Speicher Benutzerkonten](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Installationsschritte
Die folgende Informationen zum Azure Stapel Installationsschritte sein für die Problembehandlung hilfreich:  

| Index | Namen | Beschreibung|
| ----- | ----- | -----|
|0,11 | (DEP) Überprüfen Sie die physische Computer | Überprüfen von Hardware und der OS-Konfiguration auf physischen Knoten. |
| 0,12 | (DEP) Konfigurieren der physischen Netzwerke für Prüfung des Konzepts | Konfigurieren von virtuellen Netzwerk Schalter und Schnittstellen. |
| 0.14 | (DEP) Bereitstellen der Domäne | Bereitstellen von Active Directory auf virtuellen Computern. |
| 0,15 verwendet wird. | (DEP) Konfigurieren der Domäne | Konfigurieren Sie Domäne mit Sicherheitsgruppen usw.. |
| 0,16 | (DEP) Konfigurieren der physischen Computer | Konfigurieren von Netzwerke, Domäne beitreten und lokale Administratoren einrichten. |
| 0.18 | (STO) Konfigurieren von Speicher Cluster | Erstellen von Speicher Cluster, erstellen Sie einen Speicher Speicherpool und Server. |
| 0.19 | KLI (WERT) Fabric-Infrastruktur für die Einrichtung | Richten Sie die erforderlichen Komponenten für die Bereitstellung von Fabric aus. |
| 0,21 | (NETTO) Einrichten von BGP und NAT | Installiert BGP und NAT - nur für einen Knoten erforderlich. |
| 0,22 | (NETTO) Konfigurieren von NAT und Zeitservers | Synchronisiert den Zeitservers und NAT-Einträgen konfiguriert. |
| 40.41 | KLI (WERT) Erstellen von virtuellen Gastcomputern | Erstellen der Verwaltung von virtuellen Computern an. |
| 40.42 | (FBI) Einrichten von PowerShell JEA | Einrichten von PowerShell JEA für alle Rollen. |
| 40.43 | (FBI) Einrichten von Azure Stapel Zertifizierungsstelle | Zertifizierungsstelle Azure Stapel-Installationen. |
| 40.44 | (FBI) Konfigurieren der Zertifizierungsstelle Azure Stapel | Konfiguriert Azure Stapel Zertifizierungsstelle an. |
| 40.45 | (NETTO) Einrichten von NC auf virtuellen Computern | NC-Installationen auf den virtuellen Gastcomputern |
| 40.46 | (NETTO) Konfigurieren von NC auf virtuellen Computern | Konfigurieren von NC auf den virtuellen Gastcomputern |
| 40.47 | (NETTO) Konfigurieren von virtuellen Gastcomputern | Konfigurieren der Verwaltung von virtuellen Computern mit NC ACLs. |
| 60.61.81 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Services - FabricRing Voraussetzung | VIPs für FabricRing erstellt |
| 60.61.82 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Services – Fabric anrufen Cluster bereitstellen | Installation und Konfiguration von Azure Stapel Fabric anrufen Cluster. |
| 60.61.83 | (FBI) Bereitstellen von Admin-Erweiterungen für Ressourcenanbieter | Installieren von Admin-Erweiterungen für Ressourcenanbieter |
| 60.61.84 | (ACS) Richten Sie konsistente Azure Speicher in Knotenebene aus. | Installation und Konfiguration konsistent Azure Speicher in Ebene des Knotens. |
| 60.61.85 | (ACS) Richten Sie konsistente Azure Speicher im Clusterebene aus. | Installation und Konfiguration von Azure konsistent Speicher in Clusterebene. |
| 60.61.86 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für InfraServiceController |
| 60.61.87 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für KLI |
| 60.61.88 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für ASAppGateway |
| 60.61.89 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für Speicher-Controller |
| 60.61.90 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für HealthMonitoring |
| 60.61.91 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für ECE |
| 60.61.92 | (FBI) Bereitstellen von Azure Stapel Fabric anrufen Controller Services - Voraussetzung | Erforderliche Komponenten für PMM |
| 60.61.93 | (Katal) Erstellen von AzureStack Dienst Hauptbenutzer | Erstellen von Applications Azure Graph und Dienst Hauptbenutzer in AAD. |
| 60.61.94 | (NETTO) Setup GW virtuellen Computern | Installationen GW auf den virtuellen Gastcomputern an. |
| 60.61.95 | (NETTO) Konfigurieren von GW virtuellen Computern | Die Gastcomputern konfiguriert GW. |
| 60.61.96 | (NETTO) Bereitstellen auf Hosts iDNS | Bereitstellen Sie auf Infrastruktur Hosts iDNS |
| 60.61.97 | (NETTO) Konfigurieren von iDNS | Konfigurieren von iDNS Rolle |
| 60.61.98 | (FBI) Setup WSUS virtuellen Computern | Installationen der zentrale auf den virtuellen Gastcomputern an. |
| 60.61.99 | (FBI) Konfigurieren von WSUS virtuellen Computern | Die virtuellen Gastcomputern konfiguriert der zentrale. |
| 60.61.100 | (FBI) Setup von SQL Azure-virtuellen Computern | Azure SQL Server-Installationen auf den virtuellen Gastcomputern |
| 60.61.101 | (Katal) Setup erforderliche Komponenten für wurde virtuelle Computer an. | Richtet die erforderlichen Komponenten für Microsoft Azure Stapel auf den virtuellen Gastcomputern. |
| 60.61.102 | (Katal) Setup wurde virtuellen Computern | Installiert Microsoft Azure Stapel auf den virtuellen Gastcomputern an. |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.121 | (FBI) Bereitstellen von Ressourcenanbieter und Controller | Ressourcenanbieter und Controller-Installationen |
| 60.120.122 | (FBI) Controllerkonfiguration | Controller konfiguriert |
| 60.120.123 | (Katal) Konfigurieren von WAS virtuellen Computern | Microsoft Azure Stapel konfiguriert der virtuellen Gastcomputern. |
| 60.120.124 | (Katal) Azure Stapel AAD Konfiguration. | Konfiguriert Azure Stapel mit Azure AD an. |
| 60.120.125 | (Katal) Installieren von ADFS | Active Directory Federation Services (ADFS) installiert |
| 60.120.126 | (Katal) Installieren der ADFS-Diagramm | Azure Stapel Graph-Installationen |
| 60.120.127 | (Katal) Konfigurieren von ADFS | Active Directory Federation Services (ADFS) konfiguriert |
| 60.140.141 | (FBI) Konfigurieren von SRP | Konfiguriert Speicher-Anbieter für Ressourcen |
| 60.140.142 | (ACS) Konfigurieren von konsistent Azure Speicher. | Konfiguriert konsistent Azure Speicher. |
| 60.140.143 | (FBI) Erstellen von Speicherkonten | Erstellen Sie alle Speicherkonten von anderen Anbietern verwendet werden. |
| 60.140.144 | (FBI) Registerverwendung für SRP | Verwendung für Speicheranbieter zu registrieren. |
| 60.140.145 | KLI (WERT) Migrieren von erstellten virtuellen Computern, Hosts und Cluster zu KLI | Objekte, die von den erstellten virtuellen Computern, Hosts und Cluster migriert zu KLI |
| 60.140.146 | (FBI) Konfigurieren von Windows Defender | Windows Defender konfiguriert |
| 60.160.161 | (MO) Konfigurieren von Agents für die Überwachung | Agent für die Überwachung konfiguriert |
| 60.160.162 | (FBI) NRP Voraussetzung | Installationen NRP erforderliche Komponenten |
| 60.160.163 | (FBI) NRP Bereitstellung | Installationen NRP  |
| 60.160.164 | (FBI) NRP Konfiguration | NRP konfiguriert |
| 60.160.165 | (FBI) CRP Voraussetzung | Installationen CRP erforderliche Komponenten |
| 60.160.166 | (FBI) CRP-Bereitstellung | CRP-Installationen |
| 60.160.167 | (FBI) CRP-Konfiguration | CRP konfiguriert |
| 60.160.168 | (FBI) FRP Voraussetzung | Voraussetzungen für die FRP-Installationen |
| 60.160.169 | (FBI) FRP Bereitstellung | FRP-Installationen  |
| 60.160.170 | (FBI) FRP Konfiguration | Konfiguriert FRP |
| 60.160.174 | (FBI) Erforderliche URP | Voraussetzungen für die URP-Installationen |
| 60.160.175 | (FBI) URP Bereitstellung | URP-Installationen  |
| 60.160.176 | (FBI) URP Konfiguration | Konfiguriert URP |
| 60.160.171 | (FBI) HRP Voraussetzung | Installationen HRP erforderliche Komponenten |
| 60.160.172 | (FBI) HRP-Bereitstellung | HRP-Installationen  |
| 60.160.173 | (FBI) HRP-Konfiguration | HRP konfiguriert |
| 60.160.177 | (KV) KeyVault Voraussetzung | Voraussetzungen für die KeyVault-Installationen |
| 60.160.178 | (KV) KeyVault Bereitstellung | KeyVault-Installationen  |
| 60.160.179 | (KV) KeyVault Konfiguration | Konfiguriert KeyVault | 
| 60.190.191 | (FBI) Konfigurieren der Katalog | Konfigurieren der Katalog |
| 60.190.192 | (FBI) Konfigurieren von Diensten für Fabric anrufen | Konfigurieren von Diensten für Fabric anrufen |
| 60.221 | (FBI) Setup Console virtuellen Computern | Installationen Console-Server auf den virtuellen Gastcomputern an. |
| 60.222 | (FBI) Setup Console virtuellen Computern | Verschieben Sie DVM-Inhalt an die Konsole virtueller Computer. |
| 251 | Bereiten Sie für Neustart des zukünftigen Servers vor | Festgelegte Neustart Richtlinie |


## <a name="next-steps"></a>Nächste Schritte

[Häufig gestellte Fragen](azure-stack-FAQ.md)
