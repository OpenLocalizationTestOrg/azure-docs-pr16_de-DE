<properties
   pageTitle="Daten Sicherheit und Verschlüsselung Best Practices | Microsoft Azure"
   description="Dieser Artikel bietet eine Reihe von bewährte Methoden für die Sicherheit und Verschlüsselung mit Azure-Funktionen integriert."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Bewährte Methoden für Sicherheit Azure-Daten und Verschlüsselung

Einer der Schlüssel zum Schutz von Daten in der Cloud ist "Buchhaltung" für die möglichen Zustände, in denen Ihre Daten auftreten können, und welche Steuerelemente für diesen Status verfügbar sind. Sicherheit und Verschlüsselung bewährte Methoden der Empfehlungen werden im Sinne Azure-Daten um Staaten mit den folgenden Daten:

- Bei Rest: Dies umfasst alle Informationen, die für Speicherobjekte, Container und Datentypen, die auf einem Datenträger statisch vorhanden sind sie einen oder optische Datenträger sein.

- In Übertragung: Wenn Daten ist übertragenen zwischen Komponenten, Speicherorte oder Programmen, z. B. über das Netzwerk, über eine Dienstbus (aus lokalen in die cloud und umgekehrt, einschließlich Hybrid-Verbindung, z. B. ExpressRoute), oder während eines Prozesses/Ausgang, es wird als betrachtet wird in Bewegung.

In diesem Artikel wird eine Zusammenstellung von Azure-Daten, Sicherheit und Verschlüsselung bewährte Methoden besprochen. Bewährten werden von unserer Erfahrung mit Azure Datenschutz und Verschlüsselung und die Verwendung des Kunden, wie sich selbst abgeleitet.

Für jede bewährte Methode wird erläutert:

- Was ist die beste Methode
- Warum Sie diese bewährte Methode aktivieren möchten
- Was kann das Ergebnis sein, wenn Sie nicht die beste Methode aktivieren
- Mögliche Alternativen bewährte Methode
- Wie können Sie lernen, wie die bewährte Methode aktivieren

In diesem Artikel Azure Daten Sicherheit und bewährte Methoden für die Verschlüsselung basiert auf eine Übereinstimmung Meinung und Azure-Plattformfunktionen und Features, sie zu dem Zeitpunkt vorhanden sind, die in diesem Artikel geschrieben wurde. Meinungen und Technologien Zeitverlauf ändern und in diesem Artikel wird in regelmäßigen Abständen diese Änderungen entsprechend aktualisiert werden.

Azure-Daten Sicherheit und Verschlüsselung bewährte Verfahren in diesem Artikel beschriebenen umfassen:

- Mehrstufige Authentifizierung erzwingen
- Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC)
- Verschlüsseln von Azure-virtuellen Computern
- Verwenden von Sicherheitsmodellen hardware
- Mit sicheren Arbeitsstationen verwalten
- Aktivieren Sie die Verschlüsselung der SQL-Daten
- Schützen von Daten bei der Übertragung
- Erzwingen der Datei Ebene-Verschlüsselung


## <a name="enforce-multi-factor-authentication"></a>Mehrstufige Authentifizierung erzwingen

Der erste Schritt beim Zugriff auf Daten und deren Steuerung in Microsoft Azure werden zum Authentifizieren des Benutzers ist. [Azure mehrstufige Authentifizierung (MFA)](../multi-factor-authentication/multi-factor-authentication.md) ist eine Methode zum Überprüfen der Identität des Benutzers mithilfe einer anderen Methode als nur einen Benutzernamen und ein Kennwort an. Diese Authentifizierung Methode hilft Schutz Zugriff auf Daten und Applikationen während der Besprechung Benutzer bei Bedarf für ein einfacher Vorgang Anmeldung.

Aktivieren Sie Azure MFA für Ihre Benutzer ein, fügen Sie eine zweite Sicherheitsebene Benutzer anmelden-ins und Transaktionen hinzu. In diesem Fall möglicherweise eine Transaktion ein Dokuments in einem Dateiserver oder in Ihrer SharePoint Online zugreifen. Azure MFA auch hilft IT zum Verringern Sie der Wahrscheinlichkeit, dass eine beschädigte Anmeldeinformationen zu Organisation Daten zugreifen kann.

