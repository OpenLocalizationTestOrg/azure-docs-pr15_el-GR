<properties
   pageTitle="Επισκόπηση της υπηρεσίας παροχής πόρων δικτύου | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με τη νέα υπηρεσία παροχής πόρων δικτύου στη Διαχείριση πόρων Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Υπηρεσία παροχής πόρων δικτύου
Μια ανάγκη υποστήριξη στο σημερινή επιχειρήσεις επιτυχίας, είναι τη δυνατότητα να δημιουργήσετε και να διαχειριστείτε υπόψη εφαρμογές δικτύου μεγάλης κλίμακας με τρόπο ευέλικτη, ευέλικτη, ασφαλούς και επαναλαμβανόμενης. Διαχείριση Azure πόρων (ARM) σάς επιτρέπει να δημιουργήσετε αυτές τις εφαρμογές, ως μια ενιαία συλλογή των πόρων σε ομάδες πόρων. Οι πόροι πραγματοποιείται στις διάφορες υπηρεσίες παροχής πόρου στην περιοχή ARM.

Azure διαχείριση πόρων βασίζεται σε διαφορετικό πόρο υπηρεσίες παροχής για την παροχή πρόσβασης για τους πόρους σας. Υπάρχουν τρεις υπηρεσίες παροχής κύριο πόρων: δικτύου, αποθήκευσης και υπολογισμού. Αυτό το έγγραφο περιγράφει τα χαρακτηριστικά και τα πλεονεκτήματα της υπηρεσίας παροχής πόρων δικτύου, όπως:

- **Μετα-δεδομένων** – μπορείτε να προσθέσετε πληροφορίες σε πόρους που χρησιμοποιούν ετικέτες. Αυτές οι ετικέτες μπορεί να χρησιμοποιηθεί για την παρακολούθηση χρήσης πόρων σε ομάδες πόρων και συνδρομές.
- **Μεγαλύτερο έλεγχο του δικτύου σας** - δικτύου πόροι είναι γενικά συνδυασμό και μπορείτε να ελέγχετε τους με ένα πιο λεπτομερές τρόπο. Αυτό σημαίνει ότι έχετε περισσότερη ευελιξία στη διαχείριση των πόρων κοινωνικής δικτύωσης.
- **Ταχύτερη ρύθμιση παραμέτρων** - επειδή πόρους δικτύου είναι ελεύθερα συνδεδεμένων μπορείτε να δημιουργήσετε και να οργανώσετε πόρους δικτύου παράλληλα. Αυτό έχει μειωθεί σημαντικά ρύθμισης παραμέτρων χρόνου.
- **Έλεγχος πρόσβασης βάσει ρόλων** - RBAC παρέχει Προεπιλεγμένοι ρόλοι, με συγκεκριμένες ασφαλείας εύρος, εκτός από επιτρέπει τη δημιουργία προσαρμοσμένου τους ρόλους για τη Διαχείριση ασφαλούς.
- **Πιο εύκολη διαχείριση και την ανάπτυξη** - είναι ευκολότερο να αναπτύξετε και να διαχειριστείτε τις εφαρμογές δεδομένου ότι μπορείτε να μπορεί να δημιουργήσετε μια στοίβα ολόκληρη την εφαρμογή ως μια ενιαία συλλογή των πόρων σε μια ομάδα πόρων. Γρήγορη και για να αναπτύξετε, επειδή μπορείτε να αναπτύξετε, απλώς παρέχοντας ένα πρότυπο φορτίου JSON.
- **Γρήγορη προσαρμογή** - μπορείτε να χρησιμοποιήσετε πρότυπα δηλωτικό στυλ για να ενεργοποιήσετε την επαναλαμβανόμενη και γρήγορη προσαρμογή των αναπτύξεις.
- **Επαναλαμβανόμενο προσαρμογής** - μπορείτε να χρησιμοποιήσετε πρότυπα δηλωτικό στυλ για να ενεργοποιήσετε την επαναλαμβανόμενη και γρήγορη προσαρμογή των αναπτύξεις.
- **Διασυνδέσεις διαχείρισης** - μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις παρακάτω διασυνδέσεις για να διαχειριστείτε τους πόρους σας:
    - ΥΠΌΛΟΙΠΟ βάσει API
    - PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure CLI
    - Προεπισκόπηση πύλη
    - Γλώσσα προτύπου ARM

