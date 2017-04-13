<properties
    pageTitle="Azure CLI εντολές στη λειτουργία της υπηρεσίας διαχείρισης | Microsoft Azure"
    description="Εντολές Azure περιβάλλον γραμμής εντολών (CLI) στη λειτουργία της υπηρεσίας διαχείρισης για τη Διαχείριση αναπτύξεων του μοντέλου κλασική ανάπτυξης"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure CLI εντολές σε λειτουργία Azure Service Management (asm)

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Μπορείτε επίσης να [διαβάσετε σχετικά με όλες τις εντολές για τη διαχείριση πόρων μοντέλο](virtual-machines/azure-cli-arm-commands.md), και χρησιμοποιήστε το CLI για να [μετεγκαταστήσετε πόρους](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) από το κλασικό στο μοντέλο διαχείρισης πόρων.

Σε αυτό το άρθρο παρέχει σύνταξη και τις επιλογές για τις εντολές Azure CLI που θα χρησιμοποιούσατε συνήθως για να δημιουργήσετε και να διαχειριστείτε Azure πόρους στο μοντέλο κλασική ανάπτυξης. Μπορείτε να αποκτήσετε πρόσβαση αυτές οι εντολές, εκτελώντας το CLI στη λειτουργία Azure Service Management (asm). Δεν είναι ολοκληρωμένη αναφορά και την έκδοση CLI μπορεί να εμφανίζονται ελαφρώς διαφορετικές εντολές ή παράμετροι. 

Για να ξεκινήσετε, πρώτη [εγκατάσταση του Azure CLI](xplat-cli-install.md) και να [συνδεθείτε με τη συνδρομή σας στο Azure](xplat-cli-connect.md).

Για την τρέχουσα εντολή σύνταξη και τις επιλογές στη γραμμή εντολών, πληκτρολογήστε `azure help` ή, για να εμφανίσετε τη Βοήθεια για μια συγκεκριμένη εντολή, `azure help [command]`. Επίσης, βρείτε CLI παραδείγματα στην τεκμηρίωση για τη δημιουργία και τη διαχείριση συγκεκριμένων υπηρεσιών Azure.

Προαιρετικές παράμετροι εμφανίζονται σε αγκύλες (για παράδειγμα, `[parameter]`). Όλες οι υπόλοιπες παράμετροι απαιτούνται.

Εκτός από την εντολή συγκεκριμένο προαιρετικές παραμέτρους τεκμηριώνονται εδώ, υπάρχουν τρία προαιρετικές παράμετροι που μπορούν να χρησιμοποιηθούν για να εμφανίσετε λεπτομερείς εξόδου όπως επιλογές αίτηση και τους κωδικούς κατάστασης. Το `-v` παράμετρος παρέχει λεπτομερείς πληροφορίες, και το `-vv` παραμέτρου παρέχει ακόμα πιο λεπτομερή λεπτομερή δεδομένα εξόδου. Το `--json` επιλογή εξάγει το αποτέλεσμα σε μορφή ανεπεξέργαστα json.

## <a name="setting-asm-mode"></a>Ρύθμιση asm λειτουργία

Χρησιμοποιήστε την παρακάτω εντολή για να ενεργοποιήσετε τις εντολές της λειτουργίας Azure CLI υπηρεσία διαχείρισης.

    azure config mode asm

>[AZURE.NOTE] Το CLI Azure διαχείρισης πόρων και διαχείριση της υπηρεσίας Azure κατάστασης λειτουργίας είναι αμοιβαία αποκλειόμενα. Αυτό σημαίνει ότι, τους πόρους που έχουν δημιουργηθεί με μία λειτουργία δεν είναι δυνατό να γίνεται από την κατάσταση λειτουργίας.

## <a name="manage-your-account-information-and-publish-settings"></a>Διαχείριση πληροφοριών του λογαριασμού σας και ρυθμίσεις δημοσίευσης
Ένας τρόπος το CLI μπορεί να συνδεθείτε στο λογαριασμό σας είναι χρησιμοποιώντας τις πληροφορίες Azure τη συνδρομή σας. (Ανατρέξτε στο θέμα [σύνδεση σε Azure συνδρομή από την CLI Azure](xplat-cli-connect.md) για άλλες επιλογές.) Αυτές οι πληροφορίες μπορεί να ληφθεί από την πύλη του Azure κλασική σε ένα αρχείο ρυθμίσεων δημοσίευση, όπως περιγράφεται εδώ. Μπορείτε να εισαγάγετε το αρχείο ρυθμίσεων δημοσίευση που χρησιμοποιεί μια μόνιμη τοπική ρύθμιση παραμέτρων που το CLI για διαδοχικές λειτουργίες. Θέλετε να εισαγάγετε τις ρυθμίσεις δημοσίευσης μία φορά.

**λήψη λογαριασμό [επιλογές]**

Αυτή η εντολή εκκινεί το πρόγραμμα περιήγησης για να κάνετε λήψη του αρχείου .publishsettings από την πύλη του Azure κλασική.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**Εισαγωγή [επιλογές] λογαριασμού &lt;αρχείο >**


Αυτή η εντολή εισαγάγει ένα αρχείο publishsettings ή πιστοποιητικό ώστε να μπορεί να χρησιμοποιηθεί από το εργαλείο σε μελλοντικές περιόδους λειτουργίας.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Το αρχείο publishsettings μπορεί να περιέχουν λεπτομέρειες (δηλαδή, όνομα της συνδρομής και ID) σχετικά με περισσότερες από μία συνδρομές. Όταν εισάγετε το αρχείο publishsettings, την πρώτη εγγραφή χρησιμοποιείται ως η προεπιλεγμένη περιγραφή. Για να χρησιμοποιήσετε μια διαφορετική συνδρομή, εκτελέστε την ακόλουθη εντολή:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**Καταργήστε την επιλογή [επιλογές] λογαριασμού**

Αυτή η εντολή καταργεί το αποθηκευμένο publishsettings που έχουν εισαχθεί. Χρησιμοποιήστε αυτή την εντολή, εάν έχετε ολοκληρώσει την χρησιμοποιώντας το εργαλείο σε αυτόν τον υπολογιστή και θέλετε να εξασφαλίσετε ότι το εργαλείο δεν μπορεί να χρησιμοποιηθεί με το λογαριασμό σας στο μέλλον.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**λίστα λογαριασμών [επιλογές]**

Λίστα με τις συνδρομές που έχουν εισαχθεί

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**ρύθμιση λογαριασμού [επιλογές] &lt;συνδρομή&gt;**

Ορίστε την τρέχουσα συνδρομή σας

###<a name="commands-to-manage-your-affinity-groups"></a>Εντολές για να διαχειριστείτε τις ομάδες σας συσχέτισης

**λίστα συσχέτισης ομάδα λογαριασμών [επιλογές]**

Αυτή η εντολή παραθέτει τις ομάδες σας Azure συνάφεια.

Συνάφεια ομάδες μπορούν να οριστούν όταν μια ομάδα εικονικές μηχανές εκτείνεται σε πολλές φυσικής μηχανές. Η ομάδα συσχέτισης Καθορίζει ότι η φυσική μηχανές πρέπει να είναι όσο πιο κοντά μεταξύ τους όσο το δυνατόν, για να μειώσετε την αδράνεια δικτύου.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**ομάδα συσχέτισης λογαριασμών Δημιουργία [επιλογές] &lt;όνομα&gt;**

Αυτή η εντολή δημιουργεί μια ομάδα συσχέτισης

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**ομάδα συσχέτισης λογαριασμών εμφάνιση [επιλογές] &lt;όνομα&gt;**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες της ομάδας συσχέτισης

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**λογαριασμός ομάδα συσχέτισης διαγραφή [επιλογές] &lt;όνομα&gt;**

Αυτή η εντολή διαγράφει την καθορισμένη ομάδα συσχέτισης

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Εντολές για τη διαχείριση του περιβάλλοντος του λογαριασμού

**λίστα φάκελος λογαριασμού [επιλογές]**

Λίστα με τα περιβάλλοντα λογαριασμού

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**φάκελος λογαριασμού εμφάνιση [επιλογές] [περιβάλλον]**

Εμφάνιση λεπτομερειών περιβάλλον λογαριασμού

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**λογαριασμός φάκελος Προσθήκη [επιλογές] [περιβάλλον]**

Αυτή η εντολή προσθέτει ένα περιβάλλον με το λογαριασμό

**φάκελος λογαριασμού ρύθμιση [επιλογές] [περιβάλλον]**

Αυτή η εντολή ορίζει το περιβάλλον λογαριασμού

**λογαριασμός φάκελος διαγραφή [επιλογές] [περιβάλλον]**

Αυτή η εντολή διαγράφει το καθορισμένο περιβάλλον από το λογαριασμό

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Εντολές για τη διαχείριση του κλασική εικονικές μηχανές
Το παρακάτω διάγραμμα παρουσιάζει πώς κλασική Azure εικονικές μηχανές φιλοξενούνται στο περιβάλλον ανάπτυξης παραγωγής από μια υπηρεσία Azure cloud.

