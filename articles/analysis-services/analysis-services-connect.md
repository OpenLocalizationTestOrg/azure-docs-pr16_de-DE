<properties
   pageTitle="Abrufen von Daten aus Azure Analysis Services | Microsoft Azure"
   description="Informationen Sie zum Herstellen einer Verbindung mit und Abrufen von Daten aus einem Analysis Services-Server in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Abrufen von Daten aus Azure Analysis Services
Nachdem Sie einen Server in Azure erstellt und ein Tabellenmodell bereitgestellt, damit haben, können Benutzer in Ihrer Organisation verbinden und Untersuchen von Daten zu beginnen.

Azure Analysis Services unterstützt Clientverbindungen mit [Objektmodelle aktualisiert](#client-libraries). TOM, AMO, Adomd.Net oder MSOLAP, eine Verbindung über Xmla auf dem Server herstellen. Beispielsweise Power BI, Power BI-Desktop, Excel oder einer beliebigen Drittanbieter-Clientanwendung, die die Objektmodelle unterstützt.

## <a name="server-name"></a>Servername
Wenn Sie einen Analysis Services-Server in Azure erstellen, geben Sie einen eindeutigen Namen und den Bereich, in dem der Server ist, erstellt werden. Wenn Sie den Servernamen in eine Verbindung angeben, wird das naming Server-Schema:
```
<protocol>://<region>/<servername>
```
 Protokoll Zeichenfolge **Asazure**ist, ist der Bereich der Uri des Bereichs, in dem der Server erstellt wurde (z. B. für Westen US, westus.asazure.windows.net) und Servername ist der Name Ihres eindeutigen Servers innerhalb des Bereichs liegen.

## <a name="get-the-server-name"></a>Erhalten Sie den Namen des Servers
Bevor Sie eine Verbindung herstellen, müssen Sie den Namen des Servers zu erhalten. **Azure** -Portal > Server > **Übersicht** > **Servernamen**, kopieren Sie den gesamten Servernamen. Wenn andere Benutzer in Ihrer Organisation auf diesem Server zu verbinden, sollten Sie diese Servernamen mit ihnen gemeinsam nutzen zu können. Wenn Sie einen Servernamen angeben, muss der gesamte Pfad verwendet werden.

![Abrufen von Servernamen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>In Power BI-Desktop verbinden

> [AZURE.NOTE] Dieses Feature ist Vorschau an.

1. In [Power BI-Desktop](https://powerbi.microsoft.com/desktop/), klicken Sie auf **Daten abrufen** > **Datenbanken** > **Azure Analysis Services**.

2. **Server**fügen Sie den Servernamen aus der Zwischenablage.

3. In der **Datenbank**Wenn Sie, den Namen der tabellarischen Modelldatenbank oder Perspektive wissen, um eine Verbindung herstellen möchten, fügen Sie ihn hier. Andernfalls können Sie dieses Feld leer lassen. Sie können eine Datenbank oder eine Perspektive auf dem nächsten Bildschirm auswählen.

4. Lassen Sie die Standardoption **Verbinden live** ausgewählt, und drücken Sie dann **Verbinden**. Wenn Sie aufgefordert werden, um ein Konto einzugeben, geben Sie Ihr organisationskonto.

5. Im **Navigator**erweitern Sie den Server, und klicken Sie dann wählen Sie das Modell oder die Perspektive, um eine Verbindung herstellen möchten, und klicken Sie dann klicken Sie auf **Verbinden**. Ein einzelner Klick auf ein Modell oder eine Perspektive zeigt alle Objekte für diese Ansicht.


## <a name="connect-in-power-bi"></a>Verbinden in Power BI
1. Erstellen einer Power BI-Desktop-Datei, die eine aktive Verbindung zur Modell auf dem Server hat.

2. In [Power BI](https://powerbi.microsoft.com), klicken Sie auf **Daten abrufen** > **Dateien**. Suchen nach, und wählen Sie die Datei aus.


## <a name="connect-in-excel"></a>Verbinden Sie in Excel
Herstellen einer Verbindung mit Azure Analysis Services-Server in Excel wird mithilfe von Daten in Excel 2016 abrufen oder Power Query in früheren Versionen unterstützt. [Msolap.7 Anbieter](https://aka.ms/msolap) ist erforderlich. Herstellen einer Verbindung mit den Import-Assistenten in PowerPivot wird nicht unterstützt.

1. Klicken Sie in Excel 2016, klicken Sie im Menüband **Daten** auf **Externe Daten abrufen** > **Aus anderen Quellen** > **Von Analysis Services**.

2. Fügen Sie im Feld **Servername**den Datenverbindungs-Assistenten den Servernamen aus der Zwischenablage. Wählen Sie dann **Anmeldeinformationen**, **Verwenden Sie den folgenden Benutzernamen und Ihr Kennwort ein**, und geben Sie den organisationsinterne Benutzernamen, beispielsweise nancy@adventureworks.com, und Ihr Kennwort ein.

    ![In Excel-Anmeldung verbinden](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. Klicken Sie in **Datenbank und Tabelle wählen**wählen Sie die Datenbank und Modell oder Perspektive, und klicken Sie dann auf **Fertig stellen**.

    ![In Excel select Modell verbinden](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Verbindungszeichenfolge
Bei einer Verbindung mit Azure Analysis Services Formate über das tabellarischen Objektmodell, verwenden Sie die folgende Verbindungszeichenfolge:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrierte Azure Active Directory-Authentifizierung
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Integrierte Authentifizierung übernehmen Azure Active Directory-Anmeldeinformationscache falls vorhanden. Wenn dies nicht der Fall ist, wird das Azure-Anmeldefenster angezeigt.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure-Active Directory-Authentifizierung mit Benutzername und Kennwort
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Client-Bibliotheken
Beim Herstellen einer Verbindung mit Azure Analysis Services aus Excel oder andere Schnittstellen TOM, AsCmd, ADOMD.NET, müssen Sie möglicherweise installieren die neuesten Anbieter-Client-Bibliotheken. Erhalten Sie die neuesten an:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Nächste Schritte
[Verwalten Sie Ihre server](analysis-services-manage.md)
