<properties
    pageTitle="Azure Government Services | Microsoft Azure"
    description="Stellt und Übersicht über die verfügbaren Dienste in Azure Government"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc" />


#  <a name="security"></a>Sicherheit

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Grundsätze für die Kundendaten in Azure Government Schutz

Azure Government bietet es sich um eine Reihe von Features und Dienste, mit denen Sie Cloudlösungen Ihren Anforderungen geregelt/gesteuert Daten erstellen. Eine Lösung kompatiblen Programm ist nicht mehr als die effektive Implementierung von Out-of-Box-Government Azure-Funktionen, in Verbindung mit einer Volltonfarbe Daten aus Sicherheitsgründen.

Wenn Sie eine Lösung in Azure Government gehostet, übernimmt Microsoft zahlreiche diese Anforderungen Ebene Infrastruktur der Cloud.

Das folgende Diagramm veranschaulicht das Azure Verteidigung Modell. Microsoft bietet beispielsweise grundlegende Cloud-Infrastruktur DDOS, zusammen mit den Funktionen für Kunden wie Wertpapiers Einheiten nach Kunden-Anwendung, die DDOS benötigt.

![ALT-text](./media/azure-government-Defenseindepth.png)

Auf dieser Seite werden die grundlegenden Prinzipien zum Sichern Ihrer Dienste und Anwendungen, Bereitstellen von Leitfäden und bewährte Methoden zum Anwenden dieser Grundsätze; Kurzum, wie Kunden nutzen sollten smart Azure Government zu entsprechen, die Verpflichtung und Aufgaben, die für eine Lösung erforderlich sind, die ITAR Informationen behandelt.

 Die übergreifende Prinzipien zum Sichern von Kundendaten sind:

- Schützen von Daten mithilfe von Verschlüsselung
- Verwaltung von vertraulichen Daten
- Grad der Isolation zum Einschränken des Zugriffs auf Daten

###  <a name="protecting-customer-data-using-encryption"></a>Schützen von Kundendaten mithilfe von Verschlüsselung

Risiken minimieren und gesetzliche Vorschriften Besprechung sind die zunehmenden Fokus und Wichtigkeit Daten-Verschlüsselung steuernde. Verwenden Sie eine effektive Verschlüsselung Implementierung aktuelle Sicherheitsmaßnahmen Netzwerk- und verbessern – und das allgemeine Risiko Ihrer Cloud-Umgebung zu verringern.

#### <a name="encryption-at-rest"></a>Verschlüsselung statisch
Die Verschlüsselung von Daten bei Rest gilt für den Schutz von Kunden-Inhalten in Festplattenspeicher frei. Es gibt mehrere Methoden, dies kann, ein:

#### <a name="storage-service-encryption"></a>Speicher-Service-Verschlüsselung

Azure-Speicher-Service-Verschlüsselung ist auf die Speichergrenze Konto aktiviert was blockieren Blobs und Seitenblobs automatisch verschlüsselt werden, wenn in den Azure-Speicher geschrieben. Wenn Sie die Daten aus Azure-Speicher lesen, wird es von der Speicherdienst entschlüsselt werden, vor dem zurückgegeben wird. Verwenden Sie diese Option zum Sichern Sie Ihre Daten ohne zu ändern oder Hinzufügen von Code zu einem beliebigen Applications.

#### <a name="client-side-encryption"></a>Clientseitige Verschlüsselung
Clientseitige Verschlüsselung integriert die Java und .NET Speicher Clientbibliotheken, die Azure-Taste Tresor APIs, wodurch dies einfach implementieren verwenden können. Verwenden Sie Azure-Taste Tresor, um Zugriff auf die vertrauliche Informationen in Azure-Taste Tresor für bestimmte Einzelpersonen mit Azure Active Directory zu erhalten.

#### <a name="encryption-in-transit"></a>Bei der Übertragung Verschlüsselung

Die grundlegende Verschlüsselung verfügbar für eine Verbindung zu Azure Government unterstützt Sicherheit TLS (Transport Level) 1.2 Protokoll und x. 509-Zertifikate. Bundes-Informationen Verarbeitung FIPS (Standard) 140-2 Ebene 1 cryptographic Algorithmen auch für Infrastruktur Netzwerk Verbindungen zwischen Azure Government Rechenzentren verwendet werden.  Windows Server 2012 R2 und Windows Azure-Dateifreigaben und 8-plus virtuellen Computern können SMB 3.0 für Verschlüsselung zwischen den virtuellen Computer und die Dateifreigabe verwenden. Verschlüsseln Sie die Daten ein, bevor sie in Speicher in einer Clientanwendung übertragen werden, und zum Entschlüsseln der Daten dahinter Speicher ausgegangenen Formular clientseitige Verschlüsselung mit.

