<properties
   pageTitle="Registrieren Sie Daten aus Lake Datenspeicher in Azure Datenkatalog | Microsoft Azure"
   description="Registrieren Sie Daten aus Lake Datenspeicher in Azure-Datenkatalog"
   services="data-lake-store,data-catalog" 
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registrieren Sie Daten aus Lake Datenspeicher in Azure-Datenkatalog

In diesem Artikel erfahren Sie, wie Azure Lake Datenspeicher mit Azure Datenkatalog auf Ihre Daten innerhalb einer Organisation auffindbar zu machen, indem Sie Datenkatalog integrieren integriert werden soll. Weitere Informationen zum Katalogisierung von Daten finden Sie unter [Azure Datenkatalog](../data-catalog/data-catalog-what-is-data-catalog.md). Um Szenarien zu verstehen, in denen Sie Datenkatalog verwenden können, finden Sie unter [allgemeine Szenarien Azure Datenkatalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Aktivieren Sie Ihr Abonnement Azure** für Daten dem Store Public Preview. [Anweisungen](data-lake-store-get-started-portal.md#signup)finden Sie unter.

- **Azure dem Datenspeicher-Konto**. Folgen Sie den Anweisungen bei [den ersten Schritten mit Azure dem Datenspeicher mithilfe der Azure-Portal](data-lake-store-get-started-portal.md)an. In diesem Lernprogramm erstellen Sie ein Konto Lake Datenspeicher für **Datacatalogstore**lassen Sie uns. 

    Nachdem Sie das Konto erstellt haben, Hochladen einer Stichprobe Datenmenge darauf. In diesem Lernprogramm hochladen Sie lassen Sie uns die CSV-Dateien unter dem Ordner **AmbulanceData** im [Azure Daten dem Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/)ein. Verschiedene Clients, wie z. B. [Azure-Speicher-Explorer](http://storageexplorer.com/), können Sie die Daten zu einem Container Blob hochladen.

- **Katalog Azure-Daten**. Ihre Organisation muss bereits eine Azure Datenkatalog für Ihre Organisation erstellte verfügen. Jeweils nur ein Katalog ist für jede Organisation zulässig.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register Lake Datenspeicher als Quelle für Datenkatalog

>[AZURE.VIDEO adcwithadl] 

1. Wechseln Sie zu `https://azure.microsoft.com/services/data-catalog`, und klicken Sie auf **Erste Schritte**.

2. Melden Sie sich bei dem Datenkatalog Azure-Portal, und klicken Sie auf **Daten veröffentlichen**.

    ![Registrieren eine Datenquelle] (./media/data-lake-store-with-data-catalog/register-data-source.png "Registrieren eine Datenquelle")

3. Klicken Sie auf der nächsten Seite auf **Anwendung zu starten**. Dadurch wird die Anwendungsmanifestdatei auf Ihrem Computer herunterladen. Doppelklicken Sie auf die Manifestdatei, um die Anwendung zu starten.

4. Klicken Sie auf der Seite Willkommen klicken Sie auf **Anmelden**, und geben Sie Ihre Anmeldeinformationen.

    ![Willkommenseite] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Willkommenseite")

5. Klicken Sie auf der Seite Datenquelle auswählen **Azure Daten dem**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Wählen Sie aus der Datenquelle] (./media/data-lake-store-with-data-catalog/select-source.png "Wählen Sie aus der Datenquelle")

6. Geben Sie den Namen der Lake Datenspeicher-Konto, den Sie in Datenkatalog registrieren möchten, klicken Sie auf der nächsten Seite. Lassen Sie die anderen Optionen als Standard, und klicken Sie dann auf **Verbinden**.

    ![Verbinden mit Datenquelle] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Verbinden mit Datenquelle")

7. Die nächste Seite kann in den folgenden Segmente unterteilt werden.

    ein. Im Feld **Server-Hierarchie** stellt die Ordnerstruktur des Sees Datenspeicher-Konto an. **$Root** stellt die Lake Datenspeicher-Konto aus, und **AmbulanceData** steht für den Ordner im Stamm des Kontos Lake Datenspeicher erstellt.

    b. Das Feld **verfügbaren Objekte** Listet die Dateien und Ordner unter dem Ordner **AmbulanceData** .

    c. **Objekte registriert ist das Feld** Listet die Dateien und Ordner, die Sie in Azure Datenkatalog erfassen möchten.

    ![Datenstruktur anzeigen] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Datenstruktur anzeigen")

8. In diesem Lernprogramm verwenden sollten Sie alle Dateien im Verzeichnis registrieren. Klicken Sie für das auf die Schaltfläche (![Verschieben von Objekten](./media/data-lake-store-with-data-catalog/move-objects.png "Verschieben von Objekten")), um alle Dateien in das **Objekte registriert werden** zu verschieben. 

    Da die Daten in einer gesamten Organisation Datenkatalog erfasst werden soll, ist es ein empfohlen Ansatz einige Metadaten hinzufügen, die Sie später verwenden können, um Daten schnell zu suchen. Sie können beispielsweise eine e-Mail-Adresse für den Datenbesitzer (beispielsweise eine, die die Daten hochgeladen wird) hinzufügen oder Hinzufügen einer Kategorie, um die Daten zu identifizieren. Das Bildschirmfoto unten zeigt einer Kategorie, dass wir die Daten hinzufügen.

    ![Datenstruktur anzeigen] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Datenstruktur anzeigen")

    Klicken Sie auf **Registrieren**.

8. Die folgenden Bildschirmaufnahme kennzeichnet, dass die Daten erfolgreich im Katalog Daten registriert ist.

    ![Registrierung abgeschlossen] (./media/data-lake-store-with-data-catalog/registration-complete.png "Datenstruktur anzeigen")

9. Klicken Sie auf **Ansicht Portal** um Datenkatalog-Portal zurück, und stellen Sie sicher, dass Sie jetzt die erfassten Daten über das Portal zugreifen können. Um die Daten zu suchen, können Sie die Kategorie aus, die Sie beim Registrieren der Daten verwendet.

    ![Suchen von Daten im Katalog] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Suchen von Daten im Katalog")

10. Sie können jetzt Operationen wie Hinzufügen von Anmerkungen und Dokumentation zu den Daten. Weitere Informationen finden Sie unter den folgenden Links.
    * [Kommentieren von Datenquellen in Datenkatalog](../data-catalog/data-catalog-how-to-annotate.md)
    * [Dokument von Datenquellen in Datenkatalog](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Siehe auch

* [Kommentieren von Datenquellen in Datenkatalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Dokument von Datenquellen in Datenkatalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Andere Azure Lake Datenspeicher integrieren](data-lake-store-integrate-with-other-services.md)
