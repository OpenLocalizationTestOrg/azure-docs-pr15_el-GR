<properties
    pageTitle="Δημιουργία Hadoop, HBase ή καταιγίδας συμπλεγμάτων σε Linux σε χρήση καμπύλη και το REST API Azure HDInsight | Microsoft Azure"
    description="Μάθετε πώς να δημιουργείτε χρησιμοποιώντας καμπύλη, πρότυπα διαχείρισης πόρων Azure και το Azure REST API συμπλεγμάτων βάσει Linux HDInsight. Μπορείτε να καθορίσετε τον τύπο συμπλέγματος (Hadoop, HBase ή καταιγίδας) ή χρησιμοποιήστε δέσμες ενεργειών για την εγκατάσταση προσαρμοσμένα στοιχεία."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Δημιουργία βάσει Linux συμπλεγμάτων σε χρήση καμπύλη και το REST API Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Το REST API Azure σάς επιτρέπει να εκτελέσετε λειτουργίες διαχείρισης σχετικά με τις υπηρεσίες που φιλοξενούνται στην Azure πλατφόρμα, συμπεριλαμβανομένης της δημιουργίας νέων πόρων, όπως το HDInsight βάσει Linux συμπλεγμάτων. Σε αυτό το έγγραφο, θα μάθετε πώς μπορείτε να δημιουργήσετε Azure από διαχειριστή πόρων για να ρυθμίσετε ένα σύμπλεγμα HDInsight και σχετικές χώρου αποθήκευσης και, στη συνέχεια, χρησιμοποιήστε καμπύλη για να αναπτύξετε το πρότυπο για να το Azure REST API για να δημιουργήσετε ένα νέο σύμπλεγμα HDInsight.

