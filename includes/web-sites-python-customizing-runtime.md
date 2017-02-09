Azure wird die Ermitteln der Version von Python für virtuellen Umgebung mit der folgenden Priorität verwendet werden soll:

1. in runtime.txt im Stammordner angegebene Version
1. Version Python Einstellung im Web app-Konfiguration angegeben ist (die **Einstellungen** > **Anwendungseinstellungen** Blade für Ihre Web app im Portal Azure)
1. Python-2.7 ist der Standardwert, wenn keine der oben angegeben sind

Gültige Werte für den Inhalt 

    \runtime.txt

sind:

- Python-2.7
- Python-3.4

Wenn die micro Version (dritte Ziffer) angegeben ist, wird es ignoriert.
