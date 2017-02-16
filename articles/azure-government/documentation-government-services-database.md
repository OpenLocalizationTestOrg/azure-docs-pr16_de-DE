<properties
    pageTitle="Azure Government Dokumentation | Microsoft Azure"
    description="Dies stellt einen Vergleich der Features und Hinweise zur Entwicklung von Applications für Azure Government"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Government Azure-Datenbanken

##  <a name="sql-database"></a>SQL-Datenbank

Zusätzliche Anleitungen Metadaten Sichtbarkeitskonfiguration sowie optimale Methoden Schutz finden Sie in der<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft-Sicherheitscenter für SQL-Datenbank-Engine</a> und [Azure SQL-Datenbank öffentliche Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/) .

### <a name="variations"></a>Variationen

SQL-V12 Datenbank steht in der Regel in Azure Government aus.

Die Adresse für SQL Azure-Servern in Azure Government unterscheidet sich:

Diensttyp|Azure öffentlichen|Azure Government
---|---|---
SQL-Datenbank|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Aspekte

Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government für SQL Azure:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten gespeichert und verarbeitet in Microsoft Azure SQL können Azure Government geregelt Daten enthalten. Verwenden Sie für die Datenübertragung Azure Government geregelt Datenseite Datenbanktools an. | Azure SQL-Metadaten darf nicht gesteuert exportieren Daten enthalten. Diese Metadaten umfasst alle Konfigurationsdaten, die beim Erstellen und Verwalten Ihres Produkts Speicher eingegeben.  Geben Sie in die folgenden Felder nicht geregelt/gesteuert Daten: Datenbank Namen Abonnementname Ressourcengruppen Servernamen Server Administrator Login Bereitstellung Namen Ressourcennamen, Resource-Tags

## <a name="azure-redis-cache"></a>Azure Redis Cache

Weitere Informationen zu diesen Dienst und zur gemeinsamen Nutzung finden Sie unter [Öffentliche Dokumentation Azure Redis Cache](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Variationen

Die URLs für den Zugriff auf und Verwalten von Azure Redis Cache in Azure Government unterscheiden:

Diensttyp|Azure öffentlichen|Azure Government
---|---|---
Cache Endpunkt|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure-portal|https://Portal.Azure.com|https://Portal.Azure.US

>[AZURE.NOTE] Alle Skripts und Code muss die entsprechende Endpunkte und Umgebungen zu berücksichtigen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Azure Government Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Aspekte

Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government für Azure Redis Cache:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten gespeichert und in Azure Redis Cache verarbeitet können Azure Government geregelt Daten enthalten. | Azure Redis Cache Metadaten darf nicht gesteuert exportieren Daten enthalten. Geben Sie in die folgenden Felder nicht geregelt/gesteuert Daten: Name, VNET Name, Abonnementname, Ressourcengruppen, Ressourcen Kategorien, Redis Eigenschaften Zwischenspeichern.  

##  <a name="next-steps"></a>Nächste Schritte

Für zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>
