<properties
    pageTitle="Konfigurieren Sie einen Domänennamen für Ihre Blob-Speicher-Endpunkt | Microsoft Azure"
    description="Erfahren Sie, wie Sie eine eigene benutzerdefinierte Domäne an den Endpunkt des Blob-Speicher für ein Konto Azure-Speicher im klassischen Azure-Portal zuordnen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurieren Sie einen benutzerdefinierten Domänennamen für Ihre Blob-Speicher-Endpunkt

## <a name="overview"></a>(Übersicht)

Sie können eine benutzerdefinierte Domäne für den Zugriff auf BLOB-Daten in Ihr Konto Azure-Speicher konfigurieren. Der Standardendpunkt für Blob-Speicher ist `<storage-account-name>.blob.core.windows.net`. Wenn Sie eine benutzerdefinierte Domäne und Unterdomäne, beispielsweise **www.contoso.com** an den Endpunkt Blob für Ihr Speicherkonto, und klicken Sie dann auf Ihre Benutzer können auch Access BLOB-Daten in Ihrem Speicherkonto mit dieser Domäne zuordnen.

>[AZURE.IMPORTANT] Azure-Speicher unterstützt keine noch HTTPS mit benutzerdefinierten Domänen. Beachten Sie, dass Kunden dieses Feature interessiert sind, und es in zukünftigen Versionen stehen werden.

Es gibt zwei Möglichkeiten, um Ihre benutzerdefinierte Domäne mit den Endpunkt Blob für Ihr Speicherkonto verknüpft. Die einfachste Methode besteht im Erstellen eines Zuordnung Ihrer benutzerdefinierten Domäne und Unterdomäne an den Endpunkt BLOB-CNAME-Eintrags. Ein CNAME-Eintrag ist ein DNS-Feature, das eine Quelldomäne eine Zieldomäne zugeordnet ist. In diesem Fall ist die Quelldomäne Ihrer benutzerdefinierten Domäne und Unterdomäne – Beachten Sie, dass die Unterdomäne immer erforderlich ist. Die Zieldomäne ist der Blob-Endpunkt.

