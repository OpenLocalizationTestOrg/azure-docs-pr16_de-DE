<properties
    pageTitle="So erstellen, verwalten oder löschen ein Speicherkonto im Portal Azure | Microsoft Azure"
    description="Erstellen eines neuen Kontos mit Speicher, verwalten Sie Ihr Konto Zugriffstasten oder löschen Sie ein Speicherkonto Azure-Portal. Informationen Sie zu Standard- und Premium Speicherkonten."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Informationen über Konten Azure-Speicher

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>(Übersicht)

Ein Konto Azure-Speicher bietet einen eindeutigen Namespace zum Speichern und Datenobjekte Azure-Speicher zugreifen. Alle Objekte in einem Speicherkonto werden zusammen als Gruppe berechnet. Standardmäßig ist die Daten in Ihr Konto nur für Sie der Kontobesitzer verfügbar.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Speicher Konto Abrechnung

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Beim Erstellen einer Azure-virtuellen Computern ist ein Speicherkonto für Sie erstellt automatisch in den Speicherort der Bereitstellung, wenn Sie nicht bereits ein Speicherkonto in diesem Speicherort verfügen. So ist es nicht erforderlich, vor dem Erstellen eines Kontos Speicherplatz für Ihre virtuellen Computern Datenträger gehen Sie folgendermaßen vor. Der Kontonamen Speicher basiert auf den Namen des virtuellen Computers. Finden Sie weitere Details der [Azure-virtuellen Computern Dokumentation](https://azure.microsoft.com/documentation/services/virtual-machines/) .

## <a name="storage-account-endpoints"></a>Speicher Konto Endpunkte

Jedes Objekt, das Sie in Azure-Speicher speichern verfügt über eine eindeutige URL-Adresse. Den Namen des Kontos Speicher bildet die Unterdomäne die Adresse. Die Kombination von Unterdomäne und Domäne Namen, die für jeden Dienst spezifisch ist, bildet einen *Endpunkt* für Ihr Speicherkonto.

Beispielsweise wenn Ihr Speicherkonto *Mystorageaccount*benannt wird, sind die Standardendpunkte für Ihr Speicherkonto:

- BLOB-Dienst: http://*Mystorageaccount*. blob.core.windows.net

- Tabelle Dienst: http://*Mystorageaccount*. table.core.windows.net

- Warteschlange Dienst: http://*Mystorageaccount*. queue.core.windows.net

- Datei Dienst: http://*Mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Ein Blob-Speicherkonto macht nur den Blob-Endpunkt verfügbar.

Die URL für den Zugriff auf ein Objekt in einem Speicherkonto wird durch die Position des Objekts in die Speicher-Konto an den Endpunkt anfügen erstellt. Beispielsweise möglicherweise eine Blob-Adresse dieses Format: http://*Mystorageaccount*.blob.core.windows.net/*Mycontainer*/*Myblob*.

Sie können auch einen benutzerdefinierten Domänennamen zur Verwendung mit Ihrem Speicherkonto konfigurieren. Klassische Speicherkonten finden Sie unter [Konfigurieren einer benutzerdefinierten Domäne Namen für Ihre BLOB-Speicher Endpunkt](storage-custom-domain-name.md) Details. Für Speicherkonten Ressourcenmanager diese Funktion wurde nicht noch [Azure-Portal](https://portal.azure.com) hinzugefügt, aber Sie können mit PowerShell konfigurieren. Weitere Informationen finden Sie unter das Cmdlet " [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) ".  

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.

2. Wählen Sie im Menü Hub **neu** -> **Daten + Speicher** -> **Speicher-Konto**.

3. Geben Sie einen Namen für Ihr Speicherkonto aus. Finden Sie unter [Speicher Konto Endpunkte](#storage-account-endpoints) Details wie der Kontonamen Speicher behoben werden Ihre Objekte in Azure-Speicher verwendet wird.

    > [AZURE.NOTE] Speicher Kontonamen müssen zwischen 3 und 24 Zeichen lang sein und darf Zahlen und nur Kleinbuchstaben enthalten.
    >  
    > Ihren Kontonamen Speicher muss innerhalb Azure eindeutig sein. Azure-Portal wird angegeben, wenn Sie den Namen des Speicher-Kontos an, die, den Sie auswählen, die bereits verwendet wird.

4. Geben Sie das Modell zur Bereitstellung von zu verwendenden: **Ressourcenmanager** oder **klassische**. **Ressourcen-Manager** ist das Modell empfohlene Bereitstellung. Weitere Informationen finden Sie unter [Grundlegendes zu Ressourcenmanager und klassischen Bereitstellung](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] BLOB-Speicher-Konten können nur mit dem Modell zur Bereitstellung von Ressourcenmanager erstellt werden.

5. Wählen Sie den Typ des Speicherkontos: **Allgemeine** oder **Blob-Speicher**. **Allgemeine** ist der Standardwert.

    **Allgemeine** ausgewählt wurde, geben Sie dann die Leistung Ebene: **Standard** oder **Premium**. Die Standardeinstellung ist **Standard**. Weitere Informationen zu Standard- und Premium Speicherkonten, finden Sie unter [Einführung in Microsoft Azure-Speicher](storage-introduction.md) und [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](storage-premium-storage.md).

    Wenn **Blob-Speicher** ausgewählt wurde, geben Sie dann auf die Access-Leiste: **Hotspot** oder **aussagekräftige**. Die Standardeinstellung ist **interessant**. Finden Sie unter [Azure BLOB-Speicher: kühlt und Tastaturkürzel Ebenen](storage-blob-storage-tiers.md) für weitere Details.

6. Wählen Sie die Replikationsoption für das Speicherkonto: **LRS**, **GRS**, **RAS-GRS**oder **ZRS**. Die Standardeinstellung ist **RAS-GRS**. Weitere Informationen zu Replikationsoptionen Azure-Speicher finden Sie unter [Replikation Azure-Speicher](storage-redundancy.md).

7. Wählen Sie das Abonnement, in dem das neue Speicherkonto erstellt werden soll.

8. Geben Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md).

9. Wählen Sie die geografische Position für Ihr Speicherkonto aus. Auf welche Dienste zur Verfügung, in welcher Region stehen weitere Informationen finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/#services) .

10. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

## <a name="manage-your-storage-account"></a>Verwalten Sie Ihr Speicherkonto

### <a name="change-your-account-configuration"></a>Ändern Sie die Konfiguration des Kontos

Nachdem Sie Ihr Speicherkonto erstellt haben, können Sie die Konfiguration, wie etwa das Ändern der Replikationsoption für das Konto verwendet ändern oder Ändern der Access-Ebene für ein Konto der Blob-Speicher. Im [Portal Azure](https://portal.azure.com)navigieren Sie zu Ihrem Speicherkonto, klicken Sie auf **Alle Einstellungen** , und klicken Sie dann auf **Konfiguration** , damit die Kontokonfiguration ändern und/oder anzeigen.

> [AZURE.NOTE] Je nach der Leistung Ebene, die Sie beim Erstellen des Speicherkontos ausgewählt haben, einige Replikationsoptionen möglicherweise nicht zur Verfügung.

Die Replikationsoption ändert Ihrer Preise ändern. Weitere Informationen hierzu finden Sie unter [Azure-Speicher Preise](https://azure.microsoft.com/pricing/details/storage/) Seite.

Für BLOB-Speicher-Konten möglicherweise ändern der Access-Ebene für die Änderung zusätzlich zum Ändern der Preise anfallen. Wenden Sie die [BLOB-Speicherkonten - Preisgestaltung und Rechnungsadresse sowie](storage-blob-storage-tiers.md#pricing-and-billing) Weitere Details an.

### <a name="manage-your-storage-access-keys"></a>Verwalten Sie Ihrer Speicher-Tastenkombinationen

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicher-Tastenkombinationen, die für die Authentifizierung verwendet werden, wenn das Speicherkonto zugegriffen werden kann. Azure ermöglicht können, indem Sie zwei Speicher-Tastenkombinationen, die Tasten mit ohne Unterbrechung Ihrer Speicherdienst oder Zugriff auf diesen Dienst neu zu generieren.

> [AZURE.NOTE] Es empfiehlt sich, dass Sie keinen die Zugriffstasten Speicher mit einer anderen Person freigeben. Um Zugriff für Speicherressourcen zuzulassen, ohne dass der Zugriffstasten, können Sie eine *freigegebene Access-Signatur*verwenden. Eine Signatur freigegebenen Access bietet Zugriff auf eine Ressource in Ihr Konto für ein Intervall, das Sie definieren und mit den Berechtigungen, die Sie angeben. Weitere Informationen finden Sie unter [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Anzeigen und Kopieren von Speicher-Tastenkombinationen

Im [Portal Azure](https://portal.azure.com)navigieren Sie zu Ihrem Speicherkonto, klicken Sie auf **Alle Einstellungen** , und klicken Sie dann auf **Tastenkombinationen** zum Anzeigen, kopieren und neu zu Ihrem Konto Zugriffstasten generieren. Das Blade **Zugriffstasten** enthält darüber hinaus vorkonfiguriertes Verbindungszeichenfolgen mit Ihrem primären und sekundären Tasten, die Sie zum Verwenden in Clientanwendungen kopieren können.

#### <a name="regenerate-storage-access-keys"></a>Neu generieren Speicher-Tastenkombinationen

Es empfiehlt sich, dass Sie bei Ihrem Speicherkonto regelmäßig zum Schützen Ihrer Speicher-Verbindungen die Zugriffstasten ändern. Sodass Sie Verbindungen mit dem Speicherkonto verwalten können, mithilfe einer Access-Taste gedrückt, während Sie die anderen Zugriffstaste neu generieren, werden zwei Tastenkombinationen zugewiesen.

> [AZURE.WARNING] Erneutes Generieren der Zugriffstasten beeinflussen Dienste in Azure als auch eigene Anwendungen, die das Speicherkonto abhängig sind. Alle Clients, die Zugriffstaste zu verwenden, um das Speicherkonto zuzugreifen müssen aktualisiert werden, um den neuen Product Key zu verwenden.

**Media-Dienste** – Wenn Sie Media-Dienste verfügen, die Ihr Speicherkonto abhängig sind, müssen Sie erneut synchronisieren die Tastenkombinationen mit Ihrem Dienst Medien, nachdem Sie die Tasten erneut generieren.

**Applikationen** – Wenn Sie Webanwendungen oder Cloud Services, in denen Storage-Konto verwendet haben, Verbindungen verloren, wenn Sie Tasten, erneut generieren, wenn Sie Ihre Schlüssel einsatzbereit.

**Speicher Explorern** – Wenn Sie alle [Speicher Explorer Applications](storage-explorers.md)verwenden, wahrscheinlich müssen Sie den Speicherschlüssel verwendet, die von diesen Anwendungen zu aktualisieren.

So sieht das Verfahren zum Drehen der Zugriffstasten Speicher aus:

1. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrer Anwendungscode auf die sekundäre Zugriffstaste des Speicherkontos verweisen.

2. Die primäre Zugriffstaste für Ihr Speicherkonto neu zu generieren. Klicken Sie auf das Blade **Zugriffstasten** klicken Sie auf **Neu generieren Schlüssel1**, und klicken Sie dann auf **Ja,** um zu bestätigen, dass Sie einen neuen Product Key generieren möchten.

3. Aktualisieren der Verbindungszeichenfolgen im Code in Bezug auf die neue primäre Zugriffstaste an.

4. Generieren die sekundäre Zugriffstaste auf dieselbe Weise erneut.

## <a name="delete-a-storage-account"></a>Löschen eines Kontos Speicher

Um ein Speicherkonto zu entfernen, die Sie nicht mehr verwenden, wechseln Sie zum im [Portal Azure](https://portal.azure.com)-Speicherkonto, und klicken Sie auf **Löschen**. Löschen eines Kontos Speicher löscht das gesamte Konto, einschließlich von alle Daten in das Konto.

> [AZURE.WARNING] Es ist nicht möglich, zum Wiederherstellen eines gelöschten Speicher-Kontos oder zum Abrufen von Inhalten, die sie vor dem Löschen enthalten. Achten Sie darauf, dass Sie nichts sichern, die Sie speichern, bevor Sie das Konto löschen möchten. Dies gilt auch für alle Ressourcen WAHR in das Konto – Nachdem Sie Blob, Tabellen, Warteschlange oder Datei löschen, wird es dauerhaft gelöscht.

Um ein Speicherkonto zu löschen, die mit einer Azure-virtuellen Computern verknüpft ist, müssen Sie zunächst sicherstellen, dass alle Datenträger virtuellen Computern gelöscht wurden. Wenn Sie nicht zuerst Ihre virtuellen Computern Datenträger löschen, klicken Sie dann, wenn Sie versuchen, Ihr Speicherkonto löschen Sie werden eine Fehlermeldung angezeigt, finden Sie unter:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Wenn das Speicherkonto der Bereitstellung Klassisch verwendet, können Sie den Datenträger virtuellen Computern mit folgenden Schritten im [Portal Azure](https://manage.windowsazure.com)entfernen:

1. Navigieren Sie zu der [klassische Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte virtuellen Computern an.
3. Klicken Sie auf der Registerkarte Datenträger.
4. Wählen Sie Ihre Daten Datenträger, und klicken Sie auf Datenträger löschen.
5. Um Datenträgerabbilder zu löschen, navigieren Sie zur Registerkarte Bilder, und löschen Sie alle Bilder, die in dem Konto gespeichert sind.

Weitere Informationen finden Sie in der [Dokumentation Azure-virtuellen Computern](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Nächste Schritte

- [Azure Blob-Speicher: Kalt und Hotspot Ebenen](storage-blob-storage-tiers.md)
- [Azure Speicherreplikation](storage-redundancy.md)
- [Konfigurieren Sie die Verbindungszeichenfolgen Azure-Speicher](storage-configure-connection-string.md)
- [Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)
- Besuchen Sie den [Teamblog Azure-Speicher](http://blogs.msdn.com/b/windowsazurestorage/).
