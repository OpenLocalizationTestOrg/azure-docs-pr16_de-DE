<properties
   pageTitle="Bereitstellen eine Node.js-Anwendung, die MongoDB verwendet | Microsoft Azure"
   description="Exemplarische Vorgehensweise zum Packen von mehreren Gast ausführbare Dateien zu einem Azure Service Fabric Cluster bereitstellen"
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
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Bereitstellen von mehreren Gast ausführbare Dateien

In diesem Artikel werden die Packen und verteilen Sie mehrere Gast ausführbare Dateien mit Azure Service Fabric veranschaulicht. Finden Sie unter Erstellen und Bereitstellen von einem einzigen Fabric Service-Paket so [Gast ausführbare mit Fabric Dienst bereitstellen](service-fabric-deploy-existing-app.md).

Während diese exemplarische Vorgehensweise veranschaulicht, wie eine Anwendung mit einem Node.js front-End bereitstellen, MongoDB als Datenspeicher verwendet, können Sie die Schritte für eine Anwendung installieren, die auf eine andere Anwendung Abhängigkeiten aufweist.   

Visual Studio können Sie um das Anwendungspaket zu erstellen, das mehrere Gast-Programme enthält. Finden Sie unter [Visual Studio Verpacken eine vorhandene Anwendung verwenden](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Nachdem Sie die erste Gast ausführbare Datei hinzugefügt haben, klicken Sie mit der rechten Maustaste auf das Projekt für die Anwendung, und aktivieren Sie das **Hinzufügen neuer Dienst Fabric Service ->** das zweite ausführbare Gast-Projekt in der Lösung hinzufügen. Hinweis: Wenn Sie die Quelle in der Visual Studio-Projekt verknüpfen, wird die Visual Studio-Lösung erstellen sicherstellen, dass Ihrer Anwendungspaket mit den Änderungen in der Quelle auf dem neuesten Stand ist. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Manuelles Verpacken Sie die mehrere Gast ausführbare Anwendung
Alternativ können Sie manuell den ausführbare Gast packen. Für das manuelle Verpacken, wird in diesem Artikel unter [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool)zur Verfügung steht Fabric Service-Tool Verpackung verwendet.

### <a name="packaging-the-nodejs-application"></a>Verpacken der Node.js-Anwendungs
In diesem Artikel wird davon ausgegangen, dass Node.js auf den Knoten im Cluster Service Fabric nicht installiert ist. Daher müssen Sie Node.exe in das Stammverzeichnis der Anwendung Knoten vor dem Packen hinzufügen. Die Directory-Struktur der Anwendung Node.js (mit Express Web Framework und Jade Vorlage-Engine) sollte ähnlich der folgenden aussehen:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Im nächsten Schritt erstellen Sie eine Anwendungspaket für die Anwendung Node.js aus. Im folgenden Code wird ein Fabric Service-Anwendung-Paket, die Anwendung Node.js enthält.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Nachfolgend finden Sie eine Beschreibung der Parameter, die verwendet werden:

- **/ Source** zeigt auf das Verzeichnis der Anwendung, die verpackt werden soll.
- **/ target** definiert das Verzeichnis, in dem das Paket erstellt werden soll. Dieses Verzeichnis wurde aus dem Quellverzeichnis unterschiedlich sein.
- **/ appname** definiert den Namen der Anwendung von der vorhandenen Anwendung. Es ist wichtig zu verstehen, dass dies auf den Dienstnamen im Manifest und nicht auf den Namen der Dienst Fabric übersetzt.
- **/exe** definiert die ausführbare Datei, die Fabric-Dienst starten, in diesem Fall kenne ist `node.exe`.
- **NetShield** definiert das Argument, das zum Starten des Programms verwendet wird. Wie Node.js nicht installiert ist, muss Fabric-Dienst starten Sie den Webserver Node.js durch Ausführen `node.exe bin/www`.  `/ma:'bin/www'`weist das Tool Verpackung mit `bin/ma` als Argument für die node.exe.
- **/AppType** definiert Fabric Service-Namen für die Anwendung eingeben.

Wenn Sie zu dem Verzeichnis, die in den Parameter/target angegeben wurde suchen, sehen Sie sich, dass das Tool eine voll funktionsfähige Fabric Service-Paket erstellt hat, wie unten dargestellt:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Die generierten ServiceManifest.xml verfügt jetzt über einen Abschnitt, der wie der Webserver Node.js gestartet werden soll, beschreibt, wie im folgenden Codeausschnitt dargestellt:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
In diesem Beispiel werden der Webserver Node.js Port 3000, fragt, sodass Sie die Endpunktinformationen in der Datei ServiceManifest.xml wie unten dargestellt aktualisieren müssen.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Verpacken der Anwendungs MongoDB
Jetzt, da Sie die Anwendung Node.js verpackt haben, können Sie fortfahren und MongoDB verpacken. Wie bereits erwähnt, sind die Schritte, die Sie jetzt aufzurufen, nicht spezifisch Node.js und MongoDB. Tatsächlich gelten sie auf alle Programme, die als ein Fabric Service-Anwendung zusammengefasst werden sollen.  

Um MongoDB packen, soll um sicherzustellen, dass Sie Mongod.exe und Mongo.exe verpacken. Beide Binärdateien befinden sich der `bin` Verzeichnis von Ihrem MongoDB Installationsverzeichnis. Die Verzeichnisstruktur sieht ähnlich dem unten aus.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Dienst Fabric muss MongoDB mit einem Befehl ähnlich der folgenden gestartet, sodass Sie verwenden, müssen die `/ma` Parameter beim MongoDB packen.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Die Daten werden im Fall eines Fehlers Knoten nicht beibehalten wird, wenn Sie auf dem lokalen Verzeichnis des Knotens Datenverzeichnis MongoDB setzen. Sie sollten dauerhaften Speicher verwenden oder implementieren eine Replikatgruppe MongoDB um Datenverlust zu verhindern.  

In PowerShell oder der Befehlsshell führen wir das Verpackung-Tool mit den folgenden Parametern aus:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Damit Ihr Dienst Fabric Anwendungspaket MongoDB hinzugefügt haben, müssen Sie sicherstellen, dass die/Target Parameter Punkte in dem Verzeichnis, das die Anwendung bereits enthält zusammen mit der Anwendung Node.js manifest. Sie möchten sicherstellen, dass Sie den bestehenden Anwendungstyp für Namen verwenden.

Lassen Sie uns navigieren Sie zu dem Verzeichnis aus, und überprüfen Sie, was das Tool erstellt hat.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Wie Sie sehen können, das Tool einen neuen Ordner, MongoDB, dem Verzeichnis hinzugefügt, das die MongoDB Binärdateien enthält. Wenn Sie beim Öffnen der `ApplicationManifest.xml` Datei, können Sie sehen, dass das Paket jetzt die Anwendung Node.js und die MongoDB enthält. Der folgende Code zeigt den Inhalt des Anwendungsmanifests an.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Veröffentlichen die Anwendung
Der letzte Schritt darin ist, veröffentlichen die Anwendung auf dem lokalen Dienst Fabric Cluster mithilfe der PowerShell-Skripts unten:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Nachdem Sie die Anwendung auf dem lokalen Cluster erfolgreich veröffentlicht wird, können Sie die Anwendung Node.js auf den Port zugreifen, die wir in der Anwendung Node.js – beispielsweise Http://localhost:3000 Servicemanifests eingegeben haben.

In diesem Lernprogramm haben gezeigt, wie Sie ganz einfach zwei vorhandene Applikationen als eine Fabric Service-Anwendung verpacken. Sie haben auch gelernt, wie sie auf Dienst Fabric bereitstellen, damit es von einiger Features Dienst Fabric, z. B. hohe Verfügbarkeit und Integrität Systemintegration profitieren kann.

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zum Bereitstellen von Container mit [Service Fabric und Container (Übersicht)](service-fabric-containers-overview.md)
