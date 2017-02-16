<properties 
    pageTitle="Azure Computer Learning Web Service Parameter verwenden | Microsoft Azure" 
    description="So Azure maschinellen Learning Web Service Parameter verwenden, um das Verhalten Ihres Modells ändern, wenn der Webdienst zugegriffen wird." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Verwenden Sie Learning Web Service Parameter Azure-Computern

Veröffentlichen einer experimentieren, die Module mit konfigurierbare Parameter enthält wird ein Azure maschinellen Learning-Webdienst erstellt. In einigen Fällen möchten Sie möglicherweise das Modul Verhalten zu ändern, während der Webdienst ausgeführt wird. *Parameter der Web-Dienst* können Sie diese Aufgabe auszuführen. 

Ein gängiges Beispiel ist die [Daten importieren] einrichten[ reader] Modul, damit der Benutzer der veröffentlichten Webdienst eine andere Datenquelle angeben kann, wenn der Webdienst zugegriffen wird. Oder konfigurieren die [Daten exportieren] [ writer] Modul, dass ein anderes Ziel angegeben werden kann. Einige weitere Beispiele umfassen ändern die Anzahl von Bits für das [Feature Hashing] [ feature-hashing] Modul oder die Anzahl der gewünschten Features für die [Featureauswahl Filter-basierten] [ filter-based-feature-selection] Modul. 

Sie können Web Dienst-Parameter festlegen und diese in Ihrem Versuch einen oder mehrere Parameter für das Modul zuordnen, und können Sie angeben, ob diese erforderlich oder optional sind. Der Benutzer des Webdiensts kann dann Werte für diese Parameter bereitstellen, wenn sie den Webdienst aufzurufen. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Zum Einrichten und Verwenden von Web Service-Parameter

Definieren Sie Parameter ein Web-Dienst, indem Sie auf das Symbol neben dem Parameter für ein Modul und auswählen "Als Web Service Parameter festlegen". Dies erstellt einen neuen Parameter der Web-Dienst und verbindet es mit diesen Parameter Modul. Klicken Sie dann, wenn der Webdienst zugegriffen wird, kann der Benutzer einen Wert für Parameter für den Webdienst angeben und es wird dem Modul Parameter angewendet.

Nachdem Sie einen Web-Service-Parameter definieren, ist es auf einen beliebigen anderen Modul Parameter den Versuch verfügbar. Wenn Sie einen Parameter für ein Modul zugeordnet Web Service Parameter definieren, können Sie die gleichen Parameter der Web-Dienst für alle anderen Modul verwenden, solange der Parameter der Wert desselben Typs erwartet wird. Beispielsweise ist Parameter für den Webdienst einen numerischen Wert, kann dann es nur für Parameter für das Modul verwendet werden, die einen numerischen Wert erwartet. Wenn der Benutzer einen Wert für Parameter für den Webdienst festlegt, wird es auf alle zugeordneten Modulparameter angewendet werden.

Sie können entscheiden, ob einen Standardwert für Parameter für den Webdienst angeben. In diesem Fall ist der Parameter für den Benutzer, der den Webdienst optional. Wenn Sie einen Standardwert nicht angeben, muss der Benutzer einen Wert eingeben, wenn der Webdienst zugegriffen wird.

Die API-Dokumentation für den Webdienst enthält Informationen für den Dienst Webbenutzer Parameter für den Webdienst programmgesteuert angegeben werden, wenn Sie den Webdienst zugreifen.

>[AZURE.NOTE] Die API-Dokumentation für einen klassischen Webdienst wird über den Link **Hilfe-API** im Webdienst **DASHBOARD** in Computer Learning Studio bereitgestellt. Die API-Dokumentation für einen neuen Webdienst wird über das Portal [Azure maschinellen Learning-Webdiensten](https://services.azureml.net/Quickstart) auf den Seiten **verbrauchen** und **Swagger API** für Ihren Webdienst bereitgestellt.


##<a name="example"></a>Beispiel

Als Beispiel, angenommen wir haben ein Versuch mit einem [Daten exportieren] [ writer] Modul, das Informationen zu Azure Blob-Speicher sendet. Wir werden mit dem Namen "BLOB-Pfad" Web Service Parameter definieren, mit der der Dienst Webbenutzer beim Zugriff auf den Dienst, ändern Sie den Pfad zu Blob-Speicher.

1.  Maschinelle Learning Studio, klicken Sie auf die [Daten exportieren] [ writer] Modul, um ihn zu markieren. Ihre Eigenschaften werden im Bereich Eigenschaften rechts neben den Zeichenbereich experimentieren angezeigt.

2.  Geben Sie den Speicher:

    - Wählen Sie unter **Text Bitte betrachten Sie geben Datenziel**"Azure BLOB-Speicher" ein.
    - Wählen Sie unter **Bitte angeben Authentifizierungstyp**"Konto" ein.
    - Geben Sie die Kontoinformationen für die Azure Blob-Speicher. 
    <p />

3.  Klicken Sie auf das Symbol rechts neben den **Pfad zu Anfang mit Containerparameter BLOB**. Es sieht wie folgt aus:

    ![Web Service Abfrageparameter (Symbol)][icon]

    Wählen Sie "Als Web Service Parameter festlegen" aus.

    Klicken Sie unter **Web-Service-Parameter** unten im Bereich Eigenschaften mit dem Namen "Pfad zu Anfang Container BLOB-" wird ein Eintrag hinzugefügt. Dies ist der Parameter der Web-Dienst, jetzt ist, diese [Daten exportieren] zugeordnet[ writer] Modul Parameter.

4.  Wenn Parameter für den Webdienst umbenennen möchten, klicken Sie auf den Namen, geben Sie "BLOB-Pfad", und drücken Sie **die EINGABETASTE** . 
 
5.  Um einen Standardwert für Parameter für den Webdienst bereitstellen zu können, klicken Sie auf das Symbol rechts neben den Namen, wählen Sie "Standardwert bereitstellen", geben Sie einen Wert (beispielsweise "container1/output1.csv"), und drücken Sie **die EINGABETASTE** .

    ![Der Parameter des Diensts][parameter]

6.  Klicken Sie auf **Ausführen**. 

7.  Klicken Sie auf **Webdienst bereitstellen** , und wählen Sie den Webdienst bereitstellen **Webdienst bereitstellen [klassischen]** oder einem **Webdienst bereitstellen [New]** .

Nun kann der Benutzer des Webdiensts ein neues Ziel angeben, für die [Daten exportieren] [ writer] Modul, wenn Sie den Webdienst zugreifen.

##<a name="more-information"></a>Weitere Informationen

Eine ausführlichere Beispiel finden Sie im [Web Service Parameter](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) -Eintrag im [Computer Learning-Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Weitere Informationen zum Zugriff auf einem Computer Learning-Webdienst finden Sie unter [so einen veröffentlichten Computer learning Webdienst nutzen](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
