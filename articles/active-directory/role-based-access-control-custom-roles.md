<properties
    pageTitle="Προσαρμοσμένη τους ρόλους στο Azure RBAC | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ορίσετε μια προσαρμοσμένη ρόλων με τον έλεγχο πρόσβασης Azure Role-Based για πιο ακριβή διαχείρισης των ταυτοτήτων στο Azure τη συνδρομή σας."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Προσαρμοσμένη τους ρόλους στο Azure RBAC


Δημιουργήστε έναν προσαρμοσμένο ρόλο στο στοιχείο ελέγχου πρόσβασης Azure Role-Based (RBAC), εάν κανένα από τα ενσωματωμένα ρόλους ανταποκρίνεται στις ανάγκες σας συγκεκριμένα πρόσβασης. Προσαρμοσμένοι ρόλοι μπορούν να δημιουργηθούν με χρήση [Του PowerShell Azure](role-based-access-control-manage-access-powershell.md), [Azure περιβάλλον γραμμής εντολών](role-based-access-control-manage-access-azure-cli.md) (CLI) και το [REST API](role-based-access-control-manage-access-rest.md). Όπως ενσωματωμένη ρόλους, μπορούν να αντιστοιχιστούν προσαρμοσμένο τους ρόλους για τους χρήστες, ομάδες και εφαρμογές σε συνδρομή, ομάδα πόρων και πόρων εύρους. Προσαρμοσμένοι ρόλοι είναι αποθηκευμένες σε ένα μισθωτή του Azure AD και μπορεί να είναι κοινόχρηστο σε όλες τις συνδρομές που χρησιμοποιούν αυτόν το μισθωτή ως κατάλογο Azure AD για το subsciption.

Ακολουθεί ένα παράδειγμα ενός προσαρμοσμένου ρόλου για την παρακολούθηση και την επανεκκίνηση του εικονικές μηχανές:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Ενέργειες
Η ιδιότητα **Ενέργειες** ενός προσαρμοσμένου ρόλου καθορίζει τις λειτουργίες Azure στο οποίο ο ρόλος εκχωρεί πρόσβαση. Πρόκειται για μια συλλογή από τη λειτουργία συμβολοσειρές που προσδιορίζουν δυνατότητα ασφάλισης λειτουργίες από υπηρεσίες παροχής Azure πόρων. Η λειτουργία συμβολοσειρές που περιέχουν χαρακτήρες μπαλαντέρ (\*) εκχωρήσετε πρόσβαση σε όλες τις εργασίες που ταιριάζουν με τη συμβολοσειρά λειτουργία. Για παράδειγμα:

-   `*/read`επιχορηγήσεις πρόσβαση για ανάγνωση λειτουργίες για όλους τους τύπους πόρων όλες τις υπηρεσίες παροχής Azure πόρων.
-   `Microsoft.Network/*/read`επιχορηγήσεις πρόσβαση για ανάγνωση λειτουργίες για όλοι οι τύποι πόρων στην υπηρεσία παροχής πόρων Microsoft.Network του Azure.
-   `Microsoft.Compute/virtualMachines/*`επιχορηγήσεις να αποκτήσει πρόσβαση σε όλες τις εργασίες της εικονικές μηχανές και τους θυγατρικούς τύπους πόρων.
-   `Microsoft.Web/sites/restart/Action`επιχορηγήσεις πρόσβαση για να επανεκκινήσετε τοποθεσίες Web που διαθέτετε.

