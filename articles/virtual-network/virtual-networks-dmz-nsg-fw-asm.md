<properties
   pageTitle="Παράδειγμα DMZ – Δημιουργήστε ένα DMZ για την προστασία εφαρμογών με ένα τείχος προστασίας και NSGs | Microsoft Azure"
   description="Δημιουργήστε μια DMZ με ένα τείχος προστασίας και τις ομάδες ασφαλείας δικτύου (NSG)"
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

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Παράδειγμα 2-Δημιουργήστε μια DMZ για την προστασία εφαρμογών με ένα τείχος προστασίας και NSGs

[Επιστρέψτε στη σελίδα βέλτιστες πρακτικές ασφαλείας όριο][HOME]

Αυτό το παράδειγμα θα δημιουργήσει μια DMZ με ένα τείχος προστασίας, διακομιστές τέσσερις των windows και τις ομάδες ασφαλείας δικτύου. Αυτό θα επίσης καθοδηγήσει κάθε μία από τις σχετικές εντολές για την παροχή κατανοήσει βαθύτερη κάθε βήμα. Υπάρχει επίσης μια ενότητα σενάριο κίνηση για την παροχή ενός αναλυτικά βήμα προς βήμα πώς εξελίσσεται κίνηση μέσω των επιπέδων άμυνας για το DMZ. Τέλος, στις αναφορές ενότητα είναι τον πλήρη κωδικό και οδηγίες για τη δημιουργία αυτού του περιβάλλοντος για να δοκιμάσετε και να πειραματιστείτε με διάφορες σενάρια. 

![Εισερχόμενη DMZ με NVA και NSG][1]

## <a name="environment-description"></a>Περιγραφή περιβάλλοντος
Σε αυτό το παράδειγμα, υπάρχει μια συνδρομή που περιέχει τα εξής:

- Δύο στο cloud services: "FrontEnd001" και "BackEnd001"
- Ένα εικονικό δίκτυο "CorpNetwork", με δύο δευτερεύοντα δίκτυα: "FrontEnd" και "Παρασκηνίου"
- Μία ομάδα ασφαλείας δικτύου που έχει εφαρμοστεί σε δύο δευτερεύοντα δίκτυα
- Μια συσκευή εικονικού δικτύου, σε αυτό το παράδειγμα τείχος προστασίας NextGen Barracuda, συνδεδεμένο με το υποδίκτυο Frontend
- Σε Windows Server που αντιπροσωπεύει ένα διακομιστή εφαρμογών web ("IIS01")
- Διακομιστές δύο παράθυρα που αναπαριστά την εφαρμογή ξανά Τερματισμός διακομιστές ("AppVM01", "AppVM02")
- Σε Windows server που αντιπροσωπεύει ένα διακομιστή DNS ("DNS01")

>[AZURE.NOTE] Αν και αυτό το παράδειγμα χρησιμοποιεί τείχος προστασίας NextGen Barracuda, πολλά από τα διαφορετικές συσκευές εικονικού δικτύου μπορεί να χρησιμοποιηθεί για αυτό το παράδειγμα.

Στην ενότητα "Αναφορές" κάτω από το στοιχείο υπάρχει μια δέσμη ενεργειών του PowerShell που θα δημιουργήσετε πιο του περιβάλλοντος που περιγράφονται παραπάνω. Δημιουργία του ΣΠΣ και εικονικού δίκτυα, παρόλο που εκτελούνται από το παράδειγμα δέσμης ενεργειών, δεν περιγράφονται αναλυτικά σε αυτό το έγγραφο.
 
Για να δημιουργήσετε το περιβάλλον:

  1.    Αποθηκεύστε το αρχείο xml ρύθμισης παραμέτρων δικτύου που περιλαμβάνονται στην ενότητα αναφορές (ενημερώνονται με ονόματα, θέση και IP διευθύνσεις ώστε να ταιριάζει με το δεδομένο σενάριο)
  2.    Ενημερώστε τις μεταβλητές χρήστη στη δέσμη ενεργειών για να ταιριάζει με το περιβάλλον που πρόκειται να εκτελεστεί σε σχέση με (συνδρομές, ονόματα υπηρεσιών, κλπ) της δέσμης ενεργειών
  3.    Εκτέλεση της δέσμης ενεργειών σε PowerShell

