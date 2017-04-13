<properties
    pageTitle="Δημιουργία μιας έκθεσης της access αλλαγή ιστορικού | Microsoft Azure"
    description="Δημιουργήστε μια αναφορά που παραθέτει όλες τις αλλαγές στην access για να τις συνδρομές σας Azure με έλεγχο πρόσβασης βάσει ρόλων επάνω από τις τελευταίες 90 ημέρες."
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
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Δημιουργία μιας έκθεσης της access αλλαγή ιστορικού

Κάθε φορά που κάποιος εκχωρεί ή κάνει ανάκληση πρόσβασης εντός των συνδρομών σας, τις αλλαγές να συνδεθεί Azure συμβάντα. Μπορείτε να δημιουργήσετε αναφορές ιστορικού αλλαγών access για να δείτε όλες τις αλλαγές για τις τελευταίες 90 ημέρες.

## <a name="create-a-report-with-azure-powershell"></a>Δημιουργία αναφοράς με το Azure PowerShell
Για να δημιουργήσετε μια έκθεση της access αλλαγή ιστορικού PowerShell, χρησιμοποιήστε το `Get-AzureRMAuthorizationChangeLog` εντολή. Περισσότερες λεπτομέρειες σχετικά με αυτό το cmdlet είναι διαθέσιμες στη [Συλλογή PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1).

Όταν καλείτε αυτή την εντολή, μπορείτε να καθορίσετε ποια ιδιότητα των αναθέσεων που θέλετε που εμφανίζεται, όπως οι παρακάτω:

| Ιδιότητα | Περιγραφή |
| -------- | ----------- |
| **Ενέργεια** | Εάν access έχει εκχωρηθεί ή να ανακληθεί |
| **Καλούντος** | Ο κάτοχος της υπεύθυνοι για την αλλαγή της access |
| **Ημερομηνία** | Η ημερομηνία και ώρα που άλλαξε πρόσβασης |
| **Όνομα_καταλόγου** | Ο κατάλογος Azure Active Directory |
| **PrincipalName** | Το όνομα του χρήστη, ομάδας ή της εφαρμογής |
| **PrincipalType** | Εάν έχει ανάθεσης για ένα χρήστη, ομάδας ή εφαρμογή |
| **RoleId** | Το GUID του ρόλου που έχει εκχωρηθεί ή να ανακληθεί |
| **Όνομα ρόλου** | Το ρόλο που έχει εκχωρηθεί ή να ανακληθεί |
| **Όνομα πεδίου** | Το όνομα του τη συνδρομή, ομάδα πόρων ή πόρων |
| **ScopeType** | Εάν έχει ανάθεσης συνδρομή, ομάδα πόρων ή εύρος πόρων |
| **SubscriptionId** | Το GUID της συνδρομής Azure |
| **SubscriptionName** | Το όνομα της συνδρομής Azure |

Αυτή η εντολή παράδειγμα παραθέτει όλες τις αλλαγές πρόσβαση στην εγγραφή για τις τελευταίες επτά ημέρες:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - στιγμιότυπο οθόνης](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Δημιουργία αναφοράς με Azure CLI
Για να δημιουργήσετε μια έκθεση της access αλλαγή ιστορικού το Azure περιβάλλον γραμμής εντολών (CLI), χρησιμοποιήστε το `azure role assignment changelog list` εντολή.

## <a name="export-to-a-spreadsheet"></a>Εξαγωγή σε υπολογιστικό φύλλο
Για να αποθηκεύσετε την αναφορά, ή να χειριστείτε τα δεδομένα, εξαγάγετε τις αλλαγές πρόσβαση σε ένα αρχείο .csv. Στη συνέχεια, μπορείτε να προβάλετε την αναφορά σε ένα υπολογιστικό φύλλο για αναθεώρηση.

![Changelog προβάλλεται ως υπολογιστικό φύλλο - στιγμιότυπο οθόνης](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Δείτε επίσης
- Γρήγορα αποτελέσματα με το [Στοιχείο ελέγχου πρόσβασης Azure Role-Based](role-based-access-control-configure.md)
- Εργασία με [προσαρμοσμένη ρόλους σε Azure RBAC](role-based-access-control-custom-roles.md)
