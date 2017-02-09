<properties 
    pageTitle="So konfigurieren Sie einen Cloud-Service (klassische Portal) | Microsoft Azure" 
    description="Informationen Sie zum Azure Cloud Services konfigurieren. Erfahren Sie, aktualisieren die Konfiguration der Cloud-Dienst und Remotezugriff auf Rolleninstanzen konfigurieren." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-configure-cloud-services"></a>So konfigurieren Sie Cloud Services

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-configure-portal.md)
- [Azure klassischen-portal](cloud-services-how-to-configure.md)

Sie können die am häufigsten verwendeten Einstellungen für einen Clouddienst in der klassischen Azure-Portal konfigurieren. Oder, wenn Sie Ihre Konfigurationsdateien direkt aktualisieren möchten, laden Sie eine Konfiguration Dienstdatei zum aktualisieren möchten, und klicken Sie dann Hochladen der aktualisierten Datei und Cloud-Dienst mit den Änderungen Konfiguration aktualisieren. In beiden Fällen sind die Konfiguration Updates für alle Rolleninstanzen ab abgelegt.

Das klassische Azure-Portal können Sie auch [Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md) aktivieren

Azure kann nur 99,95 Prozent Verfügbarkeit während der Konfiguration Updates sicherstellen, wenn Sie mindestens zwei Rolleninstanzen für jede Rolle haben. Die ermöglicht eine virtuellen Computern Clientanfragen zu verarbeiten, während die anderen aktualisiert wird. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Ändern eines Cloud-Diensts

1. Im [Azure klassischen Portal](http://manage.windowsazure.com/) **Cloud Services**klicken Sie auf, klicken Sie auf den Namen des Cloud-Dienst, und klicken Sie dann auf **Konfigurieren**.

    ![Seite "Konfiguration"](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Klicken Sie auf der Seite **Konfigurieren** können Sie konfigurieren die Überwachung, Rolle aktualisieren, und wählen die Gast-Betriebssystem und Familie für Rolleninstanzen. 

2. **Überwachung**legen Sie die Überwachung Ebene ausführlich oder minimale, und konfigurieren Sie die Diagnose Verbindungszeichenfolgen, die für die Überwachung von ausführlichen erforderlich sind.

3. Für den Dienstverwaltungsrollen (gruppiert nach Rolle) können Sie die folgenden Einstellungen aktualisieren:
    
    >**Einstellungen**  
    >Ändern Sie die Werte der Einstellungen für verschiedene Konfiguration, die in den Elementen *ConfigurationSettings* der Dienstkonfigurationsdatei (.cscfg) angegeben werden.
    >
    >**Zertifikate**  
    >Ändern Sie den Fingerabdruck des Zertifikats, der für eine Rolle in SSL-Verschlüsselung verwendet wird. Wenn Sie ein Zertifikat ändern möchten, müssen Sie zuerst das neue Zertifikat (auf der Seite **Zertifikate** ) hochladen. Aktualisieren Sie dann den Fingerabdruck der Zertifikat Zeichenfolge in den Einstellungen Rolle angezeigt.

4. **Betriebssystem**können Sie Betriebssystemfamilie oder Version für Rolleninstanzen ändern, oder wählen Sie **Automatische** Aktivieren von automatischen Updates von Version des aktuellen Betriebssystems. Die Standardeinstellungen des Betriebssystems beziehen sich auf Webrollen und Worker-Rollen, aber wirken sich nicht auf virtuellen Computern.

    Während der Bereitstellung die neueste Version des Betriebssystems auf alle Rolleninstanzen installiert ist, und die Betriebssysteme werden standardmäßig automatisch aktualisiert. 
    
    Wenn Sie für Ihre Cloud-Dienst, in einer anderen Betriebssystemversion aufgrund von Kompatibilität Anforderungen im Code ausführen müssen, können Sie ein Betriebssystemfamilie und mit Version auswählen. Wenn Sie eine bestimmte Betriebssystemversion auswählen, werden das Betriebssystem von automatischen Updates für den Clouddienst angehalten. Sie benötigen, um sicherzustellen, dass die Betriebssysteme Updates zu erhalten.
    
    Wenn Sie alle Kompatibilitätsprobleme, die Ihre apps mit der neuesten Version des Betriebssystems verfügen beheben, können Sie das Betriebssystem von automatischen Updates durch Festlegen der Version des Betriebssystems auf **automatisch**aktivieren. 
    
    ![OS-Einstellungen](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Zum Speichern der Einstellungen für die Konfiguration, und drücken Sie diese, um die Rolleninstanzen, klicken Sie auf **Speichern**. (Klicken Sie auf Abbrechen Änderungen **verwerfen** .) **Speichern** und **verwerfen** werden nach der Änderung einer Einstellung der Befehlsleiste hinzugefügt.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualisieren einer Konfigurationsdatei der Cloud-Dienst

1. Herunterladen einer Cloud-Dienstkonfigurationsdatei (.cscfg) mit der aktuellen Konfiguration. Klicken Sie auf der Seite für den Clouddienst **Konfigurieren** klicken Sie auf **herunterladen**. Klicken Sie dann klicken Sie auf **Speichern**, oder klicken Sie auf **Speichern unter** zum Speichern der Datei.

2. Nach der Aktualisierung der Konfiguration Dienstdatei hochladen Sie, und wenden Sie die Konfiguration Updates:

    1. Klicken Sie auf **Hochladen**, klicken Sie auf der Seite **Konfigurieren** .
    
        ![Hochladen der Konfiguration](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. Verwenden Sie in **Konfigurationsdatei** **Durchsuchen** , wählen Sie die aktualisierte .cscfg-Datei aus.
    
    3. Wenn Ihre Cloud-Dienst Rollen, die nur eine Instanz haben enthält, aktivieren Sie das Kontrollkästchen **Konfiguration anwenden, auch wenn eine oder mehrere Rollen eine einzelne Instanz enthalten** , aktivieren Sie die Updates Konfiguration für die Rollen aus, um den Vorgang fortzusetzen.
    
        Es sei denn, Sie mindestens zwei Instanzen von jeder Rolle definiert haben, kann Azure nicht mindestens 99,95 % gewährleisten Verfügbarkeit von Ihrem Cloud-Dienst während der Konfiguration Dienstupdates. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).
    
    4. Klicken Sie auf **OK** (Häkchen). 


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name.md)ein.
* [Verwalten der Cloud-Dienst](cloud-services-how-to-manage.md).
* [Aktivieren Sie Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)
* Konfigurieren von [Ssl-Zertifikate](cloud-services-configure-ssl-certificate.md).
