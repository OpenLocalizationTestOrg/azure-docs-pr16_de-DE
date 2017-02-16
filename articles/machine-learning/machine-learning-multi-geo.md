<properties
   pageTitle="Multi-Geo Hilfedokumentation | Microsoft Azure"
   description="Informationen zum Erstellen eines Arbeitsbereichs und Veröffentlichen von einem Webdienst in einer anderen aus Süd zentralen USA (SCUS) Azure Region Azure Region."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Multi-Geo Hilfedokumentation

Dieser Artikel beschreibt, wie Sie einen Arbeitsbereich erstellen und Veröffentlichen von einem Webdienst in anderen Azure Regionen.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

1. Melden Sie sich bei dem klassischen Azure-Portal.

2.  Klicken Sie auf **+ neue** > **DATA SERVICES** > **Computer LEARNING** > **SYMBOLLEISTE erstellen**.  Wählen Sie unter **Speicherort** wie **Asien oder**eine andere Region ein.
![Hilfe Multi-Geo Bild 1][1]
3. Wählen Sie den Arbeitsbereich aus, und klicken Sie dann auf **Anmelden bei ML Studio**.
![Hilfe Multi-Geo Bild 2][2]

4. Sie verfügen nun über einen Arbeitsbereich in einem anderen Region, mit denen Sie genau wie alle anderen Arbeitsbereich möglicherweise. Suchen Sie zum Umschalten zwischen Ihre Arbeitsbereiche, in der oberen rechten Ecke des Bildschirms. Klicken Sie auf den Dropdownpfeil, wählen Sie den Bereich aus, und wählen Sie dann auf den Arbeitsbereich. Alles ist lokal in der Region Arbeitsbereich. Beispielsweise werden alle Ihre Webdienste in einem Arbeitsbereich erstellt in derselben Region, in dem sich der Arbeitsbereich befindet.
![Hilfe Multi-Geo Bild 3][3]

## <a name="open-an-experiment-from-gallery"></a>Öffnen Sie ein Versuch aus dem Katalog

Wenn Sie einem Versuch aus dem Katalog öffnen, können Sie auch die Region auswählen, den Versuch zu kopieren möchten.

![Hilfe Multi-Geo Bild 4][4a]

## <a name="web-service-management"></a>Web Servicemanagement

Verwenden Sie die regionsspezifische-Adresse, um Webdienste, beispielsweise für Umschulung programmgesteuert zu verwalten:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Dinge zu beachten

1.  Sie können nur Versuche zwischen Arbeitsbereiche kopieren, die auf den Bereich auf diese Weise gehören. Wenn Sie die zu kopierenden experimentieren müssen über Arbeitsbereiche in unterschiedlichen Regionen, können die [PowerShell](http://aka.ms/amlps) -Cmdlet [*Kopieren-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) durchführen. Eine weitere Möglichkeit besteht darin den Versuch in Katalog in aufgelistete Modus zu veröffentlichen öffnen Sie Sie dann im Arbeitsbereich aus einem anderen Region.
2.  Der Ansichtsauswahl Region wird nur Arbeitsbereiche für eine einzelne Region nacheinander angezeigt werden. In der Zukunft werden kann eine vollständige Liste der Arbeitsbereiche sehen Sie Zugriff über alle Regionen zur gleichen Zeit haben.  
3.  Eine kostenlose oder Gast Access (anonyme) wird erstellt und in einem zentralen Süd US gehostet wird In der Zukunft werden Free/Gast Access-Arbeitsbereiche in der Region zu erstellen, die Sie auswählen können.  
4.  Webdienste in einem Arbeitsbereich in Asien oder bereitgestellt werden auch in Asien oder gehostet werden. In der Zukunft haben die Flexibilität von Versuche in einem Region erstellen können, und Bereitstellen von Webdienst-Endpunkten in verschiedener Regionen generiert.  

## <a name="more-information"></a>Weitere Informationen

Stellen Sie eine Frage im [Forum Azure maschinellen Schulung](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
