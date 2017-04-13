<properties
    pageTitle="Εκχώρηση δικαιωμάτων χρήστη για τη συγκεκριμένη εργαστήριο πολιτικές | Microsoft Azure"
    description="Μάθετε πώς να εκχωρείτε δικαιώματα χρήστη στις πολιτικές συγκεκριμένες εργαστήριο DevTest Labs ανάλογα με τις ανάγκες κάθε χρήστη"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Εκχώρηση δικαιωμάτων χρήστη για τη συγκεκριμένη εργαστήριο πολιτικές

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης του PowerShell για να εκχωρήσετε στους χρήστες δικαιώματα για ένα συγκεκριμένο εργαστήριο πολιτικής. Με αυτόν τον τρόπο, δικαιώματα μπορούν να εφαρμοστούν με βάση τις ανάγκες κάθε χρήστη. Για παράδειγμα, μπορεί να θέλετε να εκχωρήσετε ένα συγκεκριμένο χρήστη τη δυνατότητα να αλλάξετε τις ρυθμίσεις πολιτικής Εικονική, αλλά όχι τις πολιτικές κόστος.

## <a name="policies-as-resources"></a>Πολιτικές ως πόρους

Όπως περιγράφεται στο άρθρο [Έλεγχος πρόσβασης βάσει ρόλων Azure](../active-directory/role-based-access-control-configure.md) , RBAC δίνει τη δυνατότητα πρόσβασης λεπτομερή διαχείρισης πόρων για Azure. Χρήση RBAC, μπορείτε να segregate καθηκόντων με την ομάδα σας DevOps και να εκχωρήσετε μόνο το ποσό της access σε χρήστες που χρειάζονται για την εκτέλεση των εργασιών τους.

Στο DevTest Labs, μια πολιτική είναι ένας τύπος πόρου που επιτρέπει την ενέργεια RBAC **Microsoft.DevTestLab/labs/policySets/policies/**. Κάθε πολιτική εργαστήριο είναι ένας πόρος στον τύπο πόρου πολιτικής και μπορεί να εκχωρηθεί ως εμβέλεια σε ένα ρόλο RBAC.

Για παράδειγμα, για να εκχωρήσετε στους χρήστες ανάγνωσης/εγγραφής δικαιώματα για την πολιτική **Μεγέθη επιτρέπεται Εικονική** , πρέπει να δημιουργήσετε ένα προσαρμοσμένο ρόλο που λειτουργεί με το **Microsoft.DevTestLab/labs/policySets/policies/** * ενέργεια, και, στη συνέχεια, εκχωρήστε τους κατάλληλους χρήστες σε αυτόν το ρόλο προσαρμοσμένες στην εμβέλεια της * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Για να μάθετε περισσότερα σχετικά με τις προσαρμοσμένες τους ρόλους στο RBAC, ανατρέξτε στο θέμα το [προσαρμοσμένο έλεγχο πρόσβασης σε ρόλους](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Δημιουργία προσαρμοσμένου ρόλου εργαστήριο χρήση του PowerShell
Για να ξεκινήσετε, θα πρέπει να διαβάσετε το παρακάτω άρθρο, το οποίο θα εξηγούν τον τρόπο εγκατάστασης και ρύθμισης παραμέτρων τα cmdlet του Azure PowerShell: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Αφού έχετε ρυθμίσει το cmdlet του Azure PowerShell, μπορείτε να εκτελέσετε τις ακόλουθες εργασίες:

- Λίστα με όλες τις λειτουργίες/ενέργειες για μια υπηρεσία παροχής πόρων
- Ενέργειες λίστας στο συγκεκριμένο ρόλο:
- Δημιουργήστε έναν προσαρμοσμένο ρόλο

Η ακόλουθη δέσμη ενεργειών PowerShell απεικονίζει παραδείγματα του τρόπου εκτέλεσης αυτές τις εργασίες:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Εκχώρηση δικαιωμάτων σε ένα χρήστη για μια συγκεκριμένη πολιτική χρήση προσαρμοσμένων ρόλων
Μόλις που έχετε ορίσει το προσαρμοσμένο ρόλους, μπορείτε να τους εκχωρήσετε στους χρήστες. Για να αντιστοιχίσετε ένα προσαρμοσμένο ρόλο σε ένα χρήστη, πρέπει πρώτα να αποκτήσετε **ObjectId** που αντιπροσωπεύει το συγκεκριμένο χρήστη. Για να κάνετε αυτό, χρησιμοποιήστε το cmdlet **Get-AzureRmADUser** .

Στο παρακάτω παράδειγμα, **ObjectId** του χρήστη *SomeUser* είναι 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Μόλις **ObjectId** για το χρήστη και ένα όνομα προσαρμοσμένο ρόλο, μπορείτε να αντιστοιχίσετε αυτόν το ρόλο του χρήστη με τα cmdlet **New-AzureRmRoleAssignment** :

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Στο προηγούμενο παράδειγμα, χρησιμοποιείται η πολιτική **AllowedVmSizesInLab** . Μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις παρακάτω πολιτικές:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Μία φορά εκχωρήσει δικαιώματα χρήστη για τις πολιτικές συγκεκριμένες εργαστήριο, εδώ θα βρείτε ορισμένα επόμενα βήματα για να λάβετε υπόψη:

- [Ασφαλή πρόσβαση σε ένα εργαστήριο](devtest-lab-add-devtest-user.md).

- [Ορισμός πολιτικών εργαστήριο](devtest-lab-set-lab-policy.md).

- [Δημιουργία προτύπου εργαστήριο](devtest-lab-create-template.md).

- [Δημιουργία προσαρμοσμένου αντικείμενα για ΣΠΣ σας](devtest-lab-artifact-author.md).

- [Προσθήκη μια Εικονική με αντικείμενα σε ένα εργαστήριο](devtest-lab-add-vm-with-artifacts.md).
