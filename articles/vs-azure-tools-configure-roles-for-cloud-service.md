<properties
   pageTitle="Konfigurieren Sie die Rollen für einen Azure-Cloud-Dienst mit Visual Studio | Microsoft Azure"
   description="Informationen Sie zum Einrichten und Konfigurieren von Rollen für Azure-Cloud-Diensten mit Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Konfigurieren Sie die Rollen für einen Azure-Cloud-Dienst mit Visual Studio

Ein Azure-Cloud-Dienst kann Worker oder Webrollen haben. Für jede Rolle müssen Sie definieren, wie die Rolle eingerichtet ist und außerdem konfigurieren, wie die Rolle ausgeführt wird. Weitere Informationen zu Rollen in der Cloud Services finden Sie unter video- [Einführung Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). Die Informationen für Ihre Cloud-Dienst werden in den folgenden Dateien gespeichert:

- **ServiceDefinition.csdef**

    Der Dienst Formulardefinitionsdatei definiert der Laufzeit Einstellungen für Ihre Cloud-Dienst, einschließlich, welche Rollen erforderlich sind, die Endpunkte und die Größe des virtuellen Computers. Keiner der in dieser Datei gespeicherten Daten können geändert werden, wenn Ihre Rolle ausgeführt wird.

- **ServiceConfiguration.cscfg**

    Konfigurationsdatei für den Dienst konfiguriert wie viele Instanzen von einer Rolle ausgeführt werden und die Werte der Einstellungen für eine Rolle definiert. In dieser Datei gespeicherten Daten können geändert werden, während Ihre Rolle ausgeführt wird.

Speichern Sie die verschiedenen Werte für diese Einstellungen für Ihre Rolle wie ausgeführt wird, benutzerspezifisch können mehrere Dienstkonfigurationen sein. Eine andere Dienstkonfiguration können für jede Bereitstellung Umgebung. Beispielsweise können Sie festlegen die Verbindungszeichenfolge für Speicher-Konto lokale Azure Speicheremulator in einer lokalen Dienstkonfiguration verwenden und erstellen eine andere Dienstkonfiguration, um den Azure-Speicher in der Cloud zu verwenden.

Wenn Sie einen neuen Azure-Cloud-Dienst in Visual Studio erstellen, werden zwei Dienstkonfigurationen standardmäßig erstellt. Diese Konfigurationen werden Azure Projekt hinzugefügt. Die Konfigurationen sind benannte:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Konfigurieren eines Azure-Cloud-Diensts

Sie können einen Azure-Cloud-Dienst aus dem Explorer-Lösung in Visual Studio konfigurieren, wie in der folgenden Abbildung gezeigt.

