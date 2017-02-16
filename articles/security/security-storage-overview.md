<properties
   pageTitle="Übersicht über die Sicherheit von Azure-Speicher | Microsoft Azure"
   description=" Azure-Speicher ist die Cloud-Speicher-Lösung für moderne, die auf die Zuverlässigkeit, Verfügbarkeit und Skalierbarkeit auf die Bedürfnisse seiner Kunden aufsetzen. In diesem Artikel bietet einen Überblick über die wichtigsten Azure Sicherheitsfeatures, die mit Azure-Speicher verwendet werden können. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Übersicht über die Sicherheit von Azure-Speicher

Azure-Speicher ist die Cloud-Speicher-Lösung für moderne, die auf die Zuverlässigkeit, Verfügbarkeit und Skalierbarkeit auf die Bedürfnisse seiner Kunden aufsetzen. Azure-Speicher bietet eine umfassende Reihe von Sicherheitsfunktionen, die an:

- Das Speicherkonto kann mithilfe von Access Control rollenbasierte und Azure Active Directory gesichert werden.
- Daten können mit clientseitig Verschlüsselung, HTTPS oder SMB 3.0 im Übergang zwischen einer Anwendung und Azure gesichert werden.
- Daten können automatisch verschlüsselt werden, wenn in den Azure-Speicher geschrieben festlegen Speicher-Service-Verschlüsselung verwenden.
- Betriebssystem und die Daten der von virtuellen Computern verwendete Datenträger können festgelegt werden, mit Azure Datenträger Verschlüsselung verschlüsselt werden.
- Delegierter Zugriff auf die Datenobjekte in Azure-Speicher kann mithilfe von freigegebenen Access Signaturen erteilt werden.
- Die verwendete von einem Benutzer beim Zugriff auf Speicher Authentifizierungsmethode kann mit Speicher Analytics überwacht werden.

Eine ausführlichere Betrachtung Sicherheit in Azure-Speicher finden Sie im [Azure Storage Security Guide](../storage/storage-security-guide.md). Dieses Handbuch bietet eine detaillierte technische Informationen in die Sicherheitsfeatures Azure-Speicher wie Speicher Konto Tasten, Daten Verschlüsselung unterwegs und am Ruhe und Speicher Analytics.

Dieser Artikel bietet einen Überblick über die Sicherheit von Azure-Features, die mit Azure-Speicher verwendet werden kann. Links erhalten zu Artikeln, mit die jede Funktion Details hierzu erhalten Sie weitere Informationen können.

Hier sind die wichtigsten Features, die in diesem Artikel behandelt werden:

- Rollenbasierte Access Control
- Delegierten Zugriff auf Speicherobjekte
- Bei der Übertragung Verschlüsselung
- Verschlüsselung bei Rest-Speicher-Service-Verschlüsselung
- Azure Datenträger-Verschlüsselung
- Azure Key Tresor

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Access Control (RBAC)

