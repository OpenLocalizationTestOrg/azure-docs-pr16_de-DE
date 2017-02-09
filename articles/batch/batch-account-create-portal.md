<properties
    pageTitle="Erstellen Sie ein Konto Azure Stapel | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines Kontos Azure Stapel im Azure-Portal zum Ausführen von umfangreichen parallele Auslastung in der cloud"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Erstellen Sie ein Stapel Azure-Konto über das Azure-portal

> [AZURE.SELECTOR]
- [Azure-portal](batch-account-create-portal.md)
- [Stapel Management .NET](batch-management-dotnet.md)

Informationen zum Erstellen eines Kontos Azure Stapel im [Portal Azure][azure_portal], und wo finden Sie wichtige Kontoeigenschaften wie Zugriffstasten und URLs zu berücksichtigen. Außerdem besprochen Stapel Preise und Verknüpfen einer Azure-Speicher-Konto bei Ihrem Konto Stapel, damit Sie die [Anwendungspakete](batch-application-packages.md) und [beibehalten der Projekt- und Aufgabenzeilen Ausgabe](batch-task-output.md)verwenden können.

## <a name="create-a-batch-account"></a>Erstellen Sie ein Konto Stapel

1. Melden Sie sich bei der [Azure-Portal][azure_portal].

2. Klicken Sie auf **neue** > **berechnen** > **Stapel Dienst**.

    ![Stapel in dem Marketplace][marketplace_portal]

