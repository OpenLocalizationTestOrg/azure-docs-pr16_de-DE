<properties
    pageTitle="Was einen Cloud-Service-Modell und Paket ist | Microsoft Azure"
    description="Beschreibt die Cloud-Service-Modells (.csdef, .cscfg) und Azure-Paket (.cspkg)"
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

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Was ist die Cloud-Service-Modell, und wie Packen ich sie?
Cloud-Dienst wird von drei Komponenten, die Service-Definition _(.csdef)_, der Dienst Config _(.cscfg)_und eine Service-Paket _(.cspkg)_erstellt. Die **ServiceDefinition.csdef** und die **ServiceConfig.cscfg** Dateien werden XML-basierten und beschreiben die Struktur des Cloud-Dienst und wie sie konfiguriert ist; als bezeichnet das Modell aus. Die **ServicePackage.cspkg** ist eine Zip-Datei, die aus der **ServiceDefinition.csdef** und unter anderem generiert wird, alle erforderlichen Binärzahl-basierten Abhängigkeit enthält. Azure erstellt einen Clouddienst aus der **ServicePackage.cspkg** und der **ServiceConfig.cscfg**.

Nachdem Azure Cloud-Dienst ausgeführt wird, können Sie es über die Datei **ServiceConfig.cscfg** umkonfigurieren, aber Sie können die Definition nicht ändern.

## <a name="what-would-you-like-to-know-more-about"></a>Was möchten Sie mehr erfahren?