## <a name="network-resources"></a>Πόροι δικτύου
Τώρα μπορείτε να διαχειριστείτε πόρους δικτύου ανεξάρτητα από αυτό, αντί να χρειάζεται τις όλες διαχειριζόμενων μέσω ενός πόρου μόνο υπολογισμού (μια εικονική μηχανή). Αυτό εξασφαλίζει υψηλότερο βαθμό ευελιξία και ευελιξία στη τη σύνταξη μιας υποδομής σύνθετη και μεγάλη κλίμακα σε μια ομάδα πόρων.

Μια εννοιολογική άποψη του μια ανάπτυξη του δείγματος που αφορούν μιας εφαρμογής πολλών επιπέδων παρουσιάζεται παρακάτω. Κάθε πόρο που μπορείτε να δείτε, όπως NIC, δημόσιες διευθύνσεις IP και ΣΠΣ, μπορείτε να διαχειριστείτε ανεξάρτητα.

![Μοντέλο πόρων δικτύου](./media/resource-groups-networking/Figure2.png)

Κάθε πόρο περιέχει ένα κοινό σύνολο ιδιοτήτων και ορίστε την ιδιότητα μεμονωμένα. Οι κοινές ιδιότητες είναι:

|Ιδιότητα|Περιγραφή|Δείγματα τιμών|
|---|---|---|
|**Όνομα**|Όνομα πόρου μοναδικών τιμών. Κάθε τύπος πόρων έχει τη δική τους περιορισμούς ονομάτων.|PIP01, VM01, NIC01|
|**θέση**|Azure περιοχή στην οποία βρίσκεται ο πόρος|westus, eastus|
|**αναγνωριστικό**|Μοναδικό αναγνωριστικό URI με βάση|/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Μπορείτε να ελέγξετε τις μεμονωμένες ιδιότητες πόρων στις παρακάτω ενότητες.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Διασυνδέσεις διαχείρισης
Μπορείτε να διαχειριστείτε το Azure κοινωνικής δικτύωσης πόρων με τους διαφορετικούς διασυνδέσεων. Σε αυτό το έγγραφο θα σας εστιάζει στην έλκει από αυτές τις διασυνδέσεις: REST API και πρότυπα.

### <a name="rest-api"></a>REST API
Όπως προαναφέρθηκε, μπορείτε να διαχειριστείτε πόρους δικτύου μέσω μια ποικιλία διασυνδέσεων, συμπεριλαμβανομένων REST API, .NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, πύλη Azure και πρότυπα.

Το Rest API συμμορφώνονται με την προδιαγραφή πρωτοκόλλου HTTP 1.1. Η γενική δομή URI του API παρουσιάζεται παρακάτω:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

Και οι παράμετροι σε άγκιστρα αντιπροσωπεύει τα ακόλουθα στοιχεία:

- **αναγνωριστικό συνδρομής** - αναγνωριστικό Azure τη συνδρομή σας.
- **χώρος ονομάτων υπηρεσίας παροχής πόρων** - χώρος ονομάτων για την υπηρεσία παροχής που χρησιμοποιείται. Η τιμή για την υπηρεσία παροχής πόρων δικτύου είναι *Microsoft.Network*.
- **όνομα περιοχής** - το όνομα της περιοχής Azure

Τις παρακάτω μεθόδους HTTP υποστηρίζονται κατά την πραγματοποίηση κλήσεων για να το REST API:

- **ΤΟΠΟΘΈΤΗΣΗ** - χρησιμοποιούνται για τη δημιουργία ενός πόρου ενός συγκεκριμένου τύπου, τροποποίησης μιας ιδιότητας πόρου ή να αλλάξετε μια συσχέτιση μεταξύ τους πόρους.
- **ΛΉΨΗ** - χρησιμοποιείται για την ανάκτηση πληροφοριών για έναν πόρο προμήθεια του φακέλου.
- **ΔΙΑΓΡΑΦΉ** - χρησιμοποιείται για τη διαγραφή ενός υπάρχοντος πόρου.

