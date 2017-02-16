### <a name="install-via-composer"></a>Über Composer installieren

1. [Installieren der Git][install-git]. Beachten Sie, dass unter Windows, Sie zu Ihrer Umgebung PATH-Variable auch die Git ausführbare Datei hinzufügen müssen. 

2. Erstellen Sie eine Datei mit dem Namen **composer.json** im Stammverzeichnis des Projekts, und fügen Sie den folgenden Code hinzu:

    ```
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```

3. Herunterladen von ** [composer.phar] [ composer-phar] ** in Ihrem Projekt aus.

4. Öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl im Projektstammverzeichnis

    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
