<properties
    pageTitle="Εξαγωγή προτύπου για τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Χρησιμοποιήστε τη διαχείριση πόρων Azure για να εξαγάγετε ένα πρότυπο από μια υπάρχουσα ομάδα πόρων."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Εξαγωγή ενός προτύπου για τη διαχείριση πόρων Azure από υπάρχοντες πόρους

Διαχείριση πόρων σάς επιτρέπει να εξαγάγετε ένα πρότυπο από διαχειριστή πόρων από υπάρχοντες πόρους στη συνδρομή σας. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο που δημιουργήθηκε για να μάθετε σχετικά με τη σύνταξη πρότυπο ή για την αυτοματοποίηση την αναδιάταξη της λύσης σας, όπως απαιτείται.

Είναι σημαντικό να λάβετε υπόψη ότι υπάρχουν δύο διαφορετικούς τρόπους για να εξαγάγετε ένα πρότυπο:

- Μπορείτε να εξαγάγετε το πραγματικό πρότυπο που χρησιμοποιήσατε για μια ανάπτυξη. Το πρότυπο που έχουν εξαχθεί περιλαμβάνει όλες τις παραμέτρους και τις μεταβλητές ακριβώς όπως που εμφανίζονταν και στο αρχικό πρότυπο. Αυτή η προσέγγιση είναι χρήσιμη όταν έχετε αναπτύξει πόρους μέσω της πύλης. Τώρα, που θέλετε να δείτε πώς μπορείτε να δημιουργήσετε το πρότυπο για να δημιουργήσετε αυτούς τους πόρους.
- Μπορείτε να εξαγάγετε ένα πρότυπο που αντιπροσωπεύει την τρέχουσα κατάσταση της ομάδας πόρων. Το πρότυπο που έχουν εξαχθεί δεν βασίζεται σε οποιοδήποτε πρότυπο που χρησιμοποιήσατε για ανάπτυξη. Αντί για αυτό, δημιουργεί ένα πρότυπο που είναι ένα στιγμιότυπο της ομάδας πόρων. Το εξαγόμενο πρότυπο έχει πολλές τιμές σχεδιασμένου και ενδέχεται να μην όσο το δυνατόν περισσότερες παραμέτρους όπως συνήθως, μπορείτε να ορίσετε. Αυτή η προσέγγιση είναι χρήσιμη όταν έχετε τροποποιήσει την ομάδα πόρων από την πύλη ή δεσμών ενεργειών. Τώρα, πρέπει να καταγράψετε την ομάδα των πόρων ως πρότυπο.

Αυτό το θέμα εμφανίζει δύο προσεγγίσεις.

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να εισέλθετε στην πύλη του Azure, δημιουργήστε ένα λογαριασμό του χώρου αποθήκευσης και εξαγωγή το πρότυπο για αυτόν το λογαριασμό χώρου αποθήκευσης. Μπορείτε να προσθέσετε ένα εικονικό δίκτυο για να τροποποιήσετε την ομάδα των πόρων. Τέλος, μπορείτε να εξαγάγετε ένα νέο πρότυπο που αντιπροσωπεύει την τρέχουσα κατάσταση. Παρόλο που αυτό το άρθρο εστιάζει σε μια απλοποιημένη υποδομή, μπορείτε να χρησιμοποιήσετε αυτά τα ίδια βήματα για να εξαγάγετε ένα πρότυπο για μια πιο σύνθετη λύση.

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

