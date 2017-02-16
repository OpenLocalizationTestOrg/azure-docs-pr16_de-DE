<properties 
    pageTitle="So erstellen Sie eine ILB ASE mit Azure Ressourcenmanager Vorlagen | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer internen laden Lastenausgleich ASE Azure Ressourcenmanager Vorlagen verwenden." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>So erstellen Sie eine ILB ASE Azure Ressourcenmanager Vorlagen verwenden

## <a name="overview"></a>(Übersicht) ##
App-Service-Umgebungen können mit eine interne virtuelle Netzwerk-Adresse anstelle eines öffentlichen VIP erstellt werden.  Diese interne Adresse wird durch eine interne Lastenausgleich (ILB) aufgerufen Azure-Komponente bereitgestellt.  Eine ILB ASE kann über das Azure-Portal erstellt werden.  Sie können auch mit Automatisierung über Azure Ressourcenmanager Vorlagen erstellt werden.  In diesem Artikel führt durch die Schritte und Syntax zum Erstellen einer ILB ASE mit Azure Ressourcenmanager Vorlagen erforderlich sind.

Automatisierung der Erstellung einer ILB ASE umfasst drei Schritte:
1. Zunächst wird die Basis ASE in einem virtuellen Netzwerk mithilfe der Adresse Lastenausgleich internen laden anstelle einer öffentlichen VIP erstellt.  Als Teil dieses Schritts wird die ILB ASE Stammdomänennamen zugewiesen.
2. Nachdem die ILB ASE erstellt wurde, wird ein SSL-Zertifikat hochgeladen.  
3. Das hochgeladene SSL-Zertifikat wird als deren "Standard" SSL-Zertifikat explizit die ILB ASE zugewiesen.  Diese SSL-Zertifikat wird zum SSL-Verkehr zu apps auf der ILB ASE verwendet werden, wenn der apps mit der gemeinsamen Stammdomäne adressiert sind, die die ASE (z. B. https://someapp.mycustomrootcomain.com) zugewiesen ist

## <a name="creating-the-base-ilb-ase"></a>Erstellen der Basis ILB ASE ##
Eine Azure Ressourcenmanager Beispielvorlage, und die zugeordneten Parameterdatei, stehen auf GitHub [hier][quickstartilbasecreate].

Die meisten Parameter in der Datei *azuredeploy.parameters.json* können erstellen sowohl ILB ASEs als auch ASEs an einem öffentlichen VIP gebunden werden.  Der Liste unter Anrufe out-Parameter der Inhalte Notiz wurden oder die sind eindeutige, eine ILB ASE zu erstellen:


- *InteralLoadBalancingMode*: In den meisten Fällen wird diese Option, um 3, die sowohl HTTP-/HTTPS-Datenverkehr auf Ports 80/443 und Daten oder Steuerelements Channel Ports bedeutet die überwacht werden vom FTP-Dienst auf den ASE festlegen an eine interne Adresse virtuelles Netzwerk zugeordnet ILB gebunden werden.  Wenn diese Eigenschaft stattdessen 2 festgelegt ist, meldet nur der FTP-Dienst an, dass Ports (Steuerelement und die Daten Kanäle) an eine Adresse ILB gebunden werden, während Sie der HTTP/HTTPS-Datenverkehr für die öffentliche VIP verbleibt.
-  *DnsSuffix*: dieser Parameter definiert die Quadratwurzel Standarddomäne, die die ASE zugewiesen werden soll.  In den öffentlichen Variation Azure-App-Verwaltungsdienst ist die Stamm-Standarddomäne für alle Web apps *azurewebsites.net*.  Jedoch, da ein ILB ASE intern virtuelle Netzwerk des Kunden verwendet wird, wird nicht es im öffentlichen Dienst Stamm Standarddomäne verwenden sinnvoll.  Eine ILB ASE sollte stattdessen eine Root Standarddomäne verfügen, die für die Verwendung innerhalb des Unternehmens interne virtuelle Netzwerk sinnvoll ist.  Beispielsweise möglicherweise ein hypothetische Contoso Unternehmen die Standarddomäne Stamm *internen contoso.com* für apps verwenden, die nur aufgelöst werden kann und innerhalb Contosos virtuelles Netzwerk verfügbar sein sollen. 
-  *IpSslAddressCount*: für diesen Parameter wird automatisch auf einen Wert von 0 in der Datei *azuredeploy.json* übernommen, da ILB ASEs nur eine einzelne ILB Adresse verfügen.  Es sind keine expliziten SSL-IP-Adressen für eine ASE ILB und daher der Pool SSL-IP-Adressen für eine ILB ASE muss auf NULL festgelegt werden, da andernfalls ein provisioning Fehler auftritt. 

Nachdem die Datei *azuredeploy.parameters.json* für eine ILB ASE ausgefüllt wurden, können die ILB ASE dann mithilfe des folgenden Powershell-Codeausschnitts erstellt werden.  Ändern Sie die Datei Pfade zu entsprechen, wo sich die Vorlagendateien Azure Ressourcenmanager auf Ihrem Computer befinden.  Vergessen Sie auch eigene Werte für das Azure Ressourcenmanager Bereitstellung Namen, und klicken Sie auf Ressourcengruppennamen angeben.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nach der Azure Ressourcenmanager wird Vorlage übermittelt, dass es die ILB ASE erstellt werden ein paar Stunden dauern wird.  Sobald die Erstellung abgeschlossen ist, wird die ILB ASE im Portal UX in der Liste der App-Service-Umgebungen für das Abonnement angezeigt, die die Bereitstellung ausgelöst wurde.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Hochladen und Konfigurieren des SSL-Zertifikats "Standard" ##

Nachdem die ILB ASE erstellt wurde, sollte ein SSL-Zertifikat der ASE zugeordnet werden, wie "Standard" SSL-Zertifikat für die SSL-Verbindungen mit apps verwenden.  Ausgehend vom hypothetische Unternehmen Contoso-Beispiel, wenn die ASE des DNS Standard ist Suffix *internen "contoso.com"*, und klicken Sie dann eine Verbindung mit *https://some-random-app.internal-contoso.com* erfordert ein SSL-Zertifikat, das für gültig ist **.internal-"contoso.com"*. 

Es gibt verschiedene Arten, um ein gültiges SSL-Zertifikat einschließlich internen Zertifizierungsstellen, erwerben ein Zertifikat aus einer externen Herausgeber und die Verwendung eines selbstsignierten Zertifikats zu erhalten.  Unabhängig von der Quelle SSL-Zertifikat müssen die folgenden Zertifikatattribute ordnungsgemäß konfiguriert sein:

- *Betreff*: Dieses Attribut muss auf festgelegt sein **Ihren-Stamm-Domäne-here.com*
- *Subject Alternative Name*: Dieses Attribut darf enthalten beide * *Ihren-Stamm-Domäne-here.com*, und * *.scm.your-Stamm-Domäne-here.com*.  Der Grund für den zweiten Eintrag ist, dass SSL-Verbindungen auf die einzelnen app zugeordnet SCM/Kudu-Website mithilfe der Adresse im Formular *your-app-name.scm.your-root-domain-here.com*gemacht werden.

Mit einem gültigen in Hand SSL-Zertifikat sind zwei weitere vorbereitende Schritte erforderlich.  Das SSL-Zertifikat muss konvertiert/als PFX-Datei gespeichert werden.  Denken Sie daran, dass die PFX-Datei muss eingeschlossen werden alle mittlere und root-Zertifikate, und auch mit einem Kennwort geschützt werden muss.

Klicken Sie dann muss die resultierende PFX-Datei in eine base64-Zeichenfolge konvertiert werden, da das SSL-Zertifikat mithilfe einer Vorlage Ressourcenmanager Azure hochgeladen werden.  Da Azure Ressourcenmanager Vorlagen Textdateien sind, muss die PFX-Datei in eine base64-Zeichenfolge konvertiert, sodass es als Parameter der Vorlage enthalten sein kann.

Der Powershell-Codeausschnitt unten zeigt ein Beispiel für ein selbst signiertes Zertifikat generieren, exportieren das Zertifikat als PFX-Datei, konvertieren die PFX-Datei in eine base64-codierte Zeichenfolge und dann speichern base64-codierte Zeichenfolge in einer separaten Datei.  Der Powershell-Code für base64-Codierung aus der [Powershell Skripts Blog]angepasst wurde[examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Nachdem das SSL-Zertifikat wurde erfolgreich erstellt und Konvertierung in eine base64-codierte Zeichenfolge, die Ressourcenmanager Azure Beispielvorlage auf GitHub zum [Konfigurieren des Standard-SSL-Zertifikats] [ configuringDefaultSSLCertificate] verwendet werden kann.

Die Parameter in der Datei *azuredeploy.parameters.json* werden nachfolgend aufgeführt:

- *AppServiceEnvironmentName*: der Name des der ILB ASE konfiguriert wird.
- *ExistingAseLocation*: Zeichenfolge mit der Azure Region, in dem die ILB ASE bereitgestellt wurde.  Beispiel: "Süd zentralen uns".
- *PfxBlobString*: die based64-codierte Zeichenfolge Darstellung der PFX-Datei.  Mithilfe des weiter oben aufgeführten Codeausschnitts, möchten Sie die Zeichenfolge in "exportedcert.pfx.b64" kopieren und fügen Sie ihn als Wert für das Attribut *PfxBlobString* .
- *Kennwort*: das Kennwort verwendet, um die PFX-Datei sichern.
- *CertificateThumbprint*: der Fingerabdruck des Zertifikats.  Wenn Sie diesen Wert von Powershell abrufen (z. B. *$certificate. Fingerabdruck* aus früheren Codeausschnitts), können Sie den Wert als-ist.  Jedoch, wenn Sie den Wert aus der Windows-Dialogfeld ' Zertifikat ' kopieren, denken Sie daran, die überflüssigen Leerzeichen zu entfernen.  Die *CertificateThumbprint* sollte ungefähr wie folgt aussehen: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *CertificateName*: ein Zeichenfolgenbezeichner geeignet Ihrer Wahl verwendet, um die Identität des Zertifikats.  Der Name dient als Teil des eindeutigen Bezeichners Azure Ressourcenmanager für die *Microsoft.Web/certificates* Entität, die das SSL-Zertifikat darstellt.  Der Regionsname **muss** mit dem folgenden Suffix: \_YourASENameHere_InternalLoadBalancingASE.  Dieses Suffix wird vom Portal als Indikator verwendet, dass das Zertifikat zum Sichern von einer ILB aktiviert ASE verwendet wird.


Beispiel abgekürzten *azuredeploy.parameters.json* sieht folgendermaßen aus:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Nachdem die Datei *azuredeploy.parameters.json* ausgefüllt wurden, kann das Standard-SSL-Zertifikat mithilfe des folgenden Powershell-Codeausschnitts konfiguriert werden.  Ändern Sie die Datei Pfade zu entsprechen, wo sich die Vorlagendateien Azure Ressourcenmanager auf Ihrem Computer befinden.  Vergessen Sie auch eigene Werte für das Azure Ressourcenmanager Bereitstellung Namen, und klicken Sie auf Ressourcengruppennamen angeben.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nachdem Sie die Vorlage Azure Ressourcenmanager gesendet wird, dass es ungefähr 40 Minuten Minuten pro ASE Front-End-dauert, die Änderung zu übernehmen.  Beispielsweise mit einer Größe Standard-ASE mit zwei Front-enden, dauert die Vorlage ungefähr eine Stunde und zwanzig Minuten.  Während der Ausführung der Vorlage werden die ASE kann nicht skaliert.  

Nach Abschluss die Vorlage apps auf der ILB ASE über HTTPS zugegriffen werden können und die Verbindungen mit dem standardmäßigen SSL Zertifikat geschützt werden.  Das standardmäßige SSL-Zertifikat wird verwendet werden, wenn Sie apps auf der ILB ASE adressiert sind, verwenden eine Kombination aus dem Namen der Anwendung und der Standard-Hostname.  Beispielsweise würden *https://mycustomapp.internal-contoso.com* verwenden standardmäßig SSL-Zertifikat für **.internal-"contoso.com"*.

Allerdings genau wie apps für den öffentlichen mit mehreren Mandanten Dienst ausgeführt, können Entwickler auch benutzerdefinierte Hostnamen für einzelne apps konfigurieren, und klicken Sie dann konfigurieren eindeutige SNI SSL Zertifikat Bindungen für einzelne apps.  


## <a name="getting-started"></a>Erste Schritte

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Einführung in die App-Service-Umgebung](app-service-app-service-environment-intro.md)

Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
