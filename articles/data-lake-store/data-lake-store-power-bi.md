<properties
   pageTitle="Analysieren von Daten in Lake Datenspeicher mithilfe von Power BI | Microsoft Azure"
   description="Verwenden von Power BI Azure Lake Datenspeicher gehörende Kehrmatrix Datenanalyse"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analysieren von Daten in Lake Datenspeicher mithilfe von Power BI

In diesem Artikel erfahren Sie, wie mit Power BI-Desktop analysieren und Visualisieren von Daten in Azure Lake Datenspeicher gespeichert.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Azure dem Datenspeicher-Konto**. Folgen Sie den Anweisungen bei [den ersten Schritten mit Azure dem Datenspeicher mithilfe der Azure-Portal](data-lake-store-get-started-portal.md)an. In diesem Artikel wird vorausgesetzt, dass Sie bereits ein Konto Lake Datenspeicher, **Mybidatalakestore**, erstellt haben, und es eine Stichprobe-Datendatei (**Drivers.txt**) geladen. Diese Beispieldatei ist auf [Azure Daten dem Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)zum Download verfügbar.

- **Power BI-Desktop**. Sie können dies vom [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331)herunterladen. 


## <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI-Desktop

1. Starten Sie Power BI-Desktop, auf Ihrem Computer.

2. Klicken Sie im Menüband **Start** auf **Daten abrufen**, und klicken Sie dann auf Weitere. Klicken Sie im Dialogfeld **Daten importieren** **Azure**klicken Sie auf, klicken Sie auf **Azure dem Datenspeicher**, und klicken Sie dann auf **Verbinden**.

    ![Verbinden mit dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Verbinden mit dem Datenspeicher")

3. Wenn Sie über den Verbinder, die in einer Entwicklungsphase wird ein Dialogfeld angezeigt wird, entscheiden Sie sich um den Vorgang fortzusetzen.

4. Klicken Sie im Dialogfeld **Microsoft Azure dem Datenspeicher** die URL auf Ihr Konto Lake Datenspeicher, und klicken Sie dann auf **OK**.

    ![URL für dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL für dem Datenspeicher")

5. Klicken Sie im nächsten Dialogfeld auf Melden Sie sich bei Lake Datenspeicher-Konto **Anmelden** . Sie werden auf der Anmeldeseite Ihrer Organisation weitergeleitet. Folgen Sie den Anweisungen, melden Sie sich bei dem Konto ein.

    ![Melden Sie sich bei dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Melden Sie sich bei dem Datenspeicher")

6. Nachdem Sie sich erfolgreich angemeldet haben, klicken Sie auf **Verbinden**.

    ![Verbinden mit dem Datenspeicher] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Verbinden mit dem Datenspeicher")

7. Das Dialogfeld nächsten zeigt der Datei, die Sie sich bei Ihrem Konto Lake Datenspeicher hochgeladen. Überprüfen Sie die Informationen aus, und klicken Sie dann auf **Laden**.

    ![Daten aus dem Datenspeicher laden] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Daten aus dem Datenspeicher laden")

8. Nachdem die Daten in Power BI erfolgreich geladen wurde, sehen Sie die folgenden Felder in der Registerkarte **Felder** .

    ![Importierte Felder] (./media/data-lake-store-power-bi/imported-fields.png "Importierte Felder")

    Bevorzugen jedoch zum Visualisieren und Analysieren der Daten, wir die Daten pro die folgenden Felder verfügbar sein

    ![Gewünschte (Felder)] (./media/data-lake-store-power-bi/desired-fields.png "Gewünschte (Felder)")

    In den nächsten Schritten fort aktualisieren wir die Abfrage, um die importierten Daten in das gewünschte Format zu konvertieren.

9. Klicken Sie im Menüband **Home** auf **Abfragen bearbeiten**.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/edit-queries.png "Bearbeiten von Abfragen")

10. Im Abfrage-Editor, klicken Sie unter der Spalte **Content** , klicken Sie auf **binäre**.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/convert-query1.png "Bearbeiten von Abfragen")

11. Symbol für eine Datei wird, die die Datei **Drivers.txt** darstellt, die Sie hochgeladen angezeigt werden. Mit der rechten Maustaste in der Datei, und klicken Sie auf **CSV**.  

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/convert-query2.png "Bearbeiten von Abfragen")

12. Es sollte eine Ausgabe angezeigt, wie unten dargestellt. Ihre Daten sind jetzt verfügbar in ein Format, das Sie zum Erstellen von Visualisierungen verwenden können.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/convert-query3.png "Bearbeiten von Abfragen")

13. Im Menüband **Start** klicken Sie auf **Schließen und anwenden**, und klicken Sie dann auf **Schließen und zu übernehmen**.

    ![Bearbeiten von Abfragen] (./media/data-lake-store-power-bi/load-edited-query.png "Bearbeiten von Abfragen")

14. Nachdem Sie die Abfrage aktualisiert wird, wird die Registerkarte **Felder** die neuen Felder für die Visualisierung verfügbar angezeigt.

    ![Aktualisierten (Felder)] (./media/data-lake-store-power-bi/updated-query-fields.png "Aktualisierten (Felder)")

15. Lassen Sie uns Erstellen eines Kreisdiagramms, um die Treiber in jeder Stadt für einen angegebenen Land darzustellen. Stellen Sie hierzu die folgenden Optionen aus.

    1. Klicken Sie auf der Registerkarte Visualisierungen auf das Symbol für ein Kreisdiagramm.

        ![Kreisdiagramm erstellen] (./media/data-lake-store-power-bi/create-pie-chart.png "Kreisdiagramm erstellen")

    2. Die Spalten, die wir verwenden möchten, sind **Spalte 4** (Namen des Orts) und **Spalte 7** (Name des Land). Ziehen Sie diese Spalten aus der Registerkarte **Felder** zur Registerkarte **Visualisierungen** , wie unten dargestellt.

        ![Erstellen von Visualisierungen] (./media/data-lake-store-power-bi/create-visualizations.png "Erstellen von Visualisierungen")

    3. Das Kreisdiagramm sollte nun wie die nachfolgend aufgeführten ähneln.

        ![Kreisdiagramm] (./media/data-lake-store-power-bi/pie-chart.png "Erstellen von Visualisierungen")

16. Durch die Seite Ebene Filter ein bestimmtes Land auswählen, können Sie jetzt die Anzahl der Treiber in jeder Stadt der ausgewählten Land anzeigen. Wählen Sie unter der Registerkarte **Visualisierungen** unter **Filter Seitenebene**, z. B. **Brasilien**.

    ![Wählen Sie ein Land] (./media/data-lake-store-power-bi/select-country.png "Wählen Sie ein Land")

17. Das Kreisdiagramm wird automatisch aktualisiert, um die Treiber in der Brasilien Orten anzuzeigen.

    ![Treiber in einem Land] (./media/data-lake-store-power-bi/driver-per-country.png "Treiber pro Land")

18. Klicken Sie im Menü **Datei** auf **Speichern** , um die Visualisierung als Power BI-Desktop-Datei zu speichern.

## <a name="publish-report-to-power-bi-service"></a>Bericht auf Power BI-Dienst veröffentlichen

Nachdem Sie Visualisierungen in Power BI-Desktop erstellt haben, können Sie ihn durch Veröffentlichung auf der Power BI-Dienst für andere Personen freigeben. Informationen dazu, wie das geht finden Sie unter [Veröffentlichen von Power BI-Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Siehe auch

* [Analysieren von Daten in Lake Datenspeicher mit Daten dem Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
