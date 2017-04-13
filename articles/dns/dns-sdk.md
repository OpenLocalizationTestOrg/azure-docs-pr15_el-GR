<properties 
   pageTitle="Δημιουργία ζώνες DNS και να καταγράψετε σύνολα στο DNS Azure χρησιμοποιώντας το .NET SDK | Microsoft Azure" 
   description="Πώς μπορείτε να δημιουργήσετε ζώνες DNS και σύνολα εγγραφών στο Azure DNS, χρησιμοποιώντας το SDK .NET." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Δημιουργία ζώνες DNS και σύνολα εγγραφών χρησιμοποιώντας το .NET SDK

Μπορείτε να αυτοματοποιήσετε λειτουργιών, για να δημιουργήσετε, να διαγράψετε ή να ενημερώσετε ζώνες, σύνολα εγγραφών και εγγραφές DNS χρησιμοποιώντας DNS SDK με βιβλιοθήκης διαχείρισης DNS .NET. Ένα έργο Visual Studio πλήρους είναι διαθέσιμη [εδώ.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Δημιουργία λογαριασμού κεφαλαίου υπηρεσίας

Συνήθως, μέσω προγραμματισμού πρόσβαση σε πόρους Azure παρέχεται μέσω ένα αποκλειστικό λογαριασμό αντί για τα δικά σας διαπιστευτήρια χρήστη. Αυτοί οι λογαριασμοί αποκλειστικό ονομάζονται "κεφαλαίου υπηρεσίας" Λογαριασμοί. Για να χρησιμοποιήσετε το δείγμα έργου Azure DNS SDK, πρέπει πρώτα να δημιουργήσετε ένα λογαριασμό υπηρεσίας κεφαλαίου και να την εκχωρήσετε τα σωστά δικαιώματα.

1. Ακολουθήστε [αυτές τις οδηγίες](../resource-group-authenticate-service-principal.md) για να δημιουργήσετε ένα λογαριασμό υπηρεσίας κεφαλαίου (το έργο δείγμα Azure DNS SDK προϋποθέτει τον έλεγχο ταυτότητας με κωδικό πρόσβασης).

2. Δημιουργήστε μια ομάδα πόρων ([δείτε εδώ πώς](../azure-portal/resource-group-portal.md)).

3. Χρήση του Azure RBAC για την εκχώρηση του λογαριασμού υπηρεσίας κεφαλαίου δικαιωμάτων 'DNS Zone συμβολής' στην ομάδα πόρων ([δείτε εδώ πώς](../active-directory/role-based-access-control-configure.md).)

4. Εάν χρησιμοποιείτε το δείγμα έργου Azure DNS SDK, επεξεργαστείτε το αρχείο 'program.cs' ως εξής:
    * Εισαγάγετε τις σωστές τιμές για το tenantId, clientId (γνωστό και ως Αναγνωριστικό λογαριασμού), μυστικό (υπηρεσία κεφαλαίου κωδικό πρόσβασης του λογαριασμού) και subscriptionId όπως χρησιμοποιούνται στο βήμα 1.
    * Πληκτρολογήστε το όνομα της ομάδας πόρων επιλέχθηκε στο βήμα 2.
    * Πληκτρολογήστε ένα όνομα ζώνης DNS της επιλογής σας.

## <a name="nuget-packages-and-namespace-declarations"></a>Πακέτα NuGet και δηλώσεις χώρου ονομάτων

Για να χρησιμοποιήσετε το .NET SDK Azure DNS, πρέπει να εγκαταστήσετε το πακέτο NuGet **Βιβλιοθήκης διαχείρισης DNS Azure** και άλλα απαιτείται Azure πακέτα.
 
1. Στο **Visual Studio**, ανοίξτε ένα έργο ή ένα νέο έργο. 

2. Μεταβείτε στις επιλογές **Εργαλεία** **>** **διαχείρισης πακέτου NuGet** **>** **Διαχείριση πακέτων NuGet για... λύση**. 

3. Κάντε κλικ στο κουμπί **Αναζήτηση**, ενεργοποιήστε το πλαίσιο ελέγχου **Συμπερίληψη προέκδοση** και πληκτρολογήστε **Microsoft.Azure.Management.Dns** στο πλαίσιο αναζήτησης.

4. Επιλέξτε το πακέτο και κάντε κλικ στην επιλογή **εγκατάσταση** για να το προσθέσετε στο έργο σας Visual Studio.
 
5. Επαναλάβετε τη διαδικασία παραπάνω για να εγκαταστήσετε επίσης τα ακόλουθα πακέτα: **Microsoft.Rest.ClientRuntime.Azure.Authentication** και **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Προσθήκη δηλώσεις χώρου ονομάτων

Προσθέστε τις ακόλουθες δηλώσεις χώρου ονομάτων

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Προετοιμασία του προγράμματος-πελάτη διαχείρισης DNS

Το *DnsManagementClient* περιέχει τις μεθόδους και τις ιδιότητες που είναι απαραίτητα για τη Διαχείριση DNS ζώνες και σύνολα εγγραφών.  Ο ακόλουθος κώδικας συνδέεται με το λογαριασμό κεφαλαίου της υπηρεσίας και δημιουργεί ένα αντικείμενο DnsManagementClient.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Δημιουργία ή ενημέρωση μιας ζώνης DNS

Για να δημιουργήσετε μια ζώνη DNS, πρώτα "Ζώνη" δημιουργείται ένα αντικείμενο να περιέχει τις παραμέτρους της ζώνης DNS. Επειδή ζώνες DNS δεν είναι συνδεδεμένα με μια συγκεκριμένη περιοχή, η θέση έχει οριστεί σε 'global'. Σε αυτό το παράδειγμα, ενός [Διαχειριστή πόρων Azure 'ετικέτα'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) προστίθεται επίσης στη ζώνη.

Για να μπορέσουν Δημιουργία ή ενημέρωση της ζώνης στο Azure DNS, το αντικείμενο ζώνης που περιέχει τις παραμέτρους ζώνη μεταβιβάζεται τη μέθοδο *DnsManagementClient.Zones.CreateOrUpdateAsyc* .

>[AZURE.NOTE] DnsManagementClient υποστηρίζει τρεις καταστάσεις λειτουργίας: σύγχρονη ('CreateOrUpdate'), ασύγχρονης ('CreateOrUpdateAsync'), ή ασύγχρονης με την πρόσβαση στην απάντηση HTTP ('CreateOrUpdateWithHttpMessagesAsync').  Μπορείτε να επιλέξετε οποιαδήποτε από τις εξής τρεις λειτουργίες, ανάλογα με τις ανάγκες της εφαρμογής σας.

Azure DNS υποστηρίζει βέλτιστου, που ονομάζεται [Etags](dns-getstarted-create-dnszone.md). Σε αυτό το παράδειγμα, καθορίζοντας "*" για το 'If-None-Match' κεφαλίδα ενημερώνει Azure DNS για να δημιουργήσετε μια ζώνη DNS, εάν δεν υπάρχει ήδη.  Η κλήση αποτυγχάνει εάν μια ζώνη με το συγκεκριμένο όνομα υπάρχει ήδη στην ομάδα πόρων που δίνεται.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Δημιουργήστε σύνολα εγγραφών DNS και εγγραφές

Εγγραφές DNS πραγματοποιείται ως ένα σύνολο εγγραφών. Ένα σύνολο εγγραφών είναι ένα σύνολο εγγραφών με το ίδιο όνομα και τύπο εγγραφής μέσα σε μια ζώνη.  Το όνομα του συνόλου εγγραφών είναι σε σχέση με το όνομα ζώνη, όχι το πλήρως προσδιορισμένο όνομα DNS.

Για να δημιουργήσετε ή να ενημερώσετε μια εγγραφή "σύνολο", "ένα"σύνολο εγγραφών"παράμετροι αντικειμένου δημιουργείται και που του μεταβιβάστηκε *DnsManagementClient.RecordSets.CreateOrUpdateAsync*". Όπως με ζώνες DNS, υπάρχουν τρεις καταστάσεις λειτουργίας: σύγχρονη ('CreateOrUpdate'), ασύγχρονης ('CreateOrUpdateAsync'), ή ασύγχρονης με την πρόσβαση στην απάντηση HTTP ('CreateOrUpdateWithHttpMessagesAsync').

Όπως με ζώνες DNS, οι λειτουργίες σε σύνολα εγγραφών περιλαμβάνουν υποστήριξη για βέλτιστου.  Σε αυτό το παράδειγμα, δεδομένου ότι έχουν καθοριστεί If-Match ούτε 'If-κανένα-Match', το σύνολο εγγραφών δημιουργείται πάντα.  Αυτή η κλήση αντικαθιστούν οποιαδήποτε υπάρχοντα συνόλου εγγραφών με το ίδιο όνομα και τύπο εγγραφής σε αυτήν τη ζώνη DNS.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Λήψη ζώνες και σύνολα εγγραφών

Οι μέθοδοι *DnsManagementClient.Zones.Get* και *DnsManagementClient.RecordSets.Get* ανακτήσετε μεμονωμένα ζώνες και σύνολα εγγραφών, αντίστοιχα. Σύνολα εγγραφών αναγνωρίζονται από τον τύπο τους, το όνομα και την ομάδα ζώνη και πόρων που υπάρχουν στο. Οι ζώνες αναγνωρίζονται από το όνομα του ατόμου και της ομάδας πόρων που υπάρχουν στο.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Ενημερώστε ένα υπάρχον σύνολο εγγραφών

Για να ενημερώσετε ένα υπάρχον σύνολο εγγραφών DNS, πρώτα ανάκτηση συνόλου εγγραφών, ενημερώστε τα περιεχόμενα συνόλου εγγραφών, στη συνέχεια, υποβάλετε την αλλαγή.  Σε αυτό το παράδειγμα, θα σας να καθορίσετε το 'Etag' από το σύνολο ανακτημένα εγγραφή στην παράμετρο If-Match. Η κλήση αποτυγχάνει εάν μια λειτουργία ταυτόχρονες έχει τροποποιηθεί το σύνολο στο μεταξύ εγγραφών.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Λίστα ζώνες και σύνολα εγγραφών

Για να παραθέσετε ζώνες, χρησιμοποιήστε τις μεθόδους *DnsManagementClient.Zones.List...* , οι οποίες υποστηρίζουν παραθέτει όλες τις ζώνες σε μια ομάδα που δίνεται πόρων είτε όλες τις ζώνες σε μια δεδομένη Azure συνδρομή (σε ομάδες πόρων.) Για να παραθέσετε σύνολα εγγραφών, χρησιμοποιήστε μεθόδους *DnsManagementClient.RecordSets.List...* , οι οποίες υποστηρίζουν είτε παραθέτει όλα τα σύνολα εγγραφών σε μια δεδομένη ζώνη ή μόνο αυτά τα σύνολα εγγραφών από έναν συγκεκριμένο τύπο.

Λάβετε υπόψη κατά την καταχώρηση ζώνες και μπορεί να είναι σελιδοποίηση σύνολα εγγραφής που προκύπτει.  Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να επαναλάβετε τις σελίδες των αποτελεσμάτων. (Μια τεχνητά μικρό μέγεθος σελίδας "2" χρησιμοποιείται για να επιβάλετε σελιδοποίησης; στην πράξη θα πρέπει να παραλειφθεί η παράμετρος αυτή και το προεπιλεγμένο μέγεθος σελίδας.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Επόμενα βήματα

Κάντε λήψη του [Azure DNS .NET SDK δείγμα έργου](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), που περιλαμβάνει περαιτέρω παραδείγματα του τρόπου χρήσης του Azure DNS .NET SDK, συμπεριλαμβανομένων των παραδείγματα για άλλους τύπους εγγραφών DNS.
