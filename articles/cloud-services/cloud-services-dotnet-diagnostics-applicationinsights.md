<properties
   pageTitle="Behandeln von Problemen mit der Cloud-Diensten mit Anwendung Einsichten | Microsoft Azure"
   description="Erfahren Sie, wie Cloud Service Problemen mit der Anwendung Einsichten zum Verarbeiten von Daten aus Azure-Diagnose."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Behandeln von Problemen mit der Cloud-Diensten mit Anwendung Einsichten

Mit [Azure SDK 2,8](https://azure.microsoft.com/downloads/) und Azure-Diagnose Erweiterung können 1,5 Sie jetzt die Diagnose Azure-Daten für Ihre Cloud-Dienst direkt an Anwendung Einsichten senden. Die verschiedenen Typen von Protokollen gesammelten von Azure-Diagnose Anwendungsprotokolle, Windows-Ereignisprotokollen einschließlich, ETW protokolliert und Leistungsindikatoren können jetzt an Anwendung Einsichten gesendet und im Portal Anwendung Einsichten Benutzeroberfläche visualisiert werden. Wenn zusammen mit der Anwendung Einsichten SDK verwendet, können Sie jetzt Einblicke in Metrik und Protokolle in Kürze aus Ihrer Anwendung als auch das System und Infrastruktur Ebenen Daten aus Azure-Diagnose stammen erhalten.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Konfigurieren von Azure-Diagnose zum Senden von Daten an die Anwendung Einsichten

Wie folgt vor, um den setup-Projekt Ihrer Cloud-Dienst, um Daten Azure-Diagnose an Anwendung Einsichten zu senden.

1) Klicken Sie in Visual Studio Solution Explorer mit der rechten Maustaste auf eine Rolle, und wählen Sie **Eigenschaften** , um die Rolle-Designer zu öffnen

![Eigenschaften der Lösung Explorer Rolle][1]

2) Rolle-Designer, klicken Sie im Diagnoseabschnitt aktivieren Sie das Kontrollkästchen zum **Senden von Diagnosedaten Anwendung Einsichten**

![Rolle Designer Diagnosedaten an Anwendung Einsichten senden][2]

3) Wählen Sie im Dialogfeld, das eingeblendet wird, die Anwendung Einsichten Ressource, die Sie Azure Diagnosedaten senden möchten. Im Dialogfeld können Sie wählen Sie eine vorhandene Anwendung Einsichten Ressource aus Ihrem Abonnement oder Key Instrumentation für eine Anwendung Einsichten Ressource manuell angeben. Wenn Sie eine vorhandene Anwendung Einsichten Ressource besitzen können Sie auf, indem Sie auf den Link **Erstellen einer neuen Ressource** , der werden klassischen Azure-Portal ein Browserfenster geöffnet, in dem Sie eine Ressource auf Einsichten erstellen können, erstellen. Weitere Informationen zum Erstellen einer Anwendung Einsichten Ressource finden Sie unter [Erstellen einer neuen Anwendung Einsichten Ressource](../application-insights/app-insights-create-new-resource.md)

![Wählen Sie die Anwendung Einsichten Ressource][3]

4) Nachdem Sie die Anwendung Einsichten Ressource hinzugefügt haben, wird der Instrumentation Schlüssel für die Ressource als eine Einstellung der Dienstkonfiguration mit dem Namen **APPINSIGHTS_INSTRUMENTATIONKEY**gespeichert. Sie können diese Einstellung Konfiguration für jede Dienstkonfiguration oder -Umgebung, indem Sie eine andere Konfiguration auswählen, in der Dropdown-Dienst Konfiguration ändern ab und die Instrumentation einen neuen Product Key für diese Konfiguration angeben.

![Wählen Sie aus Dienstkonfiguration][4]

