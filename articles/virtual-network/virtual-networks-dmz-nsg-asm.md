<properties
   pageTitle="Παράδειγμα DMZ – Δημιουργήστε μια απλή DMZ με NSGs | Microsoft Azure"
   description="Δημιουργήστε μια DMZ με ομάδες ασφαλείας δικτύου (NSG)"
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
   ms.author="jonor;sivae"/>

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Παράδειγμα 1 – Δημιουργήστε μια απλή DMZ με NSGs

[Επιστρέψτε στη σελίδα βέλτιστες πρακτικές ασφαλείας όριο][HOME]

Αυτό το παράδειγμα θα δημιουργήσει μια απλή DMZ με τέσσερις διακομιστές των windows και ομάδες ασφαλείας δικτύου. Αυτό θα επίσης καθοδηγήσει κάθε μία από τις σχετικές εντολές για την παροχή κατανοήσει βαθύτερη κάθε βήμα. Υπάρχει επίσης μια ενότητα σενάριο κίνηση για την παροχή ενός αναλυτικά βήμα προς βήμα πώς εξελίσσεται κίνηση μέσω των επιπέδων άμυνας για το DMZ. Τέλος, στις αναφορές ενότητα είναι τον πλήρη κωδικό και οδηγίες για τη δημιουργία αυτού του περιβάλλοντος για να δοκιμάσετε και να πειραματιστείτε με διάφορες σενάρια. 

![Εισερχόμενη DMZ με NSG][1]

## <a name="environment-description"></a>Περιγραφή περιβάλλοντος
Σε αυτό το παράδειγμα, υπάρχει μια συνδρομή που περιέχει τα εξής:

- Δύο στο cloud services: "FrontEnd001" και "BackEnd001"
- Ένα εικονικό δίκτυο και "CorpNetwork", με δύο δευτερεύοντα δίκτυα; "FrontEnd" και "Παρασκηνίου"
- Μια ομάδα ασφαλείας δικτύου που έχει εφαρμοστεί σε δύο δευτερεύοντα δίκτυα
- Σε Windows Server που αντιπροσωπεύει ένα διακομιστή εφαρμογών web ("IIS01")
- Διακομιστές δύο παράθυρα που αναπαριστά την εφαρμογή ξανά Τερματισμός διακομιστές ("AppVM01", "AppVM02")
- Σε Windows server που αντιπροσωπεύει ένα διακομιστή DNS ("DNS01")

Στην ενότητα "Αναφορές" κάτω από το στοιχείο υπάρχει μια δέσμη ενεργειών του PowerShell που θα δημιουργήσετε πιο του περιβάλλοντος που περιγράφονται παραπάνω. Δημιουργία του ΣΠΣ και εικονικού δίκτυα, παρόλο που εκτελούνται από το παράδειγμα δέσμης ενεργειών, δεν περιγράφονται αναλυτικά σε αυτό το έγγραφο. 

Για να δημιουργήσετε το περιβάλλον;

  1.    Αποθηκεύστε το αρχείο xml ρύθμισης παραμέτρων δικτύου που περιλαμβάνονται στην ενότητα αναφορές (ενημερώνονται με ονόματα, θέση και IP διευθύνσεις ώστε να ταιριάζει με το δεδομένο σενάριο)
  2.    Ενημερώστε τις μεταβλητές χρήστη στη δέσμη ενεργειών για να ταιριάζει με το περιβάλλον που πρόκειται να εκτελεστεί σε σχέση με (συνδρομές, ονόματα υπηρεσιών, κλπ) της δέσμης ενεργειών
  3.    Εκτέλεση της δέσμης ενεργειών σε PowerShell

**Σημείωση**: της περιοχής που καθορίζεται στη δέσμη ενεργειών PowerShell πρέπει να συμφωνεί με την περιοχή που καθορίζεται από το αρχείο xml ρύθμισης παραμέτρων δικτύου.

