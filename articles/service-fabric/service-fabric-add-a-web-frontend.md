<properties
   pageTitle="Erstellen Sie ein Web-front-End für eine Anwendung mit ASP.NET Core | Microsoft Azure"
   description="Verfügbar machen der Fabric Service-Anwendungs im Web mithilfe einer ASP.NET Core Web-API Project und zwischen reaktionsgruppendienst Kommunikation über ServiceProxy aus."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Erstellen Sie ein Web-Dienst front-End für eine Anwendung mit ASP.NET Core

Standardmäßig bieten Azure Service Fabric Services keine öffentliche Schnittstelle im Web. Um Ihre Anwendungsfunktionalität HTTP-Clients bereitzustellen, müssen Sie zum Erstellen eines Projekts dienen als Einstiegspunkt, und klicken Sie dann von dort aus auf Ihre einzelnen Dienste kommunizieren.

In diesem Lernprogramm wir wählen, wo wir in diesem Lernprogramm [Erstellen Ihrer ersten Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) unterbrochen und Hinzufügen von einem Webdienst vor dem Dienst dynamische Zähler. Wenn Sie dies nicht bereits getan haben, sollten Sie zurückgehen und dieses Lernprogramm zuerst durchgehen.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Hinzufügen eines ASP.NET Core-Diensts an Ihrer Anwendung

ASP.NET Core ist eine einfache, Plattformen Websiteentwicklung Framework, die Sie erstellen moderne Web-Benutzeroberfläche und web-APIs verwenden können. Lassen Sie uns ein Projekt ASP.NET Web API unseren vorhandenen Anwendung hinzufügen.

>[AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie [installieren .NET Core 1.0][dotnetcore-install].

1. Klicken Sie im Explorer-Lösung, mit der rechten Maustaste **Services** innerhalb des Anwendungsprojekts, und wählen Sie **Hinzufügen > neue Dienst Fabric Service**.

    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung][vs-add-new-service]

2. Klicken Sie auf der Seite **erstellen einen Dienst** **ASP.NET Core** wählen Sie aus, und geben sie einen Namen.

    ![ASP.NET-Webdienst auswählen im Dialogfeld neue Dienst][vs-new-service-dialog]

3. Die nächste Seite bietet eine Reihe von ASP.NET Core Project-Vorlagen. Beachten Sie, dass diese dieselben Vorlagen, die Sie anzeigen möchten, wenn Sie ein Projekt ASP.NET Core außerhalb einer Fabric Service-Anwendung erstellt haben. In diesem Lernprogramm wählen wir **Web-API**. Sie können jedoch die gleichen Konzepte anwenden, bis hin zum Erstellen einer vollständigen Webanwendung.

    ![ASP.NET Projekttyp auswählen][vs-new-aspnet-project-dialog]

    Nachdem Ihr Projekt Web-API erstellt wurde, müssen Sie zwei Dienste in Ihrer Anwendung. Wie Sie die Anwendung zu erstellen weiterhin, fügen Sie weitere Dienste auf genau die gleiche Weise. Jeder kann unabhängig Versionsnummern und aktualisierten sein.