**Σημείωση**: της περιοχής που καθορίζεται στη δέσμη ενεργειών PowerShell πρέπει να συμφωνεί με την περιοχή που καθορίζεται από το αρχείο xml ρύθμισης παραμέτρων δικτύου.

Όταν η δέσμη ενεργειών εκτελείται με επιτυχία, ενδέχεται να ληφθεί τα ακόλουθα βήματα μετά τη δέσμη ενεργειών:

1.  Ρύθμιση τους κανόνες τείχους προστασίας, αυτό καλύπτεται στην παρακάτω ενότητα με τίτλο: κανόνες τείχους προστασίας.
2.  Προαιρετικά στην ενότητα αναφορές είναι δύο δέσμες ενεργειών για να ρυθμίσετε το διακομιστή web και εφαρμογή διακομιστή με μια εφαρμογή web απλό για να επιτρέψετε δοκιμών με αυτήν τη ρύθμιση παραμέτρων DMZ.

Στην επόμενη ενότητα εξηγείται περισσότερες από τις προτάσεις δέσμες ενεργειών σε σχέση με τις ομάδες ασφαλείας δικτύου.

## <a name="network-security-groups-nsg"></a>Ομάδες ασφαλείας δικτύου (NSG)
Για αυτό το παράδειγμα, μια ομάδα NSG είναι ενσωματωμένη και, στη συνέχεια, φορτώνεται με έξι κανόνες. 

>[AZURE.TIP] Σε γενικές γραμμές, θα πρέπει πρώτα να δημιουργήσετε συγκεκριμένες κανόνων "Αποδοχή" και, στη συνέχεια, τελευταίος τους πιο γενικό κανόνες "Απόρριψη". Το υποδεικνύει που του έχουν ανατεθεί προτεραιότητα που είναι κανόνες που υπολογίζεται πρώτα. Αφού εντοπίσετε κίνηση για να εφαρμόσετε σε έναν συγκεκριμένο κανόνα, αξιολογούνται περαιτέρω κανόνες. Κανόνες NSG να εφαρμόσετε είτε την κατεύθυνση εισερχομένων ή εξερχομένων (από την πλευρά του υποδικτύου).

Δηλωτικά, τους παρακάτω κανόνες που δημιουργούνται για την εισερχόμενη κίνηση:

1.  Επιτρέπεται η εσωτερική κυκλοφορία DNS (θύρα 53)
2.  Επιτρέπεται η κυκλοφορία RDP (θύρα 3389) από το Internet για τυχόν εικονική Μηχανή
3.  Επιτρέπεται η κυκλοφορία HTTP (θύρα 80) από το Internet για να το NVA (τείχος προστασίας)
4.  Επιτρέπεται οποιαδήποτε κυκλοφορία (όλες οι θύρες) από IIS01 στο AppVM1
5.  Οποιαδήποτε κυκλοφορία (όλες οι θύρες) από το Internet για ολόκληρο το VNet (και τα δύο δευτερεύοντα δίκτυα) δεν επιτρέπεται η
6.  Δεν επιτρέπεται η οποιαδήποτε κυκλοφορία (όλες οι θύρες) από το υποδίκτυο Frontend στο υποδίκτυο παρασκηνίου

Με αυτούς τους κανόνες δεσμευμένο με κάθε υποδίκτυο, εάν μια αίτηση HTTP ήταν εισερχομένων από το Internet στο διακομιστή web, και οι δύο κανόνες 3 (επιτρέπουν) και 5 (απόρριψη) θα εφαρμοστεί, αλλά επειδή κανόνα 3 έχει υψηλότερη προτεραιότητα, μόνο θα εφαρμόζεται και δεν θα προέρχονται κανόνα 5 σε αναπαραγωγή. Έτσι η αίτηση HTTP θα επιτρέπεται να το τείχος προστασίας. Εάν η ίδια κυκλοφορία ήταν προσπαθεί να επικοινωνήσει με το διακομιστή DNS01, κανόνα 5 (άρνηση) θα είναι το πρώτο για να εφαρμόσετε και η κίνηση δεν θα επιτρέπεται να στείλει στο διακομιστή. Κανόνας 6 (άρνηση) αποκλείει το υποδίκτυο Frontend από συζήτησης στο υποδίκτυο παρασκηνίου (εκτός των επιτρεπόμενων κίνηση σε κανόνες 1 και 4), αυτό προστατεύει το δίκτυο παρασκηνίου σε περίπτωση μια παραχωρήσεων εισβολέας την εφαρμογή web στον το Frontend, ο εισβολέας θα έχουν περιορισμένη πρόσβαση στο δίκτυο παρασκηνίου "προστατευμένη" (μόνο για τους πόρους που εκτίθενται στο διακομιστή AppVM01).

