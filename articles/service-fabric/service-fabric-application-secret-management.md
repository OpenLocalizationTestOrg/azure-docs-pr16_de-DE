
<properties
   pageTitle="Verwaltung von vertraulichen Daten in Clientanwendungen Dienst Fabric | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie gesichert geheimen Werte in einer Fabric Service-Anwendung."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="managing-secrets-in-service-fabric-applications"></a>Verwaltung von vertraulichen Daten in Clientanwendungen Fabric Service

Dieses Handbuch führt Sie durch die Schritte zum Verwalten von vertrauliche Informationen in einer Fabric Service-Anwendung. Kennwörter können, werden vertrauliche Informationen, wie z. B. Speicher Verbindungszeichenfolgen, Kennwörter oder anderen Werten, die nicht im nur-Text behandelt werden soll.

Dieses Handbuch verwendet Azure-Taste Tresor zum Verwalten von Tasten und Kennwörter an. *Verwenden von* vertrauliche Informationen in einer Anwendung ist jedoch Cloud-Plattform-unabhängige an, die in einem Cluster gehostet an einer beliebigen Stelle bereitgestellt werden zu ermöglichen. 

## <a name="overview"></a>(Übersicht)

Es wird empfohlen zum Verwalten der Konfiguration diensteinstellungen bis [Service Konfigurations-Paketen][config-package]. Konfigurationspaketen werden Versionsnummern und aktualisierbar bis verwaltete parallele Updates mit Dienststatus-Überprüfung und automatische Zurücksetzen. Dies ist bevorzugten an globale Konfiguration, wie die Wahrscheinlichkeit, dass eine globale Dienstausfall verringert. Verschlüsselte Kennwörter sind keine Ausnahmen. Dienst Fabric weist integrierte Funktionen zum Verschlüsseln und Entschlüsseln Werte in einer Konfiguration Settings.xml Paketdatei Zertifikat Verschlüsselung verwenden.

Das folgende Diagramm veranschaulicht den grundlegenden Fluss für geheimen Management in einer Fabric Service-Anwendung:

![Geheimnis Management (Übersicht)][overview]

Es gibt vier Hauptschritte in dieser Fluss aus:

 1. Ein Daten-Chiffrierung Zertifikat zu erhalten.
 2. Installieren Sie das Zertifikat in Ihren Cluster aus.
 3. Verschlüsseln Sie geheimen Werte ein, wenn Sie eine Anwendung mit dem Zertifikat bereitstellen, und fügen Sie diese in einem Dienst Settings.xml Konfigurationsdatei.
 4. Lesen einer verschlüsselten Werte außerhalb des Settings.xml durch Entschlüsseln mit demselben Chiffrierung Zertifikat. 

[Azure-Taste Tresor] [ key-vault-get-started] wird hier als sicherer Speicherort für die Feiertage und als eine Möglichkeit zum Aufrufen von Zertifikate installiert sind, klicken Sie auf Dienst Fabric Cluster in Azure verwendet werden. Wenn Sie nicht in Azure bereitstellen, müssen Sie nicht Schlüssel Tresor verwenden, um vertrauliche Informationen in Service Fabric Applications zu verwalten.

## <a name="data-encipherment-certificate"></a>Daten-Chiffrierung Zertifikat

Ein Daten-Chiffrierung Zertifikat ausschließlich für und Verschlüsselung von Konfigurationswerte in einem Dienst Settings.xml verwendet wird und nicht für die Authentifizierung verwendet wird. Das Zertifikat muss erfüllen:

 - Das Zertifikat muss einen privaten Schlüssel enthalten.
 - Das Zertifikat muss für Key Exchange, exportiert in eine Datei Personal Information Exchange (PFX-Datei) erstellt werden.
 - Die Verwendung des Zertifikats Key wird Server- oder Client-Authentifizierung nicht enthalten und muss Daten Chiffrierung (10) enthalten. 
 
 Beim Erstellen eines selbstsignierten Zertifikats mithilfe der PowerShell, beispielsweise die `KeyUsage` Kennzeichnung muss auf festgelegt sein `DataEncipherment`:

 ```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```


## <a name="install-the-certificate-in-your-cluster"></a>Installieren Sie das Zertifikat in Ihren cluster

Dieses Zertifikat muss auf jedem Knoten im Cluster installiert sein. Es wird zur Laufzeit in einem Dienst Settings.xml gespeicherte Werten Entschlüsseln verwendet werden. [So erstellen Sie einen Cluster mit Azure Ressourcenmanager] finden Sie unter[ service-fabric-cluster-creation-via-arm] für Anweisungen zum einrichten. 

## <a name="encrypt-application-secrets"></a>Verschlüsseln Sie die Anwendung Kennwörter

