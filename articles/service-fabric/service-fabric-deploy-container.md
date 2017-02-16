<properties
   pageTitle="Service Fabric und Bereitstellen von Containern | Microsoft Azure"
   description="Dienst Fabric und die Verwendung von Containern Bereitstellen von Applications Microservice. In diesem Artikel werden die Funktionen, die Dienst Fabric für Container enthält und wie Sie ein Windows-Container Bild in einem Cluster bereitstellen"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Vorschau: Bereitstellen eines Containers Windows mit Fabric Service

> [AZURE.SELECTOR]
- [Bereitstellen von Windows-Container](service-fabric-deploy-container.md)
- [Bereitstellen von Docker Container](service-fabric-deploy-container-linux.md)

In diesem Artikel führt Sie durch das Erstellen von Sammelartikeleinheit Diensten in Windows-Container. 

>[AZURE.NOTE] Dieses Feature ist in der Vorschau für Linux und für die Verwendung mit Windows Server 2016 derzeit nicht zur Verfügung. Dies wird in der Vorschau für Windows Server 2016 in die nächste Version von Fabric Service zur Verfügung und in zukünftigen Versionen unterstützte anschließend sein.

Dienst Fabric weist verschiedene Container-Funktionen, mit die Hilfe Sie mit der Erstellung von Microservices, die in Containern werden bestehen Applications. Diese werden Sammelartikeleinheit Services genannt. 

Die Funktionen umfassen.

- Container Image-Bereitstellung und-Aktivierung
- Ressource governance
- Repository-Authentifizierung
- Container Port Host Port-Zuordnung
- Containern, Suche und Kommunikation
- Möglichkeit zum Konfigurieren und Einrichten der Umgebungsvariablen

Können die einzelnen Funktionen, die wiederum eigenständig beim Packen Sammelartikeleinheit Diensts, in die Anwendung aufgenommen werden sollen.

## <a name="packaging-a-windows-container"></a>Verpacken eines Windows-Containers

