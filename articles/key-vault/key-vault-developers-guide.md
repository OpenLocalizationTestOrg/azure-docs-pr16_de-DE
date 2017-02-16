<properties
   pageTitle="Wichtiger Tresor Developer's Guide | Microsoft Azure"
   description="Azure-Taste Tresor können Entwickler um cryptographic Tasten innerhalb der Microsoft Azure-Umgebung zu verwalten. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Azure Key Tresor Developer's Guide
Verwenden die Taste Tresor, Sie werden möglicherweise vertrauliche Informationen von in Ihrer Anwendung sicheren Zugriff auf beispielsweise:

- Tasten und Kennwörter werden ohne den Code selbst schreiben geschützt werden und einfach können sie aus einer Anwendung heraus verwendet.
- Sie zwar können haben Ihre eigenen Kunden und verwalten ihre eigenen Schlüssel, sodass Sie sich auf das Herzstück Softwarefunktionen bereitstellt konzentrieren können. Auf diese Weise werden Ihre Anwendungen nicht die Zuständigkeit oder potenzielle Haftung für Ihren Kunden Mandanten Tasten und Kennwörter besitzen.
- Ihrer Anwendung können Schlüssel zum Signieren und Verschlüsselung noch behält die Key-Verwaltung außerhalb Ihrer Anwendung so, dass die Lösung für die Anwendung geeignet ist, die geografischen verteilt ist.

