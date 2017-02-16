<properties
   pageTitle="Dienst Fabric-Endpunkte angeben | Microsoft Azure"
   description="So Endpunkt Ressourcen in einer Servicemanifest, einschließlich zum Einrichten von HTTPS Endpunkte beschreiben"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Angeben von Ressourcen in einer Servicemanifest

## <a name="overview"></a>(Übersicht)

Servicemanifests kann Ressourcen, die vom Dienst deklariert/geändert werden, ohne Ändern des kompilierten Codes verwendet werden. Azure Service-Struktur unterstützt die Konfiguration von Endpunkt Ressourcen für den Dienst an. Der Zugriff auf die Ressourcen, die in der Servicemanifest angegeben werden kann über die SecurityGroup im Anwendungsmanifest gesteuert werden. Die Deklaration von Ressourcen können diese Ressourcen zum Zeitpunkt der Bereitstellung, was bedeutet, dass der Dienst eine neue Konfiguration Funktion vorstellen muss nicht geändert werden soll. Die Schemadefinition für die Datei ServiceManifest.xml wird mit dem Dienst Fabric SDK und Tools *C:\Programme c:\Programme\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*installiert.

## <a name="endpoints"></a>Endpunkte

Wenn eine Ressource Endpunkt in Servicemanifests definiert ist, weist Dienst Fabric Ports aus dem Bereich der Anwendung reservierte Port Wenn ein Anschluss ausdrücklich angegeben nicht zur Verfügung. Betrachten Sie beispielsweise den Endpunkt *ServiceEndpoint1* angegeben, die in den Manifesten Codeausschnitt nach diesem Absatz angegeben sind. Darüber hinaus können Dienste auch einen bestimmten Port in einer Ressource anfordern. Dienst Replikate auf anderen Clusterknoten ausgeführte können verschiedene Portnummern zugeordnet werden, während Replikate von einem Dienst auf demselben Knoten den Port freigeben. Der Dienst Replikate können diese Ports klicken Sie dann je nach Bedarf für Replikation und Clientanfragen Abhören verwenden.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

[Konfigurieren von Stateful zuverlässigen Services](service-fabric-reliable-services-configuration.md) Systems anhand Hier finden Sie weitere Informationen zum Verweisen auf Endpunkte aus der Config paketeinstellungen (settings.xml) ablegen.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Beispiel: Angeben eines HTTP-Endpunkts des Diensts

Die folgenden Servicemanifest definiert werden, eine Ressource der TCP-Endpunkt und zwei HTTP-Endpunkt Ressourcen in der &lt;Ressourcen&gt; Element.

HTTP-Endpunkte werden automatisch an, dass ACL vom Dienst Fabric würden.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Beispiel: Angeben eines HTTPS-Endpunkts des Diensts

Das HTTPS-Protokoll bietet Serverauthentifizierung und auch für die Verschlüsselung der Kommunikation zwischen Client und Server verwendet wird. Wenn auf Ihrem Dienst Fabric Dienst HTTPS aktivieren möchten, geben Sie das Protokoll in der *Ressourcen -> Endpunkte-Endpunkt >* Abschnitt Service, wie zuvor für den Endpunkt *ServiceEndpoint3*dargestellt.

>[AZURE.NOTE] Protokoll des Diensts kann während der Aktualisierung der Anwendung geändert werden, ohne unlocked bildet einen abgeschnitten werden.


Hier ist ein Beispiel ApplicationManifest, die Sie für HTTPS festlegen müssen. Der Fingerabdruck für das Zertifikat muss bereitgestellt werden. Die EndpointRef ist ein Verweis auf EndpointResource in ServiceManifest, für die Sie das HTTPS-Protokoll festgelegt. Sie können mehrere EndpointCertificate hinzufügen.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
