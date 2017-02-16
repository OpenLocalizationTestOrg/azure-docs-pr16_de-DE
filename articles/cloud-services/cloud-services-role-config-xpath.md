<properties 
pageTitle="XPath-Blatts für Cloud Services-Rolle Config | Microsoft Azure" 
description="Die verschiedenen XPath-Einstellungen in der Cloud-Dienst Rolle Config können Sie um Einstellungen als eine Umgebungsvariable verfügbar zu machen." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Verfügbar machen Sie Rolle Konfiguration Einstellungen als Umgebungsvariable mit XPath

In der Cloud-Dienst Worker oder Web Rolle Definition Dienstdatei können Sie die Konfigurationswerte Runtime als Umgebungsvariablen verfügbar machen. Die folgenden XPath-Werte werden unterstützt (die API Werte entsprechen).

Diese XPath-Werte sind auch über die Bibliothek [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) verfügbar. 

## <a name="app-running-in-emulator"></a>App im Emulator ausgeführt

Gibt an, dass die app im Emulator ausgeführt wird.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Code  | Varianz X = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Bereitstellung-ID

Ruft die Bereitstellung-ID für die Instanz ab.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Code  | Var DeploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Rollen-ID 

Ruft die ID für die Instanz des aktuellen Rolle ab.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Code  | Varianz-Id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Domäne aktualisieren

Ruft die Domäne aktualisieren der Instanz an.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code  | Varianz Ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Fehlerstrukturanalyse-Domäne

Ruft die Fehlerstrukturanalyse-Domäne der Instanz an.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code  | Varianz fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Name der Rolle

Ermittelt den Rollennamen der Instanzen.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Code  | Varianz Rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Config-Einstellung

Der Wert der angegebenen Konfiguration Einstellung abgerufen.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Code  | Varianz Einstellung = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Lokaler Speicherpfad

Der Speicherpfad der lokalen für die Instanz abgerufen.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Code  | Var LocalResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Lokaler Speichergröße

Ruft die Größe eines lokalen Speicher für die Instanz ab.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Code  | Var LocalResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Endpunktprotokoll 

Ruft das Endpunktprotokoll für die Instanz ab.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Code  | Protokoll Var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. Protokoll; |

## <a name="endpoint-ip"></a>Endpunkt IP

Ruft den angegebenen Endpunkt IP-Adresse ein.

| Typ | Beispiel |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Code  | Adresse Var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Endpunktport 

Ruft den Endpunktport für die Instanz ab.

| Typ  | Beispiel |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Code  | Port Var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Port; |





## <a name="example"></a>Beispiel

Hier ist ein Beispiel für eine Worker-Rolle, die eine Startaufgabe, mit On erstellt `TestIsEmulated` zum Festlegen der [ @emulated XPath-Wert](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu der Datei [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Erstellen eines Pakets [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) an.

Aktivieren Sie [remote Desktop](cloud-services-role-enable-remote-desktop.md) für eine Rolle.
