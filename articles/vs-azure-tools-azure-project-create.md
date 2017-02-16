<properties
   pageTitle="Erstellen eines Azure-Projekts mit Visual Studio | Microsoft Azure"
   description="Erstellen eines Azure-Projekts mit Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Erstellen eines Azure-Projekts mit Visual Studio

Azure-Tools für Visual Studio bereitstellen eine Vorlage, in dem Sie einen Clouddienst zur Azure erstellen kann. Mit den Tools können auch mit Ihnen die Konfiguration, Debuggen und Bereitstellen von Cloud-Dienst für Azure.

Eine Azure-Cloud-Service-Lösung enthält die folgenden Arten von Projekten:

- **Azure-Projekt**

    Die Azure Project gelten Zuordnungen auf die Rolle Projekte in der Lösung. Darüber hinaus ermöglicht die Dienstdefinition und Service-Konfigurations-Dateien. Der Dienst Formulardefinitionsdatei definiert der Laufzeit Einstellungen für Ihre Anwendung einschließlich, welche Rollen erforderlich sind, die Endpunkte und die Größe des virtuellen Computers. Konfigurationsdatei für den Dienst konfiguriert wie viele Instanzen von einer Rolle ausgeführt werden und die Werte der Einstellungen für eine Rolle definiert. Weitere Informationen zu diesen Einstellungen, finden Sie unter [So: Konfigurieren Sie die Rollen für einen Azure-Cloud-Dienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Die Rolle Project Web**

    Eine Worker-Rolle führt Hintergrund Verarbeitung aus. Eine Worker-Rolle kann mit den Diensten von Speicherplatz und andere internetbasierte Dienste kommunizieren. Eine Worker-Rolle kann eine beliebige Anzahl von HTTP, HTTPS oder TCP Endpunkte haben.

    - **ASP.NET Webrolle**beim Erstellen einer Anwendungs ASP.NET mit einem Web-front-end
    - **ASP.NET MVC5 Webrolle**
    - **ASP.NET MVC4 Webrolle**
    - **ASP.NET MVC3 Webrolle**
    - **WCF Service Webrolle**zum Erstellen eines WCF-Diensts
    - **Silverlight Business Anwendung Webrolle** (erfordert Visual Studio 2012)

- **Cache Worker-Rolle**

    Eine Rolle, die einen dedizierten Cache an Ihrer Anwendung bereitstellt.

- **Worker-Rolle mit Service Bus Warteschlange**

    Service Bus Warteschlange, die Nachricht Warteschlange Funktionen zur Kommunikation mit dem Worker-Prozess bereitstellt. Weitere Informationen finden Sie unter [So verwenden Sie Bus Servicewarteschlangen](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>So erstellen Sie ein Azure-Cloud-Service-Projekt in Visual Studio

1. Starten Sie Microsoft Visual Studio als Administrator an.

1. Wählen Sie in der Menüleiste **Datei**, **neu**, **Projekt**aus.

1. Klicken Sie im Bereich **Projekttypen** wählen Sie die **Cloud** Knoten Vorlage aus dem Visual C#- oder Visual Basic-Projekt aus.

1. Wählen Sie im Bereich **Vorlagen** **Azure-Cloud-Dienst**aus.

1. Geben Sie an, welche Version von .NET Framework entwickeln von Projekten verwenden möchten.

1. Geben Sie einen Namen und Speicherort für das Projekt und einen Namen für die Lösung. Wählen Sie die Schaltfläche **OK** aus.

1. Klicken Sie im Dialogfeld **Neue Azure-Projekt** wählen Sie die Rollen aus, die Sie hinzufügen möchten, und wählen Sie die nach-rechts-Schaltfläche, um sie zu Ihrer Lösung hinzufügen. Sie können beliebig viele Rollen nach Bedarf hinzufügen.

1. Um eine Rolle umbenennen, die Sie zu Ihrem Projekt hinzugefügt haben, zeigen Sie auf die Rolle im Dialogfeld **Neue Azure-Projekt** , und wählen Sie das Symbol **Umbenennen** rechts neben der Rolle. Sie können auch eine Rolle innerhalb Ihrer Lösung umbenennen, nachdem er hinzugefügt wurde.