3. Das Blade **Neukunde Stapel** wird angezeigt. Finden Sie unter Elemente *eines* bis *e* unter eine Beschreibung der einzelnen Blade Elemente.

    ![Erstellen Sie ein Konto Stapel][account_portal]

    ein. **Kontoname**: einen eindeutigen Namen für Ihr Konto Stapel. Dieser Name muss innerhalb der Azure Region eindeutig sein, die das Konto erstellt wurde (siehe unten *Position* ). Es kann nur Kleinbuchstaben, Zahlen, enthalten und 3-24 Zeichen lang sein.

    b. **Abonnements**: ein Abonnement in der das Konto Stapel erstellt. Wenn Sie nur ein Abonnement besitzen, ist es standardmäßig aktiviert.

    c. **Ressourcengruppe**: eine vorhandene Ressource für Ihr neues Konto für den Stapel gruppieren oder optional einen neuen erstellen.

    d. **Standort**: Azure ein Bereich, in dem das Konto Stapel zu erstellen. Nur die Bereiche, indem Sie Ihr Abonnement und Ressourcengruppe unterstützt werden als Optionen angezeigt.

    e. **Speicher-Konto** (optional): ein **allgemeiner** Speicher-Konto zugeordnet werden soll (Link) bei Ihrem neuen Stapel-Konto. Weitere Informationen hierzu finden Sie unter [verknüpfte Azure-Speicher-Konto](#linked-azure-storage-account) unter.

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.

  Im Portal gibt an, dass es **Bereitstellen von** dem Konto ist, und nach Abschluss des Vorgangs eine Benachrichtigung **Bereitstellungen erfolgreich verlaufen ist** *Benachrichtigungen wird*.

## <a name="view-batch-account-properties"></a>Anzeigen von Kontoeigenschaften Stapel

Nachdem das Konto erstellt wurde, können Sie das **Blatt Konto Blade** Zugriff auf ihre Einstellungen und Eigenschaften öffnen. Sie können alle Konten-Einstellungen und Eigenschaften über das Menü links des Blades Stapel Konto zugreifen.

![Stapel Konto vorher in Azure-portal][account_blade]

* **Stapel Konto URL**: Anwendungen, die Sie, in der [Stapel Entwicklung APIs erstellen](batch-technical-overview.md#batch-development-apis) Konto URL zum Verwalten von Ressourcen und Ausführen von Aufträgen in das Konto benötigen. URL-Konto Stapel weist das folgende Format:

    `https://<account_name>.<region>.batch.azure.com`

![Stapel Konto URL-Portal][account_url]

* **Zugriffstasten**: Ihrer Anwendung auch eine Zugriffstaste benötigen, bei der Arbeit mit Ressourcen in Ihr Konto Stapel. Geben Sie zum Anzeigen oder neu generieren Ihres Kontos Stapel Zugriffstasten, `keys` im linken Menü **Suchfeld** auf das Blatt Konto Blade, wählen Sie dann **Tasten**.

    ![Stapel Konto Tasten Azure-Portal][account_keys]

## <a name="pricing"></a>Preise

Stapel-Konten werden nur in einer "kostenlosen Ebene" Was bedeutet, dass Sie belastet werden nicht für das Blatt Konto selbst angeboten. Sie unterliegen für die zugrunde liegenden Azure berechnen Ressourcen, die Ihre Stapel Lösungen zu nutzen, und für die Ressourcen, die von anderen Diensten verbraucht, wenn Ihre Auslastung ausführen. Angenommen, Sie unterliegen für die berechnen-Knoten in der Pools und für die Daten Sie speichern Azure-Speicher als ein- oder Ausgaben für Ihre Vorgänge. Wenn Sie das Feature [Anwendungspakete](batch-application-packages.md) des Blatts verwenden, sind Sie auf ähnliche Weise für den Azure-Speicher Ressourcen zum Speichern Ihrer Anwendungspaketen belastet. Finden Sie unter [Stapel Preise] [ batch_pricing] Weitere Informationen.

## <a name="linked-azure-storage-account"></a>Verknüpfte Speicher Azure-Konto

Wie bereits zuvor erwähnt, können Sie (optional) eine **Allgemeine** Speicher-Konto bei Ihrem Konto Stapel verknüpfen. Das Feature [Anwendungspakete](batch-application-packages.md) des Blatts verwendet Blob-Speicher in einer verknüpften allgemeine Speicher-Konto, wie die [Stapel Datei Konventionen .NET](batch-task-output.md) Bibliothek. Diese optionale Features hilft Ihnen bei der Bereitstellung Programme, die durch die Ihre Aufgaben Stapel ausgeführt und Beibehalten von Daten, die sie verursachen.

Stapelverarbeitung aktuell unterstützt *nur* **Allgemeine** Speicher Kontotyp wie in Schritt 5, [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account), in [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben. Wenn Sie ein Konto Azure-Speicher bei Ihrem Konto Stapel verknüpfen, werden Sie sicher, dass Link *nur* eine **Allgemeine** Speicher-Konto.

![Erstellen eines 'Allgemeine Zwecke' Speicher-Kontos][storage_account]

Es empfiehlt sich, dass Sie ein Konto Speicherplatz für den exklusiven Zugriff von Ihrem Konto Stapel erstellen.

>[AZURE.WARNING] Achten Sie beim Neuerstellen Zugriffstasten eines verknüpften Speicher-Kontos. Generieren Sie nur eine Speicher kontoschlüssel neu zu, und klicken Sie auf das verknüpfte Speicher Konto Blade **Synchronisieren Tasten** auf. Warten Sie fünf Minuten an, damit die Tasten verteilen auf Knoten berechnen Pools, und klicken Sie dann erneut generieren und bei Bedarf den anderen Schlüssel zu synchronisieren. Wenn Sie beide Schlüssel gleichzeitig neu generieren, Ihre Datenverarbeitungsknoten entweder Schlüssel für die Synchronisierung nicht möglich, und sie verlieren Zugriff auf das Konto Speicher.

  ![Erneutes Generieren Speicher Konto Tasten][4]

## <a name="batch-service-quotas-and-limits"></a>Stapel Dienstkontingente und Grenzwerte

Bitte beachten Sie, dass wie bei Ihrem Abonnement Azure und andere Dienste Azure, bestimmte [Kontingente und Grenzwerte](batch-quota-limit.md) auf Stapel Konten angewendet wird. Aktuelle Kontingente für ein Konto Stapel werden im Portal in das Konto **Eigenschaften**angezeigt.

![Stapel Konto Kontingente Azure-Portal][quotas]

Beachten Sie diese Kontingente sind Sie entwerfen und dieselbe Skalierung von Ihrem Stapel Auslastung. Beispielsweise wenn Ihre Ressourcenpool die Ziel-Anzahl von Computeknoten Eingang nicht zur Verfügung, die Sie angegeben haben, Sie möglicherweise die Core Kontingent für Ihr Konto Stapel erreicht haben.

Beachten Sie auch, dass Sie nicht mit einem einzigen Stapel-Konto für Ihr Abonnement Azure beschränkt sind. Sie können mehrere Stapel Auslastung in einem Stapel Konto ausführen, oder Sie können Ihre Auslastung zwischen Stapel Konten im selben Abonnement, aber in unterschiedlichen Azure Regionen verteilen.

Zahlreiche diese Kontingente können einfach mit einer kostenlosen Product Supportanfrage Azure-Portal übermittelt wurde erhöht. Weitere Informationen über das Kontingent erhöht anfordern [Kontingente und Grenzwerte für den Dienst Azure Stapel](batch-quota-limit.md) zu.

## <a name="other-batch-account-management-options"></a>Andere Stapel Management Kontooptionen

Zusätzlich zur Verwendung des Azure-Portals an, können Sie auch erstellen und Verwalten von Stapel-Konten mit den folgenden:

* [Stapel PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Stapel Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte

* Finden Sie unter den [Stapel Azure-Features (Übersicht)](batch-api-basics.md) erfahren Sie mehr über den Stapel Dienst Konzepte und Funktionen. Im Artikel erläutert die primären Stapel Ressourcen wie Pools, Datenverarbeitungsknoten, Projekte und Vorgänge und bietet einen Überblick des Diensts Features, mit denen die Arbeitsbelastung Ausführung umfangreiche berechnen.

* Grundlagen der Entwicklung einer Stapelverarbeitung aktivierte Anwendung mithilfe der [Stapel .NET Client-Bibliothek](batch-dotnet-get-started.md). Im [Artikel Einführung](batch-dotnet-get-started.md) führt Sie durch eine arbeiten-Anwendung, die den Stapel Dienst zum Ausführen einer Arbeitsbelastung auf mehreren berechnen Knoten verwendet und umfasst mit Azure-Speicher für Arbeitsbelastung Datei Staging und abrufen.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Erneutes Generieren Speicher Konto Tasten"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
