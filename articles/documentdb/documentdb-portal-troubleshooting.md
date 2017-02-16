<properties
    pageTitle="Behandeln von Problemen mit DocumentDB Portal | Microsoft Azure"
    description="Informieren Sie sich zur Lösung von Problemen im DocumentDB Azure-Portal aus." 
    services="documentdb"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mimig"/>

# <a name="azure-documentdb-portal-troubleshooting-tips"></a>Tipps zur Problembehandlung Azure DocumentDB-portal

In diesem Artikel werden die Azure-Portal DocumentDB Behebung. 

## <a name="resources-are-missing"></a>Ressourcen fehlen

**Problem**: Datenbanken oder Websitesammlungen fehlen aus Ihrem Portal Blades.

**Lösung**: tieferzustufen Anwendungsverwendung unter der maximalen Durchsatz Kontingent für die Websitesammlung ausgeführt werden. 

**Erläuterung**: das Portal ist eine Anwendung wie jedes andere, Anrufe zu DocumentDB Datenbank und Websitesammlung. Wenn Ihre Anfragen aktuell aufgrund Anrufe in einer separaten Anwendung gedrosselt werden möglicherweise im Portal auch gedrosselt werden bewirken, dass Ressourcen nicht im Portal angezeigt werden. Um das Problem zu beheben, die Ursache für die Verwendung der hohen Durchsatz Adresse, und aktualisieren Sie dann auf das Portal Blade. Informationen zur Verwendung von messen und unteren Durchsatz Verwendung finden Sie im Abschnitt [Durchsatz](documentdb-performance-tips.md#throughput) des Artikels [Performance Tipps](documentdb-performance-tips.md) .
 
## <a name="pages-or-blades-wont-load"></a>Seiten oder Blades werden nicht geladen

**Problem**: Seiten und Blades im Portal nicht angezeigt.

**Lösung**: tieferzustufen Anwendungsverwendung unter der maximalen Durchsatz Kontingent für die Websitesammlung ausgeführt werden. 

**Erläuterung**: das Portal ist eine Anwendung wie jedes andere, Anrufe zu DocumentDB Datenbank und Websitesammlung. Wenn Ihre Anfragen aktuell aufgrund Anrufe in einer separaten Anwendung gedrosselt werden möglicherweise im Portal auch gedrosselt werden bewirken, dass Ressourcen nicht im Portal angezeigt werden. Um das Problem zu beheben, die Ursache für die Verwendung der hohen Durchsatz Adresse, und aktualisieren Sie dann auf das Portal Blade. Informationen zur Verwendung von messen und unteren Durchsatz Verwendung finden Sie im Abschnitt [Durchsatz](documentdb-performance-tips.md#throughput) des Artikels [Performance Tipps](documentdb-performance-tips.md) .

## <a name="add-collection-button-is-disabled"></a>Hinzufügen von Schaltfläche Websitesammlung deaktiviert ist

**Problem**: in der Datenbank Blade, die **Sammlung hinzufügen** Schaltfläche deaktiviert ist.

**Erläuterung**: Wenn Ihr Abonnement Azure Vorteile Gutschriften, zugeordnet ist, wie kostenlose Gutschriften aus ein MSDN-Abonnement angeboten und alle Ihre Gutschriften für den Monat verwendet haben, können Sie nicht mehr alle zusätzlichen Websitesammlungen in DocumentDB erstellt.

**Lösung**: die Ausgaben Beschränkung aus Ihrem Konto entfernen.

1. Im Portal Azure in der Jumpbar **Abonnements**klicken Sie auf, klicken Sie auf das Abonnement der Datenbank DocumentDB zugeordnet, und klicken Sie dann in das **Abonnement** Blade auf **Verwalten**. 
    ![Definiert die DocumentDB bietet mehrere, auch (niedrige) Konsistenz-Modelle zur Auswahl](./media/documentdb-portal-troubleshooting/documentdb-change-billing.png)

2. In dem neuen Browserfenster sehen Sie sich, dass Sie keine Gutschriften verbleibende haben. Klicken Sie auf die Schaltfläche **Ausgaben Grenzwert entfernen** , um die Ausgaben für den aktuellen Abrechnungszeitraum oder endlos entfernen. Beenden Sie den Assistenten zum Hinzufügen oder Ihre Kreditkarteninformationen zu bestätigen. 
    ![Definiert die DocumentDB bietet mehrere, auch (niedrige) Konsistenz-Modelle zur Auswahl](./media/documentdb-portal-troubleshooting/documentdb-remove-spending-limit.png)

 
## <a name="query-explorer-completes-with-errors"></a>Abfrage-Explorer abgeschlossen mit Fehlern ist.

Finden Sie unter [Behandeln von Problemen mit der Abfrage-Explorer](documentdb-query-collections-query-explorer.md#troubleshoot).

## <a name="no-data-available-in-monitoring-tiles"></a>Keine Daten in Kacheln für die Überwachung verfügbar.

Finden Sie unter [Behandeln von Überwachung Kacheln](documentdb-monitor-accounts.md#troubleshooting).

## <a name="no-documents-returned-in-document-explorer"></a>Keine Dokumente zurückgegeben, die im Dokument-Explorer

Finden Sie unter [Problembehandlung bei Document Explorer](documentdb-view-json-document-explorer.md#troubleshoot).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weiterhin im Portal Probleme haben, e-Mail- [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com) für Unterstützung oder Datei anfordern ein Support im Portal auf **Durchsuchen**, **Hilfe + Support**, und klicken Sie dann auf **Supportanfrage erstellen**.
