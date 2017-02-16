<properties
    pageTitle="Versionsverlauf Azure-Diagnose"
    description="Erläuterung der Änderungen in den unterschiedlichen Versionen von Azure Diagnose als gelieferten mit unterschiedlichen Versionen von Microsoft Azure SDKs."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Versionsverlauf für Microsoft Azure-Diagnose

Neu bei Azure Diagnose? Finden Sie unter [Übersicht über die Azure-Diagnose](azure-diagnostics.md).

Jede Version von Azure SDK Lieferumfang normalerweise auf eine neue Version von Azure-Diagnose. In der nachfolgenden Tabelle beschreibt die Azure SDK und Azure-Diagnose-Versionen, die die SDK-Version zugeordnet.



Azure SDK-version | Azure Diagnose version | Modell
--- | --- | ---
1.x      | 1.0 | -Plug-in
2.0 zu 2.4| 1.0 | "
2,5      | 1.2 | Erweiterung
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1,5 | "


Die neueste Version ist 1,5, die im Lieferumfang von Azure SDK 2,8. Die Version von Azure-Diagnose Erweiterung, die im Lieferumfang des SDKS wird nur für lokale Emulator Szenarien verwendet. Alle bereitgestellte Anwendung verwendet automatisch die neueste Version aus, wenn Azure ausgeführt unabhängig davon, welche Version von SDK der Anwendung mit erstellt wurde. Jedoch, es sei denn, Sie das aktuelle Azure SDK installiert haben, möglicherweise nicht alle wurden die Tools, mit denen Sie die neuen Features zu nutzen.

Verschiedene Features und Änderungen, die nachfolgend beschriebenen.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure SDK 2,8 hinzugefügt, die Möglichkeit zum Senden von Diagnosedaten zu [Anwendung Einsichten](./application-insights/app-insights-cloudservices.md) diagnostizieren Sie Probleme in Ihrer Anwendung als auch die Ebene System und Infrastruktur zu erleichtern.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 Diagnose Änderungen

Die folgenden Änderungen wurden Azure SDK 2.6 Cloud-Dienst Projekte in Visual Studio vorgenommen. (Diese Änderungen gelten auch für spätere Versionen von Azure SDK.)

- Der lokale Emulator unterstützt jetzt Diagnose. Sie können also Diagnosedaten sammeln und stellen Sie sicher, dass es sich bei Ihrer Anwendung ist die richtigen Spuren erstellen, während Sie entwickeln und Testen in Visual Studio. Die Verbindungszeichenfolge `UseDevelopmentStorage=true` Diagnose Datensammlung ermöglicht, während Sie Ihr Projekt Cloud-Dienst in Visual Studio ausführen mithilfe von Azure Speicheremulator. Alle Diagnosedaten werden im Speicher (Entwicklung Speicher) Konto erfasst.

- Die Verbindungszeichenfolge für Diagnose Speicher-Konto (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) wird erneut in der Dienstkonfigurationsdatei (.cscfg) gespeichert. In Azure SDK 2,5 wurde das Diagnose Speicher-Konto in der Datei diagnostics.wadcfgx angegeben.

Es gibt einige wichtige Unterschiede zwischen wie die Verbindungszeichenfolge in Azure SDK 2.4 und früher gearbeitet haben und Funktionsweise in Azure SDK 2.6 und höher.

- In Azure SDK 2.4 und früher, wurde die Verbindungszeichenfolge als eine Laufzeit das Plug-in Diagnose verwendet können Sie die Kontoinformationen Speicher für die Übertragung von Diagnose Protokolle zu gelangen.

- In Azure SDK 2.6 und höher ist die Verbindungszeichenfolge Diagnose so konfigurieren Sie die Diagnose Erweiterung mit den entsprechenden Speicher Kontoinformationen während der Veröffentlichung von Visual Studio verwendet. Die Verbindungszeichenfolge können Sie verschiedene Speicherkonten für unterschiedliche Service Konfigurationen definieren, die beim Veröffentlichen Visual Studio verwendet wird. Da der Diagnose-Plug-in (nach Azure SDK 2,5) nicht mehr verfügbar ist, kann nicht die .cscfg Datei allein die Erweiterung Diagnose aktivieren. Sie müssen die Erweiterung separat mithilfe von Tools wie Visual Studio oder PowerShell zu aktivieren.

- Zum Konfigurieren der Diagnose Erweiterung mit PowerShell vereinfachen, enthält die Ausgabe Paket aus Visual Studio auch die öffentliche Konfigurations-XML für die Diagnose Erweiterung für jede Rolle aus. Visual Studio verwendet die Verbindungszeichenfolge Diagnose die Kontoinformationen Speicher im öffentlichen Konfiguration präsentieren gefüllt wird. Die öffentliche Config-Dateien werden in den Ordner Extensions erstellt und Muster PaaSDiagnostics folgen. <RoleName>. PubConfig.xml. Alle Basis PowerShell-Bereitstellungen können dieses Muster jede Konfiguration zu einer Rolle zuordnen.

- Die Verbindungszeichenfolge in der Datei .cscfg wird auch von Azure-Portal zum Zugriff auf die Diagnosedaten, sodass es auf der Registerkarte **Überwachung** angezeigt werden kann. Die Verbindungszeichenfolge ist zum Konfigurieren des Diensts zum Anzeigen von ausführlicher Überwachung Daten im Portal erforderlich.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrieren von Projekten auf Azure SDK 2.6 und höher

