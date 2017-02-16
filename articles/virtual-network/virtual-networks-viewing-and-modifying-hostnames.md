<properties 
   pageTitle="Anzeigen und Ändern von Hostnamen | Microsoft Azure"
   description="Zum Anzeigen und Ändern von Hostnamen für Azure-virtuellen Computern web und Worker-Rollen für die namensauflösung"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Anzeigen und Ändern von Hostnamen

Damit Ihre Rolleninstanzen von Hostnamen verwiesen werden kann, müssen Sie den Wert für den Hostnamen in der Konfiguration Dienstdatei für jede Rolle festlegen. Sie nicht, indem Sie den Namen der gewünschten Host das Attribut **VmName** des Elements **Rolle** hinzufügen. Der Wert für das Attribut **VmName** wird als Basis für den Hostnamen für jede Instanz der Rolle verwendet. Angenommen, wenn **VmName** *Webrole lautet* , und es drei Instanzen dieser Rolle werden, werden die Hostnamen der Instanzen *webrole0*, *webrole1*und *webrole2*. Sie müssen keinen Hostnamen für virtuelle Computer in der Konfigurationsdatei angeben, da der Hostname für einen virtuellen Computer basierend auf den Namen des virtuellen Computers ausgefüllt wird. Weitere Informationen zum Konfigurieren von einem Microsoft Azure-Dienst finden Sie unter [Azure Service Konfigurationsschema (.cscfg Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Anzeigen von Hostnamen

Sie können die Hostnamen von virtuellen Computern und Rolleninstanzen in einen Cloud-Dienst mithilfe der folgenden Tools anzeigen.

### <a name="azure-portal"></a>Azure-Portal

Sie können das [Azure-Portal](http://portal.azure.com) verwenden, um die Hostnamen für virtuelle Computer auf das Blade Übersicht für einen virtuellen Computer anzuzeigen. Beachten Sie, dass das Blade einen Wert für **Name** und **Hostname zeigt**beibehalten. Obwohl sie anfangs gleich sind, ändert Ändern der Hostname den Namen des virtuellen Computers oder der Rolleninstanz sich nicht.

Rolleninstanzen können auch im Azure-Portal angezeigt werden, wenn Sie die Instanzen in einen Cloud-Dienst Liste der Hostname wird jedoch nicht angezeigt. Einen Namen für jede Instanz werden angezeigt, aber die Namen der Hostname nicht darstellt.

### <a name="service-configuration-file"></a>Konfigurationsdatei-Dienst

Sie können die Konfiguration Dienstdatei für eine bereitgestellte Dienst aus dem Blade **Konfigurieren** des Diensts in der Azure-Portal herunterladen. Sie können dann für das Attribut **VmName** für das Element **Name der Rolle** der Hostname finden Sie unter Suchen. Beachten Sie, dass dieser Hostname als Basis für jede Instanz der Rolle der Hostname verwendet wird beibehalten. Angenommen, wenn **VmName** *Webrole lautet* , und es drei Instanzen dieser Rolle werden, werden die Hostnamen der Instanzen *webrole0*, *webrole1*und *webrole2*.

### <a name="remote-desktop"></a>Remotedesktop

Nachdem Sie Remote Desktop (Windows), Windows PowerShell Remote (Windows) oder SSH (Linux und Windows)-Verbindungen zu Ihrem virtuellen Computern oder Rolleninstanzen aktiviert haben, können Sie den Hostnamen aus einer aktiven Remotedesktop Verbindung auf verschiedene Weise anzeigen:

- Geben Sie bei der Eingabeaufforderung oder SSH Terminal Hostname ein.

- Geben Sie Ipconfig/auffordern Sie, den Befehl vollständiges (nur Windows).

- Anzeigen der Name des Computers in den Systemeinstellungen (nur Windows).

### <a name="azure-service-management-rest-api"></a>Azure Servicemanagement REST-API

Gehen Sie von einem REST-Client wie folgt vor:

1. Stellen Sie sicher, dass Sie ein Client-Zertifikat Verbindung Azure-Portal haben. Um ein Client-Zertifikat zu erhalten, führen Sie die Schritte, die in [wie: herunterladen und Importeinstellungen veröffentlichen und Abonnementinformationen](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Legen Sie einen Kopfzeile-Eintrag mit dem Namen X-ms-Version mit einem Wert der 2013-11-01.

1. Senden Sie eine Besprechungsanfrage in folgendem Format: https://management.core.windows.net/\<Subscrition-Id\>/services/Hostedservices/\<Service-Name\>?embed-Detail = WAHR

1. Suchen Sie das Element **HostName** für jedes Element **RoleInstance** .

>[AZURE.WARNING] Sie können auch das Suffix internen Domänennamen für Ihre Cloud-Dienst aus der restlichen Anruf Antwort, indem Sie das Element **InternalDnsSuffix** aktivieren, oder führen Sie Ipconfig/alle über eine Befehlszeile, in einer Sitzung Remote Desktop (Windows) oder laufenden Katze /etc/resolv.conf aus einer SSH Terminal (Linux) anzeigen.

## <a name="modifying-a-hostname"></a>Ändern einer hostname

Sie können den Hostnamen virtuellen Computern oder Instanz der Rolle durch eine geänderte Konfiguration Dienstdatei hochladen oder durch Umbenennen des Computers aus einer Sitzung Remotedesktop ändern.

## <a name="next-steps"></a>Nächste Schritte

[Mit einer namensauflösung von (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure Service Konfigurationsschema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Die Konfigurationsschema Azure virtuelles Netzwerk](http://go.microsoft.com/fwlink/?LinkId=248093)

[Festlegen der DNS-Einstellungen mit Netzwerk Konfigurationsdateien](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