Όταν η δέσμη ενεργειών εκτελείται με επιτυχία επιπλέον προαιρετικά βήματα μπορούν να ληφθούν, στην ενότητα αναφορές είναι δύο δέσμες ενεργειών για να ρυθμίσετε το διακομιστή web και εφαρμογή διακομιστή με μια εφαρμογή web απλό για να επιτρέψετε δοκιμών με αυτήν τη ρύθμιση παραμέτρων DMZ.

Οι παρακάτω ενότητες παρέχουν μια λεπτομερή περιγραφή των ομάδων ασφαλείας δικτύου και τον τρόπο λειτουργίας τους για αυτό το παράδειγμα, παρουσίαση μερικών μέσω των βασικών γραμμών της δέσμης ενεργειών του PowerShell.

## <a name="network-security-groups-nsg"></a>Ομάδες ασφαλείας δικτύου (NSG)
Για αυτό το παράδειγμα, μια ομάδα NSG είναι ενσωματωμένη και, στη συνέχεια, φορτώνεται με έξι κανόνες. 

>[AZURE.TIP] Σε γενικές γραμμές, θα πρέπει πρώτα να δημιουργήσετε συγκεκριμένες κανόνων "Αποδοχή" και, στη συνέχεια, τελευταίος τους πιο γενικό κανόνες "Απόρριψη". Το υποδεικνύει που του έχουν ανατεθεί προτεραιότητα που είναι κανόνες που υπολογίζεται πρώτα. Αφού εντοπίσετε κίνηση για να εφαρμόσετε σε έναν συγκεκριμένο κανόνα, αξιολογούνται περαιτέρω κανόνες. Κανόνες NSG να εφαρμόσετε είτε την κατεύθυνση εισερχομένων ή εξερχομένων (από την πλευρά του υποδικτύου).

Δηλωτικά, τους παρακάτω κανόνες που δημιουργούνται για την εισερχόμενη κίνηση:

1.  Επιτρέπεται η εσωτερική κυκλοφορία DNS (θύρα 53)
2.  Επιτρέπεται η κυκλοφορία RDP (θύρα 3389) από το Internet για τυχόν εικονική Μηχανή
3.  Επιτρέπεται η κυκλοφορία HTTP (θύρα 80) από το Internet στο διακομιστή web (IIS01)
4.  Επιτρέπεται οποιαδήποτε κυκλοφορία (όλες οι θύρες) από IIS01 στο AppVM1
5.  Οποιαδήποτε κυκλοφορία (όλες οι θύρες) από το Internet για ολόκληρο το VNet (και τα δύο δευτερεύοντα δίκτυα) δεν επιτρέπεται η
6.  Δεν επιτρέπεται η οποιαδήποτε κυκλοφορία (όλες οι θύρες) από το υποδίκτυο Frontend στο υποδίκτυο παρασκηνίου

Με αυτούς τους κανόνες δεσμευμένο με κάθε υποδίκτυο, εάν μια αίτηση HTTP ήταν εισερχομένων από το Internet στο διακομιστή web, και οι δύο κανόνες 3 (επιτρέπουν) και 5 (απόρριψη) θα εφαρμοστεί, αλλά από κανόνα 3 έχει την υψηλότερη προτεραιότητα μόνο θα εφαρμόζεται και δεν θα προέρχονται κανόνα 5 σε αναπαραγωγή. Έτσι θα επιτρέπεται η αίτηση HTTP στο διακομιστή web. Εάν η ίδια κυκλοφορία ήταν προσπαθεί να επικοινωνήσει με το διακομιστή DNS01, κανόνα 5 (άρνηση) θα είναι το πρώτο για να εφαρμόσετε και η κίνηση δεν θα επιτρέπεται να στείλει στο διακομιστή. Κανόνας 6 (άρνηση) αποκλείει το υποδίκτυο Frontend από συζήτησης στο υποδίκτυο παρασκηνίου (εκτός των επιτρεπόμενων κίνηση σε κανόνες 1 και 4), αυτό προστατεύει το δίκτυο παρασκηνίου σε περίπτωση μια παραχωρήσεων εισβολέας την εφαρμογή web στον το Frontend, ο εισβολέας θα έχουν περιορισμένη πρόσβαση στο δίκτυο παρασκηνίου "προστατευμένη" (μόνο για τους πόρους που εκτίθενται στο διακομιστή AppVM01).

