<properties
    pageTitle="Konvertieren von einem Computer Learning Schulung experimentieren in einer Vorhersage experimentieren | Microsoft Azure"
    description="Informationen zum Konvertieren von einem Computer Learning Schulung experimentieren, verwendet für Schulung Vorhersageanalytik Modell, um eine Vorhersage besser, die als Webdienst bereitgestellt werden kann."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Konvertieren von einem Computer Learning Schulung experimentieren in einer Vorhersage experimentieren

Azure maschinellen Learning ermöglicht es Ihnen zu erstellen, testen und Bereitstellen von Lösungen Vorhersageanalytik.

Nachdem Sie erstellt und auf einer *Schulung experimentieren* zum Schulen von Vorhersageanalytik Modell durchlaufen haben und Sie zur gemeinsamen Nutzung von Punktzahl neue Daten bereit sind, müssen Sie vorbereiten und Optimieren Ihrer experimentieren Sie bewerten. Sie können dieses *Vorhersage experimentieren* dann als Azure Webdienst bereitstellen, sodass die Benutzer können Daten in Ihr Modell senden und Empfangen von Vorhersagen des Modells.

Konvertieren in einer Vorhersage experimentieren, erhalten Sie ausgebildete Modell als Webdienst bereitgestellt werden bieten. Benutzer des Webdiensts sendet Eingabedaten an Ihr Modell und Modell werden die Vorhersageergebnisse zurücksenden. Wie Sie in einer Vorhersage experimentieren konvertieren sollten Sie also Bedenken wie erwartet Modell aus, um die von anderen verwendet werden.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Die Vorgehensweise zum Konvertieren einer Schulung experimentieren zu einer Vorhersage experimentieren umfasst drei Schritte:

1.  Speichern Sie das Modell der Computer-Schulung, die Sie haben gelernt, und Ersetzen Sie den Computer learning Algorithmus und [Zug Modell] [ train-model] Module mit gespeicherten ausgebildeten Modell.
2.  Kürzen der experimentieren, um nur die Module, die für die Bewertung erforderlich sind. Eine Schulung experimentieren Sie zahlreiche Module, die für die Schulung erforderlich sind, aber nicht benötigt werden, nachdem Sie das Modell vorbereitet und zur Bewertung verwendet wird.
3.  Definieren Sie, wo in Ihrem experimentieren Sie Daten aus den Dienst Webbenutzer akzeptieren, und welche Daten zurückgegeben werden.

## <a name="set-up-web-service-button"></a>Web-Service-Schaltfläche

Nachdem Sie Ihre experimentieren (Schaltfläche am unteren Rand des Zeichenbereichs experimentieren**Ausführen** ) ausgeführt haben, wird die Schaltfläche **Web-Dienst** (auswählen die Option **Vorhersage Webdienst** ) für Sie die drei Schritte der Konvertierung Ihrer Schulung experimentieren zu einer Vorhersage experimentieren ausführen:

1.  Es werden ausgebildete Modell gespeichert, wie ein Modul im Abschnitt **Ausgebildeten Modelle** der Palette Modul (auf der linken Seite des Zeichnungsbereichs experimentieren), dann ersetzt den Computer learning Algorithmus und [Zug Modell] [ train-model] Module mit dem gespeicherten ausgebildeten Modell.
2.  Entfernt die Module, die nicht klar erforderlich sind. In diesem Beispiel umfasst die [Aufgeteilten Daten][split], 2nd [Punktzahl Modell][score-model], und [Modell auswerten] [ evaluate-model] Module.
3.  Es erstellt Web Service Eingabe und Module ausgeben und standardmäßig an Ihre Versuch hinzugefügt.

Die folgenden experimentieren Schulung beispielsweise ein zwei-Klasse stärkere Entscheidungsbaummodell Erhebung von Beispieldaten verwenden:

![Schulung experimentieren][figure1]