1. Στην [πύλη του Azure](https://portal.azure.com), επιλέξτε **Δημιουργία** > **χώρου αποθήκευσης** > **λογαριασμού χώρου αποθήκευσης**.

      ![Δημιουργία χώρου αποθήκευσης](./media/resource-manager-export-template/create-storage.png)

2. Δημιουργία λογαριασμού χώρου αποθήκευσης με το όνομα του **χώρου αποθήκευσης**, τα αρχικά σας και την ημερομηνία. Το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μοναδικό σε Azure. Εάν το όνομα χρησιμοποιείται ήδη, θα δείτε ένα μήνυμα σφάλματος που υποδεικνύει ότι το όνομα είναι σε χρήση. Δοκιμάστε μια παραλλαγή. Για την ομάδα πόρων, δημιουργήστε μια νέα ομάδα πόρων και ονομάστε το **ExportGroup**. Μπορείτε να χρησιμοποιήσετε τις προεπιλεγμένες τιμές για τις άλλες ιδιότητες. Επιλέξτε **Δημιουργία**.

      ![Δώστε τις τιμές για το χώρο αποθήκευσης](./media/resource-manager-export-template/provide-storage-values.png)

Ανάπτυξη μπορεί να διαρκέσει ένα λεπτό. Αφού ολοκληρωθεί η ανάπτυξη, τη συνδρομή σας περιέχει το λογαριασμό χώρου αποθήκευσης.

## <a name="view-a-template-from-deployment-history"></a>Προβολή ενός προτύπου από το ιστορικό ανάπτυξης

1. Μεταβείτε στο το blade ομάδα πόρων για τη νέα ομάδα πόρων. Παρατηρήστε ότι το blade εμφανίζει το αποτέλεσμα της τελευταίας ανάπτυξης. Επιλέξτε αυτήν τη σύνδεση.

      ![blade ομάδα πόρων](./media/resource-manager-export-template/resource-group-blade.png)

2. Μπορείτε να δείτε ένα ιστορικό των αναπτύξεων για την ομάδα. Σε περίπτωση σας, το blade παραθέτει πιθανώς μόνο μία ανάπτυξη. Επιλέξτε αυτήν την ανάπτυξη.

     ![τελευταία ανάπτυξης](./media/resource-manager-export-template/last-deployment.png)

3. Το blade εμφανίζει μια σύνοψη της ανάπτυξης. Στη σύνοψη περιλαμβάνει την κατάσταση της ανάπτυξης και τις εργασίες και τις τιμές που παρείχατε για τις παραμέτρους του. Για να δείτε το πρότυπο που χρησιμοποιήσατε για την ανάπτυξη, επιλέξτε **Προβολή πρότυπο**.

     ![Προβολή σύνοψης ανάπτυξης](./media/resource-manager-export-template/deployment-summary.png)

4. Διαχείριση πόρων ανακτά τα ακόλουθα έξι αρχεία για εσάς:

   1. **Πρότυπο** - το πρότυπο που καθορίζει την υποδομή για τη λύση. Όταν δημιουργήσατε το λογαριασμό χώρου αποθήκευσης μέσω της πύλης, διαχείριση πόρων χρησιμοποιείται ένα πρότυπο για να το αναπτύξετε και αποθηκεύσατε αυτό το πρότυπο για μελλοντική αναφορά.
   2. **Παράμετροι** - ένα αρχείο παραμέτρων που μπορείτε να χρησιμοποιήσετε για τη μεταβίβαση τιμών κατά τη διάρκεια της ανάπτυξης. Περιέχει τις τιμές που παρείχατε κατά την πρώτη ανάπτυξη, αλλά μπορείτε να την αλλάξετε οποιαδήποτε από αυτές τις τιμές, όταν έχετε αναπτύξτε ξανά το πρότυπο.
   3. **CLI** - του Azure περιβάλλον γραμμής εντολών (CLI) κρυπτογραφημένο αρχείο δέσμης ενεργειών που μπορείτε να χρησιμοποιήσετε για να αναπτύξετε το πρότυπο.
   4. **PowerShell** - αρχείο δέσμης ενεργειών του PowerShell Azure που μπορείτε να χρησιμοποιήσετε για να αναπτύξετε το πρότυπο.
   5. **.NET** - κλάσης A .NET που μπορείτε να χρησιμοποιήσετε για να αναπτύξετε το πρότυπο.
   6. **Κείμενο φωνητικής γραφής** - κλάσης A φωνητικής γραφής που μπορείτε να χρησιμοποιήσετε για να αναπτύξετε το πρότυπο.

     Τα αρχεία είναι διαθέσιμα μέσω συνδέσεων κατά μήκος του blade. Από προεπιλογή, το blade εμφανίζει το πρότυπο.

       ![προβολή προτύπου](./media/resource-manager-export-template/view-template.png)

     Ας αποδίδει ιδιαίτερη προσοχή στο πρότυπο. Το πρότυπο θα πρέπει να μοιάζει με:

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "Παράμετροι": {"όνομα": {"Τύπος": "Συμβολοσειράς"}, "accountType": {"Τύπος": "Συμβολοσειράς"}, "θέση": {"Τύπος": "Συμβολοσειράς"}, "encryptionEnabled": {"προεπιλεγμένη τιμή": false, "Τύπος": "Bool"}}, "Πόροι": [{"Τύπος": "Microsoft.Storage/storageAccounts", "sku": {"όνομα": "[parameters('accountType')]"}, "Είδος": "Αποθήκευση", "όνομα": "[parameters('name')]", "apiVersion": "2016-01-01", "θέση": "[parameters('location')]", "Ιδιότητες": {"κρυπτογράφηση": {"Υπηρεσίες": {"blob": {"έχει ενεργοποιηθεί": "[parameters('encryptionEnabled')]"}}, "κλειδιού": "Microsoft.Storage"}}}]}
 
