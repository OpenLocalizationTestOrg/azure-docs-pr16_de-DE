<properties
    pageTitle="Die Access-Dienstleistung (Java) zurückgegebene SAML-Ansicht"
    description="Erfahren Sie, wie die Access-Dienstleistung in Java-Clientanwendungen auf Azure gehostet zurückgegebene SAML anzeigen."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Zum Anzeigen der Azure-Dienst zurückgegebene SAML

Dieses Handbuch zeigt Ihnen, wie Sie die zugrunde liegenden Security Assertion Markup Language (SAML) zurückgegeben an Ihrer Anwendung Azure Access Control Service (ACS) anzeigen. Des Leitfadens beruht auf dem [zum Authentifizieren Webbenutzer mit Azure Access Steuerelement Dienst mithilfe von "Ellipse"][] -Thema, indem Sie Codes, die SAML-Informationen angezeigt, werden. Die fertige Anwendung sieht ähnlich wie der folgende aus.

![Beispiel für die Ausgabe SAML][saml_output]

Weitere Informationen zu ACS finden Sie unter Abschnitt [Weitere Schritte](#next_steps) .

> [AZURE.NOTE]
> Den Azure Access Services Steuerelement Filter handelt es sich um eine Community Technology Preview. Als Vorabversion der Software ist es formell von Microsoft nicht unterstützt.

## <a name="prerequisites"></a>Erforderliche Komponenten

Führen Sie die Aufgaben in diesem Handbuch, führen Sie die im Beispiel unter [So authentifizieren Webbenutzer mit Azure Access Steuerelement Dienst mithilfe von "Ellipse"][] , und diese als Ausgangspunkt für dieses Lernprogramm verwenden.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Fügen Sie der Bibliothek JspWriter Ihrer Build Pfad und Bereitstellung Assembly hinzu

Fügen Sie die Bibliothek, die **javax.servlet.jsp.JspWriter** Verzicht auf Ihre eigene Pfad und Bereitstellung Assembly enthält. Wenn Sie Tomcat verwenden, ist die Bibliothek **Jsp-api.jar**, die sich im Ordner Apache **Bibliothek** befindet.

1. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **MyACSHelloWorld**auf **Pfad erstellen**, klicken Sie auf **Erstellen Pfad konfigurieren**, klicken Sie auf der Registerkarte **Bibliotheken** , und klicken Sie dann auf **Externe JARs hinzufügen**.
2. Klicken Sie im Dialogfeld **Auswahl JAR-** navigieren Sie zu der erforderlichen JAR, wählen Sie ihn aus, und klicken Sie dann auf **Öffnen**.
3. Das Dialogfeld **Eigenschaften für MyACSHelloWorld** noch geöffnet ist und klicken Sie auf **Bereitstellung Assembly**.
4. Klicken Sie im Dialogfeld **Bereitstellung Webassembly** auf **Hinzufügen**.
5. Klicken Sie im Dialogfeld **Neue Assemblydirektive** klicken Sie auf **Java erstellen Pfadeinträge** aus, und klicken Sie dann auf **Weiter**.
6. Wählen Sie die entsprechende Bibliothek, und klicken Sie auf **Fertig stellen**.
7. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für MyACSHelloWorld** zu schließen.

## <a name="modify-the-jsp-file-to-display-saml"></a>Ändern Sie die Datei JSP um SAML anzuzeigen.

Ändern Sie **index.jsp** , um den folgenden Code verwenden.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Führen Sie die Anwendung

1. Führen Sie die Anwendung im Emulator Computer oder in Azure, mit der bei [zum Authentifizieren Webbenutzer mit Azure Access Steuerelement Dienst mithilfe von "Ellipse"][]dokumentierten Schritte bereitstellen.
2. Starten Sie einen Browser, und öffnen Sie die Webanwendung. Nachdem Sie sich an Ihrer Anwendung anmelden, sehen Sie SAML-Informationen, einschließlich der Sicherheit Assertion vom Identitätsanbieter bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Zum weiteren Untersuchen Sie der ACS-Funktionalität und um komplexere Szenarien experimentieren möchten, finden Sie unter [Access Steuerelement Service 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[So Web Benutzerauthentifizierung mit Azure Access Control Service "Ellipse" verwenden]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 