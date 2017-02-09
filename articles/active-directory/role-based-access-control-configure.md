<properties
    pageTitle="Verwenden von Access rollenbasierte Control Azure-Portal | Microsoft Azure"
    description="Erste Schritte mit Access-Steuerelement im Portal Azure rollenbasierte in Access-Verwaltung. Verwenden Sie zum Zuweisen von Berechtigungen zu Ressourcen rollenzuweisungen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Verwenden von rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Abonnementressourcen Azure

> [AZURE.SELECTOR]
- [Verwalten des Zugriffs nach Benutzer oder Gruppe](role-based-access-control-manage-assignments.md)
- [Verwalten des Zugriffs von Ressourcen](role-based-access-control-configure.md)

Azure rollenbasierte Access Steuerelement (RBAC) ermöglicht die Verwaltung von abgestimmte Zugriff für Azure. Verwenden RBAC, können Sie nur der Menge von Access erteilen, dass Benutzer für ihre Aufgaben benötigen. In diesem Artikel soll Ihnen den Einstieg und die Verwendung mit RBAC Azure-Portal zu erzielen. Wenn Sie möchten weitere Details wie RBAC Zugriff verwalten kann, finden Sie unter [Was ist rollenbasierte Access Control](role-based-access-control-what-is.md).

## <a name="view-access"></a>Anzeigen von access
Sie sehen, wer eine Ressource, Ressourcengruppe oder Abonnement aus deren Hauptfenster Blade im [Azure-Portal](https://portal.azure.com)zugreifen können. Angenommen, möchten wir anzeigen, wer auf eine unsere Ressourcengruppen zugreifen kann:

1. Wählen Sie in der Navigationsleiste auf der linken Seite **Ressourcengruppen** aus.  
    ![Ressourcengruppen - Symbol](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Wählen Sie den Namen der Ressourcengruppe aus dem Blade **Ressourcengruppen** ein.
3. Wählen Sie aus dem linken Menü **Steuerung des Benutzerzugriffs (IAM)** aus.  
4. Mit dem Bibliothekssteuerungsblade Access Listet alle Benutzer, Gruppen und Anwendungen, die der Ressourcengruppe der Zugriff gewährt wurde.  

    ![Benutzer Blade - zugewiesen vererbte im Vergleich mit einer Access-screenshot](./media/role-based-access-control-configure/view-access.png)

Beachten Sie, dass einige Benutzer **zugewiesen** wurden während andere **vererbt** darauf zugreifen. Access ist speziell für die Ressourcengruppe zugewiesen oder aus einer Zuordnung in das Abonnement übergeordneten geerbter.

> [AZURE.NOTE] Klassische Abonnement-Administratoren und co-Administratoren werden Besitzer des Abonnements in das neue RBAC Modell berücksichtigt.


## <a name="add-access"></a>Hinzufügen des Zugriffs
Sie erteilen Zugriff von innerhalb der Ressource, Ressourcengruppe oder Abonnement, die den Bereich der Rolle Zuordnung ist.

1. Wählen Sie auf der Access-Bibliothekssteuerungsblade **Hinzufügen** aus.  
2. Wählen Sie die Rolle aus, die Sie aus dem Blade **Wählen Sie eine Rolle** zuweisen möchten.
3. Wählen Sie die Benutzer, die Gruppe oder die Anwendung in Ihrem Verzeichnis, dem Sie Zugriff gewähren möchten. Sie können mit Anzeigenamen, e-Mail-Adressen und Objektbezeichnern Verzeichnis suchen.  

    ![Hinzufügen von Benutzern Blade – Screenshot suchen](./media/role-based-access-control-configure/grant-access2.png)

4. Wählen Sie **OK** , um die Aufgabe zu erstellen. Das **Hinzufügen von Benutzer** Popup zeigt den Verlauf an.  
    ![Hinzufügen von Benutzer Statusanzeige - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)

Nachdem eine rollenzuweisung erfolgreich hinzugefügt haben, wird es auf dem **Benutzer** -Blade angezeigt.

## <a name="remove-access"></a>Zugriff muss entfernt werden

1. Wählen Sie die Rolle Zuordnung auf die Access-Bibliothekssteuerungsblade aus.
2. Wählen Sie in der Zuordnung Details Blade **Entfernen** .  
3. Wählen Sie **Ja** , entfernen zu bestätigen.  
    ![Benutzer Blade - aus Rolle Screenshot entfernen](./media/role-based-access-control-configure/remove-access1.png)

Vererbte Zuordnungen können nicht entfernt werden. Beachten Sie, dass die Schaltfläche Entfernen abgeblendet ist in der nachstehenden Abbildung. Prüfen Sie stattdessen die Details **Zugewiesen an** ein. Wechseln Sie zu der Ressource, die es aufgeführt, um die rollenzuweisung zu entfernen.

![Benutzer Blade - entfernen vererbt Access deaktiviert Screenshot der Schaltfläche](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Andere Tools zum Verwalten des Zugriffs
Sie können die Rollen zuweisen und Verwalten des Zugriffs mit RBAC Azure-Befehle in andere Tools als Azure-Portal.  Führen Sie die Links, um weitere Informationen zu den erforderlichen Komponenten und erste Schritte mit der Azure RBAC Befehle aus.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure Line-Benutzeroberfläche](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen eines Access-Änderungsverlauf Berichts](role-based-access-control-access-change-history-report.md)
- Finden Sie die [integrierten RBAC-Rollen](role-based-access-built-in-roles.md)
- Definieren Sie eigene [benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md)
