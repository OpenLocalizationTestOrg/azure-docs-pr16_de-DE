<properties 
pageTitle="Aktivieren von Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services" 
description="So konfigurieren Sie Ihre Azure-Cloud-Service-Anwendung zu remote desktop-Verbindungen zulassen" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Aktivieren Sie Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services

>[AZURE.SELECTOR]
- [Azure klassischen-portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Remotedesktop können Sie auf den Desktop einer Rolle in Azure ausgeführt zugreifen. Eine Remote Desktop-Verbindung können zum Beheben und Diagnostizieren von Problemen mit der Anwendung während der Ausführung. 

Sie können eine Remotedesktop Verbindung während der Entwicklung Ihrer Rolle aktivieren, indem Sie einschließlich der Remotedesktop Module in der Definition Ihrer Service, oder Sie können auch den Remotedesktop durch die Remote Desktop-Erweiterung aktivieren. Die bevorzugte Methode besteht darin, die Erweiterung Remotedesktop verwenden, wie Sie Remotedesktop aktivieren können, selbst wenn die Anwendung bereitgestellt wird, ohne die Anwendung erneut bereitstellen. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurieren von Remotedesktop vom klassischen Azure-portal
Das Azure klassische Portal basiert auf dem Remote Desktop-Erweiterung Ansatz, damit Sie Remotedesktop aktivieren können, selbst wenn die Anwendung bereitgestellt wird. Die **Konfigurieren** der Seite für Ihre Cloud-Dienst können Sie Remotedesktop aktivieren, ändern das lokale Administratorkonto zum Verbinden mit den virtuellen Computern, die das Zertifikat in der Authentifizierung und das Ablaufdatum festlegen. 


1. Klicken Sie auf die **Cloud Services**, klicken Sie auf den Namen des Cloud-Dienst, und klicken Sie dann auf **Konfigurieren**.

2. Klicken Sie auf **Remote**.
    
    ![Remote-Cloud-Diensten](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Alle Rolleninstanzen werden gestartet, wenn Sie zuerst Remotedesktop aktivieren, und klicken Sie auf OK (Häkchen). Um einen Neustart zu verhindern, muss das Zertifikat zum Verschlüsseln des Kennworts für die Rolle installiert sein. Um zu verhindern, dass einen Neustart, [ein Zertifikat für die Cloud-Dienst hochladen](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) , und klicken Sie dann auf dieses Dialogfeld zurückzukehren.
    

3. **Rollen**wählen Sie die Rolle aus, die Sie aktualisieren oder **Alle** auszuwählen, für alle Rollen möchten.

4. Stellen Sie eine der folgenden Schritte aus:
    
    - Aktivieren Sie das Kontrollkästchen **Remotedesktop aktivieren** , um Remotedesktop zu aktivieren. Um Remote Desktop zu deaktivieren, deaktivieren Sie das Kontrollkästchen.
    
    - Erstellen Sie ein Konto in Remote Desktop-Verbindungen mit der Rolleninstanzen verwenden.
    
    - Aktualisieren Sie das Kennwort für das vorhandene Konto ein.
    
    - Wählen Sie eine hochgeladene Zertifikat für die Authentifizierung (Hochladen des Zertifikats mit **Hochladen** , auf der Seite **Zertifikate** ) verwenden, oder erstellen ein neues Zertifikat aus. 
    
    - Ändern Sie das Datum für die Konfiguration Remotedesktop.

5. Wenn Sie Ihre Aktualisierungen Konfiguration fertig sind, klicken Sie auf **OK** (Häkchen).


## <a name="remote-into-role-instances"></a>Remote in Rolleninstanzen
Nachdem Remotedesktop, klicken Sie auf die Rollen aktiviert ist können Sie Remote in eine Instanz der Rolle über verschiedene Tools zur Verfügung.

Verbindung zu einer Instanz der Rolle vom klassischen Azure-Portal:
    
  1.   Klicken Sie auf **Instanzen** , um die Seite **Instanzen** zu öffnen.
  2.   Wählen Sie eine Instanz der Rolle, die Remotedesktop konfiguriert hat.
  3.   Klicken Sie auf **Verbinden**, und folgen Sie den Anweisungen auf den Desktop öffnen. 
  4.   Klicken Sie auf **Öffnen** und dann auf **Verbinden** um die Remotedesktop-Verbindung zu starten. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Verwenden Sie Visual Studio zu remote in eine Instanz der Rolle

In Visual Studio Server-Explorer:

1. Erweitern Sie die **Azure\\Cloud Services\\[Name der Cloud-Dienst]** Knoten.
2. Erweitern Sie entweder **Staging** oder **Fertigung**.
3. Erweitern Sie die einzelne Rolle aus.
4. Mit der rechten Maustaste eine Rolle Instanzen, klicken Sie auf **Verbindung herstellen über Remote Desktop...**, und geben Sie den Benutzernamen und das Kennwort ein. 

![Server-Explorer Remotedesktop](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Mithilfe von PowerShell RDP-Datei
Das Cmdlet " [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) " können Sie die Datei RDP abrufen. Die RDP-Datei können mit Remote Desktop-Verbindung Sie den Cloud-Dienst zugreifen.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Die RDP-Datei über die Service Management REST-API programmgesteuert herunterladen
Die REST-Operation [RDP-Datei herunterladen](https://msdn.microsoft.com/library/jj157183.aspx) können Sie die RDP-Datei herunterladen. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>So konfigurieren Sie Remotedesktop in der Definition Dienstdatei

Diese Methode können Sie Remote Desktop für die Anwendung während der Entwicklung aktivieren. Bei dieser Vorgehensweise müssen verschlüsselte Kennwörter gespeichert werden in der Dienstkonfiguration Datei- und alle Updates für den remote desktop-Konfiguration müssten eine erneute Bereitstellung der Anwendung. Wenn Sie diese Nachteile zu vermeiden, die sollten Sie den oben beschriebenen remote desktop-basierte Erweiterung Ansatz verwenden, möchten.  

Sie können Visual Studio [aktivieren eine Remotedesktop-Verbindung](../vs-azure-tools-remote-desktop-roles.md) mit dem Dienst Definition Dateiansatz verwenden.  
Die Schritte beschrieben, wie die sich benötigt für die Service-Modell-Dateien mit Remotedesktop aktivieren. Visual Studio wird automatisch ändert diese beim Veröffentlichen.

### <a name="set-up-the-connection-in-the-service-model"></a>Einrichten der Verbindung in der Service-Modells 
Verwenden Sie das **Imports** Element, um das Modul **RAS** und das Modul **RemoteForwarder** zu der Datei [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) importieren.

Die Definition Dienstdatei sollten ähnlich wie im folgenden Beispiel wird mit der `<Imports>` Element hinzugefügt.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Die Datei [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) sollte ähnlich wie im folgenden Beispiel werden, beachten Sie die `<ConfigurationSettings>` und `<Certificates>` Elemente. Das angegebene Zertifikat muss [Cloud-Dienst geladen](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Zusätzliche Ressourcen

[So konfigurieren Sie Cloud Services](cloud-services-how-to-configure.md)