<properties
   pageTitle="Διαχείριση πόρων και κλασική ανάπτυξης | Microsoft Azure"
   description="Περιγράφει τις διαφορές μεταξύ του μοντέλου ανάπτυξης διαχείρισης πόρων και το κλασικό (ή διαχείριση της υπηρεσίας) μοντέλο ανάπτυξης."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure διαχείριση πόρων έναντι κλασική ανάπτυξης: Κατανόηση των μοντέλων ανάπτυξης και της κατάστασης των πόρων σας

Σε αυτό το θέμα, θα μάθετε σχετικά με τη διαχείριση πόρων Azure και μοντέλα κλασική ανάπτυξης, την κατάσταση των πόρων σας και γιατί είχαν αναπτυχθεί τους πόρους σας με ένα ή το άλλο. Η διαχείριση πόρων και τα μοντέλα κλασική ανάπτυξης αντιπροσωπεύουν δύο διαφορετικούς τρόπους για την ανάπτυξη και τη διαχείριση του Azure λύσεις. Μπορείτε να εργαστείτε με αυτά μέσω δύο διαφορετικά σύνολα API και τους πόρους ανεπτυγμένος μπορούν να περιέχουν σημαντικές διαφορές. Τα δύο μοντέλα δεν είναι πλήρως συμβατό με τις μεταξύ τους. Αυτό το θέμα περιγράφει τις διαφορές.

Για να απλοποιήσετε την ανάπτυξη και τη διαχείριση των πόρων, η Microsoft συνιστά τη χρησιμοποιήσετε για τη διαχείριση πόρων για όλους τους πόρους νέα. Εάν είναι δυνατόν, η Microsoft συνιστά αναπτύξτε ξανά υπάρχοντες πόρους μέσω της διαχείρισης πόρων.

