# <a name="using-cdn-for-azure"></a>Verwenden für Azure CDN

Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung für die Bereitstellung von Inhalten mit hoher Bandbreite durch Zwischenspeichern Blobs und statischen Inhalt der berechnen Instanzen am physischen Knoten in den Vereinigten Staaten, Europa, Asien, Australien und Südamerika. Eine aktuelle Liste der CDN Knoten Speicherorte finden Sie unter [Azure CDN Knoten Speicherorte].

Dieser Vorgang umfasst die folgenden Schritte aus:

* [Schritt 1: Erstellen eines Speicher-Kontos](#Step1)
* [Schritt 2: Erstellen eines neuen CDN Endpunkts für Ihr Speicherkonto](#Step2)
* [Schritt 3: Zugriff auf Ihre Inhalte CDN](#Step3)
* [Schritt 4: Entfernen von Inhalten CDN](#Step4)

Die Vorteile von CDN Azure Daten zwischenspeichern umfassen:

-   Erzielen Sie eine bessere Leistung und Benutzer Endbenutzer, die alles andere als eine Inhaltsquelle, sind und Anwendungen, wo sind viele 'Internet Schleifen' erforderlich, um Inhalte zu laden
-   Verteilte umfangreiche besser erfolgt hohen Auslastung, sagen Sie am Anfang eines Ereignisses wie eine Einführung eines Produkts verarbeitet

Vorhandene CDN Kunden können nun die Azure CDN im [Azure klassischen Portal]verwenden. Das CDN ist ein Feature Add-on Ihrem Abonnement und hat einen eigenen [Abrechnung planen].

<a id="Step1"> </a>
<h2>Schritt 1: Erstellen eines Speicher-Kontos</h2>

Verwenden Sie das folgende Verfahren zum Erstellen eines neuen Kontos von Speicherplatz für ein Abonnement Azure ein. Ein Speicherkonto haben Zugriff auf Dienste Azure-Speicher. Das Speicherkonto darstellt, die höchste Ebene des Namespace für den Zugriff auf die einzelnen Komponenten des Azure-Speicher: BLOB-Dienste, Warteschlange Services und Tabelle Services. Weitere Informationen über die Dienste Azure-Speicher finden Sie unter [Verwenden der Azure-Speicher-Dienste](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Um ein Speicherkonto zu erstellen, müssen Sie entweder den Dienst oder gemeinsame Administratoren für das Abonnement verknüpft sein.

> [AZURE.NOTE] Informationen zu diesem Vorgang mithilfe der Azure Service Management-API finden Sie unter Thema Verweis [Speicher-Konto erstellen](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) .

**Erstellen eines Kontos Speicherplatz für ein Azure-Abonnement**

1.  Melden Sie sich bei der [Azure klassischen Portal]werden soll.
2.  Klicken Sie in der unteren linken Ecke auf **neu**. Klicken Sie im Dialogfeld **neue** wählen Sie **Data Services aus**und dann auf **Speicher**, klicken Sie dann auf **Symbolleiste erstellen**.

    Das Dialogfeld **Speicherkonto erstellen** wird angezeigt.

    ![Erstellen von Speicher-Konto][create-new-storage-account]

4. Geben Sie in das Feld **URL** einen Unterdomänennamen ein. Dieser Eintrag kann von 3-24 Kleinbuchstaben und Zahlen enthalten.

    Dieser Wert wird der Hostname in der URI, der verwendet wird, um Adressen für Ressourcen, die für das Abonnement Blob, Warteschlange oder Tabelle. Um eine Ressource Container im Blob-Dienst zu beheben, verwenden Sie einen URI in folgendem Format ein, wo * &lt;StorageAccountLabel&gt; * bezieht sich auf den Sie in das Feld **Geben Sie eine URL**eingegeben haben:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;Mycontainer&gt; *

    **Wichtige:** Die Bezeichnung URL die Unterdomäne des Speicherkontos URI-Formularen und muss unter alle gehosteten Dienste in Azure eindeutig sein.

    Dieser Wert wird auch als Namen für dieses Speicherkonto im Portal oder verwendet, wenn dieses Konto programmgesteuert Zugriff auf.

5.  Wählen Sie in der Dropdown-Liste **Region/Zugehörigkeit Gruppe** einer Region oder die Zugehörigkeit Gruppe für das Speicherkonto aus. Wählen Sie eine Gruppe Zugehörigkeit anstelle einer Region, wenn Sie Ihre Speicherdienste in derselben Data Center mit anderen Diensten Windows Azure sein, die Sie verwenden möchten. Dies kann die Leistung verbessern und keine Gebühren für Ausgang angefallen sind.  

    **Hinweis:** Zum Erstellen einer Gruppe für die Zugehörigkeit öffnen Sie **für** den Bereich des Verwaltungsportal zu, klicken Sie auf **Gruppen**, und klicken Sie dann auf entweder **Hinzufügen einer Gruppe für die Zugehörigkeit** oder **Hinzufügen**. Sie können auch erstellen und Verwalten von Gruppen die mithilfe der Windows Azure Service Management-API. Weitere Informationen finden Sie unter [Gruppen die Vorgänge].

6. Wählen Sie aus der Dropdownliste **Abonnement** des Abonnements, dem für das Speicher-Konto verwendet werden.
7.  Klicken Sie auf **Speicher-Konto erstellen**. Die Vorgehensweise zum Erstellen des Speicherkontos möglicherweise mehrere Minuten dauern.
8.  Um zu überprüfen, dass das Speicherkonto erfolgreich erstellt wurde, stellen Sie sicher, dass das Konto in den Elementen, für die **Speicherung** aufgelistet, mit dem Status **Online**angezeigt wird.

<a id="Step2"> </a>
<h2>Schritt 2: Erstellen eines neuen CDN Endpunkts für Ihr Speicherkonto</h2>

Beim Aktivieren CDN Access mit einem Speicherkonto oder Dienst gehostet wird, die alle öffentlich zugänglichen Objekte zum Zwischenspeichern von CDN Kante berechtigt sind. Wenn Sie ein Objekt, die derzeit in der CDN zwischengespeichert ist ändern, wird der neue Inhalt erst zur Verfügung über die CDN das CDN seinen Inhalt aktualisiert, wenn die zwischengespeicherte Inhalte Time-to-live-Periode abläuft.

**So erstellen einen neuen CDN Endpunkt für Ihr Speicherkonto**

1. Klicken Sie im [Azure klassischen Portal]im Navigationsbereich auf **CDN**.

2. Klicken Sie im Menüband auf **neu**. Wählen Sie im Dialogfeld **neue** **App-Dienste**, und klicken Sie dann auf **CDN**und dann auf **Schnellen Erstellen**aus.

3. Wählen Sie in der Dropdownliste den **Ursprungsdomäne** das Speicherkonto, die, das Sie im vorherigen Abschnitt aus der Liste Verfügbare Speicher Konten erstellt haben. 

4. Klicken Sie auf die Schaltfläche **Erstellen** , um den neuen Endpunkt zu erstellen.

5. Nachdem Sie der Endpunkt erstellt wurde, wird es in eine Liste von Endpunkten für das Abonnement angezeigt. Die Listenansicht zeigt die URL an, mit der zwischengespeicherte Inhalt als auch die Origin-Domäne zugreifen. 

    Die Domäne Origin ist die Position, von der das CDN Inhalt zwischengespeichert. Die Domäne Origin kann entweder ein Speicherkonto oder einem Cloud-Dienst werden; für dieses Beispiel ist ein Speicherkonto verwendet. Speicher Inhalt wird zwischengespeichert Kante Servern entsprechend entweder eine Cache-Control-Einstellung, die Sie angeben oder die Standardheuristiken des Netzwerks Zwischenspeichern. 


    > [AZURE.NOTE] Die Konfiguration für den Endpunkt erstellt werden nicht sofort verfügbar sein; Es kann bis zu 60 Minuten für die Registrierung über das Netzwerk CDN weitergegeben dauern. Benutzer versuchen, verwenden Sie den Domänennamen CDN sofort möglicherweise Statuscode 400 (Ungültige Anforderung) erhalten, bis der Inhalt über das CDN verfügbar ist.

<a id="Step3"> </a>
<h2>Schritt 3: Access CDN Inhalt</h2> 

Zwischengespeicherte Inhalte auf das CDN zugreifen zu können, verwenden Sie die CDN-URL im Portal bereitgestellt. Die Adresse für einen zwischengespeicherten Blob werden ähnlich wie der folgende aus:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*MyPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Schritt 4: Entfernen des Inhalts aus dem CDN</h2>

Wenn Sie ein Objekt in der Azure Content Delivery Network (CDN) im cache nicht mehr verwenden möchten, können Sie eine der folgenden Schritte ausführen:

-   Für eine Azure Blob können Sie das Blob aus dem öffentlichen Container löschen.
-   Sie können den Container vornehmen statt Geiz privat. Weitere Informationen finden Sie unter [Einschränken des Zugriffs mit Containern und Blobs](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Sie können deaktivieren oder löschen Sie den CDN Endpunkt Verwaltungsportal verwenden.
-   Sie können Ihre gehosteten Dienst zum nicht mehr Antworten auf Besprechungsanfragen für das Objekt ändern.

Objekt bereits in der CDN zwischengespeichert bleibt Cache, bis die Time-to-live-Periode für das Objekt abläuft. Wenn die Time-to-live-Periode abläuft, wird das CDN prüfen, ob der Endpunkt CDN noch gültig ist und das Objekt weiterhin anonym zugegriffen werden. Wenn es nicht der Fall ist, wird das Objekt nicht mehr zwischengespeichert werden.

Die Möglichkeit, Inhalte sofort zu löschen, wird auf Azure-Verwaltungsportal derzeit nicht unterstützt. Wenden Sie sich an [Azure unterstützen](https://azure.microsoft.com/support/options/) , wenn Sie unmittelbar Inhalte löschen müssen. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

-   [So erstellen Sie eine Gruppe für die Zugehörigkeit in Azure]
-   [Wie: Verwalten von Speicherkonten für ein Abonnement Azure]
-   [Informationen zu den Servicemanagement API]
-   [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN Knoten Speicherorte]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure klassischen-portal]: https://manage.windowsazure.com/
  [Abrechnung plan]: /pricing/calculator/?scenario=full
  [So erstellen Sie eine Gruppe für die Zugehörigkeit in Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Informationen zu den Servicemanagement API]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
