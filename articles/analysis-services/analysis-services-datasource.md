<properties
   pageTitle="Datenquelle Verbindungen | Microsoft Azure"
   description="Beschreibt datenquellenverbindungen für Datenmodelle in Azure Analysis Services."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Datenquelle Verbindungen

Datenmodelle in Azure Analysis Services möglicherweise unterschiedliche Datenanbieter vorzuschreiben auf bestimmte Datenquellen. In einigen Fällen möglicherweise tabellarische Modellen Herstellen einer Verbindung mit Datenquellen mithilfe der systemeigenen Anbieter wie etwa SQL Server Native Client (SQLNCLI11), einen Fehler zurück.

Beispielsweise; oder, wenn Sie eine in-Memory direkte Abfrage Modellieren von Daten, die eine Verbindung herstellt mit einer Datenquelle Cloud wie SQL Azure-Datenbank, wenn Sie als SQLOLEDB systemeigenen Anbieter verwenden sehen Sie möglicherweise Fehlermeldung: **"Die Anbieter, die 'SQLNCLI11.1' ist nicht registriert"**.

Oder, wenn ein DirectQuery-Modell, das Herstellen einer Verbindung mit einer lokalen Datenquellen, wenn Sie systemeigene-Anbieter verwenden, sind möglicherweise Fehlermeldung angezeigt: **"Fehler beim Erstellen des OLE DB Zeile festlegen. Falsche Syntax bei 'LIMIT' "**.

## <a name="data-source-providers"></a>Datenanbieter für die Datenquelle

Die folgende Datenquelle-Anbieter für in-Memory unterstützt werden direkte Datenmodelle Abfrage aus, bei der Verbindung mit einer lokalen oder cloud-Datenquellen:

|               | **Datenquelle**                     | **Im Arbeitsspeicher**                            |  **Direkte Abfrage**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Cloud**                     | SQL Azure Datawarehouse      | .NET Framework-Datenanbieter für SQLServer | .NET Framework-Datenanbieter für SQLServer |
|                           | SQL Azure-Datenbank            | .NET Framework-Datenanbieter für SQLServer | .NET Framework-Datenanbieter für SQLServer |
| **Lokal** (über Gateway) | SqlServer                    | SQL Server Native Client 11.0               | .NET Framework-Datenanbieter für SQLServer |
|                           |  SqlServer                             | Microsoft OLE DB-Anbieter für SQLServer    |   .NET Framework-Datenanbieter für SQLServer                                          |
|                           |  SqlServer                             | .NET Framework-Datenanbieter für SQLServer |  .NET Framework-Datenanbieter für SQLServer                                           |
|                           | Oracle                        | Microsoft OLE DB-Anbieter für Oracle        | Oracle-Datenanbieter für .NET               |
|                           |  Oracle                             | Oracle-Datenanbieter für .NET               | Oracle-Datenanbieter für .NET                                            |
|                           | Teradata                      | OLE DB-Anbieter für Teradata                | Teradata-Datenanbieter für .NET             |
|                           |  Teradata                             | Teradata-Datenanbieter für .NET             |  Teradata-Datenanbieter für .NET                                            |
|                           | Analytics-Plattform-System | .NET Framework-Datenanbieter für SQLServer | .NET Framework-Datenanbieter für SQLServer |


> [AZURE.NOTE] Stellen Sie sicher, dass bei der lokalen Gateway mit 64-Bit-Anbieter installiert werden.

Beim Migrieren von einer lokalen SQL Server Analysis Services-Tabellenmodell mit Azure Analysis Services möglicherweise der Anbieter geändert werden.

**Angabe eines Anbieters für die Datenquelle**

1. In SSDT > **Tabellarischen Modell-Explorer** > **Datenquellen**, mit der rechten Maustaste in einer Datenquellenverbindungs, und klicken Sie dann auf **Datenquelle bearbeiten**.

2. Klicken Sie unter **Verbindung bearbeiten**klicken Sie auf **Erweitert** , um das nächste Eigenschaften-Fenster zu öffnen.

3. **Erweiterte**Eigenschaften festlegen > **Anbieter**, und wählen Sie dann den entsprechenden Anbieter.

## <a name="impersonation"></a>Identitätswechsel
In einigen Fällen ist es erforderlich, geben Sie ein Konto für den anderen Identitätswechsel wahrscheinlich aus. Konto für den Identitätswechsel kann in SSDT oder SSMS angegeben werden muss.

Klicken Sie auf lokale Datenquellen:

- Wenn SQL-Authentifizierung verwenden zu können, sollten Identitätswechsel Dienstkontos.
- Wenn Windows-Authentifizierung verwenden zu können, legen Sie die Windows Benutzerkennwort. Für SQL Server wird die Windows-Authentifizierung mit einem bestimmten Identitätswechselkonto nur für in-Memory-Datenmodelle unterstützt.

Cloud-Datenquellen:

- Wenn SQL-Authentifizierung verwenden zu können, sollten Identitätswechsel Dienstkontos.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie auf eine lokale Datenquelle verfügen, achten Sie darauf, das [lokale Gateway](analysis-services-gateway.md)zu installieren. Weitere Informationen zum Verwalten des Servers unter SSDT oder SSMS finden Sie unter [Verwalten der Server](analysis-services-manage.md).