Beim eines Containers packen, können Sie entweder mit einer Visual Studio-Projektvorlage oder [das Anwendungspaket Manuelles Erstellen](#manually)auswählen. Verwenden Visual Studio, werden Anwendung Paketstruktur und Manifestdateien durch den neuen Projektassistenten für Sie erstellt (Dies ist in die nächste Version in Kürze).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Visual Studio verwenden, um ein vorhandenes Container Bild Verpacken

>[AZURE.NOTE] In zukünftigen Versionen von Visual Studio SDK Tools werden Sie möglicherweise zum Hinzufügen eines Containers zur Anwendung auf ähnliche Weise, ausführbare Gast heute hinzufügen können. Finden Sie unter [Bereitstellen mit Service Fabric ausführbare Gast](service-fabric-deploy-existing-app.md) . Derzeit müssen Sie manuelle Verpacken führen Sie wie nachstehend beschrieben.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Manuelles Verpacken und Bereitstellen von eines Containers
Die Vorgehensweise zum Manuelles Verpacken eines Sammelartikeleinheit Diensts basiert auf die folgenden Schritte aus:

1. Der Container veröffentlicht auf Ihr Repository.
2. Erstellen Sie die Paket Directory-Struktur.
3. Bearbeiten der Dienstmanifestdatei an.
4. Bearbeiten Sie die Anwendungsmanifestdatei ein.

## <a name="container-image-deployment-and-activation"></a>Container Image-Bereitstellung und Aktivierung.
Im Fabric Service- [Anwendungsmodell](service-fabric-application-model.md)stellt einen Container für eine Anwendungshost, in dem mehrere Dienst Replikate platziert werden. Zum Bereitstellen und Aktivieren eines Containers, setzen Sie den Namen des Bilds Container in einer `ContainerHost` Element im Servicemanifests.

In Servicemanifests Hinzufügen einer `ContainerHost` zeigen Sie für den Eintrag, und legen Sie die `ImageName` den Namen der Container Repository und Bild sein. Das folgende teilweise Manifest zeigt ein Beispiel für die Bereitstellung von den Container aufgerufen *Myimage:v1* aus einem Repository *Myrepo* bezeichnet

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Sie können die Eingabe von Befehlen zu dem Bild Container bereitstellen, durch Angabe der optionalen `Commands` Element durch ein Komma getrennt Befehlssatz innerhalb des Containers ausführen. 

## <a name="resource-governance"></a>Ressource governance
Governance Ressource ist eine Funktion des Containers und beschränkt die Ressourcen, die der Container auf dem Host verwenden können. Die `ResourceGovernancePolicy`, im Anwendungsmanifest angegeben wird, bietet die Möglichkeit, eine Ressource Grenzwerte für ein Service-Paket Code deklarieren. Grenzwerte für Ressource können für festgelegt werden.

- Arbeitsspeicher
- MemorySwap
- CpuShares (CPU relativen Stärke)
- MemoryReservationInMB  
- BlkioWeight (BlockIO relativen Stärke). 

>[AZURE.NOTE] In zukünftigen Versionen für die Angabe von bestimmten TextBlock EA Grenzen, z. B. IOPs unterstützen Sie, und schreibgeschützt, dass/s und andere möglich ist.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Repository-Authentifizierung
Zum Herunterladen eines Containers müssen Sie möglicherweise für die Anmeldung an den Container Repository Anmeldeinformationen. Die Anmeldeinformationen, die im *Anwendungsmanifest* angegeben werden zur Angabe des Anmeldeinformationen oder SSH-Taste zum Herunterladen des Bilds Container aus dem Bild Repository verwendet.  Im folgenden Beispiel wird ein Konto namens *Testbenutzer* zusammen mit dem Kennwort in unverschlüsselter Form. Dies wird **nicht** empfohlen.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Das Kennwort und sollte mit einem Zertifikat, das auf dem Computer bereitgestellt, die verschlüsselt werden kann.

Im folgenden Beispiel wird ein Konto namens *Testbenutzer* mit dem Kennwort verschlüsselt mit einem Zertifikat *MyCert*bezeichnet. Sie können die `Invoke-ServiceFabricEncryptText` Powershell-Befehl zum Erstellen des geheimen verschlüsselte-Texts für das Kennwort ein. Wie finden Sie in diesem Artikel [Verwaltung von vertraulichen Daten in Clientanwendungen Dienst Fabric](service-fabric-application-secret-management.md) Details. Der private Schlüssel des Zertifikats an, das Kennwort entschlüsseln muss auf den lokalen Computer in einer Out-of-Band-Methode (in Azure über Cloud hier) bereitgestellt werden. Klicken Sie dann beim Dienst Fabric Service-Paket auf den Computer bereitstellt, ist es das Geheimnis entschlüsseln und zusammen mit den Namen des Kontos, mit dem Container Repository unter Verwendung dieser Anmeldeinformationen zu authentifizieren können.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Container Port Host Port-Zuordnung
Sie können einen Host Port verwendet zur Kommunikation mit dem Container durch Angeben von Konfigurieren eines `PortBinding` in Anwendungsmanifest. Die Port Bindung ordnet den Port, den der Dienst innerhalb des Containers auf einen Port auf dem Host überwacht.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Containern, Suche und Kommunikation
Verwenden der `PortBinding` Richtlinie können Sie einen Container Anschluss zum Zuordnen eines `Endpoint` in Servicemanifests wie im folgenden Beispiel dargestellt. Der Endpunkt `Endpoint1` kann ein fester Anschluss, beispielsweise port 80 oder angeben, kein Port überhaupt, in diesem Fall wird ein zufälliger Port aus den Zuordnungseinheiten für die Anwendung Portbereich für Sie ausgewählt.

Für Gast Container, Angeben einer `Endpoint` wie dies in der Servicemanifest ermöglicht Service-Struktur bis automatisch diesen Endpunkt zum Dienst Naming veröffentlichen, damit andere Dienste im Cluster dieser Container mithilfe der auflösen Dienst REST Abfragen ermitteln können. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Indem Sie mit dem Dienst Naming registrieren, möglich einfach Containern, Kommunikation im Code in Ihrem Container mithilfe des [reverse Proxy](service-fabric-reverseproxy.md)ist. Sie müssen lediglich bieten den überwachenden reverse Proxy http-Port und den Namen der Dienste, die Sie mit kommunizieren, indem Sie diese als Umgebungsvariablen festlegen möchten. Finden Sie im nächste Abschnitt erfahren Sie, wie Aktion aus.  

## <a name="configure-and-set-environment-variables"></a>Konfigurieren und Einrichten der Umgebungsvariablen
Umgebungsvariablen können angegebenen Foe jedes Paket Code im Dienst manifest für beide Dienste in Containern oder als Prozesse/Gast ausführbare Datei bereitgestellt werden. Diese Variablenwerten-Umgebung, die können speziell im Anwendungsmanifest überschrieben oder während der Bereitstellung als Anwendungsparameter angegeben werden.

Die folgenden Servicemanifest XML-Ausschnitt zeigt ein Beispiel so Umgebungsvariablen für ein Paket Code angeben. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Dieser Variablen können Ebene manifest der Anwendung überschrieben werden:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Im Beispiel oben haben wir für einen expliziten Wert angegeben der `HttpGateway` Umgebungsvariable (19000) während der Wert für `BackendServiceName` Parameter wird festgelegt, über die `[BackendSvc]` Anwendungsparameter. So können Sie den Wert für angeben `BackendServiceName`zum Zeitpunkt der Bereitstellung der Anwendung Wert und keinen festen Wert im Manifest. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Führen Sie die Beispiele für die Anwendung und Servicemanifest
So sieht ein Beispiel Anwendungsmanifest, die im Container Features zeigt.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Im folgenden Beispiel wird Servicemanifest (im vorhergehenden Anwendungsmanifest angegeben), die die Features Container anzeigt

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