- Mit der Version der Taste Tresor September 2016 Ihrer Anwendung nun umso verwenden der Taste Tresor Zertifikate. Für Weitere Informationen finden Sie unter **über Schlüssel, Kennwörter, und Zertifikate** Artikel im [restlichen Bezug](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Weitere allgemeine Informationen zur Azure-Taste Tresor finden Sie unter [Was ist die Taste Tresor](key-vault-whatis.md).

## <a name="videos"></a>Videos
In diesem Video werden Verfahren zum Erstellen von Ihrem eigenen Key Tresor und Einsatzbreite aus der Anwendung 'Hallo Schlüssel Tresor' Stichprobe.

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Links zu Ressourcen, die in das Video angegeben ist:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Beispiel-Code Azure Key Tresor](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Erfahren Sie, können Sie weitere führen Sie die [Taste Tresor Blog](http://aka.ms/kvblog) und die [Taste Tresor Forum](http://aka.ms/kvforum)teilnehmen.

## <a name="creating-and-managing-key-vaults"></a>Erstellen und Verwalten von Key Depots

Sie können vor dem Arbeiten mit Azure-Taste Tresor im Code ein, erstellen und Verwalten von Depots über REST, Ressourcenmanager Vorlagen, PowerShell oder CLI, wie in den folgenden Artikeln beschrieben:

- [Erstellen und Verwalten von Key Depots mit weiteren](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Erstellen und Verwalten von Key Depots mit PowerShell](key-vault-get-started.md)
- [Erstellen und Verwalten von Key Depots mit CLI](key-vault-manage-with-cli.md)
- [Erstellen Sie eines Key Tresor, und fügen Sie einen geheimen über eine Vorlage Azure Ressourcenmanager](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Vorgänge gegen Schlüssel Depots werden durch AAD authentifiziert und autorisiert über Schlüssel Tresor des eigenen Richtlinien, pro Tresor definiert.

## <a name="coding-with-key-vault"></a>Programmieren mit Key Tresor

Das Taste Tresor Management-System für Programmierer besteht aus mehreren Schnittstellen, mit weiteren als Grundlage, [Schlüssel Tresor REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Sie können, unterliegen erfolgreiche Autorisierung die folgenden Aktionen ausführen:

- Verwalten von cryptographic [Erstellen](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Importieren](https://msdn.microsoft.com/library/azure/dn903626.aspx), [Aktualisieren](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Löschen](https://msdn.microsoft.com/library/azure/dn903611.aspx) und andere Vorgänge mithilfe von Tasten

- Verwalten von vertraulichen Daten [Abrufen](https://msdn.microsoft.com/library/azure/dn903633.aspx), [Aktualisieren](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Löschen](https://msdn.microsoft.com/library/azure/dn903613.aspx) oder andere Vorgänge verwenden

- Verwenden Sie cryptographic Tasten mit [Vorzeichen](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Überprüfen](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) und [Verschlüsseln](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[Entschlüsseln](https://msdn.microsoft.com/library/azure/dn878097.aspx) Vorgänge

Die folgenden SDKs sind für das Arbeiten mit Schlüssel Tresor verfügbar:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[.NET SDK-Dokumentation](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js SDK-Dokumentation](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[.NET SDK-Paket auf Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK-Paket](https://www.npmjs.com/package/azure-keyvault)|

Weitere Informationen zur 2.x Version des .NET SDK finden Sie unter den [Versionsinformationen](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Beispielcode
Vollständige Beispiele für die Verwendung der Taste Tresor mit Ihrer Anwendung finden Sie unter:

- Beispiel-Anwendung .NET *HelloKeyVault* und einer Azure Webdienst-Beispiel. [Azure-Taste Tresor Codebeispielen](http://www.microsoft.com/download/details.aspx?id=45343)
- Lernprogramm, damit Sie die Informationen zum Azure-Taste Tresor aus einer Webanwendung in Azure verwenden können. [Verwenden von Azure Key Tresor aus einer Webanwendung](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>Gewusst wie

In den folgenden Artikeln und Szenarien bieten Vorgang spezifische Leitfäden für die Arbeit mit Azure-Taste Tresor:

- [Key Tresor Mandanten-ID nach dem Verschieben von Abonnement ändern](key-vault-subscription-move-fix.md) – Wenn Sie Ihr Abonnement Azure von Mandanten A auf Mandanten B verschieben, werden Ihre vorhandene Key Depots nicht zugegriffen werden kann, indem Sie die Hauptbenutzer (Benutzer und Applikationen) Mandanten b Beheben dieser mit diesem Leitfaden.
- [Zugreifen auf Schlüssel Tresor hinter der Firewall](key-vault-access-behind-firewall.md) - zu einem Key Tresor zugreifen, die die wichtigsten Tresor Clientanwendung benötigt mehrere Endpunkte für verschiedene Funktionalitäten zugreifen können.

- [So generieren und Transfer HSM-Protected Schlüssel für die Taste Tresor Azure](key-vault-hsm-protected-keys.md) - können Sie planen, generieren und dann eigene HSM geschützte Schlüssel zur Verwendung mit Azure-Taste Tresor übertragen.
- [Wie sichere Werte (zum Beispiel Kennwörter) während der Bereitstellung übergeben](../resource-manager-keyvault-parameter.md) -, wenn Sie einen sicheren Wert (wie etwa ein Kennwort) als Parameter während der Bereitstellung übergeben müssen, können Sie diesen Wert als Geheimnis einen Azure-Taste Tresor und Verweis den Wert in anderen Ressourcenmanager Vorlagen speichern.
- [So verwenden Sie die Taste Tresor für extensible Key-Verwaltung mit SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - der SQL Server-Connector für die Taste Tresor Azure ermöglicht SQL Server und SQL-in-VM zur Nutzung des Azure-Taste Tresor Diensts als Provider Extensible Key Management (EKM) die Schlüssel für Applikationen-Link zu schützen; Transparent Data Verschlüsselung, Sicherung Verschlüsselung und Verschlüsselung Spalte.
- [Verfahren zum Bereitstellen Zertifikaten zu virtuellen Computern aus Tresor Key](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - einer Cloud-Anwendung, die auf Windows Azure in einen virtuellen Computer ausgeführte benötigt ein Zertifikat. Wie erhalte Sie dieses Zertifikat in diesem virtuellen Computer heute?
- [Vorgehensweise für die Installation Schlüssel Tresor mit durchgehende Key Drehung und Überwachung](key-vault-key-rotation-log-monitoring.md) - diese schrittweise Anleitung zum Einrichten von Key Drehung und Überwachung mit Azure-Taste Tresor.

Weitere Task-spezifische Anleitungen zur Integration und Schlüssel Depots mit Azure verwenden finden Sie unter [Ryan Jonas Azure Ressourcenmanager Vorlage Beispiele für Schlüssel Tresor](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Mit Key Tresor integriert

Die folgenden Artikel sind zu anderen Szenarien und Diensten, die uns von Stellen oder mit Schlüssel Tresor integrieren.

- [Azure Datenträger Verschlüsselung](../security/azure-security-disk-encryption.md) nutzt die Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux um Lautstärke Verschlüsselung für das Betriebssystem und Festplatten mit den Daten zu ermöglichen. Die Lösung ist integriert mit Azure-Taste Tresor, mit deren Hilfe Sie steuern und Verwalten der Schlüssel zur Verschlüsselung und vertrauliche Informationen in Ihrem Abonnement Key Tresor und dabei sicherstellen, dass alle Daten im virtuellen Computern Laufwerke zum Rest in Azure-Speicher verschlüsselt werden.


## <a name="supporting-libraries"></a>Unterstützung von Bibliotheken

- [Microsoft Azure Schlüssel Tresor Core-Bibliothek](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) bietet `IKey` und `IKeyResolver` Schnittstellen für die Tasten aus Bezeichnern Auffinden und Ausführen von Vorgängen mit Tasten.

- [Microsoft Azure Schlüssel Tresor Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) bietet erweiterte Funktionen für Azure-Taste Tresor.

## <a name="other-key-vault-resources"></a>Weitere Ressourcen Tresor-Taste
- [Key Tresor-Blog](http://aka.ms/kvblog)
- [Key Tresor-Forum](http://aka.ms/kvforum)
