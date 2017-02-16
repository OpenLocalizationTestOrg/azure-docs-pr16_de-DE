
<properties
   pageTitle="Erstellen Sie einen sicheren Dienst Fabric Cluster über das Azure-Portal | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie eine sichere Dienst Fabric Cluster in Azure mithilfe der Azure-Portal und Azure-Taste Tresor einrichten."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/21/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Erstellen Sie einen Dienst Fabric Cluster in Azure mithilfe des Azure-Portals

> [AZURE.SELECTOR]
- [Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md)
- [Azure-portal](service-fabric-cluster-creation-via-portal.md)

Dies ist eine schrittweise Anleitung, die Sie durch die Schritte zum Einrichten von einem sicheren Dienst Fabric Cluster in Azure mithilfe des Azure-Portals geführt. Dieses Handbuch führt Sie durch die folgenden Schritte aus:

 - Einrichten von Key Tresor Tasten zum Clustersicherheit zu verwalten.
 - Erstellen Sie einen gesicherten Cluster in Azure über das Azure-Portal an.
 - Authentifizieren Sie Administratoren Zertifikate verwenden.

>[AZURE.NOTE] Weitere Erweiterte Sicherheitsoptionen, z. B. Benutzerauthentifizierung mit Azure Active Directory und Einrichten von Zertifikate Anwendung Wertpapiers, [Erstellen Sie den Cluster mithilfe der Ressourcenmanager Azure][create-cluster-arm].

Ein sicherer Cluster ist ein Cluster, der verhindert den nicht autorisierten Zugriff auf Management-Vorgänge und, der bereitstellen, aktualisieren und Löschen von Applications, Services und die darin enthaltenen Daten enthält. Ein unsicher Cluster handelt es sich um einen Cluster, dass jeder zu einem beliebigen Zeitpunkt Herstellen einer Verbindung mit und Verwaltungsvorgänge ausführen kann. Obwohl es so erstellen Sie einen unsicheren Cluster möglich ist, ist es **dringend empfohlen, um eine sichere Cluster zu erstellen**. Ein neuer Cluster muss eine unsicheren Cluster, **können nicht geschützt werden, höher** ist - erstellt werden.