Υπάρχει ένας κανόνας εξερχομένων προεπιλεγμένη που επιτρέπει την κυκλοφορία ανάληψη στο Internet. Για αυτό το παράδειγμα, θα σας είστε επιτρέποντας την εξερχόμενη κυκλοφορία και δεν τροποποιώντας τους κανόνες εξερχομένων. Για να κλειδώσετε κίνηση στο και οι δύο κατευθύνσεις, τη δρομολόγηση που ορίζονται από το χρήστη είναι απαραίτητο, αυτό είναι εξερευνήσει σε μια διαφορετική παράδειγμα που μπορεί να βρίσκονται στο [κύριο ασφαλείας όριο εγγράφου][HOME].

Οι παραπάνω αναφέρονται κανόνες NSG είναι παρόμοια με τους κανόνες NSG στο [παράδειγμα 1 - Δημιουργήστε μια απλή DMZ με NSGs][Example1]. Εξετάστε την περιγραφή NSG σε αυτό το έγγραφο για μια λεπτομερή ματιά σε κάθε κανόνα NSG και τα χαρακτηριστικά του.

## <a name="firewall-rules"></a>Κανόνες τείχους προστασίας
Ένα πρόγραμμα-πελάτης διαχείρισης θα πρέπει να έχει εγκατασταθεί σε έναν Υπολογιστή για να διαχειριστείτε το τείχος προστασίας και να δημιουργήσετε τις ρυθμίσεις παραμέτρων, είναι απαραίτητο. Σχετικά με τον τρόπο για να διαχειριστείτε τη συσκευή, ανατρέξτε στο θέμα τον προμηθευτή τεκμηρίωση από το τείχος προστασίας σας (ή άλλα NVA). Το υπόλοιπο αυτής της ενότητας θα περιγράφουν τη ρύθμιση παραμέτρων του τείχους προστασίας μόνη της, μέσω του προμηθευτές διαχείρισης προγράμματος-πελάτη (δηλαδή, δεν Azure πύλη ή PowerShell).

