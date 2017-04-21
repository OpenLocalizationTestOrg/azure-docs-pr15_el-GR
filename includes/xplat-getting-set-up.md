<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Χρήση του Azure CLI

Ακολουθήστε τα παρακάτω βήματα θα σας βοηθήσει να χρησιμοποιήσετε Azure CLI εύκολα με την πιο πρόσφατη έκδοση και την κατάλληλη συνδρομή. Εάν πρέπει να εγκαταστήσετε το Azure CLI και συνδεθείτε πρώτα στο λογαριασμό σας, ανατρέξτε στο θέμα [περιβάλλον γραμμής εντολών Azure (Azure CLI)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Βήμα 1: Ενημέρωση Azure CLI έκδοση

Για να χρησιμοποιήσετε Azure CLI οπωσδήποτε εντολές με την υπηρεσία διαχείρισης λειτουργία, θα πρέπει να έχετε μια πρόσφατη έκδοση εάν είναι δυνατό. Για να επαληθεύσετε τη δική σας έκδοση, πληκτρολογήστε `azure --version`. Θα πρέπει να δείτε κάτι παρόμοιο με:

    $ azure --version
    0.8.17 (node: 0.10.25)

Εάν θέλετε να ενημερώσετε την έκδοση του Azure CLI, ανατρέξτε στο θέμα [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Βήμα 2: Ρύθμιση της λογαριασμός Azure και συνδρομή

Αφού έχετε συνδέσει το Azure CLI με το λογαριασμό που θέλετε να χρησιμοποιήσετε, ενδέχεται να έχετε περισσότερες από μία συνδρομές. Εάν το κάνετε, πρέπει να εξετάσετε τις συνδρομές που είναι διαθέσιμες για το λογαριασμό σας, πληκτρολογώντας `azure account list`, και, στη συνέχεια, επιλέξτε τη συνδρομή που θέλετε να χρησιμοποιήσετε, πληκτρολογώντας `azure account set <subscription id or name> true` όπου _συνδρομή αναγνωριστικό ή όνομα_ είναι το αναγνωριστικό συνδρομής ή στο όνομα της συνδρομής που θέλετε να εργαστείτε με με την τρέχουσα περίοδο λειτουργίας. Θα πρέπει να δείτε κάτι παρόμοιο με το εξής:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Εάν δεν έχετε ήδη ένα λογαριασμό Azure, αλλά έχετε συνδρομή σε συνδρομή στο MSDN, μπορείτε να λάβετε δωρεάν Azure πιστώσεων, ενεργοποιώντας το [MSDN πλεονεκτήματα συνδρομητών εδώ](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) --ή μπορείτε να χρησιμοποιήσετε το δωρεάν λογαριασμό. Είτε θα λειτουργεί για πρόσβαση Azure.
