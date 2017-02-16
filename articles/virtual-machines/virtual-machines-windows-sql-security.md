<properties
    pageTitle="Sicherheitsaspekte für SQLServer in Azure | Microsoft Azure"
    description="In diesem Thema bezieht sich auf Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt und sind allgemeine Hinweise zum Sichern von SQL Server in einer Azure-virtuellen Computern ausgeführt."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Sicherheitsaspekte für SQLServer in Azure-virtuellen Computern
 
Dieses Thema enthält allgemeine Sicherheitsrichtlinien, die Ihnen helfen, sicheren Zugriff auf SQL Server-Instanzen einer Azure-virtuellen Computer herstellen. Um eine bessere Schutz zu Instanzen der SQL Server-Datenbank in Azure zu gewährleisten, empfehlen wir jedoch, dass Sie die herkömmlichen lokalen Sicherheitsmaßnahmen zusätzlich zu best Practices für die Sicherheit für Azure implementieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Weitere Informationen zu Practices für die SQL Server-Sicherheit finden Sie unter [SQL Server 2008 R2 Sicherheit Best Practices - Betrieb und Verwaltungsaufgaben](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure – Konformität mit mehreren branchenspezifische Vorschriften und Standards, die Sie zum Erstellen einer kompatiblen Lösung mit SQL Server auf einem virtuellen Computer ausführen aktivieren können. Informationen zur Einhaltung gesetzlicher Vorschriften mit Azure finden Sie unter [Azure Trust Center](https://azure.microsoft.com/support/trust-center/).

Es folgt eine Liste der Mittelpunkt, die beim Konfigurieren und Herstellen einer Verbindung mit der Instanz von SQL Server auf eine Azure-virtuellen Computer berücksichtigt werden sollten.

## <a name="considerations-for-managing-accounts"></a>Faktoren für das Verwalten von Konten:

- Erstellen Sie ein lokales Administratorkonto eindeutige, das nicht **Administrator**heißt.

- Verwenden Sie komplexe sicherer Kennwörter für alle Konten aus. Weitere Informationen dazu, wie Sie ein sicheres Kennwort, [Tipps zum Erstellen einer sicherer Kennwörter](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) finden Sie im Artikel erstellen.

- Standardmäßig wählt Azure Windows-Authentifizierung während der Installation von SQL Server-virtuellen Computern an. Daher ist der Benutzername **SA** deaktiviert und ein Kennwort von Setup zugeordnet ist. Wir empfehlen, die der Benutzernamen **SA** sollten nicht verwendet oder aktiviert. Im folgenden sind alternative Strategien gegebenenfalls eine SQL-Anmeldung:
    - Erstellen Sie ein SQL-Konto, das Mitglied von Sysadmin ist.
    - Wenn Sie einen Benutzernamen **SA** verwenden möchten, aktivieren Sie die Anmeldung und Namen Sie, und weisen Sie ein neues Kennwort zu.
    - Sowohl die Optionen, die zuvor erwähnt wurden erfordern eine Änderung der Authentifizierungsmodus mit **SQL Server und Windows-Authentifizierungsmodus**. Weitere Informationen finden Sie unter [Server-Authentifizierungsmodus ändern](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Überlegungen zum Sichern von Verbindungen mit Azure-virtuellen Computern:

- Erwägen Sie [Azure-virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md) den virtuellen Computern anstelle von öffentlichen RDP Ports verwalten.

- Verwenden einer [Sicherheitsgruppe Netzwerk](../virtual-network/virtual-networks-nsg.md) (NSG) zulassen oder verweigern Netzwerkverkehr zu Ihrem virtuellen Computern an. Wenn Sie eine NSG verwenden und einen Endpunkt ACL bereits angeordnet haben möchten, entfernen Sie zuerst den Endpunkt ACL. Informationen hierzu finden Sie unter [Verwalten von Access Control Lists (ACLs) für die Endpunkte mithilfe der PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Wenn Sie Endpunkte verwenden, entfernen Sie beliebigen Endpunkten des virtuellen Computers, wenn Sie nicht verwenden. Hinweise zur Verwendung von ACLs mit Endpunkten sind finden Sie unter [Verwalten der ACL für einen Endpunkt](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Aktivieren Sie eine Option verschlüsselte Verbindung für eine Instanz von SQL Server-Datenbank-Engine in Azure virtuellen Computern an. Konfigurieren von SQL Server-Instanz mit einem signierten Zertifikat. Weitere Informationen finden Sie unter [Verschlüsselt Verbindungen aktivieren, um die Datenbank-Engine](https://msdn.microsoft.com/library/ms191192.aspx) und die [Syntax der Verbindungszeichenfolge](https://msdn.microsoft.com/library/ms254500.aspx).

- Wenn Ihre virtuellen Computern nur von einem bestimmten Netzwerk zugegriffen werden soll, verwenden Sie die Windows-Firewall zum Einschränken des Zugriffs auf bestimmte IP-Adressen oder Netzwerksubnets aus.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie auch bewährte Methoden für die Leistung interessiert sind, finden Sie unter [Leistung bewährte Methoden für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-performance.md).

Weitere Themen im Zusammenhang mit dem SQL Server in Azure-virtuellen Computern ausgeführt wird finden Sie unter [SQL Server auf Azure-virtuellen Computern Übersicht](virtual-machines-windows-sql-server-iaas-overview.md).
