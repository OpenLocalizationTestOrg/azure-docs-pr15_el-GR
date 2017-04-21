## <a name="how-to-deploy-with-azure-cli"></a>Πώς μπορείτε να αναπτύξετε με Azure CLI

1. Συνδεθείτε στο λογαριασμό σας Azure.

        azure login

  Μετά την παροχή τα διαπιστευτήριά σας, η εντολή επιστρέφει το αποτέλεσμα της σύνδεσής σας.

        ...
        info:    login command OK

2. Εάν έχετε πολλές συνδρομές, δώστε το αναγνωριστικό συνδρομής που θέλετε να χρησιμοποιήσετε για ανάπτυξη.

        azure account set <YourSubscriptionNameOrId>

3. Μεταβείτε σε λειτουργική μονάδα Azure από διαχειριστή πόρων

        azure config mode arm

   Θα λάβετε επιβεβαίωση της νέας κατάστασης λειτουργίας.

        info:     New mode is arm

4. Εάν δεν έχετε μια υπάρχουσα ομάδα πόρων, δημιουργήστε μια νέα ομάδα πόρων. Εισαγάγετε το όνομα της ομάδας πόρων και θέση που χρειάζεστε για τη λύση.

        azure group create -n ExampleResourceGroup -l "West US"

   Επιστρέφεται μια σύνοψη της νέας ομάδας πόρων.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Για να δημιουργήσετε μια νέα ανάπτυξη για την ομάδα πόρων, εκτελέστε την ακόλουθη εντολή και δώστε τις απαραίτητες παραμέτρους. Οι παράμετροι θα περιλαμβάνει ένα όνομα για την ανάπτυξη, το όνομα του σας ομάδα πόρων, η διαδρομή ή διεύθυνση URL στο πρότυπο που δημιουργήσατε και τυχόν άλλες παραμέτρους που απαιτούνται για το σενάριό σας.

   Έχετε τις ακόλουθες επιλογές για την παροχή τιμές παραμέτρων:

   - Χρήση παραμέτρων ενσωματωμένη και ένα τοπικό πρότυπο.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Χρήση παραμέτρων ενσωματωμένη και μια σύνδεση σε ένα πρότυπο.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Χρησιμοποιήστε ένα αρχείο παραμέτρων.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Όταν έχει αναπτυχθεί την ομάδα των πόρων, θα δείτε μια σύνοψη των ανάπτυξης.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Για πληροφορίες σχετικά με την πιο πρόσφατη ανάπτυξη.

         azure group log show -l ExampleResourceGroup

7. Για λεπτομερείς πληροφορίες σχετικά με τα σφάλματα ανάπτυξης.

         azure group log show -l -v ExampleResourceGroup