Υπάρχει ένας κανόνας εξερχομένων προεπιλεγμένη που επιτρέπει την κυκλοφορία ανάληψη στο Internet. Για αυτό το παράδειγμα, θα σας είστε επιτρέποντας την εξερχόμενη κυκλοφορία και δεν τροποποιώντας τους κανόνες εξερχομένων. Για να κλειδώσετε κίνηση και στις δύο κατευθύνσεις, τη δρομολόγηση που ορίζονται από το χρήστη είναι απαραίτητη, αυτό είναι εξερευνήσει σε "Παράδειγμα 3" κάτω από το στοιχείο.

Κάθε κανόνας εξετάζεται με περισσότερες λεπτομέρειες ως εξής (**Σημείωση**: οποιοδήποτε στοιχείο το κάτω από τη λίστα στο που ξεκινά με ένα σύμβολο δολαρίου (π.χ.: $NSGName) είναι μια μεταβλητή που ορίζονται από το χρήστη από τη δέσμη ενεργειών στην ενότητα αναφορά αυτού του εγγράφου):

1. Πρώτα πρέπει να δημιουργηθούν μια ομάδα ασφαλείας δικτύου για τη διατήρηση των κανόνων:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  Ο πρώτος κανόνας σε αυτό το παράδειγμα θα επιτρέψετε την κυκλοφορία DNS μεταξύ όλων των εσωτερικό δικτύων στο διακομιστή DNS στο υποδίκτυο παρασκηνίου. Ο κανόνας περιλαμβάνει ορισμένες σημαντικές παράμετροι:
  - "Πληκτρολογήστε" υποδεικνύει σε ποια κατεύθυνση της κίνησης αυτός ο κανόνας θα ισχύσει; Αυτό είναι από την πλευρά του υποδικτύου ή εικονική μηχανή (ανάλογα με το πού είναι συνδεδεμένο σε αυτό NSG). Επομένως, εάν ο τύπος είναι "Εισερχόμενα" κίνηση εισέρχεται στο υποδίκτυο, θα εφαρμοστεί ο κανόνας και κλείσετε το υποδίκτυο κίνηση δεν επηρεάζεται από αυτόν τον κανόνα.
  - "Προτεραιότητα" ορίζει τη σειρά με την οποία θα αξιολογείται μια ροή κυκλοφορίας. Στο κάτω τον αριθμό όσο υψηλότερη η προτεραιότητα. Μόλις ένας κανόνας ισχύει για μια συγκεκριμένη κυκλοφορία ροής, χωρίς περαιτέρω κανόνες υποβάλλονται σε επεξεργασία. Επομένως, αν έναν κανόνα με προτεραιότητα 1 επιτρέπει την κυκλοφορία, και έναν κανόνα με προτεραιότητα 2 αποτρέπει την κίνηση, και δύο κανόνες ισχύουν για την κίνηση, στη συνέχεια, θα επιτρέπεται η κίνηση να ροής (εφόσον κανόνας 1 είχε υψηλότερη προτεραιότητα χρειάστηκαν εφέ και έχουν εφαρμόζεται κανένας κανόνας περαιτέρω).
  - "Ενέργεια" υποδεικνύει εάν κίνηση που επηρεάζονται από αυτόν τον κανόνα είναι αποκλεισμένοι ή επιτρεπόμενοι.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Αυτός ο κανόνας θα σας επιτρέψει RDP ροή κυκλοφορίας από το internet στη θύρα RDP σε οποιονδήποτε διακομιστή είτε υποδικτύου στο το VNET. Αυτός ο κανόνας χρησιμοποιεί δύο ειδικοί τύποι προθέματα διεύθυνση; "VIRTUAL_NETWORK" και "INTERNET". Αυτό είναι ένας εύκολος τρόπος για τη διεύθυνση σε μια μεγαλύτερη κατηγορία προθέματα διεύθυνση.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Αυτός ο κανόνας επιτρέπει την εισερχόμενη κυκλοφορία internet για να πατήσετε το διακομιστή web. Αυτό δεν αλλάζει τη συμπεριφορά δρομολόγησης; επιτρέπει μόνο την κυκλοφορία που προορίζονται για IIS01 για τη μεταβίβαση. Επομένως, εάν η κυκλοφορία από το Internet περιείχε το διακομιστή web ως προορισμό αυτός ο κανόνας θα επιτρέπουν και διακοπή επεξεργασίας επιπλέον κανόνων. (Στον κανόνα με προτεραιότητα 140 όλα άλλες εισερχόμενη κυκλοφορία του internet έχει αποκλειστεί). Εάν το μόνο που επεξεργασία κυκλοφορία HTTP, θα μπορούσε να είναι περιορισμένο περαιτέρω αυτόν τον κανόνα για να επιτρέψετε μόνο 80 θύρα προορισμού.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Αυτός ο κανόνας επιτρέπει την κυκλοφορία για τη μεταβίβαση από το διακομιστή IIS01 στο διακομιστή AppVM01, μια νεότερη έκδοση μπλοκ κανόνα όλα άλλες Frontend την κυκλοφορία παρασκηνίου. Για να βελτιώσετε αυτόν τον κανόνα, εάν η θύρα είναι γνωστό που θα πρέπει να προστεθεί. Για παράδειγμα, εάν ο διακομιστής των υπηρεσιών IIS είναι πάτημα μόνο SQL Server σε AppVM01, η περιοχή θύρα προορισμού θα πρέπει να αλλάξει από "*" (οποιαδήποτε) για να 1433 (θύρα SQL), επιτρέποντας ένα μικρότερο επιφάνεια εισερχομένων επίθεση σε AppVM01 πρέπει ποτέ να παραβιαστεί της εφαρμογής web.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Αυτός ο κανόνας αποτρέπει την κυκλοφορία από το internet για όλους τους διακομιστές του δικτύου. Σε συνδυασμό με τον κανόνα με προτεραιότητα 110 και 120, επιτρέπει μόνο εισερχόμενη κυκλοφορία internet για το τείχος προστασίας και RDP θύρες σε άλλους διακομιστές και μπλοκ οτιδήποτε άλλο. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  Ο κανόνας τελικό αποτρέπει την κίνηση από το υποδίκτυο Frontend στο υποδίκτυο παρασκηνίου. Επειδή πρόκειται για ένα μόνο κανόνα εισερχομένων, αντίστροφη κυκλοφορία επιτρέπεται (από το υπόβαθρο για να το Frontend).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Κίνηση σενάρια
