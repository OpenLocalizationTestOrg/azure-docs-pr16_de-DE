<properties
    pageTitle="Azure Stapel Schlüssel Tresor Einführung | Microsoft Azure"
    description="Erfahren Sie, wie Azure Stapel Schlüssel Tresor Schlüssel und Kennwörter verwaltet werden"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Einführung in die wichtigsten Tresor in Azure Stapel #

## <a name="before-you-start"></a>Bevor Sie beginnen

In diesem Artikel wird Folgendes vorausgesetzt:

- Der Reader hat Zugriff auf ein Abonnement, das Azure-Taste Tresor aktiviert hat.
- Der PowerShell Azure SDK ist konfiguriert und.
- Für die Veröffentlichung TP2 können alle Mandanten zugänglichen-Operationen nur von PowerShell ausgeführt werden.

## <a name="key-vault-basics"></a>Taste Tresor-Grundlagen

Taste Tresor in Azure Stapel hilft cryptographic Tasten schützen und Kennwörter, die cloud-Anwendungen und Dienste verwenden, die. Mithilfe von Key Tresor können Sie Schlüssel und Kennwörter (z. B. Authentifizierung, Speicher Konto Tasten, Schlüssel für die Verschlüsselung, PFX-Dateien, und Kennwörter) verschlüsseln.

Taste Tresor den Vorgang Key-Verwaltung optimiert und ermöglicht Ihnen die Kontrolle über Tasten, die Zugriff auf und verschlüsseln die Daten. Entwickler können Schlüssel für die Entwicklung und Tests in Minuten erstellen, und klicken Sie dann in der Herstellung Tasten nahtlos migrieren. Sicherheits-Administratoren können erteilen (über die Berechtigung zum Schlüssel, bei Bedarf und widerrufen).

Jeder mit einem Stapel Azure-Abonnement kann erstellen und Verwenden von Key Depots. Obwohl Schlüssel Tresor Entwickler und Sicherheitsadministratoren Vorteile, kann von einem Administrator, der andere Azure Stapel Dienste für eine Organisation verwaltet es implementiert und verwaltet werden. Beispielsweise dieser Administrator kann melden Sie sich mit einem Stapel Azure-Abonnement Erstellen einer Tresor für die Organisation in der Schlüssel speichern, und klicken Sie dann für diese Betriebsaufgaben verantwortlich sein:

- Erstellen Sie oder importieren Sie einen Schlüssel oder geheim
- Widerrufen Sie oder löschen Sie einer Taste oder geheim
- Autorisieren von Benutzern oder Clientanwendungen auf die wichtigsten Tresor zugreifen, sodass sie zu verwalten oder dessen Schlüssel und Kennwörter verwenden können
- Konfigurieren der Verwendung von Key (z. B. signieren oder verschlüsseln)

Dieser Administrator dann Entwickler mit URIs zu ihrer Anwendung aufzurufen, enthalten, und bieten, die Sicherheit zuständiger Administrator Protokollierungsinformationen, die wichtigsten Verwendung.

Entwickler können auch die Tasten direkt verwalten mithilfe von APIs. Weitere Informationen finden Sie unter Key Tresor developer's Guide.

## <a name="scenarios"></a>Szenarien

In der folgenden Tabelle sind einige der Stelle, an der Schlüssel Tresor helfen den Bedürfnissen der Entwickler und Sicherheitsadministratoren Szenarios beschrieben:


### <a name="developer-for-an-azure-stack-application"></a>Entwicklertools für eine Anwendung Azure Stapel

**Problem**: Ich möchte Anwendung für Azure Stapel zu schreiben, der Schlüssel zum Signieren und Verschlüsseln verwendet, aber ich möchte diese außerhalb meiner Anwendung sein, damit die Lösung für die Anwendung geeignet ist, die geografischen verteilt ist.

**Anweisung**: Tasten in einem Tresor gespeichert sind und von URI bei Bedarf aufgerufen.


### <a name="developer-for-software-as-a-service-saas"></a>Entwicklertools für Software als Service (SaaS)

**Problem:** Die Zuständigkeit oder potenzielle Haftung werden nicht für meine Kunden Mandanten Tasten und Kennwörter.

**Anweisung:** Kunden können Sie ihre eigenen Schlüssel in Azure Stapel importieren und verwalten können. Ich möchte die Kunden besitzen und ihre Schlüssel verwalten, so dass ich konzentrieren können, klicken Sie auf Aktionen, was am besten, muss die stellt das Herzstück Software Features zur Verfügung.


### <a name="chief-security-officer-cso"></a>Leiter Sicherheitsbeauftragter (CSO)

**Problem:** Ich möchte, um sicherzustellen, dass meine Organisation Kontrolle über die wichtigsten Lebenszyklus ist und der wichtigsten Verwendung zu überwachen.

**Anweisung** Taste Tresor dient dazu, dass Microsoft nicht sehen oder der Schlüssel extrahieren.  Wenn eine Anwendung cryptographic Vorgänge mithilfe von Tasten Kunden ausführen muss, geschieht Tresor Schlüssel für die Anwendung. Die Anwendung wird nicht der Kunden Tasten angezeigt.  Obwohl wir mehrere Azure Stapel Dienste und Ressourcen verwenden, möchten die Tasten von einem einzigen Ort in Azure Stapel verwalten werden. Der Tresor stellt eine Oberfläche, unabhängig davon, wie viele Depots stehen Ihnen in Azure Stapel, welche Bereiche sie Support und welche Anwendung sie verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte mit Key Tresor](azure-stack-kv-getting-started.md)