Χρήση `Get-AzureRmProviderOperation` (σε PowerShell) ή `azure provider operations show` (στο Azure CLI) σε εργασίες καταλόγου των υπηρεσιών παροχής Azure πόρων. Μπορείτε επίσης να χρησιμοποιήσετε αυτές τις εντολές για να επαληθεύσετε ότι μια συμβολοσειρά η λειτουργία είναι έγκυρη και για να αναπτύξετε τις συμβολοσειρές λειτουργία μπαλαντέρ.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![Στιγμιότυπο του PowerShell - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | Λειτουργία FT, OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI στιγμιότυπο οθόνης - azure παροχής λειτουργίες Εμφάνιση "Microsoft.Compute/virtualMachines/\*/action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Χρησιμοποιήστε την ιδιότητα **NotActions** , εάν το σύνολο των λειτουργιών που θέλετε να επιτρέψετε ορίζεται πιο εύκολα εξαιρώντας περιορισμένες λειτουργίες. Η access εκχωρήσει έναν προσαρμοσμένο ρόλο υπολογίζεται αφαιρώντας τις λειτουργίες **NotActions** από τις λειτουργίες **Ενέργειες** .

> [AZURE.NOTE] Εάν ένας χρήστης έχει αντιστοιχιστεί σε ένα ρόλο που αποκλείει τους μια λειτουργία σε **NotActions**και έχει ανατεθεί μια δεύτερη ρόλο που εκχωρεί πρόσβαση την ίδια λειτουργία, ο χρήστης θα επιτρέπεται για να εκτελέσετε αυτήν τη λειτουργία. **NotActions** δεν είναι ένας κανόνας απόρριψη – είναι απλώς ένας εύκολος τρόπος για να δημιουργήσετε ένα σύνολο λειτουργίες που επιτρέπονται όταν συγκεκριμένες εργασίες που πρέπει να εξαιρούνται.

## <a name="assignablescopes"></a>AssignableScopes
Η ιδιότητα **AssignableScopes** του προσαρμοσμένου ρόλου Καθορίζει το εύρος (συνδρομές, ομάδες πόρων ή πόρων) εντός των οποίων το προσαρμοσμένο ρόλο είναι διαθέσιμη για την ανάθεση. Μπορείτε να κάνετε το προσαρμοσμένο ρόλο διαθέσιμη για ανάθεση σε μόνο τους συνδρομές ή τις ομάδες πόρων που απαιτούν την και δεν δευτερεύουσα αλληλογραφία εμπειρία χρήστη για τα υπόλοιπα των συνδρομών ή των ομάδων πόρων.

Έγκυρες εκχωρήσιμο εύρους παραδείγματα:

-   "/ συνδρομές/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ συνδρομές/e91d47c4-76f3-4271-a796-21b4ecfe3624" - καθιστά το ρόλο διαθέσιμο για ανάθεση σε δύο συνδρομές.
-   "/ συνδρομές/c276fc76-9cd4-44c9-99a7-4fd71546436e" - καθιστά το ρόλο διαθέσιμο για ανάθεση σε μια μεμονωμένη εγγραφή.
-  "/ συνδρομές/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/δικτύου" - είναι διαθέσιμες για μια ανάθεση ρόλου μόνο στην ομάδα πόρων δικτύου.

> [AZURE.NOTE] Πρέπει να χρησιμοποιήσετε τουλάχιστον μία συνδρομή "," ομάδα πόρων "ή" αναγνωριστικό πόρου.

## <a name="custom-roles-access-control"></a>Έλεγχος πρόσβασης προσαρμοσμένο ρόλων
Η ιδιότητα **AssignableScopes** του προσαρμοσμένου ρόλου ελέγχει επίσης που μπορεί να προβάλετε, να τροποποιήσετε και να διαγράψετε το ρόλο.

- Ποιος μπορεί να δημιουργήσει ένα προσαρμοσμένο ρόλο;
    Οι κάτοχοι (και οι διαχειριστές χρηστών πρόσβασης) συνδρομών, ομάδες πόρων, και τους πόρους μπορείτε να δημιουργήσετε προσαρμοσμένες ρόλους για χρήση σε αυτές τις επιλογές εύρους.
    Ο χρήστης που δημιουργεί το ρόλο που πρέπει να έχετε τη δυνατότητα να εκτελέσετε `Microsoft.Authorization/roleDefinition/write` πράξη σε όλα τα **AssignableScopes** του ρόλου.

- Ποιος μπορεί να τροποποιήσετε έναν προσαρμοσμένο ρόλο;
    Οι κάτοχοι (και οι διαχειριστές χρηστών πρόσβασης) συνδρομών, ομάδες πόρων και τους πόρους, να τροποποιήσετε προσαρμοσμένες ρόλους σε αυτά τα πεδία. Οι χρήστες πρέπει να έχετε τη δυνατότητα να εκτελέσετε το `Microsoft.Authorization/roleDefinition/write` πράξη σε όλα τα **AssignableScopes** ενός προσαρμοσμένου ρόλου.

- Ποιος μπορεί να δει Προσαρμοσμένοι ρόλοι;
    Όλοι οι ενσωματωμένες ρόλοι στο Azure RBAC να επιτρέπεται η προβολή τους ρόλους που είναι διαθέσιμες για την ανάθεση. Οι χρήστες που μπορούν να εκτελούν το `Microsoft.Authorization/roleDefinition/read` λειτουργία σε ένα εύρος μπορούν να προβάλουν τους ρόλους RBAC που είναι διαθέσιμες για ανάθεση σε συγκεκριμένο εύρος.

## <a name="see-also"></a>Δείτε επίσης
- [Έλεγχος πρόσβασης βάσει ρόλων](role-based-access-control-configure.md): γρήγορα αποτελέσματα με το RBAC στην πύλη του Azure.
- Μάθετε πώς μπορείτε να διαχειριστείτε την πρόσβαση με:
    - [PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST API](role-based-access-control-manage-access-rest.md)
- [Ενσωματωμένη ρόλους](role-based-access-built-in-roles.md): Μάθετε λεπτομέρειες σχετικά με τους ρόλους που παρέχονται τυπική στο RBAC.