#### <a name="allowed-web-to-web-server"></a>(*Επιτρέπεται*) Διακομιστής Web στο Web
1.  Σελίδα χρήστη HTTP αιτήσεις Internet από FrontEnd001.CloudApp.Net (Internet υπηρεσία Cloud αντικριστές)
2.  Υπηρεσία φάσεις κίνηση μέσω Άνοιγμα τελικό σημείο στη θύρα 80 προς IIS01 στο cloud (ο διακομιστής web)
3.  Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    Εφαρμογή κανόνα NSG 3 (Internet για να IIS01), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
4.  Κίνηση επισκέψεις εσωτερική διεύθυνση IP του διακομιστή web IIS01 (10.0.1.5)
5.  IIS01 κάνει ακρόαση για κυκλοφορία web, λάβει αυτό το αίτημα και ξεκινά η επεξεργασία της αίτησης
6.  IIS01 ζητά από τον SQL Server σε AppVM01 πληροφορίες
7.  Δεν υπάρχει κανόνες εξερχόμενων στο προσκήνιο υποδίκτυο, επιτρέπεται κίνηση
8.  Το υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    NSG κανόνα 3 (Internet στο τείχος προστασίας) δεν εφαρμόζονται, μετακίνηση στο επόμενο κανόνα
  4.    4 κανόνα NSG εφαρμογή (IIS01 να AppVM01), κυκλοφορία επιτρέπεται, διακοπή επεξεργασίας κανόνων
