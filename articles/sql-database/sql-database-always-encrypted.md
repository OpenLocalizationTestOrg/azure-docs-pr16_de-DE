<properties
    pageTitle="Immer verschlüsselt: Schützen Sie vertrauliche Daten in Azure SQL-Datenbank mit Verschlüsselung der Datenbank | Microsoft Azure"
    description="Schützen Sie vertrauliche Daten in einer SQL-Datenbank in Minuten."
    keywords="Verschlüsseln von Daten, Sql-Verschlüsselung, Verschlüsselung der Datenbank, vertrauliche Daten immer verschlüsselt"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Immer verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und speichern Sie Ihrer Schlüssel für die Verschlüsselung Zertifikat Windows store

> [AZURE.SELECTOR]
- [Azure Key Tresor](sql-database-always-encrypted-azure-key-vault.md)
- [Zertifikat-Windows store](sql-database-always-encrypted.md)


In diesem Artikel wird gezeigt, wie Sie vertrauliche Daten in einer SQL-Datenbank mit Verschlüsselung der Datenbank zu sichern, indem Sie mit dem [Assistenten immer verschlüsselt](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Es auch zeigt, wie der Schlüssel für die Verschlüsselung im Windows Store Zertifikat gespeichert.

Verschlüsselte ist immer eine neue Daten Verschlüsselung Technologie in Azure SQL-Datenbank und SQL Server, mit der Schutz statischer vertrauliche Daten auf dem Server, während Bewegung zwischen Client und Server, und während die Daten verwendet werden, die vertraulichen Daten nie Sicherstellung als nur-Text innerhalb der Datenbanksystem angezeigt wird. Nachdem Sie Daten verschlüsseln, können nur Clientanwendungen oder app-Servern mit Zugriff auf die Schlüssel nur-Text-Daten zugreifen. Ausführliche Informationen finden Sie unter [Immer verschlüsselt (Datenbankmodul)](https://msdn.microsoft.com/library/mt163865.aspx).

Nach dem Konfigurieren der Datenbank, wenn immer verschlüsselt verwenden möchten, erstellen Sie eine Clientanwendung in c# mit Visual Studio für die Arbeit mit der verschlüsselten Daten.

Führen Sie die Schritte in diesem Artikel erfahren, wie immer verschlüsselt für eine SQL Azure-Datenbank einrichten. In diesem Artikel erfahren Sie, wie Sie die folgenden Aufgaben ausführen:

- Mithilfe des Assistenten immer verschlüsselt in SSMS [Um immer verschlüsselt Tasten](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)zu erstellen.
    - Erstellen Sie eine [Spalte Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Erstellen eines [Verschlüsselungsschlüssels für Spalte (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Erstellen einer Datenbanktabelle und verschlüsseln Spalten.
- Erstellen Sie eine Anwendung, die fügt, markiert, und zeigt die Daten aus der verschlüsselten Spalten an.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm verwenden müssen Sie:

- Ein Azure-Konto und Ihr Abonnement. Wenn Sie eine, melden Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)besitzen.
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) Version 13.0.700.242 oder höher.
- [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) oder höher (auf dem Clientcomputer).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Erstellen Sie eine leere SQL-Datenbank
1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)aus.
2. Klicken Sie auf **neue** > **Daten + Speicher** > **SQL-Datenbank**.
3. Erstellen Sie eine **leere** Datenbank mit dem Namen **des Technologieüberblicks** auf einem neuen oder vorhandenen Server. Weitere Informationen zum Erstellen einer Datenbank im Azure-Portal finden Sie unter [Erstellen einer SQL-Datenbank in Minuten](sql-database-get-started.md).

    ![Erstellen Sie eine leere Datenbank](./media/sql-database-always-encrypted/create-database.png)

Sie benötigen die Verbindungszeichenfolge später im Lernprogramm. Nachdem Sie die Datenbank erstellt wird, wechseln Sie zu der neuen Technologieüberblick Datenbank, und kopieren Sie die Verbindungszeichenfolge. Sie können die Verbindungszeichenfolge zu einem beliebigen Zeitpunkt abrufen, aber es ist einfach, um ihn zu kopieren, Idee Azure-Portal.

1. Klicken Sie auf **SQL-Datenbanken** > **Technologieüberblick** > **Datenbank Verbindungszeichenfolgen anzeigen**.
2. Kopieren Sie die Verbindungszeichenfolge für **ADO.NET**.

    ![Kopieren Sie die Verbindungszeichenfolge](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Verbinden Sie mit der Datenbank mit SSMS

Öffnen Sie SSMS und auf dem Server mit der Datenbank Technologieüberblick verbinden.


1. SSMS zu öffnen. (Klicken Sie auf **Verbinden** > **-Datenbankmodul** an, das **mit Server verbinden** -Fenster zu öffnen, wenn er nicht geöffnet ist).
2. Geben Sie Ihren Servernamen und die Anmeldeinformationen ein. Der Servernamen finden Sie in der SQL-Datenbank Blade und in der Verbindungszeichenfolge Sie zuvor kopiert haben. Geben Sie den vollständigen Servernamen ein, einschließlich *database.windows.net*.

    ![Kopieren Sie die Verbindungszeichenfolge](./media/sql-database-always-encrypted/ssms-connect.png)

Wenn das **Neue Firewall-Regel** -Fenster geöffnet wird, melden Sie sich bei Azure und Erstellen einer neuen Firewallregel für Sie SSMS lassen.


## <a name="create-a-table"></a>Erstellen einer Tabelle

In diesem Abschnitt erstellen Sie eine Tabelle Patienten Daten aufnehmen soll. Dies ist eine normale Tabelle Anfangs – wird Verschlüsselung konfigurieren Sie im nächsten Abschnitt.

1. Erweitern Sie **Datenbanken**aus.
1. Mit der rechten Maustaste in der Datenbank **Technologieüberblick** , und klicken Sie auf **Neue Abfrage**.
2. Die neue Abfragefenster und **Ausführen** der folgenden Transact-SQL (T-SQL) fügen sie.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Verschlüsseln von Spalten (immer verschlüsselt konfigurieren)

SSMS bietet einen Assistenten, um problemlos immer verschlüsselt durch das Einrichten der CMK, CEK und verschlüsselten Spalten für Sie konfigurieren.

1. Erweitern Sie **Datenbanken** > **Technologieüberblick** > **Tabellen**.
2. Mit der rechten Maustaste in der Tabelle **an** , und wählen Sie **Spalten verschlüsseln** , um den Assistenten immer verschlüsselt zu öffnen:

    ![Verschlüsseln von Spalten](./media/sql-database-always-encrypted/encrypt-columns.png)

Der Assistent immer verschlüsselt umfasst die folgenden Abschnitte: **Auswahl einer Spalte**, die **Konfiguration Master Key** (CMK), **Prüfung**und **Zusammenfassung**.

### <a name="column-selection"></a>Auswahl einer Spalte ###

Klicken Sie auf der Seite **Einführung** zum Öffnen der **Ausgewählten Spalte** Seite auf **Weiter** . Auf dieser Seite, wählen Sie die Spalten zu verschlüsseln, [den Typ der Verschlüsselung, und welche Verschlüsselungsschlüssels Spalte (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) verwenden.

Verschlüsseln Sie **Versicherungsnummer** und **Geburtsdatum** Informationen für die einzelnen Patienten. Die Spalte **Versicherungsnummer** wird deterministisch Verschlüsselung, verwendet unterstützt Gleichheit Suchvorgänge, Verknüpfungen und Gruppieren nach. Die **BirthDate** -Spalte wird zufällige Verschlüsselung, verwenden Sie die Vorgänge nicht unterstützt.

Legen Sie den **Typ der Verschlüsselung** für die Spalte **Versicherungsnummer** auf **deterministische** und die Spalte **Geburtsdatum** **Randomized**ein. Klicken Sie auf **Weiter**.

![Verschlüsseln von Spalten](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Master Key-Konfiguration###

Die Seite **Master Key Konfiguration** ist, wo Sie Ihre CMK einrichten, und wählen Sie die Taste Store aus, in dem die CMK gespeichert werden. Aktuell, können Sie eine CMK in Windows Store Zertifikat, Azure-Taste Tresor oder einem Hardwaresicherheitsmodul (HSM) speichern. In diesem Lernprogramm erfahren, wie Ihre Keys im Windows Store Zertifikat gespeichert wird.

Stellen Sie sicher, dass **Windows Zertifikat Store** ausgewählt ist, und klicken Sie auf **Weiter**.

![Master Key-Konfiguration](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Überprüfung###

Verschlüsseln jetzt die Spalten oder speichern ein PowerShell-Skripts zu einem späteren Zeitpunkt ausführen können. In diesem Lernprogramm wählen Sie **Weiter zu jetzt fertig stellen** aus, und klicken Sie auf **Weiter**.

### <a name="summary"></a>Zusammenfassung###

Stellen Sie sicher, dass die Einstellungen sind alle korrigieren, und klicken Sie auf **Fertig stellen** , um die Einrichtung für immer verschlüsselt abzuschließen.

![Zusammenfassung](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Überprüfen des Assistenten Aktionen

Nachdem der Assistent abgeschlossen ist, wird die Datenbank für immer verschlüsselt eingerichtet. Der Assistent ausgeführt, die folgenden Aktionen aus:

- Erstellt eine CMK an.
- Erstellt eine CEK.
- So konfiguriert, dass die ausgewählten Spalten für die Verschlüsselung. Die Tabelle **Patienten** weist derzeit keine Daten, aber alle vorhandenen Daten in den ausgewählten Spalten ist nun verschlüsselt.

Sie können die Erstellung der Schlüssel in SSMS überprüfen, indem Sie auf **Technologieüberblick** > **Sicherheit** > **Tasten immer verschlüsselt**. Sie können jetzt die neuen Tastenkombination angezeigt, die der Assistent für Sie erstellt.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Erstellen einer Clientanwendung, mit die die verschlüsselten Daten zusammenarbeitet

Jetzt, da immer verschlüsselt eingerichtet haben, können Sie eine Anwendung erstellen, die *fügt* und *markiert* die verschlüsselten Spalten durchführt. Wenn Sie die Anwendung Beispiel erfolgreich ausführen zu können, führen Sie es auf dem gleichen Computer, wo Sie der Assistent immer verschlüsselt ist. Wenn Sie die Anwendung auf einem anderen Computer ausführen zu können, müssen Sie Ihre Zertifikate immer verschlüsselt an den Computer mit der Clientanwendung bereitstellen.  

> [AZURE.IMPORTANT] Wenn nur-Text-Daten auf dem Server mit Spalten immer verschlüsselt übergeben, muss die Anwendung [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) Objekte verwenden. Übergeben Literale Werten ohne SqlParameter Objekte ergibt in eine Ausnahme.


1. Öffnen Sie Visual Studio und erstellen Sie eine neue c#. Stellen Sie sicher, dass Ihr Projekt auf **.NET Framework 4.6** oder höher festgelegt ist.
2. Nennen Sie das Projekt **AlwaysEncryptedConsoleApp** , und klicken Sie auf **OK**.

![Neue Console-Anwendung](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Ändern der Verbindungszeichenfolge so, dass immer verschlüsselt aktivieren

In diesem Abschnitt wird erläutert, wie immer verschlüsselt in Ihrer Datenbank-Verbindungszeichenfolge aktivieren. Die app Console ändern Sie soeben erstellten im nächsten Abschnitt, "Immer verschlüsselt Beispiel Console-Anwendung."


Wenn immer verschlüsselt aktivieren möchten, müssen Sie die Verbindungszeichenfolge das Schlüsselwort **Spalte Verschlüsselung Einstellung** hinzu, und legen Sie ihn auf **aktiviert**.

Sie können dies direkt in der Verbindungszeichenfolge festlegen, oder Sie können mithilfe einer [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)festlegen. Die Anwendung Stichprobe im nächsten Abschnitt wird gezeigt, wie **SqlConnectionStringBuilder**verwenden.

> [AZURE.NOTE] Dies ist der einzige Änderung in einer bestimmten Clientanwendung immer verschlüsselt erforderlich. Wenn Sie eine vorhandene Anwendung verfügen, deren Verbindungszeichenfolge extern zu speichert (d. h., in einer Config-Datei), Sie möglicherweise immer verschlüsselt ohne Ändern von Code aktivieren.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Aktivieren Sie immer verschlüsselt in der Verbindungszeichenfolge

Fügen Sie das folgende Schlüsselwort zur Verbindungszeichenfolge hinzu:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Aktivieren Sie immer mit einer SqlConnectionStringBuilder verschlüsselt

Mit dem folgende Code wird gezeigt, wie immer verschlüsselt aktivieren, indem Sie die [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) auf [aktiviert](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)werden.

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Immer verschlüsselte Beispiel Console-Anwendung

In diesem Beispiel wird veranschaulicht, wie Sie:

- Ändern der Verbindungszeichenfolge so, dass immer verschlüsselt aktivieren.
- Einfügen von Daten in der verschlüsselten Spalten.
- Wählen Sie einen Datensatz durch Filtern nach einem bestimmten Wert in einer verschlüsselten Spalte ein.

Ersetzen Sie den Inhalt von **"Program.cs"** mit den folgenden Code ein. Ersetzen Sie die Verbindungszeichenfolge für die globale ConnectionString Variable in der Zeile direkt oberhalb der Main-Methode durch Ihre gültige Verbindungszeichenfolge aus dem Azure-Portal an. Dies ist der einzige Änderung, die Sie an diesem Code vornehmen müssen.

Führen Sie die app, um die Aktion immer verschlüsselt angezeigt werden.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-the-data-is-encrypted"></a>Stellen Sie sicher, dass die Daten verschlüsselt sind

Sie können schnell überprüfen, dass die tatsächlichen Daten auf dem Server durch Abfragen von Daten mit SSMS **Patienten** verschlüsselt werden. (Verwenden Sie die aktuelle Verbindung, in dem die Spalte Verschlüsselung Einstellung nicht noch nicht aktiviert ist.)

Führen Sie die folgende Abfrage in der Datenbank Technologieüberblick.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Sie können sehen, dass die verschlüsselten Spalten nicht nur-Text-Daten enthalten.

   ![Neue Console-Anwendung](./media/sql-database-always-encrypted/ssms-encrypted.png)


Wenn SSMS Zugriff auf die nur-Text-Daten verwenden möchten, können Sie Hinzufügen der **Spalte Verschlüsselung Einstellung = aktiviert** Parameter, um die Verbindung.

1. Klicken Sie in SSMS mit der rechten Maustaste in des Servers unter **Objekt-Explorer**, und klicken Sie dann auf **Trennen**.
2. Klicken Sie auf **Verbinden** > **-Datenbankmodul** an, öffnen Sie das Fenster **mit Server verbinden** , und klicken Sie dann auf **Optionen**.
3. Klicken Sie auf **Weitere Verbindungsparameter** und Typ **Spalte Verschlüsselung Einstellung = aktiviert**.

    ![Neue Console-Anwendung](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Führen Sie die folgende Abfrage in der Datenbank **Technologieüberblick** .

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Sie können jetzt im Nur-Text-Daten in den verschlüsselten Spalten anzeigen.


    ![Neue Console-Anwendung](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Wenn Sie eine Verbindung mit SSMS (oder einem beliebigen Client) von einem anderen Computer herstellen, hat keinen Zugriff auf den Schlüssel für die Verschlüsselung und werden nicht in der Lage, die Daten zu entschlüsseln.



## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine Datenbank, die immer verschlüsselt verwendet erstellen, sollten Sie die folgenden Aktionen ausführen:

- Führen Sie dieses Beispiel von einem anderen Computer an. Es keinen Zugriff auf die Tasten Verschlüsselung, damit es keinen Zugriff auf die nur-Text-Daten und nicht erfolgreich ausgeführt werden.
- [Drehen und Ihre Schlüssel Aufräumen](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migrieren von Daten, die bereits mit immer verschlüsselt verschlüsselt sind](https://msdn.microsoft.com/library/mt621539.aspx).
- [Immer verschlüsselt Bereitstellen von Zertifikaten anderer Clientcomputer](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (siehe Abschnitt "Ausführenden Zertifikate verfügbar zu Applications und Users").

## <a name="related-information"></a>Weitere Informationen

- [Immer verschlüsselt (Cliententwicklung)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Transparent Data-Verschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-Verschlüsselung](https://msdn.microsoft.com/library/bb510663.aspx)
- [Immer verschlüsselte-Assistenten](https://msdn.microsoft.com/library/mt459280.aspx)
- [Immer verschlüsselte Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
