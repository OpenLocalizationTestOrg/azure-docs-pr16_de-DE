<properties
    pageTitle="Cloud Services häufig gestellte Fragen zu | Microsoft Azure"
    description="Häufig gestellte Fragen zu Cloud Services ein."
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
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloud Services häufig gestellte Fragen
In diesem Artikel finden Sie Antworten auf einige häufig gestellten Fragen zur Microsoft Azure-Cloud-Dienste. Sie können auch die [Azure unterstützen häufig gestellte Fragen zu](http://go.microsoft.com/fwlink/?LinkID=185083) allgemeinen Azure Preise und Support-Informationen besuchen. Sie können auch die [Cloud Services virtueller Speicher Seite](cloud-services-sizes-specs.md) finden Sie Größeninformationen.

## <a name="certificates"></a>Zertifikate

### <a name="where-should-i-install-my-certificate"></a>Wo sollte ich meine Zertifikat installieren?

- **Meine**  
Anwendungszertifikat mit privaten Schlüssel (\*PFX, \*p12).

- **ZERTIFIZIERUNGSSTELLE**  
Wechseln Sie alle Ihre zwischen-XT für Zertifikate in diesem Speicher (Richtlinie und Sub Zertifizierungsstellen).

- **QUADRATWURZEL**  
Die Stammzertifizierungsstelle speichern, damit Ihre Hauptfenster Stamm-CA-Zertifikat hier eingefügt werden sollen.

### <a name="i-cant-remove-expired-certificate"></a>Abgelaufenes Zertifikat kann nicht entfernt werden.

Azure verhindert, dass Sie ein Zertifikat entfernen, während sie verwendet wird. Sie müssen entweder Löschen der Umgebung, die das Zertifikat verwendet, oder aktualisieren die Bereitstellung mit einem anderen oder verlängert Zertifikat.

### <a name="delete-an-expired-certificate"></a>Löschen eines abgelaufenen Zertifikats

Solange das Zertifikat nicht verwendet wird, können Sie das [Entfernen-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell-Cmdlet So entfernen Sie ein Zertifikat verwenden.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Mit dem Windows Azure-Servicemanagement für Erweiterungen Zertifikate sind abgelaufen werden.

Diese Zertifikate werden erstellt, sobald die Clouddienst, wie etwa die Erweiterung Remotedesktop eine Erweiterung hinzugefügt wird. Diese Zertifikate werden nur zum Verschlüsseln und Entschlüsseln der privaten Konfigurations der Erweiterung verwendet. Es spielt keine Rolle, wenn diese Zertifikate ablaufen. Das Ablaufdatum ist nicht aktiviert.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Urkunden aufgeführt, die ich gelöscht haben werden angezeigt

Diese wahrscheinlich aufgrund eines Tools werden angezeigt, die Sie, die wie z. B. Visual Studio verwenden. Wenn Sie eine Verbindung mit einem Tool, die ein Zertifikat verwendet wird wiederherzustellen, wird er erneut in Azure hochgeladen werden.

### <a name="my-certificates-keep-disappearing"></a>Meine Zertifikate verschwinden beibehalten

Wenn die Instanz des virtuellen Computers-Freigabe, werden alle lokale Änderungen verloren. Verwenden einer [Startaufgabe](cloud-services-startup-tasks.md) Zertifikate für des virtuellen Computers jedes Mal installieren, die die Rolle wird gestartet.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Ich kann meine Management Zertifikate im Portal nicht finden

[Management Zertifikate](..\azure-api-management-certs.md) sind nur in der klassischen Azure-Portal zur Verfügung. Verwaltung von Zertifikaten verwendet im aktuelle Azure-Portal nicht. 

### <a name="how-can-i-disable-a-management-certificate"></a>Wie kann ich ein Zertifikat Management deaktivieren?

[Verwaltung von Zertifikaten](..\azure-api-management-certs.md) kann nicht deaktiviert werden. Sie löschen diese über das klassische Azure-Portal, wenn Sie nicht mehr verwendet werden sollen.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Wie kann ich ein SSL-Zertifikat für eine bestimmte IP-Adresse erstellen?

Führen Sie die Schritte in der [ein Zertifikat Lernprogramm erstellen](cloud-services-certs-create.md)aus. Verwenden Sie die IP-Adresse als den DNS-Namen ein.

## <a name="security"></a>Sicherheit

### <a name="disable-ssl-30"></a>Deaktivieren Sie SSL 3.0

Damit deaktivieren SSL 3.0 und TLS-Sicherheit verwendet wird, erstellen Sie einen Start-Vorgang die beschrieben ist auf diesen Blogbeitrag: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Skalieren Sie Cloud-Dienst

### <a name="i-cannot-scale-beyond-x-instances"></a>Ich kann nicht X skalierbar Instanzen

Ihr Abonnement Azure sind maximal auf die Anzahl der Kerne, die Sie verwenden können. Skalierung funktioniert nicht, wenn Sie alle verfügbaren Kerne verwendet haben. Wenn Sie maximal 100 Kerne verfügen, bedeutet dies angenommen, Sie möglicherweise 100 A1 Größe virtuellen Computern Instanzen für Ihre Cloud-Dienst oder 50 A2 Größe virtuellen Computern Instanzen anzeigen.

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>IP in einen Cloud-Service Multi-VIP kann nicht reserviert werden.

Vergewissern Sie sich, dass die Instanz von virtuellen Computern, der Sie versuchen, sich die IP-Adresse reservieren aktiviert ist. Zweites, stellen Sie sicher, dass Sie die reservierte IP-Adressen für aufwändig Staging und Herstellung-Bereitstellungen verwenden. **Keine** Ändern der Einstellungen, während die Bereitstellung aktualisieren ist.

