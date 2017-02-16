<properties
    pageTitle="Zulassen der Anwendung Revtrieve Azure Stapel Schlüssel Tresor Kennwörter | Microsoft Azure"
    description="Verwenden Sie eine Beispiel-app mit Azure Stapel Schlüssel Tresor entwickelt"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Führen Sie die Anwendung Stichprobe für die Taste Tresor 

In diesem Handbuch Verwenden Sie eine Beispiel-Anwendung zum Abrufen von vertraulichen Daten und Kennwörtern aus Tresor-Taste.

## <a name="download-the-samples-and-prepare"></a>Die Beispiele herunterladen und Vorbereiten

Herunterladen der Azure-Taste Tresor Client Beispiele von der [Seite mit Azure Schlüssel Tresor Client Beispielen](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Extrahieren Sie den Inhalt der ZIP-Datei mit Ihrem lokalen Computer an.

Lesen Sie die **README.md** -Datei (Dies ist eine Textdatei), und folgen Sie dann die Anweisungen.

## <a name="run-sample-1--hellokeyvault"></a>Führen Sie Beispiel #1 – HelloKeyVault
HelloKeyVault ist eine Console-Anwendung, die durch die Schlüsselszenarien geführt, die durch die Taste Tresor unterstützt werden:

  1. Erstellen/importieren Sie einen Schlüssel (HSM oder Software Schlüssel)
  2. Verschlüsseln einer geheim Drücken einer Taste Inhalten
  3. Umbrechen Sie die Inhalte-Taste drücken einer Taste Tresor Taste
  4. Entpacken Sie den Inhalten Schlüssel
  5. Das Geheimnis entschlüsseln
  6. Legen Sie einen geheimen Schlüssel

Die Console-Anwendung sollte über keine Änderungen ausführen, mit dem Unterschied, dass die entsprechenden Konfiguration Einstellungen in App.Config entsprechend den folgenden Schritten aktualisiert werden sollen:

1. Aktualisieren der Einstellungen für app-Konfiguration HelloKeyVault\App.config mit Ihrem Tresor URL, Anwendung Hauptbenutzer-ID und geheim. Die Informationen kann optional mit **scripts\GetAppConfigSettings.ps1**generiert werden.
2. Aktualisieren Sie die Werte der Variablen in GetAppConfigSettings.ps1 obligatorischen.
3. Starten Sie Windows PowerShell-Fenster an.
4. Führen Sie das Skript GetAppConfigSettings.ps1 innerhalb der PowerShell-Fenster an.
5. Kopieren Sie die Ergebnisse des Skripts in der Datei HelloKeyVault\App.config.


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines virtuellen Computers mit einem Kennwort Tresor-Taste](azure-stack-kv-deploy-vm-with-secret.md)

[Bereitstellen eines virtuellen Computers mit einem Zertifikat Tresor-Taste](azure-stack-kv-push-secret-into-vm.md)