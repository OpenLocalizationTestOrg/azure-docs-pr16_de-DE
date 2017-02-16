
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Beispiel-Konfigurationsdatei Verbindung Zeichenfolge Wertpapiers zurück


Es ist falsche die Verbindungszeichenfolge als literalen im C#-Code eingefügt. Es empfiehlt sich, die Verbindungszeichenfolge in einer Konfigurationsdatei zu setzen. Es können Sie jederzeit ohne zu kompilieren müssen die Zeichenfolge bearbeiten.

Angenommen, Ihr kompilierte C#-Programm heißt **ConsoleApplication1.exe**, und dass diese .exe in befindet einer **Bin\debug\* * Directory.

In diesem Beispiel werden die meisten Teile der Verbindungszeichenfolge in einer Konfigurationsdatei mit dem Namen genau **ConsoleApplication1.exe.config**gespeichert. Diese Datei Config muss auch befinden sich in **Bin\debug\**.

Die XML-Daten in der folgenden Konfigurationsdatei wird eine Verbindungszeichenfolge mit Namen **ConnectionString4NoUserIDNoPassword**. Der C#-Code wird für diese Zeichenfolge gesucht.

Sie müssen reale Namen in die Platzhalter bearbeiten:

- {Your_serverName_here}
- {Your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Diese Abbildung dorthin wir zwei Parameter ausgelassen werden:

- Benutzer-ID = {Your_userName_here};
- Kennwort = {Your_password_here};


Sie können diese einbeziehen, aber manchmal ist es besser, wenn das Programm diese Werte von Tastatureingaben vom Benutzer erhalten haben. Dies ist abhängig.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
