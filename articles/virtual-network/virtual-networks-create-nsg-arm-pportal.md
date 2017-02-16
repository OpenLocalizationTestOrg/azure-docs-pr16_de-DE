<properties 
   pageTitle="So erstellen Sie in der Cloud-Modus mithilfe des Azure-Portals NSGs | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Bereitstellen von NSGs in der Cloud mithilfe des Azure-Portals"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Zum Verwalten von NSGs über das Azure-portal

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch [NSGs im Bereitstellungsmodell klassischen erstellen](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Im Beispiel unten aufgeführten Befehle eine einfache-Umgebung, die bereits erstellt erwarten PowerShell basierend auf dem Szenario oben. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst erstellen Sie Umgebung für die Bereitstellung von [dieser Vorlage](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klicken Sie auf **Bereitstellen in Azure**, ersetzen Sie den Parameter Standardwerte bei Bedarf, und folgen Sie den Anweisungen im Portal. Die folgenden Schritte aus verwenden **RG-NSG** als den Namen der Ressourcengruppe, die, der auf die Vorlage bereitgestellt wurde.

## <a name="create-the-nsg-frontend-nsg"></a>Erstellen der NSG-Front-End-NSG

Führen Sie die folgenden Schritte aus, um die NSG **NSG-Front-End** erstellen wie oben beschriebenen Szenario dargestellt.

1. Mithilfe eines Browsers und navigieren Sie zu http://portal.azure.com und, falls notwendig, melden Sie sich mit Ihrem Azure-Konto.
2. Klicken Sie auf **Durchsuchen >** > **Netzwerk Sicherheitsgruppen**.

    ![Azure-Portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Klicken Sie in das **Netzwerk Sicherheitsgruppen** Blade auf **Hinzufügen**.
  
    ![Azure-Portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. Erstellen Sie in der Blade- **Netzwerk-Sicherheitsgruppe erstellen** eine benannte *NSG-Front-End* in der Ressourcengruppe *RG-NSG* NSG, und klicken Sie dann auf **Erstellen**.

    ![Azure-Portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Erstellen Sie Regeln in eine vorhandene NSG

Um Regeln in einer vorhandenen NSG vom Azure-Portal zu erstellen, führen Sie die folgenden Schritte aus.

2. Klicken Sie auf **Durchsuchen >** > **Netzwerk Sicherheitsgruppen**.

3. Klicken Sie in der Liste der NSGs auf **NSG-Front-End-** > **eingehende Sicherheitsregeln**

    ![Azure-Portal - NSG-Front-End](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. Klicken Sie in der Liste der **Regeln für eingehende Sicherheit**auf **Hinzufügen**.

    ![Azure-Portal - Regel hinzufügen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. Erstellen einer Regel, die mit dem Namen *Web-Regel* Priorität von *200* festlegen, dass Access über *TCP* Port *80* auf einem beliebigen virtuellen Computer aus jeder Quelle in das Blade **die eingehende Sicherheitsregel hinzufügen** , und klicken Sie dann auf **OK**. Beachten Sie, dass die meisten diese Einstellungen bereits Standardwerte sind.

    ![Azure-Portal - regeleinstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Nach ein paar Sekunden wird die neue Regel in den NSG angezeigt.

    ![Azure-Portal - neue Regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Wiederholen Sie die Schritte 6, um eine eingehende Regel, die mit dem Namen *Rdp-Regel* mit einer Priorität von *250* zulassen des Zugriffs über *TCP* zu Port *3389* zu einem beliebigen virtuellen Computer aus jeder Quelle zu erstellen.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Zuordnen der NSG mit dem Front-End-Subnetz

1. Klicken Sie auf **Durchsuchen >** > **Ressourcengruppen** > **RG-NSG**.
2. Klicken Sie in das Blade **RG-NSG** auf **...**  >  **TestVNet**.

    ![Azure-Portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Klicken Sie in das Blade **Einstellungen** auf **Subnets** > **Front-End** > **Netzwerk-Sicherheitsgruppe** > **NSG-Front-End**.

    ![Azure-Portal - Subnetz Einstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. Klicken Sie in der **Front-End** -Blade auf **Speichern**.

    ![Azure-Portal - Subnetz Einstellungen](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Erstellen der NSG-Back-End-NSG

Zum Erstellen von **NSG-Back-End-** NSG, und ordnen es mit dem **Back-End-** Subnetz, führen Sie die folgenden Schritte aus.

1. Wiederholen Sie die Schritte in [Erstellen der NSG-Front-End-NSG](#Create-the-NSG-FrontEnd-NSG) zum Erstellen einer benannten *NSG-Back-End-* NSG
2. Wiederholen Sie die Schritte in [Erstellen von Regeln in einer vorhandenen NSG](#Create-rules-in-an-existing-NSG) zum Erstellen von **eingehende** Regeln in der folgenden Tabelle aus.

  	|Eingehende Regel|Ausgehende Regel|
  	|---|---|
  	|![Azure-Portal - eingehende Regel](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure-Portal - ausgehende Regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Wiederholen Sie die Schritte in [der NSG mit dem Front-End-Subnetz zuordnen](#Associate-the-NSG-to-the-FrontEnd-subnet) , **NSG-Back-End-** NSG mit dem **Back-End-** Subnetz zugeordnet werden soll.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Verwalten von vorhandenen NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Aktivieren der Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.