Sie können Ihr Speicherkonto mit rollenbasierte Access Control (RBAC) sichern. Einschränken des Zugriffs basierend auf der [minimalen Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) und [wissen müssen](https://en.wikipedia.org/wiki/Need_to_know) Sicherheit Grundsätze muss unbedingt für Organisationen, die Sicherheitsrichtlinien für den Zugriff auf Daten erzwingen möchten. Indem Sie Gruppen und Anwendungen mit einem bestimmten Bereich die entsprechende RBAC-Rolle zuweisen, werden diese Zugriffsrechte erteilt. [Integrierte RBAC-Rollen](../active-directory/role-based-access-built-in-roles.md), wie z. B. Speicher Konto Mitwirkender, können Sie Benutzern Berechtigungen zuweisen.

Weitere Informationen:

- [Rollenbasierte Access-Azure-Active Directory-Steuerelements](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegierten Zugriff auf Speicherobjekte

Eine freigegebene Access-Signatur (SAS) bietet delegierten Zugriff auf Ihr Speicherkonto Ressourcen. Die SAS bedeutet, dass Sie gewähren können, dass ein Client Berechtigungen für Objekte in Ihr Speicherkonto für einen angegebenen Zeitraum Zeit und mit einem zuvor festgelegten Berechtigungen eingeschränkt. Sie können diese eingeschränkten Berechtigungen erteilen, ohne zu Ihrem Konto Zugriffstasten freigeben. Die SAS ist ein URI, der in deren Abfrageparameter alle Informationen für den authentifizierten Zugriff auf eine Speicherressource erforderlich umfasst. Um Speicherressourcen mit der SAS zugreifen zu können, muss der Client nur die SAS auf die entsprechenden oder Methode zu übergeben.

Weitere Informationen:

- [Grundlegendes zu SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Erstellen Sie und verwenden Sie eines SAS mit Blob-Speicher](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Bei der Übertragung Verschlüsselung
Bei der Übertragung Verschlüsselung ist ein Verfahren zum Schutz Ihrer Daten, wenn sie über Netzwerke übertragen wird. Mit Azure-Speicher können Sie die Daten mit sichern:

- [Verschlüsselung Transport Ebene](../storage/storage-security-guide.md#encryption-in-transit), wie z. B. HTTPS hinzu, wenn Sie Daten in oder aus Azure-Speicher übertragen.
- [Verschlüsselung](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), wie z. B. SMB 3.0 Verschlüsselung für Dateifreigaben Azure.
- [Clientseitige Verschlüsselung](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), zum Verschlüsseln der Daten, bevor diese in den Speicher übertragen werden und die Daten zu entschlüsseln, nachdem sie nicht über ausreichend Speicher übertragen werden.

Weitere Informationen zu clientseitige Verschlüsselung:

- [Clientseitige Verschlüsselung für Microsoft Azure-Speicher](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud-Sicherheit steuert Reihe: Verschlüsseln von Daten bei der Übertragung](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Verschlüsselung statisch

Für viele Organisationen ist die [Verschlüsselung der Daten statisch sind](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt in Richtung Datenschutz, Compliance und Hoheit Daten. Es gibt drei Azure Features, mit denen Verschlüsselung der Daten, die "at Rest" ist:

- [Die Verschlüsselung der Speicher Service](../storage/storage-security-guide.md#encryption-at-rest) können Sie anfordern, dass der Speicherdienst Daten automatisch verschlüsselt, wenn es in Azure-Speicher geschrieben.
- [Clientseitige Verschlüsselung](../storage/storage-security-guide.md#client-side-encryption) bietet auch das Feature der Verschlüsselung statisch sind.
- [Azure Datenträger Verschlüsselung](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) können Sie die OS Festplatten und die von einer IaaS virtuellen Computers verwendete Daten-Datenträger verschlüsseln.

Weitere Informationen zu Speicher-Service-Verschlüsselung:

- [Azure-Speicher-Service-Verschlüsselung](https://azure.microsoft.com/services/storage/) steht für [Azure BLOB-Speicher](https://azure.microsoft.com/services/storage/blobs/). Details zu anderen Typen Azure-Speicher finden Sie unter [Datei](https://azure.microsoft.com/services/storage/files/), [Datenträger (Premium Speicher)](https://azure.microsoft.com/services/storage/premium-storage/), [Tabelle](https://azure.microsoft.com/services/storage/tables/)und [Warteschlange](https://azure.microsoft.com/services/storage/queues/).
- [Service-Verschlüsselung von Daten in der restlichen Azure-Speicher](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Datenträger-Verschlüsselung

Azure Verschlüsselung virtuellen Computern (virtuellen Computern) hilft Ihnen Adresse organisationsinterne Sicherheit und Einhaltung von Vorschriften Anforderungen durch Ihre Datenträger virtueller Computer (einschließlich Start- und Laufwerke) mit Tasten und Richtlinien, die Sie in [Die Taste Tresor Azure](https://azure.microsoft.com/services/key-vault/)steuern verschlüsseln.

Verschlüsselung virtuellen Computern funktioniert für Linux und Windows-Betriebssysteme auf. Außerdem verwendet können Sie zu schützen, verwalten und überwacht die Verwendung der Schlüssel für die Verschlüsselung der Datenträger Schlüssel Tresor. Alle Daten in der Datenträger virtueller Computer ist bei Rest in Ihren Konten Azure-Speicher mithilfe von Industriestandard die Verschlüsselung verschlüsselt. Die Verschlüsselung Festplatten Lösung für Windows basiert auf [Microsoft der BitLocker](https://technet.microsoft.com/library/cc732774.aspx), und die Lösung Linux basiert auf [dm-Crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Weitere Informationen:

- [Azure Datenträger Verschlüsselung für Windows und Linux IaaS virtuellen Computern](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Tresor

Azure Datenträger Verschlüsselung verwendet [Azure Schlüssel Tresor](https://azure.microsoft.com/services/key-vault/) , mit deren Hilfe Sie steuern und Verwalten von Schlüssel zur Verschlüsselung und vertrauliche Informationen in Ihrem Abonnement Key Tresor und dabei sicherstellen, dass alle Daten im virtuellen Computern Laufwerke zum Rest in Ihrem Azure Storage verschlüsselt werden. Sie sollten Schlüssel Tresor Tasten und die Richtlinienverwendung überwacht verwenden.

Weitere Informationen:

- [Was ist die Taste Tresor Azure?](../key-vault/key-vault-whatis.md)
- [Erste Schritte mit Azure-Taste Tresor](../key-vault/key-vault-get-started.md)
