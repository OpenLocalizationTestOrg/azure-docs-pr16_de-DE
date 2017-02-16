<properties
    pageTitle="So erstellen Sie eine Auflistung Hybrid für Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie eine Bereitstellung von RemoteApp zu erstellen, die mit Ihrem internen Netzwerk verbinden."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo"/>

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>So erstellen Sie eine Auflistung Hybrid für Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Es gibt zwei Arten von Azure RemoteApp Websitesammlungen aus:

- Cloud: befindet sich vollständig in Azure. Sie können auch alle Daten in der Cloud speichern (, damit eine Auflistung Cloud nur) oder zum Herstellen einer Verbindung eine VNET mit Ihrer Websitesammlung und Speichern von Daten vorhanden.   
- Hybrid: enthält ein virtuelles Netzwerk für lokale Access - dies erfordert die Verwendung von Azure AD- und einer lokalen Active Directory-Umgebung.

Wissen nicht, welche die benötigten? Schauen Sie sich [die Art der Websitesammlung müssen Sie für Azure RemoteApp](remoteapp-collections.md).

Dieses Lernprogramm führt Sie durch die Vorgehensweise zum Erstellen einer Websitesammlungs Hybrid. Es gibt acht Schritte aus:

1.  Entscheiden Sie, welche [Bild](remoteapp-imageoptions.md) für Ihre Websitesammlung verwendet werden soll. Erstellen ein benutzerdefiniertes Bild oder verwenden Sie eine der Microsoft Bilder in Ihrem Abonnement enthalten können.
2. Richten Sie Ihre virtuelle Netzwerk. Schauen Sie sich die Informationen [VNET Planung](remoteapp-planvnet.md) und [Anpassen der Größe](remoteapp-vnetsizing.md) .
2.  Erstellen einer Websitesammlung.
2.  Teilnehmen an der Websitesammlung der lokalen Domäne.
3.  Hinzufügen eines Vorlage für Ihre Sammlung.
4.  Konfigurieren der Verzeichnissynchronisation. Azure RemoteApp erfordert, dass Sie integrieren mit Azure Active Directory nach entweder 1) Konfigurieren von Azure Active Directory synchronisieren mit die Option Kennwort synchronisieren oder 2) Konfigurieren von Azure Active Directory Sync ohne die Option Kennwort synchronisieren, jedoch mit einer Domäne, die in AD FS verbunden ist. Schauen Sie sich die [von Konfigurationsinformationen für Active Directory mit RemoteApp](remoteapp-ad.md).
5.  Veröffentlichen von RemoteApp apps.
6.  Konfigurieren des Benutzerzugriffs.

**Vorbemerkung**

Sie müssen vor dem Erstellen der Auflistung die folgenden Schritte durchführen:

