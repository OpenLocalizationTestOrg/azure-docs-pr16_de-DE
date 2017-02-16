<properties
   pageTitle="Azure Active Directory reporting - Vorschau | Microsoft Azure"
   description="Listet die verschiedenen verfügbaren Berichte für Azure Active Directory-Vorschau"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory reporting - Vorschau

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-reporting-azure-portal.md)
- [Azure klassischen-portal](active-directory-reporting-guide.md)

*Diese Dokumentation ist Teil des [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

Mit Berichte in der Vorschau Azure Active Directory erhalten Sie alle Informationen, die Sie müssen ermitteln, wie Ihre Umgebung schlagen ist. [Was ist in der Vorschau?](active-directory-preview-explainer.md)

Es gibt zwei Hauptbereichen von Berichten:

- **Anmeldung Aktivitäten** – Informationen über die Verwendung von verwalteten Applikationen und Benutzeraktivitäten Anmeldung

- **Überwachungsprotokolle** - Aktivität Systeminformationen zu Benutzer und Verwaltung von Gruppen, Ihren verwalteten Applikationen und Directory Aktivitäten

Je nach dem Bereich der Daten, die Sie suchen, können Sie diese Berichte entweder, indem Sie auf **Benutzer und Gruppen** oder **Enterprise-Anwendungen** in der Liste der Dienste im [Azure-Portal](https://portal.azure.com)zugreifen.

## <a name="sign-in-activities"></a>Anmeldung Aktivitäten

### <a name="user-sign-in-activities"></a>Anmeldung Benutzeraktivitäten

Mit den Informationen, die vom Benutzer anmelden Bericht bereitgestellt wird finden Sie Antworten auf Fragen wie:

- Was ist die Anmeldung Muster eines Benutzers?
- Wie viele Benutzer müssen Benutzer über eine Woche angemeldet?
- Was ist der Status der diese Add-Ins melden Sie sich?

Der Einstiegspunkt auf diese Daten ist der Benutzer anmelden Diagramm im Abschnitt **Übersicht** unter **Benutzer und Gruppen**.

 ![Reporting] (./media/active-directory-reporting-azure-portal/05.png "Reporting")

Benutzer anmelden Grafik können Sie entnehmen wöchentliche Aggregationen des Anmeldefensters ins für alle Benutzer in einem bestimmten Zeitraum. Das Standardformat für den Zeitraum beträgt 30 Tage.

![Reporting] (./media/active-directory-reporting-azure-portal/02.png "Reporting")

Wenn Sie an einem Tag im Diagramm Anmeldung klicken, erhalten Sie eine ausführliche Liste der Aktivitäten Anmeldung.

![Reporting] (./media/active-directory-reporting-azure-portal/03.png "Reporting")

Jede Zeile in der Liste Anmeldung Aktivitäten erhalten Sie ausführliche Informationen zum ausgewählten Anmeldung wie Folgendes:

- Wer angemeldet hat?

- Welchen Anteil der zugehörigen UPN?

- Welche Anwendung wurde das Ziel der Anmeldung?

- Was ist die IP-Adresse von der Anmeldung?

- Welchen Anteil der Status der Anmeldung?

### <a name="usage-of-managed-applications"></a>Verwendung von verwalteten Clientanwendungen

Mit einer Anwendung reduzierte Ansicht Ihrer Daten anmelden können Sie Fragen beantworten:

- Wer ist Meine Programme verwenden?

- Was sind die oberen 3-Anwendungen in Ihrer Organisation?

- Ich haben sich eine Anwendung zuletzt bereitgestellt. Wie wird es schlagen?


Der Einstiegspunkt für diese Daten ist im oberen 3 Applications in Ihrer Organisation innerhalb der letzten 30 Tage Bericht im Abschnitt **Übersicht** unter **Enterprise Applications**.

 ![Reporting] (./media/active-directory-reporting-azure-portal/06.png "Reporting")


Die app Verwendung Graph wöchentlichen Aggregationen von einen bestimmten Zeitraum ins für Ihre oberen 3 Applikationen anmelden. Das Standardformat für den Zeitraum beträgt 30 Tage.

![Reporting] (./media/active-directory-reporting-azure-portal/78.png "Reporting")

Wenn Sie möchten, können Sie den Fokus auf eine bestimmte Anwendung festlegen.

![Reporting] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Reporting")


Wenn Sie an einem Tag in der app-Verwendung: Grafik klicken, erhalten Sie eine ausführliche Liste der Aktivitäten Anmeldung.


![Reporting] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Reporting")



Die Option **Signieren-ins** gibt Ihnen einen Überblick über alle Ereignisse abgeschlossen den Clientanwendungen.

![Reporting] (./media/active-directory-reporting-azure-portal/85.png "Reporting")

Mithilfe der Spaltenauswahl können Sie die Datenfelder auswählen, die angezeigt werden soll.

![Reporting] (./media/active-directory-reporting-azure-portal/column_chooser.png "Reporting")



### <a name="filtering-sign-ins"></a>Melden Sie sich-ins filtern

Sie können Sign-ins durch ein Zeitintervall beschränken Sie den Umfang der angezeigten Daten filtern.

![Reporting] (./media/active-directory-reporting-azure-portal/927.png "Reporting")


Eine andere Methode zum Filtern der Einträge Anmeldung Aktivitäten ist für bestimmte Einträge suchen.
Search-Methode können Sie Ihre Sign-ins um bestimmte **Benutzer**, **Gruppen** oder **Anwendungen**Bereich.


![Reporting] (./media/active-directory-reporting-azure-portal/84.png "Reporting")

## <a name="audit-logs"></a>Überwachungsprotokolle

Ü Protokollen in Azure Active Directory können Datensätze Systemaktivitäten für Compliance.

Es gibt drei Kategorien für verwandte Aktivitäten im Portal Azure Überwachung aus:

- Benutzer und Gruppen   

- Applikationen

- Verzeichnis   


Eine vollständige Liste der Bericht-Aktivitäten überwachen finden Sie in der [Liste der Bericht von Ereignissen](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Der Eintrag, zeigen Sie auf alle ü Daten ist **Überwachungsprotokolle** im Abschnitt **Aktivität** von **Azure Active Directory**.


![Überwachung] (./media/active-directory-reporting-azure-portal/61.png "Überwachung")


Ein Überwachungsprotokoll weist eine Listenansicht, die die Akteuren zeigt (,), die Aktivitäten (was) und welche Ziele.


![Überwachung] (./media/active-directory-reporting-azure-portal/345.png "Überwachung")


Durch Klicken auf ein Element in der Listenansicht, erhalten Sie weitere Details.

![Überwachung] (./media/active-directory-reporting-azure-portal/873.png "Überwachung")




### <a name="users-and-groups-audit-logs"></a>Benutzer und Gruppen Überwachungsprotokolle


Benutzer und Gruppen basierende Überwachungsberichte können Sie Antworten auf Fragen erhalten:

- Welche Arten von Updates wurden angewendet, die Benutzer?

- Wie viele Benutzer geändert wurden?

- Wie viele Kennwörter geändert wurden?

- Was ist ein Administrator im Verzeichnis bereits erledigt?

- Was sind die Gruppen, die hinzugefügt wurden?

- Gibt es Gruppen mit Mitgliedschaft Änderungen?

- Wurden die Besitzer der Gruppe geändert?

- Welche Lizenzen einer Gruppe oder einem Benutzer zugewiesen wurden?


Wenn Sie ü Daten überprüfen, die auf Benutzer und Gruppen beziehen möchten, finden Sie unter **Überwachungsprotokolle** eine gefilterte Ansicht im Abschnitt **Aktivität** von **Benutzern und Gruppen**.


![Überwachung] (./media/active-directory-reporting-azure-portal/93.png "Überwachung")


### <a name="application-audit-logs"></a>Überwachungsprotokolle Anwendung

Mit der Anwendung-basierten Überwachungsberichten, erhalten Sie Antworten auf Fragen wie:

- Was sind die Anwendungen, die hinzugefügt oder aktualisiert werden?

- Was sind die Programme, die entfernt wurden?

- Geändert ein Dienst Prinzip für eine Anwendung hat sich?

- Wurden die Namen von Applications geändert?

- Zustimmung zur Anwendung erteilt?


Wenn Sie ü Daten, die mit Anwendungen verknüpft ist, überprüfen möchten, finden Sie im Abschnitt **Aktivität** der **Enterprise-Anwendungen**eine gefilterte Ansicht unter **Überwachungsprotokolle** .


![Überwachung] (./media/active-directory-reporting-azure-portal/134.png "Überwachung")


### <a name="filtering-audit-logs"></a>Zum Filtern von Überwachungsprotokollen

Sie können einen Bericht durch ein Zeitintervall beschränken Sie den Umfang der angezeigten Daten filtern.

![Überwachung] (./media/active-directory-reporting-azure-portal/324.png "Überwachung")

Ein anderes Verfahren zum Filtern der Posten eines Überwachungsprotokolls wird für bestimmte Einträge gesucht.

![Überwachung] (./media/active-directory-reporting-azure-portal/237.png "Überwachung")

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Azure-Active Directory Reporting Guide](active-directory-reporting-guide.md).