Αυτό το πρότυπο είναι η πραγματική προτύπου που χρησιμοποιείται για να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης. Παρατηρήστε περιέχει παραμέτρους που σας επιτρέπουν να αναπτύξετε διαφορετικούς τύπους λογαριασμών χώρου αποθήκευσης. Για να μάθετε περισσότερα σχετικά με τη δομή ενός προτύπου, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md). Για την πλήρη λίστα των συναρτήσεων, μπορείτε να χρησιμοποιήσετε σε ένα πρότυπο, ανατρέξτε στο θέμα [Διαχείριση πόρων Azure πρότυπο συναρτήσεις](resource-group-template-functions.md).


## <a name="add-a-virtual-network"></a>Προσθήκη εικονικού δικτύου

Το πρότυπο που έχετε λάβει στην προηγούμενη ενότητα αναπαριστώνται την υποδομή για συγκεκριμένη αρχική ανάπτυξη. Ωστόσο, αυτό δεν θα λογαριασμού για τυχόν αλλαγές που κάνετε μετά την ανάπτυξη.
Για την απεικόνιση σε αυτό το πρόβλημα, ας τροποποιήσετε την ομάδα των πόρων, προσθέτοντας ένα εικονικό δίκτυο μέσω της πύλης.

1. Στο blade την ομάδα πόρων, επιλέξτε **Προσθήκη**.

      ![Προσθήκη πόρου](./media/resource-manager-export-template/add-resource.png)

2. Επιλογή **δικτύου εικονικές** από τους διαθέσιμους πόρους.

      ![Επιλέξτε εικονικού δικτύου](./media/resource-manager-export-template/select-vnet.png)

2. Ονομάστε το εικονικό δίκτυο **VNET**και χρησιμοποιήστε τις προεπιλεγμένες τιμές για τις άλλες ιδιότητες. Επιλέξτε **Δημιουργία**.

      ![ρύθμιση ειδοποίησης](./media/resource-manager-export-template/create-vnet.png)

3. Αφού το εικονικό δίκτυο έχει αναπτυχθεί με επιτυχία την ομάδα πόρων, εξετάστε ξανά το ιστορικό ανάπτυξης. Τώρα βλέπετε δύο αναπτύξεις. Εάν δεν βλέπετε το δεύτερο ανάπτυξης, ίσως χρειαστεί να κλείσετε το blade ομάδα πόρων και ανοίξτε το ξανά. Επιλέξτε την πιο πρόσφατη ανάπτυξης.

      ![ανάπτυξη του ιστορικού](./media/resource-manager-export-template/deployment-history.png)

4. Προβολή του προτύπου για συγκεκριμένη ανάπτυξη. Παρατηρήστε ότι το ορίζει μόνο το εικονικό δίκτυο. Δεν περιλαμβάνει το λογαριασμό χώρου αποθήκευσης που αναπτύσσεται νωρίτερα. Δεν χρειάζεται πλέον ένα πρότυπο που αντιπροσωπεύει όλους τους πόρους σας ομάδα πόρων.

## <a name="export-the-template-from-resource-group"></a>Εξαγωγή του προτύπου από ομάδα πόρων

