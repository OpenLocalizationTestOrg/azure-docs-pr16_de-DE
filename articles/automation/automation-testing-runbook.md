<properties 
    pageTitle="Testen eine Runbooks in Azure Automatisierung | Microsoft Azure"
    description="Bevor Sie eine Runbooks in Azure Automatisierung veröffentlichen, können Sie testen, um sicherzustellen, dass die wie erwartet funktioniert.  Dieser Artikel beschreibt, wie eine Runbooks testen und deren Ausgabe anzeigen."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Testen eine Runbooks in Azure Automatisierung
Wenn Sie eine Runbooks testen, die [Entwurfsversion](automation-creating-importing-runbook.md#publishing-a-runbook) ausgeführt, und alle Aktionen, die sie ausführt abgeschlossen werden. Keine Historie erstellt, aber die [Ausgabe](automation-runbook-output-and-messages.md#output-stream) und [Warnung und Fehler](automation-runbook-output-and-messages.md#message-streams) Streams werden angezeigt, in der Test Bereich ausgeben. Nur, wenn die [$VerbosePreference Variable](automation-runbook-output-and-messages.md#preference-variables) weiter festgelegt ist, werden Nachrichten in den [ausführlichen Stream](automation-runbook-output-and-messages.md#message-streams) im Ausgabebereich angezeigt.

Obwohl die Entwurfsversion ausgeführt wird, wird den Workflow ausgeführt wird, normalerweise des Runbooks weiterhin und führt Aktionen für Ressourcen in der Umgebung. Daher sollten Sie nur bei nicht Herstellung Ressourcen Runbooks testen.

Die Vorgehensweise zum Testen jedes [Typs Runbooks](automation-runbook-types.md) ist gleich, und es gibt keinen Unterschied zwischen den Text-Editor und den grafischen-Editor im Portal Azure testen.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>So testen Sie eine Runbooks Azure-Portal

Sie können mit einem beliebigen [Typ Runbooks](automation-runbook-types.md) Azure-Portal arbeiten.

1. Öffnen Sie die Entwurfsversion der des Runbooks im [Text-Editor](automation-editing-a-runbook.md#Portal) oder [grafisch Editor](automation-graphical-authoring-intro.md)ein.
2. Klicken Sie auf **Testen** , um das Blade Test zu öffnen.
3. Wenn die Runbooks Parameter verfügt, werden diese im linken Bereich aufgelistet, in dem Sie Werte für den Test zu verwendende bereitstellen können.
4. Wenn der Test auf eine [Hybrid Runbooks Worker](automation-hybrid-runbook-worker.md)ausgeführt werden soll, ändern Sie **Run Settings** in **Hybrid Worker** , und wählen Sie den Namen der Zielgruppe.  Lassen Sie andernfalls die Standardeinstellung **Azure** zum Ausführen des Tests in der Cloud.
5. Klicken Sie auf die Schaltfläche **Start** , um den Test zu starten.
6. Wenn des Runbooks [PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks) oder [Graphical](automation-runbook-types.md#graphical-runbooks)ist, können Sie beenden oder Anhalten sie ihn mit den Schaltflächen unterhalb der Ausgabebereich getestet wird. Wenn Sie die Runbooks anhalten, abgeschlossen vor angehalten wird die aktuelle Aktivität ist. Nachdem des Runbooks unterbrochen wurde, können Sie ihn beenden oder starten Sie ihn erneut.
7. Prüfen Sie die Ausgabe des Runbooks im Ausgabebereich ein.


## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen oder Importieren einer Runbooks, finden Sie unter [Erstellen oder Importieren einer Runbooks in Azure Automatisierung](automation-creating-importing-runbook.md)
- Wenn Sie weitere Informationen zur Erstellung Grafischen finden Sie unter [Graphical authoring in Azure Automatisierung](automation-graphical-authoring-intro.md)
- Um mit PowerShell Workflow Runbooks anzufangen, finden Sie unter [Meine erste PowerShell Workflow Runbooks](automation-first-runbook-textual.md)
- Weitere Informationen zum Konfigurieren von Runboks Status zur Rückkehr finden Sie unter Nachrichten und Fehler, einschließlich empfohlene Vorgehensweisen [Runbooks Ausgabe und Nachrichten in Azure Automatisierung](automation-runbook-output-and-messages.md)