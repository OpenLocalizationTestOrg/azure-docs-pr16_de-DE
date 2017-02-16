<properties
   pageTitle="Grundlegendes zu Fabric Service-Anwendung und Richtlinien für Sicherheit | Microsoft Azure"
   description="Übersicht über die zum Ausführen einer Fabric Service-Anwendung unter System und lokale Sicherheitskonten, einschließlich des SetupEntry Punkts, muss eine Anwendung bestimmte Berechtigungen Aktionen ausführen, bevor er beginnt"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Konfigurieren von Sicherheitsrichtlinien für die für eine Anwendung
Azure Service-Struktur bietet die Möglichkeit, die Anwendung, die im Cluster unter verschiedenen Benutzerkonten ausgeführt. Dienst Fabric sichert auch die von den Clientanwendungen zum Zeitpunkt der Bereitstellung unter dem Benutzerkonto wie Dateien, Verzeichnisse und Zertifikate verwendeten Ressourcen auf. Dadurch wird ausgeführt Applications, auch in einer freigegebenen gehosteten Umgebung secure voneinander. 

Führen Sie standardmäßig Dienst Fabric Applikationen unter dem Konto an, dem unter der Fabric.exe Prozess ausgeführt wird. Dienst Fabric bietet die Möglichkeit zum Ausführen von Applications unter einem lokalen Benutzerkonto oder eine lokale Systemkonto an, in dem Anwendungsmanifest angegeben. Unterstützte lokales System Kontotypen für sind **LocalUser**, **Netzwerkdienst**, **Lokaler Dienst**und **LocalSystem**.

 Beim Dienst Fabric auf Windows Server in Ihrem Data Center mithilfe des eigenständigen Installationsprogramms ausführen, können Sie die Domänenkonten Active Directory (AD) verwenden.

Benutzergruppen können erstellt, damit Sie einen oder mehrere Benutzer auf die einzelnen Gruppen zusammen verwaltet werden hinzugefügt werden können und definiert werden. Dies ist nützlich, wenn mehrere Benutzer anderen Dienst Eintrag verweist vorhanden sind, und sie müssen bestimmte allgemeine Administratorrechte verfügen, die auf Ebene der Gruppe verfügbar sind.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Konfigurieren Sie die Richtlinie für den Dienst SetupEntryPoint

In der [Anwendungsmodell](service-fabric-application-model.md) beschriebenen ist die **SetupEntryPoint** einen berechtigten Einstiegspunkt, der mit den gleichen Anmeldeinformationen als Dienst Fabric (normalerweise das Konto *Netzwerkdienst* ) vor einem beliebigen anderen Einstiegspunkt ausgeführt wird. Die ausführbare Datei durch **Einstiegspunkt** angegeben ist normalerweise der langer Service-Host, damit den Diensthost ausführbare mit hohen Berechtigungen für längere Zeit ausführen müssen Sie keine getrennte Setup Einstiegspunkt Probleme. Die ausführbare Datei durch **Einstiegspunkt** angegeben wird ausgeführt, nach dem **SetupEntryPoint** erfolgreich beenden. Der resultierende Prozess überwacht und neu gestartet wurde, Anfangsbuchstaben erneut **SetupEntryPoint**, wenn es jemals beendet oder stürzt ab.

Im folgenden finden eine einfache Dienst Manifesten Beispiel mit der SetupEntryPoint und der wichtigste Einstiegspunkt für den Dienst.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Konfigurieren Sie die Richtlinie mit einem lokalen Konto

Nachdem Sie den Dienst, damit eine Setup Einstiegspunkt konfiguriert haben, können Sie die Sicherheitsberechtigungen ändern, die unter im Anwendungsmanifest ausgeführt wird. Das folgende Beispiel veranschaulicht, wie zur Konfiguration des Diensts unter Rechte des Benutzerkontos Administrator ausführen.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Erstellen Sie zuerst einen Abschnitt **Hauptbenutzer** mit einem Benutzernamen, z. B. SetupAdminUser ein. Dies zeigt an, dass der Benutzer ein Mitglied der Gruppe der Administratoren ist.

