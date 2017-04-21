<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Χρήση του Azure CLI με Azure διαχείριση πόρων (ARM)

Να μπορέσετε να χρησιμοποιήσετε το Azure CLI με εντολές για τη διαχείριση πόρων και πρότυπα για την ανάπτυξη του Azure πόρους και φόρτους εργασίας χρησιμοποιώντας τις ομάδες πόρων, θα χρειαστεί ένα λογαριασμό με Azure (φυσικά). Εάν δεν έχετε ένα λογαριασμό, μπορείτε να λάβετε μια [δωρεάν δοκιμαστική έκδοση του Azure εδώ](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Εάν δεν έχετε ήδη ένα λογαριασμό Azure, αλλά έχετε συνδρομή σε συνδρομή στο MSDN, μπορείτε να λάβετε δωρεάν Azure πιστώσεων, ενεργοποιώντας το [MSDN πλεονεκτήματα συνδρομητών εδώ](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) --ή μπορείτε να χρησιμοποιήσετε το δωρεάν λογαριασμό. Είτε θα λειτουργεί για πρόσβαση Azure.

### <a name="step-1-verify-the-azure-cli-version"></a>Βήμα 1: Επαλήθευση της έκδοσης Azure CLI

Για να χρησιμοποιήσετε Azure CLI οπωσδήποτε εντολές και τα πρότυπα ARM, πρέπει να έχετε τουλάχιστον την έκδοση 0.8.17. Για να επαληθεύσετε τη δική σας έκδοση, πληκτρολογήστε `azure --version`. Θα πρέπει να δείτε κάτι παρόμοιο με:

    $ azure --version
    0.8.17 (node: 0.10.25)

Εάν πρέπει να ενημερώσετε την έκδοση του Azure CLI, ανατρέξτε στο θέμα [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Βήμα 2: Επαλήθευση χρησιμοποιείτε έναν εταιρικό ή σχολικό ταυτότητας με Azure

Μπορείτε να χρησιμοποιήσετε τη λειτουργία εντολή ARM μόνο εάν χρησιμοποιείτε ένα [μισθωτή του Azure Active Directory](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) ή ένα [Κύριο όνομα υπηρεσίας](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Αυτοί ονομάζονται επίσης *αναγνωριστικά οργανισμού*.)

Για να δείτε εάν έχετε ένα, συνδεθείτε στο, πληκτρολογώντας `azure login` και χρησιμοποιείτε το τηλέφωνο της εργασίας ή σχολείου όνομα χρήστη και τον κωδικό πρόσβασης όταν σας ζητηθεί. Εάν έχετε ένα, θα πρέπει να βλέπετε κάπως έτσι:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Εάν δεν βλέπετε αυτό, πρέπει να δημιουργήσετε ένα νέο μισθωτή (ή μια υπηρεσία κεφαλαίου) με την ταυτότητά σας λογαριασμό Microsoft. (Αυτό συμβαίνει συχνά με προσωπικά συνδρομές στο MSDN ή δωρεάν δοκιμαστικές συνδρομές.) Για να δημιουργήσετε έναν εταιρικό ή σχολικό αναγνωριστικό από το Azure λογαριασμό που δημιουργήθηκε με το αναγνωριστικό Microsoft, ανατρέξτε στο θέμα [συσχετισμός ενός καταλόγου Azure AD με μια νέα συνδρομή Azure](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Εάν πιστεύετε ότι θα πρέπει να έχετε ήδη ένα αναγνωριστικό οργανισμού, ίσως χρειαστεί να επικοινωνήσετε με το άτομο που δημιούργησε το λογαριασμό για εσάς.

### <a name="step-3-choose-your-azure-subscription"></a>Βήμα 3: Επιλέξτε τη συνδρομή σας στο Azure

Εάν έχετε μόνο μία συνδρομή στο λογαριασμό σας στο Azure, Azure CLI συσχετίζεται με αυτήν τη συνδρομή από προεπιλογή. Εάν έχετε περισσότερες από μία συνδρομές, πρέπει να επιλέξετε τη συνδρομή που θέλετε να χρησιμοποιήσετε, πληκτρολογώντας `azure account set <subscription id or name> true` όπου _συνδρομή αναγνωριστικό ή όνομα_ είναι το αναγνωριστικό συνδρομής ή στο όνομα της συνδρομής που θέλετε να εργαστείτε με με την τρέχουσα περίοδο λειτουργίας.

Θα πρέπει να δείτε κάτι παρόμοιο με το εξής:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Βήμα 4: Τοποθετήστε το Azure CLI στην κατάσταση λειτουργίας ARM

Για να χρησιμοποιήσετε τη λειτουργία διαχείρισης πόρων Azure (ARM) με το Azure CLI, πληκτρολογήστε `azure config mode arm`. Θα πρέπει να δείτε κάτι παρόμοιο με το εξής:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Μπορείτε να μεταβείτε ξανά για να χρησιμοποιήσετε τις εντολές της διαχείρισης Azure service, πληκτρολογώντας `azure config mode asm`.
