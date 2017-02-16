<properties
   pageTitle="Generische SQL-Verbinder Schritt-für Schritt | Microsoft Azure"
   description="In diesem Artikel wird Sie eine einfache HR-System mithilfe der generische SQL-Verbinder schrittweise durchlaufen."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Generische schrittweise SQL-Connector
In diesem Thema wird eine schrittweise Anleitung. Erstellt eine einfachen HR Beispieldatenbank, und verwenden es für einige Benutzer und deren Gruppenmitgliedschaft importieren.

## <a name="prepare-the-sample-database"></a>Vorbereiten der Beispieldatenbank
Führen Sie auf einem Server mit SQL Server finden Sie im [Anhang A](#appendix-a)SQL-Skript ein. Dieses Skript erstellt eine Beispieldatenbank mit dem Namen GSQLDEMO. Das Objektmodell für die erstellte Datenbank sieht wie dieses Bild aus:  
![Objektmodell](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Erstellen Sie auch einen Benutzer, die, den Sie verwenden, um mit der Datenbank herstellen möchten. In dieser Anleitung erfahren Sie wird der Benutzer als FABRIKAM\SQLUser bezeichnet und befindet sich in der Domäne.

## <a name="create-the-odbc-connection-file"></a>Erstellen der ODBC-Verbindungsdatei
Allgemeine SQL-Connector ist ODBC verwenden, für die Verbindung mit dem Remoteserver. Zuerst müssen wir eine Datei mit den ODBC-Verbindungsinformationen zu erstellen.

1. Starten Sie das Programm ODBC-Verwaltung auf dem Server.  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Wählen Sie die Registerkarte **Datei-DSN**aus. Klicken Sie auf **Hinzufügen**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. Die Out-of-Box-Treiber funktioniert detaillierter, also wählen Sie ihn aus und klicken Sie auf **Weiter >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Geben Sie der Datei einen Namen ein, beispielsweise **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Klicken Sie auf **Fertig stellen**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Uhrzeit, zu die Verbindung konfiguriert. Geben Sie eine gute Beschreibung der Datenquelle, und geben Sie den Namen des Servers mit SQL Server.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Wählen Sie zum Authentifizieren mit SQL aus. In diesem Fall verwenden wir die Windows-Authentifizierung.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Geben Sie den Namen der Beispieldatenbank, **GSQLDEMO**.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Beibehalten alles Standard auf diesem Bildschirm. Klicken Sie auf **Fertig stellen**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Um zu überprüfen, dass alles wie erwartet funktioniert, klicken Sie auf **Datenquelle testen**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Stellen Sie sicher, dass der Test erfolgreich war.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. Die ODBC-Konfigurationsdatei sollten jetzt im Datei-DSN sichtbar sein.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Wir haben jetzt die Datei, die wir benötigen, und beginnen Sie können den Verbinder zu erstellen.

## <a name="create-the-generic-sql-connector"></a>Erstellen des allgemeinen SQL-Verbinders

1. Wählen Sie die Synchronisierung Dienst-Manager UI **Verbindern** und **Erstellen**. Wählen Sie **Generische SQL (Microsoft)** , und geben sie einen aussagekräftigen Namen.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Suchen Sie die DSN-Datei, die Sie im vorherigen Abschnitt erstellt haben, und Laden Sie es auf dem Server hoch. Geben Sie die Anmeldeinformationen für die Verbindung mit der Datenbank.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. In dieser Anleitung erfahren Sie wir sind und erleichtern uns und angenommen, es zwei Objekttypen **Benutzer** und **Gruppen gibt**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Um die Attribute zu finden, möchten wir den Verbinder, um diese Attribute erkennen, indem Sie die Tabelle Items. Da **Benutzer** ein reserviertes Wort in SQL ist, müssen wir es in eckigen Klammern [] bereitstellen.  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Zeit, die Attribute Anker und der DN definieren. Für **Benutzer**verwenden wir die Kombination der zwei Attribute Username und EmployeeID. Für die **Gruppe**verwenden wir Gruppenname (nicht realistische in Praxis, sondern auch für diese Anleitung Funktionsweise).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Nicht alle Attributtypen können in einer SQL-Datenbank nicht erkannt werden. Das Attribut Bezugstyp nicht im besonderen. Für den Objekttyp Gruppe müssen wir die OwnerID und MemberID Bezug zu ändern.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. Den Attributen, die wir als Referenz Attribute im vorherigen Schritt den Objekttyp erforderlich ausgewählt werden diese Werte ein Verweis auf. In diesem Fall Benutzertyp Objekt.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Wählen Sie auf der Seite globale Parameter als die Delta Strategie **Wasserzeichen** aus. Geben Sie in dem Datum/Uhrzeit-Format **JJJJ / MM / TT HH: mm:**.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Wählen Sie auf der Seite **Partitionen konfigurieren und Hierarchien** beide Objekttypen aus.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. Wählen Sie auf die **Objekttypen auswählen** und die **Attribute auswählen**Objekttypen und alle Attribute aus. Klicken Sie auf der Seite **Konfigurieren Anker** auf **Fertig stellen**.

## <a name="create-run-profiles"></a>Erstellen von Profilen ausführen

1. Wählen Sie in der Synchronisierung Dienst-Manager UI **Verbinder**, und **Führen Sie Profile konfigurieren**. Klicken Sie auf **Neues Profil**. Zunächst mit **Vollständigen Import**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Wählen Sie den **Vollständigen Import (nur Bereitstellung)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Wählen Sie die Partition **Objekt = Benutzer**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Wählen Sie **Tabelle** aus, und geben Sie **[Benutzer]**. Führen Sie einen Bildlauf nach unten bis zum Abschnitt Typ mehrwertigen Objekt, und geben Sie die Daten wie in der nachstehenden Abbildung. Wählen Sie auf **Fertig stellen** , um den Schritt zu speichern.
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Wählen Sie **neuen Schritt**aus. Wählen Sie dieses Mal **Objekt = Gruppe**. Klicken Sie auf der letzten Seite verwenden Sie die Konfiguration wie in der nachstehenden Abbildung aus. Klicken Sie auf **Fertig stellen**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Optional: Wenn Sie möchten, können Sie zusätzliche ausführen Profile konfigurieren. Für diese exemplarische Vorgehensweise wird nur für den vollständigen Import verwendet.
7. Klicken Sie auf **OK** , um die gewünschten ausführen Profile geändert haben.

## <a name="add-some-test-data-and-test-the-import"></a>Hinzufügen von einige Daten testen und Testen des Imports
Füllen Sie einige Testdaten in der Beispieldatenbank aus. Wenn Sie bereit sind, wählen Sie **Ausführen** und **vollständigen Import**.

Hier ist ein Benutzer mit zwei Telefonnummern und einer Gruppe mit einigen Mitgliedern aus.  
![CS1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Anhang A
**SQL-Skript zum Erstellen der Beispieldatenbank**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
