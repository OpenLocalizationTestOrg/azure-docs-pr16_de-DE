<properties
   pageTitle="Azure Datenkatalog Versionsinformationen | Microsoft Azure"
   description="Versionsinformationen für Azure Datenkatalog."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure Datenkatalog Versionsinformationen

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Lassen Sie Notizen für die November 20 2015 der Azure-Datenkatalog

### <a name="opening-data-sources-in-power-bi-desktop"></a>Öffnen von Datenquellen in Power BI-Desktop

Wenn Sie die Option "Öffnen in Power BI Desktop" aus dem **Datenkatalog Azure** -Portal zu verwenden, können Benutzer eine der zwei Probleme in der Power BI-Desktop-Anwendung auftreten:

- Es wird ein Dialogfeld mit dem Titel "Können nicht genutzt werden zum Öffnen des Dokuments" angezeigt.
- Die Power BI-Desktop-Anwendung geöffnet wird, aber die Datei leer sein scheint

Für jede Situation kann das Problem gelöst werden, indem herunterladen und Installieren der neuesten Version von Power BI-Desktop aus [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Lassen Sie Notizen für die November 13 2015 der Azure-Datenkatalog

### <a name="registering-and-connecting-to-teradata"></a>Registrieren und Herstellen einer Verbindung mit einer Teradata

Teradata-Datenquellen, die Benutzer den richtigen Teradata ODBC-Treiber installiert haben, müssen die Verbindung mit dem, die die Bitness der verwendeten Software (32-Bit- oder 64-Bit) entsprechen.

Zum diese ADC Release-Datum der letzten [Teradata-ODBC-Treiber für Windows (Version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) kompatibel ist mit Office 2013, aber nicht mit Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Lassen Sie Notizen für die Juli 13 2015 der Azure-Datenkatalog

### <a name="registering-and-connecting-to-oracle-database"></a>Registrieren und Herstellen einer Verbindung mit einer Oracle-Datenbank

Beim Herstellen einer Verbindung mit müssen Oracle-Datenbank Datenquellen Benutzer die richtigen Oracle-Treiber installiert haben, die die Bitness der verwendeten Software (32-Bit- oder 64-Bit) entsprechen.

-   Beim Registrieren Oracle-Datenquellen auf einem Computer unter 32-Bit-Windows werden die 32-Bit-Oracle-Treiber verwendet werden
-   Beim Registrieren Oracle-Datenquellen auf einem Computer unter Windows 64-Bit-Version werden die 64-Bit-Oracle-Treiber verwendet werden
-   Bei der Verbindung mit einer Oracle-Datenquellen mithilfe von Excel auf einem Computer mit der 32-Bit-Version von Microsoft Office werden auf 64-Bit-Windows einschließlich, der 32-Bit-Oracle-Treiber verwendet werden
-   Bei der Verbindung mit einer Oracle-Datenquellen mithilfe von Excel auf einem Computer unter der 64-Bit-Version von Microsoft Office werden die 64-Bit-Oracle-Treiber verwendet werden

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registrieren und Herstellen einer Verbindung mit SQL Server Reporting Services

Unterstützung für SQL Server Reporting Services (SSRS) Datenquellen ist derzeit auf nur Server einheitlichen Modus beschränkt. Unterstützung für SSRS im SharePoint-Modus wird in einer späteren Version hinzukommen.

### <a name="opening-data-assets-in-excel"></a>Datenbestände in Excel öffnen

Beim Datenbestände in Microsoft Excel aus dem **Datenkatalog Azure** -Portal zu öffnen, können Benutzer mit einem Dialogfeld **Sicherheitshinweis für Microsoft Excel** aufgefordert. Dies ist die Standardansicht, erwartet, und Benutzer können auswählen **Aktivieren** , um den Vorgang fortzusetzen.

Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren von Sicherheitswarnungen zu Verknüpfungen und Dateien von verdächtigen Websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy und Richtlinie Konfiguration und Daten Quelle Registrierung

Benutzer können eine Situation auftreten, in dem sie sich anmelden können Datenkatalog Azure-Portal an, aber bei dem Versuch, an der Quelle-Registration-Tool Anmelden eine Fehlermeldung angezeigt, die verhindert, dass sie auftretenden auf Protokollierung.

Es gibt zwei mögliche Ursachen für dieses Problem Verhalten:

**Ursache 1: Active Directory Federation Services-Konfiguration** Tools für die Datenquelle Registrierung verwendet formularbasierte Authentifizierung an-gegen Active Directory überprüft. Für die erfolgreiche Anmeldung muss formularbasierte Authentifizierung von einem Active Directory-Administrator in der globalen Richtlinie Authentifizierung aktiviert sein.

In einigen Fällen kann dieses Fehlerbehandlungsverhalten auftreten, nur, wenn der Benutzer auf das Unternehmensnetzwerk ist oder nur, wenn der Benutzer von außerhalb des Unternehmensnetzwerks eine Verbindung herstellt. Die globale Authentifizierungsrichtlinie ermöglicht Authentifizierungsmethoden für Intranet- und extranet-Verbindungen separat aktiviert werden. Fehler bei der Anmeldung können auftreten, wenn für das Netzwerk formularbasierte Authentifizierung nicht aktiviert ist, aus denen der Benutzer eine Verbindung herstellt.

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).

**Ursache 2: Proxy-Netzwerkkonfiguration** Wenn das Unternehmensnetzwerk einen Proxyserver verwendet, Tools für die Registrierung über den Proxy eine Verbindung zu Azure Active Directory möglicherweise nicht. Benutzer können sicherstellen, dass Tools für die Registrierung durch Bearbeiten des Tools Konfigurationsdatei, die Datei in diesem Abschnitt hinzu:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Suchen Sie die Datei RegistrationTool.exe.config, starten Sie das Registrierungstool, und klicken Sie dann öffnen Sie den Windows Task-Manager. Klicken Sie auf der Registerkarte Details im Task-Manager mit der rechten Maustaste auf RegistrationTool.exe, und wählen Sie im Popupmenü Dateispeicherort öffnen.