Konfigurieren Sie anschließend unter dem Abschnitt **ServiceManifestImport** einer Richtlinie, um diese Tilgungsanteile **SetupEntryPoint**anzuwenden. Dies weist Fabric Service, dass beim Ausführen der Datei **MySetup.bat** RunAs mit Administratorrechten sein soll. Vorausgesetzt, dass Sie haben *eine Richtlinie für den Einstiegspunkt angewendet* , der Code in **MyServiceHost.exe** ausgeführt wird, klicken Sie unter System **Netzwerkdienst** -Konto. Dies ist das Standardkonto, dem alle Felder und Optionen Dienst als ausgeführt werden.

Fügen Sie die Datei MySetup.bat wir nun mit dem Visual Studio zu der Administratorrechte zu testen. In Visual Studio mit der rechten Maustaste auf das Projekt-Dienst, und fügen Sie eine neue Datei mit MySetup.bat hinzu.

Als Nächstes stellen Sie sicher, dass die Datei MySetup.bat in das Service-Paket enthalten ist. Standardmäßig ist es nicht. Wählen Sie die Datei, mit der rechten Maustaste, um das Kontextmenü aufzurufen, und wählen Sie **Eigenschaften**aus. Das Eigenschaftendialogfeld sicher, dass **Kopieren in die Ausgabeverzeichnis** zu **Kopieren, wenn neuer**festgelegt ist. Finden Sie unter folgenden Screenshot.

![Visual Studio CopyToOutput für SetupEntryPoint Batchdatei][image1]

Jetzt öffnen Sie die Datei MySetup.bat, und fügen Sie die folgenden Befehle:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Als Nächstes erstellen und die Lösung zu einem Cluster lokale Entwicklung bereitstellen.  Nachdem der Dienst siehe Dienst Fabric Explorer gestartet wurde, können Sie sehen, dass die MySetup.bat auf eine von zwei Arten erfolgreich war. Öffnen Sie ein PowerShell-Eingabeaufforderung und den Typ:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Notieren Sie dann den Namen des Knotens, in dem der Dienst bereitgestellt wurde und Schritte im Dienst Fabric-Explorer, beispielsweise Knoten 2, aus. Als Nächstes navigieren Sie zu dem Ordner Anwendung Instanz Arbeit, um die Datei out.txt zu finden, die den Wert von **TestVariable**zeigt. Für Beispiel, wenn auf Knoten 2, dann können Sie diesen Dienst bereitgestellt wurde dieser Pfad für die **MyApplicationType**wechseln kann:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Konfigurieren Sie die Richtlinie mit lokalen Systemkonten
Häufig empfiehlt es sich zum Ausführen des Startskripts mit einem lokalen Systemkonto statt mit einem Administratorkonto wie vor dargestellt. Ausführen der RunAs funktioniert Richtlinie als Administratoren in der Regel nicht gut, da Computern der Benutzerkontensteuerung (UAC) standardmäßig aktiviert ist. In diesem Fall **wird empfohlen, führen Sie die SetupEntryPoint als lokales System anstelle eines lokalen Benutzer zur Administratorengruppe hinzugefügt**. Im folgenden Beispiel wird das Festlegen der SetupEntryPoint als lokales System ausgeführt.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Starten einer SetupEntryPoint PowerShell Befehle
Um den Punkt **SetupEntryPoint** PowerShell auszuführen, können Sie in einer Batchdatei **PowerShell.exe** ausführen, die in eine Datei PowerShell verweist. Fügen Sie zunächst eine PowerShell-Datei des Projekts Dienst, wie z. B. **MySetup.ps1**ein. Denken Sie daran, die Eigenschaft *Kopieren, wenn neuer* festlegen, dass die Datei auch in das Service-Paket enthalten ist. Im folgenden Beispiel wird eine einfache Batchdatei zum Starten einer PowerShell Datei mit der Bezeichnung MySetup.ps1, die eine System-Umgebungsvariable aufgerufen **TestVariable**festlegt.


