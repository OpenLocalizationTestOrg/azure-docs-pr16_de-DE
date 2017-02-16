<properties
   pageTitle="Verwalten von berechnen Power in Azure SQL-Data Warehouse (Azure Portal) | Microsoft Azure"
   description="Azure Portals Aufgaben zum Verwalten von Power zu berechnen. Maßstab berechnen Ressourcen durch DWUs anpassen. Oder anhalten und fortsetzen Ressourcen zum Speichern von Kosten zu berechnen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Verwalten von berechnen Power in Azure SQL-Data Warehouse (Azure Portal)

> [AZURE.SELECTOR]
- [(Übersicht)](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skalierung der Leistung durch Skalierung berechnen, Ressourcen und Speicher zum Ändern der auf die Bedürfnisse Ihrer Arbeitsbelastung entsprechen. Speichern von Kosten durch Skalierung zurück Ressourcen während nicht Höchstwert Zeiten oder Anhalten berechnen ganz. 

Diese Sammlung von Aufgaben verwendet im Azure-Portal an:

- Maßstab berechnen
- Anhalten berechnen
- Berechnen des Lebenslaufs

Weitere Informationen finden Sie unter [Übersicht zu berechnen verwalten][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Maßstab berechnen power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Berechnen von Ressourcen zu ändern:

1. Öffnen Sie das [Azure-Portal][], öffnen Sie Ihre Datenbank, und klicken Sie auf **Skalieren**.

    ![Klicken Sie auf skalieren][1]

1. Bewegen Sie in der Blade-Farben-Skala den Schieberegler nach links oder rechts, um die Einstellung DWU ändern.

    ![Schieberegler][2]

1. Klicken Sie auf **Speichern**. Es wird eine bestätigungsmeldung angezeigt. Klicken Sie auf **Ja,** um zu bestätigen oder **keine** um den Vorgang abzubrechen.

    ![Klicken Sie auf Speichern][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten berechnen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

So beenden Sie eine Datenbank:

1. Öffnen Sie das [Azure-Portal][] an, und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status **Online**ist. 

    ![Onlinestatus][6]

1. Berechnen und Speicherkapazität Ressourcen anzuhalten, klicken Sie auf **Anhalten**, und klicken Sie dann eine bestätigungsmeldung angezeigt wird. Klicken Sie auf **Ja,** um zu bestätigen oder **keine** um den Vorgang abzubrechen.

    ![Bestätigen Sie anhalten][7]

1. Während der SQL Data Warehouse die Datenbank gestartet wird, ist der Status **Anhalten**.
2. Wenn der Status " **angehalten**" ist das Anhalten ausgeführt wird und Sie sind nicht mehr für DWUs belastet wird.

    ![Anhalten status][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Berechnen des Lebenslaufs

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Um eine Datenbank fortzusetzen:

1. Öffnen Sie das [Azure-Portal][] an, und öffnen Sie Ihre Datenbank. Beachten Sie, dass der Status " **angehalten**" ist. 

    ![Anhalten-Datenbank][4]

1. Zum Fortsetzen die Datenbank klicken Sie auf **Start**, und klicken Sie dann eine bestätigungsmeldung wird angezeigt. Klicken Sie auf **Ja,** um zu bestätigen oder **keine** um den Vorgang abzubrechen.

    ![Bestätigen Sie Lebenslauf][5]

1. Während der SQL Data Warehouse die Datenbank gestartet wird, ist der Status "Fortsetzen".
2. Wenn der Status **online**ist, ist die Datenbank bereit.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Übersicht über die Verwaltung][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Verwaltung (Übersicht)]: ./sql-data-warehouse-overview-manage.md
[Verwalten von berechnen (Übersicht)]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure-portal]: http://portal.azure.com/
