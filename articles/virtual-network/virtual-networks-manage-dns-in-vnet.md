<properties 
   pageTitle="Verwalten von DNS-Server, eine virtuelle Netzwerk (VNet) verwendet wird"
   description="Informationen Sie zum Hinzufügen und Entfernen von DNS-Server in einem virtuellen Netzwerk (Vnet)"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Verwalten von DNS-Server, eine virtuelle Netzwerk (VNet) verwendet wird

Sie können die Liste der in einer VNet im Verwaltungsportal oder in der Konfigurationsdatei Netzwerk verwendeten DNS-Server verwalten. Sie können bis zu 12 DNS-Server für jede VNet hinzufügen. Wenn Sie DNS-Server angeben, ist es wichtig, um sicherzustellen, dass Sie Ihre DNS-Server in der richtigen Reihenfolge für Ihre Umgebung Liste. DNS-Serverlisten funktionieren nicht Round-Robert. Sie werden in der Reihenfolge verwendet, die sie angegeben sind. Ist der erste DNS-Server in der Liste erreicht werden können, wird der Client verwendet die DNS-Server unabhängig davon, ob der DNS-Server ordnungsgemäß oder nicht funktioniert. Um die Reihenfolge DNS-Server für Ihre virtuelle Netzwerk zu ändern, entfernen Sie die DNS-Server aus der Liste, und fügen Sie sie wieder in der, die von Ihnen gewünschten Reihenfolge.

>[AZURE.WARNING] Nachdem die DNS-Liste aktualisiert wurde, müssen Sie den virtuellen Computern in Ihrem Netzwerk virtuelle ansässig ist, dass sie der neuen DNS Server Settings abholen starten. Virtuellen Computern weiterhin ihre aktuelle Konfiguration verwenden, bis sie neu gestartet werden.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Bearbeiten einer Liste von DNS-Servern für ein virtuelles Netzwerk Verwaltungsportal verwenden

1. Melden Sie sich im **Verwaltungsportal**.

1. Klicken Sie auf **Netzwerke**im Navigationsbereich, und klicken Sie dann auf den Namen des virtuellen Netzwerks in der Spalte **Name** .

1. Klicken Sie auf **Konfigurieren**.

1. In der **DNS-Server**können Sie Folgendes konfigurieren:

    - **Registrieren (hinzufügen) eine neue DNS-Server –** Geben Sie einfach den Namen und die IP-Adresse in die Felder aus. Fügt einen DNS-Server mit Ihrem virtuelle Netzwerk Liste DNS-Server, und auch den DNS-Server mit Azure registriert.

    - **Hinzufügen eines DNS-Servers, die zuvor registrierte –** Wenn Sie bereits einen DNS-Server mit Azure registriert haben, können Sie ihn aus der Liste vorab eingetragenen auswählen.

    - **So entfernen Sie einen DNS-Server aus dem virtuellen Netzwerk –** Klicken Sie auf das X neben dem Server, die, den Sie entfernen möchten. Beachten Sie, dass dies nur den Server aus dieser Liste virtuelles Netzwerk entfernt. Der DNS-Server bleibt registrierten in Azure für Ihre anderen virtuelle Netzwerke zu verwenden. Wenn Sie einen DNS-Server aus Ihrem Abonnement löschen möchten, wechseln Sie zur Seite **Netzwerken -> DNS-Server** .

    - **Um die Reihenfolge der DNS-Server –** Entfernen Sie alle DNS-Server, die aufgeführt sind, und fügen Sie sie dann in der Reihenfolge wieder die gewünschte. Denken Sie daran, dass es sich nicht um eine Liste der Funktion RUNDEN-Robert DNS handelt.

    - **So benennen Sie einen DNS-Server – um** Markieren Sie den DNS-Server in der Liste aus, und geben Sie den neuen Namen ein. Dies wird einen neuen DNS-Server in Azure registrieren sowie zu der Liste der DNS-Server für Ihre virtuelle Netzwerk hinzuzufügen. Der alten DNS-Server und seine IP-Adresse bleibt für Azure registriert ist. Sie können sie auf der Seite **DNS-Server** löschen, wenn Sie es nicht für alle anderen virtuelle Netzwerke verwenden.

1. Klicken Sie auf am unteren Rand der Seite **Speichern** , um die neue Konfiguration ein DNS-Server zu speichern.

1. Starten Sie den virtuellen Computern befindet sich in das virtuelle Netzwerk aus, um die neuen DNS-Einstellungen erfassen können.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Bearbeiten einer Liste von DNS-Servern mit einer Konfigurationsdatei Netzwerk

Um eine Liste der DNS-Server mithilfe einer Netzwerk-Konfigurationsdatei zu bearbeiten, werden Sie zuerst Ihre Einstellungen Konfiguration aus dem Verwaltungsportal exportieren. Sie klicken Sie dann im Netzwerk Konfigurationsdatei bearbeiten und rückwärts Verwaltungsportal zu importieren. Es folgt eine Liste auf hoher Ebene Schritte aus, um den Vorgang abzuschließen.

1. Exportieren Sie Ihre Netzwerkeinstellungen virtuelle in eine Konfigurationsdatei Netzwerk ein. Weitere Informationen und Schritte zum Exportieren der Konfiguration Netzwerkeinstellungen finden Sie unter [Virtuelle Netzwerkeinstellungen in einem Netzwerk Konfigurationsdatei exportieren](virtual-networks-using-network-configuration-file.md).

1. Geben Sie die DNS-Serverinformationen für das virtuelle Netzwerk an. Weitere Informationen zum Festlegen von eines DNS-Servers finden Sie unter [Festlegen eines DNS-Servers in einer virtuellen Netzwerk Konfigurationsdatei](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Weitere Informationen zu Netzwerk-Konfigurationsdateien finden Sie unter [Azure virtuelle Netzwerk Konfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100.aspx) und [einem virtuellen Netzwerk mithilfe einer Netzwerk-Konfigurationsdatei konfigurieren](virtual-networks-using-network-configuration-file.md).

1. Importieren der Konfigurationsdatei Netzwerk an. Weitere Informationen sowie mögliche Schritte, um Ihr Netzwerk Konfiguration zu importieren finden Sie unter [Importieren einer Konfigurationsdatei Netzwerk](virtual-networks-using-network-configuration-file.md).

1. Starten Sie den virtuellen Computern befindet sich in das virtuelle Netzwerk aus, um die neuen DNS-Einstellungen erfassen können.
