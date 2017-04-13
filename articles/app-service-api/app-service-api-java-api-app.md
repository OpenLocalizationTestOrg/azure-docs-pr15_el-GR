<properties
    pageTitle="Δημιουργήστε και αναπτύξτε μια εφαρμογή Java API στο Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα πακέτο εφαρμογών Java API και να αναπτύξετε για Azure εφαρμογής υπηρεσίας."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Δημιουργήστε και αναπτύξτε μια εφαρμογή Java API στο Azure εφαρμογής υπηρεσίας

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή Java και αναπτύξει API υπηρεσίας εφαρμογών Azure χρησιμοποιώντας [Git]. Μπορείτε να ακολουθήσετε τις οδηγίες σε αυτό το πρόγραμμα εκμάθησης σε κανένα λειτουργικό σύστημα που μπορεί να εκτελέσει Java. Ο κώδικας σε αυτό το πρόγραμμα εκμάθησης είναι δημιουργούνται χρησιμοποιώντας [Maven]. [Jax-r] χρησιμοποιείται για να δημιουργήσετε την υπηρεσία RESTful και δημιουργείται με βάση τις προδιαγραφές μετα-δεδομένων [Swagger] χρησιμοποιώντας τον [Swagger επεξεργασίας].

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

1. [Κιτ Java για προγραμματιστές 8] \(ή νεότερη έκδοση)
1. [Maven] εγκατεστημένο στον υπολογιστή σας ανάπτυξης
1. [Git] εγκατεστημένο στον υπολογιστή σας ανάπτυξης
1. Μια συνδρομή επί πληρωμή ή [δωρεάν δοκιμαστική έκδοση] στο [Microsoft Azure]
1. Μια εφαρμογή δοκιμής HTTP, όπως [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Το API χρησιμοποιώντας Swagger.IO scaffold

Χρήση του προγράμματος επεξεργασίας online swagger.io, μπορείτε να εισαγάγετε Swagger JSON ή YAML κώδικα που αντιπροσωπεύει τη δομή του API σας. Μόλις το API επιφάνεια έχει σχεδιαστεί, μπορείτε να εξαγάγετε κώδικα για μια ποικιλία πλατφόρμες και πλαισίων. Στην επόμενη ενότητα, τον κωδικό scaffolded θα τροποποιηθούν για να συμπεριλάβετε τη λειτουργία mock. 

Αυτή η επίδειξη θα ξεκινήσει με ένα σώμα Swagger JSON που θα επικολλήσετε στο πρόγραμμα επεξεργασίας swagger.io, η οποία θα χρησιμοποιηθεί για τη δημιουργία κώδικα με τη χρήση του JAX-r για να αποκτήσετε πρόσβαση σε ένα τελικό σημείο REST API. Στη συνέχεια, που θα επεξεργαστείτε τον κώδικα scaffolded για να επιστρέψετε mock δεδομένα, προσομοίωση μια ενσωματωμένη επάνω μηχανισμό διατήρησης δεδομένων REST API.  

1. Αντιγράψτε τον παρακάτω κώδικα Swagger JSON στο Πρόχειρο:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Μεταβείτε το [Online Swagger προγράμματος επεξεργασίας]. Μία φορά, κάντε κλικ στην επιλογή στο στοιχείο μενού **Αρχείο -> Επικόλληση JSON** .

    ![Στοιχείο μενού JSON επικόλλησης][paste-json]

1. Επικολλήστε το επαφές λίστα API Swagger JSON που αντιγράψατε προηγουμένως. 

    ![Επικόλληση JSON κώδικα στο Swagger][pasted-swagger]

1. Προβολή των σελίδων τεκμηρίωση και σύνοψη API απόδοσης στο πρόγραμμα επεξεργασίας. 

    ![Προβολή Swagger που δημιουργούνται από έγγραφα][view-swagger-generated-docs]

1. Ενεργοποιήστε την επιλογή μενού **Δημιουργία διακομιστή-> JAX RS** για να scaffold τον κώδικα πλευρά του διακομιστή που θα επεξεργαστείτε αργότερα για να προσθέσετε mock υλοποίηση. 

    ![Δημιουργία στοιχείου μενού κώδικα][generate-code-menu-item]

    Όταν δημιουργείται τον κώδικα, θα σας δώσει ένα αρχείο ZIP για να κάνετε λήψη. Αυτό το αρχείο περιέχει τον κώδικα scaffolded κατά τη διαδικασία δημιουργίας κώδικα Swagger και όλες που σχετίζονται Δημιουργήστε δεσμών ενεργειών. Αποσυμπίεση ολόκληρη τη βιβλιοθήκη σε έναν κατάλογο στο σας σταθμούς εργασίας ανάπτυξης. 

## <a name="edit-the-code-to-add-api-implementation"></a>Επεξεργαστείτε τον κώδικα για να προσθέσετε υλοποίηση API

Σε αυτήν την ενότητα, θα μπορείτε να αντικαταστήσετε υλοποίησης του κώδικα που δημιουργούνται από το Swagger πλευρά του διακομιστή με τον προσαρμοσμένο κώδικα. Το νέο κωδικό θα επιστρέψει μια οντοτήτων ArrayList της επαφής στο πρόγραμμα-πελάτη κλήσης. 

1. Ανοίξτε το αρχείο μοντέλου *Contact.java* , το οποίο βρίσκεται στο φάκελο *γ/λ/src/java/swagger/εισόδου/εξόδου/μοντέλο* , χρησιμοποιώντας το [Visual Studio κώδικα] ή το πρόγραμμα επεξεργασίας κειμένου Αγαπημένα. 

    ![Άνοιγμα επαφής μοντέλου αρχείου][open-contact-model-file]

1. Προσθέστε την ακόλουθη κατασκευή τάξη **επαφή** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Ανοίξτε το αρχείο εφαρμογής υπηρεσίας *ContactsApiServiceImpl.java* , που βρίσκεται στο φάκελο *src/κύριες/java/εισόδου/εξόδου/swagger/api/impl* , χρησιμοποιώντας το [Visual Studio κώδικα] ή το πρόγραμμα επεξεργασίας κειμένου Αγαπημένα.

    ![Άνοιγμα της επαφής υπηρεσίας αρχείο κώδικα][open-contact-service-code-file]

1. Για να αντικαταστήσετε τον κώδικα στο αρχείο με αυτόν τον νέο κωδικό για να προσθέσετε μια εφαρμογή mock στον κώδικα της υπηρεσίας. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Ανοίξτε μια γραμμή εντολών και αλλάξτε καταλόγου στον ριζικό φάκελο της εφαρμογής σας.

1. Εκτελέστε την παρακάτω εντολή Maven για να δημιουργήσετε τον κώδικα και εκτελέστε το χρησιμοποιώντας το διακομιστή εφαρμογής Jetty τοπικά. 

        mvn package jetty:run

1. Θα πρέπει να βλέπετε το παράθυρο εντολών απεικονίζει ότι Jetty έχει ξεκινήσει τον κωδικό στη θύρα 8080. 

    ![Άνοιγμα της επαφής υπηρεσίας αρχείο κώδικα][run-jetty-war]

1. Χρησιμοποιήστε [Postman] για να κάνετε μια αίτηση "λήψη όλων των επαφών" μέθοδος API στο api/http://localhost:8080/επαφές.

    ![Καλέστε το API επαφών][calling-contacts-api]

1. Χρησιμοποιήστε [Postman] για να κάνετε μια αίτηση για τη μέθοδο API "Λήψη συγκεκριμένη επαφή" βρίσκεται στο http://localhost:8080/api/επαφές/2.

    ![Καλέστε το API επαφών][calling-specific-contact-api]

1. Τέλος, μπορείτε να δημιουργήσετε το αρχείο ΠΟΛΈΜΟΥ Java (αρχειοθήκη Web) εκτελώντας την ακόλουθη εντολή Maven στην κονσόλα σας. 

        mvn package war:war

1. Όταν δημιουργηθεί το αρχείο ΠΟΛΈΜΟΥ, θα τοποθετηθεί στο φάκελο **προορισμού** . Μεταβείτε στο φάκελο **προορισμού** και μετονομάστε το αρχείο ΠΟΛΈΜΟΥ **ROOT.war**. (Βεβαιωθείτε ότι η μετατροπή σε κεφαλαία ταιριάζει με αυτήν τη μορφή).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Τέλος, εκτελέστε τις ακόλουθες εντολές από τον ριζικό φάκελο της εφαρμογής σας για να δημιουργήσετε ένα φάκελο **ανάπτυξης** για να χρησιμοποιήσετε για να αναπτύξετε το αρχείο ΠΟΛΈΜΟΥ Azure. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Δημοσιεύστε το αποτέλεσμα στο Azure εφαρμογής υπηρεσίας

Σε αυτήν την ενότητα θα μάθετε πώς μπορείτε να δημιουργήσετε μια νέα εφαρμογή API με την πύλη Azure, προετοιμασία συγκεκριμένη εφαρμογή API για τη φιλοξενία εφαρμογών Java και να αναπτύξετε το αρχείο ΠΟΛΈΜΟΥ που μόλις δημιουργήσατε να Azure εφαρμογής υπηρεσίας για να εκτελέσετε τη νέα εφαρμογή API. 

1. Δημιουργήστε μια νέα εφαρμογή API στην [πύλη του Azure], κάνοντας κλικ στο στοιχείο μενού **Δημιουργία -> Web + Mobile -> εφαρμογή API** , καταχωρήσετε τις λεπτομέρειες της εφαρμογής σας και, στη συνέχεια, κάνοντας κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία νέας εφαρμογής API][create-api-app]