![Konfigurieren der Cloud-Dienst](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>So konfigurieren Sie einen Azure-Cloud-Dienst

1. Um jede Rolle im Projekt Azure aus **Lösung Explorer**zu konfigurieren, öffnen Sie das Kontextmenü für die Rolle im Azure Projekt, und wählen Sie dann auf **Eigenschaften**.

    Eine Seite mit dem Namen der Rolle ist im Visual Studio-Editor angezeigt. Die Seite zeigt die Felder für die Registerkarte **Konfiguration** .

1. Wählen Sie den Namen der Dienstkonfiguration, die Sie bearbeiten möchten, klicken Sie in der Liste **Dienstkonfiguration** .

    Wenn Sie alle der Dienstkonfigurationen für diese Rolle ändern möchten, können Sie **Alle Konfigurationen**auswählen.

    >[AZURE.IMPORTANT] Wenn Sie eine bestimmte Dienstkonfiguration auswählen, werden einige Eigenschaften deaktiviert, weil er nur für alle Konfigurationen festgelegt werden können. Um diese Eigenschaften bearbeiten zu können, müssen Sie alle Konfigurationen auswählen.

    Sie können nun eine Registerkarte alle aktivierten Eigenschaften für die Ansicht aktualisieren auswählen.

## <a name="change-the-number-of-role-instances"></a>Ändern der Anzahl der Rolleninstanzen

Um die Leistung von Ihrem Cloud-Dienst zu verbessern, können Sie die Anzahl der Instanzen von einer Rolle ändern, die ausgeführt werden, basierend auf der Anzahl der Benutzer oder die für eine bestimmte Rolle erwartet laden. Ein virtuellen Computer wird für jede Instanz von einer Rolle erstellt, wenn der Clouddienst in Azure ausgeführt wird. Dies wirkt sich die Rechnung für die Bereitstellung von dieser Clouddienst. Weitere Informationen zur Abrechnung finden Sie unter [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>So ändern Sie die Anzahl der Instanzen für eine Rolle

1. Wählen Sie auf der Registerkarte **Konfiguration** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** die Dienstkonfiguration, die Sie aktualisieren möchten.

    >[AZURE.NOTE] Sie können die Anzahl der Instanzen für eine bestimmte Dienstkonfiguration oder für alle Dienstkonfigurationen festlegen.

1. Geben Sie im Textfeld **Anzahl der Instanzen** die Anzahl der Instanzen, die für diese Rolle beginnen soll.

    >[AZURE.NOTE] Jede Instanz ist auf einer separaten virtuellen Computern ausgeführt werden, wenn Sie Ihre Cloud-Dienst Azure veröffentlichen.

1. Wählen Sie die Schaltfläche **Speichern** , klicken Sie auf der Symbolleiste, um diese Änderungen zur Konfigurationsdatei zu speichern.

## <a name="manage-connection-strings-for-storage-accounts"></a>Verwalten der Verbindungszeichenfolgen für Speicherkonten

Sie können hinzufügen, entfernen oder Verbindungszeichenfolgen für Ihre Dienstkonfigurationen ändern. Sie sollten andere Verbindungszeichenfolgen für unterschiedliche Service Konfigurationen. Angenommen, Sie möchten eine lokale Verbindungszeichenfolge für eine lokale Dienstkonfiguration mit dem Wert der `UseDevelopmentStorage=true`. Möglicherweise möchten Sie auch eine Cloud Dienstkonfiguration konfigurieren, die ein Speicherkonto in Azure verwendet.

>[AZURE.WARNING] Wenn Sie die Informationen des Kontos Azure-Speicher für eine Verbindungszeichenfolge für Speicher-Konto eingeben, werden diese Informationen in der Konfiguration Dienstdatei lokal gespeichert. Allerdings werden diese Informationen aktuell nicht als verschlüsselten Text gespeichert.

Mit einem anderen Wert für jede Dienstkonfiguration, müssen Sie keinen anderen Verbindungszeichenfolgen in der Cloud-Dienst verwenden, oder ändern Sie den Code ein, wenn Sie Ihre Cloud-Dienst Azure veröffentlichen. Können Sie den bestehenden Namen für die Verbindungszeichenfolge im Code und der Wert wird unterschiedlich sein, basierend auf die Dienstkonfiguration, die Sie auswählen, wenn Sie Ihre Cloud-Dienst erstellen, oder wenn Sie ihn um seine Erlaubnis.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Verbindungszeichenfolgen für Speicherkonten verwalten

1. Wählen Sie die Registerkarte **Einstellungen** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** die Dienstkonfiguration, die Sie aktualisieren möchten.

    >[AZURE.NOTE] Sie können die Verbindungszeichenfolgen für eine bestimmte Dienstkonfiguration aktualisieren, aber wenn Sie zum Hinzufügen oder Löschen einer Verbindungszeichenfolge müssen Sie müssen alle Konfigurationen auswählen.

1. Wählen Sie die **Einstellung hinzufügen** -Schaltfläche zum Hinzufügen einer Verbindungszeichenfolge aus. Ein neuer Eintrag wird zur Liste hinzugefügt.

1. Geben Sie in das Textfeld **Name** den Namen, den Sie für die Verbindungszeichenfolge verwenden möchten.

1. Wählen Sie in der Dropdownliste **Typ** **Verbindungszeichenfolge**aus.

1. Wenn Sie den Wert für die Verbindungszeichenfolge ändern möchten, wählen Sie das Auslassungszeichen (...) aus. Das Dialogfeld **Speicher Verbindungszeichenfolge erstellen** wird angezeigt.

1. Um lokale Speicheremulator Konto verwenden möchten, wählen Sie das Optionsfeld **Microsoft Azure Speicheremulator** aus, und wählen Sie dann auf die Schaltfläche **OK** .

1. Um ein Speicherkonto in Azure verwenden zu können, wählen Sie das Optionsfeld **Ihres Abonnements** , und wählen Sie das gewünschte Speicherkonto.

1. Wenn Sie benutzerdefinierte Anmeldeinformationen verwenden möchten, wählen Sie die Optionsschaltfläche **manuell eingegebenen Anmeldeinformationen** aus. Geben Sie den Kontonamen Speicher und die primäre oder zweite-Taste. Informationen zum Erstellen eines Kontos Speicher und so geben Sie die Details für das Speicherkonto im Dialogfeld **Speicher Verbindungszeichenfolge erstellen** finden Sie unter [Vorbereiten veröffentlichen oder eine Azure-Anwendung von Visual Studio bereitstellen](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Um eine Verbindungszeichenfolge zu löschen, wählen Sie die Verbindungszeichenfolge aus, und wählen Sie dann auf die Schaltfläche **Einstellung entfernen** .

1. Wählen Sie das Symbol **Speichern** auf der Symbolleiste auf die betreffenden Änderungen in der Konfiguration Dienstdatei speichern aus.

1. Um die Verbindungszeichenfolge in der Dienstkonfigurationsdatei zuzugreifen, müssen Sie den Wert für die Einstellung Konfiguration erhalten. Der folgende Code zeigt ein Beispiel, in dem Blob-Speicher erstellt wird, und die Daten mithilfe einer Verbindungszeichenfolge hochgeladen `MyConnectionString` aus der Dienstkonfigurationsdatei, wenn ein Benutzer auf der Seite Default.aspx in die Web-Rolle für einen Azure-Cloud-Dienst **Button1** auswählt. Fügen Sie den folgenden Anweisungen Default.aspx.cs verwenden:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Öffnen Sie Default.aspx.cs in der Entwurfsansicht, und fügen Sie eine Schaltfläche aus der Toolbox. Fügen Sie den folgenden Code ein, um die `Button1_Click` Methode. In diesem Code wird `GetConfigurationSettingValue` zum Abrufen des Werts aus der Dienstkonfigurationsdatei für die Verbindungszeichenfolge. Dann ein Blob im Speicherkonto erstellt wurde, auf die verwiesen wird, in der Verbindungszeichenfolge `MyConnectionString` und schließlich die Anwendung Text in den Blob hinzufügt.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Hinzufügen von benutzerdefinierten Einstellungen in Ihrem Azure-Cloud-Dienst verwenden

Benutzerdefinierte Einstellungen in der Dienstkonfigurationsdatei können Sie einen Namen und den Wert für eine Zeichenfolge für eine bestimmte Dienstkonfiguration hinzufügen. Sie können diese Einstellung verwenden, um ein Feature in der Cloud-Dienst konfigurieren, indem lesen den Wert der Einstellung, und verwenden diesen Wert, um die Logik in Ihren Code steuern. Sie können diesen Dienst Konfigurationswerte ändern, ohne neu erstellen Ihrer Service-Paket oder, wenn Ihre Cloud-Dienst ausgeführt wird. Code kann bei einer Änderung der Einstellung für Benachrichtigungen über überprüfen. Siehe [RoleEnvironment.Changing Ereignis](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Sie können hinzufügen, entfernen oder Ändern von benutzerdefinierten Einstellungen für Ihre Dienstkonfigurationen. Sie sollten unterschiedliche Werte für diese Zeichenfolgen für unterschiedliche Service Konfigurationen.

Mit einem anderen Wert für jede Dienstkonfiguration, müssen Sie keinen andere Zeichenfolgen in der Cloud-Dienst verwenden, oder ändern Sie den Code ein, wenn Sie Ihre Cloud-Dienst Azure veröffentlichen. Können Sie den bestehenden Namen für die Zeichenfolge im Code und der Wert wird unterschiedlich sein, basierend auf die Dienstkonfiguration, die Sie auswählen, wenn Sie Ihre Cloud-Dienst erstellen, oder wenn Sie ihn um seine Erlaubnis.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Hinzufügen von benutzerdefinierten Einstellungen in Ihrem Azure-Cloud-Dienst verwenden

1. Wählen Sie die Registerkarte **Einstellungen** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** die Dienstkonfiguration, die Sie aktualisieren möchten.

    >[AZURE.NOTE] Sie können Zeichenfolgen für eine bestimmte Dienstkonfiguration aktualisieren, aber wenn Sie hinzufügen oder Löschen einer Zeichenfolge müssen, müssen Sie **Alle Konfigurationen**auswählen.

1. Um eine Zeichenfolge hinzuzufügen, wählen Sie die Schaltfläche **Einstellung hinzufügen** aus. Ein neuer Eintrag wird zur Liste hinzugefügt.

1. Geben Sie in das Textfeld **Name** den Namen, den Sie für die Zeichenfolge verwenden möchten.

1. Wählen Sie in der Dropdownliste **Typ** **Zeichenfolge**ein.

1. Geben Sie zum Hinzufügen oder ändern Sie den Wert für die Zeichenfolge, in das Textfeld **Wert** den neuen Wert ein.

1. Zum Löschen einer Zeichenfolge wählen Sie die Zeichenfolge aus, und wählen Sie dann auf die Schaltfläche **Einstellung entfernen** .

1. Wählen Sie die Schaltfläche **Speichern** , klicken Sie auf der Symbolleiste, um diese Änderungen zur Konfigurationsdatei zu speichern.

1. Für den Zugriff auf die Zeichenfolge in der Konfiguration Dienstdatei müssen Sie den Wert für die Einstellung Konfiguration erhalten.

    Sie müssen Sie sicherstellen, dass das folgende mit Anweisungen werden bereits hinzugefügt und Default.aspx.cs wie im vorherigen Schritt.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Fügen Sie den folgenden Code ein, um die `Button1_Click` Methode, um diese Zeichenfolge auf die gleiche Weise, dass Sie eine Verbindungszeichenfolge zugreifen. Ihre Code durchführen kann dann einige bestimmte Code basierend auf dem Wert der Zeichenfolge Einstellungen für die Dienstkonfigurationsdatei, die verwendet wird.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Verwalten von lokalen Speicher für jede Instanz der Rolle

Sie können lokalen Dateisystem-Speicherung für jede Instanz von einer Rolle hinzufügen. Sie können hier lokale Daten speichern, die nicht von anderen Rollen zugegriffen werden muss. Hier können keine Daten, die Sie nicht benötigen, in der Tabelle, Blob oder SQL-Datenbank-Speicher speichern gespeichert werden. Beispielsweise können Sie diese lokalen Speicher zum Zwischenspeichern von Daten, die möglicherweise erneut verwendet werden. Diese gespeicherten Daten können von anderen Instanzen von einer Rolle zugegriffen werden. 

Lokaler Speicher Einstellungen gelten für alle Dienstkonfigurationen. Sie können nur hinzufügen, entfernen Sie oder ändern Sie der lokalen Speicher für alle Dienstkonfigurationen.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Zum Verwalten von lokalen Speicher für jede Instanz der Rolle

1. Wählen Sie die Registerkarte **Lokale Speicher** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** **Alle Konfigurationen**aus.

1. Um einen Eintrag Lokaler Speicher hinzufügen, wählen Sie die Schaltfläche **Lokalen Speicher hinzufügen** aus. Ein neuer Eintrag wird zur Liste hinzugefügt.

1. Geben Sie in das Textfeld **Name** den Namen, den Sie für diese lokalen Speicher verwenden möchten.

1. Geben Sie in das Textfeld **Größe** die Größe in MB, die Sie für diese lokalen Speicherplatz benötigen.

1. Um die Daten in dieser lokalen Speicher entfernen, wenn die virtuellen Computern für diese Rolle wiederverwendet wird, wählen Sie das Kontrollkästchen **säubern auf Papierkorb** .

1. Wählen Sie zum Bearbeiten eines vorhandenen Eintrags für lokale Speicher der Zeile, die Sie aktualisieren müssen. Dann können Sie die Felder, bearbeiten, wie in den vorherigen Schritten beschrieben.

1. Um einen Eintrag Lokaler Speicher zu löschen, wählen Sie in der Liste den Eintrag Speicher aus, und wählen Sie dann auf die Schaltfläche **Lokalen Speicher entfernen** .

1. Um diese Änderungen auf den Dienstkonfigurationsdateien speichern möchten, wählen Sie das Symbol **Speichern** auf der Symbolleiste aus.

1. Um den lokalen Speicher zugreifen, den Sie in der Dienstkonfigurationsdatei hinzugefügt haben, müssen Sie den Wert der Einstellung Konfiguration lokale Ressource abrufen. Verwenden Sie die folgenden Codezeilen, um diesen Wert zugreifen und erstellen eine Datei namens **MyStorageTest.txt** und Schreiben Sie eine Textzeile Testdaten in dieser Datei. Sie können diesen Code in Hinzufügen der `Button_Click` Methode, die Sie im vorherigen Verfahren verwendet:

1. Sie müssen Sie sicherstellen, dass das folgende mit Anweisungen Default.aspx.cs hinzugefügt werden:

    ```
    using System.IO;
    using System.Text;
    ```

1. Fügen Sie den folgenden Code ein, um die `Button1_Click` Methode. Dies erstellt die Datei im lokalen Speicher und schreibt Testdaten in dieser Datei.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Optional) Um diese Datei anzuzeigen, die Sie erstellt haben, wenn Sie Ihre Cloud-Dienst lokal ausführen, gehen Sie folgendermaßen vor:

  1. Führen Sie die Web-Rolle, und wählen Sie **Button1** , um sicherzustellen, dass der Code in `Button1_Click` aufgerufen wird.

  1. Klicken Sie im Infobereich öffnen Sie das Kontextmenü für das Azure-Symbol, und wählen Sie **Berechnen Emulator-Benutzeroberfläche anzeigen**. Das Dialogfeld **Azure-Serveremulator umfasst** angezeigt wird.

  1. Wählen Sie die Web-Rolle aus.

  1. Wählen Sie auf der Menüleiste **Tools**, **Öffnen lokalen Speicher**ein. Ein Windows Explorer-Fenster angezeigt wird.

  1. Geben Sie in der Menüleiste **MyStorageTest.txt** in das **Suchtextfeld** ein, und wählen Sie dann die **EINGABETASTE** , um die Suche zu starten.

    Die Datei wird in den Suchergebnissen angezeigt.

  1. Um den Inhalt der Datei anzuzeigen, öffnen Sie das Kontextmenü für die Datei, und wählen Sie **Öffnen**aus.

## <a name="collect-cloud-service-diagnostics"></a>Sammeln der Cloud-Service-Diagnose

Sie können Diagnosedaten für Ihren Azure-Cloud-Dienst sammeln. Diese Daten werden mit einem Speicherkonto hinzugefügt. Sie sollten andere Verbindungszeichenfolgen für unterschiedliche Service Konfigurationen. Angenommen, Sie möchten ein lokaler Speicher-Konto für eine lokale Dienstkonfiguration, die einen Wert von UseDevelopmentStorage hat = wahr. Möglicherweise möchten Sie auch eine Cloud Dienstkonfiguration konfigurieren, die ein Speicherkonto in Azure verwendet. Weitere Informationen zur Azure-Diagnose finden Sie unter Sammeln Protokollierung Daten von Azure-Diagnose verwenden.

>[AZURE.NOTE] Die lokale Dienstkonfiguration ist bereits so konfiguriert, dass die lokale Ressourcen verwenden. Wenn Sie die Konfiguration der Cloud-Dienst verwenden, den Azure-Cloud-Dienst veröffentlichen, wird der Verbindungszeichenfolge zurück, die Sie angeben, wenn Sie veröffentlichen auch für die Diagnose Verbindungszeichenfolge verwendet, wenn Sie eine Verbindungszeichenfolge angegeben haben. Wenn Sie Ihre Cloud-Dienst mit Visual Studio packen, wird die Verbindungszeichenfolge in die Dienstkonfiguration nicht geändert werden.

### <a name="to-collect-cloud-service-diagnostics"></a>Zum Erfassen von Cloud-Service-Diagnose

1. Wählen Sie auf der Registerkarte **Konfiguration** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** die Dienstkonfiguration, die Sie aktualisieren oder **Alle Konfigurationen**auswählen möchten.

    >[AZURE.NOTE] Sie können das Speicherkonto für eine bestimmte Dienstkonfiguration aktualisieren, aber wenn Sie verwenden möchten, aktivieren oder Deaktivieren von Diagnose müssen Sie alle Konfigurationen auswählen.

1. Aktivieren Sie das Kontrollkästchen **Diagnose aktivieren** , um Diagnose zu aktivieren.

1. Wählen Sie das Auslassungszeichen (...), um den Wert für das Speicherkonto ändern.

    Das Dialogfeld **Speicher Verbindungszeichenfolge erstellen** wird angezeigt.

1. Um eine lokale Verbindungszeichenfolge verwenden möchten, wählen Sie Azure-Speicher Emulator Option aus, und wählen Sie dann auf die Schaltfläche **OK** .

1. Um mit Ihrer Azure-Abonnement verknüpft ist ein Speicherkonto verwenden, wählen Sie **Ihr Abonnement** .

1. Wenn ein Speicherkonto für die lokale Verbindungszeichenfolge verwenden möchten, wählen Sie die Option **manuell eingegebenen Anmeldeinformationen** aus.

    Weitere Informationen zum Erstellen eines Kontos Speicher und so geben Sie die Details für das Speicherkonto im Dialogfeld **Speicher Verbindungszeichenfolge erstellen** finden Sie unter [Vorbereiten veröffentlichen oder eine Azure-Anwendung von Visual Studio bereitstellen](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Wählen Sie das Speicherkonto, die, das Sie im Feld **Kontoname**verwenden möchten.

    Wenn Sie manuell eingeben Ihrer Anmeldeinformationen Speicher, kopieren Sie oder geben Sie Ihre Primärschlüssel in **Konto-Taste**. Dieser Schlüssel kann vom [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)kopiert werden. So kopieren Sie diesen Schlüssel, aus der Ansicht **Speicherkonten** im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)wie folgt vor:
    
  1. Wählen Sie das Speicherkonto, das Sie für Ihre Cloud-Dienst verwenden möchten.

  1. Wählen Sie am unteren Rand des Bildschirms Schaltfläche **Zugriffstasten verwalten** . Das Dialogfeld **Zugriffstasten verwalten** wird angezeigt.

  1. Wenn die Zugriffstaste kopieren möchten, wählen Sie die Schaltfläche **in die Zwischenablage kopieren** aus. Sie können jetzt Schlüssel in das Feld **kontoschlüssel** einfügen.

1. Das Speicherkonto verwenden, das Sie, wie der Verbindungszeichenfolge bereitstellen für Diagnose (und Zwischenspeichern) Wenn Sie Ihre Cloud-Dienst Azure veröffentlichen das Kontrollkästchen **Update Entwicklung Speicher Verbindungszeichenfolgen für Diagnose und Zwischenspeichern mit Azure-Speicher Anmeldeinformationen beim Veröffentlichen in Azure Konto** auswählen.

1. Wählen Sie die Schaltfläche **Speichern** , klicken Sie auf der Symbolleiste, um diese Änderungen zur Konfigurationsdatei zu speichern.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Ändern der Größe des virtuellen Computers verwendet für jede Rolle

Sie können die Größe des virtuellen Computers für jede Rolle festlegen. Sie können nur diese Größe für alle Dienstkonfigurationen festlegen. Wenn Sie einen kleineren Schriftgrad ein Computer auswählen, wird weniger CPU-Kerne, Arbeitsspeicher und lokalen Festplatten Speicher zugewiesen. Die zugewiesene Bandbreite ist auch kleiner. Weitere Informationen zu diesen Größen und den zugeordneten Ressourcen finden Sie unter [Größe für Cloud-Dienste](cloud-services/cloud-services-sizes-specs.md).

Die benötigten Ressourcen für jeden virtuellen Computer in Azure wirkt sich auf die Kosten des Ausführens der Cloud-Dienst in Azure. Weitere Informationen zur Abrechnung Azure finden Sie unter [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Ändern die Größe des virtuellen Computers

1. Wählen Sie auf der Registerkarte **Konfiguration** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** **Alle Konfigurationen**aus.

1. Um die Größe des virtuellen Computers für diese Rolle auszuwählen, wählen Sie die geeignete Größe aus der Liste **virtueller Speicher** aus.

1. Wählen Sie die Schaltfläche **Speichern** , klicken Sie auf der Symbolleiste, um diese Änderungen zur Konfigurationsdatei zu speichern.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Verwalten von Endpunkten und Zertifikate für Ihre Rollen

Sie konfigurieren durch das Protokoll, die Port-Nummer angeben und für HTTPS, SSL Zertifikatinformationen Netzwerke Endpunkte. -Versionen vor Juni 2012 unterstützt HTTP, HTTPS und TCP. Die Juni 2012-Version unterstützt die Protokolle und UDP. Sie können keine UDP für von Endpunkten in die Serveremulator verwenden. Sie können nur für interne Endpunkte dieses Protokoll verwenden.

Um die Sicherheit Ihrer Azure-Cloud-Dienst zu verbessern, können Sie Endpunkte erstellen, die das HTTPS-Protokoll verwenden. Wenn Sie einen Cloud-Service, der vom Kunden verwendet wird verfügen, als bestellt, z. B. Sie um sicherzustellen, dass ihre Informationen sicher sind, mithilfe von SSL.

Sie können auch die Endpunkte, die intern verwendet werden kann oder extern, hinzufügen. Externe Endpunkte werden Eingabewerte Endpunkte bezeichnet. Von außen liegenden Tabellenblättern ermöglicht eine andere Access, zeigen Sie auf Benutzer zu Ihrem Clouddienst an. Wenn Sie einen WCF-Dienst verfügen, können Sie einen internen Endpunkt für eine Webrolle zu verwenden, um diesen Dienst zuzugreifen verfügbar machen möchten.

>[AZURE.IMPORTANT] Sie können nur die Endpunkte für alle Dienstkonfigurationen aktualisieren.

Wenn Sie HTTPS Endpunkte hinzufügen, müssen Sie ein SSL-Zertifikat verwenden. Hierfür können Sie Ihre Rolle für alle Dienstkonfigurationen Zertifikate zuordnen und verwenden Sie diese für Ihre Endpunkte.

>[AZURE.IMPORTANT] Diese Zertifikate werden nicht mit Ihrem Dienst verpackt. Sie müssen Ihre Zertifikate separat auf Azure über das [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen.

Alle Management Zertifikate, die Sie Ihrem Dienstkonfigurationen zuordnen anwenden nur, wenn Ihre Cloud-Dienst in Azure ausgeführt wird. Wenn Ihre Cloud-Dienst in der lokalen Entwicklungsumgebung ausgeführt wird, wird ein standard-Zertifikat, das von der Azure-Serveremulator verwaltet wird verwendet.

### <a name="to-add-a-certificate-to-a-role"></a>So fügen ein Zertifikat einer Rolle

1. Wählen Sie die Registerkarte **Zertifikate** aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** **Alle Konfigurationen**aus.

    >[AZURE.NOTE] Zum Hinzufügen oder Entfernen von Zertifikaten, müssen Sie alle Konfigurationen auswählen. Sie können den Namen und den Fingerabdruck für eine bestimmte Dienstkonfiguration aktualisieren, wenn dies erforderlich ist.

1. Wenn Sie ein Zertifikat für diese Rolle hinzufügen möchten, wählen Sie die Schaltfläche **Zertifikat hinzufügen** aus. Ein neuer Eintrag wird zur Liste hinzugefügt.

1. Geben Sie in das Textfeld **Name** den Namen für das Zertifikat ein.

1. Wählen Sie in der Liste **Speicherort** den Speicherort für das Zertifikat, das Sie hinzufügen möchten.

1. Wählen Sie in der Liste **Name speichern** im Store, den Sie verwenden, um das Zertifikat auswählen möchten.

1. Um das Zertifikat hinzuzufügen, wählen Sie das Auslassungszeichen (...) aus. Im Dialogfeld **Windows-Sicherheit** wird angezeigt.

1. Wählen Sie das Zertifikat, das Sie verwenden möchten, verwenden Sie in der Liste aus, und wählen Sie dann auf die Schaltfläche **OK** .

    >[AZURE.NOTE] Wenn Sie ein Zertifikat aus dem Zertifikatspeicher hinzufügen, werden alle zwischen-XT für Zertifikate automatisch an den Einstellungen Konfiguration für Sie hinzugefügt. Zwischen-XT für diese Zertifikate hinzufügen müssen auch in Azure hochgeladen werden, um ordnungsgemäß des Diensts für SSL konfigurieren.

1. Um ein Zertifikat zu löschen, wählen Sie das Zertifikat aus, und wählen Sie dann auf die Schaltfläche **Zertifikat entfernen** .

1. Wählen Sie das Symbol **Speichern** in der Symbolleiste, um diese Änderungen an den Dienstkonfigurationsdateien speichern aus.

### <a name="to-manage-endpoints-for-a-role"></a>Zum Verwalten von Endpunkten für eine Rolle

1. Wählen Sie die **Endpunkte** (Registerkarte) aus.

1. Wählen Sie in der Liste **Dienstkonfiguration** **Alle Konfigurationen**aus.

1. Wenn Sie einen Endpunkt hinzufügen, wählen Sie die Schaltfläche **Endpunkt hinzufügen** aus. Ein neuer Eintrag wird zur Liste hinzugefügt.

1. Geben Sie in das Textfeld **Name** den Namen, den Sie für diesen Endpunkt verwenden möchten.

1. Wählen Sie in der Liste **Typ** den Typ des Endpunkts an, die Sie benötigen.

1. Wählen Sie aus der Liste **Protokoll** das Protokoll für den Endpunkt, den Sie benötigen.

1. Ist von außen liegenden Tabellenblättern, geben Sie in das Textfeld **Öffentlicher Port** den öffentlichen Port verwenden.

1. Geben Sie im Textfeld **"Privat" Port** den privaten Anschluss verwenden.

1. Wenn für der Endpunkt das Https-Protokoll erforderlich ist, wählen Sie in der Liste **Name des Zertifikats SSL** ein Zertifikat verwenden.

    >[AZURE.NOTE] Diese Liste enthält die Zertifikate, die Sie für diese Rolle auf der Registerkarte **Zertifikate** hinzugefügt haben.

1. Wählen Sie die Schaltfläche **Speichern** , klicken Sie auf der Symbolleiste, um diese Änderungen an den Dienstkonfigurationsdateien speichern.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure-Projekten in Visual Studio durch [Konfigurieren eines Projekts Azure](vs-azure-tools-configuring-an-azure-project.md)lesen. Erfahren Sie mehr über das Schema der Cloud-Dienst [Referenzinformationen](https://msdn.microsoft.com/library/azure/dd179398)zu lesen.
