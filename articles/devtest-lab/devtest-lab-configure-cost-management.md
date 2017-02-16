<properties
    pageTitle="Anzeigen den monatlichen Übung geschätzte Kosten Trend in Azure DevTest Kursen | Microsoft Azure"
    description="Lernen Sie die Azure DevTest Labs monatliche geschätzte Kosten Trend Diagramm aus."
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="tarcher"/>

# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Den monatliche Übung geschätzte Kosten Trend in Azure DevTest Einheiten anzeigen

Das Feature Kosten Verwaltung von DevTest Labs können Sie die Kosten für Ihre Übung besser nachverfolgen. In diesem Artikel veranschaulicht, wie das Diagramm **Monatliche geschätzte Kosten Trend** zu verwenden, um den aktuellen Kalender Monat geschätzte Kosten bis Datum und die geplanten Kosten der am Ende des Monats für den aktuellen Kalender Monat anzuzeigen. In diesem Artikel erfahren Sie, wie Sie das monatlichen geschätzte Kosten Trend Diagramm Azure-Portal anzeigen.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Anzeigen des Diagramms monatliche geschätzte Kosten Trend

Um die monatlichen geschätzte Kosten Trend Diagramm anzeigen möchten, gehen Sie folgendermaßen vor: 

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus.

1. Wählen Sie **Weitere Dienste**, und wählen Sie dann in der Liste **DevTest Labs** .

1. Wählen Sie aus der Liste der Labs die gewünschten Übung aus.   

1. Wählen Sie in der Übung des Blade **Kosten Einstellungen**aus.

1. Wählen Sie in der Übung die **Kosten Einstellungen** Blade **Übung Kosten Trend**aus.

1. Die folgende Abbildung zeigt ein Beispiel für ein Diagramm Kosten. 

    ![Kosten Diagramm](./media/devtest-lab-configure-cost-management/graph.png)

Der **geschätzte Kosten** Wert ist der aktuelle Kalender Monat geschätzte Kosten bis Datum. Die **Prognose Kosten** sind die Kosten für den gesamten aktuellen Kalender Monat mithilfe der Übung Kosten für die vorherigen fünf Tage berechnet.
 
Die Kosten Beträge werden auf die nächste ganze Zahl aufgerundet. Beispiel: 

- 5.01 rundet bis zu 6 
- 5.50 rundet bis zu 6
- 5.99 rundet bis zu 6

Wie es über dem Diagramm besagt, sind die Kosten, die im Diagramm angezeigt werden, dass die *geschätzte* Kosten mit [nutzungsbasierte](https://azure.microsoft.com/offers/ms-azr-0003p/) Sätzen anbieten.
Darüber hinaus folgende sind in der Berechnung *nicht* enthalten:

- CSP und Dreamspark Abonnements werden derzeit nicht unterstützt werden, wie Azure DevTest Labs die [Azure Abrechnung APIs](../billing-usage-rate-card-overview.md) verwendet, um die Kosten Übung zu berechnen, die CSP oder Dreamspark Abonnements nicht unterstützt.
- Ihr Angebot Sätzen. Wir können derzeit nicht mit Ihrem Angebot Sätzen (siehe unter Ihrem Abonnement), dass Sie mit Microsoft oder Microsoft Partner ausgehandelt haben. Wir verwenden die nutzungsbasierte Sätzen.
- Ihre steuern
- Ihre senken
- Ihre Abrechnung Währung. Die Kosten Übung wird derzeit nur in US-Dollar Währung angezeigt.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte von Blogbeiträgen

- [Zwei weitere Möglichkeiten, Ihre Kosten planmäßig in DevTest Kursen zu halten](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
- [Warum Schwellenwerte Kosten?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige Schritte, versuchen als Nächstes:

- [Definieren Übung Richtlinien](./devtest-lab-set-lab-policy.md) – erfahren Sie, wie Sie die verschiedenen Richtlinien gesteuert, wie Ihre Übung und deren virtuellen Computern verwendet werden. 
- [Erstellen benutzerdefinierter Bild](./devtest-lab-create-template.md) - beim Erstellen eines virtuellen Computers, geben Sie an eine Basis, die ein benutzerdefiniertes Bild oder ein Bild Marketplace werden kann. In diesem Artikel veranschaulicht, wie Sie ein benutzerdefiniertes Bild aus einer Datei virtuelle Festplatte zu erstellen.
- [Konfigurieren von Marketplace Bilder](./devtest-lab-configure-marketplace-images.md) - DevTest Labs unterstützt das Erstellen von virtuellen Computern basierend auf Bilder Azure Marketplace. In diesem Artikel wird veranschaulicht, wie Sie angeben, welche, sofern vorhanden, Azure Marketplace-Bilder werden können beim Erstellen von virtuellen Computern in einer Kurs verwendet.
- [Erstellen eines virtuellen Computers in einem Kurs](./devtest-lab-add-vm-with-artifacts.md) - veranschaulicht zum Erstellen eines virtuellen Computers aus einem Basis Bild (entweder benutzerdefinierte oder Marketplace), und wie Sie Elemente in Ihrem virtuellen Computer konzipiert.