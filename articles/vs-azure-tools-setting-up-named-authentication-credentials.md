<properties
   pageTitle="Einrichten von Anmeldeinformationen für die Authentifizierung benannte | Microsoft Azure"
   description="Hier erfahren Sie, wie zu für die Anmeldeinformationen, die von Visual Studio können Besprechungsanfragen in Azure veröffentlichen Sie eine Anwendung in Azure von Visual Studio oder einen vorhandenen Clouddienst überwachen authentifizieren.. "
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

# <a name="setting-up-named-authentication-credentials"></a>Einrichten von benannten Authentifizierungsinformationen

Veröffentlichen Sie eine Anwendung in Azure von Visual Studio oder einen vorhandenen Clouddienst zu überwachen, müssen Sie Anmeldeinformationen bereitstellen, die Visual Studio Besprechungsanfragen in Azure authentifizieren verwenden können. Es gibt mehrere Orte in Visual Studio, in dem Sie sich anmelden können, diese Anmeldeinformationen bereitzustellen. Beispielsweise können Sie vom Server-Explorer, öffnen Sie das Kontextmenü für den Knoten **Azure** und **Verbinden in Azure**auswählen. Bei der Anmeldung, die mit Ihrem Konto Azure Abonnementinformationen steht in Visual Studio, und es ist nicht mehr Sie ausführen müssen.

Azure Tools unterstützt auch eine ältere Möglichkeit zum Bereitstellen von Anmeldeinformationen, die (publishsettings-Abonnementdatei) verwenden. In diesem Thema werden diese Methode, die immer noch Azure SDK 2.2 unterstützt wird.

Die folgenden Elemente sind für Azure-Authentifizierung erforderlich.

- Ihr Abonnement-ID

- Ein gültiges x. 509 v3 Zertifikat

>[AZURE.NOTE] Das x. 509 v3 Zertifikat Schlüssel muss mindestens 2048 Bits bestehen. Azure lehnt alle Zertifikat diese Anforderung nicht entsprechen, oder ist, die ungültig.

Visual Studio werden Ihre Abonnement-ID zusammen mit den Zertifikatsdaten als Anmeldeinformationen verwendet. Die entsprechenden Anmeldeinformationen sind in der Abonnementdatei (PUBLISHSETTINGS-Datei) verwiesen die enthält einen öffentlichen Schlüssel für das Zertifikat. Die Abonnementdatei kann Anmeldeinformationen für mehrere Abonnements enthalten.

Sie können die Abonnementinformationen im Dialogfeld **Neu/Edit Abonnement** bearbeiten, wie weiter unten in diesem Thema erläutert.

Wenn Sie ein Zertifikat selbst erstellen möchten, können Sie finden Sie in den Anweisungen in [Erstellen und ein Zertifikat Management für Azure hochladen](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) und laden das Zertifikat manuell [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Diese Anmeldeinformationen, die Visual Studio zum Verwalten Ihrer Cloud Services erfordert nicht dieselben Anmeldeinformationen, die zum Authentifizieren einer Anforderung anhand der Azure-Speicher-Dienste erforderlich sind.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Ändern oder Exportieren von Anmeldeinformationen für die Authentifizierung in Visual Studio

Sie können auch einrichten, ändern oder exportieren Ihre Anmeldeinformationen in das Dialogfeld **Neues Abonnement** , die angezeigt wird, wenn Sie eine der folgenden Aktionen ausführen:

- Öffnen Sie im **Server-Explorer**das Kontextmenü für den Knoten **Azure** , wählen Sie **Abonnements verwalten**, wählen Sie die Registerkarte **Zertifikate** , und wählen Sie die Schaltfläche **neu** oder **Bearbeiten** .

- Wenn Sie einen Azure-Cloud-Dienst aus dem **Azure-Anwendung veröffentlichen** -Assistenten veröffentlichen, wählen Sie in der Liste **Wählen Sie Ihr Abonnement** **Verwalten** aus und klicken Sie dann wählen Sie die Registerkarte Zertifikate aus, und wählen Sie dann auf die Schaltfläche **neu** oder **Bearbeiten** .

Im folgende Verfahren wird davon ausgegangen, dass im Dialogfeld **Neues Abonnement** geöffnet ist.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Einrichten von Anmeldeinformationen für die Authentifizierung in Visual Studio

1. **Wählen Sie ein vorhandenes Zertifikat** für Authentifizierungsliste wählen Sie ein Zertifikat aus.

1. Wählen Sie die Schaltfläche **Kopieren Sie den vollständigen Pfad** ein. Der Pfad für das Zertifikat (CER-Datei) wird in die Zwischenablage kopiert.

    >[AZURE.IMPORTANT] Um Ihre Azure-Anwendung von Visual Studio zu veröffentlichen, müssen Sie dieses Zertifikat [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen.

1. Das Zertifikat [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen:

    1. Wählen Sie den Link Azure-Portal aus.

         Der [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885) wird geöffnet.

    1. Melden Sie sich bei der [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885), und wählen Sie dann auf die Schaltfläche **Cloud Services** .

    1. Wählen Sie aus der Cloud-Dienst, der Sie interessiert.

        Die Seite für den Dienst wird geöffnet.

    1. Wählen Sie die Schaltfläche **Hochladen** aus, klicken Sie auf die Registerkarte **Zertifikate** .

    1. Fügen Sie den vollständigen Pfad der CER-Datei, die Sie soeben erstellt haben, und geben Sie dann das Kennwort ein, das Sie angegeben haben.