1. Μετά τη δημιουργία της εφαρμογής σας API, ανοίξτε την εφαρμογή **ρυθμίσεων** blade και, στη συνέχεια, κάντε κλικ στο στοιχείο μενού **Ρυθμίσεις εφαρμογής** . Επιλέξτε τις πιο πρόσφατες εκδόσεις Java από τις διαθέσιμες επιλογές, στη συνέχεια, επιλέξτε την πιο πρόσφατη Tomcat από το μενού **κοντέινερ Web** και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Ρύθμιση του Java στο το blade εφαρμογής API][set-up-java]

1. Κάντε κλικ στο στοιχείο μενού ρυθμίσεις **διαπιστευτηρίων ανάπτυξης** και δώστε ένα όνομα χρήστη και τον κωδικό πρόσβασης που θέλετε να χρησιμοποιήσετε για τη δημοσίευση των αρχείων σας εφαρμογή API. 

    ![Ορισμός διαπιστευτηρίων ανάπτυξης][deployment-credentials]

1. Κάντε κλικ στο στοιχείο μενού ρυθμίσεων **ανάπτυξης προέλευσης** . Μία φορά, κάντε κλικ στο κουμπί **Επιλογή προέλευσης** , επιλέξτε την επιλογή **Τοπικό Git αποθετήριο δεδομένων** και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. Αυτό θα δημιουργήσει ένα αποθετήριο Git εκτελείται στο Azure, που έχει μια συσχέτιση με την εφαρμογή σας API. Κάθε φορά που αποθηκεύετε κώδικα για να το *πρωτότυπο* υποκατάστημα του αποθετηρίου σας Git, τον κωδικό θα δημοσιευτεί σε σας live παρουσία API εφαρμογή εκτελείται. 

    ![Ρυθμίστε ένα νέο αρχείο φύλαξης τοπικό Git][select-git-repo]

