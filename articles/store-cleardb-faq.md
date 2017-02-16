<properties
    pageTitle="Häufig gestellte Fragen zu ClearDB MySql-Datenbanken mit Azure-App-Verwaltungsdienst | Microsoft Azure"
    description="Antworten auf häufig gestellte Fragen zur Verwendung von ClearDB MySQL-Datenbanken mit Azure-App-Verwaltungsdienst."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Häufig gestellte Fragen zu ClearDB MySql-Datenbanken mit Azure-App-Verwaltungsdienst

Hier finden Sie Antworten auf häufige gestellte Fragen zur Verwendung und erwerben ClearDB MySQL-Datenbanken für Azure Web Apps.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Welche Optionen habe ich für MySQL auf Azure?

Sie haben mehrere Optionen aus:

* [ClearDB freigegeben MySQL-Datenbank](/marketplace/partners/cleardb/databases/)

* [MySQL-Premium ClearDB Cluster](/marketplace/partners/cleardb-clusters/cluster/)

* [Ausführen einer Azure-virtuellen Computers MySQL-cluster](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Einzelne Instanz von MySQL eine Azure-virtuellen Computers ausgeführt](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB ist mit einer MySQL Hostingdienst und verwaltet die MySQL-Infrastruktur für Sie. Wenn Sie eigene MySQL-Cluster oder die Datenbank auf einer Azure-virtuellen Computern ausführen, müssen Sie den MySQL-Server eingerichtet und halten Sie es mit Patches aktualisiert.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Benötige ich eine Kreditkarte für die Web app + MySQL-Vorlage, in dem Azure Marketplace?

Dies hängt von den Typ des Abonnements, das Sie verwenden. Hier sind einige häufig verwendete Abonnement Typen aus:

* [Quellenbesteuerung](/offers/ms-azr-0003p/): erfordert eine Kreditkarte, oder wenn Sie eine kostenpflichtiges MySQL-Datenbank kaufen Ihre Kreditkarte belastet wird.

* [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/): Gutschriften enthält, für die Verwendung mit Microsoft Azure services, aber nicht zulassen Kauf von Drittanbieter-Ressourcen. Erwerben von Drittanbietern Services oder ein kostenpflichtiges MySQL-Datenbank, die Sie ein Abonnement aktiviert Kreditkarte verwenden müssen. Für Web Apps können Sie eine kostenlose ClearDB MySQL-Datenbank erstellen.

* [MSDN-Abonnement](/pricing/member-offers/msdn-benefits/) , und **Wechseln Sie als Sie bezahlen MSDN Entwickler Test**: ähnlich wie kostenlose Testversion, ein MSDN-Abonnement erfordert, dass Sie eine Kreditkarte erwerben eine kostenpflichtiges MySQL-Lösung von ClearDB besitzen.

* [Enterprise Agreement (EA)](/pricing/enterprise-agreement/): EA-Kunden für ihre EA jedes Quartal für alle ihre Azure Marketplace (von Drittanbietern) Einkäufe in einer separaten, konsolidierte Rechnung fakturiert sind. Sie sind außerhalb monetäre Zusicherung für alle Einkäufe Marketplace Abrechnung. Bitte beachten Sie, dass die zu diesem Zeitpunkt Azure Store nicht verfügbar für Kunden in Aserbaidschan, Kroatien, Norwegen und Puerto Rico registriert ist. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): Sie können nur kostenlose ClearDB Datenbanken für Web Apps erstellen. Es gibt keine Beschränkung für die Anzahl der kostenlosen ClearDB MySQL-Datenbanken, die Sie erstellen können. Beachten Sie, dass kostenlose Datenbanken nicht verwendet werden, für die Herstellung Web apps, wie dieser Dienst nur für Testversion vorgesehen ist.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Warum belastet kann ich 3.50 $ für eine Web app + MySQL aus dem Azure Marketplace wurde?

Die Datenbank Standardoption ist Titan, also $3.50. Wir die Kosten während der Erstellung der Datenbank nicht anzeigen, und Sie können eine Datenbank, die, der Sie anwenden möchten nicht, versehentlich erwerben. Wir versuchen, suchen Sie eine Möglichkeit, das zu verbessern, aber bis zu diesem Zeitpunkt müssen Sie alle Ihre ausgewählten Preisgestaltung Ebenen für Web app und die Datenbank vor dem Klicken auf **Erstellen** , und beginnen Sie die Bereitstellung der Ressourcen überprüfen.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Ich bin MySQL auf eigene Azure-virtuellen Computern ausgeführt werden. Kann ich meine Azure Web app zu meiner-Datenbank herstellen?