Der Dienst Fabric SDK besitzt integrierte geheime Funktionen zum Verschlüsseln und entschlüsseln. Geheimnis Werte können programmgesteuert in dienstleistungscode zum Zeitpunkt der integrierten verschlüsselt und dann entschlüsselt und gelesen werden. 

Der folgende PowerShell-Befehl wird so verschlüsseln Sie einen geheimen Schlüssel verwendet. Sie müssen dasselbe Chiffrierung Zertifikat verwenden, das in Ihrem Cluster verschlüsselten Text für geheimen Werte werden installiert ist:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Die resultierende Base-64-Zeichenfolge enthält sowohl die verschlüsselten geheimen Text als auch die Informationen über das Zertifikat, das zum Verschlüsseln verwendet wurde.  Die Base-64-codierte Zeichenfolge in einem Parameter in des Diensts Settings.xml Konfigurationsdatei mit eingefügt werden kann die `IsEncrypted` Attribut festlegen, um `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Fügen Sie Anwendung geheimen Daten in der Anwendungsinstanzen  

Bereitstellung auf verschiedenen Umgebungen sollte idealerweise als automatisierte wie möglich sein. Dies kann durch Durchführung geheimen Verschlüsselung in einer Umgebung erstellen und Bereitstellen von verschlüsselten Daten als Parameter beim Erstellen von Anwendungsinstanzen erfolgen.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Verwenden von überschreibbaren Parametern in Settings.xml

Konfigurationsdatei Settings.xml ermöglicht überschreibbar Parameter, die zum Zeitpunkt der Erstellung Anwendung bereitgestellt werden können. Verwenden der `MustOverride` Attribut statt Angabe eines Werts für einen Parameter:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Um Werte in Settings.xml aufzuheben, deklarieren Sie einen Parameter außer Kraft setzen, für den Dienst in ApplicationManifest.xml aus:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Der Wert kann nun als eine *Anwendungsparameter* angegeben werden, beim Erstellen einer Instanz der Anwendung. Erstellen eine Instanz der Anwendung kann Skripts erfasst werden mithilfe der PowerShell oder geschrieben in c# für eine einfache Integration in einem Prozess erstellen.

Mithilfe der PowerShell, für der Parameter bereitgestellt wird die `New-ServiceFabricApplication` Befehl als [Tabelle](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Mit c#, Anwendungsparameter werden angegeben einer `ApplicationDescription` als eine `NameValueCollection`:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a>Entschlüsseln Sie geheimen Daten aus dienstleistungscode

Services-Dienst Struktur unter Netzwerkdienst werden standardmäßig unter Windows ausgeführt und haben keinen Zugriff auf den Knoten ohne einige zusätzliche Setup installierten Zertifikate.

Beim verwenden ein Zertifikat einer Daten-Chiffrierung, müssen Sie stellen Sie sicher, dass Netzwerkdienst oder jeden Dienst-Benutzerkonto ausgeführt werden, klicken Sie unter verfügt über Zugriff auf das Zertifikat des privaten Schlüssel aus. Gewähren des Zugriffs für den Dienst automatisch, wenn Sie dazu konfigurieren behandelt Dienst Fabric. Diese Konfiguration kann in ApplicationManifest.xml durch die Definition von Benutzern und Sicherheitsrichtlinien für die Feiertage vorgenommen werden. Im folgenden Beispiel wird das Konto Netzwerkdienst Lesezugriff auf ein Zertifikat, das von seinem Fingerabdruck definierten angegeben:

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [AZURE.NOTE] Beim Kopieren eines Zertifikatfingerabdrucks aus das Zertifikat Store Snap-in unter Windows wird eine unsichtbare Zeichen am Anfang der Zeichenfolge Fingerabdruck platziert. Dieses Zeichen nicht sichtbare verursachen einen Fehler bei dem Versuch, suchen Sie nach Fingerabdruck ein Zertifikat, daher müssen Sie dieses zusätzliche Zeichen zu löschen.

### <a name="use-application-secrets-in-service-code"></a>Verwenden Sie Anwendung vertrauliche Informationen in dienstleistungscode

Ermöglicht die API für den Zugriff auf Werte in einem Konfigurationspaket Settings.xml einfach Entschlüsseln von Werten, die die `IsEncrypted` Attribut legen Sie auf `true`. Da der verschlüsselte Text Informationen über das Zertifikat für die Verschlüsselung enthält, müssen Sie nicht das Zertifikat manuell zu suchen. Das Zertifikat muss nur auf dem Knoten installiert werden, die der Dienst ausgeführt wird, klicken Sie auf. Rufen Sie einfach die `DecryptValue()` Methode zum Abrufen des Originalwert Geheimnis:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Ausführen von Programmen mit anderen sicherheitsgrupe Berechtigungen](service-fabric-application-runas-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
