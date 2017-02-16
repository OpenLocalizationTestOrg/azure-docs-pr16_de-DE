<properties 
    pageTitle="Protokollierung für maschinelle Learning-Webdiensten | Microsoft Azure" 
    description="Informationen Sie zum Aktivieren der Protokollierung für maschinelle Learning-Webdiensten. Protokollierung bietet zusätzliche Informationen, um die APIs zu beheben." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Aktivieren der Protokollierung für maschinelle Learning-Webdiensten  

Dieses Dokument enthält Informationen über die Möglichkeit, Protokollierung klassischen Webdienste. Aktivieren der Protokollierung in Web Services bietet zusätzliche Informationen über nur eine Fehlernummer und einer Nachricht, die Ihre Anrufe an den Computer Learning-APIs bei der Problembehandlung helfen können.  

So aktivieren Sie die Protokollierung in Webdienste im klassischen Azure-Portal:   

1.  Melden Sie sich bei [Azure klassischen-portal](https://manage.windowsazure.com/)
2.  Klicken Sie in der Spalte links Features auf **Computer Schulung**.
3.  Klicken Sie auf den Arbeitsbereich, klicken Sie dann **Webdienste**.
4.  Klicken Sie in der Liste der Web-Dienste auf den Namen des Webdiensts.
5.  Klicken Sie in der Liste Endpunkte auf den Endpunktnamen.
6.  Klicken Sie auf **Konfigurieren**.
7.  Legen Sie die **Diagnose SPUR Ebene** auf *Fehler* oder *Alle*, und klicken Sie auf **Speichern**.

Zum Aktivieren der Protokollierung im Portal Azure maschinellen Learning-Webdiensten.

1. Melden Sie sich am Portal [Azure maschinellen Learning-Webdiensten](https://services.azureml.net) .
2. Klicken Sie auf klassische Webdienste.
3.  Klicken Sie in der Liste der Web-Dienste auf den Namen des Webdiensts.
4.  Klicken Sie in der Liste Endpunkte auf den Endpunktnamen.
5.  Klicken Sie auf **Konfigurieren**.
6.  Legen Sie die **Protokollierung** auf *Fehler* oder *Alle*, und klicken Sie auf **Speichern**.

## <a name="the-effects-of-enabling-logging"></a>Die Effekte des Aktivieren der Protokollierung

Wenn die Protokollierung aktiviert ist, verknüpft alle, die für Diagnose und Fehler vom ausgewählten Endpunkt Azure-Speicher-Konto angemeldet sind, mit Workspace des Benutzers. Sie können dieses Speicherkonto im Portal Azure klassischen Dashboard-Ansicht (unten im Abschnitt schnellen Blick) von seinem Arbeitsbereich anzeigen.  

Die Protokolle können verhindert, die verschiedene Tools für 'Ein Azure-Konto Speicher Durchsuchen' angezeigt werden. Die einfachste möglicherweise mit dem Konto Speicher im klassischen Azure-Portal einfach navigieren, und klicken Sie auf **Container**. Sie möchten einen Container namens **ml-Diagnose**lesen. Dieser Container enthält die Diagnoseinformationen für alle die Webdienst-Endpunkte für alle Arbeitsbereiche, die mit diesem Konto Speicher verbunden sind. 
 
## <a name="log-blob-detail-information"></a>Protokollinformationen Blob-Details

Jede Blob im Container enthält Diagnoseinformationen für genau eine der folgenden Aktionen aus:

-   Eine Ausführung der Stapel Ausführung Methode  
-   Eine Ausführung der Anforderung / Antwort-Methode  
-   Initialisierung eines Containers Anforderung / Antwort
  
Der Name der einzelnen Blob weist ein Präfix im folgenden Format an: 

{Arbeitsbereich Id}-{-Webdienst ab Id}-{Endpunkt Id} / {Log Typ}  

Wo ist Log-Typ einen der folgenden Werte:  

- Stapelverarbeitung  
- Punktzahl/Besprechungsanfragen  
- Punktzahl/Initialisierung  