* Ich möchte Weitere Informationen zu den Dateien [ServiceDefinition.csdef](#csdef) und [ServiceConfig.cscfg](#cscfg) erhalten.
* Bieten Sie ich weiß bereits über Sie mir [einige Beispiele](#next-steps) für Was kann ich konfigurieren können.
* Ich möchte die [ServicePackage.cspkg](#cspkg)erstellen.
* Verwende ich Visual Studio, und ich möchte...
    * [Erstellen eines neuen Cloud-Diensts][vs_create]
    * [Konfigurieren Sie einen vorhandenen Clouddienst][vs_reconfigure]
    * [Bereitstellen eines Projekts Cloud-Dienst][vs_deploy]
    * [Remotedesktop in eine Instanz der Cloud-Dienst][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Die Datei **ServiceDefinition.csdef** gibt an, die Einstellungen, die von Azure zum Konfigurieren eines Cloud-Diensts verwendet werden. Das [Azure Service Definition Schema (.csdef Datei)](https://msdn.microsoft.com/library/azure/ee758711.aspx) stellt das zulässige Format für eine Definition Dienstdatei. Im folgenden Beispiel wird die Einstellungen, die für das Web und Worker Rollen definiert werden können:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
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
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Sie können auf die [Service Definition Schema] [] für ein besseres Verständnis der hier verwendete XML-Schema verweisen, hier jedoch eine kurze Erläuterung der einiger Elemente:

**Websites**  
Enthält die Definitionen für Websites oder Web Applications, die in IIS7 gehostet werden.

**InputEndpoints**  
Enthält die Definitionen für Endpunkte, mit denen die Cloud-Dienst wenden Sie sich an.

**InternalEndpoints**  
Enthält die Definitionen für Endpunkte, die miteinander kommunizieren von Rolleninstanzen verwendet werden.

**ConfigurationSettings**  
Enthält die Einstellungsdefinitionen für Features für eine bestimmte Rolle.

**Zertifikate**  
Enthält die Definitionen für Zertifikate, die für eine Rolle erforderlich sind. Im vorhergehenden Beispiel zeigt ein Zertifikat, das für die Konfiguration von Azure verbinden verwendet wird.

**LocalResources**  
Enthält die Definitionen für lokale Speicherressourcen. Eine lokale Speicherressource ist ein reservierte Verzeichnis im Dateisystem des virtuellen Computers in der eine Instanz von einer Rolle ausgeführt wird.

**Importe**  
Enthält die Definitionen für importierte Module. Im vorhergehenden Beispiel zeigt die Module für Remote Desktop-Verbindung und Azure zu verbinden.

**Beim Start**  
Enthält Aufgaben, die ausgeführt werden, wenn die Rolle gestartet wird. Die Aufgaben werden in einer .cmd oder ausführbare Datei definiert.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Die Konfiguration der Einstellungen für Ihre Cloud-Dienst wird durch die Werte in der Datei **ServiceConfiguration.cscfg** bestimmt. Sie geben Sie die Anzahl der Instanzen, die Sie für jede Rolle in dieser Datei bereitstellen möchten. Die Werte für die Konfiguration Einstellungen, die Sie in der Definition Dienstdatei definiert werden die Konfiguration Dienstdatei hinzugefügt. Die Fingerabdrücke für alle Management Zertifikate, die Cloud-Dienst zugeordnet sind, werden auch zu der Datei hinzugefügt. Das [Azure Service Konfigurationsschema (.cscfg Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx) stellt das zulässige Format für eine Konfiguration Dienstdatei.

Konfigurationsdatei der Dienst ist nicht mit der Anwendung, die verpackt aber in Azure als separate Datei hochgeladen wird und wird verwendet, um die Cloud-Dienst konfigurieren. Sie können eine neue Konfiguration Dienstdatei hochladen, ohne erneutes Ihre Cloud-Dienst bereitstellen. Die Konfigurationswerte für den Clouddienst können geändert werden, während der Clouddienst ausgeführt wird. Im folgenden Beispiel wird die Konfiguration Einstellungen, die für das Web und Worker Rollen definiert werden können:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Sie können dem [Dienst Konfigurationsschema](https://msdn.microsoft.com/library/azure/ee758710.aspx) für ein besseres Verständnis des hier verwendeten XML-Schemas verweisen, es ist jedoch eine kurze Erläuterung der Elemente:

**Instanzen**  
Konfiguriert die Anzahl der Instanzen für die Rolle ausführen. Um zu verhindern, dass Ihre Cloud-Dienst zunehmend potenziell während des Upgrades nicht verfügbar, ist es wird empfohlen, dass Sie mehr als eine Instanz der Web zugängliche Rollen bereitstellen. Auf diese Weise, befolgen Sie die Richtlinien im [Azure berechnen Dienst Ebene Vertrag SERVICELEVEL](http://azure.microsoft.com/support/legal/sla/), die garantiert 99,95 % externe Konnektivität für Internet zugänglichen Rollen aus, wenn zwei oder mehr Rolleninstanzen für einen Dienst bereitgestellt werden.

**ConfigurationSettings**  
Konfiguriert die Einstellungen für die laufenden Instanzen für eine Rolle. Den Namen der `<Setting>` Elemente müssen die Einstellungsdefinitionen in der Definition Dienstdatei übereinstimmen.

**Zertifikate**  
Konfiguriert die Zertifikate, die vom Dienst verwendet werden. Im vorhergehenden Beispiel zeigt, wie das Zertifikat für das Modul RAS definiert. Der Wert für das Attribut *Fingerabdruck* muss mit der Fingerabdruck des Zertifikats festgelegt werden.

<p/>

 >[AZURE.NOTE] Mit einem Texteditor der Fingerabdruck für das Zertifikat in die Konfigurationsdatei hinzugefügt werden kann, oder der Wert auf die Registerkarte **Zertifikate** der Seite **Eigenschaften** der Rolle in Visual Studio hinzugefügt werden kann.



## <a name="defining-ports-for-role-instances"></a>Definieren von Ports für Rolleninstanzen
Azure kann nur über einen Einstiegspunkt zu einer Webrolle. Dies bedeutet, dass die gesamten Verkehr über eine IP-Adresse erfolgt. Sie können Ihre Websites, um einen Port freigeben, indem Sie die Kopfzeile Host, um die Anforderung an den richtigen Ort direkte konfigurieren konfigurieren. Sie können auch Ihre Programme mit bekannten Ports auf die IP-Adresse anhören konfigurieren.

Das folgende Beispiel zeigt die Konfiguration für eine Webrolle mit einer Website und Web-Anwendung. Die Website als den Eintrag Standardspeicherort auf Port 80 konfiguriert ist, und die Webanwendungen sind so konfiguriert, dass um Besprechungsanfragen von einer alternativen Host Kopfzeile zu erhalten, die "mail.mysite.cloudapp.net" aufgerufen wird.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Ändern der Konfiguration von einer Rolle
Sie können die Konfiguration von Ihrem Cloud-Dienst aktualisieren, während sie in Azure, läuft, ohne den Dienst offline schalten. Um Informationen zur Konfiguration zu ändern, können entweder eine neue Konfigurationsdatei hochladen oder bearbeiten Sie die Konfigurationsdatei an Ort und auf dem laufenden Dienst anwenden. Folgenden Änderungen können an der Konfiguration eines Diensts vorgenommen werden:

- **Ändern der Werte von Konfigurations-Einstellungen**  
Eine Konfiguration festlegen ändert, kann eine Instanz der Rolle beim auswählen, um die Änderung anzuwenden, während die Instanz online, oder wenn Sie die Instanz ordnungsgemäß Papierkorb, und wenden Sie die Änderung, während die Instanz offline ist.

- **Ändern der Suchtopologie Dienst der Rolleninstanzen**  
Suchtopologie Änderungen wirken sich nicht Instanzen ausgeführte werden, es sei denn, in dem eine Instanz entfernt wird. Alle verbleibende Instanzen müssen wiederverwendet werden im Allgemeinen nicht. Sie können jedoch freizugebenden Rolleninstanzen als Antwort auf eine Suchtopologie ändern.

- **Ändern den Fingerabdruck des Zertifikats**  
Sie können nur ein Zertifikat aktualisieren, wenn eine Instanz der Rolle im Offlinemodus befindet. Ist ein Zertifikat hinzugefügt, gelöscht oder geändert werden, während eine Instanz der Rolle online ist, schaltet Azure ordnungsgemäß die Instanz offline das Zertifikat aktualisieren und ihn wieder online schalten, nachdem die Änderung abgeschlossen ist.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Behandeln von Konfiguration Änderungen mit Service Runtime-Ereignisse
[Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) umfasst [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) Namespace, der Klassen bereitstellt, für die Interaktion mit der Azure-Umgebung, die vom Code in einer Instanz von einer Rolle ausführen. [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) -Klasse definiert die folgenden Ereignisse, die vor und nach der Änderung einer Konfiguration ausgelöst werden:

- **Ereignis [Ändern](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)**  
Dies tritt auf, bevor der Änderung der Konfiguration auf eine bestimmte Instanz einer Rolle, wodurch Sie die Möglichkeit der Instanzen Rolle ausführen, falls erforderlich angewendet wird.
- **[Geänderte](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) Ereignis**  
Tritt nach der Änderung der Konfiguration auf eine bestimmte Instanz einer Rolle angewendet wird.

> [AZURE.NOTE] Da Änderungen Zertifikat immer die Instanzen von einer Rolle offline zu schalten, wird kein RoleEnvironment.Changing oder RoleEnvironment.Changed-Ereignisse ausgelöst.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Um eine Anwendung als in Azure Cloud-Dienst bereitstellen zu können, müssen Sie zuerst die Anwendung in das entsprechende Format verpacken. Das Befehlszeile **CSPack** Tool (mit dem [Azure SDK](https://azure.microsoft.com/downloads/)installiert) können die Paketdatei als Alternative zum Visual Studio erstellen.

**CSPack** verwendet den Inhalt der Definition Dienstdatei und Konfiguration Dienstdatei der Inhalt des Pakets definiert. **CSPack** generiert eine Anwendung Paketdatei (.cspkg), die Sie mithilfe der [Azure-Portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy)zu Azure hochladen können. Standardmäßig das Paket heißt `[ServiceDefinitionFileName].cspkg`, aber Sie können mit einen anderen Namen angeben der `/out` Möglichkeit, **CSPack**.

**CSPack** befindet sich in der Regel am  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (unter Windows) steht zur Verfügung, indem Sie die **Eingabeaufforderung für Microsoft Azure** -Verknüpfung, die mit dem SDK installiert ist.  
>  
>Führen Sie das Programm CSPack.exe allein Dokumentation über alle möglichen Schalter und Befehle anzeigen.

<p />

>[AZURE.TIP]
Führen Sie Ihre Cloud-Dienst lokal im **Microsoft Azure-Serveremulator umfasst**, verwenden Sie die Option **/copyonly** , die diese Option die binären Dateien für die Anwendung zu einem Verzeichnislayout kopiert aus, die sie in der Serveremulator ausgeführt werden können.

### <a name="example-command-to-package-a-cloud-service"></a>Beispielbefehl Verpacken einen Cloud-Dienst
Im folgende Beispiel wird eine Anwendungspaket mit den Informationen für eine Webrolle erstellt. Der Befehl gibt die Definition Dienstdatei, Verzeichnis verwenden, in dem binäre Dateien gefunden werden können, sowie den Namen der Paketdatei.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Wenn die Anwendung einer Webrolle und der Rolle Worker enthält, wird mit dem folgende Befehl verwendet:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Wo sind die Variablen wie folgt definiert:

| Variable                  | Wert |
| ------------------------- | ----- |
| \[DirectoryName\]         | Im Unterverzeichnis im Stammverzeichnis des Projekts, das die Datei .csdef des Projekts Azure enthält.|
| \[ServiceDefinition\]     | Der Name der Definition-Dienstdatei. Standardmäßig wird diese Datei ServiceDefinition.csdef Namen.  |
| \[OutputFileName\]        | Den Namen für die generierten Paketdatei. Dies wird in der Regel auf den Namen der Anwendung festgelegt. Wenn kein Dateiname angegeben ist, wird das Anwendungspaket als erstellt \[ApplicationName\].cspkg.|
| \[RoleName\]              | Der Name der Rolle definierten in der Definition Dienstdatei.|
| \[RoleBinariesDirectory] | Die Position der binären Dateien für die Rolle.|
| \[VirtualPath\]           | Die physische Verzeichnisse für jeden virtuellen Pfad im Abschnitt Websites der Dienstdefinition definiert.|
| \[PhysicalPath\]          | Die physische Verzeichnisse des Inhalts für jedes virtuellen Pfad in den Websiteknoten der Dienstdefinition definiert.|
| \[RoleAssemblyName\]      | Der Name der Binärdatei für die Rolle.| 


## <a name="next-steps"></a>Nächste Schritte

Ich bin ein Cloud-Service-Paket erstellen, und ich möchte...

* [Einrichten von Remotedesktop für eine Instanz der Cloud-Dienst][remotedesktop]
* [Bereitstellen eines Projekts Cloud-Dienst][deploy]

Verwende ich Visual Studio, und ich möchte...

* [Erstellen eines neuen Cloud-Diensts][vs_create]
* [Konfigurieren Sie einen vorhandenen Clouddienst][vs_reconfigure]
* [Bereitstellen eines Projekts Cloud-Dienst][vs_deploy]
* [Einrichten von Remotedesktop für eine Instanz der Cloud-Dienst][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
