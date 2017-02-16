<properties 
   pageTitle="Preise Ebene Empfehlungen für SQL Azure-Datenbank" 
   description="Beim Ändern von Preise sind Ebenen der Azure-Portal Preise Ebene Empfehlungen vorausgesetzt, sollten Sie die Ebene, die am besten geeignet ist, die zum Ausführen einer vorhandenen Azure SQL-Datenbank die Arbeitsbelastung geeignet ist. Preisgestaltung Ebenen beschreiben die Dienstalter Ebene und Leistung von einer SQL-Datenbank." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL-Datenbank Preisgestaltung Ebene Empfehlungen

 Preisgestaltung Ebene Empfehlungen vorschlagen das Dienstalter Ebene und Leistung, das zum Ausführen einer vorhandenen SQL Azure-Datenbank Arbeitsbelastung am besten geeignet ist.

> [AZURE.NOTE] Preisgestaltung Ebene Empfehlungen sind nur für das Web und Business Datenbanken und flexible Datenbank-Pools verfügbar und nur die [Azure-Portal](https://portal.azure.com/).


Erste Preise Ebene Empfehlungen, während die folgenden Aufgaben:

- [Ändern der Ebene und Leistung Dienstalter (Preisgestaltung Ebene) einer SQL-Datenbank](sql-database-scale-up.md)
- [Upgrade Azure SQL-Datenbankserver an V12](sql-database-upgrade-server-portal.md)
- Navigieren Sie zu Ihrem Server V12. Finden Sie unter [SQL-Datenbank Preise Ebene Empfehlungen](sql-database-service-tier-advisor.md).
- [Erstellen einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>(Übersicht)

Der SQL-Datenbank-Dienst zur Analyse aktuelle Leistung und Features Anforderungen Bewertung zurückliegenden Ressourceneinsatz für eine SQL­Datenbank. Darüber hinaus wird die kleinsten zulässigen Dienstebene bestimmt anhand der Größe der Datenbank, und aktiviert [Geschäftskontinuität](sql-database-business-continuity.md) Features. 

Diese Informationen werden analysiert und das Dienstalter Ebene und Leistung, der am besten geeignet ist, für die Ausführung der Datenbank des normalen arbeiten und verwalten, dass sie die aktuellen Funktionsumfang ist, wird empfohlen.

- Der Dienst untersucht der vorherigen 15 bis 30 Tage zurückliegende Daten (Ressource: Einsatz hinzu, Datenbankgröße und Datenbankaktivität) und führt einen Vergleich zwischen dem Betrag verbraucht Ressourcen und die ist-Einschränkungen aktuell verfügbaren Service Ebenen und Leistung Ebenen.
- Daten werden in Intervallen von 15 Sekunden analysiert und des Intervalls Resultset wird in der vorhandenen Dienstebene kategorisiert und Leistungsstufe, der am besten geeignet ist, für den Umgang mit diesem Resultset des Arbeitsbelastung.
- Diese 15 Sekunde Beispiele sind in der größeren 15-30 Tage Analyse dann aggregiert und empfiehlt sich das Dienstalter Ebene und Leistung, das optimal 95 % von der zurückliegenden Arbeitsbelastung behandelt werden kann.

### <a name="recommendations"></a>Empfehlungen

Basierend auf Ihrer Datenbank Verwendung, gibt es aktuell 2 Feldkategorien für Empfehlungen, die auftreten können:


| Empfehlungen | Beschreibung |
| :--- | :--- |
| Upgrade | Upgrade auf eine neue Kategorie. |
| Nicht verfügbar | Eine Datenbank erfordert eine minimale Arbeitsbelastung oder ungefähr 35 Tage Aktivität. Es ist nicht genügend Daten, um eine gültige Empfehlungen bereitzustellen. |

## <a name="getting-pricing-tier-recommendations"></a>Erste Ebene Empfehlungen Preise

Rufen Sie die Preise Ebene Empfehlungen, indem Sie eine vorhandene Web oder Business Datenbank auswählen, klicken Sie auf **Alle Einstellungen**und anschließend **Preise Ebene (Maßstab DTUs)**klicken Sie auf. (Preisgestaltung Ebene Empfehlungen stehen auch zur Verfügung, wenn Sie [Upgrade Azure SQL-Datenbankserver an V12](sql-database-upgrade-server-portal.md).)

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)aus.
2. Klicken Sie auf **Durchsuchen** > **SQL-Datenbanken**.
4. Klicken Sie in das Blade **SQL-Datenbanken** auf die Datenbank, der eine Empfehlungen für angezeigt werden sollen:

    ![Wählen Sie die Datenbank][1]

5. In der Datenbank Blade **Alle Einstellungen** und wählt dann **Preise Ebene (Maßstab DTUs)**.


7. Die **Preise Ebenen empfohlen** öffnen, wo Sie klicken Sie auf die vorgeschlagenen Ebene können, und klicken Sie dann auf die Schaltfläche **auswählen** , in die Ebene zu ändern.

    ![Melden Sie sich bei der Vorschau][4]

8. Klicken Sie optional auf **Verwendungsdetails anzeigen** , um das Blade **Preise in Empfehlungen Details** zu öffnen, in dem Sie die empfohlene Ebene für die Datenbank, die einem Featurevergleich zwischen der aktuellen und empfohlene Ebenen und ein Diagramm der zurückliegenden Ressource: Einsatz Analyse anzeigen können.

    ![Melden Sie sich bei der Vorschau][5]



## <a name="summary"></a>Zusammenfassung

Preisgestaltung Ebene Empfehlungen bieten eine automatisierte Oberfläche für die Datensammlung werden für jede SQL-Datenbank und die beste Dienst Ebene/Leistung Ebene Kombination basierend auf einer Datenbank die tatsächliche Leistung und Features sonstigen Erfordernissen empfehlen. Klicken Sie auf das Blade Einstellungen **Preise Ebene (Maßstab DTUs)** , um anzuzeigen, Preise Ebene Empfehlungen für alle Datenbanken Web und Business.



## <a name="next-steps"></a>Nächste Schritte

Je nach der Datenbank bestimmte Details ein Upgrade oder Downgrade normalerweise Durchführung unmittelbar geschieht nicht. Im Portal werden Benachrichtigungen bereitstellen, als die Datenbank Übergänge an die neue Ebene, oder Sie können den Aktualisierungsstatus überwachen, indem Sie die Ansicht [sys.dm_operation_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx) in der SQL-Datenbankserver master-Datenbank.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
