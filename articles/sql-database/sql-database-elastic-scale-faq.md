<properties 
    pageTitle="Flexible SQL Azure-Skala häufig gestellte Fragen zu | Microsoft Azure" 
    description="Häufig gestellte Fragen zu SQL Azure-Datenbank flexible skalieren ein." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Flexible Datenbanktools häufig gestellte Fragen 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Wenn ich einen einzelne-Mandanten pro Shard und kein Sharding Schlüssel haben, wie Auffüllen ich die Sharding Taste für das Schema Info?

Das Schema Informationsobjekt dient nur Zusammenführungsszenarien teilen. Ist eine Anwendung grundsätzlich Single-Mandanten, das Zusammenführen von Teilen ist nicht erforderlich, und daher besteht keine Notwendigkeit das Schema Informationsobjekt gefüllt wird.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Bereitstellung ich habe eine Datenbank nach der und bereits ich a-Manager Shard-Karte, wie registriere ich mich in dieser neue Datenbank als eine Shard?

Finden Sie unter **[Hinzufügen eines Shard zur Anwendung mithilfe der flexible Datenbank-Client-Bibliothek](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Wie viel Kosten flexible Datenbanktools?

Mithilfe der flexible Datenbank-Client-Bibliothek keine Kosten entstehen. Kosten fällig nur für SQL Azure-Datenbanken, die Sie für mehrere Shards hinweg und der Shard Karte-Manager verwenden, sowie das Web/Worker-Rollen, die Sie für das Zusammenführen von Teilen bereitstellen.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Warum funktionieren meine Anmeldeinformationen nicht, wenn ich eine Shard von einem anderen Server hinzufügen?
Verwenden Sie keine Anmeldeinformationen in Form von "Benutzer ID=username@servername”, verwenden Sie stattdessen einfach" Benutzer-ID = Benutzername ".  Darüber hinaus werden Sie sicher, dass das Login "Username" auf die Shard Berechtigungen verfügt.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Muss ich a-Manager Shard-Karte erstellen und füllen mehrere Shards hinweg jedes Mal, wenn ich meine Programme starten?

Nein – die Erstellung der Shard Karte Managers (z. B. **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) wird einmalig.  Die Anwendung sollte den Anruf **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** beim Start der Anwendung verwenden.  Es sollte nur ein solcher Anruf pro Anwendungsdomäne aus.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Habe ich Fragen zur Nutzung von flexible Datenbanktools, wie erhalte ich diese beantwortet? 

Bitte erreichen Sie uns im [Forum Azure SQL-Datenbank](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Wenn ich eine datenbankverbindung mit einem Sharding Key erhalten haben, kann ich weiterhin Abfragen von Daten für andere Sharding Tasten auf der gleichen Shard aus.  Ist dies beabsichtigt?

Die flexible skalieren-APIs bieten Ihnen eine Verbindung mit der richtigen Datenbank für Ihren Sharding Schlüssel, jedoch bieten keine Sharding Key filtern.  Fügen Sie gegebenenfalls Ihrer Abfrage zum Einschränken des Gültigkeitsbereichs mit dem bereitgestellten Sharding Key **WHERE** -Klausel hinzu.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kann ich in meinem Shard festlegen eine andere Azure-Datenbank-Edition für jede Shard verwenden?

Ja, eine Shard ist eine einzelne Datenbank und somit konnte eine Shard eine Premium Edition sein, während eine andere eines Standard Edition sein. Darüber hinaus kann die Edition von einer Shard mehrmals während der Lebensdauer der Shard nach oben oder unten skalieren.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Eine Datenbank während eines geteilten oder Zusammenführen bedeutet die Erbringung der geteilten Zusammenführen Tool (oder löschen)? 

Nein. **Teilen von** JOIN-Operationen die Zieldatenbank muss vorhanden sein, mit dem entsprechenden Schema und bei der Shard Karte-Manager registriert sein.  Für Vorgänge **Zusammenführen** müssen Sie die Shard aus dem Shard Karte-Manager löschen und löschen Sie die Datenbank.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 