![Azure τεχνική διαγράμματος](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Δημιουργία-νέα** δημιουργεί τη μονάδα δίσκου στο χώρο αποθήκευσης αντικειμένων blob (δηλαδή, e:\ στο διάγραμμα); **Επισύναψη** επισυνάπτει ένα δίσκο ήδη που έχουν δημιουργηθεί αλλά συνημμένη μια εικονική μηχανή.

**εικονική δημιουργία [επιλογές] &lt;όνομα dns > &lt;εικόνα > &lt;userName > [κωδικός πρόσβασης]**

Αυτή η εντολή δημιουργεί μια εικονική μηχανή Azure. Από προεπιλογή, κάθε εικονική μηχανή (εικονική) δημιουργείται στο δικό του υπηρεσία στο cloud. Μπορείτε να καθορίσετε ότι πρέπει να προστεθεί μια εικονική μηχανή σε μια υπάρχουσα υπηρεσία cloud μέσω της χρήσης της επιλογής - c που τεκμηριώνονται εδώ.

Εικονική την εντολή "Δημιουργία", όπως το Azure κλασική πύλη, μόνο δημιουργεί εικονικές μηχανές στο περιβάλλον ανάπτυξης παραγωγής. Δεν υπάρχει επιλογή για να δημιουργήσετε μια εικονική μηχανή στο περιβάλλον ανάπτυξης ενδιάμεσου σταδίου από μια υπηρεσία στο cloud. Εάν η συνδρομή σας δεν διαθέτει έναν υπάρχοντα λογαριασμό Azure χώρου αποθήκευσης, η εντολή δημιουργεί μία.

Μπορείτε να καθορίσετε μια θέση μέσω της--παραμέτρου θέση, ή μπορείτε να καθορίσετε μια ομάδα συσχέτισης μέσω της--παραμέτρου συσχέτισης ομάδας. Εάν δεν παρέχεται, θα σας ζητηθεί να δώσετε έναν από μια λίστα με έγκυρες θέσεις.

Τον κωδικό πρόσβασης που παρέχεται πρέπει να είναι 8-123 χαρακτήρες και να πληροί τις απαιτήσεις πολυπλοκότητα κωδικού πρόσβασης από το λειτουργικό σύστημα που χρησιμοποιείτε για αυτόν τον υπολογιστή εικονική.

Εάν σκοπεύετε να χρησιμοποιήσετε SSH για τη Διαχείριση ανεπτυγμένος Linux εικονικό μηχάνημα (όπως συνήθως συμβαίνει), πρέπει να ενεργοποιήσετε το SSH μέσω της επιλογής -e όταν δημιουργείτε την εικονική μηχανή. Δεν είναι δυνατή η ενεργοποίηση SSH αφού δημιουργηθεί η εικονική μηχανή.

Εικονικές μηχανές Windows να ενεργοποιήσετε RDP αργότερα, προσθέτοντας θύρα 3389 ως τελικού σημείου.

Τις ακόλουθες προαιρετικές παραμέτρους υποστηρίζονται για αυτήν την εντολή:

**- c,--σύνδεση** δημιουργία η εικονική μηχανή μέσα σε μια ανάπτυξη του ήδη που έχουν δημιουργηθεί σε μια υπηρεσία παροχής φιλοξενίας. Εάν - vmname δεν χρησιμοποιείται με αυτήν την επιλογή, το όνομα του νέου υπολογιστή εικονικές δημιουργείται αυτόματα.<br />
**-n,--εικονική όνομα** Καθορίστε το όνομα του υπολογιστή εικονική. Η παράμετρος αυτή σας μεταφέρει φιλοξενίας όνομα υπηρεσίας από προεπιλογή. Εάν δεν έχει καθοριστεί - vmname, το όνομα για τη νέα εικονική μηχανή δημιουργείται ως &lt;όνομα υπηρεσίας >&lt;αναγνωριστικό >, όπου &lt;αναγνωριστικό > είναι ο αριθμός των υπάρχοντα εικονικές μηχανές την υπηρεσία συν 1. Για παράδειγμα, εάν μπορείτε να χρησιμοποιήσετε αυτή την εντολή για να προσθέσετε μια εικονική μηχανή σε μια υπηρεσία φιλοξενίας MyService που έχει ένα υπάρχον εικονική μηχανή, η νέα εικονική μηχανή ονομάζεται MyService2.<br />
**-u,--blob-url** Καθορίστε τη διεύθυνση URL χώρο αποθήκευσης αντικειμένων blob προορισμού στην οποία θα δημιουργηθεί ο δίσκος εικονική μηχανή του συστήματος. <br />
**-z,--εικονική μέγεθος** Καθορίστε το μέγεθος του η εικονική μηχανή. Οι έγκυρες τιμές είναι: "ExtraSmall", "Μικρές", "Μεσαία", "Μεγάλο", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Η προεπιλεγμένη τιμή είναι "Μικρό". <br />
**-r** Προσθέτει συνδεσιμότητας RDP μια εικονική μηχανή των Windows. <br />
**-e,--ssh** Προσθέτει συνδεσιμότητας SSH μια εικονική μηχανή των Windows. <br />
**-t,--ssh πιστοποιητικού** Καθορίζει το πιστοποιητικό SSH. <br />
**-s** Η συνδρομή <br />
**-o,--Κοινότητας** Η καθορισμένη εικόνα είναι εικόνα Κοινότητας <br />
**-w** Το όνομα εικονικού δικτύου <br/>
**-l,--θέση** Καθορίζει τη θέση (για παράδειγμα, "Βόρεια κεντρική ΜΑΣ"). <br />
**-a,--συσχέτισης-ομάδα** Καθορίζει την ομάδα συσχέτισης.<br />
**-w,--όνομα εικονικού δικτύου** Καθορίστε το εικονικό δίκτυο στο οποίο θέλετε να προσθέσετε τη νέα εικονική μηχανή. Εικονικών δικτύων μπορεί να είναι ρύθμιση και διαχείριση από την πύλη του Azure κλασική.<br />
**-b,--υποδικτύου ονόματα** Καθορίζει τα ονόματα υποδικτύου για να αντιστοιχίσετε την εικονική μηχανή.

Σε αυτό το παράδειγμα, MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB είναι μια εικόνα που παρέχεται από την πλατφόρμα. Για περισσότερες πληροφορίες σχετικά με το λειτουργικό σύστημα εικόνες, ανατρέξτε στο θέμα εικονική εικόνα της λίστας.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**Δημιουργία από εικονική &lt;όνομα dns > &lt;ρόλο αρχείο >**

Αυτή η εντολή δημιουργεί μια εικονική μηχανή Azure από ένα αρχείο ρόλο JSON.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**εικονική λίστα [επιλογές]**

Αυτή η εντολή παραθέτει Azure εικονικές μηχανές. Η επιλογή--json Καθορίζει ότι τα αποτελέσματα επιστρέφονται με ανεπεξέργαστα JSON μορφή.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**λίστα θέση εικονική [επιλογές]**

Αυτή η εντολή παραθέτει όλες τις διαθέσιμες λογαριασμός Azure θέσεις.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**εικονική εμφάνιση [επιλογές] &lt;όνομα >**

Αυτή η εντολή εμφανίζει λεπτομέρειες σχετικά με μια εικονική μηχανή Azure. Η επιλογή--json Καθορίζει ότι τα αποτελέσματα επιστρέφονται με ανεπεξέργαστα JSON μορφή.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**εικονική διαγραφή [επιλογές] &lt;όνομα >**

Αυτή η εντολή διαγράφει μια εικονική μηχανή Azure. Από προεπιλογή, αυτή η εντολή δεν διαγράφει το αντικειμένων blob του Azure από τα οποία έχουν δημιουργηθεί ο δίσκος λειτουργικού συστήματος και του δίσκου δεδομένων. Για να διαγράψετε το αντικείμενο blob και η εικονική μηχανή στο οποίο βασίζεται, καθορίστε την επιλογή -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**εικονική Έναρξη [επιλογές] &lt;όνομα >**

Αυτή η εντολή ξεκινά μια εικονική μηχανή Azure.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**εικονική επανεκκινήστε [επιλογές] &lt;όνομα >**

Αυτή η εντολή επανεκκίνηση μια εικονική μηχανή Azure.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**εικονική τερματισμού [επιλογές] &lt;όνομα >**

Αυτή η εντολή τερματίζει μια εικονική μηχανή Azure. Μπορείτε να χρησιμοποιήσετε την επιλογή -p για να καθορίσετε ότι ο πόρος υπολογισμού δεν έχουν τεθεί σε τερματισμού.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**καταγραφή εικονική &lt;εικονική όνομα > &lt;όνομα προορισμού εικόνας >**

Αυτή η εντολή καταγράφει μια εικόνα Azure εικονική μηχανή.

Μια εικόνα εικονική μηχανή μπορούν να καταγραφούν μόνο εάν η κατάσταση εικονική μηχανή είναι **Διακοπή**. Τερματίστε την εικονική μηχανή πριν να συνεχίσετε.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**εικονική εξαγωγή [επιλογές] &lt;εικονική όνομα > &lt;διαδρομή αρχείου >**

Αυτή η εντολή εξάγει μια εικόνα Azure εικονική μηχανή σε ένα αρχείο

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Εντολές για τη διαχείριση του τελικά σημεία Azure εικονική μηχανή
Το παρακάτω διάγραμμα παρουσιάζει την αρχιτεκτονική μιας τυπικής ανάπτυξης πολλών παρουσιών της κλασικής εικονικό μηχάνημα. Σε αυτό το παράδειγμα, θύρα 3389 είναι ανοιχτή σε κάθε εικονική μηχανή (για πρόσβαση RDP). Υπάρχει επίσης μια εσωτερική διεύθυνση IP (για παράδειγμα, 168.55.11.1) σε κάθε εικονική μηχανή που χρησιμοποιείται από τη μονάδα εξισορρόπησης φόρτου στην κυκλοφορία δρομολόγηση στην εικονική μηχανή της. Αυτή η διεύθυνση IP εσωτερικού επίσης μπορεί να χρησιμοποιηθεί για την επικοινωνία μεταξύ εικονικές μηχανές.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Εξωτερική αιτήσεων σε εικονικές μηχανές δούμε μια μονάδα εξισορρόπησης φόρτου. Αυτόν το λόγο, δεν μπορεί να έχει καθοριστεί προσκλήσεις σε σχέση με ένα συγκεκριμένο εικονικό μηχάνημα σε αναπτύξεις με πολλές εικονικές μηχανές. Για αναπτύξεις με πολλές εικονικές μηχανές, αντιστοίχιση θύρας πρέπει να ρυθμιστεί μεταξύ του εικονικές μηχανές (Εικονική θύρα) και τη μονάδα εξισορρόπησης φόρτου (λίβρες-θύρα).

**Δημιουργία τελικού σημείου εικονική &lt;εικονική όνομα > &lt;λίβρες θύρα > [εικονική θύρα]**

Αυτή η εντολή δημιουργεί ένα τελικό σημείο εικονική μηχανή. Μπορείτε επίσης να χρησιμοποιήσετε -u ή--ενεργοποίηση-άμεσους-server-return για να καθορίσετε εάν για να ενεργοποιήσετε την επιστροφή απευθείας διακομιστή σε αυτό το τελικό σημείο, απενεργοποιημένη από προεπιλογή.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**εικονική τελικού σημείου δημιουργία μίας [επιλογές] &lt;εικονική όνομα > &lt;λίβρες θύρα > [:&lt;εικονική θύρα > [:&lt;πρωτόκολλο > [:&lt;enable-άμεσους-server-return > [:&lt;όνομα συνόλου λίβρες > [:&lt;δοκιμή του πρωτοκόλλου > [:&lt;θύρα δοκιμή του > [:&lt;δοκιμή του διαδρομή > [:&lt;όνομα εσωτερικού λίβρες >]]] {1 -*}**

Δημιουργία πολλών εικονική τελικά σημεία.

**τελικό σημείο εικονική διαγραφή [επιλογές] &lt;εικονική όνομα > &lt;όνομα τελικού σημείου >**

Αυτή η εντολή διαγράφει ένα τελικό σημείο εικονική μηχανή.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**λίστα τελικού σημείου εικονική &lt;εικονική όνομα >**

Αυτή η εντολή παραθέτει όλα τα τελικά σημεία εικονική μηχανή. Η επιλογή--json Καθορίζει ότι τα αποτελέσματα επιστρέφονται με ανεπεξέργαστα JSON μορφή.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**εικονική ορίου ενημέρωσης [επιλογές] &lt;εικονική όνομα > &lt;όνομα τελικού σημείου >**

Αυτή η εντολή ενημερώνει ένα τελικό σημείο εικονική σε νέες τιμές με αυτές τις επιλογές.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**τελικό σημείο εικονική εμφάνιση [επιλογές] &lt;εικονική όνομα >**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες της τα τελικά σημεία σε μια εικονική μηχανή

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Εντολές για να διαχειριστείτε τις εικόνες σας Azure εικονική μηχανή

Εικονική μηχανή εικόνες βρίσκονται οι καταγραφές του έχει ήδη ρυθμιστεί εικονικές μηχανές οι οποίες μπορούν να αναπαραχθούν όπως απαιτείται.

**λίστα εικόνων εικονική [επιλογές]**

Αυτή η εντολή λαμβάνει μια λίστα με εικονική μηχανή εικόνες. Υπάρχουν τρεις τύποι των εικόνων: δημιουργία εικόνων που έχουν δημιουργηθεί από τρίτους κατασκευαστές, το οποίο είναι το πρόθεμα με το όνομα του προμηθευτή και τις εικόνες που εικόνων που έχουν δημιουργηθεί από τη Microsoft, που είναι το πρόθεμα "MSFT",. Για να δημιουργήσετε εικόνες, μπορείτε να καταγράψετε μια υπάρχουσα εικονική μηχανή ή να δημιουργήσετε μια εικόνα από μια προσαρμοσμένη .vhd που έχουν αποσταλεί με το χώρο αποθήκευσης αντικειμένων blob. Για περισσότερες πληροφορίες σχετικά με τη χρήση μιας προσαρμοσμένης .vhd, ανατρέξτε στο θέμα Δημιουργία εικόνα εικονική.
Η επιλογή--json Καθορίζει ότι τα αποτελέσματα επιστρέφονται με ανεπεξέργαστα JSON μορφή.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**Εμφάνιση εικόνα εικονική [επιλογές] &lt;όνομα >**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες μιας εικόνας εικονική μηχανή.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**Διαγραφή εικόνας εικονική [επιλογές] &lt;όνομα >**

Αυτή η εντολή διαγράφει μια εικονική μηχανή εικόνα.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**Δημιουργία εικονική εικόνα &lt;όνομα > [διαδρομή προέλευσης]**

Αυτή η εντολή δημιουργεί μια εικονική μηχανή εικόνα. Τα αρχεία σας προσαρμοσμένη .vhd έχουν αποσταλεί στο χώρο αποθήκευσης blob και, στη συνέχεια, η εικόνα εικονική μηχανή δημιουργείται από εκεί. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε αυτή η εικονική μηχανή εικόνα για να δημιουργήσετε μια εικονική μηχανή. Απαιτούνται οι παράμετροι θέση και το λειτουργικό σύστημα.

>[AZURE.NOTE]Προς το παρόν αυτή η εντολή υποστηρίζει μόνο την αποστολή των αρχείων σταθερό .vhd. Για να αποστείλετε ένα αρχείο δυναμική .vhd, χρησιμοποιήστε τα [βοηθητικά προγράμματα Azure VHD για μετάβαση](https://github.com/Microsoft/azure-vhd-utils-for-go).

Ορισμένα συστήματα επιβάλλουν όρια περιγραφής αρχείων ανά διεργασία. Εάν υπάρχει υπέρβαση αυτό το όριο, το εργαλείο εμφανίζει ένα σφάλμα αρχείου περιγραφής όριο. Μπορείτε να εκτελέσετε την εντολή ξανά χρησιμοποιώντας το -p &lt;αριθμού > παραμέτρου για να μειώσετε τον μέγιστο αριθμό παράλληλες αποστολές. Ο προεπιλεγμένος μέγιστος αριθμός των αποστολών παράλληλες είναι 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Εντολές για τη διαχείριση δεδομένων δίσκων σας Azure εικονική μηχανή

Δίσκων δεδομένων είναι .vhd αρχεία στο χώρο αποθήκευσης αντικειμένων blob που μπορούν να χρησιμοποιηθούν από μια εικονική μηχανή. Για περισσότερες πληροφορίες σχετικά με τον τρόπο δίσκων δεδομένων αναπτύσσονται αντικειμένων blob χώρου αποθήκευσης, ανατρέξτε στο θέμα Azure τεχνική διάγραμμα φαίνεται παραπάνω.

Οι εντολές για επισύναψη δίσκων δεδομένων (Επισύναψη azure εικονική δίσκου και azure εικονική δίσκου επισύναψη-νέα) αντιστοιχίσετε έναν αριθμό λογικής μονάδας (LUN) στο δίσκο συνημμένα δεδομένα, όπως απαιτείται από το πρωτόκολλο SCSI. Ο πρώτος δίσκος δεδομένων που έχουν επισυναφθεί σε μια εικονική μηχανή εκχωρείται LUN 0, το επόμενο είναι που του έχουν ανατεθεί LUN 1, και ούτω καθεξής.

Όταν απόσπαση ένα δίσκο δεδομένων με το δίσκο azure εικονική απόσπαση εντολή, χρησιμοποιήστε το &lt;lun&gt; παραμέτρου για να υποδείξετε ποιες δίσκου για να αποσπάσετε.

>[AZURE.NOTE] Μπορείτε πάντα πρέπει να αποσπάσετε δίσκων δεδομένων με αντίστροφη σειρά, ξεκινώντας με τον LUN υψηλότερη με αρίθμηση που έχει αντιστοιχιστεί. Το επίπεδο Linux SCSI δεν υποστηρίζει απόσπασης LUN μικρότερες τιμές, ενώ εξακολουθεί να είναι συνδεδεμένος με μεγαλύτερες LUN. Για παράδειγμα, που δεν πρέπει να αποσπάσετε LUN 0 Εάν εξακολουθεί να είναι συνδεδεμένος LUN 1.

**Εμφάνιση δίσκου εικονική [επιλογές] &lt;όνομα >**

Αυτή η εντολή εμφανίζει λεπτομέρειες σχετικά με ένα δίσκο Azure.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**λίστα δίσκων εικονική [επιλογές] [εικονική-όνομα]**

Αυτή η εντολή λίστες Azure δίσκων ή δίσκων που έχουν επισυναφθεί σε μια καθορισμένη εικονική μηχανή. Εάν εκτελείται με μια παράμετρο ονόματος εικονική μηχανή, επιστρέφει όλων των δίσκων που έχουν επισυναφθεί σε η εικονική μηχανή. LUN 1 δημιουργείται με την εικονική μηχανή και τυχόν άλλες που παρατίθενται δίσκων είναι συνημμένα ξεχωριστά.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Εκτέλεση αυτής της εντολής χωρίς εικονική μηχανή όνομα παραμέτρου επιστρέφει όλων των δίσκων.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**Διαγραφή δίσκου εικονική [επιλογές] &lt;όνομα >**

Αυτή η εντολή διαγράφει ένα δίσκο Azure από έναν προσωπικό χώρο αποθήκευσης. Ο δίσκος πρέπει να είναι αποσυνδεθεί από τον υπολογιστή εικονικές πριν να διαγραφεί.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**Δημιουργία δίσκου εικονική &lt;όνομα > [διαδρομή προέλευσης]**

Αυτή η εντολή αποστολές και καταγράφει ένα δίσκο Azure. --blob url,--θέση, ή--συσχέτισης ομάδας πρέπει να έχει καθοριστεί. Εάν χρησιμοποιήσετε αυτή την εντολή με [διαδρομή προέλευσης], το καθορισμένο αρχείο .vhd αποστέλλεται και δημιουργείται μια εικόνα. Μπορείτε, στη συνέχεια, να επισυνάψετε αυτή η εικόνα σε μια εικονική μηχανή με χρήση του δίσκου εικονική επισύναψη.

Ορισμένα συστήματα επιβάλλουν όρια περιγραφής αρχείων ανά διεργασία. Εάν υπάρχει υπέρβαση αυτό το όριο, το εργαλείο εμφανίζει ένα σφάλμα αρχείου περιγραφής όριο. Μπορείτε να εκτελέσετε την εντολή ξανά χρησιμοποιώντας το -p &lt;αριθμού > παραμέτρου για να μειώσετε τον μέγιστο αριθμό παράλληλες αποστολές. Ο προεπιλεγμένος μέγιστος αριθμός των αποστολών παράλληλες είναι 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**εικονική δίσκου αποστολής [επιλογές] &lt;διαδρομή προέλευσης > &lt;blob-url > &lt;κλειδί λογαριασμού χώρου αποθήκευσης >**

Αυτή η εντολή σάς επιτρέπει να αποστείλετε ένα δίσκο εικονική

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**Επισύναψη εικονική δίσκου &lt;εικονική όνομα > &lt;όνομα δίσκου εικόνας >**

Αυτή η εντολή επισυνάπτει μια υπάρχουσα δίσκου στο χώρο αποθήκευσης αντικειμένων blob μια υπάρχουσα εικονική μηχανή αναπτυχθεί σε μια υπηρεσία στο cloud.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**εικονική δίσκου επισύναψη-νέα &lt;εικονική όνομα > &lt;μέγεθος-σε-gb > [blob-url]**

Αυτή η εντολή επισυνάπτει ένα δίσκο δεδομένων μια εικονική μηχανή Azure. Σε αυτό το παράδειγμα, 20 είναι το μέγεθος του νέου δίσκου, σε gigabyte, για να επισυναφθεί. Προαιρετικά, μπορείτε να χρησιμοποιήσετε μια διεύθυνση URL blob ως το όρισμα τελευταία για να καθορίσετε ρητά την blob προορισμού για να δημιουργήσετε. Εάν δεν καθορίσετε μια διεύθυνση URL blob, δημιουργείται αυτόματα ένα αντικείμενο blob.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**Απόσπαση εικονική δίσκου &lt;εικονική όνομα > &lt;lun >**

Αυτή η εντολή αποσυνδέει ένα δίσκο δεδομένων που έχουν επισυναφθεί σε μια εικονική μηχανή Azure. &lt;LUN > προσδιορίζει το δίσκο, για να αποσυνδεθεί. Για να λάβετε μια λίστα των δίσκων που σχετίζονται με ένα δίσκο, πριν να αποσυνδέσετε, χρησιμοποιήστε εικονική δίσκου-λίστα &lt;εικονική όνομα >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Εντολές για τη διαχείριση των υπηρεσιών Azure cloud σας

Υπηρεσίες Azure cloud είναι εφαρμογών και υπηρεσιών που φιλοξενούνται στο web ρόλους και εργασίας. Το παρακάτω εντολές μπορούν να χρησιμοποιηθούν για να διαχειριστείτε τις υπηρεσίες Azure cloud.

**υπηρεσία δημιουργία [επιλογές] &lt;όνομα_υπηρεσίας >**

Αυτή η εντολή δημιουργεί μια υπηρεσία στο cloud

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**υπηρεσία εμφάνιση [επιλογές] &lt;όνομα_υπηρεσίας >**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες της μια υπηρεσία Azure cloud

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**λίστα υπηρεσιών [επιλογές]**

Αυτή η εντολή παραθέτει τις υπηρεσίες Azure cloud.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**υπηρεσία διαγραφή [επιλογές] &lt;όνομα >**

Αυτή η εντολή διαγράφει μια υπηρεσία Azure cloud.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Για να επιβάλετε τη διαγραφή, χρησιμοποιήστε το `-q` παραμέτρου.


## <a name="commands-to-manage-your-azure-certificates"></a>Εντολές για τη διαχείριση πιστοποιητικών Azure

Τα πιστοποιητικά Azure service είναι συνδεδεμένο στο λογαριασμό σας Azure πιστοποιητικά SSL. Για περισσότερες πληροφορίες σχετικά με τα πιστοποιητικά Azure, ανατρέξτε στο θέμα [Διαχείριση πιστοποιητικών](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**λίστα υπηρεσιών πιστοποιητικού [επιλογές]**

Αυτή η εντολή παραθέτει Azure πιστοποιητικά.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Δημιουργία πιστοποιητικού υπηρεσίας &lt;πρόθεμα dns > &lt;αρχείο > [κωδικός πρόσβασης]**

Αυτή η εντολή αποστέλλει ένα πιστοποιητικό. Αφήστε κενό για τα πιστοποιητικά που δεν προστατεύεται με κωδικό πρόσβασης του κωδικού πρόσβασης.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Διαγραφή πιστοποιητικού υπηρεσίας [επιλογές] &lt;αποτύπωση >**

Αυτή η εντολή διαγράφει ένα πιστοποιητικό.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Εντολές για τη Διαχείριση εφαρμογών web της

Μια εφαρμογή Azure web είναι μια ρύθμιση web που είναι προσβάσιμη από URI. Εφαρμογές Web φιλοξενούνται σε εικονικές μηχανές Windows, αλλά δεν χρειάζεται να σκέφτεστε σχετικά με τις λεπτομέρειες της δημιουργίας και ανάπτυξης η εικονική μηχανή στον εαυτό σας. Οι λεπτομέρειες χειρίζεται για εσάς Azure.

**λίστα τοποθεσιών [επιλογές]**

Αυτή η εντολή παραθέτει τις εφαρμογές web.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**Ορισμός τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή ορίζει επιλογές ρύθμισης παραμέτρων για την εφαρμογή web της [όνομα]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**τοποθεσία deploymentscript [επιλογές]**

Αυτή η εντολή δημιουργεί μια δέσμη ενεργειών ανάπτυξης προσαρμοσμένο

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**τοποθεσία δημιουργία [επιλογές] [όνομα]**

Αυτή η εντολή δημιουργεί μια εφαρμογή web και τοπικού καταλόγου.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Το όνομα της τοποθεσίας πρέπει να είναι μοναδικό. Δεν μπορείτε να δημιουργήσετε μια τοποθεσία με το ίδιο όνομα DNS με μια υπάρχουσα τοποθεσία.

**Αναζήτηση τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή ανοίγει την εφαρμογή web της σε ένα πρόγραμμα περιήγησης.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**Εμφάνιση της τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή εμφανίζει λεπτομέρειες για μια εφαρμογή web.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**Διαγραφή τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή διαγράφει μια εφαρμογή web.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **τοποθεσία αντιμετάθεσης [επιλογές] [όνομα]**

Αυτή η εντολή αντιμεταθέτει δύο υποδοχές εφαρμογής web.

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

**- q ή **--χωρίς μηνύματα **: δεν γίνεται ερώτηση επιβεβαίωσης. Χρησιμοποιήστε αυτήν την επιλογή σε αυτοματοποιημένες δέσμες ενεργειών.


**τοποθεσία Έναρξη [επιλογές] [όνομα]**

Αυτή η εντολή ξεκινά μια εφαρμογή web.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**Διακοπή τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή διακόπτει μια εφαρμογή web.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**τοποθεσία επανεκκίνηση [επιλογές] [όνομα]**

Αυτή η εντολή διακόπτει και, στη συνέχεια, ξεκινά μια καθορισμένη τοποθεσία web app.

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

**--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.


**λίστα θέση τοποθεσίας [επιλογές]**

Αυτή η εντολή παραθέτει τις θέσεις web app.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Εντολές για να διαχειριστείτε τις ρυθμίσεις της εφαρμογής web app

**λίστα appsetting τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή παραθέτει τη ρύθμιση εφαρμογή που προσθέσατε στην εφαρμογή web.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**τοποθεσία appsetting Προσθήκη [επιλογές] &lt;keyvaluepair > [όνομα]**

Αυτή η εντολή προσθέτει μια ρύθμιση εφαρμογής σε εφαρμογή web ως ένα ζεύγος τιμή του κλειδιού.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**τοποθεσία appsetting διαγραφή [επιλογές] &lt;κλειδί > [όνομα]**

Αυτή η εντολή διαγράφει τη ρύθμιση καθορισμένη εφαρμογή από την εφαρμογή web.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**τοποθεσία appsetting εμφάνιση [επιλογές] &lt;κλειδί > [όνομα]**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες της ρύθμισης καθορισμένο εφαρμογής

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Εντολές για τη διαχείριση πιστοποιητικών εφαρμογής web

**λίστα πιστοποιητικού τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή εμφανίζει μια λίστα με τα πιστοποιητικά εφαρμογής web.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**τοποθεσία πιστοποιητικού Προσθήκη [επιλογές] &lt;πιστοποιητικό διαδρομή > [όνομα]**

**Διαγραφή τοποθεσίας πιστοποιητικού [επιλογές] &lt;αποτύπωση > [όνομα]**

**Εμφάνιση πιστοποιητικού [επιλογές] τοποθεσίας &lt;αποτύπωση > [όνομα]**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες του πιστοποιητικού

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Εντολές για να διαχειριστείτε τις συμβολοσειρές σύνδεσης εφαρμογής web

**λίστα συμβολοσειρά σύνδεσης τοποθεσίας [επιλογές] [όνομα]**

**συμβολοσειρά σύνδεσης τοποθεσίας Προσθήκη [επιλογές] &lt;όνομα_σύνδεσης > &lt;τιμή > &lt;τύπος > [όνομα]**

**συμβολοσειρά σύνδεσης τοποθεσίας διαγραφή [επιλογές] &lt;όνομα_σύνδεσης > [όνομα]**

**συμβολοσειρά σύνδεσης τοποθεσίας εμφάνιση [επιλογές] &lt;όνομα_σύνδεσης > [όνομα]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Εντολές για τη διαχείριση των εγγράφων προεπιλεγμένη εφαρμογή web

**λίστα defaultdocument τοποθεσίας [επιλογές] [όνομα]**

**τοποθεσία defaultdocument Προσθήκη [επιλογές] &lt;έγγραφο > [όνομα]**

**τοποθεσία defaultdocument διαγραφή [επιλογές] &lt;έγγραφο > [όνομα]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Εντολές για τη διαχείριση του αναπτύξεων εφαρμογών web

**λίστα ανάπτυξη τοποθεσίας [επιλογές] [όνομα]**

**ανάπτυξη της τοποθεσίας εμφάνιση [επιλογές] &lt;commitId > [όνομα]**

**Επανάληψη ανάπτυξης ανάπτυξης τοποθεσίας [επιλογές] &lt;commitId > [όνομα]**

**τοποθεσία ανάπτυξης github [επιλογές] [όνομα]**

**σύνολο χρηστών τοποθεσίας ανάπτυξης [επιλογές] [όνομα χρήστη] [φάση]**

###<a name="commands-to-manage-your-web-app-domains"></a>Εντολές για τη Διαχείριση τομέων εφαρμογής web

**λίστα τομέων τοποθεσίας [επιλογές] [όνομα]**

**τοποθεσία τομέα Προσθήκη [επιλογές] &lt;dn > [όνομα]**

**Διαγραφή τοποθεσίας τομέα [επιλογές] &lt;dn > [όνομα]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Εντολές για να διαχειριστείτε τις αντιστοιχίσεις πρόγραμμα χειρισμού εφαρμογής web

**λίστα χειρισμού τοποθεσίας [επιλογές] [όνομα]**

**πρόγραμμα χειρισμού τοποθεσία Προσθήκη [επιλογές] &lt;επέκταση > &lt;επεξεργαστής > [όνομα]**

**Διαγραφή τοποθεσίας πρόγραμμα χειρισμού [επιλογές] &lt;επέκταση > [όνομα]**

###<a name="commands-to-manage-your-web-jobs"></a>Εντολές για τη διαχείριση των εργασιών σας Web

**λίστα εργασιών τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή λίστα όλων των εργασιών web κάτω από μια εφαρμογή web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--Τύπος έργου** &lt;τύπος έργου >: προαιρετική. Ο τύπος του webjob. Έγκυρη τιμή είναι "που ενεργοποιούνται από" ή "συνεχόμενη". Από προεπιλογή επιστρέφουν webjobs όλων των τύπων.
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

**τοποθεσία έργου εμφάνιση [επιλογές] &lt;όνομα_εργασίας > &lt;jobType > [όνομα]**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες μιας εργασίας συγκεκριμένες web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας >: υποχρεωτικό. Το όνομα του το webjob.
+ **--Τύπος έργου** &lt;τύπος έργου >: υποχρεωτικό. Ο τύπος του webjob. Έγκυρη τιμή είναι "που ενεργοποιούνται από" ή "συνεχόμενη".
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

**Διαγραφή τοποθεσίας έργου [επιλογές] &lt;όνομα_εργασίας > &lt;jobType > [όνομα]**

Αυτή η εντολή διαγράφει την εργασία καθορισμένη τοποθεσία web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας > απαιτείται. Το όνομα του το webjob.
+ **--Τύπος έργου** &lt;τύπος έργου > απαιτείται. Ο τύπος του webjob. Έγκυρη τιμή είναι "που ενεργοποιούνται από" ή "συνεχόμενη".
+ **-q** ή **--χωρίς μηνύματα**: δεν γίνεται ερώτηση επιβεβαίωσης. Χρησιμοποιήστε αυτήν την επιλογή σε αυτοματοποιημένες δέσμες ενεργειών.
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

**τοποθεσία έργου αποστολής [επιλογές] &lt;όνομα_εργασίας > &lt;jobType > <jobFile> [όνομα]**

Αυτή η εντολή διαγράφει την εργασία καθορισμένη τοποθεσία web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας >: υποχρεωτικό. Το όνομα του το webjob.
+ **--Τύπος έργου** &lt;τύπος έργου >: υποχρεωτικό. Ο τύπος του webjob. Έγκυρη τιμή είναι "που ενεργοποιούνται από" ή "συνεχόμενη".
+ **--αρχείο εργασίας** &lt;αρχείο εργασίας >: υποχρεωτικό. Το αρχείο έργου.
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

**τοποθεσία έργου Έναρξη [επιλογές] &lt;όνομα_εργασίας > &lt;jobType > [όνομα]**

Αυτή η εντολή ξεκινά η εργασία καθορισμένη τοποθεσία web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας >: υποχρεωτικό. Το όνομα του το webjob.
+ **--Τύπος έργου** &lt;τύπος έργου >: υποχρεωτικό. Ο τύπος του webjob. Έγκυρη τιμή είναι "που ενεργοποιούνται από" ή "συνεχόμενη".
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

**Διακοπή εργασίας τοποθεσίας [επιλογές] &lt;όνομα_εργασίας > &lt;jobType > [όνομα]**

Αυτή η εντολή διακόπτει την εργασία καθορισμένη τοποθεσία web. Μπορεί να διακοπεί μόνο συνεχής έργα.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας >: υποχρεωτικό. Το όνομα του το webjob.
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

###<a name="commands-to-manage-your-web-jobs-history"></a>Εντολές για τη διαχείριση του ιστορικού των εργασιών σας Web

**λίστα ιστορικού εργασίας τοποθεσίας [επιλογές] [όνομα_εργασίας] [όνομα]**

Η εντολή αυτή εμφανίζει ένα ιστορικό των την εκτέλεση της εργασίας καθορισμένη τοποθεσία web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας >: υποχρεωτικό. Το όνομα του το webjob.
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

**Προβολή ιστορικού εργασίας τοποθεσίας [επιλογές] [όνομα_εργασίας] [runId] [όνομα]**

Αυτή η εντολή λαμβάνει τις λεπτομέρειες της εργασίας που εκτελείται για το έργο καθορισμένη τοποθεσία web.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--όνομα εργασίας** &lt;όνομα εργασίας >: υποχρεωτικό. Το όνομα του το webjob.
+ **--Εκτέλεση αναγνωριστικού** &lt;αναγνωριστικού εκτέλεση >: προαιρετική. Το αναγνωριστικό του ιστορικού εκτέλεση. Εάν δεν καθορίζονται, εμφάνιση των πιο πρόσφατων εκτέλεση.
+ **--υποδιαίρεση** &lt;υποδιαίρεση >: το όνομα της εσοχής για να ξεκινήσετε ξανά.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Εντολές για τη διαχείριση του Διαγνωστικά εφαρμογής web

**Κάντε λήψη του αρχείου καταγραφής της τοποθεσίας [επιλογές] [όνομα]**

Κάντε λήψη του αρχείου .zip που περιέχει τα Διαγνωστικά της εφαρμογής σας web.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**πυκνότητα καταγραφής τοποθεσίας [επιλογές] [όνομα]**

Αυτή η εντολή συνδέεται τερματικού σας με την υπηρεσία καταγραφής ροής.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**αρχείο καταγραφής τοποθεσίας ρύθμιση [επιλογές] [όνομα]**

Αυτή η εντολή ρυθμίζει τις παραμέτρους των διαγνωστικών επιλογών για την εφαρμογή web.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Εντολές για τη διαχείριση του αποθετήρια εφαρμογής web

**τοποθεσία αποθετήριο κλάδο [επιλογές] &lt;κλάδο > [όνομα]**

**Διαγραφή τοποθεσίας αποθετήριο [επιλογές] [όνομα]**

**Συγχρονισμός αποθετήριο τοποθεσίας [επιλογές] [όνομα]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Εντολές για τη διαχείριση του κλίμακας εφαρμογής web

**λειτουργία κλίμακα τοποθεσίας [επιλογές] &lt;λειτουργία > [όνομα]**

**τοποθεσία κλίμακα παρουσίες [επιλογές] &lt;παρουσίες > [όνομα]**


## <a name="commands-to-manage-azure-mobile-services"></a>Εντολές για τη διαχείριση των υπηρεσιών Azure Mobile

Azure υπηρεσίες Mobile συγκεντρώνει ένα σύνολο Azure υπηρεσιών με δυνατότητα υποστήριξης δυνατοτήτων για τις εφαρμογές σας. Εντολές υπηρεσιών Mobile χωρίζονται στις ακόλουθες κατηγορίες:

+ [Εντολές για τη Διαχείριση παρουσίες υπηρεσία κινητών τηλεφώνων](#Mobile_Services)
+ [Εντολές για τη διαχείριση της υπηρεσίας κινητών τηλεφώνων ρύθμισης παραμέτρων](#Mobile_Configuration)
+ [Εντολές για τη Διαχείριση πινάκων υπηρεσία κινητών τηλεφώνων](#Mobile_Tables)
+ [Εντολές για τη διαχείριση της υπηρεσίας κινητών τηλεφώνων δέσμες ενεργειών](#Mobile_Scripts)
+ [Εντολές για τη Διαχείριση προγραμματισμένες εργασίες](#Mobile_Jobs)
+ [Εντολές για να κλιμακωθεί μια υπηρεσία κινητών τηλεφώνων](#Mobile_Scale)

Τις ακόλουθες επιλογές ισχύουν για περισσότερες εντολές Mobile υπηρεσίες:

+ **-h** ή **--Βοήθεια**: εμφάνιση εξόδου πληροφορίες χρήσης.
+ **-s `<id>` ** ή **--συνδρομή `<id>` **: Χρησιμοποιήστε μια συγκεκριμένη συνδρομή, που έχουν καθοριστεί ως `<id>`.
+ **-v** ή **--λεπτομερής**: σύνταξη λεπτομερή δεδομένα εξόδου.
+ **--json**: σύνταξη JSON εξόδου.

### <a name="Mobile_Services"></a>Εντολές για τη Διαχείριση παρουσίες υπηρεσία κινητών τηλεφώνων

**κινητές συσκευές θέσεις [επιλογές]**

Αυτή η εντολή παραθέτει διαφορετικές γεωγραφικές θέσεις που υποστηρίζονται από τις υπηρεσίες Mobile.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobile-Δημιουργία [επιλογές] [όνομα_υπηρεσίας] [sqlAdminUsername] [sqlAdminPassword]**

Αυτή η εντολή δημιουργεί μια κινητή υπηρεσία μαζί με μια βάση δεδομένων SQL και το διακομιστή.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **- r `<sqlServer>` ** ή **--της `<sqlServer>` **: χρήση ενός υπάρχοντος διακομιστή βάσης δεδομένων SQL, που έχουν καθοριστεί ως `<sqlServer>`.
+ **-d `<sqlDb>` ** ή **--sqlDb `<sqlDb>` **: Χρήση υπάρχουσας βάσης δεδομένων SQL, που έχουν καθοριστεί ως `<sqlDb>`.
+ **-l `<location>` ** ή **--θέση `<location>` **: δημιουργία της υπηρεσίας σε μια συγκεκριμένη θέση, που έχουν καθοριστεί ως `<location>`. Εκτελέστε το azure θέσεις κινητές συσκευές για να λάβετε διαθέσιμες θέσεις.
+ **--sqlLocation `<location>` **: δημιουργία του SQL server σε ένα συγκεκριμένο `<location>`; η προεπιλογή είναι στη θέση της υπηρεσίας κινητές συσκευές.

**κινητές συσκευές διαγραφή [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή διαγράφει μια κινητή υπηρεσία μαζί με τη βάση δεδομένων SQL και το διακομιστή.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-δ** ή **--deleteData**: Διαγραφή όλων των δεδομένων από αυτήν την υπηρεσία κινητών από τη βάση δεδομένων.
+ **-μια** ή **--deleteAll**: διαγραφή βάσης δεδομένων SQL και διακομιστή.
+ **-q** ή **--χωρίς μηνύματα**: δεν γίνεται ερώτηση επιβεβαίωσης. Χρησιμοποιήστε αυτήν την επιλογή σε αυτοματοποιημένες δέσμες ενεργειών.

**κινητές συσκευές λίστα [επιλογές]**

Αυτή η εντολή παραθέτει τις υπηρεσίες κινητές συσκευές σας.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**κινητές συσκευές εμφάνιση [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή εμφανίζει λεπτομέρειες σχετικά με μια υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**κινητές συσκευές επανεκκίνηση [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή επανεκκίνηση μια παρουσία υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**κινητές συσκευές καταγραφής [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή επιστρέφει αρχεία καταγραφής υπηρεσία κινητών τηλεφώνων, με φιλτράρισμα όλων των τύπων αρχείου καταγραφής, αλλά `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **- r `<query>` ** ή **--ερωτήματος `<query>` **: εκτελεί το καθορισμένο αρχείο καταγραφής ερώτημα.
+ **-t `<type>` ** ή **--Τύπος `<type>` **: φιλτράρισμα των αρχείων καταγραφής που επιστρέφεται από εγγραφή `<type>`, που μπορεί να είναι `information`, `warning`, ή `error`.
+ **-k `<skip>` ** ή **--παραλείψετε `<skip>` **: Παραλείπει τον αριθμό των γραμμών που καθορίζεται από `<skip>`.
+ **-p `<top>` ** ή **--επάνω `<top>` **: επιστρέφει ένα συγκεκριμένο πλήθος γραμμών, που καθορίζεται από `<top>`.

> [AZURE.NOTE] Το **--ερωτήματος** παραμέτρου θα υπερισχύει **--τύπο**, **--παραλείψετε**, και **--επάνω**.

**κινητές συσκευές ανάκτηση [επιλογές] [unhealthyservicename] [healthyservicename]**

Αυτή η εντολή ανακτά μια κατεστραμμένη κινητή υπηρεσία μετακινώντας το σε καλή κατάσταση υπηρεσίας κινητές συσκευές σε διαφορετική περιοχή.

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

**-q** ή **--χωρίς μηνύματα**: απόκρυψη στην ερώτηση επιβεβαίωσης της αποκατάστασης.

**πλήκτρο Mobile αναδημιουργήσετε [επιλογές] [όνομα_υπηρεσίας] [τύπος]**

Αυτή η εντολή δημιουργεί ξανά το πλήκτρο εφαρμογής υπηρεσίας κινητών τηλεφώνων.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Βασικοί τύποι είναι `master` και `application`.

> [AZURE.NOTE] Όταν αναδημιουργήσετε πλήκτρα, ίσως δεν είναι δυνατή η πρόσβαση στην κινητή υπηρεσία προγράμματα-πελάτες που χρησιμοποιούν το παλιό κλειδί. Όταν θα ξαναδημιουργήσετε το πλήκτρο εφαρμογής, θα πρέπει να μπορείτε να ενημερώσετε την εφαρμογή σας με τη νέα τιμή κλειδιού.

**πλήκτρο Mobile ρύθμιση [επιλογές] [όνομα_υπηρεσίας] [τύπος] [τιμή]**

Αυτή η εντολή ορίζει το πλήκτρο υπηρεσία κινητών τηλεφώνων με μια συγκεκριμένη τιμή.


### <a name="Mobile_Configuration"></a>Εντολές για τη διαχείριση της υπηρεσίας κινητών τηλεφώνων ρύθμισης παραμέτρων

**λίστα κινητή ρύθμισης παραμέτρων [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή παραθέτει επιλογές ρύθμισης παραμέτρων για μια υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**κινητές συσκευές config γρήγορα [επιλογές] [όνομα_υπηρεσίας] [πλήκτρο]**

Αυτή η εντολή λαμβάνει μια επιλογή συγκεκριμένη ρύθμιση παραμέτρων για μια κινητή υπηρεσία, σε αυτήν την περίπτωση δυναμικής σχήματος.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**ρύθμιση κινητών config [επιλογές] [όνομα_υπηρεσίας] [πλήκτρο] [τιμή]**

Αυτή η εντολή ορίζει μια επιλογή συγκεκριμένη ρύθμιση παραμέτρων για μια κινητή υπηρεσία, σε αυτήν την περίπτωση δυναμικής σχήματος.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Εντολές για τη Διαχείριση πινάκων υπηρεσία κινητών τηλεφώνων

**λίστα πίνακα κινητή [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή παραθέτει όλους τους πίνακες στην υπηρεσία κινητές συσκευές.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**Εμφάνιση με κίνηση-Πίνακας [επιλογές] [όνομα_υπηρεσίας] [όνομα πίνακα]**

Αυτή η εντολή εμφανίζει επιστρέφει λεπτομέρειες σχετικά με ένα συγκεκριμένο πίνακα.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**κινητές συσκευές πίνακα Δημιουργία [επιλογές] [όνομα_υπηρεσίας] [όνομα πίνακα]**

Αυτή η εντολή δημιουργεί έναν πίνακα.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

+ **-p `&lt;permissions>` ** ή **--δικαιώματα `&lt;permissions>` **: λίστα κόμματα `<operation>` = `<permission>` ζεύγη, όπου `<operation>` είναι `insert`, `read`, `update`, ή `delete` και `&lt;permissions>` είναι `public`, `application` (προεπιλογή), `user`, ή `admin`.

**ανάγνωση κινητή δεδομένων [επιλογές] [όνομα_υπηρεσίας] [όνομα πίνακα] [query]**

Αυτή η εντολή διαβάζει δεδομένα από έναν πίνακα.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-k `<skip>` ** ή **--παραλείψετε `<skip>` **: Παραλείπει τον αριθμό των γραμμών που καθορίζεται από `<skip>`.
+ **-t `<top>` ** ή **--επάνω `<top>` **: επιστρέφει ένα συγκεκριμένο πλήθος γραμμών, που καθορίζεται από `<top>`.
+ **-l** ή **--λίστα**: επιστρέφει δεδομένα σε μορφή λίστας.

**Ενημέρωση πίνακα κινητή [επιλογές] [όνομα_υπηρεσίας] [όνομα πίνακα]**

Αυτή η εντολή αλλάζει διαγράψει τα δικαιώματα σε έναν πίνακα σε μόνο για διαχειριστές.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-p `&lt;permissions>` ** ή **--δικαιώματα `&lt;permissions>` **: λίστα κόμματα `<operation>` = `<permission>` ζεύγη, όπου `<operation>` είναι `insert`, `read`, `update`, ή `delete` και `&lt;permissions>` είναι `public`, `application` (προεπιλογή), `user`, ή `admin`.
+ **--deleteColumn `<columns>` **: διαχωρισμένο με κόμματα λίστα των στηλών για να διαγράψετε, ως `<columns>`.
+ **-q** ή **--χωρίς μηνύματα**: Διαγράφει στήλες χωρίς ερώτηση επιβεβαίωσης.
+ **--addIndex `<columns>` **: διαχωρισμένο με κόμματα λίστα των στηλών για να συμπεριλάβετε στο ευρετήριο.
+ **--deleteIndex `<columns>` **: διαχωρισμένο με κόμματα λίστα των στηλών για να αποκλείσετε από το ευρετήριο.

**κινητές συσκευές πίνακα Διαγραφή [επιλογές] [όνομα_υπηρεσίας] [όνομα πίνακα]**

Αυτή η εντολή διαγράφει έναν πίνακα.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Καθορίστε την παράμετρο - q για να διαγράψετε τον πίνακα χωρίς επιβεβαίωση. Κάντε τα εξής για να αποτρέψετε την αποκλεισμό των δεσμών ενεργειών αυτοματοποίησης.

**κινητές συσκευές δεδομένων περικοπή [επιλογές] [όνομα_υπηρεσίας] [όνομα πίνακα]**

Αυτή η εντολή καταργεί όλες τις γραμμές των δεδομένων από τον πίνακα.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Εντολές για τη Διαχείριση δεσμών ενεργειών

Εντολές σε αυτήν την ενότητα που χρησιμοποιούνται για τη διαχείριση των δεσμών ενεργειών του διακομιστή που ανήκουν σε μια κινητή υπηρεσία. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με δέσμες ενεργειών διακομιστή σε υπηρεσίες Mobile](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**λίστα κινητές συσκευές δέσμης ενεργειών [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή παραθέτει καταχωρημένες δέσμες ενεργειών, συμπεριλαμβανομένων των δεσμών ενεργειών πίνακα και χρονοδιάγραμμα.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**κινητές συσκευές λήψη της δέσμης ενεργειών [επιλογές] [όνομα_υπηρεσίας] [όνομα_δέσμης_ενεργειών]**

Αυτή η εντολή κάνει λήψη της δέσμης ενεργειών εισαγωγή από τον πίνακα TodoItem σε ένα αρχείο με το όνομα `todoitem.insert.js` στο το `table` υποφάκελο.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-p `<path>` ** ή **--διαδρομή `<path>` **: Η θέση του αρχείου στην οποία θέλετε να αποθηκεύσετε τη δέσμη ενεργειών, όπου ο τρέχων κατάλογος εργασίας είναι η προεπιλεγμένη.
+ **-f `<file>` ** ή **--αρχείο `<file>` **: το όνομα του αρχείου στην οποία θέλετε να αποθηκεύσετε τη δέσμη ενεργειών.
+ **-o** ή **--Παράκαμψη**: αντικατάσταση ενός υπάρχοντος αρχείου.
+ **-c** ή **--κονσόλας**: συντάξετε τη δέσμη ενεργειών στην κονσόλα αντί σε ένα αρχείο.

**κινητές συσκευές δέσμης ενεργειών αποστολής [επιλογές] [όνομα_υπηρεσίας] [όνομα_δέσμης_ενεργειών]**

Αυτή η εντολή αποστέλλει μια δέσμη ενεργειών με το όνομα `todoitem.insert.js` από το `table` υποφάκελο.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Το όνομα του αρχείου πρέπει να αποτελείται από τα ονόματα πινάκων και λειτουργία. Πρέπει να βρίσκεται στον πίνακα υποφάκελο σε σχέση με τη θέση όπου εκτελείται η εντολή. Μπορείτε επίσης να χρησιμοποιήσετε το **-f `<file>` ** ή **--αρχείο `<file>` ** παραμέτρου για να καθορίσετε ένα διαφορετικό όνομα αρχείου και μια διαδρομή προς το αρχείο που περιέχει τη δέσμη ενεργειών για την καταχώρηση.


**Διαγραφή κινητές συσκευές δέσμης ενεργειών [επιλογές] [όνομα_υπηρεσίας] [όνομα_δέσμης_ενεργειών]**

Αυτή η εντολή καταργεί την υπάρχουσα δέσμη ενεργειών εισαγωγή από τον πίνακα TodoItem.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Εντολές για τη Διαχείριση προγραμματισμένες εργασίες

Εντολές σε αυτήν την ενότητα που χρησιμοποιούνται για τη Διαχείριση προγραμματισμένες εργασίες που ανήκουν σε μια κινητή υπηρεσία. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προγραμματισμός εργασιών](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**λίστα κινητή έργου [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή παραθέτει προγραμματισμένες εργασίες.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**εργασία κινητή δημιουργήσει [επιλογές] [όνομα_υπηρεσίας] [όνομα_εργασίας]**

Αυτή η εντολή δημιουργεί μια εργασία με όνομα `getUpdates` που έχει προγραμματιστεί να εκτελεστεί ανά ώρα.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-i `<number>` ** ή **--διάστημα `<number>` **: το χρονικό διάστημα της εργασίας, μορφή ακέραιου αριθμού. Η προεπιλεγμένη τιμή είναι `15`.
+ **-u `<unit>` ** ή **--intervalUnit `<unit>` **: Η μονάδα για το _χρονικό διάστημα_, που μπορεί να είναι μία από τις παρακάτω τιμές:
    + **λεπτό** (προεπιλογή)
    + **ώρα**
    + **ημέρα**
    + **μήνα**
    + **κανένας** (στη ζήτηση εργασίες)
+ **-t `<time>`** **--ώρα έναρξης `<time>` ** Εκτελέστε την ώρα έναρξης της πρώτης για τη δέσμη ενεργειών, σε μορφή ISO. Η προεπιλεγμένη τιμή είναι `now`.

> [AZURE.NOTE] Νέες εργασίες δημιουργούνται σε κατάσταση απενεργοποίησης, επειδή εξακολουθεί να πρέπει να αποσταλεί μια δέσμη ενεργειών. Χρησιμοποιήστε την εντολή **Αποστολή κινητή δέσμη ενεργειών** για την αποστολή μιας δέσμης ενεργειών και την εντολή **Ενημέρωση έργου κινητές συσκευές** για να ενεργοποιήσετε την εργασία.

**Ενημέρωση κινητή εργασία [επιλογές] [όνομα_υπηρεσίας] [όνομα_εργασίας]**

Την ακόλουθη εντολή επιτρέπει την απενεργοποίησης `getUpdates` εργασία.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-i `<number>` ** ή **--διάστημα `<number>` **: το χρονικό διάστημα της εργασίας, μορφή ακέραιου αριθμού. Η προεπιλεγμένη τιμή είναι `15`.
+ **-u `<unit>` ** ή **--intervalUnit `<unit>` **: Η μονάδα για το _χρονικό διάστημα_, που μπορεί να είναι μία από τις παρακάτω τιμές:
    + **λεπτό** (προεπιλογή)
    + **ώρα**
    + **ημέρα**
    + **μήνα**
    + **κανένας** (στη ζήτηση εργασίες)
+ **-t `<time>`** **--ώρα έναρξης `<time>` ** Εκτελέστε την ώρα έναρξης της πρώτης για τη δέσμη ενεργειών, σε μορφή ISO. Η προεπιλεγμένη τιμή είναι `now`.
+ **- `<status>` ** ή **--κατάσταση `<status>` **: την κατάσταση της εργασίας, το οποίο μπορεί να είναι είτε `enabled` ή `disabled`.

**Διαγραφή κινητή εργασίας [επιλογές] [όνομα_υπηρεσίας] [όνομα_εργασίας]**

Αυτή η εντολή καταργεί την προγραμματισμένη εργασία getUpdates από το διακομιστή TodoList.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Διαγραφή μιας εργασίας διαγράφει επίσης τη δέσμη ενεργειών που έχουν αποσταλεί.

### <a name="Mobile_Scale"></a>Εντολές για να κλιμακωθεί μια υπηρεσία κινητών τηλεφώνων

Εντολές σε αυτήν την ενότητα που χρησιμοποιούνται για να κλιμακωθεί μια υπηρεσία κινητών τηλεφώνων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κλίμακας μια υπηρεσία κινητών τηλεφώνων](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**κινητές συσκευές κλίμακα εμφάνιση [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή εμφανίζει πληροφορίες κλίμακας, όπως την τρέχουσα κατάσταση λειτουργίας υπολογισμού και τον αριθμό των παρουσιών.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**κινητές συσκευές κλίμακα αλλαγή [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή αλλάζει την κλίμακα της υπηρεσίας κινητών από δωρεάν λειτουργία premium.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **- c `<mode>` ** ή **--computeMode `<mode>` **: στη λειτουργία υπολογισμού πρέπει να είναι είτε `Free` ή `Reserved`.
+ **-i `<count>` ** ή **--numberOfInstances `<count>` **: Ο αριθμός των παρουσιών που χρησιμοποιήθηκε κατά την εκτέλεση της λειτουργίας δεσμευμένη.

> [AZURE.NOTE] Όταν ορίζετε την κατάσταση λειτουργίας υπολογισμού για να `Reserved`, όλων των κινητών υπηρεσιών στην ίδια περιοχή εκτελείται σε λειτουργία premium.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Εντολές για να ενεργοποιήσετε δυνατότητες προεπισκόπηση της υπηρεσίας κινητών

**λίστα κινητή προεπισκόπηση [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή εμφανίζει τις διαθέσιμες δυνατότητες προεπισκόπηση της συγκεκριμένης υπηρεσίας και αν είναι ενεργοποιημένες.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**κινητές συσκευές προεπισκόπηση Ενεργοποίηση [επιλογές] [όνομα_υπηρεσίας] [όνομα δυνατότητας]**

Αυτή η εντολή ενεργοποιεί τη δυνατότητα καθορισμένο προεπισκόπησης για μια υπηρεσία κινητών τηλεφώνων. Μόλις ενεργοποιηθεί, δυνατότητες preview δεν μπορεί να απενεργοποιηθεί για μια υπηρεσία κινητών τηλεφώνων.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Εντολές για τη διαχείριση της υπηρεσίας κινητών APIs

**κινητές συσκευές api λίστα [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή εμφανίζει μια λίστα με κινητή υπηρεσίας προσαρμοσμένο API που έχετε δημιουργήσει για την υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**κινητές συσκευές api δημιουργία [επιλογές] [όνομα_υπηρεσίας] [apiname]**

Δημιουργεί ένα προσαρμοσμένο API υπηρεσία κινητών τηλεφώνων

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

**-p** ή **--δικαιώματα** &lt;δικαιώματα >: μια λίστα με κόμματα &lt;μέθοδο > =&lt;δικαιωμάτων > ζεύγη.

**κινητές συσκευές api ενημέρωση [επιλογές] [όνομα_υπηρεσίας] [apiname]**

Αυτή η εντολή ενημερώνει το API προσαρμοσμένο καθορισμένη υπηρεσία κινητών τηλεφώνων.

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-p** ή **--δικαιώματα** &lt;δικαιώματα >: μια λίστα με κόμματα &lt;μέθοδο > =&lt;δικαιωμάτων > ζεύγη.
+ **-f** ή **--επιβάλετε**: αντικαθιστά τις προσαρμοσμένες αλλαγές στο αρχείο μετα-δεδομένων δικαιώματα.

**κινητές συσκευές api διαγραφή [επιλογές] [όνομα_υπηρεσίας] [apiname]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Αυτή η εντολή διαγράφει το API προσαρμοσμένο καθορισμένη υπηρεσία κινητών τηλεφώνων.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Εντολές για τη διαχείριση των ρυθμίσεων εφαρμογής κινητής εφαρμογής

**λίστα κινητή appsetting [επιλογές] [όνομα_υπηρεσίας]**

Αυτή η εντολή εμφανίζει τις ρυθμίσεις εφαρμογής κινητής εφαρμογής της συγκεκριμένης υπηρεσίας.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**κινητές συσκευές appsetting Προσθήκη [επιλογές] [όνομα_υπηρεσίας] [όνομα] [τιμή]**

Αυτή η εντολή προσθέτει μια προσαρμοσμένη εφαρμογή ρύθμισης για την υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**Διαγραφή κινητή appsetting [επιλογές] [όνομα_υπηρεσίας] [όνομα]**

Αυτή η εντολή καταργεί τη ρύθμιση καθορισμένη εφαρμογή για την υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**Εμφάνιση κινητή appsetting [επιλογές] [όνομα_υπηρεσίας] [όνομα]**

Αυτή η εντολή καταργεί τη ρύθμιση καθορισμένη εφαρμογή για την υπηρεσία κινητών τηλεφώνων.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Διαχείριση εργαλείο τοπικές ρυθμίσεις

Τοπικές ρυθμίσεις είναι το Αναγνωριστικό συνδρομής και το προεπιλεγμένο όνομα λογαριασμού χώρου αποθήκευσης.

**λίστα ρύθμισης παραμέτρων [επιλογές]**

Αυτή η εντολή εμφανίζει τις ρυθμίσεις παραμέτρων.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**[επιλογές] συνόλου παραμέτρων &lt;όνομα&gt;,&lt;τιμή&gt;**

Αυτή η εντολή αλλάζει μια ρύθμιση παραμέτρων.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Εντολές για τη Διαχείριση υπηρεσίας Bus

Χρησιμοποιήστε αυτές τις εντολές για τη διαχείριση του λογαριασμού υπηρεσίας Bus

**χώρος ονομάτων μικρές ελέγχου [επιλογές] &lt;όνομα >**

Ελέγξτε ότι ένα χώρο ονομάτων bus υπηρεσίας είναι νόμιμο και διαθέσιμη.

**Δημιουργία χώρου ονομάτων μικρές &lt;όνομα > &lt;θέση >**

Δημιουργεί ένα πεδίο ονομάτων Bus υπηρεσίας.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**Διαγραφή χώρου ονομάτων μικρές &lt;όνομα >**

Καταργήστε ένα χώρο ονομάτων.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**λίστα μικρές χώρου ονομάτων**

Λίστα όλοι οι χώροι ονομάτων που έχει δημιουργηθεί για το λογαριασμό σας.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**λίστα θέση μικρές χώρου ονομάτων**

Εμφανίζεται μια λίστα με όλες τις θέσεις διαθέσιμο χώρο ονομάτων.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**Εμφάνιση ονομάτων μικρές &lt;όνομα >**

Εμφάνιση λεπτομερειών σχετικά με ένα συγκεκριμένο πεδίο ονομάτων.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**επαλήθευση του χώρου ονομάτων μικρές &lt;όνομα >**

Ελέγξτε αν ο χώρος ονομάτων είναι διαθέσιμη.

## <a name="commands-to-manage-your-storage-objects"></a>Εντολές για τη διαχείριση του χώρου αποθήκευσης αντικειμένων

###<a name="commands-to-manage-your-storage-accounts"></a>Εντολές για να διαχειριστείτε τους λογαριασμούς σας στο χώρο αποθήκευσης

**λίστα λογαριασμών χώρου αποθήκευσης [επιλογές]**

Αυτή η εντολή εμφανίζει τους λογαριασμούς χώρου αποθήκευσης στη συνδρομή σας.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**Εμφάνιση λογαριασμού χώρου αποθήκευσης [επιλογές]<name>**

Αυτή η εντολή εμφανίζει πληροφορίες σχετικά με το λογαριασμό καθορισμένο χώρου αποθήκευσης, συμπεριλαμβανομένων των ιδιοτήτων URI και του λογαριασμού.

**[επιλογές] Δημιουργία λογαριασμού χώρου αποθήκευσης<name>**

Αυτή η εντολή δημιουργεί ένα λογαριασμό του χώρου αποθήκευσης με βάση τις επιλογές που παρέχονται.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-e** ή **--ετικέτα** &lt;ετικέτα >: την ετικέτα για το λογαριασμό χώρου αποθήκευσης.
+ **-d** ή **--Περιγραφή** &lt;περιγραφή >: το λογαριασμό χώρου αποθήκευσης περιγραφή.
+ **-l** ή **--θέση** &lt;όνομα >: γεωγραφική περιοχή στην οποία θέλετε να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης.
+ **-a** ή **--συσχέτισης ομάδα** &lt;όνομα >: ομάδα συσχέτισης με το οποίο θέλετε να συσχετίσετε το λογαριασμό χώρου αποθήκευσης. 
+ **--Τύπος**: υποδεικνύει τον τύπο του λογαριασμού για να δημιουργήσετε: είτε τυπική χώρου αποθήκευσης με την επιλογή πλεονασμού (LRS/ZRS/Εξοπλισμό/RAGRS) ή Premium χώρου αποθήκευσης (PLRS).

**ρύθμιση λογαριασμού χώρου αποθήκευσης [επιλογές]<name>**

Αυτή η εντολή ενημερώνει το λογαριασμό καθορισμένο χώρου αποθήκευσης.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-e** ή **--ετικέτα** &lt;ετικέτα >: την ετικέτα για το λογαριασμό χώρου αποθήκευσης.
+ **-d** ή **--Περιγραφή** &lt;περιγραφή >: το λογαριασμό χώρου αποθήκευσης περιγραφή.
+ **-l** ή **--θέση** &lt;όνομα >: γεωγραφική περιοχή στην οποία θέλετε να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης.
+ **--Τύπος**: υποδεικνύει τον νέο τύπο λογαριασμού: είτε τυπική χώρου αποθήκευσης με την επιλογή πλεονασμού (LRS/ZRS/Εξοπλισμό/RAGRS) ή Premium χώρου αποθήκευσης (PLRS).

**διαγραφή λογαριασμού χώρου αποθήκευσης [επιλογές]<name>**

Αυτή η εντολή διαγράφει το λογαριασμό καθορισμένο χώρου αποθήκευσης.

Αυτή η εντολή υποστηρίζει την ακόλουθη επιπλέον επιλογή:

**-q** ή **--χωρίς μηνύματα**: δεν γίνεται ερώτηση επιβεβαίωσης. Χρησιμοποιήστε αυτήν την επιλογή σε αυτοματοποιημένες δέσμες ενεργειών.

###<a name="commands-to-manage-your-storage-account-keys"></a>Εντολές για τη διαχείριση των αριθμών-κλειδιών λογαριασμού χώρου αποθήκευσης

**Πλήκτρα για το λογαριασμό χώρου αποθήκευσης λίστας [επιλογές]<name>**

Αυτή η εντολή παραθέτει τα πλήκτρα κύριας και δευτερεύουσας για το χώρο αποθήκευσης στο συγκεκριμένο λογαριασμό.

**Πλήκτρα για το λογαριασμό χώρου αποθήκευσης ανανέωση [επιλογές]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Εντολές για τη διαχείριση του χώρου αποθήκευσης

**λίστα κοντέινερ χώρου αποθήκευσης [επιλογές] [πρόθεμα]**

Αυτή η εντολή εμφανίζει τη λίστα κοντέινερ χώρου αποθήκευσης για ένα λογαριασμό του καθορισμένου χώρου αποθήκευσης. Το λογαριασμό χώρου αποθήκευσης είναι που καθορίζεται από τη συμβολοσειρά σύνδεσης ή το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **-p** ή **-πρόθεμα** &lt;πρόθεμα >: το πρόθεμα ονόματος κοντέινερ χώρου αποθήκευσης.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης σε κατάσταση εντοπισμού σφαλμάτων.

**Εμφάνιση κοντέινερ χώρου αποθήκευσης [επιλογές] [κοντέινερ]**
**κοντέινερ χώρου αποθήκευσης δημιουργία [επιλογές] [κοντέινερ]**

Αυτή η εντολή δημιουργεί ένα κοντέινερ χώρου αποθήκευσης για το χώρο αποθήκευσης στο συγκεκριμένο λογαριασμό. Το λογαριασμό χώρου αποθήκευσης είναι που καθορίζεται από τη συμβολοσειρά σύνδεσης ή το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-p** ή **-πρόθεμα** &lt;πρόθεμα >: το πρόθεμα ονόματος κοντέινερ χώρου αποθήκευσης.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης σε κατάσταση εντοπισμού σφαλμάτων.

**Διαγραφή κοντέινερ χώρου αποθήκευσης [επιλογές] [κοντέινερ]**

Αυτή η εντολή διαγράφει το χώρο αποθήκευσης στο καθορισμένο κοντέινερ. Το λογαριασμό χώρου αποθήκευσης είναι που καθορίζεται από τη συμβολοσειρά σύνδεσης ή το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-p** ή **-πρόθεμα** &lt;πρόθεμα >: το πρόθεμα ονόματος κοντέινερ χώρου αποθήκευσης.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης σε κατάσταση εντοπισμού σφαλμάτων.

**Ορισμός κοντέινερ χώρου αποθήκευσης [επιλογές] [κοντέινερ]**

Αυτή η εντολή ορίζει λίστα ελέγχου πρόσβασης για το κοντέινερ χώρου αποθήκευσης. Το λογαριασμό χώρου αποθήκευσης είναι που καθορίζεται από τη συμβολοσειρά σύνδεσης ή το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-p** ή **-πρόθεμα** &lt;πρόθεμα >: το πρόθεμα ονόματος κοντέινερ χώρου αποθήκευσης.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης σε κατάσταση εντοπισμού σφαλμάτων.

###<a name="commands-to-manage-your-storage-blob"></a>Εντολές για τη διαχείριση του χώρου αποθήκευσης αντικειμένων blob

**χώρος αποθήκευσης αντικειμένων blob λίστα [επιλογές] [κοντέινερ] [πρόθεμα]**

Αυτή η εντολή επιστρέφει μια λίστα με το χώρο αποθήκευσης αντικειμένων blob στο χώρο αποθήκευσης στο καθορισμένο κοντέινερ.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-p** ή **-πρόθεμα** &lt;πρόθεμα >: το πρόθεμα ονόματος κοντέινερ χώρου αποθήκευσης.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης σε κατάσταση εντοπισμού σφαλμάτων.

**χώρος αποθήκευσης αντικειμένων blob εμφάνιση [επιλογές] [κοντέινερ] [blob]**

Αυτή η εντολή εμφανίζει τις λεπτομέρειες της το καθορισμένο χώρο αποθήκευσης αντικειμένων blob.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-p** ή **-πρόθεμα** &lt;πρόθεμα >: το πρόθεμα ονόματος κοντέινερ χώρου αποθήκευσης.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης στο εντοπισμού σφαλμάτων.

**χώρος αποθήκευσης αντικειμένων blob διαγραφή [επιλογές] [κοντέινερ] [blob]**

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-b** ή **--blob** &lt;blobName >: το όνομα του το χώρο αποθήκευσης αντικειμένων blob για να διαγράψετε.
+ **-q** ή **--χωρίς μηνύματα**: καταργήστε το καθορισμένο χώρο αποθήκευσης blob χωρίς επιβεβαίωση.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης στο εντοπισμού σφαλμάτων.

**χώρος αποθήκευσης αντικειμένων blob αποστολής [επιλογές] [αρχείο] [κοντέινερ] [blob]**

Αυτή η εντολή αποστείλετε το καθορισμένο αρχείο για το αντικείμενο blob specified\ χώρου αποθήκευσης.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-b** ή **--blob** &lt;blobName >: το όνομα του το χώρο αποθήκευσης αντικειμένων blob για την αποστολή.
+ **-t** ή **--blobtype** &lt;blobtype >: Ο τύπος χώρου αποθήκευσης αντικειμένων blob: σελίδας ή μπλοκ.
+ **-p** ή **--Ιδιότητες** &lt;ιδιότητες >: χώρος αποθήκευσης αντικειμένων blob ιδιοτήτων για απεσταλμένο αρχείο. Ιδιότητες είναι κλειδί = τιμή ζεύγος s και διαχωρισμένα με ερωτηματικό. Οι διαθέσιμες ιδιότητες είναι contentType, contentEncoding, contentLanguage και cacheControl.
+ **-m** ή **μετα-δεδομένων--** &lt;μετα-δεδομένα >: χώρος αποθήκευσης αντικειμένων blob μετα-δεδομένων απεσταλμένο αρχείο. Μετα-δεδομένων είναι κλειδί = ζεύγη τιμής μια d διαχωρισμένα με ελληνικό ερωτηματικό (;).
+ **--concurrenttaskcount** &lt;concurrenttaskcount >: Ο μέγιστος αριθμός των αιτήσεων ταυτόχρονη αποστολή.
+ **-q** ή **--χωρίς μηνύματα**: να αντικαταστήσετε το καθορισμένο χώρο αποθήκευσης blob χωρίς επιβεβαίωση.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης στο εντοπισμού σφαλμάτων.

**χώρος αποθήκευσης αντικειμένων blob λήψη [επιλογές] [κοντέινερ] [blob] [προορισμός]**

Αυτή η εντολή λήψεις το καθορισμένο χώρο αποθήκευσης αντικειμένων blob.

Αυτή η εντολή υποστηρίζει τις ακόλουθες πρόσθετες επιλογές:

+ **--κοντέινερ** &lt;κοντέινερ >: το όνομα του κοντέινερ χώρου αποθήκευσης για τη δημιουργία.
+ **-b** ή **--blob** &lt;blobName >: το όνομα του χώρου αποθήκευσης αντικειμένων blob.
+ **-d** ή **--προορισμού** [προορισμός]: λήψη αρχείου ή καταλόγου η διαδρομή προορισμού.
+ **-m** ή **--checkmd5**: το md5sum ελέγχου για το αρχείο που έχετε λάβει.
+ **--concurrenttaskcount** &lt;concurrenttaskcount > ο μέγιστος αριθμός των αιτήσεων ταυτόχρονη αποστολή
+ **-q** ή **--χωρίς μηνύματα**: αντικατάσταση του αρχείου προορισμού χωρίς επιβεβαίωση.
+ **-a** ή **--όνομα λογαριασμού** &lt;όνομα λογαριασμού >: το όνομα του λογαριασμού χώρου αποθήκευσης.
+ **-k** ή **--κλειδί λογαριασμού** &lt;accountKey >: το κλειδί λογαριασμού χώρου αποθήκευσης.
+ **-c** ή **--συμβολοσειρά σύνδεσης** &lt;συμβολοσειρά σύνδεσης >: τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.
+ **--Εντοπισμός σφαλμάτων**: εκτελεί την εντολή χώρου αποθήκευσης στο εντοπισμού σφαλμάτων.

## <a name="commands-to-manage-sql-databases"></a>Εντολές για τη Διαχείριση βάσεων δεδομένων SQL

Χρησιμοποιήστε αυτές τις εντολές για να διαχειριστείτε τις βάσεις δεδομένων SQL Azure

###<a name="commands-to-manage-sql-servers"></a>Εντολές για τη Διαχείριση διακομιστών SQL Server.

Χρησιμοποιήστε αυτές τις εντολές για να διαχειριστείτε τους διακομιστές SQL

**Δημιουργία SQL server &lt;administratorLogin > &lt;administratorPassword > &lt;θέση >**

Δημιουργήστε ένα διακομιστή βάσης δεδομένων

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**Εμφάνιση του SQL server &lt;όνομα >**

Εμφάνιση λεπτομερειών διακομιστή.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**λίστα SQL server**

Λήψη της λίστας των διακομιστών.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**Διαγραφή του SQL server &lt;όνομα >**

Διαγράφει ένα διακομιστή

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Εντολές για τη Διαχείριση βάσεων δεδομένων SQL

Χρησιμοποιήστε αυτές τις εντολές για να διαχειριστείτε τις βάσεις δεδομένων SQL.

**SQL db δημιουργία [επιλογές] &lt;όνομα_διακομιστή > &lt;όνομα βάσης δεδομένων > &lt;administratorPassword >**

Δημιουργεί μια παρουσία της βάσης δεδομένων

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL db εμφάνιση [επιλογές] &lt;όνομα_διακομιστή > &lt;όνομα βάσης δεδομένων > &lt;administratorPassword >**

Εμφάνιση λεπτομερειών βάσης δεδομένων.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**λίστα db SQL [επιλογές] &lt;όνομα_διακομιστή > &lt;administratorPassword >**

Λίστα των βάσεων δεδομένων.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL db διαγραφή [επιλογές] &lt;όνομα_διακομιστή > &lt;όνομα βάσης δεδομένων > &lt;administratorPassword >**

Διαγραφή μιας βάσης δεδομένων.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Εντολές για να διαχειριστείτε τους κανόνες τείχους προστασίας του SQL Server

Χρησιμοποιήστε αυτές τις εντολές για να διαχειριστείτε τους κανόνες τείχους προστασίας του SQL Server

**SQL firewallrule δημιουργία [επιλογές] &lt;όνομα_διακομιστή > &lt;Όνομα_κανόνα > &lt;startIPAddress > &lt;endIPAddress >**

Δημιουργήστε έναν κανόνα τείχους προστασίας για SQL Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL firewallrule εμφάνιση [επιλογές] &lt;όνομα_διακομιστή > &lt;Όνομα_κανόνα >**

Εμφάνιση τείχος προστασίας λεπτομέρειες κανόνα.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**λίστα firewallrule SQL [επιλογές] &lt;όνομα_διακομιστή >**

Λίστα με τους κανόνες τείχους προστασίας.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL firewallrule διαγραφή [επιλογές] &lt;όνομα_διακομιστή > &lt;Όνομα_κανόνα >**

Αυτή η εντολή διαγράφει έναν κανόνα τείχους προστασίας.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Εντολές για τη Διαχείριση εικονικού δίκτυά σας

Χρησιμοποιήστε αυτές τις εντολές για τη Διαχείριση εικονικού δίκτυά σας

**δίκτυο vnet δημιουργία [επιλογές] &lt;θέση >**

Δημιουργήστε ένα εικονικό δίκτυο.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**δίκτυο παρουσίασης vnet &lt;όνομα >**

Εμφάνιση λεπτομερειών ενός εικονικού δικτύου.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**λίστα vnet δικτύου**

Λίστα με όλα τα υπάρχοντα εικονικό δίκτυα.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**Διαγραφή δικτύου vnet &lt;όνομα >**

Διαγράφει το καθορισμένο εικονικού δικτύου.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**Εξαγωγή δικτύου [διαδρομή αρχείου]**

Για ρύθμιση παραμέτρων δικτύου για προχωρημένους, μπορείτε να εξαγάγετε τις παραμέτρους δικτύου τοπικά. Η ρύθμιση παραμέτρων δικτύου εξαγόμενα περιλαμβάνει ρυθμίσεις διακομιστή DNS, ρυθμίσεις εικονικού δικτύου, ρυθμίσεις τοποθεσίας τοπικού δικτύου και άλλες ρυθμίσεις.

**Εισαγωγή δικτύου [διαδρομή αρχείου]**

Εισαγάγετε μια ρύθμιση παραμέτρων τοπικού δικτύου.

**dnsserver δικτύου καταχώρηση [επιλογές] &lt;dnsIP >**

Καταχώρηση ενός διακομιστή DNS που σκοπεύετε να χρησιμοποιήσετε για την επίλυση ονομάτων στη ρύθμιση παραμέτρων του δικτύου σας.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**λίστα dnsserver δικτύου**

Παραθέτει όλους τους διακομιστές DNS που έχουν καταχωρηθεί στη ρύθμιση παραμέτρων του δικτύου σας.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**dnsserver δικτύου unregister [επιλογές] &lt;dnsIP >**

Καταργεί μια καταχώρηση διακομιστή DNS από τη ρύθμιση παραμέτρων δικτύου.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

