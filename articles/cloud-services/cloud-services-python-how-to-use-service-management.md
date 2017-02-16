<properties
    pageTitle="So verwenden Sie die Servicemanagement API (Python) – Übersicht der Leistungsmerkmale"
    description="Erfahren Sie, wie Sie allgemeine Aufgaben zur Verwaltung von Dienst programmgesteuert aus Python durchführen."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>So verwenden Sie Servicemanagement aus Python

> [AZURE.NOTE] Service Management-API wird mit der neuen Ressource Management-API, derzeit verfügbar in eine Vorabversion ersetzt.  [Ressourcenverwaltung Azure-Dokumentation](http://azure-sdk-for-python.readthedocs.org/) Weitere Informationen finden Sie mithilfe der neuen Ressource Management-API aus Python.

In diesem Handbuch wird gezeigt, wie allgemeine Aufgaben zur Verwaltung von Dienst programmgesteuert aus Python ausführen. Die **ServiceManagementService** -Klasse im [Azure SDK für Python](https://github.com/Azure/azure-sdk-for-python) unterstützt den programmgesteuerten Zugriff auf viele der Dienst verwaltungsbezogenen Funktionen, die im [Azure klassischen Portal] zur Verfügung[ management-portal] (z. B. **erstellen, aktualisieren, und Löschen von Cloud-Diensten, Bereitstellungen, Datendienste Management und virtuellen Computern**). Diese Funktion kann bei der Erstellung von Applications, die programmgesteuerten Zugriff auf Servicemanagement benötigen hilfreich sein.

## <a name="WhatIs"> </a>Neuigkeiten Servicemanagement
Der Dienst Management-API ermöglicht programmgesteuerten Zugriff auf viele die Service-Funktionalität über das [Azure klassischen Portal][management-portal]. Die Azure SDK für Python können Sie Ihre Clouddienste und Speicherkonten verwalten.

Wenn die Management-API verwenden möchten, müssen Sie [ein Azure-Konto erstellen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Konzepte
Die Azure SDK für Python umgebrochen wird der [Azure Service Management API][svc-mgmt-rest-api], welche eine REST-API ist. Alle Vorgänge der API über SSL ausgeführt werden und sich gegenseitig authentifiziert x. 509 v3-Zertifikate verwenden. Der Verwaltungsdienst kann aus einem Dienst unter in Azure oder direkt über das Internet aus jeder Anwendung, die eine HTTPS-Anforderung senden und empfangen eine HTTPS-Antwort kann zugegriffen werden.

## <a name="Installation"> </a>Installation

Alle in diesem Artikel beschriebenen Funktionen stehen in der `azure-servicemanagement-legacy` -Paket, das Sie verwenden Pip installieren können. Für Weitere Informationen zur Installation (zum Beispiel, wenn Sie neu bei Python sind), wenden Sie sich an dieser Artikel: [Installation von Python und der Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Wie: Herstellen einer Verbindung Servicemanagement mit
Zum Verbinden mit den Endpunkt Servicemanagement, benötigen Sie Ihre Azure-Abonnement-ID und einem gültigen Management Zertifikat. Sie können Ihr Abonnement-ID über das [Azure klassischen Portal]abrufen[management-portal].

> [AZURE.NOTE] Seit Azure SDK für Python v0.8.0 ist es möglich, erstellte mit OpenSSL beim Ausführen unter Windows von Zertifikaten verwenden.  Dies erforderlich macht Python 2.7.4 oder höher. Es empfiehlt sich Benutzer OpenSSL statt PFX-Datei, da die Unterstützung für PFX-Datei verwenden, die Zertifikate in der Zukunft wahrscheinlich entfernt werden.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Verwaltung von Zertifikaten für Windows/Mac/Linux (OpenSSL)
[OpenSSL](http://www.openssl.org/) können Sie Ihr Management Zertifikat erstellen.  Zum Erstellen von zwei Zertifikate, eine für den Server tatsächlich erforderlichen (eine `.cer` Datei) und eine für den Client (eine `.pem` Datei). Zum Erstellen der `.pem` ablegen, ausführen:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Zum Erstellen der `.cer` Zertifikat, ausführen:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Weitere Informationen zu Azure Zertifikate finden Sie unter [Übersicht über Zertifikate für Azure-Cloud-Dienste](./cloud-services-certs-create.md). Eine vollständige Beschreibung der OpenSSL Parameter finden Sie in der Dokumentation unter [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Nachdem Sie diese Dateien erstellt haben, müssen Sie Hochladen der `.cer` Datei Azure über die Aktion "Hochladen" der Registerkarte "Einstellungen" des [Azure klassischen Portal][management-portal], und Sie müssen Notieren Sie sich der Speicherort der `.pem` Datei.

Nachdem Sie Ihre Abonnement-ID erhalten haben, ein Zertifikat erstellt und hochgeladen der `.cer` Datei mit Azure, können Sie an den Endpunkt Azure Management verbinden, indem Sie die Abonnement-Id und den Pfad zu übergeben die `.pem` Datei **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Im vorherigen Beispiel `sms` ist ein **ServiceManagementService** -Objekt. Die **ServiceManagementService** -Klasse ist die primäre Klasse zum Verwalten von Azure-Diensten verwendet.

### <a name="management-certificates-on-windows-makecert"></a>Verwaltung von Zertifikaten für Windows (MakeCert)

Sie können ein selbst signiertes Management Zertifikat erstellen, klicken Sie auf Ihrem Computer mit `makecert.exe`.  Öffnen Sie ein **Eingabeaufforderungsfenster Visual Studio** als **Administrator** aus, und verwenden Sie den folgenden Befehl, ersetzen *AzureCertificate* mit der Name des Zertifikats aus, die Sie verwenden möchten.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Der Befehl erstellt die `.cer` Datei, und es in der **persönlichen** Zertifikats Store installiert. Weitere Informationen hierzu finden Sie unter [Übersicht über Zertifikate für Azure-Cloud-Dienste](./cloud-services-certs-create.md).

Nachdem Sie das Zertifikat erstellt haben, müssen Sie zum Hochladen der `.cer` Datei Azure über die Aktion "Hochladen" der Registerkarte "Einstellungen" des [Azure klassischen Portal][management-portal].

Nachdem Sie Ihre Abonnement-ID erhalten haben, ein Zertifikat erstellt und hochgeladen der `.cer` Datei mit Azure, können Sie an den Endpunkt Azure Management verbinden, indem Sie die Abonnement-Id und den Speicherort des Zertifikats **ServiceManagementService** (in diesem Fall ersetzen *AzureCertificate* mit dem Namen des Zertifikats) in Ihrem **persönlichen** Zertifikats Store übergeben:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Im vorherigen Beispiel `sms` ist ein **ServiceManagementService** -Objekt. Die **ServiceManagementService** -Klasse ist die primäre Klasse zum Verwalten von Azure-Diensten verwendet.

## <a name="ListAvailableLocations"> </a>Wie: Liste der verfügbaren Speicherorte aus

Um den jeweiligen Speicherorten aufzulisten, die für Hostingdienste verfügbar sind, verwenden Sie die **Liste\_Speicherorte** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Beim Erstellen einer Cloud-Dienst oder Speicherdienst müssen Sie einen gültigen Speicherort bereitstellen. Die **Liste\_Speicherorte** Methode gibt immer eine aktuelle Liste der aktuell verfügbaren Speicherorte aus. Sind derzeit verfügbaren Speicherorte aus:

- Westen Europa
- North Europa
- Oder Asien
- Ostasien
- USA – zentral
- Nord-zentralen US
- Süd zentralen US
- Westen US
- Ostasiatische US
- Japan OST
- Japan "Westen"
- Brasilien Süd
- Australien OST
- Australien oder

## <a name="CreateCloudService"> </a>Wie: erstellen einen Cloud-Dienst

Wenn Sie eine Anwendung erstellen und im Azure ausführen, werden der Code und Konfiguration zusammen eine Azure [-Cloud-Dienst][] (als ein *Dienst gehostet* in vorherigen Versionen Azure bezeichnet) bezeichnet. Die **Erstellen\_gehostet\_Service** Methode können Sie einen neuen gehosteten Dienst können, indem Sie einen gehosteten Dienstnamen (das in Azure eindeutig sein muss), einer Bezeichnung (automatisch base64-codiert), einer Beschreibung und einen Speicherort erstellen.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Sie können die gehosteten Dienste für Ihr Abonnement mit Liste der **Liste\_gehostet\_Services** Methode:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Wenn Sie Informationen zu einem bestimmten gehosteten Dienst erhalten möchten, Sie können dazu durch Übergeben des Namens gehosteten Dienst, um die **abrufen\_gehostet\_Service\_Eigenschaften** Methode:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Nachdem Sie einen Clouddienst erstellt haben, können Sie den Code bereitstellen, mit dem Dienst unter der **Erstellen\_Bereitstellung** Methode.

## <a name="DeleteCloudService"> </a>Wie: Löschen einen Cloud-Dienst

Sie können einen Clouddienst löschen, indem Sie den Dienstnamen zu übergeben die **Löschen\_gehostet\_Service** Methode:

    sms.delete_hosted_service('myhostedservice')

Bevor Sie einen Dienst löschen können, müssen alle Bereitstellungen für den Dienst zuerst gelöscht werden. (Finden Sie unter [wie: löschen eine Bereitstellung](#DeleteDeployment) Details.)

## <a name="DeleteDeployment"> </a>Wie: löschen eine Bereitstellung

Verwenden Sie zum Löschen einer bereitstellungs der **Löschen\_Bereitstellung** Methode. Im folgenden Beispiel wird veranschaulicht, wie eine Bereitstellung von benannten löschen `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Wie: Erstellen einer Speicherdienst

Ein [Speicherdienst](../storage/storage-create-storage-account.md) erhalten Sie Zugriff auf Azure [Blobs](../storage/storage-python-how-to-use-blob-storage.md), [Tabellen](../storage/storage-python-how-to-use-table-storage.md)und [Warteschlangen](../storage/storage-python-how-to-use-queue-storage.md)an. Zum Erstellen einer Speicherdienst benötigen Sie einen Namen für den Dienst (zwischen 3 und 24 Kleinbuchstaben und innerhalb Azure eindeutig), eine Beschreibung, einer Bezeichnung (bis zu 100 Zeichen automatisch base64-codiert) und einen Speicherort aus. Im folgenden Beispiel wird gezeigt, wie eine Speicherdienst durch Auswahl einer Position zu erstellen.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Notiz im vorherigen Beispiel, die den Status der **Erstellen\_Speicher\_Konto** Vorgang abgerufen werden kann, indem Sie das Ergebnis zurückgegebene übergeben **Erstellen\_Speicher\_Konto** zu den **abrufen\_Vorgang\_Status** Methode.  

Sie können Ihre Speicherkonten und ihre Eigenschaften mit Liste der **Liste\_Speicher\_Konten** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Wie: Löschen einer Speicherdienst

Sie können eine Speicherdienst löschen, indem Sie den Namen des Speicher-Dienst zu übergeben die **Löschen\_Speicher\_Konto** Methode. Löschen einer Speicherdienst löscht alle Daten im Dienst (Blobs, Tabellen und Warteschlangen) gespeichert.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Wie: Liste der verfügbaren Betriebssysteme

Um den Betriebssystemen aufzulisten, die für Hostingdienste verfügbar sind, verwenden Sie die **Liste\_Betrieb\_Systeme** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternativ können Sie die **Liste\_Betrieb\_System\_Familien** Methode, die den Betriebssystemen durch Familie gruppiert:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Wie: erstellen ein Bilds Betriebssystem

Um das Bild Repository ein Betriebssystemabbild hinzugefügt haben, verwenden Sie die **Hinzufügen\_os\_Bild** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Um die Bilder Betriebssystem aufzulisten, die verfügbar sind, verwenden Sie die **Liste\_os\_Bilder** Methode. Es enthält alle Plattform Bilder und Grafiken des Benutzers:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Wie: Löschen eines Bilds Betriebssystem

Verwenden Sie zum Löschen eines Bilds Benutzer der **Löschen\_os\_Bild** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Wie: erstellen ein virtuellen Computers

Zum Erstellen eines virtuellen Computers, müssen Sie zuerst einen [Cloud-Dienst](#CreateCloudService)zu erstellen.  Erstellen Sie dann den virtuellen Computern Bereitstellung mithilfe der **Erstellen\_virtuellen\_Computer\_Bereitstellung** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Wie: Löschen einer virtuellen Computern

Zum Löschen eines virtuellen Computers zuerst Löschen der Bereitstellung mithilfe der **Löschen\_Bereitstellung** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Cloud-Dienst kann dann gelöscht werden mithilfe der **Löschen\_gehostet\_Service** Methode:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Gewusst wie: Erstellen eines virtuellen Computers aus einem Bild aufgenommene virtuellen Computern

Um ein Bild virtueller Computer zu erfassen, rufen Sie zuerst die **erfassen\_virtuellen Computer\_Bild** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Weiter, um sicherzustellen, dass Sie das Bild erfolgreich aufgezeichnet haben, verwenden Sie die **Liste\_virtuellen Computer\_Bilder** api, und vergewissern Sie sich das Bild wird in den Ergebnissen angezeigt:

    images = sms.list_vm_images()

Verwenden Sie schließlich des virtuellen Computers verwenden das erfasste Bild erstellen, die **Erstellen\_virtuellen\_Computer\_Bereitstellung** Methode wie zuvor, jedoch diesmal stattdessen in der Vm_image_name übergeben

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Weitere Informationen zum Erfassen von einem Linux virtuellen Computern finden Sie unter [wie eine Linux virtuellen Computern erfassen.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Weitere Informationen zum Erfassen von einem Windows-Computer finden Sie unter [wie einem Windows-Computer erfassen.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Nächste Schritte

Die Grundlagen des Servicemanagement bearbeitet haben, können Sie Zugriff auf die [vollständige API Dokumentation zum Microsoft Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) und einfach zum Verwalten Ihrer Anwendungs Python komplexe Aufgaben ausführen.

Weitere Informationen finden Sie unter der [Python Developer Center](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[Cloud-Dienst]:/services/cloud-services/

