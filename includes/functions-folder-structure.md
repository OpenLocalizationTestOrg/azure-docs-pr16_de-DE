
Der Code für alle Funktionen in einer app für die angegebene Funktion befindet sich in einem Stammordner, der eine Hostkonfigurationsdatei enthält und einem oder mehreren Unterordnern, von denen jede enthalten den Code für eine separate Funktion, wie im folgenden Beispiel

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Die Datei *host.json* enthält einige Runtime-spezifische Konfiguration und befindet sich im Stammverzeichnis der app (Funktion). Informationen zu Einstellungen, die verfügbar sind, finden Sie unter [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) im WebJobs.Script Repository-Wiki.

Jede Funktion weist einen Ordner, der eine oder mehrere Codedateien, die function.json Konfiguration und anderen Abhängigkeiten enthält.