9.  AppVM01 λαμβάνει το ερώτημα SQL και απαντούν
10. Εφόσον δεν υπάρχουν εξερχομένων κανόνες στο υποδίκτυο παρασκηνίου επιτρέπεται η απόκριση
11. Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    Δεν υπάρχει κανένας κανόνας NSG που ισχύει για εισερχόμενα κίνηση από το υποδίκτυο παρασκηνίου στο προσκήνιο υποδίκτυο, ώστε να κανένα από τα NSG κανόνες ισχύουν
  2.    Ο προεπιλεγμένος κανόνας σύστημα επιτρέποντας την κυκλοφορία μεταξύ δευτερευόντων δικτύων επιτρέπει αυτήν την κυκλοφορία, ώστε να επιτρέπεται η κίνηση.
12. Ο διακομιστής των υπηρεσιών IIS λαμβάνει την απάντηση SQL και ολοκληρώνει την απόκριση HTTP και αποστέλλει αιτούντα
13. Καθώς υπάρχουν επιτρέπεται καμία κανόνες εξερχόμενων στο υποδίκτυο Frontend την απάντηση και ο χρήστης Internet λαμβάνει την ιστοσελίδα που ζητήθηκε.

#### <a name="allowed-rdp-to-backend"></a>(*Επιτρέπεται*) RDP για υπολογιστή στο παρασκήνιο
1.  Διαχειριστής του διακομιστή στο internet αιτήσεις RDP περίοδο λειτουργίας να AppVM01 σε BackEnd001.CloudApp.Net:xxxxx όπου xxxxx είναι ο αριθμός θύρας που έχει αντιστοιχιστεί τυχαία για RDP να AppVM01 (τη θύρα που του έχουν ανατεθεί βρίσκονται στην πύλη του Azure ή μέσω του PowerShell)
2.  Υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    Εφαρμογή NSG κανόνα 2 (RDP), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
3.  Με κανόνες εξερχομένων, προεπιλεγμένους κανόνες ισχύουν και επιτρέπεται η κυκλοφορία επιστροφής
4.  Είναι ενεργοποιημένη η περίοδος RDP
5.  AppVM01 ζητά κωδικού πρόσβασης χρήστη

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Επιτρέπεται*) Αναζήτηση DNS διακομιστή Web στο διακομιστή DNS
1.  Web Server, IIS01, τροφοδοσία δεδομένων στο www.data.gov τις ανάγκες, αλλά πρέπει να επιλύσετε τη διεύθυνση.
2.  Η ρύθμιση παραμέτρων δικτύου για τις λίστες VNet DNS01 (10.0.2.4 στο υποδίκτυο παρασκηνίου) ως πρωτεύοντα διακομιστή DNS, IIS01 στέλνει την αίτηση DNS για να DNS01
3.  Δεν υπάρχει κανόνες εξερχόμενων στο προσκήνιο υποδίκτυο, επιτρέπεται κίνηση
4.  Υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.     Εφαρμογή NSG κανόνα 1 (DNS), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
5.  Διακομιστή DNS λαμβάνει την αίτηση
6.  Διακομιστής DNS δεν έχει τη διεύθυνση στο cache και ζητά από ένα διακομιστή DNS ρίζας στο internet
7.  Δεν υπάρχει κανόνες εξερχόμενων υποδικτύου παρασκηνίου, επιτρέπεται κίνηση
8.  Απόκριση διακομιστή DNS στο Internet, επειδή αυτήν την περίοδο λειτουργίας ξεκίνησε εσωτερικά, επιτρέπεται η απόκριση
9.  Αποθηκεύει προσωρινά την απόκριση διακομιστή DNS και απαντούν στην αρχική αίτηση επιστροφής σε IIS01
10. Δεν υπάρχει κανόνες εξερχόμενων υποδικτύου παρασκηνίου, επιτρέπεται κίνηση
11. Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    Δεν υπάρχει κανένας κανόνας NSG που ισχύει για εισερχόμενα κίνηση από το υποδίκτυο παρασκηνίου στο προσκήνιο υποδίκτυο, ώστε να κανένα από τα NSG κανόνες ισχύουν
  2.    Ο προεπιλεγμένος κανόνας σύστημα επιτρέποντας την κυκλοφορία μεταξύ δευτερευόντων δικτύων να επιτρέψετε την κυκλοφορία αυτό, ώστε να επιτρέπεται η κίνηση
