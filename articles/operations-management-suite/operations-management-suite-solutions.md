<properties
   pageTitle="Lösungen in Vorgänge Management Suite (OMS) | Microsoft Azure"
   description="Lösungen erweitern die Funktionalität der Vorgänge Management Suite (OMS) durch die Bereitstellung verpackt Management Szenarien, die Kunden ihre OMS-Arbeitsbereich hinzugefügt werden können.  Dieser Artikel enthält Details zu wie benutzerdefinierten Lösungen erstellte Kunden und Partner."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Management-Lösungen in Vorgänge Management Suite (OMS) (Preview)

>[AZURE.NOTE]Dies ist über der vorläufigen Dokumentation für Management-Lösung OMS die derzeit in der Vorschau sind.    

Management-Lösungen erweitern die Funktionalität der Vorgänge Management Suite (OMS) durch die Bereitstellung verpackt Management Szenarien, die Kunden ihre Umgebung hinzufügen können.  Zusätzlich zu [Lösungen von Microsoft bereitgestellt](../log-analytics/log-analytics-add-solutions.md)können Partner und Kunden Management-Lösungen für in ihrer eigenen Umgebung verwendet werden oder für Kunden in der Community zur Verfügung gestellt.

## <a name="finding-and-installing-management-solutions"></a>Suchen und Installieren von Management-Lösungen
Es gibt mehrere Methoden zum Suchen und Installieren von Management-Lösungen, wie in den folgenden Abschnitten beschrieben.

### <a name="azure-marketplace"></a>Azure Marketplace
Management-Lösungen von Microsoft bereitgestellt und vertrauenswürdige Partnern aus dem Azure Marketplace Azure-Portal installiert werden können.

