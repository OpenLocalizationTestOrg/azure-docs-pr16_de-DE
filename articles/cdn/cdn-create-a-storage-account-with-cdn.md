<properties
    pageTitle="Integrieren von Speicher-Konto mit CDN | Microsoft Azure"
    description="Erfahren Sie, wie der Azure Content Delivery Network (CDN) zum Übermitteln von hoher Bandbreite Inhalten durch Zwischenspeichern Blobs aus Azure-Speicher verwenden."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Integrieren von Speicher-Konto mit CDN

CDN kann Zwischenspeichern von Inhalten aus Azure-Speicher aktiviert sein. Entwickler vergleichbar eine globale Lösung für die Bereitstellung von Inhalten mit hoher Bandbreite durch Zwischenspeichern Blobs und statischen Inhalt der berechnen Instanzen am physischen Knoten in den Vereinigten Staaten, Europa, Asien, Australien und Südamerika.


## <a name="step-1-create-a-storage-account"></a>Schritt 1: Erstellen eines Speicher-Kontos

Verwenden Sie das folgende Verfahren zum Erstellen eines neuen Kontos von Speicherplatz für ein Abonnement Azure ein. Ein Speicherkonto haben Zugriff auf Dienste Azure-Speicher. Das Speicherkonto darstellt, die höchste Ebene des Namespace für den Zugriff auf die einzelnen Komponenten des Azure-Speicher: BLOB-Dienste, Warteschlange Services und Tabelle Services. Weitere Informationen finden Sie in die [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md).

Um ein Speicherkonto zu erstellen, müssen Sie entweder den Dienst oder gemeinsame Administratoren für das Abonnement verknüpft sein.

> [AZURE.NOTE] Es gibt mehrere Methoden, die Sie zum Erstellen eines Speicher-Kontos, einschließlich der Azure-Portal und Powershell verwenden können.  In diesem Lernprogramm wird das Azure-Portal verwenden.  

**Erstellen eines Kontos Speicherplatz für ein Azure-Abonnement**

1.  Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie in der oberen linken Ecke **neu**aus. Klicken Sie im Dialogfeld **neue** **Daten + Speicher**, klicken Sie auf **Speicher-Konto**.

    Das Blade **Speicher-Konto erstellen** wird angezeigt.

    ![Erstellen von Speicher-Konto][create-new-storage-account]

4. Geben Sie im Feld **Name** einen Unterdomänennamen ein. Dieser Eintrag kann 3-24 Kleinbuchstaben und Zahlen enthalten.

    Dieser Wert wird der Hostname in der URI, der verwendet wird, um Adressen für Ressourcen, die für das Abonnement Blob, Warteschlange oder Tabelle. Um eine Ressource Container im Blob-Dienst zu beheben, verwenden Sie einen URI in folgendem Format ein, wo * &lt;StorageAccountLabel&gt; * bezieht sich auf den Sie in das Feld **Geben Sie eine URL**eingegeben haben:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;Mycontainer&gt; *

    **Wichtige:** Die Bezeichnung URL die Unterdomäne des Speicherkontos URI-Formularen und muss unter alle gehosteten Dienste in Azure eindeutig sein.

    Dieser Wert wird auch als Namen für dieses Speicherkonto im Portal oder verwendet, wenn dieses Konto programmgesteuert Zugriff auf.

5. Lassen Sie die Standardeinstellungen für **Bereitstellungsmodell**, **Konto Art**, **Leistung**und **Replikation**aus. 

6. Wählen Sie das **Abonnement** , die für das Speicher-Konto verwendet werden.

7. Wählen Sie aus, oder erstellen Sie eine **Ressourcengruppe**.  Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Wählen Sie einen Speicherort für Ihr Speicherkonto ein.

8. Klicken Sie auf **Erstellen**. Die Vorgehensweise zum Erstellen des Speicherkontos möglicherweise mehrere Minuten dauern.


## <a name="step-2-create-a-new-cdn-profile"></a>Schritt 2: Erstellen eines neuen CDN-Profils

Ein Profil CDN ist eine Zusammenstellung von CDN Endpunkte.  Jedes Profil enthält einen oder mehrere CDN Endpunkte.  Möglicherweise möchten Sie mehrere Profile zum Organisieren Ihrer Endpunkte CDN von Internet-Domäne, Webanwendung oder anderen Kriterien verwenden.

