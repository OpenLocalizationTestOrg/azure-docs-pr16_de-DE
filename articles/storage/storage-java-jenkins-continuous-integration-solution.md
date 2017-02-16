<properties 
    pageTitle="Azure-Speicher mit einer Jenkins fortlaufende Integration-Lösung mit | Microsoft Azure" 
    description="In diesem Lernprogramm zeigen, wie den Azure Blob-Dienst verwenden, wie ein Repository für eine Jenkins fortlaufende Integration Lösung erstellte Elemente zu erstellen." 
    services="storage" 
    documentationCenter="java" 
    authors="dineshmurthy" 
    manager="jahogg" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Verwenden von Azure-Speicher mit einer Lösung Jenkins fortlaufende Integration

## <a name="overview"></a>(Übersicht)

Die folgende Informationen wird gezeigt, wie Blob-Speicher als Repository von Erstellen von Elementen durch eine Lösung Jenkins fortlaufender Integration (CI) erstellt oder als Quelle für herunterladbaren Dateien verwenden, um in einem Generator-Prozess verwendet werden. Eines der Stelle, an der Sie dies nützlich ist Szenarios wird beim sind in einer agilen Entwicklungsumgebung (mit Java oder anderen Sprachen) schreiben, Builds basierend auf betreiben fortlaufende Integration und benötigen Sie ein Repository für Ihre Elemente erstellen, damit Sie konnte, beispielsweise andere Mitglieder der Organisation, Ihre Kunden, freigeben oder verwalten ein Archiv. Ein weiteres Szenario ist, wenn Ihre eigene Job selbst andere Dateien, z. B. Abhängigkeiten als Teil der Build Eingabemethoden herunterladen erfordert.

In diesem Lernprogramm werden Sie den Azure-Speicher-Plug-In für Jenkins CI von Microsoft zur Verfügung gestellt verwenden.

## <a name="overview-of-jenkins"></a>Übersicht über Jenkins ##

Jenkins ermöglicht fortlaufenden Integration eines Softwareprojekts, da Entwickler einfach die Änderungen Code integriert werden soll, und haben Builds erstellt automatisch und häufig, damit auch zur Steigerung der Produktivität der Entwickler. Builds bilden, und erstellen Elemente können auf verschiedenen Repositorys hochgeladen werden. In diesem Thema wird zur Verwendung von Azure Blob-Speicher als Repository für die Generator-Elemente angezeigt. Es wird auch wie Abhängigkeiten von Azure Blob-Speicher heruntergeladen werden.