> [AZURE.IMPORTANT] Τα βήματα σε αυτό το έγγραφο χρησιμοποιούν ο προεπιλεγμένος αριθμός εργαζόμενου κόμβους (4) για ένα σύμπλεγμα HDInsight. Εάν σκοπεύετε να υπερβαίνει τα 32 εργαζόμενου κόμβους, είτε στο σύμπλεγμα δημιουργίας ή την κλίμακα του συμπλέγματος μετά τη δημιουργία, στη συνέχεια, πρέπει να επιλέξετε ένα μέγεθος κεφαλής κόμβο με τουλάχιστον 8 πυρήνων και 14GB ram.
>
> Για περισσότερες πληροφορίες σχετικά με μεγέθη κόμβο και σχετικές κόστος, ανατρέξτε στο θέμα [HDInsight τις τιμές](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. Το Azure CLI χρησιμοποιείται για να δημιουργήσετε μια υπηρεσία κεφαλαίου, η οποία χρησιμοποιείται για τη δημιουργία κωδικοί ελέγχου ταυτότητας για αιτήσεις για το Azure REST API.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. Αυτό το βοηθητικό πρόγραμμα μέσω του συστήματος διαχείρισης πακέτου ή μπορούν να ληφθούν από [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Εάν χρησιμοποιείτε το PowerShell για να εκτελέσετε τις εντολές σε αυτό το έγγραφο, πρέπει πρώτα να καταργήσετε το `curl` ψευδώνυμο που δημιουργεί από προεπιλογή. Αυτό το ψευδώνυμο χρησιμοποιεί ενεργοποίηση-WebRequest, ένα cmdlet του PowerShell, αντί για καμπύλη όταν χρησιμοποιείτε το `curl` εντολή από μια γραμμή εντολών του PowerShell και θα επιστρέφουν σφάλματα για πολλές από τις εντολές που χρησιμοποιούνται σε αυτό το έγγραφο.
    > 
    > Για να καταργήσετε αυτό το ψευδώνυμο, χρησιμοποιήστε τα ακόλουθα από τη γραμμή εντολών του PowerShell:
    >
    > `Remove-item alias:curl`
    >
    > Όταν έχει καταργηθεί το ψευδώνυμο, θα πρέπει να μπορείτε να χρησιμοποιήσετε την έκδοση του καμπύλη που έχετε εγκαταστήσει στο σύστημά σας.

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Δημιουργία προτύπου

Azure πρότυπα διαχείρισης πόρων είναι JSON έγγραφα που περιγράφουν μια __ομάδα πόρων__ και όλους τους πόρους του (όπως το HDInsight.) Αυτή η προσέγγιση με βάση το πρότυπο σάς επιτρέπει να για να ορίσετε όλους τους πόρους που χρειάζεστε για HDInsight σε ένα πρότυπο και να διαχειριστείτε τις αλλαγές στην ομάδα ως σύνολο μέσω __αναπτύξεις__ που ισχύουν αλλαγές στην ομάδα.

Πρότυπα παρέχονται συνήθως στα δύο μέρη. το ίδιο το πρότυπο και ένα αρχείο παράμετροι που μπορείτε να συμπληρώσετε με συγκεκριμένες τιμές στη ρύθμιση παραμέτρων σας. Για exmaple, το όνομα του συμπλέγματος, όνομα διαχειριστή και τον κωδικό πρόσβασης. Όταν χρησιμοποιείτε απευθείας το REST API, πρέπει να συνδυάσετε τα εξής σε ένα αρχείο. Η μορφή αυτού του εγγράφου JSON είναι:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Για παράδειγμα, ακολουθεί μια συγχώνευση των αρχείων προτύπου και τις παραμέτρους από [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), που δημιουργεί ένα σύμπλεγμα βάσει Linux χρησιμοποιώντας έναν κωδικό πρόσβασης για την ασφάλιση ο λογαριασμός χρήστη SSH.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

Αυτό το παράδειγμα θα χρησιμοποιηθεί στα βήματα σε αυτό το έγγραφο. Πρέπει να αντικαταστήσετε το σύμβολο κράτησης θέσης _τιμές_ στην ενότητα __παράμετροι__ στο τέλος του εγγράφου με τις τιμές που θέλετε να χρησιμοποιήσετε για το σύμπλεγμά σας.

##<a name="login-to-your-azure-subscription"></a>Συνδεθείτε στη συνδρομή σας στο Azure

Ακολουθήστε τα βήματα που τεκμηριώνονται σε [σύνδεση με μια συνδρομή του Azure από το περιβάλλον γραμμής εντολών Azure (Azure CLI)](../xplat-cli-connect.md) και να συνδεθείτε με σας χρησιμοποιώντας τη συνδρομή του `azure login` εντολή.

##<a name="create-a-service-principal"></a>Δημιουργία αρχής υπηρεσίας

> [AZURE.NOTE] Αυτά τα βήματα είναι ένα συνοπτικό έκδοση των πληροφοριών που παρέχεται στην ενότητα _Έλεγχος ταυτότητας υπηρεσίας κεφαλαίου με κωδικό πρόσβασης - Azure CLI_ του εγγράφου [τον έλεγχο ταυτότητας αρχής υπηρεσίας με τη διαχείριση πόρων Azure](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Αυτά τα βήματα Δημιουργήστε μια νέα υπηρεσία κεφαλαίου που μπορεί να χρησιμοποιηθεί για τον έλεγχο ταυτότητας τις αιτήσεις REST API που χρησιμοποιούνται για τη δημιουργία Azure πόρους, όπως ένα σύμπλεγμα HDInsight.

1. Από τη γραμμή εντολών, περιόδου λειτουργίας τερματικού ή κέλυφος, χρησιμοποιήστε την ακόλουθη εντολή για μια λίστα των συνδρομών σας Azure.

        azure account list
        
    Στη λίστα, επιλέξτε τη συνδρομή που θέλετε να χρησιμοποιήσετε το και σημειώστε τη στήλη " __αναγνωριστικό__ ". Αυτό είναι το __Αναγνωριστικό συνδρομής__ και θα χρησιμοποιηθεί στα περισσότερα από τα βήματα σε αυτό το έγγραφο.

2. Δημιουργία νέας εφαρμογής στο Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Αντικαταστήστε τις τιμές για το `--name`, `--home-page`, και `--identifier-uris` με τις δικές σας τιμές. Δώστε έναν κωδικό πρόσβασης για τη νέα εγγραφή υπηρεσίας καταλόγου Active Directory.
    
    > [AZURE.NOTE] Καθώς δημιουργείτε αυτής της εφαρμογής για τον έλεγχο ταυτότητας μέσω αρχής υπηρεσίας, η `--home-page` και `--identifier-uris` τιμές δεν χρειάζεται να αναφέρονται σε μια πραγματική σελίδα web που φιλοξενείται στο internet. Μόλις πρέπει να είναι μοναδικό URIs.
    
    Από τα δεδομένα που επιστρέφονται, αποθηκεύστε την τιμή __αναγνωριστικό εφαρμογής__ .
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Δημιουργήστε μια υπηρεσία κεφαλαίου χρησιμοποιώντας την τιμή __αναγνωριστικό εφαρμογής__ επιστρέφονται προηγουμένως.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Από τα δεδομένα που επιστρέφονται, αποθηκεύστε την τιμή __Αναγνωριστικό αντικειμένου__ .
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Να αναθέσει το ρόλο __κατόχου__ για την υπηρεσία κεφαλαίου χρησιμοποιώντας την τιμή __Αναγνωριστικό αντικειμένου__ επιστρέφονται προηγουμένως. Μπορείτε, επίσης, πρέπει να χρησιμοποιήσετε το __Αναγνωριστικό συνδρομής__ που λάβατε νωρίτερα.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Μόλις ολοκληρωθεί αυτή την εντολή, η υπηρεσία κεφαλαίου τώρα έχει πρόσβαση κατόχου για το αναγνωριστικό της καθορισμένης συνδρομής.

##<a name="get-an-authentication-token"></a>Λήψη ενός διακριτικού ελέγχου ταυτότητας

1. Χρησιμοποιήστε τα ακόλουθα για να βρείτε το __Αναγνωριστικό μισθωτή__ για τη συνδρομή σας.

        azure account show -s <subscription ID>
        
    Από τα δεδομένα που επιστρέφονται, βρείτε το __Αναγνωριστικό μισθωτή__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Δημιουργήστε έναν νέο κωδικό χρησιμοποιώντας το Azure REST API.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Αντικατάσταση __TenantID__, __αναγνωριστικό εφαρμογής__και __τον κωδικό πρόσβασης__ με τις τιμές που λαμβάνονται ή χρησιμοποιείται ήδη.

    Εάν αυτή η αίτηση είναι επιτυχής, θα λαμβάνετε μια απάντηση 200 σειρά και σώμα απάντησης θα περιέχει ένα έγγραφο JSON.

    Το έγγραφο JSON που επιστρέφονται από αυτή την αίτηση θα περιέχει ένα στοιχείο με το όνομα __access_token__; η τιμή αυτού του στοιχείου είναι το διακριτικό πρόσβασης, πρέπει να χρησιμοποιήσετε με τον έλεγχο ταυτότητας των αιτήσεων που χρησιμοποιούνται στις επόμενες ενότητες αυτού του εγγράφου.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε μια νέα ομάδα πόρων. Πρέπει πρώτα να δημιουργήσετε την ομάδα να μπορέσετε να δημιουργήσετε τους πόρους, όπως το HDInsight σύμπλεγμα. 

* Αντικαταστήστε __SubscriptionID__ με το Αναγνωριστικό συνδρομής που λαμβάνεται κατά τη δημιουργία του αρχικού υπηρεσίας.
* Αντικαταστήστε __AccessToken__ με το διακριτικό πρόσβασης που λάβατε στο προηγούμενο βήμα.
* Αντικαταστήστε __DataCenterLocation__ με το κέντρο δεδομένων που θέλετε να δημιουργήσετε την ομάδα πόρων και τους πόρους στο. Για παράδειγμα, 'Νότια κεντρικής ΜΑΣ'. 
* Αντικαταστήστε το __ResourceGroupName__ με το όνομα που θέλετε να χρησιμοποιήσετε για αυτήν την ομάδα:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Εάν αυτή η αίτηση είναι επιτυχής, θα λαμβάνετε μια απάντηση 200 σειρά και σώμα απάντησης θα περιέχει ένα έγγραφο JSON που περιέχει πληροφορίες σχετικά με την ομάδα. Το `"provisioningState"` στοιχείο θα περιέχει μια τιμή του `"Succeeded"`.

##<a name="create-a-deployment"></a>Δημιουργία μιας ανάπτυξης

Χρησιμοποιήστε τα ακόλουθα για να αναπτύξετε τη ρύθμιση παραμέτρων του συμπλέγματος (πρότυπο και παράμετρο, οι τιμές) στην ομάδα πόρων.

* Αντικατάσταση __SubscriptionID__ και __AccessToken__ με τις τιμές που χρησιμοποιούνται προηγουμένως. 
* Αντικαταστήστε __ResourceGroupName__ με το όνομα της ομάδας πόρων που δημιουργήσατε στην προηγούμενη ενότητα.
* Αντικατάσταση __DeploymentName__ με το όνομα που θέλετε να χρησιμοποιήσετε για αυτήν την ανάπτυξη.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Εάν έχετε αποθηκεύσει το έγγραφο JSON που περιέχει το πρότυπο και τις παραμέτρους σε ένα αρχείο, μπορείτε να χρησιμοποιήσετε τα παρακάτω αντί `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Εάν αυτή η αίτηση είναι επιτυχής, θα λαμβάνετε μια απάντηση 200 σειρά και σώμα απάντησης θα περιέχει ένα έγγραφο JSON που περιέχει πληροφορίες σχετικά με τη λειτουργία ανάπτυξη.

> [AZURE.IMPORTANT] Σημειώστε ότι η ανάπτυξη έχει υποβληθεί, αλλά δεν έχει ολοκληρωθεί αυτήν τη στιγμή. Μπορεί να χρειαστούν αρκετά λεπτά, συνήθως είναι περίπου 15, για την ανάπτυξη για να ολοκληρωθεί.

##<a name="check-the-status-of-a-deployment"></a>Έλεγχος της κατάστασης μιας ανάπτυξης

Για να ελέγξετε την κατάσταση της ανάπτυξης, χρησιμοποιήστε τα εξής:

* Αντικατάσταση __SubscriptionID__ και __AccessToken__ με τις τιμές που χρησιμοποιούνται προηγουμένως. 
* Αντικαταστήστε __ResourceGroupName__ με το όνομα της ομάδας πόρων που δημιουργήσατε στην προηγούμενη ενότητα.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Αυτό θα επιστρέψει πληροφορίες JSON εγγράφου που περιέχει πληροφορίες σχετικά με τη λειτουργία ανάπτυξη. Το `"provisioningState"` στοιχείο θα περιέχει την κατάσταση της ανάπτυξης; Εάν αυτό περιέχει μια τιμή του `"Succeeded"`, στη συνέχεια, την ανάπτυξη ολοκληρώθηκε με επιτυχία. Σε αυτό το σημείο, το σύμπλεγμά σας πρέπει να είναι διαθέσιμη για χρήση.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει ένα σύμπλεγμα HDInsight με επιτυχία, χρησιμοποιήστε τα παρακάτω για να μάθετε πώς μπορείτε να εργαστείτε με το σύμπλεγμά σας. 

###<a name="hadoop-clusters"></a>Hadoop συμπλεγμάτων

* [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)
* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
* [Χρήση MapReduce με HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase συμπλεγμάτων

* [Γρήγορα αποτελέσματα με το HBase σε HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ανάπτυξη εφαρμογών Java για HBase σε HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Καταιγίδας συμπλεγμάτων

* [Ανάπτυξη τοπολογίες Java για καταιγίδας στην HDInsight](hdinsight-storm-develop-java-topology.md)
* [Χρησιμοποιήστε τα στοιχεία Python στο καταιγίδας στην HDInsight](hdinsight-storm-develop-python-topology.md)
* [Ανάπτυξη και εποπτεία τοπολογίες με καταιγίδας στην HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
