<properties 
pageTitle="Kommunikation für Rollen in der Cloud Services | Microsoft Azure" 
description="Rolleninstanzen in der Cloud Services können Endpunkte (http, Https, Tcp, Udp), die für sie definiert haben, die mit außerhalb oder zwischen anderen Rolleninstanzen kommunizieren." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Aktivieren der Kommunikation für Rolleninstanzen in azure

Cloud-Dienstverwaltungsrollen kommunizieren durch internen und externen Verbindungen. Externe Verbindungen werden **Eingabewerte Endpunkte** bezeichnet, während interne Verbindungen **internen Endpunkte**genannt werden. In diesem Thema beschrieben, wie die [Dienstdefinition](cloud-services-model-and-package.md#csdef) zum Erstellen von Endpunkten ändern.


## <a name="input-endpoint"></a>Eingabemethoden-Endpunkt
Der Eingabewerte Endpunkt wird verwendet, wenn Sie einen Port an eine Position außerhalb verfügbar machen möchten. Geben Sie den Typ des und den Port für den Endpunkt die dann für die internen und externen Ports für den Endpunkt angewendet wird. Wenn Sie möchten, können Sie einen anderen internen Port für den Endpunkt mit dem Attribut [LokalerAnschluss](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) angeben.

Der Eingabewerte Endpunkt können die folgenden Protokolle: **http, Https, Tcp und Udp**.

Um von außen liegenden Tabellenblättern zu erstellen, fügen Sie des **InputEndpoint** untergeordnetes Elements an die **Endpunkte** Bestandteil einer Web oder Arbeitskollegen Rolle aus.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Instanz von Endpunkt
Instanz von Endpunkten ähneln Eingabemethoden Endpunkte können Sie aber bestimmte öffentlich zugänglichen Ports für jede einzelne Rolleninstanz mithilfe von Port Weiterleitung auf dem Lastenausgleich zuordnen. Sie können einen einzelnen öffentlich zugänglichen Port oder einen Bereich von Ports angeben.

Der Instanz von Endpunkt kann nur **Tcp** oder **Udp** als Protokoll verwenden.

Um eine Instanz von Endpunkt zu erstellen, fügen Sie des **InstanceInputEndpoint** untergeordnetes Elements an die **Endpunkte** Bestandteil einer Web oder Arbeitskollegen Rolle aus.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Interner Endpunkt
Interne Endpunkte stehen für die Instanz-Instanz-Kommunikation. Der Port ist optional, und wenn nicht angegeben, wird an den Endpunkt ein dynamischer Port zugewiesen. Ein Portbereich kann verwendet werden. Es gibt maximal fünf interne Endpunkte pro Rolle aus.

Der interne Endpunkt kann die folgenden Protokolle verwenden: **http, Tcp, Udp, alle**.

Um einen internen Eingabewerte Endpunkt zu erstellen, fügen Sie des **InternalEndpoint** untergeordnetes Elements an die **Endpunkte** Bestandteil einer Web oder Arbeitskollegen Rolle aus.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Sie können auch einen Portbereich verwenden.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Worker-Rollen im Vergleich zu Webrollen

Es gibt einen geringfügigen Unterschied mit Endpunkten bei der Arbeit mit sowohl Arbeits- und Webrollen. Die Web-Rolle müssen mindestens einen einzelnen Eingabewerte Endpunkt über **http** .


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Verwenden von .NET SDK zum Zugriff von außen liegenden Tabellenblättern
Azure verwaltete Bibliothek bietet Methoden für die Rolleninstanzen zur Laufzeit kommunizieren. Vom Code innerhalb einer Instanz der Rolle ausführen können Sie Informationen über das Vorhandensein des anderen Rolleninstanzen und deren Endpunkte sowie Informationen zu der aktuellen Rolleninstanz abrufen.

> [AZURE.NOTE] Sie können nur Informationen zur Rolleninstanzen, die in der Cloud-Dienst ausgeführt werden und, die mindestens einen internen Endpunkt definieren, abrufen. Sie können keine Daten zu Rolleninstanzen in einem anderen Dienst ausgeführt beziehen.

Die Eigenschaft [Instanzen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) können zum Abrufen von Instanzen einer Rolle. Verwenden Sie zuerst die [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) , um einen Verweis auf die aktuelle Instanz der Rolle zurückzugeben, und verwenden Sie dann die Eigenschaft [Rolle](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) , um einen Verweis auf die Rolle selbst zurückzugeben.

Wenn Sie mit einer Instanz der Rolle programmgesteuert über .NET SDK verbinden, ist es relativ einfach, auf die Endpunktinformationen zugreifen. Nachdem Sie bereits mit einer bestimmten Rolle Umgebung verbunden haben, können Sie beispielsweise den Port für einen bestimmten Endpunkt mit diesem Code erhalten:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Die Eigenschaft **Instanzen** gibt eine Auflistung von Objekten **RoleInstance** . Diese Auflistung enthält immer die aktuelle Instanz. Wenn die Rolle einen internen Endpunkt nicht festgelegt wird, enthält die Auflistung, aber keine anderen Instanzen der aktuellen Instanz. Die Anzahl der Rolleninstanzen in der Auflistung werden immer 1 in den Fall, in dem kein internen Endpunkt für die Rolle definiert ist. Wenn die Rolle einen internen Endpunkt definiert, deren Instanzen werden zur Laufzeit aufgeführt, und die Anzahl der Instanzen in der Auflistung wird die Anzahl der Instanzen für die Rolle der Dienstdatei Konfiguration angegebenen entsprechen.

> [AZURE.NOTE] Die verwaltete Azure-Bibliothek bietet keine bestimmen, die Integrität des anderen Rolleninstanzen, aber diese Bewertungen Gesundheit selbst implementieren Wenn es sich bei Ihrem Dienst diese Funktionalität benötigt. [Azure-Diagnose](cloud-services-dotnet-diagnostics.md) können Sie um Informationen zum Ausführen von Rolleninstanzen zu erhalten.

Um die Port-Nummer für einen internen Endpunkt auf eine Instanz der Rolle zu bestimmen, können Sie die Eigenschaft [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) ein Wörterbuchobjekt zurück, die Endpunktnamen und die entsprechenden IP-Adressen und Ports enthält. Die Eigenschaft [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) gibt die IP-Adresse und den Port für einen angegebenen Endpunkt. Die Eigenschaft **PublicIPEndpoint** gibt den Port für einen Endpunkt Lastenausgleich. Der IP-Adressteil der **PublicIPEndpoint** -Eigenschaft wird nicht verwendet.

Hier ist ein Beispiel für die Rolleninstanzen durchläuft.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Hier ist ein Beispiel für eine Worker-Rolle, die erhält den Endpunkt verfügbar gemacht über die Dienstdefinition und für Verbindungen aus.

> [AZURE.WARNING] Dieser Code funktioniert nur für eine bereitgestellte Dienst. Wenn Sie in der Azure-Serveremulator ausführen, werden Service-Konfiguration-Elemente, die direkte Port Endpunkte (**InstanceInputEndpoint** Elemente) erstellen ignoriert.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Netzwerk Datenverkehr Regeln Rolle Kommunikation steuern
Nachdem Sie interne Endpunkte definiert haben, können Sie Netzwerk Datenverkehr Regeln hinzufügen (basierend auf die Endpunkte, die Sie erstellt haben) Steuerelement wie die Rolleninstanzen miteinander kommunizieren können. Das folgende Diagramm veranschaulicht einige häufige Szenarien für das Steuern der Rolle Kommunikation:

![Netzwerk Datenverkehr Regeln Szenarien] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Netzwerk Datenverkehr Regeln Szenarien")

Im folgenden Codebeispiel zeigt Rollendefinitionen für die Rollen in der vorherigen Abbildung gezeigt. Jede Rollendefinition enthält mindestens einen internen Endpunkt definiert:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Einschränkung der Kommunikation zwischen Rollen mit internen Endpunkten des beide feste und automatisch zugewiesene Ports ausgeführt werden sollen.

Standardmäßig Nachdem Sie ein interner Endpunkt definiert ist, kann Kommunikation von einer Rolle an den internen Endpunkt einer Rolle ohne Einschränkungen übertragen werden. Um Kommunikation einschränken möchten, müssen Sie ein Element **NetworkTrafficRules** auf das Element **ServiceDefinition** in der Definition Dienstdatei hinzufügen.

### <a name="scenario-1"></a>Szenario 1
Netzwerkverkehr wird nur von **WebRole1** an **WorkerRole1**zulassen.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Szenario 2
Netzwerkverkehr wird nur von **WebRole1** **WorkerRole1** und **WorkerRole2**ermöglicht.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Szenario 3
Netzwerkverkehr wird nur von **WebRole1** **WorkerRole1**und **WorkerRole1** zu **WorkerRole2**ermöglicht.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Szenario 4
Nur zulässt Netzwerkdatenverkehr aus **WebRole1** **WorkerRole1**, **WebRole1** , **WorkerRole2**und **WorkerRole1** zu **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Ein XML-Schema Bezug für die Elemente oben verwendeten finden Sie [hier](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum des Cloud Service- [Modell](cloud-services-model-and-package.md).