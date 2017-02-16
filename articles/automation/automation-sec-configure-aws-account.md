<properties
   pageTitle="Konfigurieren der Authentifizierung mit Amazon-Webdiensten | Microsoft Azure"
   description="Dieser Artikel beschreibt das Erstellen und Überprüfen einer AWS Anmeldeinformationen für Runbooks in Azure Automatisierung AWS Ressourcen verwalten."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="AWS Authentifizierung Aws konfigurieren"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks mit Amazon-Webdiensten authentifizieren
Automatisieren häufiger Aufgaben mit Ressourcen in Amazon Web Services (AWS) kann mit Automatisierung Runbooks in Azure ausgeführt werden.  Sie können viele Aufgaben in AWS mit Automatisierung Runbooks ebenso wie beim mit Ressourcen Azure automatisieren.  Alle, die erforderlich sind Dinge zwei:

* Ein Abonnement AWS und eine Reihe von Anmeldeinformationen.  Insbesondere der AWS Access und geheimen Schlüssel.  Weitere Informationen finden Sie im Artikel [AWS Anmeldeinformationen verwenden](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Ein Azure-Abonnement und Automatisierung Konto.  Weitere Informationen über das Einrichten einer Automatisierung Azure-Konto finden Sie im Artikel [Als Konto ausführen Azure konfigurieren](../automation/automation-sec-configure-azure-runas-account.md).  

Um mit AWS authentifizieren zu können, müssen Sie eine Reihe von AWS Anmeldeinformationen mit der Ausführung von Azure Automatisierung Runbooks authentifiziert angeben. Wenn bereits ein Automatisierung Konto erstellt und die Authentifizierung mit AWS verwenden möchten, können Sie die folgenden Abschnitte enthalten die Schritte.  Wenn Sie ein Konto für Runbooks Anwendung AWS Ressourcen dedizierter möchten, sollten Sie zuerst erstellen ein neues [Konto Automatisierung ausführen als](../automation/automation-sec-configure-azure-runas-account.md) (überspringen die Option zum Erstellen einer Tilgungsanteile Service) und führen Sie die folgenden Schritte aus.

## <a name="configure-automation-account"></a>Automatisierung Konto konfigurieren
Für die Kommunikation mit AWS Azure-Automatisierung müssen Sie zunächst Ihre Anmeldeinformationen AWS abrufen und diese als Anlagen in Azure Automatisierung speichern.  Führen Sie die folgenden Schritte aus, die in das Dokument AWS [Verwalten von Tastenkombinationen für Ihr Konto AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) Zugriffstaste erstellen und kopieren Sie die **Access-Schlüssel-ID** und **Geheim Zugriffstaste** dokumentierten (optional herunterladen die Datei Schlüssel zum sicherer Stelle speichern).

Nachdem Sie Ihre AWS Security Keys kopiert und erstellt haben, müssen Sie zum Erstellen einer Anlage Anmeldeinformationen mit einem Konto Azure Automatisierung sicher zu speichern und mit Ihrem Runbooks zu verweisen.  Führen Sie die Schritte im Abschnitt, **um einen neuen Eintrag erstellen** im Artikel [Anmeldeinformationen Anlagen in Azure Automatisierung](../automation/automation-certificates.md/###To create a new credential with the Azure portal) , und geben Sie die folgenden Informationen ein:

1. Geben Sie im Feld **Name** **AWScred** oder einen geeigneten Wert naming-Standards folgen.  
2. Geben Sie im Feld **Benutzername** Ihre **Access-ID** und den **Geheim Zugriffstaste** im Feld **Kennwort** und **Kennwort bestätigen** ein.   

## <a name="next-steps"></a>Nächste Schritte

- Reivew im Artikel Lösung [automatisieren Bereitstellung eines virtuellen Computers in Amazon-Webdiensten](../automation/automation-scenario-aws-deployment.md) erfahren, wie Runbooks zum Automatisieren von Aufgaben in AWS erstellen.