1. Αντιγράψτε το νέο αποθετήριο Git διεύθυνση URL στο Πρόχειρο. Αποθηκεύστε αυτήν ως θα είναι σημαντικές σε λίγο. 

    ![Ρυθμίστε ένα νέο αρχείο φύλαξης Git για την εφαρμογή σας][copy-git-repo-url]

1. Git push του αρχείου ΠΟΛΈΜΟΥ στο αποθετήριο online. Για να το κάνετε αυτό, μεταβείτε στο φάκελο **Ανάπτυξη** που δημιουργήσατε νωρίτερα, έτσι ώστε να μπορείτε εύκολα να ολοκληρώσετε τον κώδικα έως το αποθετήριο εκτελείται στην υπηρεσία εφαρμογής. Όταν βρίσκεστε στο παράθυρο της κονσόλας και μεταβείτε στο φάκελο όπου βρίσκεται ο φάκελος webapps, θεμάτων τις παρακάτω εντολές Git για εκκίνηση της διαδικασίας και fire απενεργοποίηση μιας ανάπτυξης. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Αφού εκδώσετε την αίτηση **push** , θα σας ζητηθεί για τον κωδικό πρόσβασης που δημιουργήσατε νωρίτερα για τα διαπιστευτήρια ανάπτυξης. Αφού εισαγάγετε τα διαπιστευτήριά σας, θα πρέπει να βλέπετε την πύλη εμφανίζεται ότι έχει αναπτυχθεί την ενημέρωση.

