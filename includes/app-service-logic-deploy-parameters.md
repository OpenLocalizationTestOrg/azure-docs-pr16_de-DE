Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt als Parameter bezeichnet, der alle Parameterwerte enthält.
Sie sollten einen Parameter für diese Werte definieren, die variiert basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleiben soll. Jeder Parameterwert wird in der Vorlage zur Definieren der Ressourcen, die bereitgestellt werden. 

Wenn Parameter zu definieren, verwenden Sie das Feld **AllowedValues** , um anzugeben, welche Werte ein Benutzer, während der Bereitstellung angeben kann. Verwenden Sie das Feld **Standardwert** zum Zuweisen des Werts an den Parameter, wenn kein Wert, während der Bereitstellung angegeben wird.

Wir beschreiben Sie jeden Parameter in der Vorlage.

### <a name="logicappname"></a>logicAppName

Der Name der Logik app zu erstellen.

    "logicAppName": {
        "type": "string"
    }