<properties
    pageTitle="Εφαρμογή Web και κλωνοποίηση χρήση του PowerShell"
    description="Μάθετε πώς μπορείτε να αντιγράψετε τις εφαρμογές Web σε νέες εφαρμογές Web με χρήση του PowerShell."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Εφαρμογή Azure εφαρμογής υπηρεσίας κλωνοποίηση χρήση του PowerShell#

Με την έκδοση του Microsoft Azure PowerShell έκδοση 1.1.0 μια νέα επιλογή έχει προστεθεί σε νέα AzureRMWebApp που θα δίνει στο χρήστη τη δυνατότητα να κλωνοποίηση μια υπάρχουσα εφαρμογή Web σε μια εφαρμογή που μόλις δημιουργήθηκε σε διαφορετική περιοχή ή στην ίδια περιοχή. Αυτό θα ενεργοποιήσει πελάτες για την ανάπτυξη ενός αριθμού των εφαρμογών σε διαφορετικές περιοχές γρήγορα και εύκολα.

Εφαρμογή κλωνοποίηση είναι αυτήν τη στιγμή υποστηρίζεται μόνο για προγράμματα του premium επίπεδο εφαρμογής υπηρεσίας. Η νέα δυνατότητα χρησιμοποιεί τους ίδιους περιορισμούς ως δυνατότητα δημιουργίας αντιγράφων ασφαλείας εφαρμογών Web, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Για να μάθετε σχετικά με τη χρήση Azure από διαχειριστή πόρων βάσει Azure PowerShell cmdlet για τη Διαχείριση εφαρμογών Web της έλεγχος [Azure από διαχειριστή πόρων με βάση τις εντολές του PowerShell για Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Κλωνοποίηση μια υπάρχουσα εφαρμογή ##

Σενάριο: Μια υπάρχουσα εφαρμογή web στην περιοχή Νότια κεντρικής ΜΑΣ, ο χρήστης θα θέλατε να κλωνοποίηση το περιεχόμενο σε μια νέα εφαρμογή web στη Βόρεια κεντρική ΜΑΣ περιοχή. Αυτό μπορεί να πραγματοποιηθεί χρησιμοποιώντας την έκδοση Azure από διαχειριστή πόρων από το cmdlet του PowerShell για να δημιουργήσετε μια νέα εφαρμογή web με την επιλογή - SourceWebApp.

Εάν γνωρίζετε το όνομα της ομάδας πόρων που περιέχει την εφαρμογή web προέλευσης, μπορεί να χρησιμοποιήσουμε την παρακάτω εντολή PowerShell για να λάβετε πληροφορίες της εφαρμογής web προέλευσης (ονομάζονται σε αυτήν την περίπτωση webapp προέλευσης):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Για να δημιουργήσετε ένα νέο πρόγραμμα υπηρεσιών εφαρμογής, μπορούμε να χρησιμοποιήσουμε εντολή Δημιουργία AzureRmAppServicePlan όπως στο ακόλουθο παράδειγμα

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Χρησιμοποιώντας την εντολή Δημιουργία AzureRmWebApp, να δημιουργήσετε τη νέα εφαρμογή web της περιοχής Βόρεια κεντρική ΜΑΣ, και να συνδέουν το σε μια υπάρχουσα σειρά premium εφαρμογής υπηρεσίας σχεδιασμός, επιπλέον θα σας μπορεί να χρησιμοποιεί την ίδια ομάδα πόρων με την εφαρμογή web προέλευσης ή Ορισμός νέας ομάδας πόρων, παρουσιάζει τα εξής που:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Για να αντιγράψετε μια υπάρχουσα εφαρμογή web, συμπεριλαμβανομένων των όλες τις συσχετισμένες ανάπτυξης υποδοχές, ο χρήστης θα πρέπει να χρησιμοποιήσετε την παράμετρο IncludeSourceWebAppSlots, την παρακάτω εντολή PowerShell παρουσιάζει τη χρήση αυτής της παραμέτρου με την εντολή Δημιουργία AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Για να αντιγράψετε μια υπάρχουσα εφαρμογή web μέσα στην ίδια περιοχή, ο χρήστης θα χρειαστεί να δημιουργήσετε μια νέα ομάδα πόρων και μια νέα υπηρεσία εφαρμογής Σχεδιασμός στην ίδια περιοχή και, στη συνέχεια, χρησιμοποιώντας την ακόλουθη εντολή PowerShell η κλωνοποίηση της εφαρμογής web

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Κλωνοποίηση μια υπάρχουσα εφαρμογή σε ένα περιβάλλον εφαρμογής υπηρεσίας ##

Σενάριο: Μια υπάρχουσα εφαρμογή web στην περιοχή Νότια κεντρικής ΜΑΣ, ο χρήστης θα θέλατε να αντιγράψετε τα περιεχόμενα σε μια νέα εφαρμογή web σε ένα υπάρχον περιβάλλον υπηρεσίας εφαρμογής (ASE).

Εάν γνωρίζετε το όνομα της ομάδας πόρων που περιέχει την εφαρμογή web προέλευσης, μπορεί να χρησιμοποιήσουμε την παρακάτω εντολή PowerShell για να λάβετε πληροφορίες της εφαρμογής web προέλευσης (ονομάζονται σε αυτήν την περίπτωση webapp προέλευσης):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Γνωρίζετε το όνομα του ASE και το όνομα της ομάδας πόρων στην οποία ανήκει το ASE, ο χρήστης να χρησιμοποιήσετε την εντολή Δημιουργία AzureRmWebApp για να δημιουργήσετε τη νέα εφαρμογή web στο την υπάρχουσα ASE, παρουσιάζει τα εξής που:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Η παράμετρος θέση απαιτείται λόγω λόγο παλαιού τύπου, αλλά στην περίπτωση τη δημιουργία μιας εφαρμογής σε μια ASE θα παραβλεφθεί. 

## <a name="cloning-an-existing-app-slot"></a>Κλωνοποίηση μια υπάρχουσα εφαρμογή υποδιαίρεση ##

Σενάριο: Ο χρήστης θα θέλατε να κλωνοποίηση μιας υπάρχουσας υποδοχής εφαρμογής Web για να είτε μια νέα εφαρμογή Web ή μια νέα υποδοχή Web App. Η νέα εφαρμογή Web μπορεί να είναι στην ίδια περιοχή ως το αρχικό υποδιαίρεση Web App ή σε διαφορετική περιοχή.

Εάν γνωρίζετε το όνομα της ομάδας πόρων που περιέχει την εφαρμογή web προέλευσης, μπορεί να χρησιμοποιήσουμε την παρακάτω εντολή PowerShell για να λάβετε πληροφορίες προέλευσης web app υποδοχή του (σε αυτήν την περίπτωση με το όνομα προέλευσης webappslot) συνδεδεμένη με Web App προέλευσης-webapp:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Το παρακάτω δείχνει τη δημιουργία μιας κλωνοποίηση της εφαρμογής web προέλευσης σε μια νέα εφαρμογή web:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Ρύθμιση παραμέτρων Manager κίνηση ενώ κλωνοποίηση μια εφαρμογή ##

Δημιουργία εφαρμογών web πολλών περιοχών και τη ρύθμιση των παραμέτρων Azure Διαχείριση κίνηση για να δρομολογείτε την κίνηση σε όλες αυτές τις εφαρμογές web, είναι ένα σενάριο n σημαντικό για να εξασφαλίσετε ότι είναι ιδιαίτερα διαθέσιμες εφαρμογές των πελατών, όταν κλωνοποίηση μια υπάρχουσα εφαρμογή web που έχετε την επιλογή για σύνδεση δύο εφαρμογές web με ένα νέο προφίλ διαχείρισης κίνηση ή ένα υπάρχον - Σημειώστε ότι μόνο από διαχειριστή πόρων Azure υποστηρίζεται έκδοση της διαχείρισης κίνηση.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Δημιουργήσετε ένα νέο προφίλ Manager κίνηση ενώ κλωνοποίηση μια εφαρμογή ###

Σενάριο: Ο χρήστης θα θέλατε να αντιγράψετε μια εφαρμογή web σε άλλη περιοχή, κατά τη ρύθμιση παραμέτρων προφίλ manager κίνηση του Azure από διαχειριστή πόρων που περιλαμβάνουν δύο εφαρμογές web. Το παρακάτω δείχνει τη δημιουργία μιας κλωνοποίηση της εφαρμογής web προέλευσης σε μια νέα εφαρμογή web κατά τη ρύθμιση παραμέτρων ένα νέο προφίλ διαχείρισης κίνηση:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Προσθήκη νέων στοιχείων κλωνοποιηθεί Web App σε υπάρχον προφίλ Manager κίνηση ###

Σενάριο: Ο χρήστης έχει ήδη ένα προφίλ διαχειριστή πόρων Azure κίνηση της διαχείρισης που εσείς θέλετε να προσθέσετε δύο εφαρμογές web ως τελικά σημεία. Για να γίνει αυτό, πρέπει πρώτα να συγκεντρώσετε τα υπάρχοντα id προφίλ manager κίνηση, θα χρειαστεί το αναγνωριστικό συνδρομής, όνομα ομάδας πόρων και το υπάρχον όνομα προφίλ manager κίνηση.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Αφού έχετε το αναγνωριστικό διαχείρισης κίνηση, τα ακόλουθα δείχνει τη δημιουργία μιας κλωνοποίηση της εφαρμογής web προέλευσης σε μια νέα εφαρμογή web κατά την προσθήκη τους σε υπάρχον προφίλ Manager κίνηση:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Τρέχουσα περιορισμούς ##

Αυτή η δυνατότητα είναι αυτήν τη στιγμή στην προεπισκόπηση, Εργαζόμαστε για να προσθέσετε νέες δυνατότητες με τον καιρό, η παρακάτω λίστα είναι οι περιορισμοί γνωστά την τρέχουσα έκδοση της εφαρμογής κλωνοποίηση:

- Ρυθμίσεις αυτόματης κλίμακας δεν είναι κλωνοποιηθεί
- Δεν είναι κλωνοποιηθεί ρυθμίσεις χρονοδιάγραμμα δημιουργίας αντιγράφων ασφαλείας
- Ρυθμίσεις VNET δεν είναι κλωνοποιηθεί
- Εφαρμογή ιδέες δεν ρυθμίζονται αυτόματα στην εφαρμογή web του προορισμού
- Δεν είναι κλωνοποιηθεί εύκολο ρυθμίσεις ελέγχου ταυτότητας
- Δεν είναι κλωνοποιηθεί kudu επέκτασης
- Δεν είναι κλωνοποιηθεί κανόνων συμβουλών
- Δεν είναι κλωνοποιηθεί περιεχομένου της βάσης δεδομένων


### <a name="references"></a>Αναφορές ###
- [Azure διαχείριση πόρων με βάση τις εντολές του PowerShell για Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Εφαρμογή Web και κλωνοποίηση με πύλη Azure](app-service-web-app-cloning-portal.md)
- [Δημιουργία αντιγράφου ασφαλείας μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας](web-sites-backup.md)
- [Azure υποστήριξη διαχείρισης πόρων για προεπισκόπηση Manager κυκλοφορία Azure](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Εισαγωγή στο περιβάλλον εφαρμογής υπηρεσίας](app-service-app-service-environment-intro.md)
- [Χρήση του Azure PowerShell με τη Διαχείριση Azure πόρων](../powershell-azure-resource-manager.md)
