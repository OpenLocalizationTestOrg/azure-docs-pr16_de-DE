<properties
    pageTitle="Synchronisieren von Azure AD verbinden: optimale Methoden zum Ändern der Standard-Konfigurations | Microsoft Azure"
    description="Enthält bewährte Methoden zum Ändern der Standard-Konfigurations von Azure AD verbinden synchronisieren."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Synchronisieren von Azure AD verbinden: optimale Methoden zum Ändern der Standard-Konfigurations
In diesem Thema dient unterstützte und nicht unterstützte Änderungen für die Synchronisierung Azure AD verbinden zu beschreiben.

Die Konfiguration Azure AD verbinden erstellte erwartungsgemäß "ist" für die meisten Umgebungen, die lokalen Active Directory mit Azure AD synchronisieren. In einigen Fällen ist es jedoch erforderlich, bei einigen Änderungen an einer Konfiguration zu einer bestimmten müssen oder Anforderung erfüllen anzuwenden.

## <a name="changes-to-the-service-account"></a>Änderungen des Dienstkontos
Azure AD verbinden synchronisieren läuft unter einem Dienstkonto vom Assistenten erstellt. Dieses Dienstkonto enthält der Schlüssel für die Verschlüsselung auf die Datenbank verwendeten synchronisieren. Es wird erstellt, mit einem 127 Zeichen lange Kennwort und das Kennwort abläuft nicht festgelegt ist.

- Es ist **nicht unterstützte** ändern oder Zurücksetzen des Kennworts des Dienstkontos. Auf diese Weise löscht die Schlüssel für die Verschlüsselung und der Dienst ist nicht auf die Datenbank zugreifen und kann nicht starten.

## <a name="changes-to-the-scheduler"></a>Änderungen an der scheduler
Beginnend mit der Versionen aus Build 1.1 (Februar 2016) kann der [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) ein anderen synchronisieren Zyklus als den Standardwert 30 Minuten haben konfigurieren.

## <a name="changes-to-synchronization-rules"></a>Änderungen an der von Synchronisierungsregeln
Der Assistent bietet eine Konfiguration, die für die am häufigsten verwendeten Szenarien entwickelt überfüllt ist. Falls Sie Änderungen an der Konfiguration vornehmen möchten, müssen Sie diese Regeln, um eine unterstützte Konfiguration immer noch befolgen.

- Sie können [Attribut Zahlungen ändern](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) , wenn die standardmäßige direkte Attribut Zahlungen nicht für Ihre Organisation geeignet sind.
- Wenn Sie [nicht Fluss ein Attribut möchten](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) und entfernen alle vorhandenen Attributwerte in Azure AD, müssen Sie zum Erstellen einer Regel für dieses Szenario.
- [Deaktivieren einer Regel für unerwünschte synchronisieren](#disable-an-unwanted-sync-rule) , statt sie gelöscht. Eine gelöschte Regel wird bei einer Aktualisierung neu erstellt.
- [Eine Out-of-Box-Regel ändern](#change-an-out-of-box-rule), sollten Sie erstellen Sie eine Kopie der ursprünglichen Regel, und deaktivieren die Out-of-Box-Regel. Der Regel-Editor synchronisieren auffordert und hilft Ihnen.
- Exportieren von Synchronisierungsregeln benutzerdefinierten mit der Synchronisierung Regel-Editor. Der Editor bietet Ihnen ein PowerShell-Skript, die Sie verwenden können, um einfach in einem Szenario zur Wiederherstellung nach neu erstellen.

>[AZURE.WARNING] Die Synchronisierung von Out-of-Box-Regeln aufzustellen ein Fingerabdrucks. Wenn Sie diese Regeln ändern, entsprechen der Fingerabdruck ist nicht mehr. Probleme möglicherweise müssen Sie in der Zukunft, wenn Sie versuchen, eine neue Version von Azure AD verbinden anzuwenden. Nur nehmen Sie vor, die es beschrieben wird wie in diesem Artikel.

### <a name="disable-an-unwanted-sync-rule"></a>Deaktivieren einer Regel für unerwünschte synchronisieren
Löschen Sie eine Regel Out-of-Box synchronisieren nicht. Es wird während der nächsten Aktualisierung neu erstellt.

In einigen Fällen hat der Assistenten zum Installieren eine Konfiguration gefertigt, die für Ihre Suchtopologie nicht funktionieren. Beispielsweise werden, wenn Sie eine Konto-Topologie haben, aber haben Sie das Schema in der Kontengesamtstruktur mit dem Exchange-Schema erweitert, dann Regeln für Exchange für das Konto und Ressource erstellt. In diesem Fall müssen Sie die Synchronisierung Regel für Exchange zu deaktivieren.

![Regel deaktiviert synchronisieren](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

In der Abbildung oben hat der Assistent zum Installieren ein altes Exchange 2003-Schemas in der Kontengesamtstruktur gefunden. Diese Erweiterung Schema wurde hinzugefügt, bevor die Ressourcengesamtstruktur in Fabrikam Umgebung eingeführt wurde. Um sicherzustellen, dass keine Attribute aus der alten Exchange-Implementierung synchronisiert werden, sollte die Regel synchronisieren deaktiviert werden, wie gezeigt.

### <a name="change-an-out-of-box-rule"></a>Ändern einer Out-of-Box-Regel
Wenn Sie eine Regel Out-of-Box ändern müssen, sollten Sie erstellen Sie eine Kopie der Regel Out-of-Box und die ursprüngliche Regel deaktivieren. Nehmen Sie dann die Änderungen an den duplizierten Regel ein. Im Regel-Editor für synchronisieren, wird Sie mit diesen Schritten helfen. Wenn Sie eine Regel Out-of-Box öffnen, werden Sie in diesem Dialogfeld angezeigt:  
![Warnung aus Feld Regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Wählen Sie **Ja,** um eine Kopie der Regel erstellen. Die duplizierte Regel wird geöffnet.  
![Duplizierten Regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Diese duplizierten Regel nehmen Sie alle erforderlichen Änderungen an Umfang, Verknüpfung und Transformationen aus.

## <a name="next-steps"></a>Nächste Schritte

**Themen (Übersicht)**

- [Synchronisieren von Azure AD verbinden: verstehen und Anpassen der Synchronisierung](active-directory-aadconnectsync-whatis.md)
- [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
