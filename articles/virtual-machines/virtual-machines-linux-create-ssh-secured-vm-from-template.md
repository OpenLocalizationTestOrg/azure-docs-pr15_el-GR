<properties
    pageTitle="Δημιουργήστε μια Εικονική Linux χρησιμοποιώντας ένα πρότυπο Azure | Microsoft Azure"
    description="Δημιουργήστε μια Εικονική Linux Azure χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Δημιουργήστε μια Εικονική Linux χρησιμοποιώντας ένα πρότυπο Azure

Σε αυτό το άρθρο θα μάθετε πώς να αναπτύσσουν γρήγορα μια εικονική μηχανή Linux στην Azure χρησιμοποιώντας ένα πρότυπο Azure.  Το άρθρο απαιτεί τα εξής:

- Azure λογαριασμού ([λάβετε μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)).

- το [Azure CLI](../xplat-cli-install.md) συνδεθεί με `azure login`.

- στη λειτουργία διαχείρισης πόρων Azure _πρέπει να είναι σε_ Azure CLI `azure config mode arm`.

Επίσης γρήγορα, μπορείτε να αναπτύξετε ένα πρότυπο Εικονική Linux χρησιμοποιώντας την [πύλη του Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-command-summary"></a>Γρήγορη εντολή σύνοψη

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Λεπτομερή ανάλυση

Πρότυπα σάς επιτρέπουν να δημιουργήσετε ΣΠΣ σε Azure με ρυθμίσεις που θέλετε να προσαρμόσετε κατά την εκκίνηση, ρυθμίσεις, όπως τα ονόματα των χρηστών και ονόματα. Για αυτό το άρθρο, θα σας ξεκινάτε ένα πρότυπο Azure χρησιμοποιώντας μια Εικονική Ubuntu μαζί με μια ομάδα ασφαλείας δικτύου (NSG) με τη θύρα 22 ανοιχτό για SSH.

Azure πρότυπα διαχείρισης πόρων είναι JSON αρχεία που μπορούν να χρησιμοποιηθούν για απλές μη επαναλαμβανόμενες εργασίες όπως ξεκινάτε μια Εικονική Ubuntu ως ολοκληρωμένο σε αυτό το άρθρο.  Πρότυπα του Azure επίσης μπορεί να χρησιμοποιηθεί για να δημιουργήσετε σύνθετες ρυθμίσεις παραμέτρων Azure από ολόκληρο περιβάλλοντα όπως μιας στοίβας ανάπτυξης δοκιμή, την ή παραγωγής.

## <a name="create-the-linux-vm"></a>Δημιουργήστε την Εικονική Linux

Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να καλέσετε `azure group create` για να δημιουργήσετε μια ομάδα πόρων και να αναπτύξετε μια Εικονική Linux ασφαλές SSH την ίδια στιγμή, χρησιμοποιώντας [αυτό το πρότυπο διαχείρισης πόρων Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Να θυμάστε ότι στο παράδειγμά σας πρέπει να χρησιμοποιήσετε τα ονόματα που είναι μοναδικές στο περιβάλλον σας. Αυτό το παράδειγμα χρησιμοποιεί `myResourceGroup` ως το όνομα της ομάδας πόρων, και `myVM` ως το όνομα Εικονική.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Το αποτέλεσμα θα πρέπει να μοιάζει με το εξής αποτέλεσμα μπλοκ:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Αυτό το παράδειγμα αναπτυχθεί μια Εικονική χρησιμοποιώντας το `--template-uri` παραμέτρου.  Μπορείτε να επίσης λήψη ή δημιουργήστε ένα πρότυπο τοπικά και μεταβιβάζουν του χρησιμοποιώντας το πρότυπο το `--template-file` παραμέτρου με μια διαδρομή για το αρχείο προτύπου ως όρισμα. Το Azure CLI σάς ειδοποιεί για τις παραμέτρους απαιτείται από το πρότυπο.

## <a name="next-steps"></a>Επόμενα βήματα

Αναζήτηση στη [συλλογή "Πρότυπα"](https://azure.microsoft.com/documentation/templates/) για να ανακαλύψετε τι πλαισίων εφαρμογής για την ανάπτυξη Επόμενο.
