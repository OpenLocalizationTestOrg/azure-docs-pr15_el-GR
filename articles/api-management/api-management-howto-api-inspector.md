<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε την επιθεώρηση API για ανίχνευση κλήσεις Azure API διαχείρισης" 
    description="Μάθετε πώς να ανιχνεύσετε κλήσεων με την επιθεώρηση API Azure API διαχείρισης." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Πώς μπορείτε να χρησιμοποιήσετε την επιθεώρηση API για ανίχνευση κλήσεις Azure API διαχείρισης

API διαχείρισης παρέχει ένα εργαλείο επιθεώρηση API για να σας βοηθήσει με τον εντοπισμό σφαλμάτων και αντιμετώπιση προβλημάτων APIs σας. Η επιθεώρηση API μπορεί να χρησιμοποιηθεί μέσω προγραμματισμού και μπορούν να χρησιμοποιηθούν απευθείας από την πύλη για προγραμματιστές. 

Εκτός από την ανίχνευση λειτουργίες, API επιθεώρηση παρακολουθεί επίσης αξιολογήσεις [έκφραση πολιτικής](https://msdn.microsoft.com/library/azure/dn910913.aspx) . Για μια επίδειξη, ανατρέξτε στο θέμα [177 επεισοδίου εξώφυλλο Cloud: περισσότερες δυνατότητες διαχείρισης API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) και προώθηση ταινίας σε 21:00.

Αυτός ο οδηγός παρέχει μια Γνωρίστε χρήση API επιθεώρηση.

>[AZURE.NOTE] Επιθεώρηση API ανιχνεύσεις μόνο δημιουργούνται και διατίθενται για αιτήσεις που περιέχει τα πλήκτρα συνδρομή που ανήκει ο λογαριασμός [διαχειριστή](api-management-howto-create-groups.md) .

## <a name="trace-call"></a> Χρήση API επιθεώρηση για να παρακολουθήσετε μια κλήση

Για να χρησιμοποιήσετε το API επιθεώρηση, προσθέστε μια **ocp-apim-ανίχνευσης: true** κεφαλίδα στην κλήση λειτουργίας, αίτησης και, στη συνέχεια, κάντε λήψη και έλεγχος της ανίχνευσης χρησιμοποιώντας τη διεύθυνση URL που υποδεικνύεται από την κεφαλίδα απόκρισης **ocp-apim-ανίχνευση-θέση** . Αυτό μπορεί να γίνει ορθογώνιο και μπορεί να γίνει απευθείας από την πύλη για προγραμματιστές.

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε την επιθεώρηση API για λειτουργίες ανίχνευση, χρησιμοποιώντας το βασικό API Αριθμομηχανής που έχει ρυθμιστεί στην εκμάθηση γρήγορα αποτελέσματα [Διαχείριση το πρώτο API](api-management-get-started.md) . Εάν δεν έχετε ολοκληρώσει το πρόγραμμα εκμάθησης που χρειάζονται μόνο λίγα λεπτά για να εισαγάγετε το βασικό API Αριθμομηχανής ή μπορείτε να χρησιμοποιήσετε μια άλλη API της επιλογής σας όπως το API Αντήχηση. Κάθε παρουσία της υπηρεσίας διαχείρισης API περιλαμβάνει προκαθορισμένες ρυθμίσεις παραμέτρων με API Αντήχηση που μπορούν να χρησιμοποιηθούν για να πειραματιστείτε με και ενημερωθείτε σχετικά με το API διαχείρισης. Το API Αντήχηση επιστρέφει πίσω όποιο εισαγωγής αποστέλλεται σε αυτήν. Για να χρησιμοποιήσετε, μπορείτε να ενεργοποιήσετε τις Ρηματικές HTTP και η τιμή που επιστρέφεται θα είναι απλώς ό, τι στέλνετε. 



Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Προγραμματιστής πύλης** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Λειτουργίες μπορεί να ονομάζεται απευθείας από την πύλη για προγραμματιστές που παρέχει έναν εύχρηστο τρόπο για να προβάλετε και να ελέγξετε τις λειτουργίες του API.

>Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

![Πύλη διαχείρισης API για προγραμματιστές][api-management-developer-portal-menu]

Κάντε κλικ στην επιλογή **APIs** από το επάνω μενού και, στη συνέχεια, κάντε κλικ στην επιλογή **Βασική Αριθμομηχανή**.

![API αντήχηση][api-management-api]

Κάντε κλικ στην επιλογή **Δοκιμή** για να δοκιμάσετε τη λειτουργία **Προσθήκη δύο ακέραιους αριθμούς** .

![Δοκιμάστε το][api-management-open-console]

Διατήρηση της προεπιλεγμένης τιμές παραμέτρων και επιλέξτε το πλήκτρο συνδρομή για το προϊόν που θέλετε να χρησιμοποιήσετε από το **κλειδί συνδρομή** αναπτυσσόμενη λίστα.

Από προεπιλογή στην πύλη για προγραμματιστές του **Ocp-Apim-ανίχνευση** κεφαλίδα ήδη έχει οριστεί στην **τιμή true**. Αυτή η κεφαλίδα ρυθμίζει τις παραμέτρους δημιουργείται μια ανίχνευση ή όχι.

![Αποστολή][api-management-http-get]

Κάντε κλικ στην επιλογή **Αποστολή** για να καλέσετε τη λειτουργία.

![Αποστολή][api-management-send-results]

Στην απάντηση κεφαλίδες θα είναι ένα **ocp-apim-ανίχνευση-θέση** με μια τιμή που είναι παρόμοιο με το παρακάτω παράδειγμα.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Ανίχνευση μπορεί να ληφθεί από την καθορισμένη θέση και να αναθεωρήσετε όπως φαίνεται στο επόμενο βήμα.

## <a name="inspect-trace"> </a>Έλεγχος της ανίχνευσης

Για να δείτε τις τιμές στην ανίχνευση, λήψης του αρχείου ανίχνευσης από τη διεύθυνση URL **ocp-apim-ανίχνευση-θέση** . Αυτό είναι ένα αρχείο κειμένου σε μορφή JSON και περιέχει καταχωρήσεις παρόμοιο με το παρακάτω παράδειγμα.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Επόμενα βήματα

-   Παρακολουθήστε μια επίδειξη της ανίχνευσης πολιτικής παραστάσεων σε [177 επεισοδίου εξώφυλλο Cloud: περισσότερες δυνατότητες διαχείρισης API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Γρήγορη προώθηση σε 21:00 για να δείτε την επίδειξη.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 