Εάν είστε εξοικειωμένοι με τη διαχείριση πόρων, που μπορεί να θέλετε να πρώτα Αναθεωρήστε την ορολογία που ορίζονται από το στην [Επισκόπηση της διαχείρισης πόρων Azure](azure-resource-manager/resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>Ιστορικό των μοντέλων ανάπτυξης

Azure παρέχονται αρχικά μόνο το μοντέλο κλασική ανάπτυξης. Σε αυτό το μοντέλο, κάθε πόρο υπήρχε ανεξάρτητη; δεν υπήρχε τρόπος να ομαδοποιήσετε Σχετικοί πόροι. Αντί για αυτό, θα έπρεπε να παρακολουθείτε τους πόρους που αποτελούνται λύσης ή της εφαρμογής σας με μη αυτόματο τρόπο και να θυμάστε ότι για να διαχειριστείτε τους στο συντονισμένη προσέγγιση. Για να αναπτύξετε μια λύση, θα έπρεπε να δημιουργήσετε κάθε πόρο μεμονωμένα μέσω της πύλης κλασική ή να δημιουργήσετε μια δέσμη ενεργειών που αναπτυχθεί όλους τους πόρους στη σωστή σειρά. Για να διαγράψετε μια λύση, θα έπρεπε να διαγράψετε μεμονωμένα κάθε πόρο. Δεν εύκολα θα μπορούσε να εφαρμόσετε και ενημέρωση πολιτικών του ελέγχου πρόσβασης για Σχετικοί πόροι. Τέλος, μπορείτε να εφαρμόσετε δεν ετικετών με πόρους ετικέτα τους με τους όρους που σας βοηθούν να παρακολουθείτε τους πόρους σας και Διαχείριση χρέωσης.

Στο 2014, Azure εισάγονται διαχείριση πόρων, η οποία προσθέσατε την έννοια μιας ομάδας πόρων. Ομάδα πόρων είναι ένα κοντέινερ για τους πόρους που έχουν ένα κοινό κύκλου ζωής. Το μοντέλο ανάπτυξης διαχείρισης πόρων παρέχει πολλά πλεονεκτήματα:

- Μπορείτε να ανάπτυξη, διαχείριση και την παρακολούθηση όλων των υπηρεσιών για τη λύση ως ομάδα, και όχι χειρισμός αυτές τις υπηρεσίες μεμονωμένα.
- Επανειλημμένα για να αναπτύξετε τη λύση σας σε όλη τη διάρκεια του κύκλου ζωής και εμπιστεύεστε τους πόρους σας έχουν αναπτυχθεί σε μια συνεπή κατάσταση.
- Μπορείτε να εφαρμόσετε τον έλεγχο πρόσβασης σε όλους τους πόρους σας ομάδα πόρων και οι οποίες θα εφαρμοστούν αυτόματα όταν νέων πόρων προστίθενται στην ομάδα πόρων.
- Μπορείτε να εφαρμόσετε ετικέτες με πόρους για να οργανώσετε λογικά όλους τους πόρους στη συνδρομή σας.
- Μπορείτε να χρησιμοποιήσετε σημειογραφίας αντικειμένων JavaScript (JSON) για να ορίσετε την υποδομή για τη λύση. Το αρχείο JSON είναι γνωστό ως πρότυπο για τη διαχείριση πόρων.
- Μπορείτε να ορίσετε τις εξαρτήσεις μεταξύ τους πόρους, ώστε να αναπτύσσονται στη σωστή σειρά.

Κατά την προσθήκη διαχειριστή πόρων, όλοι οι πόροι έχουν προστεθεί αναδρομική προεπιλεγμένες ομάδες πόρων. Εάν δημιουργήσετε έναν πόρο με κλασική ανάπτυξης τώρα, ο πόρος δημιουργείται αυτόματα μέσα σε μια προεπιλεγμένη ομάδα πόρων για αυτήν την υπηρεσία, ακόμα και αν δεν καθορίσατε αυτήν την ομάδα πόρων στην ανάπτυξη. Ωστόσο, απλώς υπάρχουσες μέσα σε μια ομάδα πόρων δεν σημαίνει ότι ο πόρος έχει μετατραπεί στο μοντέλο διαχείρισης πόρων. Θα εξετάσουμε πώς κάθε υπηρεσία χειρίζεται τα μοντέλα δύο ανάπτυξης στην επόμενη ενότητα. 

## <a name="understand-support-for-the-models"></a>Κατανόηση των υποστήριξης για τα μοντέλα 

Όταν αποφασίζετε για το μοντέλο ανάπτυξης που θα χρησιμοποιήσετε για τους πόρους σας, υπάρχουν τρία σενάρια που πρέπει να γνωρίζετε:

1. Η υπηρεσία υποστηρίζει διαχείριση πόρων και παρέχει ένα μόνο τύπο.
2. Η υπηρεσία υποστηρίζει διαχείριση πόρων, αλλά παρέχει δύο τύπους - μία για διαχείριση πόρων και μία για κλασική. Αυτό το σενάριο ισχύει μόνο σε εικονικές μηχανές, λογαριασμούς χώρου αποθήκευσης και εικονικού δίκτυα.
3. Η υπηρεσία δεν υποστηρίζει διαχείριση πόρων.

Για να ανακαλύψετε εάν μια υπηρεσία υποστηρίζει διαχείριση πόρων, ανατρέξτε στο θέμα [Διαχείριση πόρων υποστηρίζονται υπηρεσίες παροχής](resource-manager-supported-services.md).

Εάν δεν υποστηρίζει την υπηρεσία που θέλετε να χρησιμοποιήσετε για τη διαχείριση πόρων, που πρέπει να συνεχίσετε να χρησιμοποιείτε κλασική ανάπτυξης.

Εάν η υπηρεσία υποστηρίζει διαχείριση πόρων και **δεν είναι** μια εικονική μηχανή, το λογαριασμό χώρου αποθήκευσης ή εικονικό δίκτυο, μπορείτε να χρησιμοποιήσετε για τη διαχείριση πόρων χωρίς οποιαδήποτε επιπλοκές.

Για εικονικές μηχανές, λογαριασμούς χώρου αποθήκευσης και εικονικών δικτύων, εάν ο πόρος που δημιουργήθηκε μέσω κλασική ανάπτυξη, πρέπει να συνεχίσετε να λειτουργούν το μέσω κλασική λειτουργίες. Εάν η εικονική μηχανή, το λογαριασμό χώρου αποθήκευσης ή εικονικού δικτύου που δημιουργήθηκε μέσω ανάπτυξης για τη διαχείριση πόρων, που πρέπει να συνεχίσετε να χρησιμοποιείτε λειτουργίες διαχείρισης πόρων. Αυτή η διάκριση να λάβετε προκαλεί σύγχυση κατά τη συνδρομή σας περιέχει συνδυασμό των πόρων που δημιουργήθηκε μέσω της διαχείρισης πόρων και κλασική ανάπτυξης. Αυτός ο συνδυασμός των πόρων να δημιουργήσετε μη αναμενόμενα αποτελέσματα, επειδή οι πόροι δεν υποστηρίζουν τις ίδιες ενέργειες.

Σε ορισμένες περιπτώσεις, μια εντολή Διαχείριση πόρων να ανακτήσετε πληροφορίες σχετικά με έναν πόρο που δημιουργήθηκε μέσω κλασική ανάπτυξης ή να εκτελέσετε μια εργασία διαχείρισης, όπως η μετακίνηση ενός κλασική πόρου σε μια άλλη ομάδα πόρων. Όμως, δεν θα πρέπει να σας δώσει την εντύπωση ότι ο τύπος υποστηρίζει λειτουργίες διαχείρισης πόρων αυτές τις περιπτώσεις. Για παράδειγμα, ας υποθέσουμε ότι έχετε μια ομάδα πόρων που περιέχει μια εικονική μηχανή που δημιουργήθηκε με κλασική ανάπτυξη. Εάν εκτελείτε την παρακάτω εντολή PowerShell για τη διαχείριση πόρων:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Επιστρέφει την εικονική μηχανή:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Ωστόσο, η διαχείριση πόρων cmdlet **Get-AzureRmVM** επιστρέφει μόνο εικονικές μηχανές αναπτυχθεί μέσω της διαχείρισης πόρων. Την ακόλουθη εντολή δεν επιστρέφει το εικονικό μηχάνημα που δημιουργήθηκε μέσω κλασική ανάπτυξης.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Μόνο οι πόροι που δημιουργήθηκε στις ετικέτες υποστήριξη διαχείρισης πόρων. Δεν μπορείτε να εφαρμόσετε ετικέτες κλασική πόρους.

## <a name="resource-manager-characteristics"></a>Διαχείριση πόρων χαρακτηριστικά

Για να σας βοηθήσει να κατανοήσετε των δύο μοντέλων, ας Αναθεωρήστε τα χαρακτηριστικά των τύπων για τη διαχείριση πόρων:

- Δημιουργήθηκε μέσω της [Azure πύλη](https://portal.azure.com/).

     ![Πύλη του Azure](./media/resource-manager-deployment-model/portal.png)

     Για υπολογισμού, αποθήκευσης και δικτύωσης πόρους, έχετε την επιλογή χρήση διαχείριση πόρων ή κλασική ανάπτυξης. Επιλέξτε **Διαχείριση πόρων**.

     ![Διαχείριση πόρων ανάπτυξης](./media/resource-manager-deployment-model/select-resource-manager.png)

- Δημιουργήθηκε με την έκδοση για τη διαχείριση πόρων του τα cmdlet του Azure PowerShell. Αυτές οι εντολές έχει τη μορφή *Ρηματικές AzureRmNoun*.

        New-AzureRmResourceGroupDeployment

- Δημιουργία μέσω του [Azure διαχείριση πόρων REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) για τις ΥΠΌΛΟΙΠΕΣ εργασίες.

- Δημιουργήθηκε στις εντολές Azure CLI εκτέλεση σε κατάσταση λειτουργίας **arm** .

        azure config mode arm
        azure group deployment create 

- Ο τύπος πόρου δεν περιλαμβάνει το **(κλασική)** στο όνομα. Η παρακάτω εικόνα εμφανίζει τον τύπο ως **λογαριασμός χώρου αποθήκευσης**.

    ![εφαρμογή Web](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Χαρακτηριστικά κλασική ανάπτυξης

Μπορεί να γνωρίζετε επίσης το μοντέλο κλασική ανάπτυξης ως το μοντέλο διαχείριση της υπηρεσίας.

Πόροι που έχουν δημιουργηθεί με το μοντέλο κλασική ανάπτυξης κοινή χρήση τα εξής χαρακτηριστικά:

- Δημιουργήθηκε μέσω της [κλασικής πύλη](https://manage.windowsazure.com)

     ![Κλασική πύλη](./media/resource-manager-deployment-model/classic-portal.png)

     Εναλλακτικά, την πύλη του Azure και προσδιορίσετε **κλασική** ανάπτυξης (για υπολογισμού, το χώρο αποθήκευσης και δικτύωση).

     ![Κλασική ανάπτυξης](./media/resource-manager-deployment-model/select-classic.png)

- Δημιουργήθηκε μέσω της υπηρεσίας διαχείρισης έκδοσης του τα cmdlet του Azure PowerShell. Αυτά τα ονόματα εντολή έχει τη μορφή *Ρηματικές AzureNoun*.

        New-AzureVM 

- Δημιουργία μέσω του [Υπηρεσίας διαχείρισης REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) για τις ΥΠΌΛΟΙΠΕΣ εργασίες.
- Δημιουργήθηκε στις εντολές Azure CLI που εκτελείται σε λειτουργία **asm** .

        azure config mode asm
        azure vm create 

- Ο τύπος πόρου περιλαμβάνει **(κλασική)** στο όνομα. Η παρακάτω εικόνα εμφανίζει τον τύπο ως **λογαριασμός χώρου αποθήκευσης (κλασική)**.

    ![Κλασική τύπου](./media/resource-manager-deployment-model/classic-type.png)

Μπορείτε να χρησιμοποιήσετε την πύλη του Azure για να διαχειριστείτε τους πόρους που έχουν δημιουργηθεί μέσω κλασική ανάπτυξης.

## <a name="changes-for-compute-network-and-storage"></a>Αλλαγές για υπολογισμού, το δίκτυο και χώρου αποθήκευσης

Το παρακάτω διάγραμμα εμφανίζει υπολογισμού, δικτύου και αποθήκευσης πόρους αναπτυχθεί μέσω της διαχείρισης πόρων.

![Διαχείριση πόρων αρχιτεκτονικής](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Λάβετε υπόψη τις παρακάτω σχέσεις μεταξύ τους πόρους:

- Όλους τους πόρους που υπάρχει μέσα σε μια ομάδα πόρων.
- Η εικονική μηχανή εξαρτάται από ένα λογαριασμό συγκεκριμένο χώρο αποθήκευσης που ορίζονται από το στην υπηρεσία παροχής πόρων χώρου αποθήκευσης για να αποθηκεύσετε το δίσκων στο χώρο αποθήκευσης αντικειμένων blob (απαιτείται).
- Η εικονική μηχανή αναφέρεται σε ένα συγκεκριμένο NIC οριστεί στην υπηρεσία παροχής πόρων δικτύου (απαιτείται) και διαθεσιμότητα που καθορίζονται στην υπηρεσία παροχής πόρων υπολογισμού (προαιρετικό).
- NIC αναφορές η εικονική μηχανή εκχωρημένη διεύθυνση IP (απαιτείται), το υποδίκτυο του εικονικού δικτύου για την εικονική μηχανή (απαιτείται), καθώς και σε μια ομάδα ασφαλείας δικτύου (προαιρετικό).
- Το υποδίκτυο μέσα σε ένα εικονικό δίκτυο αναφέρεται σε μια ομάδα ασφαλείας δικτύου (προαιρετικό).
- Η παρουσία εξισορρόπησης φόρτου αναφορές του χώρου συγκέντρωσης παρασκηνίου των διευθύνσεων IP που περιλαμβάνει το NIC από μια εικονική μηχανή (προαιρετικό) και αναφέρεται σε μια φόρτωση εξισορρόπησης δημόσια ή ιδιωτική διεύθυνση IP (προαιρετικό).

Εδώ θα βρείτε τα στοιχεία και οι σχέσεις τους για κλασική ανάπτυξη:

![Κλασική αρχιτεκτονικής](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Η κλασική λύση για τη φιλοξενία μια εικονική μηχανή περιλαμβάνει τα εξής:

- Μια υπηρεσία cloud απαιτείται που λειτουργεί ως κοντέινερ για τη φιλοξενία εικονικές μηχανές (υπολογισμού). Εικονικές μηχανές παρέχονται αυτόματα με το περιβάλλον εργασίας κάρτα δικτύου (NIC) και μια διεύθυνση IP που έχουν εκχωρηθεί από Azure. Επιπλέον, η υπηρεσία cloud περιέχει μια παρουσία εξισορρόπησης φόρτου εξωτερικών, μια δημόσια διεύθυνση IP και τελικά σημεία προεπιλογή για να επιτρέψετε απομακρυσμένης επιφάνειας εργασίας και απομακρυσμένης PowerShell την κυκλοφορία για εικονικές μηχανές που βασίζεται σε Windows και κίνηση ασφαλούς κελύφους (SSH) για βάσει Linux εικονικές μηχανές.
- Ένας λογαριασμός απαιτούμενο χώρο αποθήκευσης που αποθηκεύει τα VHD για μια εικονική μηχανή, συμπεριλαμβανομένου του λειτουργικού συστήματος, προσωρινά, καθώς και πρόσθετες δεδομένων δίσκων (αποθήκευση).
- Ένα προαιρετικό εικονικού δικτύου που λειτουργεί ως ένα πρόσθετο κοντέινερ, στο οποίο μπορείτε να δημιουργήσετε μια δομή με υποδίκτυα και να καθορίσετε το υποδίκτυο στον οποίο βρίσκεται η εικονική μηχανή (δίκτυο).

Ο παρακάτω πίνακας περιγράφει τις αλλαγές στο τον τρόπο υπολογισμού, δικτύου και αποθήκευσης υπηρεσίες παροχής πόρων αλληλεπίδρασης:

 Στοιχείο | Κλασικό | Διαχείριση πόρων
 ---|---|---
| Υπηρεσία στο cloud για εικονικές μηχανές |  Υπηρεσία cloud ήταν ένα κοντέινερ για τη διατήρηση του εικονικές μηχανές οι οποίες απαιτούνται Διαθεσιμότητα από την πλατφόρμα και εξισορρόπηση φόρτου. | Υπηρεσία cloud δεν είναι πλέον απαραίτητη για να δημιουργήσετε μια εικονική μηχανή χρησιμοποιώντας το νέο μοντέλο αντικειμένου. |
| Εικονικό δίκτυα | Ένα εικονικό δίκτυο είναι προαιρετική για την εικονική μηχανή. Εάν συμπεριλαμβάνεται, δεν μπορεί να αναπτυχθεί το εικονικό δίκτυο με τη διαχείριση πόρων. | Εικονική μηχανή απαιτεί ένα εικονικό δίκτυο που έχει αναπτυχθεί με τη διαχείριση πόρων. |
| Λογαριασμοί χώρου αποθήκευσης | Η εικονική μηχανή απαιτεί ένα λογαριασμό του χώρου αποθήκευσης που αποθηκεύει τα VHD για το λειτουργικό σύστημα, δίσκων προσωρινά, καθώς και πρόσθετες δεδομένων. | Η εικονική μηχανή απαιτεί ένα λογαριασμό του χώρου αποθήκευσης για να αποθηκεύσετε το δίσκων στο χώρο αποθήκευσης αντικειμένων blob. |
| Διαθεσιμότητα σύνολα | Διαθεσιμότητα για την πλατφόρμα ήταν υποδεικνύεται από τη ρύθμιση των παραμέτρων του ίδιου "AvailabilitySetName" στην τις εικονικές μηχανές. Ο μέγιστος αριθμός των τομέων σφαλμάτων ήταν 2. | Ορισμός διαθεσιμότητας είναι ένας πόρος που εκτίθεται από την υπηρεσία παροχής Microsoft.Compute. Εικονικές μηχανές που απαιτούν υψηλή διαθεσιμότητα πρέπει να περιλαμβάνονται στο σύνολο διαθεσιμότητα. Ο μέγιστος αριθμός των τομέων σφαλμάτων είναι τώρα 3. |
| Ομάδες συσχέτισης | Ομάδες συσχέτισης ήταν απαραίτητες για τη δημιουργία εικονικών δικτύων. Ωστόσο, με την εισαγωγή του τοπικές εικονικών δικτύων, που δεν απαιτείται πλέον. |Για να απλοποιήσετε, την έννοια ομάδες συσχέτισης δεν υπάρχει στον τα API εκτίθεται μέσω της διαχείρισης πόρων Azure. |
| Εξισορρόπηση φόρτου    | Η δημιουργία μια υπηρεσία Cloud παρέχει μια μη ρητή εξισορρόπηση φόρτου για τις εικονικές μηχανές αναπτυχθεί. | Η μονάδα εξισορρόπησης φόρτου είναι ένας πόρος που εκτίθεται από την υπηρεσία παροχής Microsoft.Network. Το περιβάλλον εργασίας πρωτεύοντος δικτύου από τις εικονικές μηχανές που πρέπει να εξισορρόπησης φόρτου πρέπει να είναι αναφορά τη μονάδα εξισορρόπησης φόρτου. Φόρτωση Balancers μπορεί να είναι εσωτερικό ή εξωτερικό. Μια παρουσία εξισορρόπησης φόρτου αναφορές του χώρου συγκέντρωσης παρασκηνίου των διευθύνσεων IP που περιλαμβάνει το NIC από μια εικονική μηχανή (προαιρετικό) και αναφέρεται σε μια φόρτωση εξισορρόπησης δημόσια ή ιδιωτική διεύθυνση IP (προαιρετικό). [Διαβάστε περισσότερα.](../articles/resource-groups-networking.md) |
|Εικονική διεύθυνση IP | Υπηρεσίες cloud γρήγορα ένα προεπιλεγμένο VIP (εικονική διεύθυνση IP) όταν μια Εικονική προστίθεται σε μια υπηρεσία cloud. Η εικονική διεύθυνση IP είναι η διεύθυνση που σχετίζεται με την μη ρητή εξισορρόπηση φόρτου.    | Δημόσια διεύθυνση IP είναι ένας πόρος που εκτίθεται από την υπηρεσία παροχής Microsoft.Network. Δημόσια διεύθυνση IP μπορεί να είναι στατικό (δεσμευμένη) ή δυναμικό. Δυναμική δημόσια διευθύνσεις IP, μπορείτε να αντιστοιχίσετε σε μια μονάδα εξισορρόπησης φόρτου. Δημόσια διευθύνσεις IP μπορεί να είναι ασφαλείς χρησιμοποιώντας τις ομάδες ασφαλείας. |
|Δεσμευμένη διεύθυνση IP Address|   Μπορείτε να δεσμεύσετε μια διεύθυνση IP στο Azure και να συσχετίσετε με μια υπηρεσία στο Cloud για να βεβαιωθείτε ότι η διεύθυνση IP είναι ασύγχρονα.   | Δημόσια διεύθυνση IP μπορούν να δημιουργηθούν σε λειτουργία "Στατική" και προσφέρει την ίδια δυνατότητα ως ένα "δεσμευμένη διεύθυνση IP". Στατική δημόσια διευθύνσεις IP μπορείτε μόνο να αντιστοιχίσετε σε μια μονάδα εξισορρόπησης φόρτου αυτήν τη στιγμή. |
|Δημόσια διεύθυνση IP (PIP) ανά Εικονική | Δημόσιες διευθύνσεις IP μπορεί επίσης να συσχετιστεί με μια Εικονική απευθείας. | Δημόσια διεύθυνση IP είναι ένας πόρος που εκτίθεται από την υπηρεσία παροχής Microsoft.Network. Δημόσια διεύθυνση IP μπορεί να είναι στατικό (δεσμευμένη) ή δυναμικό. Ωστόσο, μπορούν να αντιστοιχιστούν μόνο δυναμικής δημόσια διευθύνσεις IP για ένα περιβάλλον εργασίας δικτύου για να λάβετε μια δημόσια IP ανά Εικονική αυτήν τη στιγμή. |
|Τα τελικά σημεία| Τελικά σημεία εισόδου χρειάζεται να ρυθμιστούν σε μια εικονική μηχανή να είναι ανοιχτά ασφαλείας connectivity για ορισμένες θύρες. Μία από τις κοινές λειτουργίες σύνδεσης σε εικονικές μηχανές την εργασία με τη ρύθμιση τελικά σημεία εισόδου. | Κανόνες εισερχόμενων NAT μπορεί να ρυθμιστεί σε Balancers φόρτωσης για να επιτύχετε το ίδιο δυνατότητα της ενεργοποίησης σε συγκεκριμένες θύρες για τη σύνδεση του ΣΠΣ τα τελικά σημεία. |
|Όνομα DNS| Μια υπηρεσία cloud θα λάβετε μια μη ρητή καθολικά μοναδικό όνομα DNS. Για παράδειγμα: `mycoffeeshop.cloudapp.net`. | Τα ονόματα DNS είναι προαιρετικές παράμετροι που μπορεί να καθοριστεί σε έναν πόρο δημόσια διεύθυνση IP. Το FQDN έχει την παρακάτω μορφή - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Διασυνδέσεις δικτύου | Κύρια και δευτερεύουσα διασύνδεση δικτύου και τις ιδιότητές καθορίστηκαν ως ρύθμιση παραμέτρων δικτύου για μια εικονική μηχανή. | Περιβάλλον εργασίας του δικτύου είναι ένας πόρος που εκτίθεται από την υπηρεσία παροχής Microsoft.Network. Η διάρκεια του κύκλου ζωής του περιβάλλοντος εργασίας δικτύου δεν είναι συνδεδεμένη με μια εικονική μηχανή. Αναφέρεται η εικονική μηχανή εκχωρημένη διεύθυνση IP (απαιτείται), το υποδίκτυο του εικονικού δικτύου για την εικονική μηχανή (απαιτείται), καθώς και σε μια ομάδα ασφαλείας δικτύου (προαιρετικό). |

Για να μάθετε σχετικά με τη σύνδεση εικονικού δίκτυα από μοντέλα διαφορετική ανάπτυξη, ανατρέξτε στο θέμα [σύνδεση εικονικού δίκτυα από διαφορετική ανάπτυξη μοντέλα στην πύλη](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Μετεγκατάσταση από κλασική για τη διαχείριση πόρων

Εάν είστε έτοιμοι να μετεγκαταστήσετε τους πόρους σας από κλασική ανάπτυξης στο διαχειριστή πόρων ανάπτυξης, ανατρέξτε στο θέμα:

1. [Τεχνική βαθύ κατάρρευση στη μετεγκατάσταση πλατφόρμα που υποστηρίζονται από κλασική διαχείριση πόρων για να Azure](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Μετεγκατάσταση πλατφόρμα υποστηρίζεται IaaS πόρων από κλασική διαχείριση πόρων για να Azure](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Μετεγκατάσταση IaaS πόρους από κλασική διαχείριση πόρων για να Azure με χρήση του Azure PowerShell](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Μετεγκατάσταση IaaS πόρους από κλασική διαχείριση πόρων για να Azure χρησιμοποιώντας Azure CLI](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Συνήθεις ερωτήσεις

**Μπορώ να δημιουργήσω μια εικονική μηχανή χρησιμοποιώντας τη διαχείριση πόρων Azure να αναπτύξετε ένα λογαριασμό χώρου αποθήκευσης που δημιουργούνται με χρήση κλασική ανάπτυξης ή εικονικού δικτύου;**

Αυτό δεν υποστηρίζεται. Δεν μπορείτε να χρησιμοποιήσετε για τη διαχείριση πόρων Azure για να αναπτύξετε μια εικονική μηχανή σε ένα εικονικό δίκτυο που δημιουργήθηκε με χρήση κλασική ανάπτυξης.

**Μπορώ να δημιουργήσω μια εικονική μηχανή χρησιμοποιώντας τη διαχείριση πόρων Azure από μια εικόνα χρήστη που δημιουργήθηκε χρησιμοποιώντας τα API διαχείρισης υπηρεσίας Azure;**

Αυτό δεν υποστηρίζεται. Ωστόσο, μπορείτε να αντιγράψετε τα αρχεία VHD από ένα λογαριασμό του χώρου αποθήκευσης που δημιουργήθηκε χρησιμοποιώντας τα API διαχείρισης υπηρεσίας, και να τις προσθέσετε σε ένα νέο λογαριασμό που δημιουργήθηκε μέσω της διαχείρισης πόρων Azure.

**Τι είναι οι επιπτώσεις στην του ορίου για τη συνδρομή μου;**

Τα όρια για το εικονικές μηχανές, εικονικό δίκτυα και λογαριασμούς χώρου αποθήκευσης που δημιουργήθηκε μέσω της διαχείρισης πόρων Azure είναι ξεχωριστό από άλλα όρια. Κάθε συνδρομής λαμβάνει ορίων για να δημιουργήσετε τους πόρους με τα νέα API. Μπορείτε να διαβάσετε περισσότερα σχετικά με τα όρια επιπλέον [εδώ](../articles/azure-subscription-service-limits.md).

**Μπορώ να συνεχίσω να χρησιμοποιήσετε το αυτοματοποιημένο δέσμες ενεργειών για την προετοιμασία εικονικές μηχανές, εικονικό δίκτυα και λογαριασμούς χώρο αποθήκευσης έως τα API διαχείρισης πόρων;**

Όλα τα αυτοματισμού και δεσμών ενεργειών που έχετε δημιουργήσει εξακολουθούν να λειτουργούν για τις υπάρχουσες εικονικές μηχανές, εικονικό δίκτυα που δημιουργούνται στην περιοχή στη Διαχείριση υπηρεσίας Azure λειτουργία. Ωστόσο, οι δέσμες ενεργειών πρέπει να ενημερωθούν ώστε να χρησιμοποιούν το νέο σχήμα για τη δημιουργία τους ίδιους πόρους με τον τρόπο διαχείρισης πόρων.

**Πού μπορώ να βρω παραδείγματα προτύπων από διαχειριστή πόρων Azure;**

Ένα ολοκληρωμένο σύνολο από πρότυπα starter μπορεί να βρεθεί στην [Azure πρότυπα γρήγορη έναρξη διαχείρισης πόρων](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Επόμενα βήματα

- Για να δείτε τη δημιουργία του προτύπου που καθορίζει μια εικονική μηχανή λογαριασμού χώρου αποθήκευσης και εικονικού δικτύου, ανατρέξτε στο θέμα [αναλυτικές οδηγίες για το πρότυπο διαχείρισης πόρων](resource-manager-template-walkthrough.md).
- Για να δείτε τις εντολές για την ανάπτυξη ενός προτύπου, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).