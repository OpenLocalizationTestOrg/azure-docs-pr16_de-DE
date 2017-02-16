<properties
   pageTitle="Übersicht über die Sicherheit in Lake Datenspeicher | Microsoft Azure"
   description="Zu verstehen, wie Azure Lake Datenspeicher eine große sicherer Datenspeicher ist"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Sicherheit in Azure Lake Datenspeicher

Viele Unternehmen abzubrechen nutzen big Data Analytics für Business Einsichten smart Entscheidungen treffen helfen. Eine Organisation möglicherweise eine komplexe und geregelte Umgebung mit einer zunehmenden Anzahl von verschiedenen Benutzern verfügen. Es ist entscheidend für ein Unternehmen, um sicherzustellen, dass die wichtigen Geschäftsdaten sicherer, mit der richtigen Ebene der Zugriffsberechtigungen für einzelne Benutzer gespeichert ist. Azure Lake Datenspeicher soll helfen, diese Sicherheit zu erfüllen. In diesem Artikel lernen Sie die Sicherheitsfunktionen von Lake Datenspeicher, einschließlich:

* Authentifizierung
* Autorisierung
* Grad der Netzwerkisolation
* Datenschutz
* Überwachung

## <a name="authentication-and-identity-management"></a>Verwaltung der Authentifizierung und Identität

Der Authentifizierung wird durch die Identität des Benutzers überprüft wird, wenn der Benutzer interagiert mit Lake Datenspeicher oder mit jedem Dienst, der und Lake Datenspeicher besteht. Verwaltung von Identität und-Authentifizierung verwendet Lake Datenspeicher [Azure Active Directory](../active-directory/active-directory-whatis.md), eine umfassende Identität und Cloudlösung für die Verwaltung, die die Verwaltung von Benutzern und Gruppen vereinfacht.

Jede Azure-Abonnement kann eine Instanz der Azure Active Directory zugeordnet werden. Nur Benutzer und Dienstidentitäten, die in Ihrem Azure Active Directory-Dienst definiert sind Ihr Konto Lake Datenspeicher mithilfe des Azure-Portals Befehlszeile Tools zugreifen oder über Clientanwendungen Ihrer Organisation erstellt, mit dem Azure Daten dem Store SDK. Key Vorteile der Verwendung von Azure Active Directory als einen zentralen Zugriffskontrollmechanismus sind:

* Vereinfachte Identität Lifecycle Management. Die Identität des einen Benutzer oder eine Dienst (ein Dienst Hauptbenutzer Identität) kann schnell erstellt und schnell von einfach löschen oder deaktivieren das Konto im Verzeichnis gesperrt werden.

* Mehrstufige Authentifizierung. [Mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md) bietet eine zusätzliche Sicherheitsebene für Benutzer anmelden-ins und Transaktionen.

* Authentifizierung von einem beliebigen Client über ein standard öffnen Protokoll, beispielsweise OAuth oder OpenID.

* Föderation mit Enterprise-Verzeichnisdienst und Identitätsanbieter Cloud.

## <a name="authorization-and-access-control"></a>Autorisierung und Access-Steuerelement

Nachdem Azure Active Directory einen Benutzer authentifiziert hat, damit der Benutzer Azure Lake Datenspeicher zugreifen kann, Dateizugriffsberechtigungen Autorisierung Steuerelemente für Lake Datenspeicher. Lake Datenspeicher trennt die Autorisierung für Konto-bezogene und datenbezogene Aktivitäten in folgender Weise:

* [Rollenbasierte Access-Steuerelement](../active-directory/role-based-access-control-what-is.md) (RBAC) aus Azure zur Verwaltung von Konten
* POSIX ACL für den Zugriff auf Daten im Speicher


### <a name="rbac-for-account-management"></a>RBAC für die Verwaltung von Konten