>[AZURE.TIP] Weitere Informationen zum Erstellen von ASP.NET Core Services finden Sie in der [ASP.NET grundlegenden Dokumentation](https://docs.asp.net).

## <a name="run-the-application"></a>Führen Sie die Anwendung

Um erhalten einen Eindruck davon, was wir gemacht haben, lassen Sie uns die neue Anwendung bereitstellen und sehen Sie sich das Standardverhalten, das die Vorlage ASP.NET Core Web-API bereitstellt.

1. Drücken Sie F5 in Visual Studio für die app zu debuggen.

2. Visual Studio wird im Browser werden im Stammordner des Diensts ASP.NET Web API – eines Beitrags wie Http://localhost:33003 starten Sie nach Abschluss der Bereitstellung. Die Nummer des Ports wird zufällig zugewiesen und kann auf Ihrem Computer unterschiedlich sein. Die Vorlage ASP.NET Core Web-API verursachte keine Standardverhalten für die Quadratwurzel, damit Sie ein Fehler im Browser angezeigt werden.

3. Hinzufügen von `/api/values` an die Position im Browser. Dadurch wird aufgerufen der `Get` Methode für die ValuesController in der Web-API-Vorlage. Es gibt die Standardantwort zurück, die durch die Vorlage – ein JSON-Array bereitgestellt wird, die zwei Textzeichenfolgen enthält:

    ![ASP.NET Core Web-API Vorlage zurückgegeben Standardwerten][browser-aspnet-template-values]

    Am Ende des Lernprogramms werden wir diese Werte mit dem letzten Zählerwert enthält aus unserem dynamische Dienst ersetzt.


## <a name="connect-the-services"></a>Verbinden der Dienste

Dienst Fabric bietet vollständige Flexibilität in, wie Sie mit den Diensten von zuverlässigen kommunizieren. Innerhalb einer einzelnen Anwendung müssen Sie möglicherweise Dienste, die über TCP, andere Dienste, die über eine HTTP-REST-API zugänglich sind und weiterhin andere Dienste, die über das Web Sockets zugänglich sind zugegriffen werden. Klicken Sie auf die verfügbaren Optionen und die Nachteile Hintergrundinformationen finden Sie unter [Kommunikation mit den Diensten](service-fabric-connect-and-communicate-with-services.md). In diesem Lernprogramm wir führen Sie einen der Vorgehensweisen übersichtlich und verwenden Sie die `ServiceProxy` / `ServiceRemotingListener` Klassen, die im SDK bereitgestellt werden.

In der `ServiceProxy` Ansatz (erstellt in Remoteprozeduraufruf Anrufe oder RPC), definieren Sie eine Schnittstelle wie der öffentlichen Vertrag für den Dienst fungieren. Klicken Sie dann verwenden Sie die Benutzeroberfläche zum Generieren einer Proxyklasse für die Interaktion mit dem Dienst an.


### <a name="create-the-interface"></a>Erstellen der Benutzeroberfläche

Zunächst wird durch das Erstellen der Benutzeroberfläche als den Vertrag zwischen dem dynamische Dienst und seinen Clients, einschließlich des Projekts ASP.NET Core fungieren.

1. In Lösung Explorer mit der rechten Maustaste in Ihre Lösung, und wählen Sie **Hinzufügen** > **Neues Projekt**.

2. Wählen Sie im linken Navigationsbereich des **C#-** Eintrags aus, und wählen Sie die Vorlage **Class-Bibliothek** . Stellen Sie sicher, dass die .NET Framework-Version **4.5.2**festgelegt ist.

    ![Erstellen eines Benutzeroberflächen-Projekts des Diensts dynamische][vs-add-class-library-project]

3. In der Reihenfolge für eine Schnittstelle vom verwendet werden `ServiceProxy`, muss es von der Benutzeroberfläche IDienst abgeleitet werden. Diese Schnittstelle ist in eines der Dienst Fabric NuGet Pakete enthalten. Wenn Sie das Paket hinzufügen möchten, mit der rechten Maustaste in Ihre neue Projekts, und wählen Sie **NuGet-Pakete verwalten**aus.

4. Für das Paket **Microsoft.ServiceFabric.Services** suchen und zu installieren.

    ![Hinzufügen des Dienste NuGet-Pakets][vs-services-nuget-package]

5. Erstellen Sie eine Benutzeroberfläche in der Klassenbibliothek mit einer einzigen Methode, `GetCountAsync`, und erweitern Sie die Benutzeroberfläche von IDienst.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Implementieren der Benutzeroberfläche in Ihrem Dienst dynamische

Jetzt, da wir die Benutzeroberfläche definiert haben, müssen wir es im Dienst dynamische implementieren.

1. Fügen Sie einen Verweis auf die Bibliothek kursprojekt, die die Schnittstelle enthält, in Ihrem Dienst dynamische.

    ![Einen Verweis auf die Bibliothek kursprojekt hinzufügen dynamische Dienst][vs-add-class-library-reference]

2. Suchen Sie nach der Klasse, erbt `StatefulService`, wie `MyStatefulService`, und erweitern sie Implementieren der `ICounter` Schnittstelle.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Nun implementieren die einzelne Methode, die im definiert ist die `ICounter` Benutzeroberflächen, `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Verfügbar machen des dynamische Diensts mit einem Dienst Remote Zuhörer

Mit der `ICounter` Schnittstelle implementiert, der letzte Schritt beim Aktivieren des dynamische Diensts von anderen Diensten aufgerufen werden Channel für die Kommunikation geöffnet ist. Für dynamische Dienste Dienst Fabric bietet eine überschreibbare Methode mit dem Namen `CreateServiceReplicaListeners`. Bei dieser Methode können Sie angeben, einen oder mehrere Kommunikation Listener, basierend auf den Typ der Kommunikation, die Sie mit Ihrem Dienst aktivieren möchten.

>[AZURE.NOTE] Die entsprechende Methode zum Öffnen eines Kommunikation Channel um zustandsloser Dienste heißt `CreateServiceInstanceListeners`.

In diesem Fall ersetzen wir die vorhandene `CreateServiceReplicaListeners` Methode, und geben Sie eine Instanz von `ServiceRemotingListener`, wodurch RPC-Endpunkts erstellt kann von Clients mittels aufgerufen `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Verwenden Sie die ServiceProxy-Klasse für die Interaktion mit dem Dienst

Unsere dynamische Dienst kann nun den Datenverkehr von anderen Diensten zu erhalten. Daher müssen alle diese bleibt ist Hinzufügen des Codes zum Kommunizieren mit aus dem Webdienst ASP.NET.

1. Fügen Sie im Projekt ASP.NET einen Verweis auf die Klassenbibliothek, enthält die `ICounter` Schnittstelle.

2. Öffnen Sie im Menü **Erstellen** **Konfigurations-Manager**aus. Sie sollte nun wie folgt angezeigt:

    ![Konfigurations-Manager mit Class-Bibliothek als AnyCPU][vs-configuration-manager]

    Beachten Sie, dass die Bibliothek kursprojekt **MyStatefulService.Interface**, erstellen für jedes CPU konfiguriert ist. Wenn mit Service Fabric ordnungsgemäß arbeiten möchten, müssen sie explizit X64 ausgelegt werden. Klicken Sie auf den Dropdownpfeil Plattform wählen Sie **neu**aus, und erstellen Sie eine X64 Plattformkonfiguration.

    ![Erstellen neue Plattform für Class-Bibliothek][vs-create-platform]

3. Fügen Sie das Paket Microsoft.ServiceFabric.Services des Projekts ASP.NET einfach wie für die Bibliothek kursprojekt zuvor. Auf diese Weise können die `ServiceProxy` Class.

4. Öffnen Sie in den Ordner **Controller** der `ValuesController` Class. Beachten Sie, dass die `Get` Methode gibt aktuell nur ein Array hartcodierte Zeichenfolge "Wert1" und "Wert2" – die erfüllt, was wir zuvor im Browser gesehen haben. Ersetzen Sie diese Implementierung mit den folgenden Code ein:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    Die erste Zeile des Codes wird Key. Um die ICounter Proxy zum dynamische Dienst zu erstellen, müssen Sie zweierlei Informationen bereitstellen: eine Partitions-ID und den Namen des Diensts.

    Sie können mit Skalierung dynamische Services durch Unterteilen von ihren Status in verschiedenen Buckets auf Grundlage eines Schlüssels, das Sie, wie Kunden-ID oder Postleitzahl definieren Partitionierung. In unserer einfache Anwendung kann der dynamische Dienst nur eine Partition, damit die Taste keine Rolle spielt. Eine beliebige Taste, die Sie bereitstellen, führt auf dieselbe Partition. Weitere Informationen über das Aufteilen von Services, finden Sie unter [Partition zuverlässigen Dienst Fabric-Dienste](service-fabric-concepts-partitioning.md).

    Der Name des ist ein URI von der Textur Formular: /&lt;Application_name&gt;/&lt;Service_name&gt;.

    Mit diesen zweierlei Informationen können Dienst Fabric des Computers, dem Anfragen gesendet werden sollte eindeutig identifizieren. Die `ServiceProxy` Klasse auch nahtlos behandelt den Fall, in der Computer, der die dynamische Servicepartition hostet schlägt fehl, und einen anderen Computer muss höher gestuft werden, um seine Position zu schalten. Diese Abstraktion macht den Clientcode für den Umgang mit anderen Diensten erheblich einfacher zu schreiben.

    Sobald den Proxy vorliegt, rufen wir einfach die `GetCountAsync` Methode und das Ergebnis zurückgibt.

5. Drücken Sie erneut F5, damit die geänderte Anwendung ausführen. Wie startet zuvor Visual Studio im Browser werden im Stammordner des Webprojekts automatisch. Fügen Sie den Pfad "api/Values" sollte, und es des aktuelle Zähler Werts zurückgegeben.

    ![Der dynamische Zählerwert im Browser angezeigt][browser-aspnet-counter-value]

    Aktualisieren Sie den Browser regelmäßig, um den Zähler Wert aktualisieren.


>[AZURE.WARNING] ASP.NET Core Webserver zur Verfügung gestellt, in der Vorlage, die so genannte Kestrel, ist [für den Umgang mit direkten Internetverkehr derzeit nicht unterstützt](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Erwägen Sie zur Herstellung Szenarios Hostinganbieter ASP.NET Core Endpunkte hinter [-API Management] [ api-management-landing-page] oder ein anderes Internet zugänglichen Gateway. Beachten Sie, dass Dienst Fabric für Bereitstellung innerhalb von IIS nicht unterstützt wird.


## <a name="what-about-actors"></a>Wissenswertes zu Akteuren

In diesem Lernprogramm dienten hinzufügen ein Web-front-End, die mit einem dynamische Dienst kommuniziert werden müssen. Folgen Sie jedoch ein ähnelt dem Modell mit Akteuren sprechen. Tatsächlich ist es etwas einfacher.

Wenn Sie ein Akteur-Projekt erstellen, generiert Visual Studio ein Benutzeroberflächen-Projekt automatisch für Sie ein. Die Benutzeroberfläche können Sie einen Akteur Proxy im Web-Projekt zur Kommunikation mit den Akteur generieren. Der Kommunikationschannel wird automatisch bereitgestellt. Damit Sie nicht benötigen, nichts Unternehmen müssen, die zur Festlegung entspricht einem `ServiceRemotingListener` für den Dienst dynamische in diesem Lernprogramm verwendeten Verfahren.

## <a name="how-web-services-work-on-your-local-cluster"></a>Wie funktionieren Webdienste auf dem lokalen cluster

Im Allgemeinen können Sie genau die gleiche Fabric Service-Anwendung zu einer Umgebung mit mehreren Computern Cluster, den Sie auf Ihrem lokalen Cluster bereitgestellt bereitstellen und hochgradig sicher sein, dass es wie erwartet funktionieren. Dies ist, da der lokale Cluster einfach eine Konfiguration mit fünf Knoten ist, die auf einem einzigen Computer reduziert ist.

Wenn es sich um Webdienste geht, ist es jedoch eine Key Nuance. Wenn Ihre Cluster hinter einem Lastenausgleich befindet, wie in Azure bedeutet, müssen Sie sicherstellen, dass Ihre Webdienste auf jedem Computer bereitgestellt werden, da Lastenausgleich wird einfach Round Robert Verkehr über den Computer. Sie können dazu die `InstanceCount` für den Dienst, den speziellen Wert "-1".

Im Gegensatz dazu wird müssen Sie einen Webdienst lokal ausführen, um sicherzustellen, dass nur eine Instanz der Dienst ausgeführt. Andernfalls werden Sie Konflikte aus mehreren Prozessen auftreten, die auf den Pfad und den Port überwachen. Die Anzahl der Instanzen des Web-Dienst sollten daher "1" für lokale Bereitstellungen festgelegt werden.

Informationen zu andere Werten für andere Umgebung konfigurieren, finden Sie unter [Verwaltung Anwendungsparameter für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie einen Cluster in Azure für die Bereitstellung Ihrer Anwendungs in der cloud](service-fabric-cluster-creation-via-portal.md)
- [Weitere Informationen zum Kommunizieren mit den Diensten](service-fabric-connect-and-communicate-with-services.md)
- [Erfahren Sie mehr über das dynamische Services aufteilen](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