MySetup.bat zum Starten der PowerShell-Datei.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Fügen Sie in der PowerShell-Datei um eine System-Umgebungsvariable festzulegen Folgendes ein:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Hinweis:** Standardmäßig bei der Ausführung der Batchdatei untersucht, die die Anwendungsordner mit dem Namen **arbeiten** , für die Dateien. In diesem Fall bei der Ausführung MySetup.bat wollen wir dies die MySetup.ps1 im gleichen Ordner zu ermitteln, der von der Anwendung **Code Paket** Ordner befindet. Um diesen Ordner zu ändern, wird den Arbeitsordner festgelegt, wie das folgende Beispiel zeigt.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Verwenden für lokales Debuggen Umleitung der Konsole
Manchmal ist es sinnvoll, die Ausgabe Console Ausführen eines Skripts für das Debuggen finden Sie unter. Um dies zu tun können Sie eine Console Redirection Richtlinie, die Ausgabe in eine Datei schreibt festlegen. Die DateiAusgabe bezieht sich auf die Anwendungsordner mit dem Namen **Log** auf dem Knoten, in dem die Anwendung bereitgestellt und ausführen (Siehe, wo Sie diese finden Sie im vorherigen Beispiel).

**Hinweis: nie** verwenden Sie die Console Redirection Richtlinie in eine Anwendung, die in der Herstellung bereitgestellt werden, da dies der Anwendungsfailover auswirken kann. **Nur** verwenden Sie diese Option für lokale Entwicklung und Debuggen ausgewählt.  

Im folgenden Beispiel wird die Umleitung Console mit einem FileRetentionCount Wert festlegen.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Wenn Sie nun die Datei MySetup.ps1 zum Schreiben eines Befehls **Echo** ändern, wird dies in der Ausgabedatei für das Debuggen geschrieben werden.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Nachdem Sie das Skript Debuggen haben, entfernen Sie sofort dieser Console Redirection Richtlinie**

## <a name="configure-policy-for-service-code-packages"></a>Konfigurieren Sie die Richtlinie für den Dienst Code-Paketen 
In den vorherigen Schritten wurde gezeigt, wie "runas" Richtlinie auf SetupEntryPoint anwenden. Schauen Sie sich etwas genauer in verschiedenen Hauptbenutzer erstellen, die als Dienstrichtlinien angewendet werden können.

### <a name="create-local-user-groups"></a>Erstellen von Gruppen für lokale Benutzer
Benutzergruppen definiert und erstellt haben, werden können, können Sie einen oder mehrere Benutzer zu einer Gruppe hinzugefügt werden. Dies ist besonders hilfreich, wenn mehrere Benutzer anderen Dienst Eintrag verweist vorhanden sind, und sie müssen bestimmte allgemeine Administratorrechte verfügen, die auf Ebene der Gruppe verfügbar sind. Im folgenden Beispiel wird eine lokale Gruppe namens **LocalAdminGroup** mit Administratorrechten an. Customer1 und Customer2, zwei Benutzer, die Mitglieder dieser Gruppe lokale vorgenommen wurden.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Erstellen von lokalen Benutzern
Sie können einen lokalen Benutzer erstellen, der zum Sichern von eines Diensts innerhalb der Anwendung verwendet werden können. Wenn ein anderes Konto **LocalUser** im Abschnitt Hauptbenutzer des Anwendungsmanifests angegeben ist, erstellt Dienst Fabric lokale Benutzerkonten auf Computern, in dem die Anwendung bereitgestellt wird. Standardmäßig müssen diese Konten nicht denselben Namen wie die in das Anwendungsmanifest (z. B. Customer3 im folgenden Beispiel). In diesem Fall werden dynamisch erstellt und zufällige Kennwörter haben.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Zuweisen von Richtlinien zu den Code Service-Paketen
Im Abschnitt **RunAsPolicy** für eine **ServiceManifestImport** gibt an, das Konto aus dem Abschnitt Hauptbenutzer, der zum Ausführen eines Pakets Code verwendet werden soll. Es ordnet Code Pakete aus dem Dienstmanifest auch Benutzerkonten im Abschnitt Hauptbenutzer. Können Sie angeben, für die Einrichtung oder Haupteintrag Punkte, oder Sie können angeben, `All` für beide übernehmen. Das folgende Beispiel zeigt verschiedene Richtlinien angewendet werden:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Wenn **EntryPointType** nicht angegeben ist, ist der Standardwert auf EntryPointType festgelegt = "Main". Durch die Angabe **SetupEntryPoint** eignet sich besonders, wenn der Installationsvorgang von bestimmter hoher Berechtigung unter einem Systemkonto ausgeführt werden soll. Der ist-Servicecode ausgeführt werden kann unter einem Konto mit Berechtigungen unteren.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Anwenden einer Standardrichtlinie auf alle Dienst Code Pakete
Im Abschnitt **DefaultRunAsPolicy** wird ein Standard-Benutzerkonto für alle Code Pakete angeben, die einem bestimmten **RunAsPolicy** definiert haben, nicht verwendet. Wenn die meisten der angegebenen Servicemanifest von einer Anwendung verwendeten Code Pakete unter demselben Benutzer ausgeführt werden müssen, kann die Anwendung nur eine RunAs Standardrichtlinie mit diesem Benutzerkonto definieren. Im folgende Beispiel gibt an, dass ein Paket Code keiner **RunAsPolicy** angegeben, das Paket Code unter der angegebenen im Abschnitt Hauptbenutzer **MyDefaultAccount** ausgeführt werden soll.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Verwenden eine Gruppe von Active Directory-Domäne oder einen Benutzer
Für den Dienst Fabric auf Windows Server mit dem eigenständigen Installationsprogramm installiert kann werden, ausgeführt werden den Dienst unter die Anmeldeinformationen für eine Active Directory (AD) Benutzer oder eine Gruppenkonto. Hinweis: Dies ist der Active Directory-lokalen innerhalb Ihrer Domäne und ist nicht mit Azure Active Directory (AAD). Mithilfe der Domänenbenutzer oder Gruppen können Sie dann andere Ressourcen in der Domäne (z. B. Dateifreigaben) zugreifen, die Berechtigungen erteilt wurden.