12. IIS01 λαμβάνει την απάντηση από DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Επιτρέπεται*) Αρχείο access διακομιστή Web σε AppVM01
1.  IIS01 ζητά από ένα αρχείο στο AppVM01
2.  Δεν υπάρχει κανόνες εξερχόμενων στο προσκήνιο υποδίκτυο, επιτρέπεται κίνηση
3.  Το υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    NSG κανόνα 3 (Internet για να IIS01) δεν εφαρμόζονται, μετακίνηση στο επόμενο κανόνα
  4.    4 κανόνα NSG εφαρμογή (IIS01 να AppVM01), κυκλοφορία επιτρέπεται, διακοπή επεξεργασίας κανόνων
4.  AppVM01 λαμβάνει την αίτηση και απαντούν με το αρχείο (αν υποθέσουμε ότι επιτρέπεται η πρόσβαση)
5.  Εφόσον δεν υπάρχουν εξερχομένων κανόνες στο υποδίκτυο παρασκηνίου επιτρέπεται η απόκριση
6.  Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    Δεν υπάρχει κανένας κανόνας NSG που ισχύει για εισερχόμενα κίνηση από το υποδίκτυο παρασκηνίου στο προσκήνιο υποδίκτυο, ώστε να κανένα από τα NSG κανόνες ισχύουν
  2.    Ο προεπιλεγμένος κανόνας σύστημα επιτρέποντας την κυκλοφορία μεταξύ δευτερευόντων δικτύων επιτρέπει αυτήν την κυκλοφορία, ώστε να επιτρέπεται η κίνηση.
7.  Ο διακομιστής των υπηρεσιών IIS λαμβάνει το αρχείο

#### <a name="denied-web-to-backend-server"></a>(*Δεν επιτρέπεται η*) Ιστοσελίδα στο διακομιστή παρασκηνίου
1.  Internet χρήστης προσπαθήσει να αποκτήσετε πρόσβαση σε ένα αρχείο στο AppVM01 μέσω της υπηρεσίας BackEnd001.CloudApp.Net
2.  Καθώς υπάρχουν χωρίς τα τελικά σημεία ανοιχτό για κοινή χρήση αρχείων, αυτό θα περάσει την υπηρεσία Cloud και δεν θα επικοινωνήσει με το διακομιστή
3.  Εάν τα τελικά σημεία ανοιχτό για κάποιο λόγο, NSG κανόνα 5 (Internet για να VNet) να αποκλείσετε αυτήν την κυκλοφορία

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Δεν επιτρέπεται η*) Αναζήτηση DNS Web σε διακομιστή DNS
1.  Internet χρήστης προσπαθήσει να αναζητήσετε μια εσωτερική εγγραφή DNS στην DNS01 μέσω της υπηρεσίας BackEnd001.CloudApp.Net
2.  Εφόσον δεν υπάρχουν ανοιχτά για DNS χωρίς τα τελικά σημεία, αυτό θα περάσει την υπηρεσία Cloud και δεν θα επικοινωνήσει με το διακομιστή
3.  Εάν τα τελικά σημεία ανοιχτό για κάποιο λόγο, NSG κανόνα 5 (Internet για να VNet) να αποκλείσετε αυτήν την κυκλοφορία (Σημείωση: αυτόν τον κανόνα 1 (DNS) δεν θα ισχύουν για δύο λόγους, πρώτα τη διεύθυνση της προέλευσης είναι στο internet, αυτός ο κανόνας ισχύει μόνο για το τοπικό VNet ως προέλευση, επίσης αυτή είναι ένας κανόνας αποδοχή, ώστε το ποτέ να απορρίψετε την κίνηση)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Δεν επιτρέπεται η*) Web στην access SQL μέσω του τείχους προστασίας
1.  Χρήστης του Internet ζητά δεδομένα SQL από FrontEnd001.CloudApp.Net (Internet υπηρεσία Cloud αντικριστές)
2.  Εφόσον δεν τα τελικά σημεία είναι ανοιχτό για SQL, αυτό δεν θα μεταβιβάζουν την υπηρεσία Cloud και δεν θα επίτευξη το τείχος προστασίας
3.  Εάν τελικά σημεία ανοιχτό για κάποιο λόγο, το υποδίκτυο Frontend αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    Εφαρμογή κανόνα NSG 3 (Internet για να IIS01), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
4.  Κίνηση επισκέψεις εσωτερική διεύθυνση IP του IIS01 (10.0.1.5)
5.  IIS01 δεν είναι ακρόαση στη θύρα 1433, επομένως δεν υπάρχει απάντηση στην πρόσκληση σε