Weitere Informationen zu Jenkins kann finden Sie unter [Jenkins entsprechen](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Vorteile der Verwendung des Blob-Diensts ##

Mithilfe des Blob-Diensts hosten Ihre agilen Entwicklung erstellen Elemente bietet folgende Vorteile:

- Hohe Verfügbarkeit der Ihre eigene Elemente und/oder herunterladbaren Abhängigkeiten.
- Wenn Ihre Lösung Jenkins CI Ihre eigene Elemente uploads optimale Leistung.
- Leistung, wenn Ihre Kunden und Partner über Ihre eigene Elemente herunterladen.
- Die Kontrolle über Richtlinien für Benutzer Access, Wahl zwischen anonymer Zugriff, Ablauf-basierten freigegebenen Zugriff Signatur Access, Private usw. zugreifen.

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Sie benötigen Sie zur Verwendung des Blob-Dienstes mit Ihrer Lösung Jenkins CI Folgendes:

- Eine fortlaufende Integration Jenkins-Lösung.

    Wenn Sie aktuell eine Lösung Jenkins CI besitzen, können Sie eine Jenkins CI-Lösung mit dem folgenden Verfahren ausführen:

    1. Laden Sie auf einem Computer Java-fähige jenkins.war von <http://jenkins-ci.org>ein.
    2. Führen Sie über die Befehlszeile, die in den Ordner, die jenkins.war enthält, geöffnet ist ein:

        `java -jar jenkins.war`

    3. Öffnen Sie in Ihrem Browser `http://localhost:8080/`. Dadurch wird das Dashboard Jenkins öffnen, in dem zum Installieren und Konfigurieren der Azure-Speicher-Plug-in verwendet werden soll.

        Während eine normale Jenkins CI Lösung so werden würde als Dienst eingerichtet, der Jenkins War ausgeführt, in der Befehlszeile ausgeführt werden in diesem Lernprogramm ausreichend.

- Ein Azure-Konto. Sie können für ein Azure-Konto bei <http://www.azure.com>registrieren.

- Ein Konto Azure-Speicher. Wenn Sie bereits ein Speicherkonto besitzen, können Sie erstellen, eine mithilfe der Schritte bei [Speicherkonto erstellen](storage-create-storage-account.md#create-a-storage-account).

- Vertrautheit mit der Lösung Jenkins CI wird empfohlen, jedoch nicht erforderlich, wie der folgende Inhalt ein einfaches Beispiel verwendet wird, um Ihnen zu zeigen, dass die einzelnen Schritte bei Verwendung des Blob-Diensts als Repository für Jenkins CI Elemente erstellen.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Verwenden Sie den Dienst Blob mit Jenkins CI ##

Um den Blob-Dienst mit Jenkins verwenden zu können, müssen Sie den Azure-Speicher-Plug-in installieren, konfigurieren die-Plug-in, um Ihr Speicherkonto verwenden und erstellen Sie dann eine Aktion nach dem Erstellen, die Ihre eigene Elemente mit Ihrem Speicherkonto hochlädt. Diese Schritte werden in den folgenden Abschnitten beschrieben.

## <a name="how-to-install-the-azure-storage-plugin"></a>So installieren Sie das Plug-in Azure-Speicher ##

1. Klicken Sie in dem Dashboard Jenkins auf **Jenkins verwalten**.
2. Klicken Sie auf der Seite **Jenkins verwalten** auf **Verwalten-Plug-Ins**.
3. Klicken Sie auf der Registerkarte **verfügbar** .
4. Aktivieren Sie im Abschnitt **Element Uploads** **Microsoft Azure-Speicher-Plug-in**.
5. Klicken Sie auf **Installieren ohne Neustart** oder **jetzt herunterladen und installieren Sie nach dem Neustart**.
6. Starten Sie Jenkins neu.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>So konfigurieren Sie das Plug-in Azure-Speicher, um Ihr Speicherkonto verwenden ##

1. Klicken Sie in dem Dashboard Jenkins auf **Jenkins verwalten**.
2. Klicken Sie auf der Seite **Jenkins verwalten** auf **System konfigurieren**.
3. Im Abschnitt **Microsoft Azure Speicher Kontokonfiguration** :
    1. Geben Sie Ihren speicherkontonamen für ein, die Sie aus dem [Azure-Portal](https://portal.azure.com)abrufen können.
    2. Geben Sie Ihren kontoschlüssel von Speicher, auch aus dem [Azure-Portal](https://portal.azure.com)erhältlich.
    3. Verwenden Sie den Standardwert für **Blob-Service-Endpunkt-URL** , wenn Sie die öffentliche Azure Cloud verwenden. Wenn Sie eine andere Azure Cloud arbeiten, verwenden Sie den Endpunkt gemäß Angabe im [Portal Azure](https://portal.azure.com) für Ihr Speicherkonto. 
    4. Klicken Sie auf **Überprüfen Speicher Anmeldeinformationen** zur Überprüfung Ihres Speicherkontos. 
    5. [Optional] Wenn Sie zusätzlichen Speicher-Konten, die Ihre CI Jenkins zur Verfügung gestellt werden sollen verfügen, klicken Sie auf **Weitere Speicher-Konten hinzufügen**.
    6. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>So erstellen Sie eine Aktion nach dem Erstellen, die Ihre eigene Elemente in Ihrem Speicherkonto hochgeladen werden soll. ##

Aus Gründen der Anweisung zuerst müssen wir beim Erstellen eines Auftrags, das mehrere Dateien erstellen, und klicken Sie dann in der Aktion nach dem Erstellen zum Hochladen von Dateien mit Ihrem Speicherkonto hinzufügen.

1. Klicken Sie auf **Neues Element**, in dem Dashboard Jenkins.
2. Benennen Sie den Auftrag **MyJob**, klicken Sie auf **Erstellen eines Softwareprojekts - Schreibweise**, und klicken Sie dann auf **OK**.
3. Klicken Sie im Abschnitt **Erstellen** der Konfiguration Position klicken Sie auf **Add erstellen Schritt** , und wählen Sie **Windows ausführen Stapel Befehl**.
4. Verwenden Sie **Befehl**, in der folgenden Befehle:

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. Im Abschnitt **Aktionen nach dem Erstellen** der Auftragskonfiguration klicken Sie auf **die nach dem Build Aktion hinzufügen** und wählen Sie **Hochladen zu Azure Blob-Speicher Elemente**aus.
6. Wählen Sie für **Speicher Kontonamen**Speicher-Konto verwenden.
7. Geben Sie für den **Container mit dem Namen**den Containernamen ein. (Der Container wird erstellt, wenn er nicht bereits vorhanden ist, wenn die Generator-Elemente hochgeladen werden.) Sie können Umgebungsvariablen verwenden, also in diesem Beispiel geben Sie **${JOB_NAME}** den Containernamen.

    **Tipp**
    
    Unter dem **Befehl** ist Abschnitt, bei denen Sie ein Skript für **Windows ausführen Stapel Befehl** eingegeben, einen Link zu der Umgebungsvariablen vom Jenkins erkannt. Klicken Sie auf diesen Link, um die Umgebung Variablennamen und Beschreibungen Weitere. Beachten Sie, dass Umgebungsvariablen, die Sonderzeichen, wie etwa die **BUILD_URL** Umgebung-Variable enthalten als Containername oder allgemeine virtuellen Pfad nicht zulässig sind.

8. Klicken Sie in diesem Beispiel auf **neuen Container standardmäßig veröffentlichen** . (Wenn Sie einen privaten Container verwenden möchten, müssen Sie zum Erstellen einer freigegebenen Access-Signatur, der Zugriff gewährt. Dies ist nicht zum Gegenstand dieses Themas. Sie können weitere Informationen zur gemeinsamen Zugriff Signaturen bei [Verwendung von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Optional] Klicken Sie auf **Aufräumen Container vor dem Hochladen** , wenn den Container des Inhaltsverzeichnisses gelöscht werden sollen, bevor Elemente erstellen hochgeladen werden soll (lassen sie deaktiviert, wenn Sie nicht der Inhalt des Containers bereinigen möchten).
10. Geben Sie die **Liste der Elemente hochladen**, * *Text /*txt**.
11. Geben Sie **Allgemeine virtuelle Pfad für die hochgeladene Elemente**, für diese praktische Einführung **${Erstellen\_ID} / ${erstellen\_Zahl}**.
12. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern.
13. Klicken Sie auf dem Dashboard Jenkins auf **Jetzt erstellen** um **MyJob**auszuführen. Untersuchen der Ausgabe Console Antrag auf. Statusnachrichten zur Azure-Speicher werden in der Ausgabe Console berücksichtigt beim Start der Aktion nach dem Erstellen von Build Elemente hochladen.
14. Nach dem erfolgreichen Abschluss des Projekts können Sie die Elemente erstellen untersuchen, durch Öffnen des öffentlichen BLOBs.
    1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com).
    2. Klicken Sie auf **Speichern**.
    3. Klicken Sie auf den Namen des Speicher-Kontos an, den Sie für Jenkins verwendet.
    4. Klicken Sie auf **Container**.
    5. Klicken Sie auf den Container mit dem Namen **Myjob**, welche ist die Kleinbuchstaben Version des Namens Auftrag, die Ihnen zugewiesen werden, wenn Sie den Auftrag Jenkins erstellt haben. Container und Blob-Namen sind (Groß- und Kleinbuchstaben Groß-/Kleinschreibung beachtet) in Azure-Speicher. In der Liste der Blobs für den Container mit dem Namen **Myjob** sollte **hello.txt** und **date.txt**angezeigt werden. Kopieren Sie die URL für eine der folgenden Elemente, und öffnen Sie sie in Ihrem Browser. Sie sehen die Textdatei, die hochgeladen wurde als ein Element erstellen.

Pro Projekt kann nur eine Aktion nach dem Erstellen, die Elemente auf Azure Blob-Speicher hochlädt erstellt werden. Beachten Sie, dass die einzelne nach dem Build Aktion Elemente in Azure Blob-Speicher hochladen anderen Dateien (einschließlich Platzhaltern) und Pfade für Dateien in der **Liste der hochzuladenden Elemente** mit einem Semikolon als Trennzeichen angeben kann. Beispielsweise wenn Ihre eigene Jenkins JAR-Dateien und TXT-Dateien in den Arbeitsbereich **Erstellen** Ordner erzeugt und beide zu Azure Blob-Speicher hochgeladen werden soll, verwenden Sie die folgenden für den Wert für die **Liste der Elemente, um Sie hochzuladen** : **Erstellen /\*.jar; erstellen /\*txt**. Doppelter Doppelpunkt Syntax können Sie auch einen Pfad zur Verwendung in den Blob-Namen angeben. Beispielsweise wenn Sie die Gläser hochgeladen Abrufen mit **Binärdateien** in den Pfad Blob und die TXT-Dateien mit **Hinweise** in den Pfad Blob hochgeladen erhalten möchten, verwenden die folgenden für den Wert für die **Liste der Elemente, um Sie hochzuladen** : **Erstellen /\*. jar::binaries; erstellen /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>So erstellen Sie einen Erstellungsschritt, der downloads von Azure Blob-Speicher ##

Die folgenden Schritte zeigen, wie so konfigurieren Sie einen Erstellungsschritt, um Elemente aus Azure Blob-Speicher heruntergeladen wird. Dies ist sinnvoll, wenn Sie Elemente in Ihren Build aufnehmen möchten, beispielsweise Gläser, die Sie in Azure behalten BLOB-Speicher.

1. Klicken Sie im Abschnitt **Erstellen** der Konfiguration Position klicken Sie auf **Add erstellen Schritt** , und wählen Sie **Herunterladen aus Azure Blob-Speicher**.
2. Wählen Sie für **Speicher Kontonamen**Speicher-Konto verwenden.
3. Geben Sie für den **Container mit dem Namen**den Namen des Containers, die die Blobs enthält, die Sie herunterladen möchten. Sie können die Umgebungsvariablen verwenden.
4. Geben Sie für **Blob-Namen**den Blob-Namen ein. Sie können die Umgebungsvariablen verwenden. Darüber hinaus können Sie ein Sternchen, die als Platzhalter, nachdem Sie die ersten Buchstaben des Namens Blob angeben. Zum Beispiel **Project\* ** Geben Sie alle Blobs, deren Namen mit **Project**starten, möchten.
5. [Optional] Geben Sie den Pfad für den **Pfad herunterladen**möchten auf dem Jenkins Computer, in die Dateien aus Azure Blob-Speicher heruntergeladen werden soll. Umgebungsvariablen können auch verwendet werden. (Wenn Sie keinen Wert für das **Herunterladen der Pfad**bereitstellt, werden die Dateien aus Azure Blob-Speicher in den Auftrag des Arbeitsbereichs heruntergeladen.)

Wenn Sie weitere Elemente, die aus Azure Blob-Speicher heruntergeladen werden sollen verfügen, können Sie zusätzliche Schritte erstellen.

Nachdem Sie einen Build ausführen, können Sie die Ausgabe der Build Verlauf Console überprüfen, oder schauen Sie sich Ihrem Downloadspeicherort, um festzustellen, ob die Blobs, die von Ihnen erwarteten erfolgreich heruntergeladen wurden.  

## <a name="components-used-by-the-blob-service"></a>Komponenten, die vom Blob-Dienst verwendet werden. ##

Die folgenden bietet einen Überblick über die Komponenten des Blob.

- **Speicher-Konto**: alle Zugriff auf Azure-Speicher erfolgt über ein Speicherkonto. Dies ist der höchsten Ebene des Namespace für den Zugriff auf Blobs. Ein Konto kann eine unbegrenzte Anzahl von Containern, enthalten, solange die Gesamtgröße unter 100 TB ist.
- **Container**: ein Containers bietet eine Gruppierung einer Reihe von Blobs. Alle Blobs muss sich in einem Container. Ein Konto kann eine unbegrenzte Anzahl von Container enthalten. Ein Containers kann eine unbegrenzte Anzahl von Blobs speichern.
- **BLOB**: eine Datei mit einem beliebigen Typ und Größe. Es gibt zwei Arten von Blobs, die in Azure Storage gespeichert werden können: blockieren und Seite Blobs. Die meisten Dateien werden Blobs blockieren. Ein einzelnes blockieren Blob kann bis zu 200 GB groß sein. In diesem Lernprogramm verwendet Blobs blockieren. Seitenblobs, ein anderes BLOB-Typ können bis zu 1TB in Größe und effizienter sind sein, wenn Bereiche von Bytes in einer Datei häufig geändert werden. Weitere Informationen zu Blobs finden Sie unter [Grundlegendes zu blockieren Blobs, Anfügen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-Format**: Blobs in folgendem Format ein URL adressiert sind:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (Das oben angegebenen Format gilt für die öffentliche Azure Cloud. Wenn Sie eine andere Azure Cloud arbeiten, verwenden Sie den Endpunkt innerhalb des [Azure-Portal](https://portal.azure.com) anhand den URL-Endpunkt.)

    Im Format oben `storageaccount` steht für den Namen Ihres Kontos Speicher `container_name` steht für den Namen des Containers, und `blob_name` den Namen Ihrer Blob, Hilfethemas darstellt. In den Containername, können Sie mehrere Wege, getrennt durch einen Schrägstrich, haben **/**. Der Container Beispielname in diesem Lernprogramm wurde **MyJob**, und **${Erstellen\_ID} / ${erstellen\_Zahl}** wurde für die gemeinsame virtuellen Pfad, resultierender im Blob Probleme einer URL im folgenden Format verwendet:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Nächste Schritte

- [Jenkins entsprechen](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure SDK für Java-Speicher](https://github.com/azure/azure-storage-java)
- [Azure-Speicher Client SDK-Referenz](http://dl.windowsazure.com/storage/javadoc/)
- [Azure-Speicherservices REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Weitere Informationen finden Sie auch im [Java Developer Center](https://azure.microsoft.com/develop/java/)aus.