1. Εάν χρησιμοποιείτε το Postman ξανά για να πατήσετε την εφαρμογή API που μόλις έχει αναπτυχθεί εκτελείται στο Azure εφαρμογής υπηρεσίας, θα δείτε ότι η συμπεριφορά είναι συνεπείς και ότι τώρα το επιστρέφει τα δεδομένα με τον αναμενόμενο τρόπο και με χρήση κώδικα απλές αλλαγές για να το Swagger.io scaffolded κώδικα Java. 

    ![Χρήση του Java επαφές REST API live στο Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο μπορέσατε να ξεκινήσετε με ένα αρχείο Swagger JSON και ορισμένα scaffolded κώδικα Java που λαμβάνονται από το πρόγραμμα επεξεργασίας Swagger.io. Από εκεί, απλή τις αλλαγές σας και ένα Git ανάπτυξη διαδικασίας οδήγησε σε μια λειτουργική εφαρμογή API γραμμένο σε Java χρειάζεται. Η επόμενη προγραμμάτων εκμάθησης δείχνει τον τρόπο για την [εκμετάλλευση API εφαρμογές από προγράμματα-πελάτες JavaScript, χρησιμοποιώντας CORS][App Service API CORS]. Νεότερη έκδοση προγράμματα εκμάθησης σε μια σειρά δείχνουν πώς μπορείτε να υλοποιήσετε τον έλεγχο ταυτότητας και εξουσιοδότηση.

Για να δημιουργήσετε σε αυτό το δείγμα, μπορείτε να μάθετε περισσότερα σχετικά με το [Χώρο αποθήκευσης SDK για Java] για να διατηρηθούν τα αντικείμενα BLOB JSON. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε το [SDK Java DB εγγράφου] για να αποθηκεύσετε δεδομένα των επαφών σας στο Azure DB εγγράφου. 

Για περισσότερες πληροφορίες σχετικά με τη χρήση Java Azure, ανατρέξτε στο [Κέντρο για προγραμματιστές Java].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Πύλη του Azure]: https://portal.azure.com/
[Έγγραφο DB Java SDK]: ../documentdb/documentdb-java-application.md
[δωρεάν δοκιμαστική έκδοση]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Κέντρο για προγραμματιστές Java]: /develop/java/
[Κιτ Java για προγραμματιστές 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Πρόγραμμα επεξεργασίας Online Swagger]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Χώρος αποθήκευσης SDK για Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Πρόγραμμα επεξεργασίας swagger]: http://editor.swagger.io/
[Κώδικα Visual Studio]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