Ja. Sie können Ihre Web app zu Ihrer Datenbank herstellen, solange Sie Ihre Azure-virtuellen Computer remote Access Web app zugeordnet wurde. Weitere Informationen finden Sie unter [Installieren von MySQL auf einem virtuellen Computer](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Länder in dem ClearDB Premium MySQL-Cluster unterstützt werden?

[ClearDB Premium MySQL-Cluster](/marketplace/partners/cleardb-clusters/cluster/) sind in allen Azure Regionen mit Ausnahme von Indien, Australien, Brasilien Süd und China weltweit zur Verfügung.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Kann ich einen neuen Cluster vor dem Erstellen einer Datenbank mit ClearDB Premium Clusterlösung erstellen?

Nein, wird das Erstellen leeren ClearDB Cluster nicht unterstützt. Azure-Portal können Sie zum Erstellen von Datenbanken in einem Cluster, die einen neuen Cluster gleichzeitig erstellen können.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Werden ich gewarnt, wenn ich versuche, eine ClearDB MySQL-Datenbank zu löschen, die verwendet wird, um eins der Meine Programme?

Nein, gibt Azure keine Warnung Wenn Sie einen Einkauf Marketplace löschen, dem die Anwendung abhängig.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Welche Bereiche kann ich ClearDB Datenbanken in erstellen?

Azure Marketplace ist nicht verfügbar für Kunden in Aserbaidschan, Kroatien, Norwegen oder Puerto Rico registriert. ClearDB ist in folgenden Regionen nicht verfügbar.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Welche Preisgestaltung Ebene sollte ich für einen Herstellung Web app und die Datenbank auswählen?

Verwenden Sie Basic oder eine höhere Preisgestaltung Stufe für Web Apps. ClearDB empfehlen wir eine Saturn oder Jupiter planen. Überprüfen Sie die Features und Einschränkungen der einzelnen Preisgestaltung Stufe für [Web Apps](/pricing/details/app-service/) und [ClearDB MySQL-Datenbanken](/marketplace/partners/cleardb/databases/) zu entscheiden, die Ihren Bedürfnissen entspricht.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Wie kann ich meine ClearDB-Datenbank von einem Plan in einen anderen aktualisieren?

Im [Portal Azure](https://portal.azure.com)-können Sie eine Hostinganbieter ClearDB Mehrbenutzerdatenbank skalieren. In diesem [Artikel](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) erfahren Sie mehr zu lesen. Zurzeit unterstützt nicht für ClearDB Premium Cluster im Portal Azure Upgrade.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Meine ClearDB Datenbank Azure-Portal nicht angezeigt?

Wenn wir ClearDB Datenbank mit Azure Ressourcenmanager oder [Neue Azure-Portal](https://portal.azure.com)zu erstellen, wird es im [Alten Azure-Portal](https://manage.windowsazure.com)nicht angezeigt. Wenn Sie dies umgehen besteht darin, die Datenbank manuell zu Web app zu verknüpfen. Auf ähnliche Weise Wenn ClearDB Datenbank erstellen die [alte Portal](https://manage.windowsazure.com) nicht werden kann die Datenbank in das [Neue Azure-Portal](https://portal.azure.com)zu sehen. Es gibt keine Umgehung für im zweiten Szenario.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Wen kontaktieren ich für Support, wenn mein Datenbank nicht verfügbar ist?

Kontakt [ClearDB Unterstützung](https://www.cleardb.com/developers/help/support) für jede Datenbank zusammenhängende Probleme. Bereiten Sie Ihre Azure Abonnementinformationen vermitteln.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Kann ich für meine ClearDB MySQL-Datenbank Clusterlösung weiterer Benutzer erstellen? 

Nein. Sie können keine zusätzlichen Benutzer erstellen, jedoch können Sie zusätzliche Datenbanken auf Ihre Datenbank-Cluster ClearDB erstellen.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kann Basic/Pro Reihe Datenbanken werden aktualisiert in-situ-ähnliche umfassend skalierfähige Pläne heute ClearDB Portal?

Ja, aktualisiert grundlegende Datenreihe, die Datenbanken werden können in-situ-(grundlegende 60 bis grundlegende 500). Pro Reihe werden können in-situ (Pro 125 bis Pro 1000) eine Ausnahme bilden jedoch Pro 60 durchgeführt. Wir unterstützen keine Pro 60-Datenbank wird derzeit aktualisiert. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Wenn ich meine Ressourcen aus einem Abonnement zu einem anderen migrieren, migriert mein ClearDB MySQL-Datenbank ebenfalls? 

Wenn Sie Ressourcenmigration über Abonnements durchführen, einige [Einschränkungen](./app-service-web/app-service-move-resources.md) gelten. Eine ClearDB MySQL-Datenbank ist ein Drittanbieter-Dienst und daher nicht migriert während der Migration Azure-Abonnement. Wenn Sie nicht die Migration von der MySQL-Datenbank vor dem Migrieren von Azure Ressourcen verwaltet werden, können Ihre ClearDB MySQL-Datenbanken deaktiviert werden. Manuell zuerst migrieren Sie einer Datenbank, und führen Sie Migration Azure-Abonnement für Ihre Web app. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Kann ich eine Datenbank ClearDB aus einem Abonnement Kreditkarte zu einem Abonnement EA übertragen?

Vorhandene ClearDB Datenbanken verwenden die Kreditkarte die vorhandenen Abonnements zugeordnet. Wenn Sie ein Abonnement EA verwenden müssen Sie Ihre Daten in eine neue Datenbank migrieren:

* Kaufen Sie eine neue ClearDB-Datenbank mit Ihrem EA-Abonnement.
* Migrieren von Daten in die neue Datenbank an.
* Aktualisieren Sie die Anwendung zum Verwenden der neuen Datenbank ein.
* Löschen Sie die alte ClearDB Datenbank ein.

Beim Erstellen einer neuen Web app mit MySQL (ClearDB) oder eine MySQL-Datenbank (ClearDB) erstellen, bestimmt das Abonnement, das Sie auswählen, wie Sie für den Dienst bezahlen werden. Mit einem Abonnement EA werden wir die Aufträge der Drittanbieter-Dienste wie ClearDB Azure-Portal nicht blockiert. EA Abonnements außerhalb monetäre Zusicherung in Rechnung gestellt werden und werden in Rechnung gestellt, Quartals- und überfällige. Der Kunde EA nutzen, müssen Sie eine Zahlungsmethode aus, beispielsweise eine Kreditkarte einrichten, dass alle Dienste von Drittanbietern Marketplace bezahlen.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Wo kann ich die Gebühren für Ressourcen ein Abonnement EA ClearDB sehen?

Bei direkten EA werden Azure Marketplace Gebühren auf Enterprise Portal angezeigt. Beachten Sie, dass alle Marketplace Einkäufe und Verbrauch außerhalb monetäre Zusicherung in Rechnung gestellt werden und in Rechnung, vierteljährlich und überfällige gestellt werden. EA Kunden direkt an die Drittanbieter-Dienstanbieter bezahlen und dazu eine Zahlungsmethode aus, beispielsweise eine Kreditkarte mit ihrem Konto EA aktivieren.

Indirekte EA Kunden ihre Azure Marketplace-Abonnements auf der Seite **Abonnements verwalten** von Enterprise Portal findet, kann jedoch Preise ausgeblendet ist. Kunden sollte deren LSP erhalten Sie Informationen über die Marketplace Gebühren.

Zugriff auf Azure Marketplace für Dienste von Drittanbietern kann von Ihrem EA Azure Registrierungs-Administratoren verwaltet werden. Sie können aktivieren oder Deaktivieren von erneut Access 3rd Party Einkauf aus dem Speicher in Konten verwalten und Abonnements unter dem Abschnitt Konten in Enterprise Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Wer kann ich für Fragen zu meiner Abrechnung für ClearDB Dienste in mein Abonnement EA kontaktieren?

Wenden Sie sich im Hinblick auf Abrechnung unter deren Registrierung EA an [Enterprise-Kundensupport](http://aka.ms/AzureEntSupport) . Das EA-Portal Supportteam wird Ihre Fragen beantworten oder das Problem beheben.

 



## <a name="more-information"></a>Weitere Informationen

[Azure Marketplace häufig gestellte Fragen](/marketplace/faq/)