Führen Sie die Module in diesem Versuch im Grunde vier verschiedene Funktionen:

![Modulfunktionen][figure2]

Wenn Sie diese Schulung experimentieren in einer Vorhersage experimentieren konvertieren, einige dieser Module werden nicht mehr benötigt oder sie haben einen anderen Zweck:

- **Daten** - Daten in diesem Beispiel Dataset wird nicht verwendet, während die Bewertung - der Benutzer des Webdiensts wird bewertet werden Daten eingeben. Die Metadaten aus diesem Dataset, z. B. Datentypen, wird jedoch vom ausgebildeten Modell verwendet. Daher müssen Sie das Dataset den Vorhersage Versuch beibehalten, damit sie diese Metadaten bereitstellen kann.

- **Vorbereiten** – je nach den Daten, die zur Bewertung, übermittelt werden diese Module möglicherweise oder möglicherweise nicht erforderlich sind, um die eingehenden Daten zu verarbeiten.

    Beispielsweise ist in diesem Beispiel möglicherweise das Stichprobendataset fehlende Werte und enthalten sind die Spalten, die nicht benötigt werden, um das Modell Schulen. Damit eine [Übersichtliche fehlende Daten] [ clean-missing-data] Modul war Geschäft mit fehlenden Werten und eine [Wählen Sie Spalten im Dataset] enthalten[ select-columns] Modul war ausgeschlossen werden zusätzlichen Spalten Datenfluss enthalten. Wenn Sie wissen, dass die Daten, die zur Bewertung über den Webdienst übermittelt werden keine fehlende Werte, dann können Sie die [Säubern fehlende Daten] entfernen[ clean-missing-data] Modul. Jedoch seit der [Spalten im Dataset auswählen] [ select-columns] Modul hilft Ihnen bei definieren den Satz von Features bewertet wird, muss diese Modul bleiben.

- **Zug** - nachdem Modell trainierte, speichern Sie es als ein einzelnes ausgebildeten Modell Modul. Klicken Sie dann ersetzen Sie diese einzelne Module mit dem gespeicherten ausgebildeten Modell.

- In diesem Beispiel das Modul Teilen dient **Punktzahl** - Datenstreams in eine Reihe von Test und Schulungsdaten unterteilen. In der Vorhersage experimentieren Dies ist nicht erforderlich und entfernt werden kann. Auf ähnliche Weise 2nd [Punktzahl Modell] [ score-model] Modul und das [Modell auswerten] [ evaluate-model] Modul werden verwendet, um die Ergebnisse aus den Testdaten zu vergleichen, damit diese Module in die Vorhersage experimentieren auch nicht erforderlich sind. Verbleibende [Punktzahl Modell] [ score-model] Modul, wird jedoch benötigt, um ein Ergebnis Punktzahl über den Webdienst zurückzugeben.

So sieht Beispiel nach dem Klicken auf **Web-Dienst**:

![Konvertierte Vorhersage experimentieren.][figure3]

Dies ist möglicherweise ausreichend Vorbereiten Ihrer experimentieren als Webdienst bereitgestellt werden soll. Möglicherweise möchten jedoch führen Sie einige zusätzliche Aufgaben, die speziell für Ihre experimentieren.

### <a name="adjust-input-and-output-modules"></a>Anpassen von Eingabe- und Module

In Ihrem Versuch Schulung verwendet eine Reihe von Schulungsdaten und dann einige Verarbeitung vor, um die Daten in einem Formular zu erhalten, die der Computer Learning-Algorithmus hat. Wenn die Daten, die Sie nicht vorhaben, über den Webdienst erhalten diese Verarbeitung nicht benötigen, können Sie die **Eingabewerte Web Service-Modul** in Ihrem Versuch in einen anderen Knoten verschieben.

