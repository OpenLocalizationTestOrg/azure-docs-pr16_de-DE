<properties
    pageTitle="Was ist die Taste Tresor Azure? | Microsoft Azure"
    description="Azure-Taste Tresor hilft cryptographic Tasten schützen und Kennwörter von Cloudanwendungen und Dienste verwendet. Mithilfe von Azure-Taste Tresor können Kunden Schlüssel und Kennwörter (z. B. Authentifizierung, Speicher Konto Tasten, Daten Verschlüsselung Tasten,. verschlüsseln. PFX-Dateien und Kennwörter) mithilfe von Tasten, die durch Hardware Security Module (HSMs) geschützt werden."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Was ist die Taste Tresor Azure?

Azure-Taste Tresor ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter die [Taste Tresor Preise Seite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung

Azure-Taste Tresor hilft cryptographic Tasten schützen und Kennwörter von Cloudanwendungen und Dienste verwendet. Mithilfe von Key Tresor können Sie Schlüssel und Kennwörter (z. B. Authentifizierung, Speicher Konto Tasten, Daten Verschlüsselung Tasten,. verschlüsseln. PFX-Dateien und Kennwörter) mithilfe von Tasten, die durch Hardware Security Module (HSMs) geschützt werden. Zusätzliche Sicherheit können Sie importieren oder Tasten in HSMs generieren. Wenn Sie diese Schritte müssen, Microsoft Prozesse die Tasten in FIPS 140-2 der Ebene 2 überprüften HSMs (Hardware und Firmware) auswählen.  

Taste Tresor den Vorgang Key-Verwaltung optimiert und ermöglicht Ihnen die Kontrolle über Tasten, die Zugriff auf und verschlüsseln die Daten. Entwickler können Schlüssel für die Entwicklung und Tests in Minuten erstellen, und klicken Sie dann in der Herstellung Tasten nahtlos migrieren. Sicherheits-Administratoren können erteilen (über die Berechtigung zum Schlüssel, bei Bedarf und widerrufen).

Verwenden Sie in der folgenden Tabelle können Sie sich verstehen, wie Schlüssel Tresor helfen können, um den Bedürfnissen der Entwickler und Sicherheitsadministratoren.





| Rolle        | Problem-Anweisung           | Durch Azure Key Tresor gelöst  |
| ------------- |-------------|-----|
| Entwicklertools für eine Azure-Anwendung      | "Ich möchte Anwendung für Azure zu schreiben, der Schlüssel zum Signieren und Verschlüsseln verwendet, aber ich möchte diese Taste außerhalb meiner Anwendung sein, damit die Lösung für die Anwendung geeignet ist, die geografischen verteilt ist. <br/><br/>Ich möchte auch diese Schlüssel und Kennwörter zu geschützt werden, ohne den Code selbst schreiben. Ich möchte auch diese Schlüssel und Kennwörter einfach für mich Meine Applications für optimale Leistung verwendet werden." | √ Tasten in einem Tresor gespeichert und durch URI bei Bedarf aufgerufen.<br/><br/> √ Tasten sicher durch Azure mithilfe von Algorithmen Industriestandard, Schlüssel mit einer Länge und Hardware Security Module (HSMs).<br/><br/> √ Tasten werden in HSMs verarbeitet, die sich in der gleichen Azure Rechenzentren als die Programme befinden. Verbesserte Zuverlässigkeit und reduzierte Wartezeit als, wenn die Tasten in einem anderen Speicherort, beispielsweise lokale vorhanden dadurch.|
| Entwicklertools für Software als Service (SaaS)      |"Nicht die Zuständigkeit oder potenzielle Haftung für meine Kunden Mandanten Tasten und Kennwörter werden. <br/><br/>Ich möchte die Kunden besitzen und ihre Schlüssel verwalten, so dass ich konzentrieren können, klicken Sie auf Aktionen, was am besten, muss die stellt das Herzstück Software Features zur Verfügung." | √ Kunden können Sie ihre eigenen Schlüssel in Azure importieren und verwalten können. Wenn eine Anwendung SaaS cryptographic Vorgänge mithilfe von Tasten ihrer Kunden ausführen muss, unterstützt Schlüssel Tresor dieser Vorgänge für die Anwendung. Die Anwendung wird nicht der Kunden Tasten angezeigt.|
| Leiter Sicherheitsbeauftragter (CSO) | "Ich möchte wissen, dass unsere Applikationen FIPS 140-2 Ebene 2 HSMs für eine sichere Key-Verwaltung einhalten. <br/><br/>Ich möchte, um sicherzustellen, dass meine Organisation Kontrolle über die wichtigsten Lebenszyklus ist und der wichtigsten Verwendung zu überwachen. <br/><br/>Und zwar wir mehrere Azure Dienste und Ressourcen verwenden, ich möchte die Tasten von einem einzigen Ort in Azure verwalten."     |√-HSMs FIPS 140-2 Ebene 2 überprüft.<br/><br/>√ Schlüssel Tresor dient dazu, dass Microsoft nicht sehen oder der Schlüssel extrahieren.<br/><br/>√ in der Nähe der wichtigsten Verwendung in Echtzeit Protokollierung.<br/><br/>√ Der Tresor stellt eine Oberfläche, unabhängig davon, wie viele Depots stehen Ihnen in Azure, welche Bereiche sie Support und welche Anwendung sie verwenden. |


Jeder mit einem Azure-Abonnement kann erstellen und Verwenden von Key Depots. Zwar Schlüssel Tresor Entwickler und Sicherheitsadministratoren Vorteile, könnten Sie implementiert und eines Unternehmens-Administrator, die andere Azure Dienste für eine Organisation verwaltet verwaltet werden. Beispielsweise dieser Administrator würde mit einem Azure-Abonnement melden Erstellen einer Tresor für die Organisation in der Schlüssel speichern, und klicken Sie dann verantwortlich Betriebsaufgaben, wie:

+ Erstellen Sie oder importieren Sie einen Schlüssel oder geheim
+ Widerrufen Sie oder löschen Sie einer Taste oder geheim
+ Autorisieren von Benutzern oder Clientanwendungen auf die wichtigsten Tresor zugreifen, sodass sie zu verwalten oder dessen Schlüssel und Kennwörter verwenden können
+ Konfigurieren der Verwendung von Key (z. B. signieren oder verschlüsseln)
+ Verwendung der Monitor Schlüssel

Dieser Administrator dann bieten Entwickler URIs zu ihrer Anwendung aufzurufen, und Key Verwendung protokollierten Informationen ihrem Sicherheitsadministrator bieten. 

   ![Übersicht über Azure Key Tresor][1]

Entwickler können auch die Tasten direkt verwalten mithilfe von APIs. Weitere Informationen finden Sie unter [Key Tresor developer's Guide](key-vault-developers-guide.md).

## <a name="next-steps"></a>Nächste Schritte

Ein beim Abrufen Schritte für einen Administrator finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](key-vault-get-started.md).

Weitere Informationen zur Verwendung des für Schlüssel Tresor Protokollierung finden Sie unter [Azure Schlüssel Tresor Protokollierung](key-vault-logging.md).

Weitere Informationen zur Verwendung von Tasten und Kennwörter mit Azure-Taste Tresor finden Sie unter [Informationen zum Schlüssel, Kennwörter, und Zertifikate](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