## <a name="conclusion"></a>Ολοκλήρωση
Αυτός είναι ένας τρόπος σχετικά απλή και σαφή απομόνωσης το υποδίκτυο παρασκηνίου από εισερχόμενη κυκλοφορία των.

Μπορείτε να βρείτε περισσότερα παραδείγματα και μια επισκόπηση του δικτύου όρια ασφαλείας [εδώ][HOME].

## <a name="references"></a>Αναφορές
### <a name="main-script-and-network-config"></a>Κύρια δέσμη ενεργειών και ρύθμισης παραμέτρων δικτύου
Αποθηκεύστε την πλήρη δέσμη ενεργειών σε ένα αρχείο δέσμης ενεργειών του PowerShell. Αποθηκεύστε το ρύθμισης παραμέτρων δικτύου, σε ένα αρχείο με το όνομα "NetworkConf1.xml".
Τροποποιήστε τις μεταβλητές που ορίζονται από το χρήστη, όπως απαιτείται. Εκτελέστε τη δέσμη ενεργειών και, στη συνέχεια, ακολουθήστε τις οδηγίες εγκατάστασης κανόνα τείχος προστασίας που περιέχονται στην παραπάνω ενότητα παράδειγμα 1.

#### <a name="full-script"></a>Πλήρης δέσμης ενεργειών
Θα αυτήν τη δέσμη ενεργειών, με βάση τις μεταβλητές που ορίζονται από το χρήστη;

1.  Σύνδεση με μια συνδρομή του Azure
2.  Δημιουργία νέου λογαριασμού χώρου αποθήκευσης
3.  Δημιουργήστε μια νέα VNet και δύο δευτερευόντων δικτύων όπως ορίζεται στο αρχείο ρύθμισης παραμέτρων δικτύου
4.  Δημιουργία 4 windows server ΣΠΣ
5.  Ρύθμιση παραμέτρων NSG όπως:
  - Δημιουργία μιας NSG
  - Συμπλήρωση του με κανόνες
  - Σύνδεση του NSG με το κατάλληλο δευτερεύοντα δίκτυα

Αυτή η δέσμη ενεργειών PowerShell πρέπει να εκτελείται τοπικά και στο internet συνδεδεμένοι PC ή το διακομιστή.

>[AZURE.IMPORTANT] Κατά την εκτέλεση αυτής της δέσμης ενεργειών, μπορεί να υπάρχουν προειδοποιήσεις ή άλλα μηνύματα πληροφοριών που pop στο PowerShell. Μόνο τα μηνύματα σφάλματος σε κόκκινο χρώμα είναι αιτία για το πρόβλημα.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #   

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
      

#### <a name="network-config-file"></a>Αρχείο ρύθμισης παραμέτρων δικτύου
Αποθήκευση αυτού του αρχείου xml με ενημερωμένη θέση και να προσθέσετε τη σύνδεση σε αυτό το αρχείο στη μεταβλητή $NetworkConfigFile στη δέσμη ενεργειών παραπάνω.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Δείγμα εφαρμογής δεσμών ενεργειών
Εάν θέλετε να εγκαταστήσετε μια εφαρμογή του δείγματος για αυτό και άλλα παραδείγματα DMZ, μία έχει παρασχεθεί στην ακόλουθη σύνδεση: [Εφαρμογή δείγματος δέσμης ενεργειών][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Εισερχόμενη DMZ με NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

