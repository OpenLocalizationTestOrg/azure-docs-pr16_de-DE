<properties
    pageTitle="Installieren von Active Directory-Struktur auf ein Azure-virtuellen Netzwerk | Microsoft Azure"
    description="Ein Lernprogramm, die zum Erstellen eines neuen Active Directory-Struktur auf einem virtuellen Computer (virtueller Computer) in eine Azure-virtuellen Netzwerk erläutert."
    services="active-directory, virtual-network"
    keywords="Active Directory virtuellen Computern, installieren active Directory-Struktur, Azure-active Directory-videos "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Installieren Sie eine neue Active Directory-Struktur auf ein Azure-virtuellen Netzwerk

In diesem Thema zeigt eine [Azure virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md)So erstellen Sie eine neue Windows Server Active Directory-Umgebung auf ein Azure-virtuellen Netzwerk auf einem virtuellen Computer (virtueller Computer). In diesem Fall ist das Azure virtuelle Netzwerk nicht mit einem lokalen Netzwerk verbunden.

Sie können auch diese verwandten Themen interessant sein:

- Für ein Video zeigt, die folgenden Schritte aus, finden Sie unter [installieren eine neue Active Directory-Gesamtstruktur auf ein Azure-virtuellen Netzwerk](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- Sie können optional [Konfigurieren eines Standort-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) , und klicken Sie dann entweder eine neue Gesamtstruktur installieren oder eine lokale Gesamtstruktur eine Azure-virtuellen Netzwerk zu erweitern. Diese Schritte finden Sie unter [Installieren von Active Directory Active Directory in einem Azure-virtuellen Netzwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Konzeptionelle Anleitung zum Installieren von Active Directory-Domänendiensten (AD DS) auf ein Azure-virtuellen Netzwerk finden Sie unter [Richtlinien zum Bereitstellen von Windows Server Active Directory auf Azure virtuellen Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Szenario-Diagramm

In diesem Szenario müssen externe Benutzer Applikationen zugreifen, die auf Servern Domänenverbund ausgeführt werden. Die virtuellen Computern, die die Anwendungsserver ausgeführt werden und die virtuellen Computern, die Domänencontroller ausgeführt werden installiert in eigene Cloud-Dienst in einem Azure-virtuellen Netzwerk installiert sind. Sie sind auch innerhalb einer Verfügbarkeit einrichten für Verbesserte Fehlertoleranz enthalten.

![Active Directory-Struktur auf einer virtuellen Computern in Azure-virtuellen Netzwerk ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Wie unterscheidet sich dies von lokalen?

Es gibt keinen großen Unterschied zwischen einen Domänencontroller im Vergleich zu lokalen auf Azure installieren. In der folgenden Tabelle werden die wichtigsten Unterschiede aufgeführt.

So konfigurieren Sie...  | Lokal  | Azure virtuelles Netzwerk
------------- | -------------  | ------------
**IP-Adresse für die Domänencontroller**  | Statische IP-Adresse auf die Eigenschaften von Netzwerkadapter zuweisen   | Führen Sie das Cmdlet "Set-AzureStaticVNetIP", um eine statische IP-Adresse zuweisen
**DNS-Clientauflösung**  | Festlegen von bevorzugten und den alternativen DNS-Server-Adresse im Netzwerk Netzwerkadapter Eigenschaften der Domänenmitglieder   | Einrichten von DNS-Serveradresse auf die der virtuelle Netzwerkeigenschaften
**Active Directory-Datenbank-Speicher**  | Ändern Sie den Standardspeicherort optional von C:\  | Sie müssen Standardspeicherort von C:\ Ändern



## <a name="create-an-azure-virtual-network"></a>Erstellen Sie ein Azure-virtuelles Netzwerk

1. Melden Sie sich zum klassischen Azure-Portal aus.
2. Erstellen Sie ein virtuelles Netzwerk. Klicken Sie auf **Netzwerke** > **Erstellen eines virtuellen Netzwerks**. Verwenden Sie die Werte in der folgenden Tabelle, um den Assistenten abzuschließen.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Virtuelle Netzwerkdetails**  | <p>Name: Geben Sie einen Namen für das virtuelle Netzwerk</p><p>Region: Auswählen der nächsten region</p>
    **DNS- und VPN**  | <p>Lassen Sie DNS-Server leer</p><p>Wählen Sie die beiden VPN-Optionen</p>
    **Virtuelle Netzwerk-Adressbereiche**  | <p>Subnetz Name: Geben Sie einen Namen für Ihr Subnetz</p><p>Start-IP-: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Erstellen von virtuellen Computern zum Ausführen der Domain Controller und DNS-Serverrollen

Wiederholen Sie die folgenden Schritte zum Erstellen von virtuellen Computern, um die Rolle DC nach Bedarf zu hosten. Sie sollten mindestens zwei virtuelle DCs um Fehlertoleranz und Redundanz unterstützen bereitstellen. Wenn das Azure virtuelle Netzwerk mindestens zwei DCs enthält, die auf ähnliche Weise konfiguriert werden (d. h., sie sind beide globalen Kataloge, DNS-Server ausführen, und weder enthält alle FSMO-Funktion usw.) platzieren Sie dann die virtuellen Computern, die die DCs in eine verbesserte Fehlertoleranz festlegen Verfügbarkeit ausgeführt werden.

Zum Erstellen der virtuellen Computern anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie unter [Verwenden von Azure PowerShell zu erstellen und Windows-basierten virtuellen Maschinen vorkonfiguriert](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Klicken Sie in der klassischen-Portal auf **neu** > **berechnen** > **virtuellen Computers** > **-Katalog aus**. Verwenden Sie die folgenden Werte, um den Assistenten abzuschließen. Übernehmen Sie den Standardwert für eine Einstellung, es sei denn, ein anderer Wert vorgeschlagen oder erforderlich ist.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Wählen Sie ein Bild aus.**  | Windows Server 2012 R2 Datacenter
    **Konfiguration des virtuellen Computers**  | <p>Der Name des virtuellen Computers: Geben Sie einen einzelnes Etikett Namen (beispielsweise AzureDC1).</p><p>Name des neuen Benutzers: Geben Sie den Namen eines Benutzers nachschlagen. Diese Benutzer wird ein Mitglied der Gruppe der Administratoren des virtuellen Computers sein. Sie benötigen diesen Namen bei den virtuellen Computer zum ersten Mal anmelden. Das integrierte Konto Administrator funktioniert nicht.</p><p>Neues Kennwort/bestätigen: Geben Sie ein Kennwort</p>
    **Konfiguration des virtuellen Computers**  | <p>Cloud-Dienst: <b>Erstellen einen neuen Cloud-Dienst</b> wählen Sie aus, für den ersten virtuellen Computer und wählen Sie die Cloud-Dienst gleichnamigen aus, wenn Sie weitere virtuellen Computern erstellen, die die Rolle des DC gehostet wird.</p><p>Cloud-Dienst DNS-Name: Geben Sie einen global eindeutigen Namen</p><p>Region/Gruppe Zugehörigkeit/virtuellen Netzwerk: Geben Sie den Namen des virtuellen Netzwerks (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie <b>verwenden ein automatisch generiertes Speicherkonto</b> für den ersten virtuellen Computer aus, und wählen Sie dann die gleichen Speicher Kontonamen aus, wenn Sie weitere virtuellen Computern erstellen, die die Rolle des DC gehostet wird.</p><p>Verfügbarkeit festlegen: Wählen Sie <b>eine Sammlung Verfügbarkeit erstellen</b>aus.</p><p>Verfügbarkeit festlegen Name: Geben Sie einen Namen für die Verfügbarkeit festzulegen, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann aus, die denselben Namen, wenn Sie weitere virtuelle Computer erstellen.</p>
    **Konfiguration des virtuellen Computers**  | <p>Wählen Sie <b>den Agent virtuellen Computer installieren</b> und alle anderen Erweiterungen, die Sie benötigen.</p>
2. Fügen Sie einen Datenträger an jede virtueller Computer, die Funktion des DC-Servers ausgeführt werden kann. Der zusätzliche Datenträger ist erforderlich, die AD-Datenbanken, Protokolle und SYSVOL gespeichert. Geben Sie eine Größe für den Datenträger (z. B. 10 GB), und lassen Sie die **Host-Cache-Einstellung** **keine**festgelegt. Die einzelnen Schritte finden Sie unter [einen Datenträger Daten auf einem Windows-Computer installieren](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Öffnen Sie **Server-Manager**, nachdem Sie zuerst auf den virtuellen Computer anmelden, > **Datei- und Storage Services** zum Erstellen eines Datenträgers auf diesem Datenträger mit NTFS.
4. Reservieren einer statischen IP-Adresse für virtuellen Computern, die die Rolle des DC ausgeführt werden kann. Um eine statische IP-Adresse reservieren möchten, laden Sie die Microsoft Web Platform Installer und [Azure PowerShell installieren](../powershell-install-configure.md) , und führen Sie das Cmdlet "Set-AzureStaticVNetIP". Beispiel:

    "Get-AzureVM - ServiceName AzureDC1-benennen AzureDC1 | Set-AzureStaticVNetIP - IP-Adresse 10.0.0.4 | Update-AzureVM

Weitere Informationen zum Festlegen einer statischen IP-Adresse finden Sie unter [Konfigurieren eines statischen internen IP-Adresse für einen virtuellen Computer](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Installieren von Windows Server Active Directory

Verwenden Sie die gleiche Routine in die Verwendung der lokalen [AD DS installieren](https://technet.microsoft.com/library/jj574166.aspx) (d. h., Sie können die Benutzeroberfläche, eine Antwortdatei oder Windows PowerShell). Sie müssen Administrator angeben, um eine neue Gesamtstruktur installieren. Um den Speicherort für die Active Directory-Datenbank, Protokollen und SYSVOL angeben möchten, ändern Sie den Standardspeicherort vom Betriebssystemlaufwerk, in der zusätzliche Datenträger, den Sie den virtuellen Computer angefügt.

Nach Abschluss die Installation DC Verbinden mit dem virtuellen Computer erneut, und melden Sie sich an den DC. Denken Sie daran, um anzugeben, Anmeldeinformationen für die Domäne.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Zurücksetzen des DNS-Servers für das Azure virtuelle Netzwerk

1. Zurücksetzen der Weiterleitung DNS-Einstellungen auf dem neuen Domänencontroller/DNS-Server.
  1. Klicken Sie im Server-Manager auf **Extras** > **DNS-Einträge**.
  2. **DNS-Manager**mit der rechten Maustaste in des Namens des DNS-Servers ein, und klicken Sie auf **Eigenschaften**.
  3. Klicken Sie auf der Registerkarte **Weiterleitungen** klicken Sie auf die IP-Adresse der Weiterleitung, und klicken Sie auf **Bearbeiten**.  Wählen Sie die IP-Adresse aus, und klicken Sie auf **Löschen**.
  4. Klicken Sie auf **OK** , um ihn zu schließen und **Ok** erneut aus, um die DNS-Server-Eigenschaften zu schließen.
2. Aktualisieren der DNS-Server-Einstellung für das virtuelle Netzwerk an.
  1. Klicken Sie auf **Virtuelle Netzwerke** > doppelklicken Sie auf das virtuelle Netzwerk, die Sie erstellt haben > **Konfigurieren** > **DNS-Server**, geben Sie den Namen und DIP eines der virtuellen Computern, die Rolle des DC/DNS-Server ausgeführt wird, und klicken Sie auf **Speichern**.
  2. Wählen Sie den virtuellen Computer aus, und klicken Sie auf **neu starten** , um den virtuellen Computer zum Konfigurieren von DNS-Auflösung Einstellungen für die IP-Adresse des neuen DNS-Servers auslösen.


## <a name="create-vms-for-domain-members"></a>Erstellen von virtuellen Computern für Domänenmitglieder

1. Wiederholen Sie die folgenden Schritte zum Erstellen von virtuellen Computern als Anwendungsserver ausgeführt. Übernehmen Sie den Standardwert für eine Einstellung, es sei denn, ein anderer Wert vorgeschlagen oder erforderlich ist.

    Auf dieser Seite des Assistenten...  | Geben Sie diese Werte
    ------------- | -------------
    **Wählen Sie ein Bild aus.**  | Windows Server 2012 R2 Datacenter
    **Konfiguration des virtuellen Computers**  | <p>Der Name des virtuellen Computers: Geben Sie einen einzelnes Etikett Namen (beispielsweise AppServer1).</p><p>Name des neuen Benutzers: Geben Sie den Namen eines Benutzers nachschlagen. Diese Benutzer wird ein Mitglied der Gruppe der Administratoren des virtuellen Computers sein. Sie benötigen diesen Namen bei den virtuellen Computer zum ersten Mal anmelden. Das integrierte Konto Administrator funktioniert nicht.</p><p>Neues Kennwort/bestätigen: Geben Sie ein Kennwort</p>
    **Konfiguration des virtuellen Computers**  | <p>Cloud-Dienst: Wählen Sie **Erstellen Sie einen neuen Clouddienst** für den ersten virtuellen Computer und demselben Namen Cloud-Dienst, wenn Sie weitere virtuellen Computern erstellen, die die Anwendung gehostet wird.</p><p>Cloud-Dienst DNS-Name: Geben Sie einen global eindeutigen Namen</p><p>Region/Gruppe Zugehörigkeit/virtuellen Netzwerk: Geben Sie den Namen des virtuellen Netzwerks (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie **verwenden ein automatisch generiertes Speicherkonto** für den ersten virtuellen Computer aus, und wählen Sie dann die gleichen Speicher Kontonamen aus, wenn Sie weitere virtuellen Computern erstellen, die die Anwendung gehostet wird.</p><p>Verfügbarkeit festlegen: Wählen Sie **eine Sammlung Verfügbarkeit erstellen**aus.</p><p>Verfügbarkeit festlegen Name: Geben Sie einen Namen für die Verfügbarkeit festzulegen, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann aus, die denselben Namen, wenn Sie weitere virtuelle Computer erstellen.</p>
    **Konfiguration des virtuellen Computers**  | <p>Wählen Sie <b>den Agent virtuellen Computer installieren</b> und alle anderen Erweiterungen, die Sie benötigen.</p>
2. Nach jeder virtuellen Computer bereitgestellt wird, melden Sie sich an, und verbinden Sie es mit der Domäne. **Server-Manager**, klicken Sie auf **Lokale Server** > **Arbeitsgruppe** > **ändern...** und wählen Sie **Domäne** aus, und geben Sie den Namen Ihrer Domäne lokalen. Geben Sie die Anmeldeinformationen eines Benutzers für die Domäne, und starten Sie den virtuellen Computer, um die Domäne Verknüpfung abzuschließen.

Zum Erstellen der virtuellen Computern anstelle der Benutzeroberfläche mithilfe von Windows PowerShell finden Sie unter [Verwenden von Azure PowerShell zu erstellen und Windows-basierten virtuellen Maschinen vorkonfiguriert](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Weitere Informationen zur Verwendung von Windows PowerShell finden Sie unter [Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx) und [Azure Cmdlet verweisen](https://msdn.microsoft.com/library/azure/jj554330.aspx).


## <a name="see-also"></a>Siehe auch

-  [Wie Sie eine neue Active Directory-Struktur auf ein Azure-virtuellen Netzwerk installieren](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Richtlinien für die Bereitstellung von Windows Server Active Directory auf Azure-virtuellen Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Konfigurieren eines Standort-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Installieren von Active Directory Active Directory in einem Azure-virtuellen Netzwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: (01) virtuellen Computern-Grundlagen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) erstellen virtuelle Netzwerke und Cross lokale Konnektivität](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md)
-  [So installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Festlegen von Azure-virtuellen Computer statische IP-Adresse](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [So Azure-virtuellen Computer statische IP-Adresse zuweisen](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Installieren Sie eine neue Active Directory-Gesamtstruktur](https://technet.microsoft.com/library/jj574166.aspx)
-  [Einführung in Active Directory Domain Services (AD DS) Virtualisierung (Ebene 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
