<properties
   pageTitle="Beispiel für die Verwendung mit Sicherheit Begrenzungslinie Umgebungen | Microsoft Azure"
   description="Bereitstellen dieser einfachen Webanwendung nach dem Erstellen einer DMZ Datenverkehr Fluss Szenarien testen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Beispiel-Anwendung für die Verwendung mit Sicherheit Begrenzungslinie Umgebungen

[Kehren Sie zur Sicherheit Begrenzungslinie bewährte Methoden für die Seite][HOME]

Diese PowerShell Skripts können lokal zu installieren und eine sehr einfache Anwendung, die eine HTML-Seite vom iis01 neu front-End-Server mit dem Inhalt der Back-End-Server AppVM01 zeigt die Einrichtung iis01 neu und AppVM01 Server ausgeführt werden.

Diese app wird stellt eine einfache testen Umgebung für in vielen Beispielen DMZ und wie Änderungen auf die Endpunkte, NSGs, UDR und Firewall Effekt Regeln können Datenverkehr fließt.

## <a name="firewall-rule-to-allow-icmp"></a>Firewallregel ICMP zulassen
Dieses einfache PowerShell-Anweisung kann Windows virtuellen Computers ICMP (Ping) Datenverkehr zulassen ausgeführt werden. Damit wird einfacher testen und Problembehandlung, indem Sie das Ping-Protokoll zu pass-through die Windows-Firewall (für die meisten Linux Distros, die ICMP ist standardmäßig aktiviert).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Hinweis:** Wenn Sie verwenden die unter Skripts, diese Regel-Erweiterung Firewall ist die erste Anweisung.

## <a name="iis01---web-application-installation-script"></a>Iis01 neu - Web-Anwendung Installation-Skript
Dieses Skript wird:

1.  Öffnen Sie IMCPv4 (Ping) auf dem lokalen Server Windows-Firewall für einfacheres testen
2.  Installieren Sie IIS und .net Framework 4.5
3.  Erstellen einer ASP.NET-Webseite und eine Web.config-Datei
4.  Ändern der standardmäßigen Pool zu erleichtern Dateizugriff
5.  Festlegen des anonyme Benutzers zu Ihrem Konto und das Kennwort

Diese PowerShell-Skript sollte lokal ausgeführt werden, solange RDP in iis01 neu war.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - Datei-Server-Installation-Skript
Dieses Skript richtet Back-End für diese einfache Anwendung. Dieses Skript wird:

1.  Öffnen Sie IMCPv4 (Ping) auf der Firewall für einfacheres testen
2.  Erstellen Sie ein neues Verzeichnis
3.  Erstellen Sie eine Textdatei Zugriff von der Webseite oben Remote benutzerspezifisch
4.  Festlegen von Berechtigungen für das Verzeichnis und die Datei auf anonym, zugreifen dürfen
5.  Deaktivieren von IE verbesserte Sicherheit ermöglicht einfacher Durchsuchen von diesem server 

>[AZURE.IMPORTANT] **Bewährte Methode**: IE erweiterte Sicherheit auf einem Server Herstellung nie deaktivieren sowie es ist im Allgemeinen nicht ratsam, im Internet surfen von einem Server Herstellung. Darüber hinaus ist Öffnung Dateifreigaben für den anonymen Zugriff eine gute Idee, aber fertig hier zur Vereinfachung.

Diese PowerShell-Skript sollte lokal ausgeführt werden, solange RDP in AppVM01 war. PowerShell ist erforderlich als Administrator erfolgreichen Ausführung sicherzustellen ausgeführt werden soll.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS-Server-Installation-Skript
Kein Skript in dieser Anwendung Beispiel zum Einrichten des DNS-Servers enthalten ist. Wenn die Firewall Regeln testen, NSG oder UDR muss DNS-Verkehr enthalten, der DNS01 Server manuell installiert werden muss. Die Netzwerkkonfiguration Xml-Datei für beide Beispiele enthält DNS01 als die primäre DNS-Server und der öffentlichen DNS-Server von Stufe 3 als Sicherung DNS-Server gehostet wird. Der Ebene 3 DNS-Server würde der ist-DNS-Server für nicht lokale Datenverkehr verwendet werden, und mit DNS01 nicht setup, keine lokalen DNS würde auftreten.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
