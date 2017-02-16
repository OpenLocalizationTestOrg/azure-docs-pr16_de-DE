<properties 
    pageTitle="Wie Blitline für Bild gewünschten feature Verarbeitung - Azure Leitfaden" 
    description="Erfahren Sie, wie den Dienst Blitline Prozess Bilder innerhalb einer Azure-Anwendung verwenden." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>So verwenden Sie Blitline mit Azure und Azure-Speicher

Mit diesem Leitfaden wird erläutert, wie Blitline Dienste zuzugreifen und Aufträge zu Blitline senden.

## <a name="what-is-blitline"></a>Was ist Blitline?

Blitline ist ein cloudbasierten Bild Verarbeitung Dienst, Enterprise Ebene Bild Verarbeitung am Dezimalbruch des Preises, die es Kosten würde bereitstellt, um es sich selbst zu erstellen.

Tatsächlich bietet Bild Verarbeitung normalerweise von Grund auf für jede Website neu erstellt immer wieder durchgeführt wurde. Wir einreichen, da wir ihnen eine Millionen Zeiten auch erstellt haben. Tag, die wir entschieden, dass vielleicht es Zeit ist erledigen wir nur für alle Benutzer. Wir wissen, wie Sie tun, um schnell und effizient erledigen, und speichern Sie alle Arbeiten in der Zwischenzeit.

Weitere Informationen finden Sie unter [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Was Blitline nicht ist...

Wenn Sie verdeutlichen möchten, was Blitline hilfreich ist, ist es häufig einfacher zu identifizieren, welche Blitline nicht geschieht, bevor Sie nach vorne verschieben.

- Blitline hat keine HTML-Produkte an Bilder hochladen. Sie müssen Bilder verfügbar öffentlich oder mit eingeschränkten Berechtigungen verfügbar für Blitline erreichen können.

- Blitline führt keine Verarbeitung, wie Aviary.com Livebild

- Blitline akzeptiert keine Uploads Bild, Sie können keine Blitline Ihre Bilder direkt Pushbenachrichtigungen. Drücken Sie sie in Azure-Speicher oder anderen Stellen Blitline unterstützt und Blitline dann feststellen, wo abrufen müssen.

- Blitline ist parallele und führt keine synchrone Verarbeitung. D. h., Sie müssen Geben Sie uns eine Postback_url und können wir Ihnen mitteilen wir Abschluss Verarbeitung.

## <a name="create-a-blitline-account"></a>Erstellen Sie ein Konto Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Erstellen eines Auftrags Blitline

Blitline verwendet JSON, um die Aktionen zu definieren, die Sie auf ein Bild übernehmen möchten. Diese JSON besteht einige einfache Felder.

Das einfachste Beispiel lautet wie folgt aus:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Hier haben wir JSON, mit denen ein "Src" Bild "... boys.jpeg", und ändern Sie die Größe, die diesem Bild zu 240 x 140.

Die Anwendung-ID ist etwas, das Sie in der Registerkarte **VERBINDUNGSINFORMATIONEN** oder **Verwalten** Azure finden können. Es ist Ihre geheimen Bezeichner enthält, die Sie Aufträge auf Blitline ausführen kann.

"Speichern" identifiziert Informationen zu der Stelle, an der das Bild setzen, nachdem wir es verarbeitet haben soll. In diesem Fall einfach noch nicht wir eine definiert. Wenn kein Speicherort definiert ist Blitline speichert es lokal (und vorübergehend) bei einem eindeutigen Cloud-Speicherort. Sie werden keine Abrufen von diesem Speicherort aus den JSON von Blitline zurückgegeben wird, wenn Sie die Blitline vornehmen. Der Bezeichner "Image" ist erforderlich, und es wird Ihnen zurückgegeben, wenn diese spezielle identifizieren Bild gespeichert.

Weitere Informationen zu den *Funktionen* , wir hier unterstützt finden Sie: <http://www.blitline.com/docs/functions>

Finden Sie auch Dokumentation zu den Auftragsoptionen hier: <http://www.blitline.com/docs/api>

Nachdem Sie Ihre JSON haben Sie müssen lediglich **Beitrag** er`http://api.blitline.com/job`

Sie erhalten JSON zurück, die sieht ungefähr wie folgt aus:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Dies zeigt an, dass Blitline hat Ihre Anfrage erhalten, es in einer Verarbeitungswarteschlange ablegen hat und nach Abschluss des Bilds verfügbar werden: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>So speichern Sie ein Bild mit Ihrem Konto Azure-Speicher

Wenn Sie ein Speicher Azure-Konto besitzen, können Sie problemlos Blitline verarbeiteten Bilder in Ihrem Azure Container Pushbenachrichtigungen speichert. Durch Hinzufügen einer "Azure_destination" definieren Sie den Speicherort und die Berechtigungen für Blitline zu übertragen.

Hier ist ein Beispiel:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Durch die Werte CAPITALIZED durch ein eigenes ausgefüllt wurden, können Sie diese JSON und http://api.blitline.com/job übermitteln und das "Src" Bild mit Weichzeichnungsfilter verarbeitet und klicken Sie dann auf Azure Ziel abgelegt wird.

###<a name="please-note"></a>Bitte beachten Sie:

Die SAS muss den gesamten SAS-Url, einschließlich der Dateiname der Zieldatei enthalten.

Beispiel:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Sie können auch die neueste Version von Blitlines Azure-Speicher Dokumente lesen [können](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie blitline.com zum Lesen von Informationen zu unseren anderen Features aus:

* Blitline API Endpunkt Dokumente <http://www.blitline.com/docs/api>
* Blitline API Funktionen <http://www.blitline.com/docs/functions>
* Beispiele für die Blitline-API <http://www.blitline.com/docs/examples>
* Dritter Teil Nuget Bibliothek <http://nuget.org/packages/Blitline.Net>