Vier grundlegende Rollen werden standardmäßig für Lake Datenspeicher definiert. Die Rollen ermöglichen unterschiedliche Vorgänge auf einem Lake Datenspeicher Konto über den Azure-Portal, PowerShell-Cmdlets und REST-APIs. Die Besitzer und Mitwirkender Rollen können eine Vielzahl von Verwaltungsfunktionen für das Konto ausführen. Sie können die Rolle Leser Benutzern zuweisen, die nur mit Daten interagieren.

![RBAC-Rollen] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC-Rollen")

Beachten Sie, dass zwar Rollen für die Verwaltung von Konto zugewiesen sind, einige Rollen Zugriff auf Daten auswirken. Sie müssen ACLs verwenden, um Zugriff auf die Vorgänge zu steuern, die ein Benutzer auf das Dateisystem ausführen kann. Die folgende Tabelle enthält eine Zusammenfassung der Verwaltung von Informationsrechten und Zugriffsrechte für die Standardrollen Daten.

| Rollen                    | Verwaltung von Informationsrechten               | Zugriffsrechte für Daten | Erläuterung |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Keine Rolle zugewiesen ist         | Keine                            | ACL unterliegt    | Der Benutzer kann nicht Lake Datenspeicher Durchsuchen der Azure-Portal oder Azure PowerShell-Cmdlets verwenden. Der Benutzer kann nur Befehlszeile Tools verwenden.
| Besitzer  | Alle  | Alle  | Die Besitzerrolle ist ein Hauptbenutzer. Diese Rolle kann alles verwalten und Vollzugriff auf Daten enthält.
| Reader   | Schreibgeschützt  | ACL unterliegt    | Die Rolle Leser kann alles hinsichtlich Konto Verwaltung anzeigen wie die Benutzer die Rolle zugewiesen ist. Die Rolle Leser kann keine Änderungen vornehmen.   |
| Mitwirkenden              | Alles außer hinzufügen und Entfernen von Rollen | ACL unterliegt    | Die Rolle "Mitwirkender" kann einige Aspekte eines Kontos, z. B. Bereitstellungen und erstellen und Verwalten von Benachrichtigungen verwalten. Die Rolle "Mitwirkender" keine hinzufügen oder Entfernen von Rollen.
| Access-Administrator für Benutzer | Hinzufügen und Entfernen von Rollen            | ACL unterliegt    | Die Benutzer Access-Administratorrolle kann zur Verwaltung des Benutzerzugriffs auf Konten. |

