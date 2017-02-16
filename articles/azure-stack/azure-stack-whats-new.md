<properties
    pageTitle="Neuigkeiten in Azure Stapel | Microsoft Azure"
    description="Was ist neu in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Was ist neu in Azure Stapel Technical Preview 2
Diese Version bietet neue Features für sowohl Mandanten und Administratoren.

## <a name="network"></a>Netzwerk   
   - [iDNS](azure-stack-understanding-dns-in-tp2.md) bietet internen Netzwerk Name Registration wird und Auflösung (DNS = Domain Name System), ohne zusätzliche DNS-Infrastruktur.
   - [Virtuellen Netzwerkgateways](azure-stack-create-vpn-connection-one-node-tp2.md) bieten VPN-Konnektivitätsoptionen zu Azure oder lokalen Ressourcen.
   - Benutzer definiert leitet können Netzwerkdatenverkehr über Firewalls, Sicherheit, andere Geräte und andere Dienste weiterleiten.
   - Sie können Netzwerk-Ressourcen aus dem Marketplace erstellen.   

## <a name="storage"></a>Speicher
 - [Azure Warteschlangen](https://msdn.microsoft.com/library/dd179353.aspx) zuverlässig und beständigen Service aktivieren messaging.
 - [Speicher Analytics](https://msdn.microsoft.com/library/azure/hh343270.aspx) erfassen Speicher Leistungsdaten. Sie können diese Daten zum Verfolgen von Anfragen und Analysieren von Verwendungstrends diagnostizieren Probleme mit Ihrem Speicherkonto verwenden.
 - BLOB-Speicher unterstützt [Blockieren Vorgang angefügt werden soll](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Unterstützung für Premium Speicher-API-Konten.
 - Mandanten Sie Speicher Dienst Unterstützung für allgemeine Tools und SDK, z. B. Azure CLI, PowerShell, .NET, Python und Java SDK. 
 - [Speicher Konto freigegeben Access Signatur](https://msdn.microsoft.com/library/azure/mt584140.aspx) ermöglichen genaue Delegierung des Zugriffs auf Ihre Speicherdienste, ohne dass Ihren vollständigen kontoschlüssel freigeben.  
 - Speicherdienste verwenden jetzt [Gruppe verwaltete Dienstkonten](https://technet.microsoft.com/library/hh831477.aspx) für eine hohe Sicherheit mit niedriger Verwaltungsaufwand an.
 - Sie können nicht verwendete Mandanten Ressourcen bei Bedarf freizugeben.
 - Gelöschte Speicher Konto Aufbewahrung Länge kann konfiguriert werden.
 - Sie können die gelöschten Mandanten Speicherkonten wiederherstellen.

## <a name="compute"></a>Berechnen
- Sie können virtuellen Computern freigeben.
- Sie können virtuellen Computern Erweiterungen Zwecken Management Problembehandlung und Konfiguration erneut bereitstellen.
- Sie können Datenträger virtueller Computer die Größe ändern.
- Virtuellen Computern können mehrere Netzwerkschnittstellen aufweisen.

### <a name="portal-experience"></a>Portal-Benutzeroberfläche
 - Azure Stapel Regionen sind eine logische Einheit skalieren und Management innerhalb von Azure Stapel. In dieser Vorschau können Sie die Informationen auf Dienste wie Datenverarbeitung, Netzwerk und Speicher nach Region anzeigen.
 - Sie können nun die Benutzeroberfläche (Updates) [Azure Stapel updates.md] Vorschau anzeigen.

## <a name="key-vault"></a>Key Tresor
- [Taste Tresor in Azure Stapel](azure-stack-kv-intro.md) bietet sichere Verwaltung Ihrer Schlüssel und Kennwörter für Cloud-apps.
- Sie können Verwendung von apps und virtuellen Computern Schlüssel überwachen und überwachen.

## <a name="billing-and-usage"></a>Abrechnung und Verwendung
 - [Abrechnung und Verbrauch APIs](azure-stack-billing-and-chargeback.md) verfügbar machen Daten wie Ihrer Dienste genutzt werden.  
 - Delegierte Anbieter aktivieren Händler, Ihrer Dienste Azure Stapel an ihre Kunden anzubieten.

## <a name="monitoring-and-health"></a>Für die Überwachung und Dienststatus
 - Können Sie neue Funktionen zur Verfügung stehen, in dem Portal und dem APIs für die Überwachung die vorausschauende anzeigen und Verwalten von Benachrichtigungen für Ihre Umgebung.  

## <a name="next-steps"></a>Nächste Schritte
- [Grundlegendes zu Azure Stapel Prüfung des Konzepts ist Architektur](azure-stack-architecture.md)      
- [Grundlegendes zu Voraussetzungen für die Bereitstellung](azure-stack-deploy.md)
- [Bereitstellen von Azure Stapel](azure-stack-run-powershell-script.md)

  