Οδηγίες για τη λήψη του προγράμματος-πελάτη και τη σύνδεση με το Barracuda που χρησιμοποιούνται σε αυτό το παράδειγμα, μπορείτε να βρείτε εδώ: [Barracuda ΣΗ διαχείρισης](https://techlib.barracuda.com/NG61/NGAdmin)

Το τείχος προστασίας, κανόνες προώθησης θα πρέπει να δημιουργηθεί. Εφόσον αυτό το παράδειγμα δρομολογεί μόνο την κυκλοφορία internet εισερχομένων για να το τείχος προστασίας και, στη συνέχεια, στο διακομιστή web, απαιτείται μία μόνο προώθησης NAT κανόνα. Το τείχος προστασίας NextGen Barracuda που χρησιμοποιούνται σε αυτό το παράδειγμα ο κανόνας θα ήταν κανόνα NAT προορισμού ("θερινή ώρα NAT") για τη μεταβίβαση αυτήν την κυκλοφορία.

Για να δημιουργήσετε τον παρακάτω κανόνα (ή να επαληθεύσετε υπάρχοντα προεπιλεγμένους κανόνες), ξεκινώντας από τον πίνακα εργαλείων του προγράμματος-πελάτη Barracuda ΣΗ διαχείρισης, μεταβείτε στην καρτέλα ρύθμισης παραμέτρων, στις ρυθμίσεις παραμέτρων λειτουργικές ενότητας, κάντε κλικ στην επιλογή Ruleset. Ένα πλέγμα που ονομάζεται, "Κύριες κανόνων" θα εμφανίζονται οι υπάρχοντες κανόνες ενεργό και απενεργοποιημένη στο τείχος προστασίας. Στην επάνω δεξιά γωνία του πλέγματος αυτό είναι ένα μικρό, πράσινο "+" κουμπί, κάντε κλικ για να δημιουργήσετε έναν νέο κανόνα (Σημείωση: το τείχος προστασίας μπορεί να είναι "κλειδωμένες" για αλλαγές, εάν βλέπετε ένα κουμπί σημανθεί "Κλείδωμα" και δεν μπορείτε να δημιουργήσετε ή να επεξεργαστείτε τους κανόνες, επιλέξτε αυτό το κουμπί για να "Ξεκλείδωμα" το ruleset και να επιτρέπεται η επεξεργασία). Εάν θέλετε να επεξεργαστείτε έναν υπάρχοντα κανόνα, επιλέξτε αυτόν τον κανόνα, κάντε δεξί κλικ και επιλέξτε Επεξεργασία κανόνα.

Δημιουργία νέου κανόνα και δώστε ένα όνομα, όπως "WebTraffic". 

Το εικονίδιο κανόνα προορισμού NAT μοιάζει κάπως έτσι: ![Εικονίδιο NAT προορισμού][2]

Ο κανόνας ίδια θα είναι κάπως έτσι:

![Κανόνας τείχους προστασίας][3]

Εδώ οποιαδήποτε εισερχόμενη διεύθυνση που επισκέψεις το τείχος προστασίας προσπάθεια για την επίτευξη HTTP (θύρα 80 ή 443 για το HTTPS) θα αποστέλλεται περιβάλλοντος εργασίας "Τοπική διεύθυνση IP DHCP1" το τείχος προστασίας και θα ανακατευθύνεται στο διακομιστή Web με τη διεύθυνση IP του 10.0.1.5. Εφόσον η κίνηση είναι σύντομα στη θύρα 80 και μεταβαίνοντας στο διακομιστή web στη θύρα 80 ήταν απαιτείται καμία αλλαγή θύρα. Ωστόσο, η λίστα προορισμού μπορεί να έχουν 10.0.1.5:8080 εάν ακούσαμε μας διακομιστή Web στη θύρα 8080, επομένως, τη μετάφραση την εισερχόμενη θύρα 80 του τείχους προστασίας στη θύρα εισερχομένων 8080 στο διακομιστή web.

Μια μέθοδο σύνδεσης θα πρέπει επίσης να δηλώνεται τιμή, για τον κανόνα προορισμού από το Internet, "Δυναμική SNAT" είναι πιο κατάλληλο. 

Παρόλο που έχει δημιουργηθεί μόνο έναν κανόνα, είναι σημαντικό ότι την προτεραιότητά έχει ρυθμιστεί σωστά. Εάν στο πλέγμα όλων των κανόνων στο τείχος προστασίας είναι αυτόν τον νέο κανόνα κάτω (κάτω από τον κανόνα "BLOCKALL") θα προέρχονται ποτέ σε αναπαραγωγή. Βεβαιωθείτε ότι ο κανόνας που μόλις δημιουργήθηκε για κυκλοφορία web βρίσκεται επάνω από τον κανόνα BLOCKALL.

Αφού δημιουργηθεί ο κανόνας, πρέπει να προωθούνται στις το τείχος προστασίας και, στη συνέχεια, ενεργοποιηθεί, εάν αυτό δεν μπορεί να γίνει η αλλαγή κανόνα δεν θα ισχύσουν. Η διαδικασία push και την ενεργοποίηση περιγράφεται στην επόμενη ενότητα.

## <a name="rule-activation"></a>Ενεργοποίηση του κανόνα
Με το ruleset τροποποίησης για να προσθέσετε αυτόν τον κανόνα, το ruleset πρέπει να αποσταλεί σε το τείχος προστασίας και ενεργοποιηθεί.

![Ενεργοποίηση του κανόνα τείχους προστασίας][4]

Στην επάνω δεξιά γωνία του προγράμματος-πελάτη διαχείρισης είναι ένα σύμπλεγμα κουμπιών. Κάντε κλικ στο κουμπί "Αποστολή αλλαγών" για να στείλετε τους κανόνες που έχουν τροποποιηθεί σε το τείχος προστασίας και, στη συνέχεια, κάντε κλικ στο κουμπί "Ενεργοποίηση".

Με την ενεργοποίηση του ruleset το τείχος προστασίας αυτό το παράδειγμα περιβάλλον build έχει ολοκληρωθεί. Προαιρετικά, οι δέσμες ενεργειών Δόμηση δημοσίευση στην ενότητα αναφορές μπορούν να εκτελεστούν για να προσθέσετε μια εφαρμογή σε αυτό το περιβάλλον για να ελέγξετε την παρακάτω σενάρια κίνηση.

>[AZURE.IMPORTANT] Είναι σημαντικό να γνωρίζετε ότι θα δεν πατήσετε το διακομιστή web απευθείας. Όταν ένα πρόγραμμα περιήγησης ζητήσει μια σελίδα HTTP από FrontEnd001.CloudApp.Net, το τελικό σημείο HTTP (θύρα 80) προωθεί την κυκλοφορία για το τείχος προστασίας δεν το διακομιστή web. Το τείχος προστασίας, στη συνέχεια, σύμφωνα με τον κανόνα δημιουργήθηκε παραπάνω, NAT που αίτηση στο διακομιστή Web.

## <a name="traffic-scenarios"></a>Κίνηση σενάρια

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Επιτρέπεται) Διακομιστής Web στο Web μέσω του τείχους προστασίας
1.  Σελίδα χρήστη HTTP αιτήσεις Internet από FrontEnd001.CloudApp.Net (Internet υπηρεσία Cloud αντικριστές)
2.  Κίνηση φάσεις υπηρεσία cloud μέσω Άνοιγμα τελικό σημείο στη θύρα 80 με τοπική διασύνδεση τείχος προστασίας στην 10.0.1.4:80
3.  Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    Εφαρμογή κανόνα NSG 3 (Internet στο τείχος προστασίας), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
4.  Κίνηση επισκέψεις εσωτερική διεύθυνση IP του τείχους προστασίας (10.0.1.4)
5.  Ανατρέξτε στο θέμα κανόνα προώθησης τείχος προστασίας αυτό είναι κυκλοφορία θύρα 80, ανακατευθύνει το διακομιστή web IIS01
6.  IIS01 κάνει ακρόαση για κυκλοφορία web, λάβει αυτό το αίτημα και ξεκινά η επεξεργασία της αίτησης
7.  IIS01 ζητά από τον SQL Server σε AppVM01 πληροφορίες
8.  Δεν υπάρχει κανόνες εξερχόμενων στο προσκήνιο υποδίκτυο, επιτρέπεται κίνηση
9.  Το υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    NSG κανόνα 3 (Internet στο τείχος προστασίας) δεν εφαρμόζονται, μετακίνηση στο επόμενο κανόνα
  4.    4 κανόνα NSG εφαρμογή (IIS01 να AppVM01), κυκλοφορία επιτρέπεται, διακοπή επεξεργασίας κανόνων