Die Verfahren zum Zuordnen von Ihrer benutzerdefinierten Domäne zu Ihrem Blob-Endpunkt kann, jedoch zu Fehlern im kurzzeitig Ausfall für die Domäne, während Sie die Domäne in der [Klassischen Azure-Portal](https://manage.windowsazure.com)erfassen möchten. Wenn Ihre benutzerdefinierte Domäne aktuell eine Anwendung mit einer Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL), die erfordert, dass keine Ausfälle werden unterstützt, können Sie die Unterdomäne Azure **Asverify** verwenden, um eine mittlere Registrierungsschritt bereitzustellen, sodass die Benutzer auf Ihre Domäne, während die Zuordnung erfolgt DNS zugreifen können sollen.

Die folgende Tabelle zeigt die Stichprobe URLs für den Zugriff auf BLOB-Daten in einem Speicherkonto mit dem Namen **Mystorageaccount**. Die benutzerdefinierte Domäne für den Speicherkonto registriert ist **www.contoso.com**:

Ressourcenart|URL-Formate
---|---
Speicher-Konto|**Standard-URL:** http://mystorageaccount.blob.core.windows.net<p>**Benutzerdefinierte Domänen-URL:** http://www.contoso.com</td>
BLOB|**Standard-URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Benutzerdefinierte Domänen-URL:** http://www.contoso.com/mycontainer/myblob
Container Root|**Standard-URL:** http://mystorageaccount.blob.core.windows.net/myblob oder http://mystorageaccount.blob.core.windows.net/$ Root/Myblob<p>**Benutzerdefinierte Domänen-URL:** http://www.contoso.com/myblob oder http://www.contoso.com/$ Root/Myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Registrieren eines benutzerdefinierten Domänennamens für Ihr Speicherkonto

Verwenden Sie dieses Verfahren zum Registrieren Ihrer benutzerdefinierten Domäne, wenn Sie Fragen zu dem die Domäne kurz für Benutzer nicht verfügbar sein müssen nicht verfügen oder Ihrer benutzerdefinierte Domäne derzeit nicht Anwendung befindet.

Wenn Ihre benutzerdefinierte Domäne aktuell eine Anwendung unterstützt, die alle Ausfallzeiten haben kann, klicken Sie dann verwenden Sie das im <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">Registrieren eines benutzerdefinierten Domänennamens für Ihre Speicher-Konto mithilfe der temporären Asverify Unterdomäne</a>beschriebenen Verfahren.

Um einen benutzerdefinierten Domänennamen konfigurieren zu können, müssen Sie einen neuen CNAME-Eintrag für Ihre domänenregistrierungsstelle erstellen. Der CNAME-Eintrag gibt einen Alias für einen Domänennamen an; In diesem Fall wird die Adresse Ihrer benutzerdefinierten Domäne an den Endpunkt des Blob-Speicher für Ihr Speicherkonto zugeordnet.

Jeder Registrierungsstelle weist eine ähnliche, aber weicht Methode der Angabe eines CNAME-Eintrags, aber das Konzept ist gleich. Beachten Sie, dass viele grundlegende Domäne Registrierung Pakete DNS-Konfiguration, damit Sie Ihre Domäne Registrierung-Paket zu aktualisieren, bevor Sie den CNAME-Eintrag erstellen können müssen möglicherweise weder bietet.

1.  Navigieren Sie in der [Klassischen Azure-Portal](https://manage.windowsazure.com)zur Registerkarte **Speicher** .

2.  Klicken Sie auf der Registerkarte **Speicher** auf den Namen des Speicherkontos für die Sie die benutzerdefinierte Domäne zuordnen möchten.

3.  Klicken Sie auf die Registerkarte **Konfigurieren** .

4.  Klicken Sie am unteren Rand des Bildschirms auf **Manage Domain** , um das Dialogfeld ' **Benutzerdefinierte Domäne verwalten** ' anzuzeigen. In den Text am oberen Rand des Dialogfelds finden Sie Informationen zum Erstellen des CNAME-Eintrags. Ignorieren Sie für dieses Verfahren den Text, der an die Unterdomäne **Asverify** verweist.

5.  Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Sie können dies in einem Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**finden.

6.  Suchen Sie den Abschnitt für die Verwaltung von CNAMEs aus. Sie müssen möglicherweise zu einer Seite Erweiterte Einstellungen zu wechseln, und suchen Sie nach Wörtern **CNAME**, **Alias**oder **Unterdomänen**.

7.  Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Alias Unterdomäne, beispielsweise **"www"** oder **Fotos**. Geben Sie dann einen Hostnamen, also der Blob-Endpunkt, in dem Format **mystorageaccount.blob.core.windows.net** (wobei **Mystorageaccount** der Name Ihres Kontos Speicher ist). Verwenden der Hostname wird in den Text im Dialogfeld **Benutzerdefinierte Domäne verwalten** für Sie bereitgestellt.

8.  Nachdem Sie den CNAME-Eintrag erstellt haben, kehren Sie zu der **Benutzerdefinierten Domäne verwalten** Dialogfeld zurück, und geben Sie den Namen Ihrer benutzerdefinierten Domäne, einschließlich die Unterdomäne, in das Feld **Name der benutzerdefinierten Domäne** . Wenn Ihre Domäne **contoso.com ist** und Ihre Subdomain **"www"**, geben Sie beispielsweise **www.contoso.com**; Wenn Ihre Subdomain **Fotos**ist, geben Sie **photos.contoso.com**ein. Beachten Sie, dass die Unterdomäne erforderlich ist.

9. Klicken Sie auf die Schaltfläche **Registrieren** , um Ihre benutzerdefinierte Domäne registrieren.

    Wenn die Registrierung erfolgreich ist, sehen Sie die Nachricht, die **Ihre benutzerdefinierte Domäne aktiv ist**. Benutzer können jetzt Ansicht BLOB-Daten auf Ihrer benutzerdefinierten Domäne, solange sie über die entsprechenden Berechtigungen verfügen.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Registrieren Sie sich für Ihr Speicherkonto die Unterdomäne temporären Asverify mithilfe eine benutzerdefinierte Domäne

Verwenden Sie dieses Verfahren zum Registrieren Ihrer benutzerdefiniertes Domäne Wenn Ihre benutzerdefinierte Domäne aktuell eine Anwendung mit einer Vereinbarung zum SERVICELEVEL unterstützt, die erfordert, die es sein Störungsfreiheit. Indem Sie einen, die verweist CNAME aus Asverify erstellen. &lt;Unterdomäne&gt;. &lt;Customdomain&gt; zu Asverify. &lt;Storageaccount&gt;. blob.core.windows.net, Sie können Ihre Domäne mit Azure vorab registrieren. Anschließend können Sie einen zweiten CNAME, die von verweist erstellen &lt;Unterdomäne&gt;. &lt;Customdomain&gt; zu &lt;Storageaccount&gt;. blob.core.windows.net, an welcher Stelle Datenverkehr an Ihre benutzerdefinierte Domäne zu Ihrem Blob-Endpunkt geleitet.

Die Unterdomäne Asverify ist eine spezielle Unterdomäne von Azure erkannt. Vorangestellt **Asverify** an Ihre eigenen Unterdomäne gestatten Sie Azure Ihrer benutzerdefinierte Domäne erkennen, ohne den DNS-Eintrag für die Domäne zu ändern. Nachdem Sie den DNS-Eintrag für die Domäne ändern, wird er an den Endpunkt Blob ohne Ausfallzeiten zugeordnet werden.

1.  Navigieren Sie in der [Klassischen Azure-Portal](https://manage.windowsazure.com)zur Registerkarte **Speicher** .

2.  Klicken Sie auf der Registerkarte **Speicher** auf den Namen des Speicherkontos für die Sie die benutzerdefinierte Domäne zuordnen möchten.

3.  Klicken Sie auf die Registerkarte **Konfigurieren** .

4.  Klicken Sie am unteren Rand des Bildschirms auf **Manage Domain** , um das Dialogfeld ' **Benutzerdefinierte Domäne verwalten** ' anzuzeigen. In den Text am oberen Rand des Dialogfelds finden Sie Informationen zum Erstellen des CNAME-Eintrags, der die Unterdomäne **Asverify** verwenden.

5.  Melden Sie sich bei der Website Ihrer DNS-Registrierungsstelle, und wechseln Sie zu der Seite für die Verwaltung von DNS-Einträge. Sie können dies in einem Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**finden.

6.  Suchen Sie den Abschnitt für die Verwaltung von CNAMEs aus. Sie müssen möglicherweise zu einer Seite Erweiterte Einstellungen zu wechseln, und suchen Sie nach Wörtern **CNAME**, **Alias**oder **Unterdomänen**.

7.  Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Alias Unterdomäne, der die Unterdomäne Asverify enthält. Beispielsweise werden die Unterdomäne, die Sie angeben, in dem Format **asverify.www** oder **asverify.photos**. Geben Sie dann einen Hostnamen, also der Blob-Endpunkt, in dem Format **asverify.mystorageaccount.blob.core.windows.net** (wobei **Mystorageaccount** der Name Ihres Kontos Speicher ist). Verwenden der Hostname wird in den Text im Dialogfeld **Benutzerdefinierte Domäne verwalten** für Sie bereitgestellt.

8.  Nachdem Sie den CNAME-Eintrag erstellt haben, kehren Sie zu der **Benutzerdefinierten Domäne verwalten** Dialogfeld zurück, und geben Sie den Namen Ihrer benutzerdefinierten Domäne in das Feld **Name der benutzerdefinierten Domäne** . Wenn Ihre Domäne **contoso.com ist** und Ihre Subdomain **"www"**, geben Sie beispielsweise **www.contoso.com**; Wenn Ihre Subdomain **Fotos**ist, geben Sie **photos.contoso.com**ein. Beachten Sie, dass die Unterdomäne erforderlich ist.

9.  Aktivieren Sie das Kontrollkästchen, die besagt, **Erweitert: Verwenden Sie die Unterdomäne 'Asverify' zu meiner benutzerdefinierten Domäne Desktopclient registrieren**.

10. Klicken Sie auf die Schaltfläche **Registrieren** , um Ihre benutzerdefinierte Domäne Desktopclient registrieren.

    Wenn die Preregistration erfolgreich ist, sehen Sie die Nachricht, die **Ihre benutzerdefinierte Domäne aktiv ist**.

11. Zu diesem Zeitpunkt nach Azure Ihrer benutzerdefinierte Domäne überprüft wurde, aber den Datenverkehr an Ihre Domäne noch nicht bei Ihrem Speicherkonto verschoben. Um den Vorgang abzuschließen, kehren Sie zu Ihrer DNS-Registrierungsstelle Website zurück, und erstellen Sie eine andere CNAME-Eintrag, der Ihre Subdomain Ihrer Blob-Endpunkt zugeordnet ist. Geben Sie beispielsweise die Unterdomäne als **"www"** oder **Fotos**und der Hostname als **mystorageaccount.blob.core.windows.net** (wobei **Mystorageaccount** der Name Ihres Kontos Speicher steht) an. Mit diesem Schritt Abschluss die Registrierung Ihrer benutzerdefinierten Domäne.

12. Schließlich können Sie den CNAME-Eintrag, die Sie erstellt haben, verwenden **Asverify**, wie nur als temporären Schritt nötig war löschen.

Benutzer können jetzt Ansicht BLOB-Daten auf Ihrer benutzerdefinierten Domäne, solange sie über die entsprechenden Berechtigungen verfügen.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Stellen Sie sicher, dass die benutzerdefinierte Domäne Ihre Blob-Endpunkt verweist auf

Um zu überprüfen, dass Ihre benutzerdefinierte Domäne tatsächlich um Ihre Blob-Endpunkt zugeordnet ist, erstellen Sie einen Blob in einen öffentlichen Container in Ihr Speicherkonto aus. Klicken Sie dann in einem Webbrowser mithilfe eines URIS im folgenden Format auf das Blob zugreifen:

-   http://<*subdomain.customdomain*>/<*Mycontainer*>/<*Myblob*>

Den folgenden URI können Sie ein Webformulars über eine benutzerdefinierte **photos.contoso.com** -Unterdomäne zugreifen, die ein Blob in Ihrem **Myforms** Container zugeordnet ist:

-   http://Photos.contoso.com/MyForms/ApplicationForm.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Aufheben der Registrierung von Ihrem Speicherkonto einer benutzerdefinierten Domänennamens

Zum Aufheben der Registrierung einer benutzerdefinierten Domänennamens, gehen Sie folgendermaßen vor: 

1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com). 

2. Klicken Sie im Navigationsbereich auf **Speicher**. 

3. Klicken Sie auf der Seite **Speicher** auf den Namen des Kontos Speicherplatz auf das Dashboard anzuzeigen. 

5. Klicken Sie im Menüband auf **Manage Domain**. 

6. Klicken Sie im Dialogfeld **Benutzerdefinierte Domäne verwalten** auf **Registrierung**. 


## <a name="additional-resources"></a>Zusätzliche Ressourcen

-   [Zum Zuordnen der benutzerdefinierten Domäne zu Endpunkt Content Delivery Network (CDN)](../cdn/cdn-map-content-to-custom-domain.md)