Beim Migrieren von aus Azure SDK 2,5 auf Azure SDK 2.6 oder höher, wenn Sie eine Diagnose Speicher-Konto in der Datei .wadcfgx angegebenen hatten aus, klicken Sie dann bleibt es. Wenn Sie die Flexibilität der Verwendung von verschiedenen Speicherkonten für verschiedene Speicherkonfigurationen nutzen zu können, müssen Sie die Verbindungszeichenfolge manuell zu Ihrem Projekt hinzufügen. Wenn Sie ein Projekt aus Azure SDK 2.4 oder einer früheren Version auf Azure SDK 2.6 migrieren, bleiben die Diagnose Verbindungszeichenfolgen. Beachten Sie jedoch die Änderungen in wie Verbindungszeichenfolgen in Azure SDK 2.6 gemäß Angabe im vorherigen Abschnitt behandelt werden.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Wie bestimmt Visual Studio Diagnose-Speicher-Konto

- Wenn eine Diagnose Verbindungszeichenfolge in der Datei .cscfg angegeben wird, von Visual Studio so konfigurieren Sie die Diagnose Erweiterung beim Veröffentlichen und beim öffentlichen Konfiguration XML-Dateien beim Packen generieren verwendet.

- Wenn keine Diagnose Verbindungszeichenfolge in der Datei .cscfg angegeben ist, fällt dann Visual Studio wieder mit der in der Datei .wadcfgx angegebenen Speicher-Konto so konfigurieren Sie die Diagnose Erweiterung beim Veröffentlichen und Generieren von öffentlichen Konfiguration XML-Dateien beim Packen.

- Die Diagnose Verbindungszeichenfolge in der Datei .cscfg hat Vorrang vor dem Speicherkonto in der Datei .wadcfgx. Wenn eine Diagnose Verbindungszeichenfolge in der Datei .cscfg angegeben ist, klicken Sie dann Visual Studio verwendet werden, und des Speicherkontos .wadcfgx ignoriert.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Was bedeutet "Update Entwicklung Speicher Verbindungszeichenfolgen..." Führen Sie das Kontrollkästchen?

Das Kontrollkästchen **Aktualisieren Entwicklung Speicher Verbindungszeichenfolgen für Diagnose und Zwischenspeichern mit Microsoft Azure-Speicher Anmeldeinformationen beim Veröffentlichen in Microsoft Azure** bietet Ihnen eine bequeme Möglichkeit, jede beliebige Entwicklung Speicher Konto Verbindungszeichenfolge mit dem angegebenen während der Veröffentlichung Azure-Speicher-Konto zu aktualisieren.

Beispiel: Aktivieren Sie dieses Kontrollkästchen, und die Verbindungszeichenfolge Diagnose gibt `UseDevelopmentStorage=true`. Wenn Sie das Projekt Azure veröffentlichen, wird Visual Studio automatisch die Verbindungszeichenfolge Diagnose mit dem Speicherkonto aktualisieren, die Sie in den Veröffentlichen-Assistenten angegeben haben. Wenn jedoch eine reale Speicher als die Verbindungszeichenfolge Diagnose angegeben wurde, ist dann diesem Konto stattdessen verwendet.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnose Funktionsunterschiede zwischen Azure SDK 2.4 und früher und Azure SDK 2,5 und höher

Wenn Sie Ihr Projekt aus Azure SDK 2.4 Azure SDK 2,5 oder höher aktualisieren, sollten Sie die folgenden Diagnose Funktionsunterschiede Beachten Sie Folgendes.

- **Konfigurations-APIs sind veraltet** – programmgesteuerten Konfiguration von Diagnose steht in Azure SDK 2.4 oder einer früheren Version, jedoch wird nicht mehr unterstützt in Azure SDK 2,5 und höher. Wenn Ihre Diagnosekonfiguration derzeit in Code definiert ist, müssen Sie diese Einstellungen im Projekt in Reihenfolge für Diagnose migrierte völlig weiterzuarbeiten neu zu konfigurieren. Konfigurationsdatei Diagnose für Azure SDK 2.4 ist diagnostics.wadcfg und diagnostics.wadcfgx für Azure SDK 2,5 und höher.

- **Diagnose für Cloud-dienstanwendungen kann nur Ebene der Rolle Ebene Instanz nicht konfiguriert werden.**

- **Die Diagnosekonfiguration wird aktualisiert, jedes Mal, wenn Sie Ihre app bereitstellen** – Dies kann Unstimmigkeit Probleme verursachen, wenn Sie Ihre Diagnosekonfiguration vom Server-Explorer ändern, und klicken Sie dann Ihre app erneut.

- **In der Konfigurationsdatei Diagnose nicht im Code in Azure SDK 2,5 und höher, einem Absturz speichert konfiguriert sind** – Wenn Sie Absturz speichert Code konfiguriert haben, müssen Sie die Konfiguration von Code auf der Konfigurationsdatei manuell zu übertragen, da die Speicherabbilder während der Migration in Azure SDK 2.6 übertragen werden nicht.