10. AppVM01 λαμβάνει το ερώτημα SQL και απαντούν
11. Εφόσον δεν υπάρχουν εξερχομένων κανόνες στο υποδίκτυο παρασκηνίου επιτρέπεται η απόκριση
12. Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    Δεν υπάρχει κανένας κανόνας NSG που ισχύει για εισερχόμενα κίνησης από το υποδίκτυο παρασκηνίου στο προσκήνιο υποδίκτυο, ώστε να κανένα από τα NSG κανόνες ισχύουν
  2.    Ο προεπιλεγμένος κανόνας σύστημα επιτρέποντας την κυκλοφορία μεταξύ δευτερευόντων δικτύων επιτρέπει αυτήν την κυκλοφορία, ώστε να επιτρέπεται η κίνηση.
13. Ο διακομιστής των υπηρεσιών IIS λαμβάνει την απάντηση SQL και ολοκληρώνει την απόκριση HTTP και αποστέλλει αιτούντα
14. Επειδή αυτή είναι μια περίοδο λειτουργίας NAT από το τείχος προστασίας, ο προορισμός απόκρισης είναι (αρχικά) για το τείχος προστασίας
15. Το τείχος προστασίας λαμβάνει την απάντηση από το διακομιστή Web και το προωθεί προς το χρήστη στο Internet
16. Καθώς υπάρχουν επιτρέπεται καμία κανόνες εξερχόμενων στο υποδίκτυο Frontend την απάντηση και ο χρήστης Internet λαμβάνει την ιστοσελίδα που ζητήθηκε.

