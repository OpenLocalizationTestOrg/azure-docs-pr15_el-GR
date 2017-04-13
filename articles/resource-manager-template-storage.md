<properties
   pageTitle="Διαχείριση πόρων πρότυπο για το χώρο αποθήκευσης | Microsoft Azure"
   description="Εμφανίζει το σχήμα από διαχειριστή πόρων για την ανάπτυξη του χώρου αποθήκευσης λογαριασμούς μέσω ενός προτύπου."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Διάταξη προτύπου λογαριασμού χώρου αποθήκευσης

Δημιουργεί ένα λογαριασμό του χώρου αποθήκευσης.

## <a name="schema-format"></a>Μορφοποίηση σχήματος

Για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης, προσθέστε την ακόλουθη διάταξη στην ενότητα πόροι του προτύπου σας.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Τιμές

Στους πίνακες που ακολουθούν περιγράφουν τις τιμές πρέπει να ορίσετε στο σχήμα.

| Όνομα | Τιμή |
| ---- | ---- |
| Τύπος | Απαρίθμηση<br />Απαιτείται<br />**Microsoft.Storage/storageAccounts**<br /><br />Ο τύπος πόρου για να δημιουργήσετε. |
| apiVersion | Απαρίθμηση<br />Απαιτείται<br />**2015-06-15** ή **2015-05-01-preview**<br /><br />Η έκδοση API για να χρησιμοποιήσετε για τη δημιουργία του πόρου. | 
| Όνομα | Συμβολοσειρά<br />Απαιτείται<br />Μεταξύ 3 και 24 χαρακτήρες, μόνο αριθμούς και πεζά γράμματα.<br /><br />Το όνομα του λογαριασμού χώρου αποθήκευσης για τη δημιουργία. Το όνομα πρέπει να είναι μοναδικό σε όλους Azure. Μπορείτε να χρησιμοποιήσετε τη συνάρτηση [uniqueString](resource-group-template-functions.md#uniquestring) με κανόνες ονοματοθεσίας σας, όπως φαίνεται στο παρακάτω παράδειγμα. |
| θέση | Συμβολοσειρά<br />Απαιτείται<br />Μια περιοχή που υποστηρίζει λογαριασμούς χώρου αποθήκευσης. Για να προσδιορίσετε έγκυρη περιοχές, ανατρέξτε στο θέμα [υποστηρίζονται περιοχές](resource-manager-supported-services.md#supported-regions).<br /><br />Η περιοχή για τη φιλοξενία του λογαριασμού χώρου αποθήκευσης. |
| Ιδιότητες | Αντικείμενο<br />Απαιτείται<br />[Ιδιότητες αντικειμένου](#properties)<br /><br />Αντικείμενο που καθορίζει τον τύπο του λογαριασμού χώρου αποθήκευσης για τη δημιουργία. |

<a id="properties" />
### <a name="properties-object"></a>Ιδιότητες αντικειμένου

| Όνομα | Τιμή |
| ---- | ---- | 
| accountType | Συμβολοσειρά<br />Απαιτείται<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**ή **Premium_LRS**<br /><br />Τον τύπο του λογαριασμού χώρου αποθήκευσης. Τις επιτρεπόμενες τιμές που αντιστοιχούν σε τυπική τοπικά πλεονάζοντα, βασική ζώνη πλεονάζοντα, τυπική παν-πλεοναζουσών, τυπική παν πλεοναζουσών πρόσβαση για ανάγνωση και Premium τοπικά της πλεονάζοντα. Για πληροφορίες σχετικά με αυτούς τους τύπους λογαριασμού, ανατρέξτε στο θέμα [αποθήκευσης Azure αναπαραγωγής](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Παραδείγματα

Το παρακάτω παράδειγμα αναπτύσσει έναν τυπικό τοπικά πλεοναζόντων λογαριασμό χώρου αποθήκευσης με ένα μοναδικό όνομα με βάση το αναγνωριστικό ομάδας πόρων.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Γρήγορη έναρξη προτύπων

Υπάρχουν πολλά πρότυπα γρήγορη έναρξη που περιλαμβάνουν ένα λογαριασμό του χώρου αποθήκευσης. Τα παρακάτω πρότυπα απεικονίζουν ορισμένα κοινά σενάρια:

- [Δημιουργία λογαριασμού τυπική χώρου αποθήκευσης](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Απλή υλοποίηση της Εικονική μηχανή των Windows](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Απλή υλοποίηση της Εικονική μηχανή Linux](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Δημιουργήστε ένα προφίλ CDN, ένα τελικό σημείο CDN με ένα λογαριασμό του χώρου αποθήκευσης ως προέλευσης](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Δημιουργήστε μια υψηλή συστοιχία του SharePoint Availabilty με 9 ΣΠΣ χρησιμοποιώντας την επέκταση DSC Powershell](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Απλή ανάπτυξη του 5 κόμβο ασφαλούς υπηρεσιών συμπλέγματος υφάσματος με WAD με δυνατότητα](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Δημιουργήστε μια εικονική μηχανή από μια εικόνα των Windows με 4 δίσκων κενή δεδομένων](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Επόμενα βήματα

- Για γενικές πληροφορίες σχετικά με το χώρο αποθήκευσης, ανατρέξτε στο θέμα [Εισαγωγή στο Microsoft Azure αποθήκευσης](./storage/storage-introduction.md).
- Για παράδειγμα πρότυπα που χρησιμοποιούν το νέο λογαριασμό του χώρου αποθήκευσης με έναν εικονικό υπολογιστή, ανατρέξτε στο θέμα [Ανάπτυξη μια απλή Εικονική Linux](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) ή [ανάπτυξη μιας απλής Εικονική των Windows](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
