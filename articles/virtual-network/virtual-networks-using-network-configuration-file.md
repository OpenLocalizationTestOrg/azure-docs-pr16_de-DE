<properties 
    pageTitle="Konfigurieren eines virtuellen Netzwerks mit einer Konfigurationsdatei Netzwerk" 
    description="Anweisungen zum Exportieren und Importieren einer Konfigurationsdatei Netzwerk zu den Azure-Verwaltungsportal um erstellen oder Ändern von virtuellen Netzwerken. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Konfigurieren eines virtuellen Netzwerks mit einer Konfigurationsdatei Netzwerk

Sie können ein virtuelles Netzwerk (VNet) mithilfe der Azure-Verwaltungsportal oder mithilfe einer Konfigurationsdatei Netzwerk konfigurieren.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Erstellen und Ändern einer Konfigurationsdatei Netzwerk 
Die einfachste Möglichkeit zum Erstellen einer Konfigurationsdatei Netzwerk ist so exportieren Sie die Einstellungen im Netzwerk aus einer vorhandenen virtuelle Netzwerkkonfiguration, ändern Sie die Datei, um die Einstellungen enthalten, die Sie für Ihre virtuelle Netzwerke konfigurieren möchten.

Zum Bearbeiten der Konfigurationsdatei Netzwerk können Sie einfach öffnen Sie die Datei, nehmen Sie die entsprechenden Änderungen und speichern Sie die Datei. Alle *XML-* Editor können Sie Änderungen an der Konfigurationsdatei Netzwerk vornehmen. 

Sie sollten eng die Anleitung für die [Konfiguration Datei Schema Netzwerkeinstellungen](https://msdn.microsoft.com/library/azure/jj157100.aspx)befolgen. 

Azure betrachtet ein Subnetz, die eine Funktion bereitgestellt wurden als **verwenden**kann. Wenn ein Subnetz verwendet wird, kann es nicht geändert werden. Vor dem ändern, verschieben Sie alle Elemente, die Sie mit dem Subnetz in ein anderes Subnetz bereitgestellt haben, die geändert wird, nicht zur Verfügung.   [Verschieben eines virtuellen Computers oder eine Instanz der Rolle in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md)finden Sie unter.

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Exportieren und Importieren von virtuellen Netzwerkeinstellungen Verwaltungsportal verwenden  
Sie können importieren und Exportieren von deren Einstellungen im Netzwerk Konfiguration in Ihrem Netzwerk Konfigurationsdatei mithilfe der PowerShell oder Verwaltungsportal enthalten. Die folgenden Anweisungen hilft exportieren und Importieren mit Verwaltungsportal Ihnen. 

### <a name="to-export-your-network-settings"></a>So exportieren Sie Ihre Netzwerkeinstellungen
Beim Exportieren, werden alle Einstellungen für die virtuellen Netzwerke im Rahmen Ihres Abonnements in eine XML-Datendatei geschrieben werden. 

1. Melden Sie sich im **Verwaltungsportal**.
2. Klicken Sie im Verwaltungsportal an den unteren Rand der Seite **Netzwerke** auf **Exportieren**. 
3. Stellen Sie sicher, dass Sie das Abonnement ausgewählt haben, für das Sie Ihre Netzwerkeinstellungen exportieren möchten, klicken Sie auf das Fenster **Netzwerkkonfiguration exportieren** . Klicken Sie dann auf das Häkchen klicken Sie auf der unteren rechten Ecke. 
4. Wenn Sie aufgefordert werden, speichern Sie die Datei *NetworkConfig.xml* an die Position Ihrer Wahl.


### <a name="to-import-your-network-settings"></a>So importieren Sie Ihre Netzwerkeinstellungen

1. Klicken Sie im **Verwaltungsportal**im Navigationsbereich auf der unteren linken auf **neu**.
2. Klicken Sie auf **Netzwerkdienste** -> **virtuelles Netzwerk** -> **Konfiguration importieren**.
3. Klicken Sie auf der Seite **Importieren der Konfigurationsdatei Netzwerk** navigieren Sie zu Ihrem Netzwerk Konfigurationsdatei, und klicken Sie dann auf **den Pfeil nach rechts** .
4. Klicken Sie auf der Seite **Erstellen Ihres Netzwerks** sehen Sie die Informationen auf dem Bildschirm angezeigt, welche Abschnitte der Netzwerkkonfiguration geändert oder erstellt werden. Wenn die Änderungen falsch ist, den Sie suchen möchten, klicken Sie auf das Häkchen, um den Vorgang fortzusetzen, aktualisieren oder Erstellen von virtuellen Netzwerks. 