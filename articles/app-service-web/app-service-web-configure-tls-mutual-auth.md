<properties 
    pageTitle="So konfigurieren Sie die gemeinsamen TLS-Authentifizierung für Web App" 
    description="Informationen Sie zum Konfigurieren der Web-app, um Client Zertifikatauthentifizierung auf TLS verwenden." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>So konfigurieren Sie die gemeinsamen TLS-Authentifizierung für Web App

## <a name="overview"></a>(Übersicht) ##
Sie können Zugriff auf Azure Web app einschränken, indem Sie verschiedene Authentifizierungsarten dafür. Eine Möglichkeit dazu ist zum Authentifizieren ein Client-Zertifikat verwenden, wenn die Anforderung über TLS/SSL befindet. Dieses Verfahren heißt gemeinsamen TLS-Authentifizierung oder Client-Zertifikat, dass Anmelde- und in diesem Artikel zum Einrichten der Web-app, um Client Zertifikatauthentifizierung verwenden detailliert aufgeführt ist.

> **Hinweis:** Wenn Sie Ihre Website über HTTP und nicht HTTPS zugreifen, erhalten Sie keine Client-Zertifikat. Ja, wenn die Anwendung Client-Zertifikate erfordert sollten Sie nicht Anfragen an Ihrer Anwendung über HTTP zulassen.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>Konfigurieren von Web App für Client-Zertifikat-Authentifizierung ##
Zum Einrichten der Web-app, um Client-Zertifikate müssen Sie die Einstellung für die Website ClientCertEnabled für Ihre Web app hinzufügen, und legen Sie dafür wahr. Diese Einstellung ist nicht über die Verwaltungsoption im Portal derzeit verfügbar, und die REST-API dazu verwendet werden müssen.

Das [Tool ARMClient](https://github.com/projectkudu/ARMClient) können Sie um den Anruf REST-API gestalten zu erleichtern. Nachdem Sie sich mit dem Tool anmelden müssen Sie den folgenden Befehl eingeben:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
Ersetzen alles in {} mit Informationen für Ihre Web app und erstellen eine Datei namens enableclientcert.json mit den folgenden JSON Inhalt:

> {"Speicherort": "Meine App Webspeicherort"   
>   "Eigenschaften": {}  
>     "ClientCertEnabled": WAHR}}  

Stellen Sie sicher, den Wert "Speicherort" ändern, wo Ihre Web app befindet, z. B. US North Central oder "Westen" US usw..

> **Hinweis:** Wenn Sie ARMClient von Powershell ausführen, müssen Sie das Escapezeichen für die @ Symbol für die JSON-Datei mit einem Häkchen zurück '.

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Zugreifen auf das Client-Zertifikat aus der Web-App ##
Wenn Sie ASP.NET verwenden und der app konfigurieren, um Client Zertifikatauthentifizierung verwenden, wird das Zertifikat der Eigenschaft **HttpRequest.ClientCertificate** verfügbar sein. Für andere Stapel Anwendung wird das Client-Zertifikat in Ihrer app durch einen base64-codierte Wert in der Anfrage-Header "X ARR ClientCert" zur Verfügung. Die Anwendung kann erstellen ein Zertifikat aus diesen Wert und diese dann für Authentifizierung und Autorisierung Zwecke in Ihrer Anwendung verwenden.

## <a name="special-considerations-for-certificate-validation"></a>Besonderheiten bei der Validierung von Zertifikaten ##
Das Client-Zertifikat, das mit der Anwendung gesendet wurde geht nicht über jede Validierung von der Azure Web Apps-Plattform. Überprüfung dieses Zertifikats ist rein des Web app. So sieht Beispiel ASP.NET-Code, das Zertifikateigenschaften zwecks Authentifizierung überprüft aus.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
