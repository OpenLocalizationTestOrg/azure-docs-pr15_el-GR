<properties
    pageTitle="Διαχείριση έλεγχο πρόσβασης βάσει ρόλων (RBAC) με το Azure CLI | Microsoft Azure"
    description="Μάθετε πώς να διαχειρίζεστε βάσει ρόλων έλεγχο πρόσβασης (RBAC) με το Azure περιβάλλον γραμμής εντολών με παραθέτει τους ρόλους και ενέργειες ρόλο και με την εκχώρηση ρόλων για τη συνδρομή και εφαρμογή πεδίων."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Διαχείριση έλεγχος πρόσβασης βάσει ρόλων με το περιβάλλον γραμμής εντολών του Azure

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

Μπορείτε να χρησιμοποιήσετε έλεγχος πρόσβασης βάσει ρόλων (RBAC) στην πύλη του Azure και Azure API διαχείρισης πόρων για να διαχειριστείτε την πρόσβαση για τη συνδρομή σας και τους πόρους σε λεπτομερή επίπεδο. Με αυτήν τη δυνατότητα, μπορείτε να εκχωρήσετε πρόσβαση για χρήστες, ομάδες ή αρχές υπηρεσίας καταλόγου Active Directory, εκχώρηση ρόλων ορισμένες τους σε μια ειδική εμβέλεια.

Μπορείτε να χρησιμοποιήσετε το Azure περιβάλλον γραμμής εντολών (CLI) για τη Διαχείριση RBAC, πρέπει να έχετε τα εξής:

- Azure CLI έκδοση 0.8.8 ή νεότερη έκδοση. Για να εγκαταστήσετε την πιο πρόσφατη έκδοση και να συσχετίσετε με τη συνδρομή σας στο Azure, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md).
- Διαχείριση πόρων Azure στο Azure CLI. Μεταβείτε στο [χρησιμοποιώντας το Azure CLI με τη διαχείριση πόρων](../xplat-cli-azure-resource-manager.md) για περισσότερες λεπτομέρειες.

## <a name="list-roles"></a>Λίστα ρόλων

### <a name="list-all-available-roles"></a>Λίστα όλων των ρόλων διαθέσιμη
Για να δείτε όλους τους ρόλους διαθέσιμες, χρησιμοποιήστε:

        azure role list

Το παρακάτω παράδειγμα εμφανίζει τη λίστα με *όλους τους ρόλους διαθέσιμη*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Στιγμιότυπο οθόνης της γραμμής εντολών - λίστα azure ρόλο - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Ενέργειες λίστας από ένα ρόλο
Για να δημιουργήσετε μια λίστα ενεργειών που οφείλονται σε ένα ρόλο, χρησιμοποιήστε:

    azure role show "<role name>"

Το παρακάτω παράδειγμα εμφανίζει τις ενέργειες των ρόλων *συμβολής* και *Συμβολής εικονική μηχανή* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Στιγμιότυπο οθόνης της γραμμής εντολών - azure ρόλο εμφάνιση - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Λίστα πρόσβασης
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Εκχώρησης ρόλου λίστα αποτελεσματικές σε μια ομάδα πόρων
Για να δημιουργήσετε μια λίστα με τις αναθέσεις ρόλων που υπάρχουν σε μια ομάδα πόρων, χρησιμοποιήστε:

    azure role assignment list --resource-group <resource group name>