Anweisungen finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen mit dem Datenspeicher Konten](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Mithilfe von ACLs für Vorgänge auf dem Dateisystem

Lake Datenspeicher ist eine hierarchische Dateisystem wie Hadoop Distributed Datei System (HDFS) und [POSIX ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)unterstützt. Sie lesen (R) steuert, schreiben (w) und Ausführen von (Berechtigungen für Ressourcen, die für die Rolle Besitzer für die Gruppe Besitzer und für andere Benutzer und Gruppen x). In dem Store Public Datenvorschau (der aktuellen Version) werden die ACLs auf den Stammordner, klicken Sie auf Unterordner und für einzelne Dateien aktiviert. Die ACLs, die Sie in den Stammordner anwenden gelten auch für alle untergeordneten Ordner und Dateien.

Es empfiehlt sich, dass Sie mithilfe von [Sicherheitsgruppen](../active-directory/active-directory-accessmanagement-manage-groups.md)ACLs für mehrere Benutzer definieren. Hinzufügen von Benutzern zu einer Sicherheitsgruppe, und klicken Sie dann die ACLs für eine Datei oder einen Ordner dieser Sicherheitsgruppe zuweisen. Dies ist nützlich, wenn Sie benutzerdefinierte Access angeben möchten, da Sie zum Hinzufügen von maximal neun Einträge für benutzerspezifische beschränkt sind. Weitere Informationen dazu, wie Sie besser sichere Daten gehörende Kehrmatrix Lake Datenspeicher mithilfe von Azure-Active Directory-Sicherheitsgruppen finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppe als ACLs im Dateisystem Azure dem Datenspeicher](data-lake-store-secure-data.md#filepermissions).

![Standard- und benutzerdefinierte Liste-Zugriff] (./media/data-lake-store-security-overview/adl.acl.2.png "Standard- und benutzerdefinierte Liste-Zugriff")

## <a name="network-isolation"></a>Grad der Netzwerkisolation

Verwenden Sie Lake Datenspeicher Steuern des Zugriffs auf Ebene der Netzwerk Ihrer Datenspeicher helfen. Sie können Firewalls herzustellen und einen IP-Adressenbereich für vertrauenswürdigen Kunden definieren. Mit einer IP-Adressbereich können nur Clients, die eine IP-innerhalb des definierten Bereichs Adresse mit Lake Datenspeicher verbinden.

![Firewall-Einstellungen und IP-Zugriff] (./media/data-lake-store-security-overview/firewall-ip-access.png "Firewall-Einstellungen und IP-Adresse")

## <a name="data-protection"></a>Datenschutz

Organisationen möchten sicherstellen, dass ihre Unternehmensdaten während des gesamten Lebenszyklus sicher ist. Für Daten auf dem Weg verwendet Lake Datenspeicher Protocol Transport Layer Security (TLS) Industriestandard, um die Daten zu sichern, die zwischen einem Client und Lake Datenspeicher ausgeführt werden.

Datenschutz für Daten statisch sind in zukünftigen Versionen zur Verfügung.

## <a name="auditing-and-diagnostic-logs"></a>Überwachung und Diagnose von Protokollen

Sie können ü oder diagnostic Protokolle, je nachdem, ob Sie Protokolle für Aktivitäten verwaltungsbezogenen oder datenspezifische Aktivitäten suchen.

*  Aktivitäten verwaltungsbezogenen Azure Ressourcenmanager APIs verwenden und Azure-Portal über Überwachungsprotokolle dargestellt werden.
*  Datenspezifische Aktivitäten WebHDFS REST-APIs verwenden und Azure-Portal über Diagnoseprotokolle dargestellt werden.

### <a name="auditing-logs"></a>Überwachen von Protokollen

Um die Einhaltung von Vorschriften möglicherweise eine Organisation ausreichend Überwachungsprotokolle erforderlich, wenn es in bestimmten Fälle einsteigen muss. Lake Datenspeicher weist integrierte Überwachung und Überprüfung, und sie alle Konto Management Aktivitäten protokolliert.

Zeigen Sie für das Konto Management Audit Spuren an, und wählen Sie die Spalten, die Sie melden möchten. Sie können auch Überwachungsprotokolle in Azure-Speicher exportieren.

![Überwachungsprotokolle] (./media/data-lake-store-security-overview/audit-logs.png "Überwachungsprotokolle")

### <a name="diagnostic-logs"></a>Diagnoseprotokolle

Sie können Daten Access Überwachungsprotokolle in das Azure-Portal (in Diagnoseeinstellungen) festlegen und erstellen Sie ein Azure Blob-Speicher-Konto, in dem die Protokolle gespeichert werden.

![Diagnoseprotokolle] (./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnoseprotokolle")

Nachdem Sie diagnoseeinstellungen konfiguriert haben, können Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** anzeigen.

Weitere Informationen zum Arbeiten mit einer Diagnoseprotokollen mit Azure Lake Datenspeicher finden Sie unter [Zugriff Diagnoseprotokolle für dem Datenspeicher](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Zusammenfassung

Enterprise-Kunden verlangen eine Analytics-Cloud, die sichere und einfach zu verwenden. Azure Lake Datenspeicher soll helfen, Adresse, die diese Anforderungen über die Identität Verwaltung und Authentifizierung über Azure-Active Directory-Integration, ACL-basierten Autorisierung, Network Isolation Daten Verschlüsselung unterwegs und am ruhen lassen (in Kürze in der Zukunft), und die Überwachung.

Wenn Sie die neuen Features in Lake Datenspeicher anzeigen möchten, senden Sie uns Feedback im [Forum Daten dem Store UserVoice](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure Lake Datenspeicher](data-lake-store-overview.md)
- [Erste Schritte mit Lake Datenspeicher](data-lake-store-get-started-portal.md)
- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
