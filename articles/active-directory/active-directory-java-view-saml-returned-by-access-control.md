<properties
    pageTitle="Προβολή SAML που επιστράφηκε από την υπηρεσία ελέγχου πρόσβασης (Java)"
    description="Μάθετε πώς μπορείτε να προβάλετε SAML που επιστράφηκε από την υπηρεσία ελέγχου πρόσβασης σε εφαρμογές Java που φιλοξενούνται στο Azure."
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

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Πώς μπορείτε να προβάλετε SAML που επιστράφηκε από την υπηρεσία ελέγχου πρόσβασης Azure

Αυτός ο οδηγός θα σας δείξουν πώς μπορείτε να προβάλετε το υποκείμενο ασφαλείας διεκδίκηση σήμανσης γλώσσας (SAML) επιστρέφονται στην εφαρμογή σας από την υπηρεσία ελέγχου πρόσβασης Azure (ACS). Ο οδηγός δημιουργεί στο θέμα [πώς μπορείτε να τον έλεγχο ταυτότητας χρηστών του Web με το Azure πρόσβασης ελέγχου υπηρεσία χρησιμοποιώντας Έκλειψη][] , παρέχοντας κώδικα που εμφανίζει τις πληροφορίες SAML. Οι δυνατότητες της εφαρμογής θα είναι παρόμοιο με το εξής.

![Παράδειγμα SAML εξόδου][saml_output]

Για περισσότερες πληροφορίες σχετικά με την ACS, ανατρέξτε στην ενότητα [επόμενα βήματα](#next_steps) .

> [AZURE.NOTE]
> Το φίλτρο Azure Access υπηρεσίες ελέγχου είναι μια προεπισκόπηση τεχνολογία Κοινότητας. Ως προέκδοσης λογισμικού, δεν τυπικά υποστηρίζεται από τη Microsoft.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τις εργασίες σε αυτόν τον οδηγό, ολοκληρώστε το δείγμα πώς μπορείτε [να τον έλεγχο ταυτότητας χρηστών του Web με το Azure πρόσβασης ελέγχου υπηρεσία χρησιμοποιώντας Έκλειψη][] και χρησιμοποιήστε την ως σημείο εκκίνησης για αυτό το πρόγραμμα εκμάθησης.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Προσθήκη στη βιβλιοθήκη JspWriter σας δόμηση συγκρότησης διαδρομή και ανάπτυξη

Προσθέστε τη βιβλιοθήκη που περιέχει την κλάση **javax.servlet.jsp.JspWriter** για να σας δόμηση συγκρότησης διαδρομή και ανάπτυξη. Εάν χρησιμοποιείτε Tomcat, η βιβλιοθήκη είναι **jsp api.jar**, που βρίσκεται στο φάκελο **βιβλιοθήκης** Apache.

1. Στην Εξερεύνηση έργου του Έκλειψη, κάντε δεξί κλικ **MyACSHelloWorld**, κάντε κλικ στην επιλογή **Δημιουργία διαδρομής**, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Δημιουργία διαδρομής**, κάντε κλικ στην καρτέλα **βιβλιοθήκες** , και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη εξωτερικών JARs**.
2. Στο παράθυρο διαλόγου **Επιλογής ΒΆΖΩΝ** , μεταβείτε το ΒΆΖΟ είναι απαραίτητο, επιλέξτε το και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα**.
3. Με το παράθυρο διαλόγου **Ιδιότητες για MyACSHelloWorld** ακόμη ανοιχτή, κάντε κλικ στο κουμπί **Ανάπτυξης συγκρότησης**.
4. Στο παράθυρο διαλόγου **Συγκρότησης ανάπτυξης Web** , κάντε κλικ στην επιλογή **Προσθήκη**.
5. Στο παράθυρο διαλόγου **Νέα οδηγία συγκρότησης** , κάντε κλικ στην επιλογή **Καταχωρήσεις διαδρομή Δόμηση Java** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
6. Επιλέξτε την κατάλληλη βιβλιοθήκη και κάντε κλικ στο κουμπί **Τέλος**.
7. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου **Ιδιότητες για MyACSHelloWorld** .

## <a name="modify-the-jsp-file-to-display-saml"></a>Τροποποιήστε το αρχείο JSP για να εμφανίσετε SAML

Τροποποίηση **index.jsp** για να χρησιμοποιήσετε τον παρακάτω κώδικα.

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

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

1. Εκτελέστε την εφαρμογή σας σε προσομοίωσης του υπολογιστή ή ανάπτυξη Azure, ακολουθώντας τα βήματα που τεκμηριώνονται πώς μπορείτε [να τον έλεγχο ταυτότητας χρηστών του Web με το Azure πρόσβασης ελέγχου υπηρεσία χρησιμοποιώντας Έκλειψη][].
2. Εκκίνηση ενός προγράμματος περιήγησης και ανοίξτε την εφαρμογή web. Αφού συνδεθείτε στην εφαρμογή σας, θα δείτε SAML πληροφορίες, όπως τη διεκδίκηση ασφαλείας που παρέχεται από την υπηρεσία παροχής ταυτότητας.

## <a name="next-steps"></a>Επόμενα βήματα

Για περαιτέρω Εξερεύνηση λειτουργικότητα του ACS και να πειραματιστείτε με πιο εξελιγμένη σενάρια, ανατρέξτε στο θέμα [Access 2.0 υπηρεσία ελέγχου][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Η υπηρεσία ελέγχου Access 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Πώς μπορείτε να τον έλεγχο ταυτότητας χρηστών του Web με την υπηρεσία ελέγχου πρόσβασης Azure χρησιμοποιώντας Έκλειψη]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 