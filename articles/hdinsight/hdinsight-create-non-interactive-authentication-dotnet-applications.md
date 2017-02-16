<properties
    pageTitle="Erstellen von nicht-interaktiven Authentifizierung .NET HDInsight Applciations | Microsoft Azure"
    description="Erfahren Sie, wie nicht interaktiven Authentifizierung .NET HDInsight Applications erstellen."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Erstellen von nicht interaktiven Authentifizierung .NET HDInsight Applikationen

Sie können eine .NET Azure HDInsight-Anwendung unter der Identität des Anwendung (nicht interaktiv) oder unter der Identität des Benutzers angemeldet in der Anwendung (interaktiv) ausführen. Ein Beispiel der interaktiven Anwendung finden Sie unter [übermitteln Struktur/Schwein/Sqoop Aufträge HDInsight .NET SDK verwenden](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). In diesem Artikel wird das Erstellen von nicht interaktiven Authentifizierung .NET Anwendung, um eine Verbindung zur Azure HDInsight und Absenden eines Auftrags Struktur veranschaulicht.

Aus Ihrer Anwendung .NET benötigen Sie Folgendes:

- Ihre Azure-Abonnement Mandanten-ID
- die Azure Directory-Client-ID
- die Azure Directory geheimen Anwendungstaste.  

Der Hauptfenster Vorgang umfasst die folgenden Schritte aus:

2. Erstellen Sie eine Anwendung Azure Directory.
2. Die Anwendung AD Rollen zuweisen.
3. Entwickeln Sie Ihre Client-Anwendung.


##<a name="prerequisites"></a>Erforderliche Komponenten

- HDInsight Cluster. Sie können eine anhand der Anweisungen im [Lernprogramm überfordert](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)gefunden erstellen. 




## <a name="create-azure-directory-application"></a>Erstellen von Azure Directory-Anwendung 
Wenn Sie eine Active Directory-Anwendung erstellen, erstellt sie eigentlich die Anwendung und einem Dienst Hauptbenutzer. Sie können die Anwendung unter der Anwendung Identität ausführen.

Aktuell, müssen Sie im klassische Azure-Portal verwenden, um eine neue Active Directory-Anwendung zu erstellen. Diese Funktion wird in einer späteren Version Azure-Portal hinzugefügt werden. Sie können auch folgendermaßen über Azure PowerShell oder Azure CLI ausführen. Weitere Informationen zur Verwendung von PowerShell oder CLI mit dem Dienst Hauptbenutzer finden Sie unter [authentifizieren Service Hauptbenutzer Azure Ressourcenmanager](../resource-group-authenticate-service-principal.md).

**Zum Erstellen einer Anwendung Azure-Verzeichnis**

