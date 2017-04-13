<properties
    pageTitle="Διαχείριση έλεγχο πρόσβασης βάσει ρόλων (RBAC) με το Azure PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να διαχειριστείτε RBAC με το PowerShell Azure, συμπεριλαμβανομένων των παραθέτει τους ρόλους, εκχώρηση ρόλων και διαγραφή εκχώρησης ρόλου."
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

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Διαχειριστείτε τον έλεγχο πρόσβασης βάσει ρόλων με το Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)


Μπορείτε να χρησιμοποιήσετε έλεγχος πρόσβασης βάσει ρόλων (RBAC) στην πύλη του Azure και Azure API διαχείρισης πόρων για να διαχειριστείτε την πρόσβαση στη συνδρομή σας σε λεπτομερή επίπεδο. Με αυτήν τη δυνατότητα, μπορείτε να εκχωρήσετε πρόσβαση για χρήστες, ομάδες ή αρχές υπηρεσίας καταλόγου Active Directory, εκχώρηση ρόλων ορισμένες τους σε μια ειδική εμβέλεια.

Μπορείτε να χρησιμοποιήσετε το PowerShell για τη Διαχείριση RBAC, πρέπει να έχετε τα εξής:

- Azure PowerShell έκδοση 0.8.8 ή νεότερη έκδοση. Για να εγκαταστήσετε την πιο πρόσφατη έκδοση και να συσχετίσετε με τη συνδρομή σας στο Azure, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