Το παρακάτω παράδειγμα εμφανίζει τις αναθέσεις ρόλων στην ομάδα *pharma-πωλήσεις-projecforcast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Γραμμή εντολών RBAC Azure - λίστα ανάθεσης ρόλων azure κατά ομάδα στιγμιότυπο](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Λίστα εκχώρησης ρόλου για ένα χρήστη
Για να παραθέσετε τις αναθέσεις ρόλων για έναν συγκεκριμένο χρήστη και τις αναθέσεις που έχουν εκχωρηθεί σε ομάδες του χρήστη, χρησιμοποιήστε:

    azure role assignment list --signInName <user email>

Μπορείτε επίσης να δείτε τις αναθέσεις ρόλων που μεταβιβάζονται από τις ομάδες, τροποποιώντας την εντολή:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Το παρακάτω παράδειγμα εμφανίζει τις αναθέσεις εργασιών που έχουν εκχωρηθεί σε το *sameert@aaddemo.com* χρήστη. Αυτό περιλαμβάνει τους ρόλους που έχουν εκχωρηθεί απευθείας στο χρήστη και τους ρόλους που έχουν μεταβιβαστεί από τις ομάδες.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Στιγμιότυπο οθόνης της γραμμής εντολών - λίστα ανάθεσης ρόλων azure από το χρήστη - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Εκχώρηση πρόσβασης
Για να εκχωρήσετε πρόσβαση αφού έχετε αναθέσει το ρόλο που θέλετε να αντιστοιχίσετε, χρησιμοποιήστε:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Εκχώρηση ρόλου σε ομάδα στο εύρος συνδρομή
Για να αντιστοιχίσετε ένα ρόλο σε μια ομάδα στο εύρος συνδρομή, χρησιμοποιήστε:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Το παρακάτω παράδειγμα αντιστοιχίζει ρόλο *αναγνώστη* *Christine Koch της ομάδας* στο εύρος *συνδρομής* .


![Γραμμή εντολών Azure RBAC - δημιουργία εκχώρηση ρόλων azure με ομάδα-στιγμιότυπο οθόνης](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Εκχώρηση ρόλου σε μια εφαρμογή κατά την εμβέλεια της συνδρομής
Για να αντιστοιχίσετε ένα ρόλο σε μια εφαρμογή στο εύρος συνδρομή, χρησιμοποιήστε:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Το παρακάτω παράδειγμα εκχωρεί το ρόλο του *συνεργάτη* σε μια εφαρμογή *Azure AD* σε η επιλεγμένη συνδρομή.

 ![Γραμμή εντολών Azure RBAC - δημιουργία εκχώρηση ρόλων azure από εφαρμογή](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Εκχώρηση ρόλου σε ένα χρήστη στο εύρος ομάδα πόρων
Για να αντιστοιχίσετε ένα ρόλο χρήστη κατά την εμβέλεια ομάδα πόρων, χρησιμοποιήστε:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Το παρακάτω παράδειγμα εκχωρεί το ρόλο *Εικονική μηχανή συμβολής* για να *samert@aaddemo.com* χρήστη κατά την εμβέλεια ομάδας πόρων *Pharma-πωλήσεις-ProjectForcast* .

![Γραμμή εντολών Azure RBAC - δημιουργία εκχώρηση ρόλων azure με χρήστη-στιγμιότυπο οθόνης](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Εκχώρηση ρόλου σε ομάδα στο εύρος πόρων
Για να αντιστοιχίσετε ένα ρόλο σε μια ομάδα στο εύρος πόρων, χρησιμοποιήστε:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Το παρακάτω παράδειγμα εκχωρεί το ρόλο *Εικονική μηχανή συνεργάτη* σε μια ομάδα *Azure AD* σε *υποδίκτυο*.

![Γραμμή εντολών Azure RBAC - δημιουργία εκχώρηση ρόλων azure με ομάδα στιγμιότυπο](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Κατάργηση πρόσβασης
Για να καταργήσετε μια εκχώρηση ρόλων, χρησιμοποιήστε:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Το παρακάτω παράδειγμα καταργεί την εκχώρηση ρόλου *Συμβολής εικονική μηχανή* από το *sammert@aaddemo.com* χρήστη στην ομάδα πόρων *Pharma-πωλήσεις-ProjectForcast* .
Το παράδειγμα, στη συνέχεια, καταργεί την εκχώρηση ρόλων από μια ομάδα της συνδρομής.

![Στιγμιότυπο οθόνης της γραμμής εντολών - Διαγραφή ανάθεσης ρόλων azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Δημιουργήστε έναν προσαρμοσμένο ρόλο
Για να δημιουργήσετε ένα προσαρμοσμένο ρόλο, χρησιμοποιήστε:

    azure role create --inputfile <file path>

Το παρακάτω παράδειγμα δημιουργεί έναν προσαρμοσμένο ρόλο που ονομάζεται *Εικονική μηχανή τελεστή*. Το προσαρμοσμένο ρόλο εκχωρεί πρόσβαση σε όλες τις λειτουργίες ανάγνωσης *Microsoft.Compute*, *Microsoft.Storage*και *Microsoft.Network* υπηρεσιών παροχής πόρων και παρέχει πρόσβαση για να ξεκινήσετε, επανεκκινήστε και παρακολούθηση εικονικές μηχανές. Το προσαρμοσμένο ρόλο μπορεί να χρησιμοποιηθεί σε δύο συνδρομές. Αυτό το παράδειγμα χρησιμοποιεί ένα αρχείο JSON ως εισαγωγή.

![Στιγμιότυπο οθόνης JSON - ορισμού προσαρμοσμένης ρόλου-](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Γραμμή εντολών Azure RBAC - δημιουργία azure ρόλο - στιγμιότυπο οθόνης](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Τροποποίηση ενός προσαρμοσμένου ρόλου

Για να τροποποιήσετε έναν προσαρμοσμένο ρόλο, χρησιμοποιήστε πρώτα την `azure role show` εντολή για να ανακτήσετε ορισμού ρόλου. Στη συνέχεια, κάντε τις αλλαγές που θέλετε το αρχείο ορισμού ρόλο. Τέλος, χρησιμοποιήστε `azure role set` για να αποθηκεύσετε τον ορισμό ρόλου έχουν τροποποιηθεί.

    azure role set --inputfile <file path>

Το παρακάτω παράδειγμα προσθέτει τη λειτουργία *Microsoft.Insights/diagnosticSettings/* τις **Ενέργειες**και μια συνδρομή Azure για το **AssignableScopes** του ρόλου προσαρμοσμένο τελεστή εικονική μηχανή.

![JSON - τροποποίηση ορισμού προσαρμοσμένης ρόλου - στιγμιότυπο οθόνης](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Στιγμιότυπο οθόνης της γραμμής εντολών - azure ρόλο set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Διαγραφή προσαρμοσμένης ρόλου

Για να διαγράψετε έναν προσαρμοσμένο ρόλο, χρησιμοποιήστε πρώτα την `azure role show` εντολή για να προσδιορίσετε το **Αναγνωριστικό** του ρόλου. Στη συνέχεια, χρησιμοποιήστε το `azure role delete` εντολή για να διαγράψετε το ρόλο, καθορίζοντας το **Αναγνωριστικό**.

Το παρακάτω παράδειγμα καταργεί το προσαρμοσμένο ρόλο *Τελεστή εικονική μηχανή* .

![Στιγμιότυπο οθόνης της γραμμής εντολών - azure ρόλο delete - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Ρόλοι προσαρμοσμένης λίστας

Για να δημιουργήσετε μια λίστα με τους ρόλους που είναι διαθέσιμες για ανάθεση σε ένα εύρος, χρησιμοποιήστε το `azure role list` εντολή.

Το παρακάτω παράδειγμα εμφανίζει σε λίστα όλες ρόλο που είναι διαθέσιμες για την ανάθεση στην επιλεγμένη συνδρομή.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Στιγμιότυπο οθόνης της γραμμής εντολών - λίστα azure ρόλο - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

Στο παρακάτω παράδειγμα, το προσαρμοσμένο ρόλο *Τελεστή εικονική μηχανή* δεν είναι διαθέσιμη στην συνδρομής *Production4* επειδή δεν είναι αυτήν τη συνδρομή στο το **AssignableScopes** του ρόλου.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Στιγμιότυπο οθόνης της γραμμής εντολών - λίστα azure ρόλο για προσαρμοσμένο ρόλους - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>Θέματα RBAC
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
