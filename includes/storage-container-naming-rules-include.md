Jeder Blob in Azure-Speicher muss in einem Container befinden. Den Container Formulare Teil der Blob-Name. Beispielsweise `mycontainer` ist der Name des Containers im folgenden Beispiel Blob-URIs:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Ein Containername muss ein gültiger DNS-Name, entsprechen den folgenden Benennungskonventionen:

1. Containernamen müssen mit einem Buchstaben oder eine Zahl beginnen, und können nur Buchstaben, Zahlen und der Bindestrich (-) enthalten.
1. Jeder Bindestrichs (-) muss sofort davor und dahinter einen Buchstaben oder eine Zahl sein; aufeinander folgende Striche sind im Containernamen nicht zulässig.
1. Alle Buchstaben in einem Container muss Kleinbuchstaben.
1. Containernamen müssen zwischen 3 und 63 Zeichen lang sein.

> [AZURE.IMPORTANT] Beachten Sie, dass der Name eines Containers immer Kleinbuchstaben sein muss. Wenn Sie ein Großbuchstabe in einem Container enthalten oder Weise gegen den Container benennen Regeln, können Sie eine 400 (Ungültige Anforderung) Fehlermeldung. 