#### <a name="allowed-rdp-to-backend"></a>(Επιτρέπεται) RDP για υπολογιστή στο παρασκήνιο
1.  Διαχειριστής του διακομιστή στο internet αιτήσεις RDP περίοδο λειτουργίας να AppVM01 σε BackEnd001.CloudApp.Net:xxxxx όπου xxxxx είναι ο αριθμός θύρας που έχει αντιστοιχιστεί τυχαία για RDP να AppVM01 (τη θύρα που του έχουν ανατεθεί βρίσκονται στην πύλη του Azure ή μέσω του PowerShell)
2.  Επειδή το τείχος προστασίας ακρόαση μόνο στη διεύθυνση FrontEnd001.CloudApp.Net, δεν σχετίζεται με αυτήν τη ροή κυκλοφορίας
3.  Υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    Εφαρμογή NSG κανόνα 2 (RDP), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
4.  Με κανόνες εξερχομένων, προεπιλεγμένους κανόνες ισχύουν και επιτρέπεται η κυκλοφορία επιστροφής
5.  Είναι ενεργοποιημένη η περίοδος RDP
6.  AppVM01 ζητά κωδικού πρόσβασης χρήστη

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Επιτρέπεται) Αναζήτηση DNS διακομιστή Web στο διακομιστή DNS
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Επιτρέπεται) Αρχείο access διακομιστή Web σε AppVM01
1.  IIS01 ζητά από ένα αρχείο στο AppVM01
2.  Δεν υπάρχει κανόνες εξερχόμενων στο προσκήνιο υποδίκτυο, επιτρέπεται κίνηση
3.  Το υποδίκτυο παρασκηνίου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    NSG κανόνα 3 (Internet στο τείχος προστασίας) δεν εφαρμόζονται, μετακίνηση στο επόμενο κανόνα
  4.    4 κανόνα NSG εφαρμογή (IIS01 να AppVM01), κυκλοφορία επιτρέπεται, διακοπή επεξεργασίας κανόνων
4.  AppVM01 λαμβάνει την αίτηση και απαντούν με το αρχείο (αν υποθέσουμε ότι επιτρέπεται η πρόσβαση)
5.  Εφόσον δεν υπάρχουν εξερχομένων κανόνες στο υποδίκτυο παρασκηνίου επιτρέπεται η απόκριση
6.  Frontend υποδικτύου αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    Δεν υπάρχει κανένας κανόνας NSG που ισχύει για εισερχόμενα κίνησης από το υποδίκτυο παρασκηνίου στο προσκήνιο υποδίκτυο, ώστε να κανένα από τα NSG κανόνες ισχύουν
  2.    Ο προεπιλεγμένος κανόνας σύστημα επιτρέποντας την κυκλοφορία μεταξύ δευτερευόντων δικτύων επιτρέπει αυτήν την κυκλοφορία, ώστε να επιτρέπεται η κίνηση.
7.  Ο διακομιστής των υπηρεσιών IIS λαμβάνει το αρχείο

#### <a name="denied-web-direct-to-web-server"></a>(Δεν επιτρέπεται η) Web απευθείας σε διακομιστή Web
Επειδή το διακομιστή Web, IIS01 και το τείχος προστασίας είναι στην ίδια υπηρεσία Cloud μπορούν να μοιράζονται τα ίδια δημόσια αντικριστές διεύθυνση IP. Έτσι οποιαδήποτε κυκλοφορία HTTP θα κατευθύνεται στη το τείχος προστασίας. Ενώ θα εξυπηρετεί με επιτυχία την αίτηση, αυτό δεν μπορείτε να μεταβείτε απευθείας στο διακομιστή Web, τη διέλευσή, όπως έχει σχεδιαστεί, μέσω του τείχους προστασίας πρώτα. Ανατρέξτε στο θέμα το πρώτο σενάριο σε αυτήν την ενότητα για τη ροή κυκλοφορίας.