1. Melden Sie sich des Azure-Portals an.
2. Wählen Sie im linken Bereich **Weitere Dienste**.
3. Entweder einen Bildlauf nach unten bis zum **Lösungen** , oder geben Sie *Lösungen* in das Dialogfeld " **Filter** ".
4. Klicken Sie auf die Schaltfläche **+ Hinzufügen** .
5. Suchen Sie nach Lösungen, die Sie entweder durch Durchsuchen oder durch Klicken auf die Schaltfläche **Filter** mit der Eingabe in das Feld **Suchen alles um eins** interessiert sind.
6. Klicken Sie auf ein Element Marketplace, um deren detaillierte Informationen anzuzeigen.
4. Klicken Sie auf **Erstellen** , um den Bereich " **Lösung hinzufügen** " zu öffnen.
5. Sie werden auf die erforderlichen Informationen, wie etwa die [OMS Arbeitsbereich und Automatisierung Konto](#oms-workspace-and-automation-account) sowie Werte für alle Parameter in die Lösung aufgefordert werden.
6. Klicken Sie auf **Erstellen** , um die Lösung zu installieren.

### <a name="oms-portal"></a>OMS-Portal
Management-Lösungen, die von Microsoft bereitgestellten möglicherweise aus dem Lösungskatalog im Portal OMS installiert werden.

1. Melden Sie sich mit dem Portal OMS aus.
2. Klicken Sie auf die Kachel **Lösungskatalog** .
2. Erfahren Sie auf der Seite OMS Lösungskatalog jede Lösung zur Verfügung. Klicken Sie auf den Namen der Lösung, die Sie zu OMS hinzufügen möchten.
3. Klicken Sie auf der Seite für die Lösung, die Sie ausgewählt haben, wird die detaillierte Informationen zur Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
4. Eine neue Kachel für die Lösung, die Sie hinzugefügt haben, klicken Sie auf der Übersicht über die angezeigten Seite OMS und Sie wieder verwenden kann, nachdem der OMS-Dienst Ihre Daten verarbeitet werden.

### <a name="azure-quickstart-templates"></a>Schnellstart Azure-Vorlagen
Mitglieder der Community können Management-Lösungen für Azure Schnellstart Vorlagen einreichen.  Sie können diese Vorlagen für eine spätere Installation herunterladen oder prüfen Sie diese Informationen zum [Erstellen Ihrer eigenen Lösungen](#creating-a-solution).

1. Führen Sie die in [OMS Arbeitsbereich und Automatisierung Konto](#oms-workspace-and-automation-account) einen Arbeitsbereich und Konto verknüpfen beschriebenen Schritte aus.
2. Wechseln Sie zur [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/).  
3. Suchen Sie nach einer Lösung, der Sie interessiert.
4. Wählen Sie die Lösung aus den Ergebnissen, um die Details anzuzeigen.
5. Klicken Sie auf die Schaltfläche **in Azure bereitstellen** .
6. Sie werden aufgefordert, Informationen, wie z. B. der Ressourcengruppe und Speicherort sowie Werte für alle Parameter in die Lösung bereitzustellen.
7. Klicken Sie auf **kaufen** , um die Lösung zu installieren.

### <a name="deploy-azure-resource-manager-template"></a>Bereitstellen von Ressourcenmanager Azure-Vorlage
Lösungen, dass Sie aus der Community oder die Sie [selbst erstellen](#creating-a-solution) , werden als Vorlage Ressourcenmanager implementiert und Anfordern eines der standardmäßigen Verfahren zum [Bereitstellen von einer Vorlage können](../resource-group-template-deploy-portal.md).  Beachten Sie, dass vor der Neuinstallation der Lösung, müssen Sie erstellen und [OMS Arbeitsbereich und Automatisierung Konto](#oms-workspace-and-automation-account)verknüpfen.

## <a name="oms-workspace-and-automation-account"></a>OMS Arbeitsbereich und Automatisierung-Konto
Die meisten Management-Lösungen ist ein [Arbeitsbereich OMS](../log-analytics/log-analytics-manage-access.md) Ansichten enthalten und ein [Konto Automatisierung](../automation/automation-security-overview.md#automation-account-overview) Runbooks und zugehörige Ressourcen enthalten erforderlich. Der Arbeitsbereich und Konto muss die folgenden Anforderungen entsprechen.

- Eine Lösung kann nur eine OMS Arbeitsbereich und eine Automatisierung-Konto verwenden.  
- Der Arbeitsbereich OMS und Automatisierung-Konto verwendet werden, indem Sie eine Lösung müssen miteinander verknüpft werden. Ein Arbeitsbereich OMS möglicherweise nur eine Automatisierung-Konto verknüpft sein, und ein Konto Automatisierung kann nur in einem OMS Arbeitsbereich verknüpft werden.
- Um verknüpft sein, muss der OMS-Arbeitsbereich und Automatisierung Konto in derselben Ressourcengruppe und Region.  Die Ausnahme ist ein Arbeitsbereich OMS in ostasiatischen US Region und und Automatisierung Konto in ostasiatischen US 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Erstellen einer Verknüpfung zwischen einer OMS Arbeitsbereich und Automatisierung-Konto
Wie Sie angeben, dass des Arbeitsbereichs OMS und Automatisierung Konto abhängig von der Installationsmethode für Ihre Lösung.

- Wenn Sie eine Microsoft-Lösung über das OMS-Portal installieren, es im aktuellen OMS Arbeitsbereich installiert ist, und kein Konto Automatisierung ist erforderlich.

- Bei der Installation einer Lösung über dem Azure Marketplace Sie für eine OMS Arbeitsbereich und Automatisierung Konto aufgefordert werden, und die Verknüpfung zwischen ihnen für Sie erstellt wird.  

- Für außerhalb der Azure Marketplace-Lösungen müssen Sie die OMS Arbeitsbereich und Automatisierung Konto vor der Neuinstallation der Lösung verknüpfen.  Sie können dies, indem eine Lösung, in dem Azure Marketplace auswählen, die OMS Arbeitsbereich und Automatisierung Konto und ausführen.  Sie müssen nicht die Lösung tatsächlich zu installieren, da der Link erstellt wird, sobald die OMS Arbeitsbereich und Automatisierung Konto ausgewählt sind.  Nachdem die Verknüpfung erstellt wurde, können Sie diese OMS Arbeitsbereich und Automatisierung Konto für eine Lösung verwenden. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Überprüfen die Verknüpfung zwischen einer OMS Arbeitsbereich und Automatisierung-Konto
Sie können die Verknüpfung zwischen einem Arbeitsbereich OMS und ein Automatisierung Konto mit dem folgenden Verfahren überprüfen.

1. Wählen Sie das Konto Automatisierung, Azure-Portal.
2. Führen Sie einen Bildlauf an das Ende der Bereich **Einstellungen** .
3. Ist eines Abschnitts **OMS Ressourcen** im Bereich **Einstellungen** aufgerufen, wird dieses Konto zu einem Arbeitsbereich OMS angefügt sind.
4. Wählen Sie **Arbeitsbereich** , um die Details des Arbeitsbereichs OMS verknüpft mit diesem Konto Automatisierung anzuzeigen.


## <a name="listing-management-solutions"></a>Auflistung Management solutions
Verwenden Sie das folgende Verfahren zu, um die Verwaltung Lösungen in der Arbeitsbereiche für Ihr Azure-Abonnement verknüpft anzuzeigen.

1. Melden Sie sich des Azure-Portals an.
2. Wählen Sie im linken Bereich **Weitere Dienste**.
3. Entweder einen Bildlauf nach unten bis zum **Lösungen** , oder geben Sie *Lösungen* in das Dialogfeld " **Filter** ".
4. Lösungen in allen Ihren Arbeitsbereichen installiert werden aufgelistet.

Beachten Sie, dass Sie nur die Microsoft-Lösungen, die im aktuellen Arbeitsbereich mit dem Portal OMS installiert anzeigen können.

## <a name="removing-a-management-solution"></a>Entfernen einer Lösung
Wenn eine Lösung entfernt wird, werden alle Ressourcen in der Lösung auch entfernt.  

1. Suchen Sie die Lösung im Azure-Portal mit dem Verfahren in [Listing Lösungen](#listing-solutions).
2. Wählen Sie die Lösung, die Sie entfernen möchten.
3. Klicken Sie auf die Schaltfläche **Löschen** .

## <a name="creating-a-management-solution"></a>Erstellen einer Lösung
Vollständige Anleitung für das Erstellen von Lösungen Management sind unter [Erstellen von Lösungen in Vorgänge Management Suite (OMS)](operations-management-suite-solutions-creating.md)verfügbar. 


## <a name="next-steps"></a>Nächste Schritte

- Suche [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates) Beispiele für unterschiedliche Ressourcenmanager Vorlagen.
- Erstellen Sie eigener [Management-Lösungen](operations-management-suite-solutions-creating.md).
 