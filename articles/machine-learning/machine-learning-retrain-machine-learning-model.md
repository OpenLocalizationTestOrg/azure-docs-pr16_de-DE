<properties
    pageTitle="Neu trainieren ein Computers Learning Modell | Microsoft Azure"
    description="Informationen Sie zum neu Trainieren eines Modells und aktualisieren den Webdienst, um das Modell neu ausgebildeten Azure Computer interessante verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Neu Trainieren eines Computers Learning Modell

Als Teil der Vorgang des Operationalization Computer Learning Modelle in Azure Computer Lernen ist Ihr Modell gelernt und gespeichert. Sie verwenden sie dann zum Erstellen eines Webdiensts predicative. Der Webdienst kann dann in Websites, Dashboards und mobile-apps genutzt werden. 

Modelle, die mit maschinellen Learning erstellten sind nicht in der Regel statisch. Sobald neue Daten verfügbar sind, oder wenn der Consumer der API ihre eigenen Daten enthält muss das Modell einarbeiten. 

Umschulung kann häufig auftreten. Mit dem Feature programmgesteuerten Umschulung API können programmgesteuert neu Trainieren des Modells mithilfe der Umschulung APIs und den Webdienst mit dem Modell neu ausgebildeten aktualisieren. 

Dieses Dokument beschreibt die retraining, und es wird gezeigt, wie die APIs Umschulung verwenden.

## <a name="why-retrain-defining-the-problem"></a>Warum neu trainieren: Definieren des Problems  

Im Rahmen des Computers learning Schulungsprozess wird ein Modell gelernt mithilfe eines Satzes von Daten. Modelle, die mit maschinellen Learning erstellten sind nicht in der Regel statisch. Sobald neue Daten verfügbar sind, oder wenn der Consumer der API ihre eigenen Daten enthält muss das Modell einarbeiten.

In diesen Fällen bietet eine programmgesteuerten API bequem Sie oder Ihre APIs Consumer einen Client zu erstellen, der auf Basis einmalige oder regelmäßige, verwenden ihre eigenen Daten Modell neu trainieren können, zulassen. Sie können dann ausgewertet werden die Ergebnisse der Umschulung und aktualisieren die Webdienst-API um das Modell neu ausgebildeten verwenden.

>[AZURE.NOTE] Wenn Sie eine vorhandene Schulung experimentieren und neue Webdienst haben, möchten Sie möglicherweise Auschecken Leitungsrauschen erneute Verbindungsversuche einen vorhandenen Vorhersage Webdienst anstelle die exemplarische Vorgehensweise im folgenden Abschnitt erwähnt folgen.

## <a name="end-to-end-workflow"></a>End-to-End-Workflows 

Der Vorgang umfasst die folgenden Komponenten: A Schulung experimentieren und eine Vorhersage experimentieren als Webdienst veröffentlicht. Klicken Sie zum Aktivieren eines Modells ausgebildeten Umschulung muss die Schulung experimentieren als Webdienst mit der Ausgabe eines Modells ausgebildeten veröffentlicht werden. Dadurch API Zugriff auf das Modell für Umschulung. 

Die folgenden Schritte gelten für sowohl neu und klassischen Webdiensten:

Erstellen Sie den ursprünglichen Vorhersage Webdienst:

* Erstellen einer Schulung experimentieren
* Erstellen einer Vorhersage Web experimentieren
* Bereitstellen eines Webdiensts Vorhersage

Neu Trainieren Sie den Webdienst aus:

* Aktualisieren von Schulung experimentieren für Umschulung dürfen
* Bereitstellen des retraining Webdiensts
* Verwenden Sie den Stapel Ausführung Service-Code in das Modell neu trainieren

Der vorherigen Schritte eine exemplarische Vorgehensweise finden Sie unter [neu trainieren maschinellen Learning Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md).

Wenn Sie einen klassischen Webdienst bereitgestellt:

* Erstellen Sie einen neuen Endpunkt im Vorhersage Webdienst
* Abrufen der URL PATCH und code
* Verwenden Sie den URL PATCH für den neuen Endpunkt am retrained Modell verweisen 

Der vorherigen Schritte eine exemplarische Vorgehensweise finden Sie unter [neu trainieren einen klassischen Webdienst](machine-learning-retrain-a-classic-web-service.md).

Wenn Sie in einem klassischen Webdienst Umschulung Ansprechpartner ausführen, finden Sie unter [Problembehandlung der Umschulung eines Azure maschinellen Learning klassischen Webdiensts](machine-learning-troubleshooting-retraining-models.md).

Wenn Sie einen neuen Webdienst bereitgestellt:

* Melden Sie sich bei Ihrem Konto Azure Ressourcenmanager
* Abrufen der Definition der Web-Dienst
* Exportieren der Web Service Definition als JSON
* Aktualisieren Sie den Bezug auf die `ilearner` Blob in das JSON
* Importieren Sie die JSON in Web Service Definition
* Aktualisieren Sie den Webdienst mit neuen Web Service Definition

Der vorherigen Schritte eine exemplarische Vorgehensweise finden Sie unter [neu trainieren einen neuen Webdienst mithilfe der maschinellen Learning Management PowerShell-Cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).

Der Prozess zum Einrichten von Umschulung für einen klassischen Webdienst umfasst die folgenden Schritte aus:

![Prozessübersicht Umschulung][1]

Der Prozess zum Einrichten von Umschulung für einen neuen Webdienst umfasst die folgenden Schritte aus:

![Prozessübersicht Umschulung][7]

## <a name="other-resources"></a>Weitere Ressourcen

- [Umschulung und Aktualisieren von Azure maschinellen Learning Datenmodelle mit Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Erstellen Sie viele Computer Learning-Modelle und Web-Endpunkte aus einem Versuch, die mithilfe der PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- [AML Umschulung Modelle mithilfe von APIs](https://www.youtube.com/watch?v=wwjglA8xllg) Video wird gezeigt, wie auf Computer Learning-Modelle Azure Computer interessante erstellt neu trainieren Umschulung APIs und PowerShell verwenden.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

