<properties
    pageTitle="Installieren von Active Directory Active Directory in Azure | Microsoft Azure"
    description="Ein Lernprogramm, die erläutert, wie Sie einen Domänencontroller aus lokalen Active Directory-Struktur auf eine Azure-virtuellen Computern installieren."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Installieren von Active Directory Active Directory in einem Azure-virtuellen Netzwerk

In diesem Thema wird für lokalen Active Directory-Domäne auf Azure-virtuellen Computern (virtuelle Computer) in einem Azure-virtuellen Netzwerk beschrieben zusätzliche Domain Controller (auch bekannt als Replikat DCs) installieren.

Sie können auch diese verwandten Themen interessant sein:

-  Optional können Sie eine neue Active Directory-Struktur einer Azure virtuelle Netzwerk installieren. Diese Schritte finden Sie unter [installieren eine neue Active Directory-Gesamtstruktur einer Azure virtuelle Netzwerk](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Konzeptionelle Anleitung zum Installieren von Active Directory-Domänendiensten (AD DS) auf ein Azure-virtuellen Netzwerk finden Sie unter [Richtlinien zum Bereitstellen von Windows Server Active Directory auf Azure virtuellen Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Szenario-Diagramm

In diesem Szenario müssen externe Benutzer Applikationen zugreifen, die auf Servern Domänenverbund ausgeführt werden. Die virtuellen Computern ausführen, die die Anwendungsserver, und das Replikat DCs in einem Azure-virtuellen Netzwerk installiert sind. Das virtuelle Netzwerk kann werden mit dem lokalen Netzwerk verbunden durch eine Verbindung [zwischen Standorten VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) , wie in der folgenden Abbildung dargestellt, oder Sie können [ExpressRoute](../expressroute/expressroute-locations-providers.md) verwenden, für eine schnellere Verbindung.

Die Anwendungsserver und die DCs werden in separaten Clouddienste berechnen Verarbeitung verteilen und innerhalb der [Verfügbarkeit von Gruppen](../virtual-machines/virtual-machines-windows-manage-availability.md) für Verbesserte Fehlertoleranz bereitgestellt.
Die DCs repliziert untereinander und mit lokalen DCs mithilfe von Active Directory-Replikation. Es sind keine Synchronisierung Tools erforderlich.

![Öffentliche Ordner: Replikatsdomänencontrollers Active Directory eine Azure Vnet in einem Diagramm][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Erstellen einer Active Directory-Website für das Azure virtuelle Netzwerk

Es ist eine gute Idee, erstellen eine Website in Active Directory, die die Netzwerk Region in Bezug auf das virtuelle Netzwerk darstellt. Ihnen dabei hilft, Authentifizierung, Replikation und andere DC Speicherort Vorgänge zu optimieren. Die folgenden Schritte erläutert, wie eine Website erstellen und Weitere Hintergrundinformationen finden Sie unter [Hinzufügen einer neuen Website](https://technet.microsoft.com/library/cc781496.aspx).

1. Öffnen Sie Active Directory-Websites und-Diensten: **Server-Manager** > **Tools** > **Active Directory-Websites und-Diensten**.
2. Erstellen einer Website, um den Bereich dar, in dem Sie ein Azure-virtuelles Netzwerk erstellt: Klicken Sie auf **Websites** > **Aktion** > **neue Website** > Geben Sie den Namen der neuen Website, wie z. B. Azure uns "Westen" > Wählen Sie einen Websitehyperlink > **OK**.
3. Erstellen Sie ein Subnetz und ordnen Sie die neue Website: Doppelklicken Sie auf **Websites** > mit der rechten Maustaste **Subnetze** > **Neues Subnetz** > Geben Sie im Bereich IP-Adresse des virtuellen Netzwerks (z. B. 10.1.0.0/16 im Diagramm Szenario) > Wählen Sie die neue Azure Website > **OK**.

## <a name="create-an-azure-virtual-network"></a>Erstellen Sie ein Azure-virtuelles Netzwerk

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)auf **neu** > **Network Services** > **Virtuelles Netzwerk** > **Benutzerdefinierte erstellen** und verwenden Sie die folgenden Werte, um den Assistenten abzuschließen.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Virtuelle Netzwerkdetails**  | <p>Name: Geben Sie einen Namen für das virtuelle Netzwerk, wie z. B. WestUSVNet.</p><p>Region: Auswählen der nächsten Region.</p>
    **DNS- und VPN-Konnektivität**  | <p>DNS-Server: Geben Sie den Namen und die IP-Adresse des einen oder mehrere lokale DNS-Server.</p><p>Konnektivität: Wählen Sie **Konfigurieren einer Standort-zu-Standort VPN**.</p><p>Lokales Netzwerk: Geben Sie ein neues lokales Netzwerk.</p><p>Wenn Sie anstelle eines VPN ExpressRoute verwenden, finden Sie unter [Konfigurieren einer ExpressRoute Verbindung über ein Exchange-Anbieter](../expressroute/expressroute-locations-providers.md).</p>
    **Konnektivität zwischen Standorten**  | <p>Name: Geben Sie einen Namen für das lokale Netzwerk.</p><p>VPN-Gerät IP-Adresse: Geben Sie die öffentliche IP-Adresse des Geräts, der mit dem virtuellen Netzwerk verbunden wird. Das Gerät VPN kann sich nicht hinter einem NAT befinden</p><p>Adresse: Legen Sie die Adressbereiche für das lokale Netzwerk (z. B. 192.168.0.0/16 im Diagramm Szenario).</p>
    **Virtuelle Netzwerk-Adressbereiche**  | <p>Adressbereichs: Geben Sie den Bereich der IP-Adresse für virtuellen Computern, die Sie in das Azure virtuelle Netzwerk (z. B. 10.1.0.0/16 im Diagramm Szenario) ausführen möchten. Diese Adresse können nicht mit der Adressbereiche des lokalen Netzwerks überlappen.</p><p>Subnetze: Geben Sie einen Namen und die Adresse für ein Subnetz für die Anwendungsserver (z. B. Front-End, 10.1.1.0/24) und für die DCs (z. B. Back-End-, 10.1.2.0/24).</p><p>Klicken Sie auf **Gateway Subnetz hinzufügen**.</p>

2. Als Nächstes werden Sie des Netzwerkgateways virtuelle, um eine sichere Website-zu-Standort VPN-Verbindung erstellen konfigurieren. Die Anweisungen finden Sie unter [Konfigurieren eines virtuellen Netzwerk Gateways](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Erstellen Sie die Website-zu-Standort VPN-Verbindung zwischen dem neuen virtuellen Netzwerk und einem lokalen VPN-Gerät. Die Anweisungen finden Sie unter [Konfigurieren eines virtuellen Netzwerk Gateways](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .


## <a name="create-azure-vms-for-the-dc-roles"></a>Erstellen Sie für die Rollen DC Azure-virtuellen Computern

Wiederholen Sie die folgenden Schritte zum Erstellen von virtuellen Computern, um die Rolle DC nach Bedarf zu hosten. Sie sollten mindestens zwei virtuelle DCs um Fehlertoleranz und Redundanz unterstützen bereitstellen. Wenn das Azure virtuelle Netzwerk mindestens zwei DCs enthält, die auf ähnliche Weise konfiguriert werden (d. h., sie sind beide globalen Kataloge, DNS-Server ausführen, und weder enthält alle FSMO-Funktion usw.) platzieren Sie dann die virtuellen Computern, die die DCs in eine verbesserte Fehlertoleranz festlegen Verfügbarkeit ausgeführt werden.
Zum Erstellen der virtuellen Computern anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie unter [Verwenden von Azure PowerShell zu erstellen und Windows-basierten virtuellen Maschinen vorkonfiguriert](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)auf **neu** > **berechnen** > **virtuellen Computern** > **Vom Katalog**. Verwenden Sie die folgenden Werte, um den Assistenten abzuschließen. Übernehmen Sie den Standardwert für eine Einstellung, es sei denn, ein anderer Wert vorgeschlagen oder erforderlich ist.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Wählen Sie ein Bild aus.**  | Windows Server 2012 R2 Datacenter
    **Konfiguration des virtuellen Computers**  | <p>Der Name des virtuellen Computers: Geben Sie einen einzelnes Etikett Namen (beispielsweise AzureDC1).</p><p>Name des neuen Benutzers: Geben Sie den Namen eines Benutzers nachschlagen. Diese Benutzer wird ein Mitglied der Gruppe der Administratoren des virtuellen Computers sein. Sie benötigen diesen Namen bei den virtuellen Computer zum ersten Mal anmelden. Das integrierte Konto Administrator funktioniert nicht.</p><p>Neues Kennwort/bestätigen: Geben Sie ein Kennwort</p>
    **Konfiguration des virtuellen Computers**  | <p>Cloud-Dienst: Wählen Sie <b>Erstellen Sie einen neuen Clouddienst</b> für den ersten virtuellen Computer und demselben Namen Cloud-Dienst, wenn Sie weitere virtuellen Computern erstellen, die die Rolle des DC gehostet wird.</p><p>Cloud-Dienst DNS-Name: Geben Sie einen global eindeutigen Namen</p><p>Region/Gruppe Zugehörigkeit/virtuellen Netzwerk: Geben Sie den Namen des virtuellen Netzwerks (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie <b>verwenden ein automatisch generiertes Speicherkonto</b> für den ersten virtuellen Computer aus, und wählen Sie dann die gleichen Speicher Kontonamen aus, wenn Sie weitere virtuellen Computern erstellen, die die Rolle des DC gehostet wird.</p><p>Verfügbarkeit festlegen: Wählen Sie <b>eine Sammlung Verfügbarkeit erstellen</b>aus.</p><p>Verfügbarkeit legen Sie Name: Geben Sie einen Namen für die Verfügbarkeit festzulegen, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann aus, die denselben Namen, wenn Sie weitere virtuelle Computer erstellen.</p>
    **Konfiguration des virtuellen Computers**  | <p>Wählen Sie <b>den Agent virtuellen Computer installieren</b> und alle anderen Erweiterungen, die Sie benötigen.</p>
2. Fügen Sie einen Datenträger an jede virtueller Computer, die Funktion des DC-Servers ausgeführt werden kann. Der zusätzliche Datenträger ist erforderlich, die AD-Datenbanken, Protokolle und SYSVOL gespeichert. Geben Sie eine Größe für den Datenträger (z. B. 10 GB), und lassen Sie die **Host-Cache-Einstellung** **keine**festgelegt. Die einzelnen Schritte finden Sie unter [einen Datenträger Daten auf einem Windows-Computer installieren](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Öffnen Sie **Server-Manager**, nachdem Sie zuerst auf den virtuellen Computer anmelden, > **Datei- und Storage Services** zum Erstellen eines Datenträgers auf diesem Datenträger mit NTFS.
4. Reservieren einer statischen IP-Adresse für virtuellen Computern, die die Rolle des DC ausgeführt werden kann. Um eine statische IP-Adresse reservieren möchten, laden Sie die Microsoft Web Platform Installer und [Azure PowerShell installieren](../powershell-install-configure.md) , und führen Sie das Cmdlet "Set-AzureStaticVNetIP". Beispiel:

    "Get-AzureVM - ServiceName AzureDC1-benennen AzureDC1 | Set-AzureStaticVNetIP - IP-Adresse 10.0.0.4 | Update-AzureVM

Weitere Informationen zum Festlegen einer statischen IP-Adresse finden Sie unter [Konfigurieren eines statischen internen IP-Adresse für einen virtuellen Computer](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installieren von AD DS auf Azure-virtuellen Computern

Melden Sie sich bei eines virtuellen Computers, und stellen Sie sicher, dass Sie über die Website-zu-Standort VPN oder ExpressRoute die Verbindung zu Ressourcen in Ihrem lokalen Netzwerk Konnektivität verfügen. Installieren Sie AD DS klicken Sie dann auf den Azure-virtuellen Computern. Sie können demselben Prozess verwenden, die Sie verwenden, um eine weitere DC in Ihrem lokalen Netzwerk (UI, Windows PowerShell oder einer Antwortdatei) installieren. Wie Sie AD DS installiert haben, stellen Sie sicher, dass Sie das neue Volume für den Speicherort der AD-Datenbank, Protokollen und SYSVOL angeben. Wenn Sie eine Auffrischung auf AD DS-Installation benötigen, finden Sie unter [Installieren von Active Directory-Domänendiensten (Ebene 100)](https://technet.microsoft.com/library/hh472162.aspx) oder [eines Windows Server 2012 Replikatsdomänencontrollers in einer vorhandenen Domäne (Stufe 200) installieren](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Konfigurieren von DNS-Server für das virtuelle Netzwerk

1. Klicken Sie auf den Namen des virtuellen Netzwerks im [Azure klassischen Portal](https://manage.windowsazure.com), und klicken Sie dann auf die Registerkarte **Konfigurieren** die statischen IP-Adressen zugewiesen an die DCs anstelle eines lokalen DNS-Server die IP-Adressen verwenden [die DNS-Server IP-Adressen für Ihre virtuelle Netzwerk umkonfigurieren](../virtual-network/virtual-networks-manage-dns-in-vnet.md) .

2. Um sicherzustellen, dass alle Replikat DC virtuellen Computern im Netzwerk virtuelle mit Verwendung von DNS-Server auf virtuellen Netzwerks konfiguriert sind, klicken Sie auf **virtuellen Computern**, klicken Sie auf der Statusspalte für jeden virtuellen Computer, und klicken Sie dann auf **neu starten**. Warten Sie, bis Sie der virtuellen Computer Zustand **ausgeführt** werden, bevor Sie versuchen, sich wieder anmelden.

## <a name="create-vms-for-application-servers"></a>Erstellen von virtuellen Computern für Anwendungsserver

1. Wiederholen Sie die folgenden Schritte zum Erstellen von virtuellen Computern als Anwendungsserver ausgeführt. Übernehmen Sie den Standardwert für eine Einstellung, es sei denn, ein anderer Wert vorgeschlagen oder erforderlich ist.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Wählen Sie ein Bild aus.**  | Windows Server 2012 R2 Datacenter
    **Konfiguration des virtuellen Computers**  | <p>Der Name des virtuellen Computers: Geben Sie einen einzelnes Etikett Namen (beispielsweise AppServer1).</p><p>Name des neuen Benutzers: Geben Sie den Namen eines Benutzers nachschlagen. Diese Benutzer wird ein Mitglied der Gruppe der Administratoren des virtuellen Computers sein. Sie benötigen diesen Namen bei den virtuellen Computer zum ersten Mal anmelden. Das integrierte Konto Administrator funktioniert nicht.</p><p>Neues Kennwort/bestätigen: Geben Sie ein Kennwort</p>
    **Konfiguration des virtuellen Computers**  | <p>Cloud-Dienst: Wählen Sie **Erstellen Sie einen neuen Clouddienst** für den ersten virtuellen Computer und demselben Namen Cloud-Dienst, wenn Sie weitere virtuellen Computern erstellen, die die Anwendung gehostet wird.</p><p>Cloud-Dienst DNS-Name: Geben Sie einen global eindeutigen Namen</p><p>Region/Gruppe Zugehörigkeit/virtuellen Netzwerk: Geben Sie den Namen des virtuellen Netzwerks (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie **verwenden ein automatisch generiertes Speicherkonto** für den ersten virtuellen Computer aus, und wählen Sie dann die gleichen Speicher Kontonamen aus, wenn Sie weitere virtuellen Computern erstellen, die die Anwendung gehostet wird.</p><p>Verfügbarkeit festlegen: Wählen Sie **eine Sammlung Verfügbarkeit erstellen**aus.</p><p>Verfügbarkeit legen Sie Name: Geben Sie einen Namen für die Verfügbarkeit festzulegen, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann aus, die denselben Namen, wenn Sie weitere virtuelle Computer erstellen.</p>
    **Konfiguration des virtuellen Computers**  | <p>Wählen Sie <b>den Agent virtuellen Computer installieren</b> und alle anderen Erweiterungen, die Sie benötigen.</p>

2. Nach jeder virtuellen Computer bereitgestellt wird, melden Sie sich an, und verbinden Sie es mit der Domäne. **Server-Manager**, klicken Sie auf **Lokale Server** > **Arbeitsgruppe** > **ändern...** und wählen Sie **Domäne** aus, und geben Sie den Namen Ihrer Domäne lokalen. Geben Sie die Anmeldeinformationen eines Benutzers für die Domäne, und starten Sie den virtuellen Computer, um die Domäne Verknüpfung abzuschließen.

Zum Erstellen der virtuellen Computern anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie unter [Verwenden von Azure PowerShell zu erstellen und Windows-basierten virtuellen Maschinen vorkonfiguriert](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Weitere Informationen zur Verwendung von Windows PowerShell finden Sie unter [Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx) und [Azure Cmdlet verweisen](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

-  [Richtlinien für die Bereitstellung von Windows Server Active Directory auf Azure-virtuellen Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Wie Sie vorhandene lokale Hyper-V Domain Controller in Azure hochladen, mithilfe von Azure PowerShell](http://support.microsoft.com/kb/2904015)
-  [Installieren Sie eine neue Active Directory-Struktur auf ein Azure-virtuellen Netzwerk](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: (01) virtuellen Computern-Grundlagen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) erstellen virtuelle Netzwerke und Cross lokale Konnektivität](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Cmdlets für die Verwaltung von Azure](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