Im folgenden Beispiel wird einen AD-Benutzer aufgerufen *Testbenutzer* mit ihrer Domänenkennwort verschlüsselt mit einem Zertifikat *MyCert*bezeichnet. Sie können die `Invoke-ServiceFabricEncryptText` Powershell-Befehl zum Erstellen des geheimen verschlüsselte Texts. Wie finden Sie in diesem Artikel [Verwaltung von vertraulichen Daten in Clientanwendungen Dienst Fabric](service-fabric-application-secret-management.md) Details. Der private Schlüssel des Zertifikats an, das Kennwort entschlüsseln muss auf den lokalen Computer in einer Out-of-Band-Methode (in Azure über Ressourcenmanager hier) bereitgestellt werden. Klicken Sie dann beim Dienst Fabric Service-Paket auf den Computer bereitstellt, ist es das Geheimnis entschlüsseln und zusammen mit den Benutzernamen authentifizieren mit AD unter diese Anmeldeinformationen ausführen können.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Zuweisen von SecurityAccessPolicy für HTTP und HTTPS Endpunkte
Wenn Sie eine Richtlinie RunAs an einen Dienst anwenden und Servicemanifest Endpunkt Ressourcen mit dem HTTP-Protokoll deklariert, müssen Sie angeben, einen **SecurityAccessPolicy** , um sicherzustellen, dass die Ports, die diese Endpunkte zugeordnet sind ordnungsgemäß Access-Steuerelemente für die RunAs-Benutzerkonto, dem der Dienst ausgeführt, klicken Sie unter wird aufgeführt. Andernfalls **http** hat keinen Zugriff auf den Dienst, und Sie-Fehler für Anrufe aus dem Client abrufen. Das folgende Beispiel gilt für das Konto Customer3 einen Endpunkt aufgerufen **ServiceEndpointName**, es Vollzugriff zugewiesen.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Für den HTTPS-Endpunkt müssen Sie auch den Namen des Zertifikats an den Client zurückzugebenden anzugeben. Dies ist möglich, mithilfe von **EndpointBindingPolicy**, mit dem Zertifikat in einem Abschnitt Zertifikate im Anwendungsmanifest definiert.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Eine vollständige Anwendung Manifesten Beispiel
Das folgenden Anwendungsmanifest zeigt zahlreiche die verschiedenen Einstellungen:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* [Grundlagen des Datenmodells Anwendung](service-fabric-application-model.md)
* [Angeben von Ressourcen in einer Servicemanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