Die Einstellung für die Konfiguration der **APPINSIGHTS_INSTRUMENTATIONKEY** wird von Visual Studio verwendet, so konfigurieren Sie die Diagnose Erweiterung mit der entsprechenden Anwendung Einsichten Ressourceninformationen während der Veröffentlichung. Die Einstellung Konfiguration ist auf einfache Weise verschiedene Instrumentation Schlüssel für unterschiedliche Service Konfigurationen definieren. Visual Studio wird diese Einstellung übersetzen und beim Veröffentlichen in der Diagnose Erweiterung öffentlichen Konfiguration einzufügen. Um die Vorgehensweise zum Konfigurieren der Diagnose Erweiterung mit PowerShell zu vereinfachen, enthält das Paket Ausgabe von Visual Studio auch die öffentliche Konfiguration XML mit der entsprechenden Anwendung Einsichten Ausstattung Key enthalten. Die öffentliche Config-Dateien werden in den Ordner Extensions erstellt und Muster PaaSDiagnostics folgen. <RoleName>. PubConfig.xml. Alle Basis PowerShell-Bereitstellungen können dieses Muster jede Konfiguration zu einer Rolle zuordnen.

5) Aktivieren der **Diagnosedaten senden an Anwendung Einsichten** konfiguriert automatisch Azure Diagnose zum Senden alle Datenquellen und Ebene Fehlerprotokolle, die vom Agent Azure Diagnose zu Anwendung Einsichten gesammelt werden. Wenn Sie weitere zu konfigurieren, welche Daten an Anwendung Einsichten gesendet werden, müssen Sie die Datei *diagnostics.wadcfgx* für jede Rolle manuell bearbeiten möchten. Finden Sie unter [Konfigurieren von Microsoft Azure-Diagnose zum Senden von Daten in der Anwendung Einsichten](../azure-diagnostics-configure-applicationinsights.md) erfahren Sie mehr über die Konfiguration manuell zu aktualisieren.

Nach dem Senden von Azure Diagnosedaten an Anwendung Einblicken, dass Sie es in Azure bereitstellen können, wie gewohnt Cloud-Dienst konfiguriert ist ist und stellen die Erweiterung Azure Diagnose aktiviert. Finden Sie unter [Veröffentlichen einer mit Visual Studio-Cloud-Dienst](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Anzeigen des Inhalts von Azure Diagnose Einsichten Anwendung
In der Anwendung Einsichten Ressource für Ihre Cloud-Dienst konfiguriert wird der Azure diagnostic werden angezeigt.

Im folgenden werden wie die verschiedenen Azure Diagnose Typen Karte Anwendung Einsichten Konzepte Anmeldung bei:  

-  Datenquellen werden als benutzerdefinierte Kriterien in Anwendung Einsichten angezeigt.
-  Windows-Ereignisprotokollen werden als Spuren und benutzerdefinierte Ereignisse in Anwendung Einsichten angezeigt.
-  Sowie zusätzliche Diagnose Infrastruktur Protokolle Anwendung sich, ETW Protokolle werden als Spuren in Anwendung Einsichten angezeigt.

Azure-Diagnosedaten in der Anwendung Einsichten anzeigen:

- Verwenden Sie [Kennzahlen Explorer](../application-insights/app-insights-metrics-explorer.md) alle benutzerdefinierten Leistungsindikatoren oder die Anzahl der verschiedenen Arten von Windows-Ereignisprotokollereignisse visualisiert werden sollen.

![Benutzerdefinierte Kriterien in Kennzahlen Explorer][5]

- Verwenden Sie die [Suche](../application-insights/app-insights-diagnostic-search.md) So suchen Sie über die verschiedenen Spur Protokolle von Azure-Diagnose gesendet. Für Beispiel, wenn Sie in einer Rolle Ausnahmefehler hatten die verursacht die Rolle abstürzen und Papierkorb diese Informationen im Kanal *Anwendung* des *Windows-Ereignisprotokolls*anzeigen möchten. Die Suchfunktion können Sie sehen Sie sich die Windows-Ereignisprotokolls zurück, und erhalten die vollständige Stapel Spur für die Ausnahme, aktivieren Sie die Ursache des Problems zu finden.

![Suchen auf][6]

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen der Anwendung Einsichten SDK in der Cloud-Service](../application-insights/app-insights-cloudservices.md) , Daten zu Anfragen, Ausnahmen, Abhängigkeiten und alle benutzerdefinierten werden aus Ihrer Anwendung zu senden. In Kombination mit der Azure-Diagnose Daten, die Sie eine vollständige Übersicht über Ihre Anwendung und System alle in der gleichen Anwendung Einblicke Ressource zugreifen können.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
