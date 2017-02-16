<properties
   pageTitle="Vergleich von Azure Lake Datenspeicher mit BLOB-Speicher von Azure | Microsoft Azure"
   description="Vergleich von Azure Lake Datenspeicher mit BLOB-Speicher von Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergleich von Azure Lake Datenspeicher und Azure Blob-Speicher

Die Tabelle in diesem Artikel werden die Unterschiede zwischen Azure Lake Datenspeicher und Azure BLOB-Speicher entlang einige wichtige Aspekte des big Datenverarbeitung zusammengefasst. Azure Blob-Speicher ist eine allgemeine skalierbare Objekt Store, die für eine Vielzahl von Speicher Szenarien vorgesehen ist. Azure Lake Datenspeicher ist ein hyper-Skala Repository, die für große Daten Analytics Auslastung optimiert ist.

|    | Azure Lake Datenspeicher  | Azure Blob-Speicher  |
|----|-----------------------|--------------------|
| Zweck  | Optimierte Speicherung von big Data Analytics Auslastung                                                                          | Allgemeine Objekt für eine Vielzahl von Speicher Szenarien speichern                                                                                |
| Verwenden von Fällen                                   | Stapelverarbeitung, interaktive, streaming Analytics und Computer Learning Daten wie Protokolldateien, IoT Daten, klicken Sie auf Streams, große datasets | Alle Arten von Texten oder binären Daten, wie z. B. Anwendung sichern beenden, zusätzliche Daten, Medien-Speicher für streaming und allgemeine Zweck von Daten |
| Grundlegende Konzepte                                | Lake Datenspeicher Konto enthält Ordner, der enthält wiederum als Dateien gespeicherten Daten | Speicherkonto verfügt Container, der wiederum Daten in Form von Blobs sind |
| Struktur | Hierarchisches Dateisystem                                                                                                    | Objekt Store mit flachen namespace  |
| API   | REST-API über HTTPS | REST-API über HTTP/HTTPS                                                                                                                            |
| Serverseitige API                             | [WebHDFS kompatiblen REST-API](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure Blob Storage REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoop-Datei System Client                   | Ja                                                                                                                         | Ja                                                                                                                                                 |
| Datenoperationen - Authentifizierung            | Basierend auf [Identitäten Azure-Active Directory](../active-directory/active-directory-authentication-scenarios.md) | Basierend auf gemeinsame Kennwörter - [Konto Zugriffstasten](../storage/storage-create-storage-account.md#manage-your-storage-account) und [Freigegebenen Zugriffstasten Signatur](../storage/storage-dotnet-shared-access-signature-part-1.md).                                                                       |
| Datenoperationen - Authentication-Protokoll     | OAuth 2.0. Anrufe müssen eine gültige JWT (JSON Web Token) ausgestellt von Azure Active Directory enthalten.                     | Hashbasierten Meldungsauthentifizierungscode (HMAC). Anrufe müssen einen Base64-codierte SHA-256 Hash über einen Teil der HTTP-Anforderung enthalten. |
| Datenoperationen - Autorisierung               | POSIX Access Control Lists (ACLs).  Basierend auf Azure Active Directory Identitäten ACLs können Datei- und Ordnernamen Ebene festgelegt werden. | Für das Konto Ebene Autorisierung – für das [Konto-Tastenkombinationen](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Für Firmen-, Container oder Blob Autorisierung - Verwendung [Freigegeben Zugriffstasten Signatur](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Datenoperationen - Überwachung                    | Verfügbar. Finden Sie [hier](data-lake-store-diagnostic-logs.md) Informationen.                                                                                                                   | Verfügbar                                                                                                                                           |
| Statische Verschlüsselungsdaten                     | Transparente, serverseitigen (in Kürze verfügbar)<ul><li>Mit Dienst verwaltet Tasten</li><li>Mit Kunden verwaltete Tasten in Azure KeyVault</li></ul>| <ul><li>Transparente, serverseitigen</li> <ul><li>Mit Dienst verwaltet Tasten</li><li>Mit Kunden verwaltete Tasten in Azure KeyVault (in Kürze verfügbar)</li></ul><li>Clientseitige Verschlüsselung</li></ul> |
| Management-Vorgänge (z. B. Konto erstellen) | [Rollenbasierte Access-Steuerelement](../active-directory/role-based-access-control-what-is.md) (RBAC) aus Azure zur Verwaltung von Konten                                                                       | [Rollenbasierte Access-Steuerelement](../active-directory/role-based-access-control-what-is.md) (RBAC) aus Azure zur Verwaltung von Konten                                                                                               |
| Entwicklertools SDKs                              | .NET, Java, Node.js                                                                                                         | .NET, Java, Python, Node.js, C++, Ruby                                                                                                              |
| Analytics Arbeitsbelastung Leistung              | Optimierte Performance für parallele Analytics Auslastung. Hoher Durchsatz und IOPS.                                           | Nicht optimiert für Analytics Auslastung                                                                                                               |
| Einschränkungen bei der Dateigröße                                 | Keine Einschränkungen beim Konto Größen, Dateigrößen oder Anzahl von Dateien                                                                   | Bestimmte Grenzwerte dokumentierten [hier](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geo-Redundanz                              | Lokal redundant (mehrere Kopien von Daten in einem Azure Region)                                                             | Lokal redundante (LRS) Global redundante (GRS), Lesezugriff Global redundante (RAS GRS). Finden Sie [hier](../storage/storage-redundancy.md) Weitere Informationen |
| Dienststatus                               | Public Preview-Version                                                                                                              | In der Regel verfügbar                                                                                                                                 |
| Landes-/ Verfügbarkeit  | Finden Sie [hier](https://azure.microsoft.com/regions/#services)| Finden Sie [hier](https://azure.microsoft.com/regions/#services) |
| Kurs                                       |     Finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/data-lake-store/)| Finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Lake Datenspeicher](data-lake-store-overview.md)
- [Erste Schritte mit Lake Datenspeicher](data-lake-store-get-started-portal.md)



