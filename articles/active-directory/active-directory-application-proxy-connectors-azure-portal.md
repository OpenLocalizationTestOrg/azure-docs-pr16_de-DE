<properties
    pageTitle="Arbeiten mit Azure AD-Anwendungsproxy Verbinder | Microsoft Azure"
    description="Erläutert, wie der Verbinder in Azure AD-Anwendungsproxy Gruppen erstellen und verwalten."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Veröffentlichen von Applications in separaten Netzwerken und Speicherorte Entwurfsphase Connector - Public Preview-Version

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klassischen-portal](active-directory-application-proxy-connectors.md)


Verbinder Gruppen sind nützlich für verschiedene Szenarios, einschließlich:

- Websites mit mehreren verbundener Rechenzentren. In diesem Fall möchten so viele Datenverkehr innerhalb des Datencenters wie möglich beibehalten, da Cross-Datacenter Links teure und welche langsam sind. Sie können in jedem Datencenter, um nur die Programme, die innerhalb der Datacenter befinden, dienen Verbinder bereitstellen. Dieser Ansatz minimiert Cross-Datacenter Links und bietet eine vollständig transparent für Ihre Benutzer.
- Verwalten von Applications auf isoliert Netzwerke, die nicht Bestandteil der Hauptfenster Unternehmensnetzwerk sind installiert. Connector-Gruppen können Sie um dedizierte Verbinder auf isoliert Netzwerke auch Isolieren von Applications mit dem Netzwerk zu installieren.
- Für Applikationen auf IaaS cloudzugriff installiert bieten Verbinder Gruppen einen allgemeinen Dienst, um den Zugriff auf die Datei apps zu sichern. Verbinder Gruppen nicht zusätzliche Abhängigkeit in Ihrem Netzwerk Ihres Unternehmens erstellen oder die app-Oberfläche fragment. Verbinder auf jeder Cloud Datacenter installiert werden können und dienen nur die Programme, die in diesem Netzwerk befinden. Sie können mehrere Verbinder, um hohe Verfügbarkeit zu erreichen installieren.
- Unterstützung für Umgebungen mit mehreren Gesamtstrukturen, in denen bestimmte Verbinder pro Gesamtstruktur bereitgestellt und zu bestimmte Applikationen dienen festgelegt werden können.
- Verbinder Gruppen können in Wiederherstellung Websites entweder Failover erkennen oder als Sicherung für die wichtigsten Website verwendet werden.
- Verbinder Gruppen können auch verwendet werden, um mehrere Unternehmen von einem einzelnen Mandanten dienen.

## <a name="prerequisite-create-your-connectors"></a>Voraussetzung: Erstellen der Verbinder
Um Ihre Verbinder zu gruppieren, müssen Sie sicherstellen, dass Sie [mehrere Connectors installiert](active-directory-application-proxy-enable.md). Wenn Sie eine neue Verbindung installieren, verknüpft es automatisch die **Standard-** Connector-Gruppe aus.

## <a name="step-1-create-connector-groups"></a>Schritt 1: Erstellen von Gruppen für Verbinder
Sie können beliebig viele Verbinder-Gruppen erstellen. Erstellung der Verbinder wird im [Portal Azure](https://portal.azure.com)erreicht.

1. Wählen Sie aus **Azure Active Directory** , um zu dem Dashboard Management für Ihre Verzeichnis zu wechseln. Wählen Sie von dort aus **Enterprise Applications** > **Anwendungsproxy**.

2. Wählen Sie die Schaltfläche **Verbinder Gruppen** aus. Das neue Verbinder Gruppe Blade wird angezeigt.

3. Benennen Sie der neue Gruppe für die Verbinder, und verwenden Sie dann im Dropdownmenü auswählen, welche Connectors in dieser Gruppe angehören.

4. Wählen Sie **Speichern** aus, nach Abschluss der Verbinder Gruppe.

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Schritt 2: Zuordnen von Applications zu Ihren Connector-Gruppen
Der letzte Schritt darin, wird jede Anwendung zur Gruppe "Verbinder" festlegen, die es dient.

1. Wählen Sie das Management Dashboard für Ihr Verzeichnis, **Enterprise Applications** > **Alle Programme** > der Anwendung, die Sie einer Gruppe Verbinder zuweisen möchten > **Proxy Anwendung**.
2. Verwenden Sie im Dropdownmenü unter **Verbinder Gruppe**um die Gruppe auszuwählen, die Anwendung verwendet werden soll.
3. Wählen Sie **Speichern** , um die Änderung zu übernehmen.


## <a name="see-also"></a>Siehe auch

- [Aktivieren Sie die Anwendungsproxy](active-directory-application-proxy-enable.md)
- [Aktivieren Sie auf einmalige Anmelden](active-directory-application-proxy-sso-using-kcd.md)
- [Aktivieren von bedingten Zugriff](active-directory-application-proxy-conditional-access.md)
- [Behandeln von Problemen, die mit der Anwendungsproxy](active-directory-application-proxy-troubleshoot.md)

Sehen Sie für die neuesten Informationen und Updates sich die [Anwendungsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