#### <a name="best-practices-for-encryption"></a>Bewährte Methoden für die Verschlüsselung

- IaaS virtuellen Computern: Verwenden Sie Verschlüsselung Azure Festplatten. Aktivieren Sie die Verschlüsselung der Speicher Service so verschlüsseln Sie die virtuelle Festplatte Dateien, die verwendet werden, um diese Datenträger in Azure-Speicher zu sichern, aber dies verschlüsselt nur Daten, die neu geschriebene. Dies bedeutet, dass, wenn Sie ein virtuellen Computers erstellen und dann Speicher Dienst Verschlüsselung des Speicher-Kontos, der die Datei virtuelle Festplatte aktivieren, werden nur die Änderungen verschlüsselt werden, nicht die Originaldatei virtuelle Festplatte.
- Clientseitige Verschlüsselung: Dies ist die sicherste Methode für Ihre Daten verschlüsseln, da sie es vor der Übertragung verschlüsselt und die Daten statisch sind verschlüsselt. Es ist jedoch erforderlich, dass Sie Code hinzufügen, um Ihre Anwendung mit Speicher, die Sie vielleicht nicht tun möchten. In diesen Fällen können HTTPs für Ihre Daten bei der Übertragung und Speicher-Service-Verschlüsselung Sie zum Verschlüsseln der Daten statisch sind. Clientseitige Verschlüsselung umfasst auch weitere Auslastung der Client – müssen Sie diese in Ihre Pläne Skalierbarkeit berücksichtigt insbesondere dann, wenn Sie verschlüsseln und große Datenmengen übertragen werden.

###  <a name="protecting-customer-data-by-managing-secrets"></a>Schützen von Kundendaten durch die Verwaltung von vertraulichen Daten

Verwalten von sicheren Schlüsseln unbedingt zum Schutz von Daten in der Cloud. Kunden sollten bemüht Key-Verwaltung zu vereinfachen und Kontrolle über die Tasten von Cloudanwendungen und Dienste verwendet, um Daten zu verschlüsseln.

#### <a name="best-practices-for-managing-secrets"></a>Bewährte Methoden für die Verwaltung von vertraulichen Daten

- Verwenden Sie Tresor-Taste, um der Kennwörter verfügbar gemacht werden, bis codierten Konfigurationsdateien, Skripts, oder im Quellcode Risiken minimieren. Azure-Taste Tresor verschlüsselt Tasten (z. B. die Verschlüsselung für die Verschlüsselung der Azure Datenträger) und Kennwörter (zum Beispiel Kennwörter), indem Sie speichern diese im FIPS 140-2 Ebene 2 überprüft Hardware Security Module (HSMs). Für zusätzliche Sicherheit können Sie importieren oder in diese HSMs Schlüssel generieren.
- Anwendungscode und Vorlagen sollten nur URI-Verweise auf das Geheimnis enthalten (d. h., dass die tatsächliche Schlüssel nicht Repositorys Code, Konfiguration oder Quellcode sind). Dadurch wird verhindert, dass Key Phishing-Angriffen auf internen oder externen Repogeschäfte, wie z. B. Ernte-Bots in GitHub.
- Signifikante RBAC Steuerelemente in Schlüssel Tresor zu nutzen. Wenn ein vertrauenswürdiger Operator die Firma oder die Übertragung zu einer neuen Gruppe innerhalb des Unternehmens lässt, sollte diese Weise daran hindern, sich das Geheimnis Zugriff verhindert werden.

Weitere Informationen <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure-Taste Tresor öffentliche Dokumentation.</a>

###  <a name="isolation-to-restrict-data-access"></a>Grad der Isolation zum Einschränken des Zugriffs auf Daten

Grad der Isolation geht mit Begrenzung, Segmentierung und Container Daten den nur autorisierte Benutzer, Dienste und Applikationen beschränken. Die Trennung zwischen Mandanten beträgt beispielsweise eine grundlegende Sicherheit-Methode zum mandantenfähigen Cloud Plattformen wie Microsoft Azure. Logische Isolation verhindert, dass eine Mandanten stören die Vorgänge von einem beliebigen anderen Mandanten.

#### <a name="environment-isolation"></a>Grad der Isolation Umgebung
Die Government Azure-Umgebung ist eine physische Instanz, die von den Rest der Microsoft Netzwerk getrennt ist. Dies wird durch eine Reihe von physischen und logischen Steuerelemente erreicht, die Folgendes enthalten:

- Schützen der physische Hindernisse biometrische Geräte und Kameras verwenden.
- Verwenden von bestimmter Anmeldeinformationen und die kombinierte Authentifizierung von Microsoft Personal logischen Zugriff auf dieser Umgebung benötigen.
- Alle Service-Infrastruktur für Azure Government befindet sich innerhalb der USA.

