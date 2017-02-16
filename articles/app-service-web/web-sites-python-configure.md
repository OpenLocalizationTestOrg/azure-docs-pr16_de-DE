<properties 
    pageTitle="Konfigurieren von Python mit Azure App-Verwaltungsdienst Web Apps" 
    description="In diesem Lernprogramm werden die Optionen zum Erstellen und konfigurieren einen einfachen Webserver Gateway-Benutzeroberfläche (WSGI) kompatiblen Python Anwendung auf Azure App Dienst Web Apps." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfigurieren von Python mit Azure App-Verwaltungsdienst Web Apps

In diesem Lernprogramm werden die Optionen zum Erstellen und Konfigurieren einer einfachen Web Server Gateway-Benutzeroberfläche (WSGI) kompatiblen Python Anwendungs auf [Azure App Dienst Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Es werden zusätzliche Features der Git Bereitstellung, z. B. virtuelle Umgebung und Paketinstallation requirements.txt verwenden.


## <a name="bottle-django-or-flask"></a>Flaschen, Django oder wird?

Die Azure Marketplace enthält Vorlagen für die Framework Flaschen, Django und wird. Wenn Sie Web app im App-Verwaltungsdienst Azure Entwickeln, oder Sie nicht mit Git vertraut sind, empfehlen wir, dass Sie eine der folgenden Lernprogramme, führen Sie die eine schrittweise Anleitung zum Erstellen einer Anwendung arbeiten, aus dem Katalog Git Bereitstellung von Windows oder Mac mit enthalten:

- [Erstellen von Web-apps mit Flaschen](web-sites-python-create-deploy-bottle-app.md)
- [Erstellen von Web-apps mit Django](web-sites-python-create-deploy-django-app.md)
- [Erstellen von Web-apps mit wird](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Web app Erstellung Azure-Portal

In diesem Lernprogramm wird davon ausgegangen, einer vorhandenen Azure-Abonnement und den Zugriff auf das Portal Azure.

Wenn Sie eine vorhandene Web app nicht verfügen, können Sie eine vom [Azure-Portal](https://portal.azure.com)erstellen.  Klicken Sie auf die Schaltfläche ' Neu ' in der oberen linken Ecke, und klicken Sie auf **Web + Mobile** > **Web app**.

## <a name="git-publishing"></a>Git für die Veröffentlichung

Konfigurieren Sie anhand der Anweisungen bei der [Lokalen Bereitstellung von Git Azure App Dienst](app-service-deploy-local-git.md)Git Veröffentlichung für Ihre neu erstellten Web app. In diesem Lernprogramm verwendet Git erstellen, verwalten und unsere Python Web app auf App-Verwaltungsdienst Azure veröffentlichen.

Nachdem Sie für die Veröffentlichung Git eingerichtet haben, wird ein Repository Git erstellt und Web app zugeordnet. URL des Repositorys angezeigt wird und künftig zum Senden der Daten aus der lokalen Entwicklungsumgebung in der Cloud verwendet werden kann. Zum Veröffentlichen von Applications über Git stellen Sie sicher, dass ein Git Client auch installiert ist, und führen Sie die Anweisungen zur Verfügung gestellt, um Ihrer Web app-Inhalten an die App-Verwaltungsdienst Azure übermitteln.


## <a name="application-overview"></a>Übersicht über die Anwendung

In den nächsten Abschnitten werden die folgenden Dateien erstellt. Sie sollten im Stammverzeichnis der Git Repository platziert werden.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI Ereignishandler

WSGI ist ein Python Standard durch [PEP 3333](http://www.python.org/dev/peps/pep-3333/) definieren eine Schnittstelle zwischen dem Webserver und Python beschrieben. Es bietet eine standardisierte Schnittstelle zum Schreiben von verschiedenen Webanwendungen und Framework Python verwenden. Beliebte Python Web Framework verwenden heute WSGI. Azure App Dienst Web Apps bietet, die Sie für alle dass solche Strukturen unterstützen; Darüber hinaus können fortgeschrittene Benutzer auch eigene verfassen so lange der benutzerdefinierte Ereignishandler die WSGI Spezifikation Richtlinien folgt.

Hier ist ein Beispiel für eine `app.py` , einen benutzerdefinierten Ereignishandler definiert:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Sie können diese Anwendung lokal mit ausführen `python app.py`, navigieren Sie zu `http://localhost:5555` in Ihrem Webbrowser.


## <a name="virtual-environment"></a>Virtuelle Umgebung

Auch die oben genannten Beispiel-app externe Pakete benötigt wird nicht, ist es wahrscheinlich, dass Ihre Anwendung einige erforderlich sind.

Zum Verwalten von externen Paket Abhängigkeiten unterstützt Git Azure-Bereitstellung die Erstellung virtueller Umgebungen.

Wenn Azure ein requirements.txt im Stammverzeichnis des Repositorys erkennt, erstellt es automatisch eine virtuelle Umgebung mit dem Namen `env`. In diesem Fall nur auf der ersten Bereitstellung, oder während einer beliebigen Bereitstellung nach der ausgewählten Python Runtime geändert hat.

Es werden wahrscheinlich eine virtuelle Umgebung für die Entwicklung lokal erstellen möchten, aber nicht in Ihrem Repository Git einzuschließen.


## <a name="package-management"></a>Verwalten von Paketen

In requirements.txt aufgelisteten Pakete werden in der virtuellen Umgebung mit Pip automatisch installiert. In diesem Fall auf jede Bereitstellung, aber Pip überspringt Installation, wenn ein Paket bereits installiert ist.

Beispiel für `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python-Version

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Beispiel für `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Sie müssen zum Erstellen einer web.config-Datei, um anzugeben, wie der Server Anfragen behandelt werden sollen.

Beachten Sie, dass, wenn Sie eine Datei web.x.y.config in Ihrem Repository, x.y entspricht der ausgewählten Python Runtime, und Azure wird automatisch die entsprechende Datei als web.config kopiert haben.

Die folgenden Beispiele geben web.config basieren auf eine virtuelle Umgebung Proxy-Skript, die im nächsten Abschnitt beschrieben wird.  Sie arbeiten mit dem WSGI Ereignishandler im Beispiel verwendete `app.py` über.

Beispiel für `web.config` für Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Beispiel für `web.config` für Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statische Dateien werden direkt in den Webserver behandelt werden, ohne zu durchlaufen Python-Code zum Verbessern der Leistung.

In den Beispielen oben sollte der Speicherort der statischen Dateien auf dem Datenträger die Position in der URL übereinstimmen. Dies bedeutet, dass eine Anforderung für `http://pythonapp.azurewebsites.net/static/site.css` wird die Datei auf dem Datenträger am dienen `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`ist die Stelle, an der Sie den Ereignishandler WSGI angeben. In den Beispielen oben Sie `app.wsgi_app` , da der Ereignishandler eine Funktion namens ist `wsgi_app` in `app.py` im Stammordner.

`PYTHONPATH`kann angepasst werden, aber wenn Sie alle Ihre Abhängigkeiten in der virtuellen Umgebung installieren, indem Sie sie in requirements.txt angeben, dürfen nicht müssen Sie es ändern.


## <a name="virtual-environment-proxy"></a>Virtuelle Umgebung Proxy

Das folgende Skript wird verwendet, um den Ereignishandler WSGI abgerufen werden, aktivieren die virtuelle Umgebung und der Protokolldateien Fehler. Es ist generische und ohne Änderungen verwendet werden soll.

Inhalt des `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Anpassen der Git Bereitstellung

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Problembehandlung - Paketinstallation

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Problembehandlung - virtuellen Umgebung

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter der [Python Developer Center](/develop/python/).

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)





 
