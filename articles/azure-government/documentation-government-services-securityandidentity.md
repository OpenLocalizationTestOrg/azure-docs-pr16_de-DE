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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure Government Sicherheit und Identität

##  <a name="key-vault"></a>Key Tresor
Weitere Informationen zu diesen Dienst und zur gemeinsamen Nutzung, finden Sie unter der <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure-Taste Tresor öffentliche Dokumentation.</a>

### <a name="data-considerations"></a>Aspekte der Daten
Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government für Azure-Taste Tresor:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten, die mit einer Azure-Taste Tresor Schlüssel verschlüsselt werden, enthalten möglicherweise Daten Regulated/gesteuert. | Azure-Taste Tresor Metadaten darf nicht gesteuert exportieren Daten enthalten. Diese Metadaten umfasst alle Konfigurationsdaten, die beim Erstellen und Verwalten von Ihrem Tresor Schlüssel eingegeben.  Geben Sie in die folgenden Felder nicht Regulated/gesteuert Daten: Gruppe Ressourcennamen, Schlüssel Tresor Namen, Namen des Abonnements. |

Taste Tresor steht in der Regel in Azure Government aus. Wie in der Öffentlichkeit ist ohne Erweiterung, damit die Taste Tresor nur über PowerShell und CLI verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte

Abonnieren Sie zusätzliche Informationen und Updates, die <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>