Beispiel: Wenn Sie Azure MFA für Ihre Benutzer erzwingen und konfigurieren Sie ihn so verwenden einen Anruf oder Textnachricht als Überprüfung, wenn die Anmeldeinformationen des Benutzers gefährdet ist, der Angreifer kann nicht mehr Zugriff auf alle Ressourcen, da er nicht auf Telefon des Benutzers zugreifen kann. Organisationen, die nicht mit diesem zusätzlichen Schutz Identität hinzufügen sind weitere für Anmeldeinformationen Diebstahl Angriffen, die zur Beschädigung der Daten führen kann.

Eine Alternative für Organisationen, die die Authentifizierung Steuerelement lokal gespeichert werden sollen sollten [Azure mehrstufige Authentifizierungsserver](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), die Abkürzung MFA lokal verwenden. Mit dieser Methode werden Sie immer noch mehrstufige Authentifizierung, Beibehaltung der MFA Server lokal erzwingen können.

Weitere Informationen zum Azure MFA finden Sie im Artikel [Erste Schritte mit Azure kombinierte Authentifizierung in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC)
Einschränken des Zugriffs basierend auf der [minimalen Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) und [wissen müssen](https://en.wikipedia.org/wiki/Need_to_know) Sicherheit Grundsätze. Dies ist für Organisationen, die Sicherheitsrichtlinien für den Zugriff auf Daten erzwingen möchten. Azure rollenbasierte Access Steuerelement (RBAC) kann das Zuweisen von Berechtigungen für Benutzer, Gruppen und Anwendungen mit einem bestimmten Bereich verwendet werden. Der Bereich eine rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein.

Sie können [integrierte RBAC-Rollen](../active-directory/role-based-access-built-in-roles.md) in Azure Benutzern Berechtigungen zuweisen nutzen. Erwägen Sie *Speicher Konto Mitwirkender* für Cloud-Operatoren, die zum Verwalten von Speicherkonten und *klassischen Speicher Konto* Teilnehmerrolle zum Verwalten von Benutzerkonten klassischen Speicherplatz benötigen. Cloud-Operatoren, die zum Verwalten von virtuellen Computern und Speicher-Konto muss, sollten Sie in der *virtuellen Computern* Teilnehmerrolle hinzu.

Organisationen, die Access-Steuerelement not enforce DO durch Nutzung der Funktionen wie RBAC möglicherweise mehr Berechtigungen als für die Benutzer erforderlich zugewiesen werden. Dies kann zu Daten Kompromisse führen, indem Sie einige Benutzer haben Sie Zugriff auf Daten, die sie im ersten Schritt sollten nicht.

Sie erhalten weitere Informationen zu Azure RBAC, indem Sie im Artikel [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md)lesen.

## <a name="encrypt-azure-virtual-machines"></a>Verschlüsseln von Azure-virtuellen Computern
Für viele Organisationen ist die [Verschlüsselung der Daten statisch sind](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt in Richtung Datenschutz, Compliance und Hoheit Daten. Azure Datenträger Verschlüsselung können IT-Administratoren zum Verschlüsseln der Datenträger für Windows und (Linux Neuerung virtuellen Computern). Azure Datenträger Verschlüsselung nutzt die Branche BitLocker Standardfunktion von Windows und die Funktion DM-Crypt Linux um Lautstärke Verschlüsselung für das Betriebssystem und Festplatten mit den Daten zu ermöglichen.

Sie können Datenträger-Verschlüsselung zum schützen, und schützen Sie Daten, um Ihre Sicherheit in der Organisation und Vorschriften erfüllen Azure nutzen. Organisationen sollten auch Verschlüsselung verwenden, um zu verringern Risiken um unbefugten verwandten Daten zugreifen. Es wird auch empfohlen, dass Sie Laufwerke vor dem Schreiben von vertrauliche Daten zu verschlüsseln.

Vergewissern Sie sich an Ihre virtuellen Computers Datenbestände und Boot Volume verschlüsseln, um Daten in Ihr Konto Azure-Speicher zu schützen. Der Schlüssel für die Verschlüsselung und Kennwörter durch die Nutzung von [Azure Schlüssel Tresor](../key-vault/key-vault-whatis.md)zu schützen.

Berücksichtigen Sie bei Ihrem lokalen Windows-Servern die folgende Verschlüsselung best Practices aus:

- Verwenden von [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) für die Verschlüsselung der Daten
- Wiederherstellungsinformationen in AD DS gespeichert.
- Ist Rücksicht darauf, dass BitLocker-Schlüssel gefährdet, es empfiehlt sich, dass Sie entweder das Laufwerk So entfernen Sie alle Instanzen der BitLocker Metadaten aus dem Laufwerk formatieren oder entschlüsseln und das gesamte Laufwerk erneut verschlüsseln.

Organisationen, die Daten Verschlüsselung do not enforce viel eher, Probleme mit der Datenintegrität, z. B. bösartiger oder nicht autorisierte Benutzer Diebstahl von Daten angezeigt werden soll, und den nicht autorisierten Zugriff auf Daten im Format löschen Konten gefährdet. Neben diese Risiken Unternehmen, die gesetzlichen Vorschriften Industry, müssen als erweisen, sie sind sorgfältig und die richtige Sicherheit Steuerelemente zum Erhöhen der Sicherheit von Daten verwenden.

Sie erhalten weitere Informationen zu Azure Datenträger Verschlüsselung, indem Sie im Artikel [Azure Datenträger Verschlüsselung für Windows und Linux IaaS virtuellen Computern](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Verwenden von Hardware Security Module

Industry Verschlüsselung Lösungen verwenden geheimen Schlüssel zum Verschlüsseln Daten. Daher ist es wichtig, dass diese Schlüssel sicher gespeichert sind. Key-Verwaltung wird ein Bestandteil von Datenschutz, da es zum Speichern von geheimer Schlüsseln, mit denen Daten verschlüsseln genutzt werden.

Azure Datenträger Verschlüsselung verwendet [Azure Schlüssel Tresor](https://azure.microsoft.com/services/key-vault/) , mit deren Hilfe Sie steuern und Verwalten von Schlüssel zur Verschlüsselung und vertrauliche Informationen in Ihrem Abonnement Key Tresor und dabei sicherstellen, dass alle Daten im virtuellen Computern Laufwerke zum Rest in Azure-Speicher verschlüsselt werden. Sie sollten Azure-Taste Tresor Tasten und die Richtlinienverwendung überwacht verwenden.

Es gibt viele Risiken im Zusammenhang mit ohne entsprechende Sicherheit Steuerelemente in Ort, an dem die geheimen Schlüssel schützen, die Verschlüsselung die Daten verwendet wurden. Wenn Angreifer Zugriff auf die geheimen Schlüssel haben, werden sie die Daten entschlüsseln und haben potenziell vertrauliche Informationen zugreifen zu können.

Sie können weitere Informationen zu allgemeinen Empfehlungen für zertifikatverwaltung in Azure den Artikel [Zertifikatverwaltung in Azure: Tipps zur](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Weitere Informationen zur Azure-Taste Tresor finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Mit sicheren Arbeitsstationen verwalten

Da der Großteil der Angriffen adressieren den Endbenutzer, wird der Endpunkt mit einem der primären Punkte Angriffen. Wenn ein Angreifer den Endpunkt gefährdet, kann er die Anmeldeinformationen des Benutzers zum Zugriff auf Daten des Unternehmens genutzt werden. Die meisten Endpunkt Angriffen können die Fakultät nutzen, dass Endbenutzer Administratoren in ihren lokalen Arbeitsstationen sind.

Sie können diese Risiken reduzieren, indem Sie ein sicheres Management Arbeitsstationen verwenden. Es empfiehlt sich, dass Sie eine [Berechtigte Access Arbeitsstationen (PFOTE)](https://technet.microsoft.com/library/mt634654.aspx) verwenden, um die Angriffen in Arbeitsstationen verringern. Diese Arbeitsstationen sicheres Management hilft Ihnen der vorstehend genannten zu verringern Angriffen sicherzustellen ist die Sicherheit Ihrer Daten. Vergewissern Sie sich PFOTE zum Sichern und Sperren des Computers verwendet. Dies ist ein wichtiger Schritt, um hohe Sicherheit zusicherungen für vertrauliche Konten, Aufgaben und Datenschutz bereitzustellen.

Fehlende Endpunkt Schutz möglicherweise Anordnen von Daten Risiko, vergewissern Sie sich zum Erzwingen von Sicherheitsrichtlinien auf allen Geräten, mit denen Daten, unabhängig davon, wo Daten (Cloud oder lokal) verwenden.

Erfahren Sie, dass weitere Informationen zu Berechtigungen Arbeitsstationen zugreifen, indem Sie im Artikel [Berechtigten Zugriff Schutz](https://technet.microsoft.com/library/mt631194.aspx)lesen.

## <a name="enable-sql-data-encryption"></a>Aktivieren Sie die Verschlüsselung der SQL-Daten

[Azure SQL-Datenbank als transparent-Verschlüsselung](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) können Sie das Risiko von bösartige Aktivitäten gescannt, indem Sie in Echtzeit und Verschlüsselung von der Datenbank, zugeordneten Sicherungskopien und Transaktionsprotokolldateien bei Rest ohne Änderungen an der Anwendung ausführen.  TDE verschlüsselt die Speicherung eine gesamte Datenbank mithilfe eines symmetrischen Schlüssels des Verschlüsselungsschlüssels Datenbank bezeichnet.

Auch wenn der gesamte Speicher verschlüsselt ist, ist es sehr wichtig, zu Ihrer Datenbank selbst auch verschlüsselt werden. Dies ist eine Implementierung von der Schutz Tiefe und systematisch für den Datenschutz. Wenn Sie [Azure SQL](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) -Datenbank und vertrauliche Daten wie Kreditkarte oder Versicherungsnummer schützen möchten, können Sie Datenbanken mit FIPS 140-2 überprüft 256-Bit-AES-Verschlüsselung verschlüsseln, die die Anforderungen der viele Branchenstandards (z. B. HIPAA, PCI) entspricht.

Es ist wichtig zu verstehen, dass Dateien, die im Zusammenhang mit [Puffer Ressourcenpool Erweiterung](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) nicht verschlüsselt sind, wenn Sie eine Datenbank mithilfe von TDE verschlüsselt ist. Sie müssen die Datei Verschlüsselung auf Systemprogramme wie BitLocker verwenden oder [Datenbankschutz File System](https://technet.microsoft.com/library/cc700811.aspx) (EFS) für BPE verknüpfte Dateien.

Seit einem autorisierten Benutzer wie die Sicherheit zuständiger Administrator oder einen Datenbankadministrator die Daten zugreifen kann, selbst wenn die Datenbank mit TDE, verschlüsselt ist sollten Sie auch die folgenden Vorschläge beachten:

- SQL-Authentifizierung Ebene der Datenbank
- Mit RBAC-Rollen Azure AD-Authentifizierung
- Benutzer und Anwendungen sollten separate Konten zum Authentifizieren verwenden. Auf diese Weise können Sie schränken Sie die Berechtigungen für Anwender und Applications erteilt und Reduzieren von Risiken der bösartige Aktivitäten
- Implementieren Datenbank Sicherheitsstufe mit festen Datenbankrollen (z. B. Db_datareader oder Db_datawriter), oder Sie können benutzerdefinierte Rollen für eine Anwendung ausgewählten Datenbankobjekte explizite Berechtigungen erteilen erstellen.

Organisationen, die keine Verschlüsselung der Datenbank auf verwendet werden möglicherweise mehr Angriffen sein, die die Daten aus den SQL-Datenbanken beeinträchtigen kann.

Sie können weitere Informationen zur SQL TDE Verschlüsselung den Artikel [Als Transparent Daten-Verschlüsselung mit Azure SQL-Datenbank](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Schützen von Daten bei der Übertragung

Schützen von Daten bei der Übertragung sollten wesentlichen Bestandteil Ihrer Strategie für den Datenschutz. Da Daten vorwärts und rückwärts von vielen Orten verschoben werden, empfiehlt es sich Allgemein, dass Sie SSL/TLS-Protokolle immer verwenden, um Daten über die unterschiedlichen Standorten austauschen. In einigen Fällen möchten Sie möglicherweise den gesamten Kommunikationskanal zwischen Ihrem lokalen und Cloud isolieren Infrastruktur mithilfe eines virtuellen privaten Netzwerks (VPN).

Für die Daten zwischen Ihrem lokalen Infrastruktur und Azure verschieben sollten Sie die geeignete Sicherheitsmaßnahmen z. B. HTTPS oder VPN.

Für Organisationen, die den sicheren Zugriff auf mehrere Arbeitsstationen müssen verwenden sich lokal in Azure, [Azure Standort-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Für Organisationen, die den sicheren Zugriff von einem Computer ansässig lokalen in Azure, müssen verwenden [Punkt-zu-Standort VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Größere Datensätze können über eine dedizierte schnelle WAN verschoben werden Daten wie [ExpressRoute](https://azure.microsoft.com/services/expressroute/)verknüpfen. Wenn Sie ExpressRoute verwenden, können Sie auch verschlüsseln Sie die Daten Ebene der Anwendung-Schutz mit [SSL/TLS](https://support.microsoft.com/kb/257591) oder andere Protokolle für hinzugefügt.

Wenn Sie die Interaktion mit dem Azure-Speicher über das Portal Azure sind, auftreten, alle Transaktionen über HTTPS. [Speicher REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx) über HTTPS kann auch Interaktion mit [Azure-Speicher](https://azure.microsoft.com/services/storage/) [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)verwendet werden.

Organisationen, die nicht zum Schützen von Daten bei der Übertragung sind weitere für [Mann in zweiter Angriffen](https://technet.microsoft.com/library/gg195821.aspx), [Abhören](https://technet.microsoft.com/library/gg195641.aspx) und Sitzung Missbrauch. Diesen Angriffen können Sie zunächst den Zugriff auf vertrauliche Daten werden.

Sie erhalten weitere Informationen zu Azure VPN-Option, indem Sie im Artikel [Planen und Design für VPN-Gateway](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Erzwingen der Datei Ebene-Verschlüsselung

Eine zusätzliche Schutzebene, die die Sicherheitsebene für Ihre Daten erhöhen können, ist die Datei selbst, unabhängig von den Dateispeicherort verschlüsseln.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) verwendet Verschlüsselung, Identität und Autorisierung Richtlinien so sichern Sie Ihre Dateien und die e-Mail an. Azure RMS funktioniert über mehrere Geräte – Telefone, Tablets und PCs durch den Schutz sowohl innerhalb Ihrer Organisation und außerhalb Ihrer Organisation. Diese Funktion ist möglich, da Azure RMS Schutzebene hinzu, die mit den Daten, bleibt, selbst wenn sie Ihrer Organisation Begrenzung belässt.

Wenn Sie Azure RMS verwenden, um Ihre Dateien zu schützen, verwenden Sie Industriestandard-Verschlüsselung mit vollständiger Unterstützung von [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Wenn Sie für den Datenschutz Azure RMS nutzen, können Sie die Sicherstellung, die der Schutz mit der Datei, bleibt haben, auch wenn sie auf Speicher kopiert wird, der nicht unter der Verwaltung von IT, wie etwa einen Cloud-Service-Speicher. Dasselbe passiert per E-mail, freigegebene Dateien die Datei als Anlage an eine e-Mail-Nachricht mit Anweisungen geschützt ist wie die geschützte Anlage zu öffnen.

Beim Planen von Azure RMS Annahme empfohlen Folgendes:

- Installieren der [RMS Freigabe app](https://technet.microsoft.com/library/dn339006.aspx). Diese app integriert mit Office Applikationen durch die Installation einer Office-add-Ins, sodass die Benutzer Dateien einfach direkt schützen können.
- Konfigurieren von Anwendungen und Dienste zur Unterstützung von Azure RMS
- Erstellen von [benutzerdefinierten Vorlagen](https://technet.microsoft.com/library/dn642472.aspx) , die Ihren Anforderungen Business wiedergeben. Beispiel: e-Mails im Zusammenhang einer Vorlage für die obersten geheimen Daten, die in der obersten geheim angewendet werden soll.

Organisationen, die auf [Klassifizierung von Daten](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) und Dateischutz schwachen sind möglicherweise weitere Daten sein. Ohne die richtige Dateischutz können Organisationen nicht mehr Business Einsichten zu erhalten, auf Schäden überwachen und bösartiger Zugriff auf Dateien zu verhindern.

Sie erhalten weitere Informationen zu Azure RMS, indem Sie im Artikel [Erste Schritte mit der Verwaltung von Informationsrechten Azure](https://technet.microsoft.com/library/jj585016.aspx)lesen.