Τόσο το αίτησης και απόκρισης συμφωνεί με μια μορφή φορτίου JSON. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Azure APIs διαχείρισης πόρων](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>Γλώσσα προτύπου ARM
Εκτός από τη διαχείριση των πόρων imperatively (μέσω API ή SDK), μπορείτε επίσης να χρησιμοποιήσετε ένα στυλ δηλωτικό προγραμματισμού για να δημιουργήσετε και να διαχειριστείτε πόρους δικτύου, χρησιμοποιώντας τη γλώσσα προτύπου ARM.

Μια αναπαράσταση δείγμα προτύπου παρέχονται παρακάτω

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Το πρότυπο είναι κυρίως μια περιγραφή JSON τους πόρους και τις τιμές παρουσία εισαχθεί μέσω παραμέτρους. Το παρακάτω παράδειγμα μπορεί να χρησιμοποιηθεί για να δημιουργήσετε ένα εικονικό δίκτυο με 2 δευτερεύοντα δίκτυα.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Έχετε τη δυνατότητα να παρέχουν τις τιμές παραμέτρων με μη αυτόματο τρόπο, όταν χρησιμοποιείτε ένα πρότυπο ή μπορείτε να χρησιμοποιήσετε ένα αρχείο παραμέτρων. Το παρακάτω παράδειγμα εμφανίζει ένα σύνολο πιθανές τιμές παραμέτρων που θα χρησιμοποιηθεί με το πρότυπο παραπάνω:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Τα βασικά πλεονεκτήματα της χρήσης πρότυπα είναι οι εξής:

- Μπορείτε να δημιουργήσετε μια σύνθετη υποδομή σε μια ομάδα πόρων με δηλωτικό στυλ. Το ενορχήστρωσης της δημιουργίας τους πόρους, συμπεριλαμβανομένης της διαχείρισης εξάρτηση, γίνεται από ARM.
- Η υποδομή μπορούν να δημιουργηθούν με επαναλαμβανόμενη τρόπο σε διάφορες περιοχές και μέσα σε μια περιοχή απλώς να αλλάξετε τις παραμέτρους.
- Το στυλ δηλωτικό οδηγεί σε μικρότερη χρόνου παράδοσης σε δημιουργία τα πρότυπα και την κατάργηση της υποδομής του.

Για δείγματα προτύπων, ανατρέξτε στο θέμα [πρότυπα Azure γρήγορη έναρξη](https://github.com/Azure/azure-quickstart-templates).

Για περισσότερες πληροφορίες σχετικά με τη γλώσσα του προτύπου ARM, ανατρέξτε στο θέμα [Azure γλώσσα προτύπου για τη διαχείριση πόρων](../resource-group-authoring-templates.md).

Το παραπάνω δείγμα προτύπου χρησιμοποιεί το εικονικό δίκτυο και υποδικτύου πόρους. Υπάρχουν άλλοι πόροι δικτύου που μπορείτε να χρησιμοποιήσετε, όπως αναφέρονται παρακάτω:

### <a name="using-a-template"></a>Χρησιμοποιώντας ένα πρότυπο

Μπορείτε να αναπτύξετε τις υπηρεσίες σε Azure από ένα πρότυπο με χρήση του PowerShell, AzureCLI, ή με ένα κλικ για την ανάπτυξη από GitHub εκτέλεση. Για να αναπτύξετε τις υπηρεσίες από ένα πρότυπο σε GitHub, εκτελέστε τα ακόλουθα βήματα:

1. Ανοίξτε το αρχείο template3 από GitHub. Ως παράδειγμα, ανοίξτε το [δίκτυο εικονικές με δύο δευτερευόντων δικτύων](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Κάντε κλικ στην **Ανάπτυξη να Azure**και κατόπιν συνδεθείτε πύλη του Azure με τα διαπιστευτήριά σας.
3. Επαλήθευση του προτύπου και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.
4. Κάντε κλικ στην επιλογή **Επεξεργασία των παραμέτρων** και επιλέξτε μια θέση, όπως *Δυτική η.π.α.*, για τα vnet και δευτερεύοντα δίκτυα.
5. Εάν είναι απαραίτητο, αλλάξτε τις παραμέτρους **ADDRESSPREFIX** και **SUBNETPREFIX** και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.
6. Κάντε κλικ στην επιλογή **, επιλέξτε μια ομάδα πόρων** και, στη συνέχεια, κάντε κλικ στην ομάδα πόρων που θέλετε να προσθέσετε την vnet και δευτερεύοντα δίκτυα να. Εναλλακτικά, μπορείτε να δημιουργήσετε μια νέα ομάδα πόρων κάνοντας κλικ στην επιλογή **ή δημιουργία νέου**.
3. Κάντε κλικ στην επιλογή **Δημιουργία**. Παρατηρήστε ότι το πλακίδιο εμφανίζει **ανάπτυξης προμήθεια του προτύπου**. Μετά την ολοκλήρωση της ανάπτυξης, θα δείτε μια παρόμοια με μία οθόνη παρακάτω.

![Δείγμα προτύπου ανάπτυξης](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Επόμενα βήματα

[Azure γλώσσα προτύπου για τη διαχείριση πόρων](../resource-group-authoring-templates.md)

[Azure δικτύωση – που χρησιμοποιούνται ευρέως προτύπων](https://github.com/Azure/azure-quickstart-templates)

[Τον υπολογισμό της υπηρεσίας παροχής πόρων](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure Επισκόπηση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md)