#### <a name="per-customer-isolation"></a>Grad der Isolation pro Kunden
Azure implementiert Access steuern und Trennung bis VLAN Isolation, ACLs, laden Balancers und IP-Filter

Kunden können ihre Ressourcen weiter über Abonnements, Ressourcengruppen, virtuelle Netzwerke und Subnetze eingrenzen.

## <a name="screening"></a>Ausblenden

Die Option zuletzt bekannt gegebenen FedRAMP hoch und Verteidigungsministerium (DoD) Einfluss Ebene 4 Akkreditierung. Dies weist die Sicherheits- und Compliance-Leiste über der Umgebung Azure Government ausgelöst.

Wir werden nun alle unsere Operatoren am nationalen Stelle überprüfen mit Recht und Kreditkarte (NACLC) ausblenden, wie im Abschnitt 5.6.2.2 von der DoD Cloud Computing Sicherheit Anforderungen Leitfaden (SRG) definiert:

>[AZURE.NOTE] Die minimale Hintergrund Untersuchung erforderlich für CSP Personal Zugang zu Ebene 4 und 5 Informationen basierend auf einer "nicht kritische sensible" (z. B. des DoD ADP-2) ist ein nationalen Stelle erkundigen Recht und Kreditkarte (NACLC) (für "nicht kritische sensible" Vertragsnehmer) oder eine mittlere Risiko Hintergrund Untersuchung (MBI) für die Angabe einer "moderieren Risiko" Position.

In der folgenden Tabelle sind die unsere aktuellen Prüfung für Azure Government Operatoren zusammengefasst:

Azure Gov Filmvorführungen ab und Hintergrund Prüfungen | Beschreibung|
---|---|
US-Engagement |Überprüfung der US-Engagement.
Microsoft Cloud Hintergrund Kontrollkästchen (alle zwei Jahre)|Sozialversicherungsnummer suchen, strafrechtliche Verlauf überprüfen, Office of Fremdschlüssel Posten Control Liste (OFAC), Liste der Bureau of Industry und Sicherheit (BIS), Office von Schutz Trade Steuerelemente Debarred Personen Liste.
Nationale Stelle Kontrollkästchen mit Recht und Kreditkarte (NACLC) (alle fünf Jahre) | Fügt Fingerabdrucks Hintergrund Prüfung gegen FBI Datenbanken hinzu. Zusätzliche Informationen wechseln Sie zu der<a href="https://www.opm.gov/investigations/background-investigations/federal-investigations-notices/1997/fin97-02/"> Website für Office Personal Management</a>. | 
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/CJIS">Strafjustiz Information Services (CJIS)</a> | CJIS ist ein Status, die lokale und FBI Government ausblenden welche Prozesse Fingerabdrucks Einträge und überprüft strafrechtlichen Verläufe auf Betrieb Mitarbeiter Zugriff auf wichtige Strafjustiz Informationen (CJI) Daten bereitgestellt werden konnte.  Jeder Zustand bedeutet eigene Hintergrund überprüfen und nachfolgende Genehmigung aller Mitarbeiter mögliche Zugriff auf CJI.|

Für Azure Mitarbeiter gelten die folgenden Access Grundsätze:

- Aufgaben werden mit separaten Zuständigkeiten für das anfordern, genehmigen und Bereitstellen von Änderungen klar definiert.
- Zugriff erfolgt über definierte Schnittstellen, die bestimmten Funktionen zur Verfügung.
- Zugriff ist von in-Time (JIT), und nur auf der Basis pro Vorfall oder für eine bestimmte Wartung Ereignis und immer für eine bestimmte Dauer gewährt.
- Der Zugriff ist Regel-basierten, mit definierten Rollen, die nur die zur Behandlung dieses Problems erforderlichen Berechtigungen zugewiesen sind.

Prüfung-Standards umfassen die Überprüfung des US-Engagement aller Microsoft Support und Betrieb Personal, vor dem Zugriff auf Systeme Azure Government gehostete gewährt wird. Support in Verbindung, Daten zu übertragen müssen, verwenden die sicheren Funktionen innerhalb von Azure Government. Sichere Datenübertragung erfordert einen separaten Satz von Anmeldeinformationen für die Authentifizierung zugreifen. Angenommen, für den Zugriff auf Systemmetadaten Mitarbeiter verwenden bestimmte webbasierten internen Verwaltungstools, schreibgeschützt APIs und JIT Erhöhung.

## <a name="next-steps"></a>Nächste Schritte

Für zusätzliche Informationen und Updates Bitte abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>
