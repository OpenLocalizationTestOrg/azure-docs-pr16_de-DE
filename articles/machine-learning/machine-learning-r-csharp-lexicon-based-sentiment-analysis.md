<properties 
    pageTitle="Wörterbuch basierend Grüße Analyse | Microsoft Azure" 
    description="Wörterbuch basierend Grüße Analyse" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Wörterbuch basierend Grüße Analyse 

Wie können Sie Benutzer meinungen und messen überlegen in Richtung Marken oder Themen in sozialen Netzwerken wie Facebook Beiträgen Tweets, Prüfungen usw..? Grüße Analyse bietet eine Methode zum Analysieren von solche Fragen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Im Allgemeinen stehen zwei Methoden zum Grüße Analyse. Eine wird mithilfe eines Kontrolle Learning-Algorithmus und anderen als unbeaufsichtigt Learning behandelt werden. Ein Kontrolle Learning-Algorithmus erstellt eine Einstufung Modell im Allgemeinen auf einer großen kommentierten trennen. Seine Genauigkeit basiert auf der Qualität der Anmerkung hauptsächlich und normalerweise der Prozess Schulung werden sehr lange dauern. Neben, das beim Anwenden des Algorithmus auf eine andere Domäne ist das Ergebnis in der Regel nicht gute. Im Vergleich um zu überwacht Lern-, -Wörterbuch-basierten unbeaufsichtigt Learning verwendet Grüße Wörterbuch dar, die Speichern einer großen Daten trennen und Schulung erforderlich ist –, wodurch den gesamten Prozess viel schneller. 

Unseren [Service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) basiert auf der MPQA Subjektivität-Wörterbuch (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), also eine der am häufigsten verwendeten Subjektivität Lexika. Es gibt 5097 negative und 2533 positive Wörter in MPQA ein. Und alle diese Wörter als sicherer oder schwach Polarität kommentiert werden. Die gesamte trennen liegt unter GNU General Public License. Der Webdienst kann auf alle kurze Sätze, z. B. Tweets und Facebook Beiträge angewendet werden. 

>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts

Die Eingabedaten Text werden können, aber der Webdienst funktioniert besser mit kurze Sätze. Die Ausgabe ist einen numerischen Wert zwischen-1 und 1. Jeder Wert unter 0 kennzeichnet, dass die Absicht des Texts negativ ist; Wenn Sie über 0 positive. Der Absolute Wert des Ergebnisses kennzeichnet die Stärke von den zugeordneten Grüße. 

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Die Eingabe ist "Heute ist eine gute Tag". Die Ausgabe ist "1", der festlegt, eine positive Stimmung Ausdrücken mit der Eingabe im Satz großschreiben verknüpft ist. 

##<a name="creation-of-web-service"></a>Erstellen von Webdienst
>Dieser Webdienst wurde mithilfe von Azure maschinellen Learning erstellt. Kostenlose Testversion sowie Einführung Videos zum Erstellen von Versuche und [publishing-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Es folgt ein Screenshot der Versuch, die den Web-Dienst und Beispiel-Code für jede der Module in den Versuch erstellt.


Aus innerhalb von Azure Computer lernen, wurde eine neue, leere experimentieren erstellt. Die folgende Abbildung zeigt den Datenfluss experimentieren Analysis-Wörterbuch-basierten Grüße. Die Datei "sent_dict.csv" die MPQA Subjektivität-Wörterbuch, und festgelegt ist als eine der Eingaben des [R Skripts ausführen][execute-r-script]. Eine andere Eingabe ist eine Stichprobe überprüfen aus dem Amazon überprüfen Dataset für Test, wobei wir Auswahl Namensänderung Spalte, ausgeführt und Vorgänge teilen. Wir verwenden ein Pakets Hash Subjektivität-Wörterbuch im Speicher speichern und beschleunigen Sie den Punktzahl Berechnung aus. Der gesamte Text wird durch "tm" Paket Token und mit dem Wort im Wörterbuch Grüße verglichen werden. Schließlich wird die Bewertung durch Hinzufügen von der Gewichts aller subjektive Wörter im Text berechnet werden. 

###<a name="experiment-flow"></a>Experimentieren Fluss:

![Experimentieren Fluss][2]


####<a name="module-1"></a>Modul 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Installieren Sie Hash-Paket
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Einschränkungen

Aus einer Perspektive Algorithmus ist-Wörterbuch-basierten Grüße Analyse eine allgemeine Stimmung Ausdrücken Analysetool, mit denen sich möglicherweise besser als die Klassifizierungsmethode für bestimmte Felder nicht ausführen. Das Problem Negation ist nicht gut behandelt. Wir hartcodieren mehrere Negation von Wörtern in unserem Programm, aber eine bessere Möglichkeit ist ein Wörterbuch Negation Verwendung und einige Regeln erstellen. Webdienst führt besser auf kurze und einfache Sätze, z. B. Tweets und Facebook-Beiträge als lange und komplexe Sätze wie Amazon Prüfungen. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
