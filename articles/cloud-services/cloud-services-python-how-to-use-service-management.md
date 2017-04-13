<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε το API (Python) - Οδηγός για τη δυνατότητα διαχείρισης υπηρεσίας"
    description="Μάθετε πώς να μέσω προγραμματισμού εκτελείτε συνήθεις εργασίες διαχείρισης υπηρεσίας από Python."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Πώς μπορείτε να χρησιμοποιήσετε την τεχνική υποστήριξη από Python

> [AZURE.NOTE] Το API της υπηρεσίας διαχείρισης που αντικαθίσταται με το νέο πόρο διαχείρισης API, αυτήν τη στιγμή διαθέσιμα σε μια έκδοση προεπισκόπησης.  Ανατρέξτε στην [τεκμηρίωση διαχείρισης πόρων Azure](http://azure-sdk-for-python.readthedocs.org/) για λεπτομέρειες σχετικά με τη χρήση του νέου API διαχείρισης πόρων από Python.

Αυτός ο οδηγός σας δείχνει πώς να μέσω προγραμματισμού εκτελείτε συνήθεις εργασίες διαχείρισης υπηρεσίας από Python. Πρόσβαση μέσω προγραμματισμού σε πολλές από την υπηρεσία διαχείρισης που σχετίζονται με λειτουργίες που είναι διαθέσιμη στην [πύλη του Azure κλασική] υποστηρίζει την κλάση **ServiceManagementService** στο [SDK Azure για Python](https://github.com/Azure/azure-sdk-for-python) [ management-portal] (όπως **τη δημιουργία, ενημέρωση και διαγραφή υπηρεσίες cloud, αναπτύξεις, υπηρεσίες διαχείρισης δεδομένων, και εικονικές μηχανές**). Αυτή η λειτουργία μπορεί να είναι χρήσιμες κατά τη δημιουργία εφαρμογές που χρειάζονται πρόσβαση μέσω προγραμματισμού για τη διαχείριση της υπηρεσίας.

## <a name="WhatIs"> </a>Τι είναι η Διαχείριση υπηρεσίας
Το API διαχείρισης υπηρεσία παρέχει πρόσβαση μέσω προγραμματισμού σε πολλές από τις λειτουργίες διαχείρισης υπηρεσίας μέσω του [Azure κλασική πύλη][management-portal]. Το SDK Azure για Python σάς επιτρέπει να διαχειριστείτε τις υπηρεσίες cloud και λογαριασμούς χώρου αποθήκευσης.

Για να χρησιμοποιήσετε το API διαχείρισης υπηρεσία, πρέπει να [δημιουργήσετε ένα λογαριασμό Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Έννοιες
Το SDK Azure για Python αναδιπλώνεται το [API διαχείρισης υπηρεσίας Azure][svc-mgmt-rest-api], που είναι μια REST API. Όλες οι λειτουργίες API εκτελούνται μέσω SSL και αμοιβαία ελέγχου ταυτότητας, χρησιμοποιώντας πιστοποιητικά X.509 v3. Η υπηρεσία διαχείρισης μπορεί να είναι προσβάσιμα από μέσα σε μια υπηρεσία που εκτελείται στο Azure ή απευθείας μέσω του Internet από οποιαδήποτε εφαρμογή που μπορούν να στείλετε μια αίτηση HTTPS και λαμβάνετε μια απάντηση HTTPS.

## <a name="Installation"> </a>Εγκατάστασης

Όλες οι δυνατότητες που περιγράφονται σε αυτό το άρθρο είναι διαθέσιμες στο το `azure-servicemanagement-legacy` πακέτο, το οποίο μπορείτε να εγκαταστήσετε χρησιμοποιώντας pip. Για περισσότερες λεπτομέρειες σχετικά με την εγκατάσταση (για παράδειγμα, εάν είστε νέος χρήστης Python), ανατρέξτε σε αυτό το άρθρο: [Python κατά την εγκατάσταση και το Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Τρόπος: η σύνδεση στη Διαχείριση υπηρεσίας
Για να συνδεθείτε με το τελικό σημείο της υπηρεσίας διαχείρισης, χρειάζεστε το Αναγνωριστικό Azure συνδρομής και ένα πιστοποιητικό έγκυρη διαχείρισης. Μπορείτε να αποκτήσετε το Αναγνωριστικό συνδρομής μέσω του [Azure κλασική πύλη][management-portal].

> [AZURE.NOTE] Από το Azure SDK για Python v0.8.0, είναι τώρα μπορείτε να χρησιμοποιήσετε τα πιστοποιητικά που δημιουργήθηκαν με OpenSSL όταν εκτελείται σε Windows.  Απαιτεί Python 2.7.4 ή νεότερη έκδοση. Συνιστάται στους χρήστες να χρησιμοποιούν OpenSSL αντί για .pfx, από την υποστήριξη για .pfx πιστοποιητικά πιθανώς θα καταργηθούν στο μέλλον.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Διαχείριση πιστοποιητικών στα Windows/Mac/Linux (OpenSSL)
Μπορείτε να χρησιμοποιήσετε [OpenSSL](http://www.openssl.org/) για τη δημιουργία του πιστοποιητικού διαχείρισης.  Στην πραγματικότητα πρέπει να δημιουργήσετε δύο πιστοποιητικά, μία για το διακομιστή (μια `.cer` αρχείο) και μία για το πρόγραμμα-πελάτη (μια `.pem` αρχείου). Για να δημιουργήσετε το `.pem` αρχείων, εκτέλεση:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Για να δημιουργήσετε το `.cer` πιστοποιητικού, την εκτέλεση:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Για περισσότερες πληροφορίες σχετικά με τα πιστοποιητικά Azure, ανατρέξτε στο θέμα [Επισκόπηση των πιστοποιητικών για τις υπηρεσίες Cloud Azure](./cloud-services-certs-create.md). Για πληρέστερη περιγραφή των παραμέτρων OpenSSL, ανατρέξτε στην τεκμηρίωση στο [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Αφού δημιουργήσετε αυτά τα αρχεία, πρέπει να αποστείλετε το `.cer` αρχείο για να Azure μέσω της ενέργειας "Αποστολή" της καρτέλας "Ρυθμίσεις" από την [πύλη του Azure κλασική][management-portal], και που πρέπει να λάβετε υπόψη όπου αποθηκεύσατε το `.pem` αρχείου.

Αφού έχετε αποκτήσει το Αναγνωριστικό συνδρομής, δημιουργηθεί ένα πιστοποιητικό και που έχουν αποσταλεί το `.cer` αρχείο για να Azure, μπορείτε να συνδεθείτε με το τελικό σημείο Azure διαχείρισης, μεταφέροντας το αναγνωριστικό συνδρομής και τη διαδρομή προς το `.pem` αρχείο για να **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Στο προηγούμενο παράδειγμα, `sms` είναι ένα αντικείμενο **ServiceManagementService** . Η κλάση **ServiceManagementService** είναι η κύρια κλάση χρησιμοποιείται για τη διαχείριση των υπηρεσιών Azure.

### <a name="management-certificates-on-windows-makecert"></a>Διαχείριση πιστοποιητικών στα Windows (MakeCert)

Μπορείτε να δημιουργήσετε ένα πιστοποιητικό αυτόματης υπογραφής διαχείρισης σε σας χρησιμοποιώντας μηχανική `makecert.exe`.  Ανοίξτε μια **γραμμή εντολών Visual Studio** ως **διαχειριστής** και χρησιμοποιήστε την ακόλουθη εντολή, αντικαθιστώντας *AzureCertificate* με το όνομα του πιστοποιητικού που θέλετε να χρησιμοποιήσετε.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Η εντολή δημιουργεί το `.cer` file και άλλες διαθέσιμες εγκαταστάσεις στο χώρο αποθήκευσης **προσωπικών** πιστοποιητικών. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Επισκόπηση των πιστοποιητικών για τις υπηρεσίες Cloud Azure](./cloud-services-certs-create.md).

Αφού δημιουργήσετε το πιστοποιητικό, πρέπει να αποστείλετε το `.cer` αρχείο για να Azure μέσω της ενέργειας "Αποστολή" της καρτέλας "Ρυθμίσεις" από την [πύλη του Azure κλασική][management-portal].

Αφού έχετε αποκτήσει το Αναγνωριστικό συνδρομής, δημιουργηθεί ένα πιστοποιητικό και που έχουν αποσταλεί η `.cer` αρχείο για να Azure, μπορείτε να συνδεθείτε με το τελικό σημείο Azure διαχείρισης, μεταφέροντας το αναγνωριστικό συνδρομής και τη θέση του πιστοποιητικού στο χώρο αποθήκευσης **προσωπικών** πιστοποιητικών **ServiceManagementService** (ξανά, αντικαταστήστε *AzureCertificate* με το όνομα του πιστοποιητικού):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Στο προηγούμενο παράδειγμα, `sms` είναι ένα αντικείμενο **ServiceManagementService** . Η κλάση **ServiceManagementService** είναι η κύρια κλάση χρησιμοποιείται για τη διαχείριση των υπηρεσιών Azure.

## <a name="ListAvailableLocations"> </a>Τρόπος: λίστα διαθέσιμες θέσεις

Για να δημιουργήσετε μια λίστα με τις θέσεις που είναι διαθέσιμες για υπηρεσίες φιλοξενίας, χρησιμοποιήστε το **λίστα\_θέσεις** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Όταν δημιουργείτε μια υπηρεσία cloud ή σε υπηρεσία αποθήκευσης πρέπει να δώσετε μια έγκυρη θέση. Το **λίστα\_θέσεις** μέθοδος επιστρέφει πάντα μια ενημερωμένη λίστα προς το παρόν διαθέσιμες θέσεις. Όπως αυτής της εγγραφής, τις διαθέσιμες θέσεις είναι οι εξής:

- Δυτική Ευρώπη
- Βόρεια Ευρώπη
- Νοτιοανατολικής Ασίας
- Ανατολικής Ασίας
- Κεντρική η.π.α.
- Βόρεια κεντρική η.π.α.
- Νότια κεντρικής η.π.α.
- Δυτική η.π.α.
- Ανατολικής η.π.α.
- Ιαπωνία Ανατολή
- Δυτική Ιαπωνία
- Νότια Βραζιλίας
- Ανατολική Αυστραλία
- Αυστραλία νοτιοανατολικής

## <a name="CreateCloudService"> </a>Τρόπος: Δημιουργήστε μια υπηρεσία στο cloud

Όταν δημιουργείτε μια εφαρμογή και εκτελεί σε Azure, τον κωδικό και ρύθμισης παραμέτρων μαζί ονομάζονται μια Azure [υπηρεσία στο cloud][] (γνωστό ως μια *φιλοξενούνται υπηρεσίας* προηγούμενων εκδόσεων Azure). Το **Δημιουργία\_φιλοξενούνται\_υπηρεσία** μέθοδος σάς επιτρέπει να δημιουργήσετε μια νέα υπηρεσία, παρέχοντας ένα όνομα υπηρεσία (το οποίο πρέπει να είναι μοναδικό στο Azure), μια ετικέτα (αυτόματα κωδικοποιημένη σε base64), περιγραφή και μια θέση.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Μπορείτε να εμφανίσετε όλες τις υπηρεσίες φιλοξενούμενη για τη συνδρομή σας με το **λίστα\_φιλοξενούνται\_υπηρεσίες** μέθοδο:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Εάν θέλετε να λάβετε πληροφορίες σχετικά με μια συγκεκριμένη υπηρεσία, μπορείτε να το κάνετε, μεταφέροντας το όνομα υπηρεσία για να το **Λήψη\_φιλοξενούνται\_υπηρεσία\_ιδιότητες** μέθοδο:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Αφού δημιουργήσετε μια υπηρεσία cloud, μπορείτε να αναπτύξετε τον κωδικό για την υπηρεσία με το **Δημιουργία\_ανάπτυξης** μέθοδο.

## <a name="DeleteCloudService"> </a>Τρόπος: διαγράψτε μια υπηρεσία στο cloud

Μπορείτε να διαγράψετε μια υπηρεσία cloud, μεταφέροντας το όνομα της υπηρεσίας για να το **Διαγραφή\_φιλοξενούνται\_υπηρεσία** μέθοδο:

    sms.delete_hosted_service('myhostedservice')

Για να διαγράψετε μια υπηρεσία, πρώτα πρέπει να διαγραφούν όλα αναπτύξεις για την υπηρεσία. (Ανατρέξτε στο θέμα [Τρόπος: διαγραφή μιας ανάπτυξης](#DeleteDeployment) για λεπτομέρειες.)

## <a name="DeleteDeployment"> </a>Τρόπος: διαγραφή μιας ανάπτυξης

Για να διαγράψετε μια ανάπτυξη, χρησιμοποιήστε το **Διαγραφή\_ανάπτυξης** μέθοδο. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε μια ανάπτυξη με το όνομα `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Τρόπος: Δημιουργήστε μια υπηρεσία αποθήκευσης

Μια [υπηρεσία αποθήκευσης](../storage/storage-create-storage-account.md) παρέχει πρόσβαση σε Azure των [αντικειμένων blob](../storage/storage-python-how-to-use-blob-storage.md), [πίνακες](../storage/storage-python-how-to-use-table-storage.md)και [ουρές](../storage/storage-python-how-to-use-queue-storage.md). Για να δημιουργήσετε μια υπηρεσία αποθήκευσης, χρειάζεστε ένα όνομα για την υπηρεσία (μεταξύ 3 και 24 πεζούς χαρακτήρες και μοναδικό μέσα σε Azure), μια περιγραφή, μια ετικέτα (έως 100 χαρακτήρες, κωδικοποιημένη αυτόματα σε base64) και μια θέση. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια υπηρεσία αποθήκευσης, καθορίζοντας μια θέση.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Σημείωση στο προηγούμενο παράδειγμα που την κατάσταση της το **Δημιουργία\_χώρου αποθήκευσης\_λογαριασμού** λειτουργία μπορεί να ανακτηθεί, μεταφέροντας το αποτέλεσμα που επιστρέφεται από **Δημιουργία\_χώρου αποθήκευσης\_λογαριασμού** για να το **Λήψη\_λειτουργία\_κατάστασης** μέθοδο.  

Μπορείτε να εμφανίσετε τους λογαριασμούς σας στο χώρο αποθήκευσης και τις ιδιότητές τους με το **λίστα\_χώρου αποθήκευσης\_λογαριασμοί** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Τρόπος: διαγράψτε μια υπηρεσία αποθήκευσης

Μπορείτε να διαγράψετε μια υπηρεσία αποθήκευσης, μεταφέροντας το όνομα της υπηρεσίας χώρου αποθήκευσης για το **Διαγραφή\_χώρου αποθήκευσης\_λογαριασμού** μέθοδο. Διαγραφή μια υπηρεσία αποθήκευσης διαγράφει όλα τα δεδομένα που είναι αποθηκευμένα στην υπηρεσία (αντικείμενα blob, πίνακες και ουρές).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Τρόπος: λίστα διαθέσιμων λειτουργικών συστημάτων

Για να δημιουργήσετε μια λίστα με τα λειτουργικά συστήματα που είναι διαθέσιμες για υπηρεσίες φιλοξενίας, χρησιμοποιήστε το **λίστα\_λειτουργικό\_συστήματα** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε το **λίστα\_λειτουργικό\_σύστημα\_οικογένειες** μέθοδο, η οποία ομαδοποιεί τα λειτουργικά συστήματα από οικογένεια:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Τρόπος: Δημιουργήστε μια εικόνα του λειτουργικού συστήματος

Για να προσθέσετε μια εικόνα λειτουργικό σύστημα του αποθετηρίου εικόνα, χρησιμοποιήστε το **Προσθήκη\_os\_εικόνα** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Για να δημιουργήσετε μια λίστα με τις εικόνες λειτουργικό σύστημα που είναι διαθέσιμες, χρησιμοποιήστε το **λίστα\_os\_εικόνες** μέθοδο. Περιλαμβάνει όλες τις εικόνες πλατφόρμα και εικόνες χρήστη:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Τρόπος: Διαγραφή εικόνας λειτουργικό σύστημα

Για να διαγράψετε μια εικόνα του χρήστη, χρησιμοποιήστε το **Διαγραφή\_os\_εικόνα** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Τρόπος: Δημιουργήστε μια εικονική μηχανή

Για να δημιουργήσετε μια εικονική μηχανή, πρέπει πρώτα να δημιουργήσετε μια [υπηρεσία στο cloud](#CreateCloudService).  Στη συνέχεια, δημιουργήστε το ανάπτυξης εικονική μηχανή χρησιμοποιώντας το **Δημιουργία\_εικονικού\_μηχανικής\_ανάπτυξης** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Τρόπος: διαγράψτε μια εικονική μηχανή

Για να διαγράψετε μια εικονική μηχανή, μπορείτε πρώτα να διαγράψετε το ανάπτυξης χρησιμοποιώντας το **Διαγραφή\_ανάπτυξης** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Στη συνέχεια, μπορείτε να διαγράψετε το την υπηρεσία cloud χρησιμοποιώντας το **Διαγραφή\_φιλοξενούνται\_υπηρεσία** μέθοδο:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Τρόπος: Δημιουργήστε μια εικονική μηχανή από μια εικόνα που τραβήξατε εικονική μηχανή

Για να καταγράψετε μια εικόνα Εικονική, μπορείτε πρώτα να καλέσει το **Καταγραφή\_εικονική\_εικόνα** μέθοδο:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Στη συνέχεια, για να βεβαιωθείτε ότι έχετε καταγράψει με επιτυχία την εικόνα, χρησιμοποιήστε το **λίστα\_εικονική\_εικόνες** api, και βεβαιωθείτε ότι η εικόνα σας εμφανίζεται στα αποτελέσματα:

    images = sms.list_vm_images()

Για να δημιουργήσετε τελικά η εικονική μηχανή χρησιμοποιώντας την καταγεγραμμένη εικόνα, χρησιμοποιήστε το **Δημιουργία\_εικονικού\_μηχανικής\_ανάπτυξης** μέθοδο καθώς πριν, αλλά αυτήν τη φορά διέρχονται στο το vm_image_name αντί για αυτό

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Για να μάθετε περισσότερα σχετικά με τον τρόπο για να καταγράψετε μια εικονική μηχανή Linux, ανατρέξτε στο θέμα [πώς μπορείτε να καταγράψετε μια εικονική μηχανή Linux.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Για να μάθετε περισσότερα σχετικά με τον τρόπο για να καταγράψετε μια εικονική μηχανή των Windows, ανατρέξτε στο θέμα [πώς μπορείτε να καταγράψετε μια εικονική μηχανή Windows.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία της υπηρεσίας διαχείρισης, μπορείτε να αποκτήσετε πρόσβαση στην [πλήρη API τεκμηρίωση αναφοράς για το SDK Python Azure](http://azure-sdk-for-python.readthedocs.org/) και να εκτελέσετε σύνθετες εργασίες εύκολα για να διαχειριστείτε την εφαρμογή python.

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές Python](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[υπηρεσία στο cloud]:/services/cloud-services/