- Azure cmdlet διαχείρισης πόρων. Εγκαταστήστε τα [cmdlet του Azure από διαχειριστή πόρων](https://msdn.microsoft.com/library/mt125356.aspx) σε PowerShell.

## <a name="list-roles"></a>Λίστα ρόλων

### <a name="list-all-available-roles"></a>Λίστα όλων των ρόλων διαθέσιμη
Για να τους ρόλους RBAC λίστα που είναι διαθέσιμες για την ανάθεση και να ελέγξετε τις λειτουργίες στις οποίες παρέχουν πρόσβαση, χρησιμοποιήστε `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Ενέργειες λίστας από ένα ρόλο
Για να δημιουργήσετε μια λίστα με τις ενέργειες για ένα συγκεκριμένο ρόλο, χρησιμοποιήστε `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleDefinition για ένα συγκεκριμένο ρόλο - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Δείτε ποιος έχει πρόσβαση
Για τη λίστα RBAC αναθέσεις access, χρησιμοποιήστε `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Λίστα εκχωρήσεις ρόλων σε ένα συγκεκριμένο εύρος
Μπορείτε να δείτε όλες τις αναθέσεις πρόσβασης για μια καθορισμένη εγγραφή, ομάδα πόρων ή πόρου. Για παράδειγμα, για να δείτε το όλες τις ενεργές αναθέσεις για μια ομάδα πόρων, χρησιμοποιήστε `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleAssignment για μια ομάδα πόρων - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Λίστα ρόλων στους οποίους έχουν ανατεθεί σε ένα χρήστη
Για να δημιουργήσετε μια λίστα όλους τους ρόλους που έχουν ανατεθεί σε έναν συγκεκριμένο χρήστη και τους ρόλους που έχουν εκχωρηθεί σε τις ομάδες στις οποίες ανήκει ο χρήστης, χρησιμοποιήστε `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleAssignment για ένα χρήστη - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Διαχειριστής υπηρεσίας κλασική λίστας και εκχώρησης ρόλου coadmin
Λίστα εκχωρήσεις πρόσβασης για το διαχειριστή κλασική συνδρομής και coadministrators, χρησιμοποιήστε:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Εκχώρηση πρόσβασης
### <a name="search-for-object-ids"></a>Αναζήτηση για το αντικείμενο αναγνωριστικά
Για να αντιστοιχίσετε ένα ρόλο, χρειάζεστε για τον προσδιορισμό του αντικειμένου (χρήστη, ομάδας ή εφαρμογή) και το εύρος.

Εάν δεν γνωρίζετε το Αναγνωριστικό συνδρομής, μπορείτε να το βρείτε στο το blade **συνδρομές** στην πύλη του Azure. Για να μάθετε πώς μπορείτε να υποβάλετε ένα ερώτημα για το Αναγνωριστικό συνδρομής, ανατρέξτε στο θέμα [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) στο MSDN.

Για να λάβετε το Αναγνωριστικό αντικειμένου για μια ομάδα του Azure AD, χρησιμοποιήστε:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Για να λάβετε το Αναγνωριστικό αντικειμένου για ένα κεφάλαιο υπηρεσίας Azure AD ή την εφαρμογή, χρησιμοποιήστε:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Εκχώρηση ρόλου σε μια εφαρμογή κατά την εμβέλεια της συνδρομής
Για να εκχωρήσετε πρόσβαση σε μια εφαρμογή κατά την εμβέλεια συνδρομή, χρησιμοποιήστε:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Στιγμιότυπο οθόνης του PowerShell νέα AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Εκχώρηση ρόλου σε ένα χρήστη στο εύρος ομάδα πόρων
Για να εκχωρήσετε πρόσβαση σε ένα χρήστη στο εύρος ομάδα πόρων, χρησιμοποιήστε:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Στιγμιότυπο οθόνης του PowerShell νέα AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Εκχώρηση ρόλου σε ομάδα στο εύρος πόρων
Για να εκχωρήσετε πρόσβαση σε μια ομάδα στο εύρος πόρων, χρησιμοποιήστε:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Στιγμιότυπο οθόνης του PowerShell νέα AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Κατάργηση πρόσβασης
Για να καταργήσετε την πρόσβαση για τους χρήστες, ομάδες και εφαρμογές, χρησιμοποιήστε:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Στιγμιότυπο οθόνης του PowerShell-κατάργηση AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Δημιουργήστε έναν προσαρμοσμένο ρόλο
Για να δημιουργήσετε ένα προσαρμοσμένο ρόλο, χρησιμοποιήστε το `New-AzureRmRoleDefinition` εντολή.

Όταν δημιουργείτε μια προσαρμοσμένη ρόλων με χρήση του PowerShell, πρέπει να ξεκινήσετε με έναν από τους [ρόλους ενσωματωμένα](role-based-access-built-in-roles.md). Επεξεργαστείτε τις ιδιότητες για να προσθέσετε το *Ενέργειες*, *notActions*ή *εύρους* που θέλετε και, στη συνέχεια, αποθηκεύστε τις αλλαγές με ένα νέο ρόλο.

Το παρακάτω παράδειγμα ξεκινά με το ρόλο του *Συνεργάτη εικονική μηχανή* και που χρησιμοποιεί για να δημιουργήσετε ένα προσαρμοσμένο ρόλο που ονομάζεται *Εικονική μηχανή τελεστή*. Το νέο ρόλο εκχωρεί πρόσβαση σε όλες τις λειτουργίες ανάγνωσης *Microsoft.Compute*, *Microsoft.Storage*και *Microsoft.Network* υπηρεσιών παροχής πόρων και παρέχει πρόσβαση για να ξεκινήσετε, επανεκκινήστε και παρακολούθηση εικονικές μηχανές. Το προσαρμοσμένο ρόλο μπορεί να χρησιμοποιηθεί σε δύο συνδρομές.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Τροποποίηση ενός προσαρμοσμένου ρόλου
Για να τροποποιήσετε έναν προσαρμοσμένο ρόλο, πρώτα, χρησιμοποιήστε το `Get-AzureRmRoleDefinition` εντολή για να ανακτήσετε τον ορισμό ρόλου. Δεύτερον, πραγματοποιήστε τις επιθυμητές αλλαγές στον ορισμό ρόλου. Τέλος, χρησιμοποιήστε το `Set-AzureRmRoleDefinition` εντολή για να αποθηκεύσετε τον ορισμό ρόλου έχουν τροποποιηθεί.

Το παρακάτω παράδειγμα προσθέτει το `Microsoft.Insights/diagnosticSettings/*` λειτουργίας στον προσαρμοσμένο ρόλο *Τελεστή εικονική μηχανή* .

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Στιγμιότυπο οθόνης του PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Το παρακάτω παράδειγμα προσθέτει μια συνδρομή του Azure εκχωρήσιμο εμβελειών του ρόλου προσαρμοσμένο *Τελεστή εικονική μηχανή* .

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![Στιγμιότυπο οθόνης του PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Διαγραφή προσαρμοσμένης ρόλου

Για να διαγράψετε έναν προσαρμοσμένο ρόλο, χρησιμοποιήστε το `Remove-AzureRmRoleDefinition` εντολή.

Το παρακάτω παράδειγμα καταργεί το προσαρμοσμένο ρόλο *Τελεστή εικονική μηχανή* .

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Στιγμιότυπο οθόνης του PowerShell-κατάργηση AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Ρόλοι προσαρμοσμένης λίστας
Για να δημιουργήσετε μια λίστα με τους ρόλους που είναι διαθέσιμες για ανάθεση σε ένα εύρος, χρησιμοποιήστε το `Get-AzureRmRoleDefinition` εντολή.

Το παρακάτω παράδειγμα εμφανίζει όλους τους ρόλους που είναι διαθέσιμες για την ανάθεση στην επιλεγμένη συνδρομή.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

Στο παρακάτω παράδειγμα, το προσαρμοσμένο ρόλο *Τελεστή εικονική μηχανή* δεν είναι διαθέσιμη στην συνδρομής *Production4* επειδή δεν είναι αυτήν τη συνδρομή στο το **AssignableScopes** του ρόλου.

![Στιγμιότυπο οθόνης του PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Δείτε επίσης
- [Χρήση του Azure PowerShell με τη Διαχείριση Azure πόρων](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
