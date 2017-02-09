Aufgrund fortlaufende Entwicklung möglicherweise die installierte in Android Studio Android SDK-Version die Version der Code nicht überein. Das Android SDK verwiesen wird in diesem Lernprogramm ist Version 23, die neuesten zum Zeitpunkt der schreiben. Die Versionsnummer erhöhen wird als neue Versionen des SDK angezeigt werden, und es wird empfohlen, die neueste Version zur Verfügung.

Keine Übereinstimmung mit zwei Symptome sind:

1. Beim Erstellen oder Projekt neu erstellen, erhalten Sie eventuell Gradle Fehlermeldungen wie "**Fehler beim Ziel Google Inc.:Google APIs:n suchen**".

2. Standard Android-Objekte im Code, der aufgelöst werden muss, basierend auf `import` Anweisungen möglicherweise Fehlermeldungen generiert werden.

Wenn eine der folgenden angezeigt, möglicherweise die Version des Android SDK installiert in Android Studio nicht SDK Ziel des Projekts heruntergeladenen überein.  Nehmen Sie die folgenden Änderungen vor, um zu überprüfen, die Version:


1. Klicken Sie auf **Extras**, in Android Studio => **Android** => **SDK-Manager**. Wenn Sie die neueste Version der SDK-Plattform nicht installiert haben, klicken Sie dann auf um ihn zu installieren. Notieren Sie sich die Versionsnummer.

2. Öffnen Sie die Datei auf der Registerkarte Projekt-Explorer, klicken Sie unter **Gradle Skripts** **build.gradle (Modeule: app)**. Stellen Sie sicher, dass die **CompileSdkVersion** und **BuildToolsVersion** , auf die neueste SDK-Version installiert festgelegt sind. Die Tags sieht ungefähr so aus:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Klicken Sie im Android Studio Projekt-Explorer mit der rechten Maustaste in des Projektknoten, wählen Sie **Eigenschaften**aus, und wählen Sie in der linken Spalte **Android**. Stellen Sie sicher, dass das **Projekt erstellen Ziel** dieselbe Version SDK als die **TargetSdkVersion**festgelegt ist.

4. In Android Studio wird die Manifestdatei nicht mehr verwendet, geben Sie das Ziel SDK und SDK Mindestversion im Gegensatz zu den Fall mit "Ellipse".
