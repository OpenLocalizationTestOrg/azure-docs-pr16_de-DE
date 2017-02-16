<properties
    pageTitle="Liste der Azure-Speicher-Konto"
    description="Verwalten Sie Ihre Speicher-kontoeinstellungen mit dem Azure-Toolkit für "Ellipse""
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Liste der Azure-Speicher-Konto #

Azure-Speicherkonten aktivieren Speicherorte für den Download für Ihre JDK, Anwendungsserver und beliebige Komponenten und zum Speichern von Bundesstaat bei Verwendung von Zwischenspeichern verwendet werden. "Ellipse" unterhält eine Liste der bekannten Speicherkonten, die für Ihre Projekte in dem Arbeitsbereich "Ellipse" verfügbar sind. Wenn Sie im Dialogfeld **Konten Speicher** öffnen, in dem die Liste innerhalb einer Ellipse, Verwaltung verwendet wird, klicken Sie auf **Fenster**, klicken Sie auf **Einstellungen**, erweitern Sie **Azure**und dann auf **Speicher-Konten**.

Die nachstehende Abbildung zeigt das Dialogfeld **Speicherkonten** aus.

![][ic719496]

In diesem Dialogfeld kann auch über einen Link **Konten** in Dialogfeldern geöffnet werden, mit denen Storage-Konten, beispielsweise die folgende:

* Die Registerkarte **JDK** des Dialogfelds **Server-Konfiguration** .
* Die Registerkarte **Server** des Dialogfelds **Server-Konfiguration** .
* Das Dialogfeld **Komponente hinzufügen** .
* Eigenschaftendialogfeld **Zwischenspeichern** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>So importieren Sie Ihre Speicherkonten mit einer Einstellungsdatei veröffentlichen ##

1. Klicken Sie innerhalb des Dialogfelds **Konten Speicher** auf **aus veröffentlichen - Einstellungsdatei importieren**.
2. (Überspringen Sie diesen Schritt, wenn Sie bereits eine Einstellungsdatei veröffentlichen auf Ihrem Computer gespeichert haben.) Klicken Sie im Dialogfeld **Importieren Abonnementinformationen** auf **Veröffentlichen-Einstellungsdatei herunterladen**. Wenn Sie noch nicht bei Ihrem Azure-Konto angemeldet sind, werden Sie aufgefordert werden, sich anmelden. Klicken Sie dann werden Sie aufgefordert, das Speichern einer Azure-Einstellungsdatei veröffentlichen. (Können Sie die resultierenden Anweisungen angezeigt wird, klicken Sie auf den Seiten Anmeldung - ignorieren sie werden von der Azure-Portal bereitgestellt und für Visual Studio-Benutzer vorgesehen sind.) Speichern Sie es auf Ihrem Computer aus.
3. Klicken Sie im Dialogfeld **Importieren Abonnementinformationen** klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie die Einstellungsdatei veröffentlichen, die Sie zuvor lokal gespeichert, und klicken Sie dann auf **Öffnen**.
4. Klicken Sie auf **OK** , um das Dialogfeld **Importieren Abonnementinformationen** zu schließen.

## <a name="to-create-a-new-storage-account"></a>Zum Erstellen eines neuen Kontos mit Speicher ##

1. Klicken Sie in das Dialogfeld **Speicher-Konten** auf **Hinzufügen**.
2. Klicken Sie in dem Dialogfeld **Speicher-Konto hinzufügen** auf **neu**.
3. Geben Sie innerhalb des Dialogfelds **Konten Speicher** Werte für Folgendes ein:
    * Speicher Kontonamen ein.
    * Position des Speicherkontos.
    * Beschreibung des Speicherkontos.
    * Das Abonnement, zu dem das Speicherkonto gehört.
4. Klicken Sie auf **OK** , um das Dialogfeld **Neues Speicher-Konto** zu schließen.

Für Ihr Speicherkonto erstellt werden einige Minuten dauern. Klicken Sie auf **OK** , um das Dialogfeld **Speicher-Konto hinzufügen** zu schließen, nachdem sie erstellt wurde, und Ihr neues Speicherkonto wird die Liste der verfügbaren Speicherplatz Konten hinzugefügt werden.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Hinzufügen ein vorhandenen Speicher-Kontos zur Liste ##

1. Wenn Sie noch nicht über ein Konto Azure-Speicher verfügen, erstellen Sie anhand der Schritte in der **zum Erstellen eines neuen Abschnitts des Speicher-Konto** über aufgeführt. (Alternativ können Sie ein neues Speicherkonto bei der [Azure-Verwaltungsportal][]erstellen.)
2. Klicken Sie in das Dialogfeld **Speicher-Konten** auf **Hinzufügen**.
3. Geben Sie Werte für die **Namen** und **Zugriffstaste**, innerhalb des Dialogfelds **Speicher-Konto hinzufügen** . Kontoschlüssel Name und Zugriff muss für ein vorhandenes Konto der Azure-Speicher. Verwenden Sie im Abschnitt **Speicher** des [Azure-Verwaltungsportal][] , um Ihren Kontonamen Speicher und die Tasten anzuzeigen. Ihre **Speicher-Konto hinzufügen** Dialogfeld sieht ähnlich wie der folgende aus.

    ![][ic719497]

4. Klicken Sie auf **OK** , um das Dialogfeld **Speicher-Konto hinzufügen** zu schließen.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>So ändern Sie ein Speicherkonto, um einen neuen Access-Product Key zu verwenden. ##

1. Klicken Sie innerhalb des Dialogfelds **Konten Speicher** auf das Speicherkonto, das Sie bearbeiten, und klicken Sie dann auf **Bearbeiten**möchten.
2. Bearbeiten Sie in das Dialogfeld **Bearbeiten Speicher Konto Zugriffstaste** den **Zugriffstaste** Wert ein.
3. Klicken Sie auf **OK** , um das Dialogfeld **Speicher Konto Zugriffstaste bearbeiten** zu schließen.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>So entfernen Sie ein Speicherkonto aus der Liste in "Ellipse" beibehalten ##

1. Klicken Sie innerhalb des Dialogfelds **Konten Speicher** auf das Speicherkonto, das Sie bearbeiten, und klicken Sie dann auf **Entfernen**möchten.
2. Klicken Sie auf **OK** , wenn Sie dazu aufgefordert werden, das Speicherkonto entfernen.

>[AZURE.NOTE] Entfernen des Speicherkontos über das Dialogfeld **Konten Speicher** entfernt nur sie in der Liste der Speicherkonten in "Ellipse" machen. Sie entfernt das Speicherkonto nicht aus Ihrem Abonnement Azure. Darüber hinaus konnte Speicher-Konto erneut in der Liste angezeigt werden, nachdem "Ellipse" lädt die Details Ihres Abonnements erneut.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][] 

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