> [AZURE.TIP] Wenn Sie bereits über ein Profil CDN, die Sie in diesem Lernprogramm verwenden möchten haben, fahren Sie mit [Schritt 3](#step-3-create-a-new-cdn-endpoint)fort.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Schritt 3: Erstellen eines neuen CDN Endpunkts

**So erstellen einen neuen CDN Endpunkt für Ihr Speicherkonto**

1. Navigieren Sie zu Ihrem Profil CDN im [Verwaltungsportal Azure](https://portal.azure.com).  Sie können es auf dem Dashboard im vorherigen Schritt angeheftet haben.  Wenn Sie nicht, Sie finden können, indem Sie auf **Durchsuchen**, und klicken Sie dann **CDN Profile**und auf auf das Profil, das Sie zu Ihrem Endpunkt hinzufügen möchten.

    Das CDN Profil Blade wird angezeigt.

    ![CDN Profil][cdn-profile-settings]

2. Klicken Sie auf die Schaltfläche **Endpunkt hinzufügen** .

    ![Endpunkt-Schaltfläche "hinzufügen"][cdn-new-endpoint-button]

    Das **Hinzufügen von außen liegenden Tabellenblättern** Blade wird angezeigt.

    ![Hinzufügen von Endpunkt blade][cdn-add-endpoint]

3. Geben Sie einen **Namen** für diesen Endpunkt CDN.  Zugriff auf Ihre zwischengespeicherten Ressourcen in der Domäne an diesem Namen verwendet werden `<endpointname>.azureedge.net`.

4. Wählen Sie in der Dropdownliste **Typ Origin** *Speicher*ein.  

5. Wählen Sie in der Dropdownliste den **Ursprung Hostname** Ihrer Speicherkonto ein.

6. Lassen Sie die Standardeinstellungen für **Origin Pfad**, **Origin Host Kopf-**und **Protokoll/ursprünglichen Port**ein.  Sie müssen mindestens ein Protokoll (HTTP oder HTTPS) angeben.

    > [AZURE.NOTE] Bei dieser Konfiguration kann alle Ihre öffentlich sichtbar Container in Ihr Speicherkonto zum Zwischenspeichern von in der CDN.  Wenn Sie den Bereich auf einen einzigen Container beschränken möchten, verwenden Sie **Origin Pfad**.  Hinweis: der Container benötigen, dessen Sichtbarkeit zu öffentlichen festlegen.

7. Klicken Sie auf die Schaltfläche **Hinzufügen** , um den neuen Endpunkt zu erstellen.

8. Nachdem Sie der Endpunkt erstellt wurde, wird es in eine Liste von Endpunkten für das Profil angezeigt. Die Listenansicht zeigt die URL an, mit der zwischengespeicherte Inhalt als auch die Origin-Domäne zugreifen.

    ![CDN Endpunkt][cdn-endpoint-success]

    > [AZURE.NOTE] Der Endpunkt wird nicht sofort zur Verwendung verfügbar sein.  Es kann bis zu 90 Minuten für die Registrierung über das Netzwerk CDN weitergegeben dauern. Benutzer versuchen, verwenden Sie den Domänennamen CDN sofort möglicherweise Statuscode 404 erhalten, bis der Inhalt über das CDN verfügbar ist.


## <a name="step-4-access-cdn-content"></a>Schritt 4: Access CDN Inhalt

Zwischengespeicherte Inhalte auf das CDN zugreifen zu können, verwenden Sie die CDN-URL im Portal bereitgestellt. Die Adresse für einen zwischengespeicherten Blob werden ähnlich wie der folgende aus:

http://<*endPointName angibt*\>.azureedge.net/ <*MyPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Beim Aktivieren CDN Access mit einem Speicherkonto oder Dienst gehostet wird, die alle öffentlich zugänglichen Objekte zum Zwischenspeichern von CDN Kante berechtigt sind. Wenn Sie ein Objekt, die derzeit in der CDN zwischengespeichert ist ändern, wird der neue Inhalt erst zur Verfügung über die CDN das CDN seinen Inhalt aktualisiert, wenn die zwischengespeicherte Inhalte Time-to-live-Periode abläuft.

## <a name="step-5-remove-content-from-the-cdn"></a>Schritt 5: Entfernen des Inhalts aus dem CDN

Wenn Sie ein Objekt in der Azure Content Delivery Network (CDN) im cache nicht mehr verwenden möchten, können Sie eine der folgenden Schritte ausführen:

-   Sie können den Container vornehmen statt Geiz privat. Weitere Informationen finden Sie unter [anonyme Lesezugriff auf Container und Blobs verwalten](../storage/storage-manage-access-to-resources.md) .
-   Sie können deaktivieren oder löschen Sie den CDN Endpunkt Verwaltungsportal verwenden.
-   Sie können Ihre gehosteten Dienst zum nicht mehr Antworten auf Besprechungsanfragen für das Objekt ändern.

Objekt bereits in der CDN zwischengespeichert bleibt Cache, bis die Time-to-live-Periode für das Objekt abläuft oder der Endpunkt gelöscht wird. Wenn die Time-to-live-Periode abläuft, wird das CDN prüfen, ob der Endpunkt CDN noch gültig ist und das Objekt weiterhin anonym zugegriffen werden. Wenn es nicht der Fall ist, wird das Objekt nicht mehr zwischengespeichert werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

-   [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
