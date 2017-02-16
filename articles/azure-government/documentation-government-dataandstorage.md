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
    ms.date="09/30/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-data-and-storage"></a>Azure Government Daten und Speicher

##  <a name="azure-storage"></a>Azure-Speicher

Weitere Informationen zu diesen Dienst und zur gemeinsamen Nutzung finden Sie unter [Öffentliche Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Variationen

Die URLs für Speicherkonten in Azure Government unterscheiden:

Diensttyp|Azure öffentlichen|Azure Government
---|---|---
BLOB-Speicher|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Warteschlangenspeicher|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Table Storage|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Alle Skripts und Code muss die entsprechende Endpunkte zu berücksichtigen.  Finden Sie unter [Konfigurieren von Azure-Speicher Verbindungszeichenfolgen](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Weitere Informationen zu APIs finden Sie unter der <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Cloud Speicher Konto Konstruktor</a>.

Das Endpunkt Suffix zur Verwendung in diese überladenen ist core.usgovcloudapi.net 

### <a name="considerations"></a>Aspekte

Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government Azure Storage an:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daten eingegeben haben, gespeichert und verarbeitet innerhalb einer Azure-Speicher kann Produkt gesteuert exportieren Daten enthalten. Statische Authentifikatoren, z. B. Kennwörter und Smartcard-PIN für den Zugriff auf Azure-Plattformkomponenten. Private Schlüssel von Zertifikaten Azure-Plattformkomponenten zur Verwaltung verwendet wird. Anderen Sicherheit Informationen/vertraulichen, z. B. Zertifikate, Schlüssel für die Verschlüsselung, Master Tasten und Speicher Tasten in Azure Services gespeichert. | Azure Storage Metadata darf nicht gesteuert exportieren Daten enthalten. Diese Metadaten umfasst alle Konfigurationsdaten, die beim Erstellen und Verwalten Ihres Produkts Speicher eingegeben.  Geben Sie in die folgenden Felder nicht Regulated/gesteuert Daten: Ressourcengruppen, Bereitstellung, Ressourcennamen, Resource-Tags  

##  <a name="premium-storage"></a>Premium-Speicher

Weitere Informationen zu diesen Dienst und zur gemeinsamen Nutzung, finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Variationen

Premium-Speicher steht in der Regel in der USGov Virginia. Dies umfasst DS-Serie virtuellen Computern. 

### <a name="considerations"></a>Aspekte

Die dasselbe Speicher Daten aufgeführten gilt für Premium Speicherkonten. 

##  <a name="sql-database"></a>SQL-Datenbank

Zusätzliche Anleitungen Metadaten Sichtbarkeitskonfiguration sowie optimale Methoden Schutz finden Sie in der<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft-Sicherheitscenter für SQL-Datenbank-Engine</a> und [Azure SQL-Datenbank öffentliche Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/) .

### <a name="variations"></a>Variationen

SQL-V12 Datenbank steht in der Regel in Azure Government aus.

Die Adresse für SQL Azure-Servern in Azure Government unterscheidet sich:

Diensttyp|Azure öffentlichen|Azure Government
---|---|---
SQL-Datenbank|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Aspekte

Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government Azure Storage an:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten gespeichert und verarbeitet in Microsoft Azure SQL können Azure Government geregelt Daten enthalten. Verwenden Sie für die Datenübertragung Azure Government geregelt Datenseite Datenbanktools an. | Azure SQL-Metadaten darf nicht gesteuert exportieren Daten enthalten. Diese Metadaten umfasst alle Konfigurationsdaten, die beim Erstellen und Verwalten Ihres Produkts Speicher eingegeben.  Geben Sie in die folgenden Felder nicht geregelt/gesteuert Daten: Datenbank Namen Abonnementname Ressourcengruppen Servernamen Server Administrator Login Bereitstellung Namen Ressourcennamen, Resource-Tags

##  <a name="next-steps"></a>Nächste Schritte

Für zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>