Standardmäßig verschoben **Web-Dienst** beispielsweise das Modul **Web Service Eingabe** am oberen Rand der Datenfluss, wie in der Abbildung oben. Wenn die Eingabedaten diese Verarbeitung nicht benötigen, können jedoch dann Sie manuell die **Web-Dienst Eingabesprache** ältere Module Datenverarbeitung platzieren:

![Verschieben Sie die Eingabe der Web-Dienst][figure4]

Die eingegebenen Daten über den Webdienst bereitgestellt werden jetzt ohne Vorverarbeitungsschritten direkt in das Modul Punktzahl Modell übergeben.

Auf ähnliche Weise verschoben **Web-Dienst** standardmäßig am unteren Rand der Datenfluss Moduls Ausgabe Service Web. In diesem Beispiel der Webdienst zurückgegeben werden kann der Benutzer die Ausgabe der [Punktzahl Modell] [ score-model] Modul, wozu auch die vollständige Eingabedaten Vektor sowie die Bewertung ergibt.

Wenn Sie lieber zurückzugebenden unterschiedliche – beispielsweise, nur die Punktzahl Ergebnisse und nicht den gesamten Vektor der eingegebenen Daten – und Sie können jedoch Einfügen einer [Wählen Sie Spalten im Dataset] [ select-columns] Modul alle Spalten außer der Punktzahl Ergebnisse ausgeschlossen. Klicken Sie dann verschieben Sie das Modul für die **Ausgabe der Web-Dienst** in die Ausgabe der [Spalten im Dataset auswählen] [ select-columns] Modul:

![Verschieben Sie die Ausgabe der Web-Dienst][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Fügen Sie hinzu oder entfernen Sie zusätzliche Datenverarbeitung Module

Wenn in Ihrer experimentieren, die Sie nicht benötigt werden wissen, während bewerten Weitere Module vorhanden sind, können diese entfernt werden. Angenommen, da wir das Modul **Web Service Eingabe** eines Punkts nach der Datenverarbeitung Module aufgerufen haben, wir können entfernen [Säubern fehlende Daten] [ clean-missing-data] Modul aus dem Versuch, Vorhersage.

Unsere Vorhersage experimentieren sieht jetzt wie folgt aus:

![Entfernen zusätzliche Modul][figure6]

### <a name="add-optional-web-service-parameters"></a>Fügen Sie optional Web Service Parameter hinzu

In einigen Fällen empfiehlt es sich, dass der Benutzer des Web Service so ändern Sie das Verhalten der Module aus, wenn der Zugriff auf den Dienst. *Web Service Parametern* können Sie wie folgt vorgehen.

Ein gängiges Beispiel ist die [Daten importieren] einrichten[ import-data] Modul, damit der Benutzer des bereitgestellten Webdiensts eine andere Datenquelle angeben kann, wenn der Webdienst zugegriffen wird. Oder konfigurieren die [Daten exportieren] [ export-data] Modul, dass ein anderes Ziel angegeben werden kann.

Können Sie Web-Dienstparameter definieren und einen oder mehrere Parameter für das Modul zuordnen, und Sie können angeben, ob diese erforderlich oder optional sind. Der Benutzer des Webdiensts kann dann Werte für diese Parameter bereitstellen, beim Zugriff auf den Dienst und die Aktionen Modul entsprechend geändert werden.

Weitere Informationen zu Parametern für Web Service, finden Sie unter [Verwenden Azure maschinellen Learning Web Service Parameters ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Bereitstellen der Vorhersage experimentieren als Webdienst

Nachdem Sie nun die Vorhersage experimentieren ausreichend vorbereitet wurde, können Sie es als Azure Webdienst bereitstellen. Mithilfe des Webdiensts, Benutzer können Daten in Ihr Modell senden und das Modell zurückgegeben, deren Vorhersagen werden kann.

Weitere Informationen über den Bereitstellungsvorgang abgeschlossen finden Sie unter [Bereitstellen eines Webdiensts Azure Computer-Schulung][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