- [Registrieren](https://azure.microsoft.com/services/remoteapp/) für Azure RemoteApp an.
- Erstellen eines Benutzerkontos in Active Directory als Dienstkonto Azure RemoteApp verwenden. Schränken Sie die Berechtigungen für dieses Konto ein, sodass sie nur Computer in der Domäne hinzufügen kann.
- Sammeln Sie Informationen zu Ihrem lokalen Netzwerk: IP-Adresse, Informationen und Details von VPN-Gerät.
- Installieren Sie das [Azure PowerShell](../powershell-install-configure.md) -Modul.
- Sammeln Sie Informationen zu den Benutzern, denen Sie Zugriff gewähren möchten. Sie benötigen den Azure-Active Directory-Benutzerprinzipalnamen (z. B. name@contoso.com) für jeden Benutzer. Stellen Sie sicher, dass der UPN zwischen Azure AD entspricht und Active Directory.
- Wählen Sie das Vorlagenbild aus. Ein Azure RemoteApp Vorlagenbild enthält die apps und Programme, die Sie für Ihre Benutzer veröffentlichen möchten. Weitere Informationen finden Sie unter [Bildoptionen Azure RemoteApp](remoteapp-imageoptions.md) .
- Möchten Sie das Office 365 ProPlus Bild verwenden? Schauen Sie sich Informationen [hier](remoteapp-officesubscription.md).
- [Konfigurieren von Active Directory für RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Schritt 1: Richten Sie Ihres Netzwerks virtuelle ein
Sie können eine Hybrid-Auflistung, die ein vorhandenes Azure virtuelles Netzwerk verwendet bereitstellen, oder Sie können ein neues virtuelles Netzwerk erstellen. Ein virtuelles Netzwerk kann die Benutzer Access-Daten in Ihrem lokalen Netzwerk über RemoteApp remote-Ressourcen. Verwenden ein Azure-virtuelles Netzwerk bietet Ihrer direkten Netzwerkzugriff Websitesammlung zu anderen Azure-Diensten und virtuellen Computern, die mit diesem virtuellen Netzwerk bereitgestellt.

Stellen Sie sicher, dass Sie die Informationen [VNET Planung](remoteapp-planvnet.md) und [VNET Größe](remoteapp-vnetsizing.md) überprüfen, bevor Sie Ihre VNET erstellen.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Erstellen Sie einer Azure VNET und verknüpfen Sie ihn mit Ihrer Active Directory-Bereitstellung

Erstellt ein [virtuelles Netzwerk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)starten. Dies geschieht auf der Registerkarte **Netzwerk** Azure-Portal. Sie müssen das virtuelle Netzwerk mit Active Directory-Bereitstellung verbinden, die in Ihrem Mandanten Azure Active Directory synchronisiert wird.

Weitere Informationen finden Sie unter [erstellen ein virtuelles Netzwerks über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Sicherstellen Sie, dass Ihr Netzwerk virtuelle für Azure RemoteApp bereit ist
Bevor Sie Ihre Sammlung erstellen, lassen Sie uns sicherzustellen, dass Ihr Netzwerk für neue virtuelle bereit ist. Sie können dies überprüfen, indem Sie folgende Schritte ausführen:

1. Erstellen Sie eine Azure-virtuellen Computern innerhalb der Subnetz des virtuellen Netzwerks für RemoteApp soeben erstellten.
2. Verwenden Sie Remotedesktop Verbindung zum des virtuellen Computers. (Klicken Sie auf **Verbinden**.)
3. Ordnen Sie ihn der gleichen Active Directory-bereitstellungs, die Sie für RemoteApp verwenden möchten.

Wurde, die werden ausgeführt? Zur Azure RemoteApp bereitstehen virtuelle Netzwerk und Subnetz!

Sie können weitere Informationen zum Erstellen von Azure-virtuellen Computern und Herstellen von Verbindungen mit Remote Desktop [hier](https://msdn.microsoft.com/library/azure/jj156003.aspx)zu finden.

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Schritt 2: Erstellen einer Websitesammlungs Azure RemoteApp ##



1. Wechseln Sie im [Azure-Portal](http://manage.windowsazure.com)zur Seite Azure RemoteApp.
2. Klicken Sie auf **Neu > Erstellen mit VNET**.
3. Geben Sie einen Namen für Ihre Websitesammlung.
4. Wählen Sie den Plan, dass Sie - standard oder grundlegende verwenden möchten.
5. Wählen Sie Ihre VNET aus der Dropdown-Liste und dann auf Ihrem Subnetz gehören.
6. Wählen Sie aus, es zu Ihrer Domäne hinzuzufügen.
5. Klicken Sie auf die **Auflistung RemoteApp erstellen**.

Nachdem Sie Ihre Azure RemoteApp Sammlung erstellt wurde, doppelklicken Sie auf den Namen der Sammlung. Die von der Seite **Schnellstart** bringt – Dies ist die Stelle, an der Sie die Websitesammlung konfiguriert haben.

Schief etwas? Schauen Sie sich die [Hybrid Websitesammlung Informationen zur Problembehandlung](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Schritt 3: Verknüpfen der Websitesammlung mit der lokalen Domäne ##


1. Klicken Sie auf der Seite **Schnellstart** auf **Verknüpfung eine lokale Domäne**.
2. Hinzufügen des Dienstkontos Azure RemoteApp der lokalen Active Directory-Domäne. Sie benötigen den Namen Ihrer Domäne, Organisationseinheit, Service-Konto-Benutzernamen und das Kennwort.

    Dies ist die Informationen, die Sie gesammelt, wenn Sie die Schritte in [Active Directory für Azure RemoteApp konfigurieren](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Schritt 4: Link zu einem Bild Azure RemoteApp ##

Ein Azure RemoteApp Vorlagenbild enthält die Programme, die Sie für Benutzer freigeben möchten. Sie können ein neues [Vorlagenbild](remoteapp-imageoptions.md) erstellen oder Verknüpfen eines vorhandenen Bilds (eine bereits importiert oder Azure RemoteApp geladen). Sie können auch auf einen der Azure RemoteApp [Vorlage Bilder](remoteapp-images.md) , die Office 365 oder Office 2013 (für verwenden Sie zum Testen) Programmen enthalten verknüpfen.

Wenn Sie das neue Bild hochladen, müssen Sie geben Sie den Namen, und wählen den Speicherort für das Bild. Klicken Sie auf der nächsten Seite des Assistenten Sie finden Sie eine Reihe von PowerShell-Cmdlets - kopieren und Ausführen dieser Cmdlets aus einer erhöhten Windows PowerShell gefragt werden, ob das angegebene Bild hochladen.

Wenn Sie eine Verknüpfung zu einem vorhandenen Vorlagenbild erstellen, geben Sie einfach die ImageName, Position und zugehöriges Azure-Abonnement.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Schritt 5: Konfigurieren von Active Directory-Verzeichnissynchronisation ##

Azure RemoteApp erfordert, dass Sie integrieren mit Azure Active Directory nach entweder 1) Konfigurieren von Azure Active Directory synchronisieren mit die Option Kennwort synchronisieren oder 2) Konfigurieren von Azure Active Directory Sync ohne die Option Kennwort synchronisieren, jedoch mit einer Domäne, die in AD FS verbunden ist.

Schauen Sie sich [AD verbinden](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – in diesem Artikel hilft Ihnen beim Einrichten Verzeichnisintegration in 4 Schritten fort.

Planen von Informationen und detaillierten Schritte finden Sie in [verzeichnissynchronisierung: Wegweiser](http://msdn.microsoft.com//library/azure/hh967642.aspx) .

## <a name="step-6-publish-apps"></a>Schritt 6: Veröffentlichen von apps ##

Eine app Azure RemoteApp ist der app oder Programm, die für Ihre Benutzer bereitgestellt. Es befindet sich das Vorlagenbild, die Sie für die Websitesammlung hochgeladen. Wenn ein Benutzer eine app greift auf, es in ihrer lokalen Umgebung ausgeführt wird, aber es wirklich in Azure ausgeführt wird.

Bevor Sie Ihre Benutzer apps zugreifen können, müssen Sie diese veröffentlicht – auf diese Weise können auf Ihre Benutzer der apps über Remote Desktop-Client zugreifen.

Sie können mehrere apps in Ihrer Websitesammlung veröffentlichen. Klicken Sie aus der Veröffentlichungsseite auf **Veröffentlichen** , um eine app hinzufügen. Sie können entweder über **das Startmenü des Bilds Vorlage oder die Pfadangabe auf das Vorlagenbild für die app** veröffentlichen. Wenn Sie über **das Startmenü** hinzufügen, wählen Sie das Programm hinzufügen. Wenn Sie den Pfad zu der app bereitstellen auch, geben Sie einen Namen für die app und den Pfad zu dem sie auf das Vorlagenbild installiert ist.

## <a name="step-7-configure-user-access"></a>Schritt 7: Konfigurieren des Benutzerzugriffs ##

Jetzt, da Sie Ihre Sammlung erstellt haben, müssen Sie die Benutzer hinzufügen, die Sie wäre remote Ressourcen verwenden möchten. Benutzer Bereitstellen von Access, um in den Active Directory-Mandanten mit dem Abonnement verknüpft ist vorhanden sein müssen, die Sie zum Erstellen von dieser Websitesammlung Azure RemoteApp verwendet.

1.  Klicken Sie auf der Seite Schnellstart **Konfigurieren des Benutzerzugriffs**.
2.  Geben Sie das Konto Arbeit (aus dem Active Directory) oder Microsoft-Konto, dem Sie Zugriff gewähren möchten.

    **Hinweise:**

    Stellen Sie sicher, dass Sie verwenden die “user@domain.com” Format.

    Wenn Sie Office 365 ProPlus in Ihrer Websitesammlung verwenden, müssen Sie die Active Directory-Identitäten für Ihre Benutzer verwenden. Auf diese Weise können Lizenzierung überprüfen.


3.  Nachdem die Benutzer überprüft werden, klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte ##
Das war's auch – erfolgreich erstellt und Ihre Azure RemoteApp Hybrid Sammlung bereitgestellt haben. Im nächsten Schritt wird, dass Ihre Benutzer herunterladen und Installieren des Remote Desktop-Clients. Sie können die URL für den Client auf der Seite Azure RemoteApp Schnellstart suchen. Klicken Sie dann Benutzern die Anmeldung in den Client und den Zugriff der apps, die Sie veröffentlicht.



### <a name="help-us-help-you"></a>Helfen Sie uns Ihnen helfen
Wussten Sie schon, dass zusätzlich zur Bewertung der in diesem Artikel und Erstellen von Kommentaren nach unten unter, Sie im Artikel selbst ändern können? Fehlt etwas? Ein Problem? Schreiben ich etwas, die nur verwirrend ist? Bildlauf nach oben, und klicken Sie auf **Bearbeiten, klicken Sie auf GitHub** , um die Änderungen vornehmen – die werden zu uns kommen, zur Überprüfung, und klicken Sie dann, sobald wir sie melden Sie sich auf deaktivieren, sehen Sie Ihre Änderungen und direkt hier Verbesserungen.