#### <a name="denied-web-to-backend-server"></a>(Δεν επιτρέπεται η) Ιστοσελίδα στο διακομιστή παρασκηνίου
1.  Internet χρήστης προσπαθήσει να αποκτήσετε πρόσβαση σε ένα αρχείο στο AppVM01 μέσω της υπηρεσίας BackEnd001.CloudApp.Net
2.  Καθώς υπάρχουν χωρίς τα τελικά σημεία ανοιχτό για κοινή χρήση αρχείων, αυτό θα περάσει την υπηρεσία Cloud και δεν θα επικοινωνήσει με το διακομιστή
3.  Εάν τα τελικά σημεία ανοιχτό για κάποιο λόγο, NSG κανόνα 5 (Internet για να VNet) να αποκλείσετε αυτήν την κυκλοφορία

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Δεν επιτρέπεται η) Αναζήτηση DNS Web σε διακομιστή DNS
1.  Internet χρήστης προσπαθήσει να αναζητήσετε μια εσωτερική εγγραφή DNS στην DNS01 μέσω της υπηρεσίας BackEnd001.CloudApp.Net
2.  Εφόσον δεν υπάρχουν ανοιχτά για DNS χωρίς τα τελικά σημεία, αυτό δεν θα μεταβιβάζουν την υπηρεσία Cloud και δεν θα επικοινωνήσει με το διακομιστή
3.  Εάν τα τελικά σημεία ανοιχτό για κάποιο λόγο, NSG κανόνα 5 (Internet για να VNet) να αποκλείσετε αυτήν την κυκλοφορία (Σημείωση: αυτόν τον κανόνα 1 (DNS) δεν θα ισχύουν για δύο λόγους, πρώτα τη διεύθυνση της προέλευσης είναι στο internet, αυτός ο κανόνας ισχύει μόνο για το τοπικό VNet ως προέλευση, επίσης αυτή είναι ένας κανόνας αποδοχή, ώστε το ποτέ να απορρίψετε την κίνηση)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Δεν επιτρέπεται η) Web στην access SQL μέσω του τείχους προστασίας
1.  Χρήστης του Internet ζητά δεδομένα SQL από FrontEnd001.CloudApp.Net (Internet υπηρεσία Cloud αντικριστές)
2.  Εφόσον δεν τα τελικά σημεία είναι ανοιχτό για SQL, αυτό δεν θα μεταβιβάζουν την υπηρεσία Cloud και δεν θα επίτευξη το τείχος προστασίας
3.  Εάν τελικά σημεία ανοιχτό για κάποιο λόγο, το υποδίκτυο Frontend αρχίζει Επεξεργασία κανόνα εισερχομένων:
  1.    NSG κανόνα 1 (DNS) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  2.    NSG κανόνα 2 (RDP) δεν ισχύουν, μετακίνηση στο επόμενο κανόνα
  3.    Εφαρμογή κανόνα NSG 2 (Internet στο τείχος προστασίας), κίνηση είναι η επεξεργασία επιτρέπεται, διακόψτε κανόνα
4.  Κίνηση επισκέψεις εσωτερική διεύθυνση IP του τείχους προστασίας (10.0.1.4)
5.  Τείχος προστασίας έχει κανένας κανόνας προώθησης για SQL και αποθέτει την κυκλοφορία

## <a name="conclusion"></a>Ολοκλήρωση
Αυτός είναι ένας τρόπος σχετικά σαφή προστασία την εφαρμογή σας με ένα τείχος προστασίας και απομόνωση το υποδίκτυο παρασκηνίου από εισερχόμενης κυκλοφορίας.

Μπορείτε να βρείτε περισσότερα παραδείγματα και μια επισκόπηση του δικτύου όρια ασφαλείας [εδώ][HOME].

## <a name="references"></a>Αναφορές
### <a name="main-script-and-network-config"></a>Κύρια δέσμη ενεργειών και ρύθμισης παραμέτρων δικτύου
Αποθηκεύστε την πλήρη δέσμη ενεργειών σε ένα αρχείο δέσμης ενεργειών του PowerShell. Αποθηκεύστε το ρύθμισης παραμέτρων δικτύου, σε ένα αρχείο με το όνομα "NetworkConf2.xml".
Τροποποιήστε τις μεταβλητές που ορίζονται από το χρήστη, όπως απαιτείται. Εκτελέστε τη δέσμη ενεργειών και, στη συνέχεια, ακολουθήστε τις οδηγίες της εγκατάστασης κανόνα του τείχους προστασίας που είναι παραπάνω.

#### <a name="full-script"></a>Πλήρης δέσμης ενεργειών
Αυτή η δέσμη ενεργειών θα, με βάση τις μεταβλητές που ορίζονται από το χρήστη:

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
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
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
       myFirewall - 10.0.1.4
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
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
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
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
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
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Εισερχόμενη DMZ με NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Εικονίδιο NAT προορισμού"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Κανόνας τείχους προστασίας"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Ενεργοποίηση του κανόνα τείχους προστασίας"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
