<properties
    pageTitle="Azure DevTest Labs συνήθεις Ερωτήσεις | Microsoft Azure"
    description="Βρείτε απαντήσεις σε συνήθεις ερωτήσεις Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
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
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs συνήθεις Ερωτήσεις

Σε αυτό το άρθρο παρέχει απαντήσεις σε ορισμένες από τις πιο συνηθισμένες ερωτήσεις σχετικά με το Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Γενικά
- [Τι γίνεται εάν την ερώτησή μου δεν απαντήθηκε εδώ;](#what-if-my-question-isnt-answered-here)
- [Γιατί πρέπει να χρησιμοποιώ Azure DevTest Labs;](#why-should-i-use-azure-devtest-labs) 
- [Τι σημαίνει "Υψηλή χωρίς, από το χρήστη";](#what-does-quotworry-free-self-servicequot-mean)
- [Πώς μπορώ να χρησιμοποιήσω Azure DevTest Labs;](#how-can-i-use-azure-devtest-labs) 
- [Πώς πραγματοποιώ να χρεωθεί για Azure DevTest Labs;](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Ασφάλεια 
- [Τι είναι τα επίπεδα ασφαλείας διαφορετικό στο Azure DevTest Labs;](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Πώς μπορώ να δημιουργήσω ένα ρόλο για να επιτρέψετε στους χρήστες να εκτελούν μια συγκεκριμένη εργασία;](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>Ενοποίηση CI/CD & αυτοματισμού 
 
- [Η ενοποίηση του Azure DevTest Labs με μου toolchain CI/CD;](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Εικονικές μηχανές 
 
- [Γιατί δεν μπορώ να δω ορισμένες ΣΠΣ στο το blade εικονικές μηχανές Windows Azure που εμφανίζεται μέσα σε Azure DevTest Labs;](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Ποια είναι η διαφορά μεταξύ των προσαρμοσμένων εικόνων και των τύπων;](#what-is-the-difference-between-custom-images-and-formulas) 
- [Πώς μπορώ να δημιουργήσω πολλές ΣΠΣ από το ίδιο πρότυπο ταυτόχρονα;](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Πώς μπορώ να μετακινήσω το υπάρχον ΣΠΣ Azure σε μου εργαστήριο Azure DevTest Labs;](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Μπορώ να συνδέσω πολλών δίσκων σε ΣΠΣ μου;](#can-i-attach-multiple-disks-to-my-vms) 
- [Πώς μπορώ να αυτοματοποιήσω τη διαδικασία αποστολής αρχείων VHD για να δημιουργήσετε προσαρμοσμένες εικόνες;](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Πώς μπορώ να αυτοματοποιήσω η διαδικασία διαγραφής όλα τα ΣΠΣ στο εργαστήριο μου;](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Αντικείμενα 
 
- [Τι είναι τα αντικείμενα;](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Ρύθμιση παραμέτρων εργαστήριο 
 
- [Πώς μπορώ να δημιουργήσω ένα εργαστήριο από ένα πρότυπο από διαχειριστή πόρων Azure;](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Γιατί μου ΣΠΣ δημιουργήθηκαν σε διαφορετικές ομάδες πόρων με ονόματα αυθαίρετο; Μπορώ να μετονομάσω ή να τροποποιήσετε αυτές τις ομάδες πόρων;](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Πόσες labs μπορώ να δημιουργήσω κάτω από την ίδια συνδρομή;](#how-many-labs-can-i-create-under-the-same-subscription)
- [Πόσες ΣΠΣ μπορώ να δημιουργήσω ανά εργαστήριο;](#how-many-vms-can-i-create-per-lab)
- [Πώς κάνω κοινή χρήση μια απευθείας σύνδεση σε εργαστήριο μου;](#how-do-i-share-a-direct-link-to-my-lab)
- [Τι είναι ένας λογαριασμός Microsoft;](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων 
 
- [Το αντικείμενο απέτυχε κατά τη δημιουργία Εικονική. Πώς να τα προβλήματα;](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Γιατί δεν είναι μου υπάρχοντος εικονικού δικτύου αποθήκευση σωστά;](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Τι γίνεται εάν την ερώτησή μου δεν απαντήθηκε εδώ;
Εάν η ερώτησή σας δεν αναφέρεται εδώ, ενημερώστε μας και θα σας βοηθήσουμε να βρείτε μια απάντηση.

- Δημοσιεύστε μια ερώτηση στο [νήμα Disqus](#comments) στο τέλος του αυτές τις συνήθεις Ερωτήσεις και να επικοινωνείτε με την ομάδα του Azure Cache και άλλα μέλη της Κοινότητας σχετικά με αυτό το άρθρο.
- Για να προσεγγίσετε ευρύτερο ακροατήριο, δημοσιεύστε μια ερώτηση στο [φόρουμ Azure DevTest Labs MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)και Εμπλακείτε με την ομάδα του Azure DevTest Labs και άλλα μέλη της Κοινότητας.
- Για να κάνετε μια αίτηση δυνατότητα, υποβάλετε τις αιτήσεις και τις ιδέες για το [Azure DevTest Labs χρήστη φωνής](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Γιατί πρέπει να χρησιμοποιώ Azure DevTest Labs; 
Azure DevTest Labs να αποθηκεύσετε την ομάδα χρόνο και χρήμα σας. Προγραμματιστές να δημιουργήσετε τις δικές τους περιβάλλοντα που χρησιμοποιούν πολλές διαφορετικές βάσεις, και να χρησιμοποιήσετε αντικείμενα για να αναπτύξετε γρήγορα και να ρυθμίσετε τις εφαρμογές. Χρήση προσαρμοσμένων εικόνων και των τύπων, εικονικές μηχανές μπορούν να αποθηκευτούν ως πρότυπα και αναπαραχθούν εύκολα. Επιπλέον, labs προσφέρουν πολλές με δυνατότητα ρύθμισης παραμέτρων πολιτικών που επιτρέπουν στους διαχειριστές εργαστήριο για μείωση απορριμμάτων και τη διαχείριση μιας ομάδας περιβάλλοντα. Οι πολιτικές αυτές περιλαμβάνουν αυτόματης τερματισμού, όριο κόστος, μέγιστο ΣΠΣ ανά χρήστη και τον μέγιστο μέγεθος Εικονική. Για μια πιο αναλυτικά επεξήγηση του Azure DevTest Labs, διαβάστε την [Επισκόπηση](devtest-lab-overview.md) ή δείτε το [εισαγωγικό βίντεο](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Τι σημαίνει "Υψηλή χωρίς, από το χρήστη";
"Υψηλή χωρίς, από το χρήστη" σημαίνει ότι τους προγραμματιστές και τους υπεύθυνους δοκιμών δημιουργήσετε τις δικές τους περιβάλλοντα εφόσον χρειάζεται, και οι διαχειριστές έχουν την ασφάλεια γνωρίζουν ότι Azure DevTest Labs βοηθά στην ελαχιστοποίηση σπαταλούν και να ελέγξετε κόστος. Οι διαχειριστές μπορούν να καθορίσετε ποια Εικονική μεγέθη επιτρέπεται οι, ο μέγιστος αριθμός των ΣΠΣ, και όταν ΣΠΣ είναι εκκίνηση και τερματισμός. Azure DevTest Labs επίσης καθιστά εύκολη για την παρακολούθηση κόστους και Ορισμός ειδοποιήσεων για να παραμείνετε ενήμεροι πώς χρησιμοποιούνται οι πόροι στο εργαστήριο. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Πώς μπορώ να χρησιμοποιήσω Azure DevTest Labs; 
Azure DevTest Labs είναι χρήσιμη οποιαδήποτε στιγμή που απαιτούν την ή δοκιμή περιβάλλοντα και θέλετε να αναπαραγάγετε τα γρήγορα ή/και να διαχειριστείτε τους με το κόστος αποθήκευση πολιτικές. 

Εδώ θα βρείτε ορισμένα σενάρια που οι πελάτες μας χρησιμοποιούν Labs DevTest Azure για: 

- Διαχείριση των περιβάλλοντα ανάπτυξης και δοκιμής σε ένα σημείο, με χρήση πολιτικών για να μειώσετε το κόστος και τα προσαρμοσμένα εικόνες για να κάνετε κοινή χρήση δημιουργεί κατά μήκος την ομάδα.
- Ανάπτυξη μιας εφαρμογής με χρήση προσαρμοσμένων εικόνων για να αποθηκεύσετε την κατάσταση δίσκου σε όλο το τα στάδια ανάπτυξης.
- Παρακολούθηση κόστους σχέση με την πρόοδο. 
- Δημιουργία περιβάλλοντα μάζας δοκιμή για σκοπούς δοκιμής assurance ποιότητας.
- Χρήση αντικείμενα και τους τύπους για να ρυθμίσετε τις παραμέτρους και να αναπαραγάγετε μια εφαρμογή σε διάφορα περιβάλλοντα εύκολα. 
- Διανομή ΣΠΣ για hackathons (συνεργασίας αποκλίσεις ή δοκιμής εργασία) και, στη συνέχεια, εύκολα καταργήστε την προμήθεια τους όταν λήξει το συμβάν. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Πώς πραγματοποιώ να χρεωθεί για Azure DevTest Labs; 
Azure DevTest Labs είναι μια δωρεάν υπηρεσία, γεγονός που σημαίνει ότι η δημιουργία labs και τη ρύθμιση των παραμέτρων του πολιτικές, πρότυπα και αντικείμενα είναι δωρεάν. Μπορείτε να πληρώσετε μόνο για το Azure τους πόρους που χρησιμοποιούνται μέσα σε labs σας, όπως εικονικές μηχανές, λογαριασμούς χώρου αποθήκευσης και εικονικού δίκτυα. Για περισσότερες πληροφορίες σχετικά με το κόστος των πόρων εργαστήριο, διαβάστε σχετικά με [τις τιμές Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Τι είναι τα επίπεδα ασφαλείας διαφορετικό στο Azure DevTest Labs;  
Πρόσβαση ασφαλείας προσδιορίζεται από το [Στοιχείο ελέγχου πρόσβασης Azure Role-Based (RBAC)](../active-directory/role-based-access-built-in-roles.md). Για να κατανοήσετε τον τρόπο λειτουργίας της access, είναι χρήσιμο να κατανοήσετε τις διαφορές μεταξύ ενός δικαιωμάτων, ένα ρόλο και ένα εύρος όπως ορίζεται από RBAC.

- **Δικαίωμα** - ένα δικαίωμα είναι ένα καθορισμένο πρόσβαση σε μια συγκεκριμένη ενέργεια. Για παράδειγμα, ένα δικαίωμα μπορεί να-πρόσβαση για ανάγνωση όλα εικονικές μηχανές. 
- **Ρόλος** - ένα ρόλο είναι ένα σύνολο δικαιωμάτων που μπορούν να ομαδοποιηθούν και στους οποίους έχουν ανατεθεί σε ένα χρήστη. Για παράδειγμα, μια "κάτοχος της συνδρομής" έχει πρόσβαση σε όλους τους πόρους μέσα σε μια συνδρομή. 
- **Εύρος** - εμβέλεια είναι ένα επίπεδο στην ιεραρχία του Azure πόρου. Για παράδειγμα, ένα εύρος θα μπορούσε να είναι μια ομάδα πόρων ή μια μεμονωμένη εργαστήριο ή ολόκληρη τη συνδρομή. 
 
Στο πεδίο εφαρμογής του Azure DevTest Labs, υπάρχουν δύο τύποι με τους ρόλους για να ορίσετε δικαιώματα χρήστη: εργαστήριο κάτοχο και εργαστήριο χρήστη.

- **Κάτοχος εργαστήριο** - κάτοχος εργαστήριο έχει πρόσβαση σε πόρους μέσα σε εργαστήριο. Γι ' αυτό, αυτές μπορούν να τροποποιήσετε πολιτικές, διαβάστε και γράψτε οποιαδήποτε ΣΠΣ, αλλάξτε το εικονικό δίκτυο, και ούτω καθεξής. 
- **Εργαστήριο χρήστη** - εργαστήριο χρήστη να προβάλετε όλους τους πόρους εργαστήριο, όπως ΣΠΣ, πολιτικές και εικονικού δίκτυα, αλλά δεν μπορούν να τροποποιήσουν πολιτικές ή οποιαδήποτε ΣΠΣ που δημιουργούνται από άλλους χρήστες. Είναι επίσης πιθανό για να δημιουργήσετε προσαρμοσμένες τους ρόλους στο Azure DevTest Labs και μπορείτε να μάθετε πώς μπορείτε να κάνετε στο άρθρο [Εκχώρηση δικαιωμάτων χρήστη στις πολιτικές συγκεκριμένες εργαστήριο](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Επειδή το εύρος είναι ιεραρχικά, όταν ένας χρήστης έχει δικαιώματα σε ένα συγκεκριμένο εύρος, τους έχουν παραχωρηθεί αυτόματα αυτά τα δικαιώματα σε κάθε επίπεδο κάτω το εύρος που περικλείονται. Για παράδειγμα, αν ένας χρήστης έχει εκχωρηθεί ο ρόλος του κάτοχος της συνδρομής, στη συνέχεια, έχουν πρόσβαση σε όλους τους πόρους σε μια συνδρομή. Αυτοί οι πόροι περιλαμβάνουν όλες οι εικονικές μηχανές, όλα τα δίκτυα εικονικού και labs όλων. Έτσι, κάτοχος συνδρομή μεταβιβάζονται αυτόματα το ρόλο του κατόχου εργαστήριο. Ωστόσο, το αντίθετο δεν είναι αληθής. Κάτοχος εργαστήριο έχει πρόσβαση σε μια εργαστήριο, το οποίο είναι ένα εύρος κάτω από το επίπεδο συνδρομή. Επομένως, κάτοχος εργαστήριο δεν μπορούν να δουν εικονικές μηχανές ή εικονικού δίκτυα ή τους πόρους που βρίσκονται εκτός εργαστήριο. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Πώς μπορώ να δημιουργήσω ένα ρόλο για να επιτρέψετε στους χρήστες να εκτελούν μια συγκεκριμένη εργασία;
Εδώ, μπορείτε να βρείτε μια ολοκληρωμένη άρθρο σχετικά με τον τρόπο δημιουργίας προσαρμοσμένου τους ρόλους και εκχώρηση δικαιωμάτων σε αυτόν το ρόλο. Ακολουθεί ένα παράδειγμα μιας δέσμης ενεργειών που δημιουργεί το ρόλο "DevTest Labs για προχωρημένους χρήστη", το οποίο έχει δικαίωμα να ξεκινήσετε και να τερματίσετε όλα ΣΠΣ στο εργαστήριο:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Η ενοποίηση του Azure DevTest Labs με μου toolchain CI/CD; 
Εάν χρησιμοποιείτε VSTS, υπάρχει μια [επέκταση Azure DevTest Labs εργασίες](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) που σας επιτρέπει να αυτοματοποιήσετε σας διοχέτευσης κυκλοφορίας στο Azure DevTest Labs. Ορισμένες από τις χρήσεις των αυτή την επέκταση περιλαμβάνουν τα εξής:

- Δημιουργία και ανάπτυξη αυτόματα μια Εικονική και τη ρύθμιση παραμέτρων με την πιο πρόσφατη έκδοση με χρήση αντιγραφής αρχείων Azure ή PowerShell VSTS εργασίες. 
- Η καταγραφή της κατάστασης μια Εικονική αυτόματα μετά τη δοκιμή για να αναπαραγάγετε ένα σφάλμα στην το ίδιο Εικονική για περαιτέρω διερεύνηση. 
- Διαγραφή η Εικονική στο τέλος της διοχέτευσης κυκλοφορίας όταν δεν είναι πλέον απαραίτητη. 

Το παρακάτω καταχωρήσεων ιστολογίου παρέχουν οδηγίες και πληροφορίες σχετικά με την επέκταση VSTS:
 
- [Azure DevTest Labs – VSTS επέκτασης](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Για την ανάπτυξη μιας νέας Εικονική στα ένα υπάρχον AzureDevTestLab από VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Με την υπηρεσία διαχείρισης κυκλοφορίας VSTS για συνεχή αναπτύξεις σε AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Για άλλες toolchains CI/CD, όλα τα παραπάνω σενάρια που μπορεί να είναι έως την επέκταση εργασίες VSTS μπορείτε να επιτύχετε ομοίως έως την ανάπτυξη [προτύπων για τη διαχείριση πόρων Azure](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) με χρήση [των cmdlet του Azure PowerShell](../resource-group-template-deploy.md) και [SDK .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Μπορείτε επίσης να χρησιμοποιήσετε [REST API για DevTest Labs](http://aka.ms/dtlrestapis) ενσωμάτωση με το toolchain.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Γιατί δεν μπορώ να δω ορισμένες ΣΠΣ στο το blade εικονικές μηχανές Windows Azure που εμφανίζεται μέσα σε Azure DevTest Labs;
Όταν δημιουργείται μια Εικονική στο Azure DevTest Labs, λαμβάνει δικαίωμα για πρόσβαση σε συγκεκριμένη Εικονική. Είστε σε θέση να προβάλετε τόσο στο το blade labs και το blade **εικονικές μηχανές** . Οι χρήστες στο ρόλο DevTest Labs να δείτε όλες οι εικονικές μηχανές δημιουργείται στο εργαστήριο μέσω του εργαστήριο **όλες οι εικονικές μηχανές** blade. Ωστόσο, οι χρήστες στο ρόλο DevTest Labs δεν έχουν εκχωρηθεί αυτόματα πρόσβαση ανάγνωσης σε πόρους Εικονική που έχετε δημιουργήσει άλλους χρήστες. Επομένως, αυτά τα ΣΠΣ δεν εμφανίζονται στο το blade **εικονικές μηχανές** . 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Ποια είναι η διαφορά μεταξύ των προσαρμοσμένων εικόνων και των τύπων; 
Μια προσαρμοσμένη εικόνα είναι VHD (εικονικός χώρος στον σκληρό δίσκο), ότι ένας τύπος είναι μια εικόνα που μπορείτε να ρυθμίσετε με πρόσθετες ρυθμίσεις που μπορείτε να αποθηκεύσετε και να αναπαραγάγετε. Μια προσαρμοσμένη εικόνα μπορεί να είναι προτιμότερη εάν θέλετε να δημιουργήσετε γρήγορα πολλές περιβάλλοντα με την ίδια εικόνα βασικές, αμετάβλητες. Ένας τύπος μπορεί να είναι καλύτερη εάν θέλετε να αναπαραγάγετε τη ρύθμιση παραμέτρων του σας Εικονική με τα πιο πρόσφατα bit, ένα εικονικό υποδίκτυο ή ένα συγκεκριμένο μέγεθος. Για μια πιο αναλυτικά επεξήγηση, ανατρέξτε στο άρθρο, [προσαρμοσμένες εικόνες σύγκριση και τύπους στο DevTest Labs](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Πώς μπορώ να δημιουργήσω πολλές ΣΠΣ από το ίδιο πρότυπο ταυτόχρονα; 
Μπορείτε να χρησιμοποιήσετε την [επέκταση εργασίες VSTS](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) ή [Δημιουργία ενός προτύπου για τη διαχείριση πόρων Azure](devtest-lab-add-vm-with-artifacts.md#save-arm-template) κατά τη δημιουργία μια Εικονική και [ανάπτυξη του προτύπου για τη διαχείριση πόρων Azure από το Windows PowerShell](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Πώς μπορώ να μετακινήσω το υπάρχον ΣΠΣ Azure σε μου εργαστήριο Azure DevTest Labs; 
Σχεδίαση μια λύση για να μετακινήσετε απευθείας ΣΠΣ Azure DevTest Labs, αλλά αυτήν τη στιγμή μπορείτε να αντιγράψετε το υπάρχον ΣΠΣ Azure DevTest Labs ως εξής: 

1. Αντιγράψτε το αρχείο VHD από την υπάρχουσα Εικονική χρήση αυτής της [δέσμης ενεργειών του Windows PowerShell](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [Δημιουργία της προσαρμοσμένης εικόνας](devtest-lab-create-template.md) μέσα σε εργαστήριο Azure DevTest Labs σας. 
1. Δημιουργήστε μια Εικονική στο εργαστήριο από την προσαρμοσμένη εικόνα 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Μπορώ να συνδέσω πολλών δίσκων σε ΣΠΣ μου; 
Επισύναψη πολλών δίσκων σε ΣΠΣ υποστηρίζεται.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Πώς μπορώ να αυτοματοποιήσω τη διαδικασία αποστολής αρχείων VHD για να δημιουργήσετε προσαρμοσμένες εικόνες; 
Υπάρχουν δύο επιλογές:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) μπορεί να χρησιμοποιηθεί για να αντιγράψετε ή να αποστείλετε αρχεία VHD με το λογαριασμό χώρου αποθήκευσης που σχετίζεται με το εργαστήριο.
- [Εξερεύνηση του Microsoft Azure χώρου αποθήκευσης](../vs-azure-tools-storage-manage-with-storage-explorer.md) είναι μια μεμονωμένη εφαρμογή που εκτελείται σε Windows, OSX και Linux.   
 
Για να βρείτε το λογαριασμό χώρου αποθήκευσης προορισμού που σχετίζεται με εργαστήριο σας, ακολουθήστε τα παρακάτω βήματα:

1. Είσοδος στην [πύλη του Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Επιλέξτε **Ομάδες πόρων** από το αριστερό τμήμα του παραθύρου. 
1. Εντοπίστε και επιλέξτε την ομάδα των πόρων που σχετίζονται με εργαστήριο σας. 
1. Στην την **Επισκόπηση** blade, επιλέξτε έναν από τους λογαριασμούς χώρου αποθήκευσης. 
1. Επιλέξτε **αντικείμενα BLOB**.
1. Αναζητήστε αποστολές στη λίστα. Εάν δεν υπάρχει κανένα, επιστρέψτε στο βήμα #4 και δοκιμάστε έναν άλλο λογαριασμό χώρου αποθήκευσης.
1. Χρησιμοποιήστε τη **διεύθυνση URL** ως προορισμό σας στην εντολή AzCopy σας.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Πώς μπορώ να αυτοματοποιήσω η διαδικασία διαγραφής όλα τα ΣΠΣ στο εργαστήριο μου;

Εκτός από τη διαγραφή ΣΠΣ από εργαστήριο σας στην πύλη του Azure, μπορείτε να διαγράψετε όλα τα ΣΠΣ στο εργαστήριο σας χρησιμοποιώντας μια δέσμη ενεργειών PowerShell. Στο παρακάτω παράδειγμα, απλώς τροποποιήστε τις τιμές παραμέτρων κάτω από το σχόλιο **τιμές για να αλλάξετε** . Μπορείτε να ανακτήσετε τα `subscriptionId`, `labResourceGroup`, και `labName` τιμές από την blade εργαστήριο στην πύλη του Azure. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Τι είναι τα αντικείμενα; 
Αντικείμενα είναι προσαρμόσιμες στοιχεία που μπορούν να χρησιμοποιηθούν για να αναπτύξετε την πιο πρόσφατη bit ή τα εργαλεία αποκλίσεις σε μια Εικονική. Επισύναψη σε Εικονική σας κατά τη δημιουργία με λίγα μόνο κλικ απλές και, μόλις παρασχεθεί η Εικονική, τα αντικείμενα ανάπτυξη και ρύθμιση παραμέτρων σας Εικονική. Υπάρχουν διάφορες προ-υπαρχόντων αντικείμενα σε μας [δημόσια αποθετήριο Github](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), αλλά μπορείτε να επίσης εύκολα [Συντάκτης τη δική σας αντικείμενα](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Πώς μπορώ να δημιουργήσω ένα εργαστήριο από ένα πρότυπο από διαχειριστή πόρων Azure; 
Παρέχουμε ένα [αποθετήριο Github εργαστήριο διαχείριση πόρων Azure πρότυπα](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) που μπορείτε να αναπτύξετε ως-είναι ή να τροποποιήσετε για να δημιουργήσετε προσαρμοσμένα πρότυπα για το labs. Κάθε ένα από αυτά τα πρότυπα έχει μια σύνδεση που μπορείτε να κάνετε κλικ για να αναπτύξετε το εργαστήριο ως-βρίσκεται κάτω από τη δική σας Azure συνδρομή, ή μπορείτε να προσαρμόσετε το πρότυπο και να [αναπτύξετε με τη χρήση του PowerShell ή Azure CLI](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Γιατί μου ΣΠΣ δημιουργήθηκαν σε διαφορετικές ομάδες πόρων με ονόματα αυθαίρετο; Μπορώ να μετονομάσω ή να τροποποιήσετε αυτές τις ομάδες πόρων; 
Ομάδες πόρων δημιουργούνται με αυτόν τον τρόπο για Labs DevTest Azure για να διαχειριστείτε την πρόσβαση σε εικονικές μηχανές και δικαιωμάτων χρήστη. Ενώ μπορείτε να μετακινήσετε την εικονική Μηχανή σε μια άλλη ομάδα πόρων με το όνομα που θέλετε, κάνοντας επομένως δεν συνιστάται. Εργαζόμαστε για τη βελτίωση αυτή η εμπειρία για να επιτρέψετε μεγαλύτερη ευελιξία.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Πόσες labs μπορώ να δημιουργήσω κάτω από την ίδια συνδρομή; 
Δεν υπάρχει συγκεκριμένο όριο στον αριθμό των labs που μπορούν να δημιουργηθούν ανά συνδρομή. Ωστόσο, τους πόρους που χρησιμοποιούνται περιορίζονται ανά συνδρομή. Μπορείτε να διαβάσετε σχετικά με την [επιβληθούν περιορισμοί και όρια σε συνδρομές του Azure](../azure-subscription-service-limits.md) και [πώς μπορείτε να αυξήσετε αυτά τα όρια](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Πόσες ΣΠΣ μπορώ να δημιουργήσω ανά εργαστήριο; 
Δεν υπάρχει συγκεκριμένο όριο στον αριθμό των ΣΠΣ που μπορούν να δημιουργηθούν ανά εργαστήριο. Ωστόσο, προς το παρόν εργαστήριο υποστηρίζει μόνο περίπου 40 ΣΠΣ εκτελεί την ίδια στιγμή σε τυπική χώρου αποθήκευσης και 25 ΣΠΣ εκτελείται ταυτόχρονα στο χώρο αποθήκευσης premium. Προσπαθούμε αυτήν τη στιγμή σε αύξουσα αυτά τα όρια. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Πώς κάνω κοινή χρήση μια απευθείας σύνδεση σε εργαστήριο μου;

Για να κάνετε κοινή χρήση μια απευθείας σύνδεση στους χρήστες σας εργαστήριο μπορείτε να εκτελέσετε την ακόλουθη διαδικασία.

1. Μεταβείτε στην πύλη του Azure εργαστήριο.
2. Αντιγράψτε τη διεύθυνση URL εργαστήριο από το πρόγραμμα περιήγησης και να το μοιραστείτε με τους χρήστες σας εργαστήριο. 

>[AZURE.NOTE] Εάν οι χρήστες σας εργαστήριο είναι εξωτερικών χρηστών με ένα [λογαριασμό MSA](#what-is-a-microsoft-account) και δεν ανήκουν σε υπηρεσία καταλόγου Active directory της εταιρείας σας, μπορεί να λαμβάνουν ένα σφάλμα κατά την περιήγηση στην παρεχόμενη σύνδεση. Εάν να λαμβάνουν ένα σφάλμα, πείτε να κάντε κλικ στο όνομά της στην επάνω δεξιά γωνία της πύλης Azure και επιλέξτε τον κατάλογο όπου υπάρχει εργαστήριο από την ενότητα **καταλόγου** του μενού.

### <a name="what-is-a-microsoft-account"></a>Τι είναι ένας λογαριασμός Microsoft;

Ο λογαριασμός Microsoft είναι τα στοιχεία που χρησιμοποιείτε για σχεδόν όλα τα στοιχεία που μπορείτε να κάνετε με το Microsoft συσκευές και υπηρεσίες. Πρόκειται για μια διεύθυνση ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης που χρησιμοποιείτε για να εισέλθετε στο Skype, Outlook.com, OneDrive, Windows Phone και το Xbox LIVE – και αυτό σημαίνει ότι σας αρχεία, φωτογραφίες, τις επαφές και τις ρυθμίσεις μπορούν να σας παρακολουθούν σε οποιαδήποτε συσκευή. 

>[AZURE.NOTE] Λογαριασμό Microsoft που χρησιμοποιείται για να ονομάζεται "Windows Live ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Το αντικείμενο απέτυχε κατά τη δημιουργία Εικονική. Πώς να τα προβλήματα; 
Ανατρέξτε στην καταχώρηση ιστολογίου [Τρόπος αντιμετώπισης προβλημάτων αποτυγχάνει εργαλεία σε AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - συνταχθεί από έναν από τους MVP μας - για να μάθετε πώς μπορείτε να αποκτήσετε αρχεία καταγραφής σχετικά με το αντικείμενο απέτυχε. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Γιατί δεν είναι μου υπάρχοντος εικονικού δικτύου αποθήκευση σωστά;  
Μία πιθανότητα είναι ότι το όνομα εικονικού δικτύου περιέχει περιόδων. Αν Ναι, δοκιμάστε τις περιόδους κατάργηση ή αντικατάσταση με παύλες και, στη συνέχεια, προσπαθήστε ξανά να αποθηκεύσετε το εικονικό δίκτυο.