Για να λάβετε την τρέχουσα κατάσταση της ομάδας σας πόρων, εξαγάγετε ένα πρότυπο που εμφανίζει ένα στιγμιότυπο της ομάδας πόρων.  

> [AZURE.NOTE] Δεν μπορείτε να εξαγάγετε ένα πρότυπο για μια ομάδα πόρων που έχει περισσότερα από 200 πόρους.

1. Για να προβάλετε το πρότυπο για μια ομάδα πόρων, επιλέξτε **αυτοματοποίησης δέσμης ενεργειών**.

      ![Εξαγωγή ομάδα πόρων](./media/resource-manager-export-template/export-resource-group.png)

     Δεν όλους τους τύπους πόρων υποστηρίζει τη λειτουργία εξαγωγής προτύπου. Εάν σας ομάδα πόρων περιέχει μόνο το λογαριασμό χώρου αποθήκευσης και εικονικού δικτύου που εμφανίζονται σε αυτό το άρθρο, δεν βλέπετε ένα μήνυμα σφάλματος. Ωστόσο, εάν έχετε δημιουργήσει άλλους τύπους πόρων, ενδέχεται να εμφανιστεί ένα σφάλμα που δηλώνει ότι υπάρχει πρόβλημα με την εξαγωγή. Μάθετε τον τρόπο χειρισμού των αυτά τα θέματα στην ενότητα [επιδιόρθωση ζητημάτων εξαγωγή](#fix-export-issues) .

      

2. Μπορείτε να δείτε ξανά τα έξι αρχεία που μπορείτε να χρησιμοποιήσετε για να αναπτύξετε ξανά τη λύση, αλλά αυτήν τη στιγμή το πρότυπο είναι λίγο διαφορετικά. Αυτό το πρότυπο έχει μόνο δύο παραμέτρους: μία για το όνομα του λογαριασμού χώρου αποθήκευσης και μία για το όνομα εικονικού δικτύου.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Διαχείριση πόρων δεν ανάκτηση τα πρότυπα που χρησιμοποιήσατε κατά την ανάπτυξη. Αντί για αυτό, δημιουργείται ένα νέο πρότυπο που βασίζεται σε η τρέχουσα ρύθμιση παραμέτρων των πόρων. Για παράδειγμα, το πρότυπο ορίζει την τιμή θέση και η αναπαραγωγή λογαριασμό του χώρου αποθήκευσης σε:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Έχετε δύο επιλογές για να συνεχίσετε να εργαστείτε με αυτό το πρότυπο. Μπορείτε να κάνετε λήψη του προτύπου και να εργαστούν σε αυτήν τοπικά με ένα πρόγραμμα επεξεργασίας JSON. Εναλλακτικά, μπορείτε να αποθηκεύσετε το πρότυπο στη βιβλιοθήκη σας και να εργαστείτε σε αυτό μέσω της πύλης.

     Εάν είστε εξοικειωμένοι με τη χρήση ενός προγράμματος επεξεργασίας JSON όπως [Και στο κώδικα](resource-manager-vs-code.md) ή [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), που μπορεί να προτιμάτε τη λήψη του προτύπου τοπικά και χρησιμοποιώντας αυτό το πρόγραμμα επεξεργασίας. Εάν δεν έχουν οριστεί με ένα πρόγραμμα επεξεργασίας JSON, ίσως προτιμάτε επεξεργασίας το πρότυπο μέσω της πύλης. Το υπόλοιπο μέρος αυτού του θέματος προϋποθέτει ότι έχετε αποθηκεύσει το πρότυπο στη βιβλιοθήκη σας στην πύλη. Ωστόσο, κάνετε τις ίδιες αλλαγές σύνταξη στο πρότυπο Εάν εργάζεστε τοπικά με ένα πρόγραμμα επεξεργασίας JSON ή μέσω της πύλης.

     Για να εργαστείτε τοπικά, επιλέξτε **λήψη**.

      ![Λήψη προτύπου](./media/resource-manager-export-template/download-template.png)

     Για να εργαστείτε μέσω της πύλης, επιλέξτε **Προσθήκη στη βιβλιοθήκη**.

      ![Προσθήκη σε βιβλιοθήκη](./media/resource-manager-export-template/add-to-library.png)

     Κατά την προσθήκη ενός προτύπου στη βιβλιοθήκη, δώστε στο πρότυπο ένα όνομα και μια περιγραφή. Στη συνέχεια, επιλέξτε **Αποθήκευση**.

     ![Ορισμός προτύπου τιμών](./media/resource-manager-export-template/set-template-values.png)

4. Για να προβάλετε ένα πρότυπο που αποθηκεύονται στη βιβλιοθήκη σας, επιλέξτε **περισσότερες υπηρεσίες**, πληκτρολογήστε **τα πρότυπα** για να φιλτράρετε τα αποτελέσματα, επιλέξτε **τα πρότυπα**.

      ![Εύρεση προτύπων](./media/resource-manager-export-template/find-templates.png)

5. Επιλέξτε το πρότυπο με το όνομα που έχουν αποθηκευτεί.

      ![Επιλέξτε το πρότυπο](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Προσαρμογή του προτύπου

Το πρότυπο εξαγόμενα λειτουργεί καλά, εάν θέλετε να δημιουργήσετε τον ίδιο λογαριασμό χώρου αποθήκευσης και εικονικού δικτύου για κάθε ανάπτυξη. Ωστόσο, διαχείριση πόρων παρέχει επιλογές, ώστε να μπορείτε να αναπτύξετε τα πρότυπα με πολύ περισσότερη ευελιξία. Για παράδειγμα, κατά την ανάπτυξη, ενδέχεται να θέλετε να καθορίσετε τον τύπο του λογαριασμού χώρου αποθήκευσης για τη δημιουργία ή τις τιμές που θα χρησιμοποιήσετε για το πρόθεμα διεύθυνσης εικονικού δικτύου και το πρόθεμα υποδικτύου.

Σε αυτήν την ενότητα, προσθέτετε παραμέτρους το εξαγόμενο πρότυπο, έτσι ώστε να μπορείτε να χρησιμοποιήσετε ξανά το πρότυπο κατά την ανάπτυξη αυτούς τους πόρους για άλλα περιβάλλοντα. Μπορείτε επίσης να προσθέσετε ορισμένες δυνατότητες στο πρότυπό σας για να μειώσετε την πιθανότητα αντιμετωπίζει ένα σφάλμα κατά την ανάπτυξη του προτύπου. Δεν χρειάζεται πλέον να προβλέψει ένα μοναδικό όνομα για το λογαριασμό χώρου αποθήκευσης. Αντί για αυτό, το πρότυπο δημιουργεί ένα μοναδικό όνομα. Μπορείτε να περιορίσετε τις τιμές που μπορεί να καθοριστεί για τον τύπο λογαριασμού χώρου αποθήκευσης σε μόνο οι έγκυρες επιλογές.

1. Επιλέξτε " **Επεξεργασία** " για να προσαρμόσετε το πρότυπο.

     ![Εμφάνιση προτύπου](./media/resource-manager-export-template/show-template.png)

1. Επιλέξτε το πρότυπο.

     ![Επεξεργασία προτύπου](./media/resource-manager-export-template/edit-template.png)

1. Για να έχετε τη δυνατότητα να μεταβιβάζουν τις τιμές που μπορεί να θέλετε να καθορίσετε κατά την ανάπτυξη, αντικαταστήστε την ενότητα **παραμέτρους** με νέους ορισμούς παραμέτρου. Παρατηρήστε τις τιμές του **allowedValues** για **storageAccount_accountType**. Εάν δίνετε κατά λάθος μια μη έγκυρη τιμή, αυτό το σφάλμα αναγνωρίζεται πριν από την έναρξη της ανάπτυξης. Επίσης, θα παρατηρήσετε ότι παρέχετε μόνο ένα πρόθεμα για το όνομα του λογαριασμού χώρου αποθήκευσης και το πρόθεμα περιορίζεται στα 11 χαρακτήρες. Όταν περιορίζετε το πρόθεμα 11 χαρακτήρες, μπορείτε να διασφαλίσετε ότι το πλήρες όνομα δεν υπερβαίνει τον μέγιστο αριθμό χαρακτήρων για ένα λογαριασμό του χώρου αποθήκευσης. Το πρόθεμα σάς επιτρέπει να εφαρμόσετε μια συνθήκη ονοματοθεσίας για τους λογαριασμούς σας στο χώρο αποθήκευσης. Θα δείτε πώς μπορείτε να δημιουργήσετε ένα μοναδικό όνομα στο επόμενο βήμα.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Στην ενότητα **μεταβλητές** του προτύπου σας είναι κενή αυτήν τη στιγμή. Στην ενότητα **μεταβλητές** , μπορείτε να δημιουργήσετε τιμές που απλοποιήσετε τη σύνταξη για τα υπόλοιπα του προτύπου σας. Αντικατάσταση αυτής της ενότητας με έναν νέο ορισμό μεταβλητών. Η μεταβλητή **storageAccount_name** συνδέει το πρόθεμα από την παράμετρο σε μια μοναδική συμβολοσειρά που δημιουργείται με βάση το αναγνωριστικό της ομάδας πόρων. Δεν χρειάζεται πλέον να προβλέψει ένα μοναδικό όνομα κατά την παροχή μια τιμή παραμέτρου.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Για να χρησιμοποιήσετε τις παραμέτρους και μεταβλητή των ορισμών πόρων, αντικαταστήστε την ενότητα **πόρους** με νέα ορισμούς πόρων. Παρατηρήστε ότι έχει αλλάξει λίγο στους ορισμούς πόρων εκτός από την τιμή που έχει ανατεθεί στην ιδιότητα πόρων. Οι ιδιότητες είναι η ίδια με τις ιδιότητες από το εξαγόμενο πρότυπο. Εκχωρείτε απλώς ιδιότητες σε τιμές παραμέτρων αντί για σχεδιασμένου τιμές. Τη θέση των πόρων που έχει οριστεί για να χρησιμοποιήσετε την ίδια θέση ως την ομάδα των πόρων έως την παράσταση **.location ομάδα πόρων ()** . Η μεταβλητή που δημιουργήσατε για το όνομα του λογαριασμού χώρου αποθήκευσης γίνεται αναφορά από την παράσταση **μεταβλητές** .

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Επιλέξτε **OK** όταν ολοκληρώσετε την επεξεργασία του προτύπου.

5. Επιλέξτε **Αποθήκευση** για να αποθηκεύσετε τις αλλαγές στο πρότυπο.

     ![Αποθήκευση προτύπου](./media/resource-manager-export-template/save-template.png)

6. Για να αναπτύξετε το ενημερωμένο πρότυπο, επιλέξτε **Ανάπτυξη**.

     ![ανάπτυξη προτύπου](./media/resource-manager-export-template/deploy-template.png)

7. Παρέχετε τιμές παραμέτρων και επιλέξτε μια νέα ομάδα πόρων για την ανάπτυξη των πόρων.

## <a name="update-the-downloaded-parameters-file"></a>Ενημέρωση του αρχείου λήψης παραμέτρους

Εάν εργάζεστε με τα αρχεία που έχουν ληφθεί (αντί για τη βιβλιοθήκη πύλης), πρέπει να ενημερώσετε το αρχείο λήψης παραμέτρου. Δεν είναι πλέον ταιριάζει με τις παραμέτρους του προτύπου σας. Δεν χρειάζεται να χρησιμοποιήσετε ένα αρχείο παραμέτρων, αλλά μπορεί να απλοποιήσει τη διαδικασία, όταν αναπτύξετε εκ νέου ένα περιβάλλον. Μπορείτε να χρησιμοποιήσετε τις προεπιλεγμένες τιμές που έχουν οριστεί στο πρότυπο για πολλές από τις παραμέτρους, έτσι ώστε το αρχείο παραμέτρων χρειάζεται μόνο δύο τιμές.

Αντικαταστήστε τα περιεχόμενα του αρχείου parameters.json με:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Το αρχείο ενημερωμένη παραμέτρου παρέχει τιμές μόνο για παραμέτρους που δεν έχουν μια προεπιλεγμένη τιμή. Μπορείτε να παράσχετε τιμές για τις άλλες παραμέτρους, όταν θέλετε μια τιμή που είναι διαφορετική από την προεπιλεγμένη τιμή.

## <a name="fix-export-issues"></a>Διόρθωση προβλημάτων εξαγωγής

Δεν όλους τους τύπους πόρων υποστηρίζει τη λειτουργία εξαγωγής προτύπου. Διαχείριση πόρων συγκεκριμένα δεν εξαγωγή ορισμένους τύπους πόρων για να αποτρέψετε την εκθέσετε ευαίσθητα δεδομένα. Για παράδειγμα, εάν έχετε μια συμβολοσειρά σύνδεσης στο αρχείο config τοποθεσίας, πιθανότατα δεν θέλετε να εμφανίζεται ρητά σε ένα πρότυπο που έχουν εξαχθεί. Μπορείτε να λάβετε γύρω από αυτό το ζήτημα προσθέτοντας με μη αυτόματο τρόπο τους πόρους που λείπουν πίσω στο πρότυπό σας.

> [AZURE.NOTE] Μόνο αντιμετωπίσετε θέματα με την εξαγωγή κατά την εξαγωγή από μια ομάδα πόρων και όχι από το ιστορικό της ανάπτυξης. Εάν την τελευταία ανάπτυξη αντιπροσωπεύει με ακρίβεια την τρέχουσα κατάσταση της ομάδας πόρων, θα πρέπει να εξαγάγετε το πρότυπο από το ιστορικό ανάπτυξης και όχι από την ομάδα πόρων. Εξαγάγετε μόνο από μια ομάδα πόρων όταν έχετε κάνει αλλαγές στην ομάδα πόρων που δεν έχουν οριστεί σε ένα μεμονωμένο πρότυπο.

Για παράδειγμα, εάν κάνετε εξαγωγή ενός προτύπου για μια ομάδα πόρων που περιέχει μια εφαρμογή web, βάση δεδομένων SQL και συμβολοσειρά σύνδεσης στο αρχείο config τοποθεσίας, μπορείτε να δείτε το ακόλουθο μήνυμα:

![Εμφάνιση σφάλματος](./media/resource-manager-export-template/show-error.png)

Επιλέγοντας το μήνυμα εμφανίζει ακριβώς δεν είχαν εξαχθεί ποιους τύπους πόρων. 
     
![Εμφάνιση σφάλματος](./media/resource-manager-export-template/show-error-details.png)

Αυτό το θέμα δείχνει συνήθεις διορθωτικές ενέργειες.

### <a name="connection-string"></a>Συμβολοσειρά σύνδεσης

Στον πόρο τοποθεσίες web, προσθέστε έναν ορισμό για τη συμβολοσειρά σύνδεσης με τη βάση δεδομένων:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Επέκταση της τοποθεσίας Web

Στον πόρο τοποθεσίας web, προσθέστε έναν ορισμό για τον κωδικό για την εγκατάσταση:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Επέκταση εικονική μηχανή

Για παραδείγματα των επεκτάσεων εικονική μηχανή, ανατρέξτε στο θέμα [Δείγματα ρύθμισης παραμέτρων επέκταση Εικονική Windows Azure](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>Η πύλη εικονικού δικτύου

Προσθέστε έναν τύπο πόρου πύλης εικονικού δικτύου.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Τοπικό δίκτυο πύλης

Προσθέστε έναν τύπο πόρου πύλης τοπικού δικτύου.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Σύνδεση

Προσθέστε έναν τύπο πόρου σύνδεσης.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Επόμενα βήματα

Συγχαρητήρια! Μάθατε πώς μπορείτε να εξαγάγετε ένα πρότυπο από τους πόρους που δημιουργήσατε στην πύλη.

- Μπορείτε να αναπτύξετε ένα πρότυπο μέσω του [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md)ή [REST API](resource-group-template-deploy-rest.md).
- Για να δείτε πώς μπορείτε να εξαγάγετε ένα πρότυπο μέσω του PowerShell, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](powershell-azure-resource-manager.md).
- Για να δείτε πώς μπορείτε να εξαγάγετε ένα πρότυπο μέσω Azure CLI, ανατρέξτε στο θέμα [χρήση του Azure CLI για Mac, Linux, και Windows με το διαχειριστή πόρων Azure](xplat-cli-azure-resource-manager.md).