Die Konzepte sind für das Erstellen der secure Cluster, identisch Zuordnungseinheiten Linux Cluster oder Windows-Cluster sind. Weitere Informationen und Helper Skripts für das Erstellen der secure Linux Cluster finden Sie unter [Erstellen sicherer Cluster unter Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-cluster). Die Parameter, die durch das bereitgestellte Helper Skript ermittelt können direkt im Portal eingegeben werden, wie im Abschnitt [Erstellen Sie einen Cluster im Portal Azure](#create-cluster-portal)beschrieben.

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure
Dieses Handbuch verwendet [Azure PowerShell][azure-powershell]. Wenn Sie eine neue PowerShell-Sitzung zu starten, melden Sie sich bei Ihrem Azure-Konto, und wählen Sie Ihr Abonnement vor dem Ausführen von Azure Befehlen aus.

Melden Sie sich bei Ihrem Konto Azure aus:

```powershell
Login-AzureRmAccount
```

Wählen Sie Ihr Abonnement aus:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Taste Tresor einrichten

Dieses Teils des Handbuchs führt Sie durch die Erstellung einer Taste Tresor für einen Dienst Fabric Cluster in Azure und für Applikationen Dienst Fabric. Eine vollständige Anleitung auf-Taste Tresor, finden Sie die [Taste Tresor erste Schritte mit][key-vault-get-started].

Dienst Fabric verwendet x. 509-Zertifikate, um einen Cluster sichern. Azure-Taste Tresor dient zum Verwalten von Zertifikaten für Dienst Fabric Cluster in Azure. Wenn ein Cluster in Azure bereitgestellt wird, der Azure Ressourcenanbieter Dienst Fabric Cluster Softwareentwickler Zertifikate von Key Tresor abruft; installiert sie auf dem Cluster virtuellen Computern

Das folgende Diagramm veranschaulicht die Beziehung zwischen Schlüssel Tresor, einem Dienst Fabric Cluster und der Anbieter Azure Ressourcen, der Zertifikate in Schlüssel Tresor gespeichert werden, wenn es sich um einen Cluster erstellt verwendet:

![Zertifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der erste Schritt besteht im Erstellen einer neuen Ressourcengruppe speziell für Schlüssel Tresor. Einer eigenen Ressourcengruppe Schlüssel Tresor Inbetriebnahme wird empfohlen, damit Sie Datenverarbeitung und Speicher Ressourcengruppen – wie etwa die Ressourcengruppe, die Ihrem Dienst Fabric Cluster verfügt über - entfernen können, ohne Ihre Schlüssel und Kennwörter zu verlieren. Die Ressourcengruppe aus, die Ihrem Tresor Schlüssel hat muss in der gleichen Region als Cluster, die es verwendet wird.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Erstellen von Key Tresor 

Erstellen einer Taste Tresor in der neuen Gruppe für die Ressource an. Der Schlüssel Tresor **für Bereitstellung aktiviert werden muss** für den Dienst Fabric Ressourcenanbieter abzurufenden Zertifikate aus es, und installieren Sie auf Knoten im Cluster zulassen:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Wenn Sie eine vorhandene Schlüssel Tresor verfügen, können Sie es zur Bereitstellung mit Azure CLI aktivieren:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-to-key-vault"></a>Hinzufügen von Zertifikaten zum Schlüssel Tresor

Zertifikate werden in Dienst Architektur zur Bereitstellung von Authentifizierung und Verschlüsselung verschiedene Aspekte von einem Cluster und deren Applikationen gesichert. Weitere Informationen darüber, wie Zertifikate Dienst Struktur verwendet werden, finden Sie unter [Service Fabric Cluster Sicherheitsszenarien][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster und Server-Zertifikat (erforderlich) 

Dieses Zertifikat ist erforderlich, um einem Cluster secure und nicht autorisierten Zugriff zu verhindern. Es bietet Clustersicherheit auf einige Arten:
 
 - **Cluster Authentifizierung:** Authentifiziert Knoten-zu-Knoten-Kommunikation für Cluster Föderation an. Nur Knoten, die ihre Identität mit dieser Urkunde nachweisen können können Cluster teilnehmen.
 - **Serverauthentifizierung:** Authentifiziert die Cluster Verwaltung von Endpunkten in einer Management-Client, damit der Management-Client weiß, dass es in den richtigen Cluster zu sprechen. Dieses Zertifikat enthält auch SSL für die Verwaltung HTTPS API und Dienst Fabric Explorer über HTTPS.

Um diesen Zwecken verwendet werden kann, muss das Zertifikat die folgenden Anforderungen entsprechen:

 - Das Zertifikat muss einen privaten Schlüssel enthalten.
 - Das Zertifikat muss für Key Exchange, exportiert in eine Datei Personal Information Exchange (PFX-Datei) erstellt werden.
 - Der Name des Zertifikats Betreff muss die Zugriff auf den Dienst Fabric Cluster verwendete Domäne übereinstimmen. Dies ist erforderlich, um SSL für der Clusters HTTPS Management Endpunkte und Dienst Fabric Explorer bereitzustellen. Sie können kein SSL-Zertifikat von einer Zertifizierungsstelle (CA) beziehen, für die `.cloudapp.azure.com` Domäne. Erwerben Sie einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern muss der Name des Zertifikats Betreff des benutzerdefinierten Domänennamens für Ihren Cluster verwendet übereinstimmen.

### <a name="client-authentication-certificates"></a>Client-Authentifizierungszertifikate

Zusätzliche Client-Zertifikate authentifiziert Administratoren für Aufgaben zur Verwaltung. Dienst Fabric weist zwei Zugriffsebenen: **Administrator** und **schreibgeschützt Benutzer**. Mindestens sollte ein einzelnes Zertifikat für Administratorzugriff verwendet werden. Für weitere Zugriff auf Benutzerebene muss ein separates Zertifikat bereitgestellt werden. Weitere Informationen zu Access Rollen, finden Sie unter [rollenbasierte Access Control für Dienst Fabric Clients][service-fabric-cluster-security-roles].

Sie müssen nicht Client-Authentifizierungszertifikate zum Schlüssel Tresor für die Arbeit mit Service Fabric hochladen. Diese Zertifikate werden nur für Benutzer, die autorisiert sind für die Clusterverwaltung bereitgestellt werden müssen. 

>[AZURE.NOTE] Azure-Active Directory wird empfohlen, zum Authentifizieren von Clients für Cluster Management Vorgänge an. Um Azure Active Directory verwenden zu können, müssen Sie [Erstellen Sie einen Cluster mit Azure Ressourcenmanager][create-cluster-arm].

### <a name="application-certificates-optional"></a>Anwendung Zertifikate (optional)

Eine beliebige Anzahl zusätzlicher Zertifikate kann in einem Cluster aus Sicherheitsgründen Anwendung installiert werden. Beachten Sie vor dem Erstellen des Clusters an, die Anwendung Sicherheitsszenarien, die erfordern ein Zertifikat auf den Knoten installiert werden, wie ein:

 - Verschlüsseln und Entschlüsseln des Anwendung Konfigurationswerte
 - Verschlüsselung von Daten über Knoten während der Replikation 

Anwendung Zertifikate können nicht konfiguriert werden, wenn einen Cluster über das Azure-Portal zu erstellen. Um Anwendung Zertifikate bei der Installation Cluster konfigurieren zu können, müssen Sie [Erstellen Sie einen Cluster mit Azure Ressourcenmanager][create-cluster-arm]. Sie können auch Zertifikate Anwendung zum Cluster hinzufügen, nachdem er erstellt wurde.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatierung von Zertifikaten für die Verwendung der Ressource Azure-Anbieter

Private Schlüssel Dateien (PFX-Datei) können hinzugefügt und direkt über Schlüssel Tresor verwendet werden. Der Anbieter Azure Ressourcen erfordert jedoch Tasten in einem speziellen JSON-Format gespeichert werden, die die PFX-Datei als Base-64 enthält codierte Zeichenfolge und das Kennwort für den privaten Schlüssel. Um diese Anforderungen aufnehmen zu können, müssen Tasten in eine JSON-Zeichenfolge platziert und dann als *Kennwörter* in Schlüssel Tresor gespeichert werden.

Um dieses Verfahren zu erleichtern, steht ein PowerShell-Modul [auf GitHub][service-fabric-rp-helpers]. Wie folgt vor, um das Modul verwenden:

 1. Herunterladen des gesamten Inhalts der Repo in einem lokalen Verzeichnis. 
 2. Importieren des Moduls in der PowerShell-Fenster an:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```
     
Die `Invoke-AddCertToKeyVault` Befehl in diesem PowerShell-Modul automatisch einen Zertifikat privater Schlüssel in eine JSON-Zeichenfolge formatiert, und lädt diese in Tresor-Taste. Verwenden sie das Zertifikat Cluster und alle weiteren Anwendung Zertifikate zum Schlüssel Tresor hinzuzufügen. Wiederholen Sie diesen Schritt für jede zusätzliche Zertifikate, die Sie in Ihrem Cluster installieren möchten.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Dies sind alle erforderlichen Schlüssel Tresor Komponenten für das Konfigurieren der Dienst Fabric Cluster Ressourcenmanager Vorlage, die Zertifikate für Knoten Authentifizierung, Management Endpunkt Sicherheit und Authentifizierung installiert, und alle weiteren Anwendung Sicherheitsfeatures, die x. 509-Zertifikate verwenden. An diesem Punkt sollten Sie jetzt die folgende Einrichtung in Azure haben:

 - Taste Tresor Ressourcengruppe
   - Key Tresor
     - Cluster Serverzertifikat-Authentifizierung

</a "erstellen-Cluster-Portals" ></a>
## <a name="create-cluster-in-the-azure-portal"></a>Erstellen von Cluster im Azure-Portal

### <a name="search-for-the-service-fabric-cluster-resource"></a>Suchen Sie nach der Dienst Fabric Clusterressource

![Suchen Sie nach Dienst Fabric Cluster Vorlage Azure-Portal.][SearchforServiceFabricClusterTemplate]

 1. Melden Sie sich bei der [Azure-Portal][azure-portal].

 2. Klicken Sie auf **neu** , um eine neue Ressourcenvorlage hinzuzufügen. Suchen Sie nach der Dienst Fabric Cluster Vorlage auf der **Marketplace** unter **Alles**.

 3. Wählen Sie **Dienst Fabric Cluster** aus der Liste aus.

 4. Navigieren Sie zu dem **Dienst Fabric Cluster** Blade, klicken Sie auf **Erstellen**,

 5. Das **Erstellen von Dienst Fabric Cluster** Blade weist die folgenden vier Schritte aus.

#### <a name="1-basics"></a>1. Grundlagen von

![Screenshot zum Erstellen einer neuen Ressourcengruppe.][CreateRG]

Klicken Sie in die Grundlagen Blade müssen Sie die grundlegenden Details für Ihren Cluster bereitstellen.

 1. Geben Sie den Namen der Cluster aus.

 2. Geben Sie einen **Benutzernamen** und **ein Kennwort** für den virtuellen Computern für Remotedesktop.

 3. Vergewissern Sie sich das **Abonnement** auswählen, die Ihre Cluster, bereitgestellt werden sollen, besonders, wenn Sie mehrere Abonnements haben.

 4. Erstellen einer **neuen Ressourcengruppe**an. Es empfiehlt sich, dieser denselben Namen wie dem Cluster, da es hilft bei der Suche nach ihnen später, insbesondere dann, wenn Sie versuchen, eine Bereitstellung ändern oder löschen Ihren Cluster zu verleihen.

    >[AZURE.NOTE] Obwohl Sie eine vorhandene Ressourcengruppe verwenden möchten können, ist es empfiehlt sich das Erstellen eine neuen Ressourcengruppe aus. Dies erleichtert die Cluster löschen, die Sie nicht benötigen.

 5. Wählen Sie die **Region** , in dem Sie Cluster erstellen möchten. Sie müssen den gleichen Bereich verwenden, dem Ihrem Tresor Schlüssel ist.

#### <a name="2-cluster-configuration"></a>2. Konfiguration von cluster

![Erstellen Sie einen Knotentyp][CreateNodeType]

Konfigurieren der Clusterknoten. Knotentypen definieren, die Größen virtueller Computer, die Anzahl der virtuellen Computern und deren Eigenschaften. Cluster kann verfügen über mehrere Knotentyp muss, aber der Typ der primären Knoten (der ersten Phase, die Sie auf das Portal definieren) mindestens fünf virtuellen Computern, wie dies den Knotentyp ist der Dienst Fabric System Services platziert sind. Konfigurieren Sie **Platzierungseigenschaften** nicht, da eine Standardeigenschaft für die Platzierung von "NodeTypeName" automatisch hinzugefügt wird.

   >[AZURE.NOTE] Ein gängiges Szenario für mehrere Typen von Knoten ist eine Anwendung, die eine Front-End-Dienst und einer Back-End-Dienst enthält. Soll den Front-End-Dienst auf kleinere virtuellen Computern (virtueller Computer Größen wie D2) mit geöffneten Ports mit dem Internet, aber den Back-End-Dienst auf größeren virtueller Computer (mit virtueller Computer Größen wie D4, D6, D15 usw.) mit keine Internet zugänglichen Ports geöffnet werden sollen.

 1. Wählen Sie einen Namen für den Knoten ein (1 bis 12 Zeichen, die nur Buchstaben und Zahlen enthält) aus.

 2. Die minimale **Größe** des virtuellen Computern für den Typ des primären Knoten wird durch die Ebene **Zuverlässigkeit** gesteuert, die Sie für den Cluster auswählen. Die Standardeinstellung für die Leiste Zuverlässigkeit ist Bronze. Weitere Informationen zum Zuverlässigkeit, erfahren Sie, [wie der Dienst Fabric Cluster Zuverlässigkeit und Zuverlässigkeit auswählen][service-fabric-cluster-capacity].

 3. Wählen Sie die virtueller Speicher und die Preisgestaltung Ebene an. D-Serie virtuellen Computern haben SSD Laufwerke und sind für Applikationen dynamische dringend empfohlen. Verwenden Sie einer anderen VM SKU, die teilweise Kerne besitzt oder haben Sie kleiner als 7 GB verfügbarer Kapazität nicht. 

 4. Die minimale **Anzahl** von virtuellen Computern für den Typ des primären Knoten wird durch die Ebene **Zuverlässigkeit** gesteuert ausgewählt haben. Die Standardeinstellung für die Leiste Zuverlässigkeit ist Silber. Weitere Informationen zur Zuverlässigkeit finden Sie unter [So wählen Sie den Dienst Fabric Cluster Zuverlässigkeit und Zuverlässigkeit][service-fabric-cluster-capacity].

 5. Wählen Sie die Anzahl von virtuellen Computern für den Knoten ein. Sie können nach oben oder unten die Anzahl der virtuelle Computer in ein anderes Knoten höher skalieren, aber auf dem primären Knoten, die mindestens über die Zuverlässigkeit Ebene, die Sie ausgewählt haben gesteuert. Andere Knoten können mindestens 1 virtueller Computer aufweisen.

 6. Konfigurieren Sie benutzerdefinierte Endpunkte. In diesem Feld können Sie eine kommagetrennte Liste der Ports eingeben, die Sie über den Lastenausgleich Azure mit dem öffentlichen Internet für die Anwendung verfügbar machen möchten. Wenn Sie eine Webanwendung zum Cluster bereitstellen möchten, geben Sie zum Beispiel "80" hier um den Datenverkehr auf Port 80 in Ihren Cluster zu ermöglichen. Weitere Informationen über Endpunkte finden Sie unter [Kommunikation mit Anwendungen][service-fabric-connect-and-communicate-with-services]

 7. Konfigurieren von Cluster- **Diagnose**. Standardmäßig werden die Diagnose zum Behandeln von Problemen unterstützen auf Ihren Cluster aktiviert. Wenn Sie Diagnose Ändern der **Status** Schalter zu **Deaktivieren**deaktivieren möchten. Deaktivieren der Diagnose wird **nicht** empfohlen.

 9. Wählen Sie das Upgrade Architekturmodus auf Ihren Cluster festgelegt werden soll. Wählen Sie **Automatische**, wenn Sie möchten, dass das System automatisch wählen Sie die neueste Version, und versuchen Sie, Ihre Cluster zu aktualisieren. Legen Sie den Modus auf **manuell**, wenn Sie eine unterstützte Version auswählen möchten.

>[AZURE.NOTE] Wir unterstützt nur für Cluster, die eine unterstützte Version des Diensts Fabric ausführen. Nach Auswahl des **manuellen** -Modus, abzubrechen Sie auf die Zuständigkeit, Ihren Cluster auf eine unterstützte Version aktualisieren. Für weitere Details der Textur Modus finden Sie unter upgrade der [dienstupgrade-Fabric-Cluster Dokument.][service-fabric-cluster-upgrade]


#### <a name="3-security"></a>3. Sicherheit

![Screenshot der Sicherheitskonfigurationen Azure-Portal.][SecurityConfigs]

Der letzte Schritt stellen Zertifikatinformationen zum Sichern des Clusters mithilfe der Taste Tresor und das Zertifikat, die zuvor erstellten Informationen bereit.

 
- Füllen Sie die Felder des primären Zertifikats mit der Ausgabe vom **Cluster Zertifikat** bei der Verwendung von Key Tresor Hochladen der `Invoke-AddCertToKeyVault` PowerShell-Befehl.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

- Aktivieren Sie das **Erweiterte Einstellungen konfigurieren** , um Client-Zertifikate für **Administrator-Client** und **schreibgeschützt Client**einzugeben. Geben Sie in diesen Feldern der Fingerabdruck von Ihrem Administrator-Client-Zertifikat und den Fingerabdruck des Zertifikats schreibgeschützt Benutzer Client, falls zutreffend. Wenn Administratoren versuchen, die zum Cluster herstellen können, werden diese erteilt Zugriff nur, wenn sie ein Zertifikat mit einem Fingerabdruck verfügen, die die Werte Fingerabdruck entspricht hier eingetragen.  


#### <a name="4-summary"></a>4. Zusammenfassung

![Screenshot der Start-Karte anzeigen "Bereitstellen von Dienst Fabric Cluster". ][Notifications]

Um die Clustererstellung abgeschlossen haben, klicken Sie auf **Zusammenfassung** , um die Konfigurationen anzuzeigen, die Sie angegeben haben, oder Herunterladen Sie die Ressourcenmanager Azure Vorlage, die Ihren Cluster bereitstellen zum. Nachdem Sie die obligatorischen Einstellungen bereitgestellt haben, wird die Schaltfläche **OK** wird grün, und Sie können den Erstellungsprozess Cluster beginnen, indem Sie darauf.

Sie können den Status der Erstellung in der Benachrichtigungen anzeigen. (Klicken Sie auf das "Wort Glocke"-Symbol in der Nähe der Statusleiste in der oberen rechten Ecke Ihres Bildschirms). Wenn Sie **an Startboard anheften** beim Erstellen des Clusters geklickt haben, sehen Sie **Bereitstellen von Dienst Fabric Cluster** angehefteten an die Karte **zu starten** .

### <a name="view-your-cluster-status"></a>Anzeigen von Ihren Cluster status

![Screenshot der Cluster Details im Dashboard.][ClusterDashboard]

Nachdem Ihre Cluster erstellt wurde, können Sie Ihre Cluster im Portal prüfen:

 1. Wechseln Sie zu **Navigieren** , und klicken Sie auf **Dienst Fabric Cluster**.

 2. Suchen Sie Ihren Cluster, und klicken Sie darauf.

 3. Sie können nun die Details Ihrer Cluster im Dashboard, einschließlich des Cluster öffentlichen Endpunkt und einen Link zum Dienst Fabric Explorer anzeigen.

Im Abschnitt **Knoten Monitor** auf die Cluster Dashboard Blade gibt die Anzahl der virtuellen Computern, die sind fehlerfrei und nicht fehlerfrei an. Finden Sie weitere Details zu den Cluster Gesundheit am [Dienst Fabric Gesundheit Modell Einleitung][service-fabric-health-introduction].

>[AZURE.NOTE] Dienst Fabric Cluster erfordern eine bestimmte Anzahl von Knoten werden immer von Verfügbarkeit und Beibehalten des Status - als "Quorum verwalten" bezeichnet. Wodurch, es ist in der Regel nicht zu allen Computern im Cluster beenden, es sei denn, Sie zuerst eine [vollständige Sicherung Ihres]durchgeführt haben[service-fabric-reliable-services-backup-restore].

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Schließen Sie Remote an eine Instanz virtuellen Computern Skalierung festlegen oder eines Knotens

Jede der NodeTypes Geben Sie in Ihrem Cluster führt zu einer virtuellen Computer Skalierung festlegen erste ansetzen. Finden Sie unter [Remote Herstellen einer Verbindung eine Instanz virtueller Computer Maßstab festlegen mit] [ remote-connect-to-a-vm-scale-set] Details.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt müssen Sie einen sicheren Cluster mithilfe von Zertifikaten für die Verwaltungsauthentifizierung. [Verbinden mit Ihren Cluster](service-fabric-connect-to-secure-cluster.md) weiter, und informieren Sie sich zum [Verwalten der Anwendung Kennwörter](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: https://manage.windowsazure.com
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