1.  Melden Sie sich zum [Azure klassischen Portal]( https://manage.windowsazure.com/)aus.
2.  Wählen Sie im linken Bereich aus **Active Directory** .

    ![Azure klassischen-Portals active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Wählen Sie das Verzeichnis aus, das Sie zum Erstellen der neuen Anwendungs verwenden möchten. Die vorhandenen werden muss.
4.  Klicken Sie oben auf die vorhandenen Applikationen Liste auf **Applications** .
5.  Klicken Sie nach unten, um eine neue Anwendung hinzufügen auf **Hinzufügen** .
6.  Geben Sie **ein**, wählen Sie **Web-Anwendung und/oder Web-API**aus, und klicken Sie dann auf **Weiter**.

    ![neue Azure-active Directory-Anwendung](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Geben Sie **Anmelden URL** und **App-ID-URI**aus. **Melden Sie sich auf URL**vorsehen Sie den URI zu einer Website, die eine Anwendung beschreibt. Das Vorhandensein der Website wird nicht überprüft. Bieten Sie für APP-ID-URI des URIS, Ihrer Anwendung bezeichnet. Ein, und klicken Sie dann auf **abgeschlossen**.
Es dauert einen Moment, um die Anwendung zu erstellen.  Nachdem die Anwendung erstellt wurde, werden im Portal die Symbolleiste Reihe Seite der neuen Anwendung. Schließen Sie das Portal nicht. 

    ![Eigenschaften der neuen Azure-active Directory-Anwendung](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Dem Anwendungsclient-ID und dem geheimen Schlüssel abrufen**

1.  Klicken Sie aus der neu erstellten Seite der AD-Anwendung im oberen Menü auf **Konfigurieren** .
2.  Erstellen Sie eine Kopie der **Client-ID**an. Sie benötigen sie in Ihrer Anwendung .NET.
3.  Klicken Sie unter **Tasten**klicken Sie auf die Dropdownliste **Wählen Sie eine Dauer** , und wählen Sie entweder **Jahr 1** oder **2 Jahre**. Der Schlüsselwert wird nicht angezeigt werden, bis die Konfiguration zu speichern.
4.  Klicken Sie auf **Speichern** , klicken Sie auf den unteren Rand der Seite. Wenn der geheime Schlüssel angezeigt wird, stellen Sie eine Kopie des Schlüssels aus. Sie benötigen sie in Ihrer Anwendung .NET.

##<a name="assign-ad-application-to-role"></a>AD-Anwendung, die Rolle zuweisen

Sie müssen die Anwendung zu einer [Rolle](../active-directory/role-based-access-built-in-roles.md) zu erteilen von Berechtigungen zum Ausführen von Aktionen zuweisen. Sie können den Bereich auf der Ebene der Abonnements, Ressourcengruppe oder Ressource festlegen. Die Berechtigungen werden übernommen, um geringer Umfang (z. B. hinzufügen, die Anwendung der Rolle Leser für eine Ressourcengruppe bedeutet, dass er die Ressourcengruppe gelesen werden kann und alle darin enthaltenen Ressourcen). In diesem Lernprogramm legen Sie den Bereich auf der Gruppenebene Ressource.  Da das Azure klassische Portal Ressourcengruppen unterstützt, muss dieses Teils vom Azure-Portal ausgeführt werden. 

**Die Besitzerrolle der AD-Anwendung hinzu**

1.  Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
2.  Klicken Sie im linken Bereich auf **Ressourcengruppe** .
3.  Klicken Sie auf die Ressourcengruppe, die hdinsight Cluster enthält, in dem Sie Ihre Abfrage Struktur später in diesem Lernprogramm ausgeführt wird. Wenn zu viele Ressourcengruppen vorhanden sind, können Sie den Filter verwenden.
4.  Klicken Sie auf **Zugriff** aus dem Cluster Blade.

    ![Symbol Cloud und Thunderbolt = Schnellstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Klicken Sie auf **Hinzufügen** , aus dem **Benutzer** Blade.
6.  Führen Sie die Anweisung hinzufügen die Rolle **Besitzer** für die AD-Anwendung, dass Sie in der letzten Prozedur erstellt haben. Wenn Sie es erfolgreich abgeschlossen haben, gilt die Anwendung in das Blade Benutzer mit der Rolle Besitzer aufgeführt angezeigt.


##<a name="develop-hdinsight-client-application"></a>HDInsight Clientanwendung entwickeln

Erstellen Sie eine c# .net folgen die Anweisungen in [übermitteln Hadoop Aufträge in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)gefunden. Ersetzen Sie die Methode GetTokenCloudCredentials klicken Sie dann mit der folgenden:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Zum Abrufen von den Mandanten-ID über PowerShell:

    Get-AzureRmSubscription

Oder Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Siehe auch

- [Hadoop Aufträge in HDInsight senden](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Erstellen von Active Directory-Anwendung und Dienst Tilgungsanteile mithilfe von portal](../resource-group-create-service-principal-portal.md)
- [Authentifizieren Sie Dienst Tilgungsanteile Azure Ressourcenmanager](../resource-group-authenticate-service-principal.md)
- [Azure rollenbasierte Access Control](../active-directory/role-based-access-control-configure.md)
