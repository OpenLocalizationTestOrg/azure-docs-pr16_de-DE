<properties 
   pageTitle="Erste Schritte mit Lake Datenspeicher | Azure" 
   description="Verwenden des Portals erstellen Sie ein Konto Lake Datenspeicher und einfache Operationen im Datenspeicher Lake" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Erste Schritte mit Azure Lake Datenspeicher mithilfe der Azure-Portal

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Erfahren Sie, wie Sie mithilfe der Azure-Portal erstellen Sie ein Konto Azure Lake Datenspeicher und einfache Operationen beispielsweise wie Erstellen von Ordnern, hoch- und Herunterladen von Datendateien, löschen usw. Ihr Konto. Weitere Informationen zu Lake Datenspeicher finden Sie unter [Übersicht der Azure dem Datenspeicher](data-lake-store-overview.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Erfahren Sie schnell mit dem Videos?

Schauen Sie sich in den folgenden Videos erste Schritte mit Lake Datenspeicher.

* [Erstellen Sie ein Konto Lake Datenspeicher](https://mix.office.com/watch/1k1cycy4l4gen)
* [Verwalten von Daten in Lake Datenspeicher mit dem Daten-Explorer](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen Sie ein Konto Azure Lake Datenspeicher

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie auf **neu**, klicken Sie auf **Daten + Speicher**, und klicken Sie dann auf **Azure dem Datenspeicher**. Lesen Sie die Informationen in das Blade **Azure dem Datenspeicher** , und klicken Sie dann in der unteren linken Ecke des Blades auf **Erstellen** .

3. Geben Sie die Werte in das Blade **Neu dem Datenspeicher** wie auf dem folgenden Bildschirmfoto dargestellt:

    ![Erstellen eines neuen Kontos mit Azure dem Datenspeicher] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Erstellen eines neuen Kontos mit dem Azure-Daten")

    - **Abonnements**. Wählen Sie das Abonnement, unter dem Sie ein neues Lake Datenspeicher Konto erstellen möchten.
    - **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe aus, oder klicken Sie auf **Ressourcengruppe erstellen** , um eine zu erstellen. Eine Ressourcengruppe ist ein Container, der zugehörige Ressourcen für eine Anwendung enthält. Weitere Informationen finden Sie unter [Gruppen von Ressourcen in Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Standort**: Wählen Sie einen Speicherort, in der Sie das Konto Lake Datenspeicher erstellen möchten.

4. Wählen Sie Sie bei Bedarf das Konto Lake Datenspeicher zugreifen kann die Startboard **an Startboard anheften** .

5. Klicken Sie auf **Erstellen**. Wenn Sie das Konto aus, das Startboard anheften möchten, Sie wieder an der Startboard vorgenommen werden, und sehen Sie den Fortschritt Ihrer Lake Datenspeicher Konto bereitgestellt. Nachdem das Konto Lake Datenspeicher bereitgestellt wird, zeigt das Konto Blade nach oben.

6. Erweitern der **Essentials** Dropdown-um anzuzeigen, dass die Informationen zu Ihrem Konto Lake Datenspeicher wie die Ressource gruppieren Sie es ist, einen Teil der Position, usw. Klicken Sie auf das Symbol **Schnellstart** , um Links zu anderen Ressourcen, die im Zusammenhang mit Lake Datenspeicher finden Sie unter.

    ![Ihr Azure Datenspeicher dem Konto] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Ihr Azure Daten dem Konto")

## <a name="a-namecreatefolderacreate-folders-in-azure-data-lake-store-account"></a><a name="createfolder"></a>Erstellen von Ordnern in Azure Lake Datenspeicher-Konto

Erstellen von Ordnern unter Ihrem Konto Lake Datenspeicher zu verwalten und Daten speichern.

1. Öffnen Sie die Lake Datenspeicher Firma, die Sie soeben erstellt haben. Im linken Bereich klicken Sie auf **Durchsuchen**, klicken Sie auf **Dem Datenspeicher**, und klicken Sie dann aus dem Blade Lake Datenspeicher auf den Namen des Kontos, unter dem Ordner erstellen möchten. Wenn Sie das Konto aus, das Startboard angeheftet haben, klicken Sie auf die Kachel "Konto".

2. Klicken Sie in Ihrem Konto Blade Lake Datenspeicher auf **Daten-Explorer**.

    ![Erstellen von Ordnern in dem Datenspeicher-Konto] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Erstellen von Ordnern in dem Datenspeicher-Konto")

3. Klicken Sie auf **Neuer Ordner**in Ihrem Konto Blade Lake Datenspeicher, geben Sie einen Namen für den neuen Ordner, und klicken Sie dann auf **OK**.
    
    ![Erstellen von Ordnern in dem Datenspeicher-Konto] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Erstellen von Ordnern in dem Datenspeicher-Konto")
    
    Der neu erstellte Ordner werden in den **Daten-Explorer** Blade aufgelistet. Sie können verschachtelte Ordner bis zu einer beliebigen Ebene erstellen.

    ![Erstellen von Ordnern in dem Daten-Konto] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Erstellen von Ordnern in dem Daten-Konto")


## <a name="a-nameuploaddataaupload-data-to-azure-data-lake-store-account"></a><a name="uploaddata"></a>Hochladen von Daten in Azure Lake Datenspeicher-Konto

Sie können Ihre Daten mit einer Firma Azure Lake Datenspeicher direkt auf der Stammebene oder in einen Ordner, den Sie innerhalb des Kontos erstellt hochladen. Führen Sie auf dem Bildschirmfoto unter die Schritte aus, um eine Datei in einem Unterordner aus dem Blade **Daten-Explorer** hochladen aus. In dieser Bildschirmaufnahme hochgeladen wird die Datei in einen Unterordner in der Breadcrumb (rot markiert) angezeigt.

Wenn Sie einige Beispieldaten hochladen suchen, können Sie den Ordner **Krankenwagen Daten** aus dem [Azure Daten dem Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)abrufen.

![Hochladen von Daten] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Hochladen von Daten")


## <a name="a-namepropertiesaproperties-and-actions-available-on-the-stored-data"></a><a name="properties"></a>Eigenschaften und Aktionen auf die gespeicherten Daten zur Verfügung

Klicken Sie auf die neu hinzugefügte Datei, um das Blade **Eigenschaften** zu öffnen. Eigenschaften der Datei, und die Aktionen, die Sie an der Datei ausführen können zugeordnet sind in dieser Blade verfügbar. Sie können auch den vollständigen Pfad zu der Datei in Ihr Konto Azure Lake Datenspeicher im Feld rote auf dem folgenden Bildschirmfoto hervorgehoben kopieren.

![Eigenschaften auf der Registerkarte Daten] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Eigenschaften auf der Registerkarte Daten")

* Klicken Sie auf **Vorschau** , um eine Vorschau der Datei, direkt über den Browser anzuzeigen. Sie können das Format der Vorschau auch angeben. Klicken Sie auf **Vorschau**, klicken Sie in das Blade **Dateivorschau** auf **Format** , und geben Sie in der **Vorschau Dateiformat** Blade die Optionen wie die Anzahl der Zeilen angezeigt werden, Codierung mit Trennzeichen verwenden, usw..

  ![Vorschau-Dateiformat] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Vorschau-Dateiformat")

* Klicken Sie auf **herunterladen** , um die Datei auf Ihren Computer herunterzuladen.

* Klicken Sie auf **Datei umbenennen** zum Umbenennen der Datei.

* Klicken Sie auf **Datei löschen** , um die Datei zu löschen.


## <a name="secure-your-data"></a>Sichern Sie Ihre Daten

Sie können die Daten aus Ihrem Azure Lake Datenspeicher-Konto mithilfe von Azure Active Directory und Steuerung des Benutzerzugriffs (ACLs) sichern. Informationen dazu, wie das geht finden Sie unter [Sichern von Daten in Azure dem Datenspeicher](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Löschen Azure Lake Datenspeicher-Konto

Wenn ein Konto Azure Lake Datenspeicher aus Ihrer Blade Lake Datenspeicher löschen möchten, klicken Sie auf **Löschen**. Um den Vorgang zu bestätigen, werden Sie aufgefordert, den Namen des Kontos eingeben, die Sie löschen möchten. Geben Sie den Namen des Kontos, und klicken Sie dann auf **Löschen**.

![Löschen Daten dem Konto] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Löschen Daten dem Konto")


## <a name="next-steps"></a>Nächste Schritte

- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Access-Diagnoseprotokolle für Lake Datenspeicher](data-lake-store-diagnostic-logs.md)
