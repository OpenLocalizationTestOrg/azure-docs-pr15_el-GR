<properties
    pageTitle="RBAC: Ενσωματωμένη ρόλους | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει στο ενσωματωμένο σε ρόλους για έλεγχο πρόσβασης βάσει ρόλων (RBAC)."
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
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Ενσωματωμένη ρόλους

Azure βάσει ρόλων πρόσβασης ελέγχου (RBAC) συνοδεύεται από τους εξής ρόλους ενσωματωμένη που μπορεί να αντιστοιχιστεί σε χρήστες, ομάδες και τις υπηρεσίες. Δεν μπορείτε να τροποποιήσετε οι ορισμοί των ενσωματωμένων ρόλους. Ωστόσο, μπορείτε να δημιουργήσετε [προσαρμοσμένες τους ρόλους στο Azure RBAC](role-based-access-control-custom-roles.md) ώστε να χωρά τις συγκεκριμένες ανάγκες της εταιρείας σας.

## <a name="roles-in-azure"></a>Ρόλοι στο Azure

Ο παρακάτω πίνακας παρέχει σύντομες περιγραφές των ενσωματωμένων ρόλων. Κάντε κλικ στο όνομα ρόλο για να δείτε τη λεπτομερή λίστα των **ενεργειών** και των **notactions** για το ρόλο. Η ιδιότητα **Ενέργειες** καθορίζει τις ενέργειες επιτρεπόμενων Azure πόρους. Η ενέργεια συμβολοσειρές μπορούν να χρησιμοποιούν χαρακτήρες μπαλαντέρ. Η ιδιότητα **notactions** καθορίζει τις ενέργειες που εξαιρούνται από το επιτρεπόμενο ενέργειες.

>[AZURE.NOTE] Οι ορισμοί Azure ρόλο συνεχώς σε εξέλιξη. Σε αυτό το άρθρο διατηρείται ως ενημερωμένα όσο το δυνατόν πιο, αλλά μπορείτε να βρείτε πάντα οι πιο πρόσφατες ορισμοί ρόλων σε Azure PowerShell. Χρησιμοποιήστε τα cmdlet `(get-azurermroledefinition "<role name>").actions` ή `(get-azurermroledefinition "<role name>").notactions` ανάλογα με την περίπτωση.

| Όνομα ρόλου | Περιγραφή |
| --------- | ----------- |
| [API διαχείρισης υπηρεσίας συμβολής](#api-management-service-contributor) | Να διαχειριστείτε υπηρεσίες API διαχείρισης |
| [Εφαρμογή ιδέες στοιχείο συμβολής](#application-insights-component-contributor) | Να διαχειριστείτε στοιχεία ιδέες εφαρμογής |
| [Τελεστής αυτοματισμού](#automation-operator) | Μπορείτε να ξεκινήσετε, να διακόψετε, αναστολή και συνεχίστε εργασίες |
| [Συνεργάτης BizTalk](#biztalk-contributor) | Να διαχειριστείτε τις υπηρεσίες BizTalk |
| [ClearDB MySQL DB συμβολής](#cleardb-mysql-db-contributor) | Να διαχειριστείτε βάσεις δεδομένων ClearDB MySQL |
| [Συμβολής](#contributor) | Για να διαχειριστείτε τα πάντα εκτός από την access. |
| [Συνεργάτης εργοστασίου δεδομένων](#data-factory-contributor) | Να δημιουργήσετε και να διαχειριστείτε εργοστάσια δεδομένων και τους πόρους θυγατρικό μέσα σε αυτά. |
| [DevTest Labs χρήστη](#devtest-labs-user) | Να προβάλετε όλα τα στοιχεία και να συνδεθείτε, Έναρξη και επανεκκίνηση τερματισμού εικονικές μηχανές |
| [Ζώνη DNS συμβολής](#dns-zone-contributor) | Να διαχειριστείτε ζώνες DNS και εγγραφές |
| [Λογαριασμός DocumentDB συμβολής](#documentdb-account-contributor) | Να διαχειριστείτε τους λογαριασμούς DocumentDB |
| [Έξυπνη συστήματα λογαριασμό συμβολής](#intelligent-systems-account-contributor) | Να διαχειριστείτε τους λογαριασμούς έξυπνη συστήματα |
| [Συνεργάτης δικτύου](#network-contributor) | Μπορούν να διαχειριστούν όλους τους πόρους δικτύου |
| [Νέα Relic APM λογαριασμό συμβολής](#new-relic-apm-account-contributor) | Να διαχειριστείτε τους λογαριασμούς νέα διαχείριση επιδόσεων εφαρμογών Relic και εφαρμογές |
| [Κάτοχος](#owner) | Μπορούν να διαχειρίζονται τα πάντα, συμπεριλαμβανομένης της πρόσβασης |
| [Πρόγραμμα ανάγνωσης](#reader) | Να προβάλετε τα πάντα, αλλά δεν είναι δυνατό να κάνετε αλλαγές |
| [Redis Cache συμβολής](#redis-cache-contributor) | Να διαχειριστείτε Redis μνήμης cache |
| [Χρονοδιάγραμμα έργου συλλογές συμβολής](#scheduler-job-collections-contributor) | Να διαχειρίζεστε συλλογές χρονοδιαγράμματος έργου |
| [Συνεργάτης υπηρεσίας αναζήτησης](#search-service-contributor) | Να διαχειριστείτε υπηρεσίες αναζήτησης |
| [Διαχείριση ασφαλείας](#security-manager) | Να διαχειριστείτε στοιχεία ασφαλείας, πολιτικές ασφάλειας και εικονικές μηχανές |
| [Συνεργάτης DB SQL](#sql-db-contributor) | Να διαχειριστείτε βάσεις δεδομένων SQL, αλλά όχι τις πολιτικές που σχετίζονται με την ασφάλεια |
| [Διαχείριση ασφαλείας SQL](#sql-security-manager) | Να διαχειριστείτε τις πολιτικές που σχετίζονται με την ασφάλεια των διακομιστών SQL και βάσεων δεδομένων |
| [SQL Server συμβολής](#sql-server-contributor) | Να διαχειριστείτε τους διακομιστές SQL και βάσεων δεδομένων, αλλά όχι τις πολιτικές που σχετίζονται με την ασφάλεια |
| [Κλασική αποθήκευσης λογαριασμού συμβολής](#classic-storage-account-contributor) | Να διαχειριστείτε τους λογαριασμούς κλασική χώρου αποθήκευσης |
| [Συνεργάτης λογαριασμού χώρου αποθήκευσης](#storage-account-contributor) | Να διαχειριστείτε τους λογαριασμούς χώρου αποθήκευσης |
| [Διαχειριστής πρόσβασης χρήστη](#user-access-administrator) | Να διαχειριστείτε την πρόσβαση χρηστών σε πόρους Azure |
| [Κλασική εικονική μηχανή συμβολής](#classic-virtual-machine-contributor) | Να διαχειριστείτε κλασική εικονικές μηχανές, αλλά όχι το εικονικό δίκτυο ή χώρο αποθήκευσης λογαριασμό στο οποίο είστε συνδεδεμένοι |
| [Εικονική μηχανή συμβολής](#virtual-machine-contributor) | Να διαχειριστείτε εικονικές μηχανές, αλλά όχι το εικονικό δίκτυο ή χώρο αποθήκευσης λογαριασμό στο οποίο είστε συνδεδεμένοι |
| [Κλασική δικτύου συμβολής](#classic-network-contributor) | Να διαχειριστείτε κλασική εικονικών δικτύων και δεσμευμένες διευθύνσεις IP |
| [Συνεργάτης σχεδίου Web](#web-plan-contributor) | Μπορούν να διαχειρίζονται τα σχέδια web |
| [Τοποθεσία Web συνεργάτη](#website-contributor) | Μπορούν να διαχειρίζονται τοποθεσίες Web, αλλά όχι τα σχέδια web στην οποία είστε συνδεδεμένοι |

## <a name="role-permissions"></a>Δικαιώματα ρόλων
Στους πίνακες που ακολουθούν περιγράφουν τα συγκεκριμένα δικαιώματα που δίνονται σε κάθε ρόλο. Αυτό μπορεί να περιλαμβάνει **Ενέργειες**, που θα σας δώσει δικαιώματα, και **NotActions**, οι οποίες περιορισμός τους.

### <a name="api-management-service-contributor"></a>API διαχείρισης υπηρεσίας συμβολής
Να διαχειριστείτε υπηρεσίες API διαχείρισης

| **Ενέργειες** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Δημιουργία και διαχείριση υπηρεσιών διαχείρισης API |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τους ρόλους και εκχωρήσεις ρόλων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="application-insights-component-contributor"></a>Εφαρμογή ιδέες στοιχείο συμβολής
Να διαχειριστείτε στοιχεία ιδέες εφαρμογής

| **Ενέργειες** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και εκχωρήσεις ρόλων |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.Insights/components/* | Δημιουργία και διαχείριση στοιχείων ιδέες |
| Microsoft.Insights/webtests/* | Δημιουργία και διαχείριση δοκιμές web |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="automation-operator"></a>Τελεστής αυτοματισμού
Μπορείτε να ξεκινήσετε, να διακόψετε, αναστολή και συνεχίστε εργασίες

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και εκχωρήσεις ρόλων |
| Microsoft.Automation/automationAccounts/jobs/read | Διαβάστε αυτοματοποίηση εργασιών λογαριασμού |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Συνέχιση μιας εργασίας λογαριασμού αυτοματισμού |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Διακοπή μια εργασία λογαριασμό αυτοματισμού |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Διαβάστε αυτοματισμού λογαριασμό ροών εργασίας |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Αναστολή μια εργασία λογαριασμό αυτοματισμού |
| Microsoft.Automation/automationAccounts/jobs/write | Γράψτε αυτοματοποίηση εργασιών λογαριασμού |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Διαβάστε μια οικονομική κατάσταση εργασίας αυτοματισμού |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Διαβάστε μια οικονομική κατάσταση εργασίας αυτοματισμού |
| Microsoft.Automation/automationAccounts/read | Ανάγνωση αυτοματισμού λογαριασμών |
| Microsoft.Automation/automationAccounts/runbooks/read | Διαβάστε runbooks αυτοματισμού |
| Microsoft.Automation/automationAccounts/schedules/read | Διαβάστε αυτοματισμού οικονομικές καταστάσεις |
| Microsoft.Automation/automationAccounts/schedules/write | Γράψτε αυτοματισμού οικονομικές καταστάσεις |
| Microsoft.Insights/components/* | Δημιουργία και διαχείριση στοιχείων ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="biztalk-contributor"></a>Συνεργάτης BizTalk
Να διαχειριστείτε τις υπηρεσίες BizTalk

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και εκχωρήσεις ρόλων |
| Microsoft.BizTalkServices/BizTalk/* | Δημιουργία και διαχείριση των υπηρεσιών BizTalk |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB συμβολής
Να διαχειριστείτε βάσεις δεδομένων ClearDB MySQL

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και εκχωρήσεις ρόλων |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |
| successbricks.cleardb/Databases/* | Δημιουργία και διαχείριση βάσεων δεδομένων ClearDB MySQL |

### <a name="contributor"></a>Συμβολής
Μπορούν να διαχειρίζονται τα πάντα εκτός από την access

| **Ενέργειες** ||
| ------- | ------ |
| * | Δημιουργία και διαχείριση πόρων από όλους τους τύπους |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Δεν είναι δυνατό να διαγράψετε τους ρόλους και εκχωρήσεις ρόλων |  
| Microsoft.Authorization/*/Write | Δεν είναι δυνατό να δημιουργήσετε ρόλους και εκχωρήσεις ρόλων |

### <a name="data-factory-contributor"></a>Συνεργάτης εργοστασίου δεδομένων
Δημιουργήστε και διαχειριστείτε εργοστάσια δεδομένων και τους πόρους θυγατρικό μέσα σε αυτά.

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.DataFactory/dataFactories/* | Δημιουργήστε και διαχειριστείτε εργοστάσια δεδομένων και τους πόρους θυγατρικό μέσα σε αυτά. |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="devtest-labs-user"></a>DevTest Labs χρήστη
Να προβάλετε όλα τα στοιχεία και να συνδεθείτε, Έναρξη και επανεκκίνηση τερματισμού εικονικές μηχανές

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Compute/availabilitySets/read | Διαβάστε τις ιδιότητες των συνόλων διαθεσιμότητα |
| Microsoft.Compute/virtualMachines/*/read | Διαβάστε τις ιδιότητες μια εικονική μηχανή (Εικονική μεγέθη, κατάσταση χρόνου εκτέλεσης, Εικονική επεκτάσεις, κ.λπ.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Κατάργηση εκχώρησης εικονικές μηχανές |
| Microsoft.Compute/virtualMachines/read | Διαβάστε τις ιδιότητες μια εικονική μηχανή |
| Microsoft.Compute/virtualMachines/restart/action | Επανεκκινήστε εικονικές μηχανές |
| Microsoft.Compute/virtualMachines/start/action | Έναρξη εικονικές μηχανές |
| Microsoft.DevTestLab/*/read | Διαβάστε τις ιδιότητες ενός εργαστήριο |
| Microsoft.DevTestLab/labs/createEnvironment/action | Δημιουργία περιβάλλοντος εργαστήριο |
| Microsoft.DevTestLab/labs/formulas/delete | Διαγραφή τύπων |
| Microsoft.DevTestLab/labs/formulas/read | Διαβάστε τους τύπους |
| Microsoft.DevTestLab/labs/formulas/write | Προσθήκη ή τροποποίηση τύπων |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Αξιολόγηση πολιτικές εργαστήριο |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Συμμετοχή σε ένα χώρο συγκέντρωσης διεύθυνση φόρτωσης εξισορρόπησης παρασκηνίου |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Συμμετοχή σε μια μονάδα εξισορρόπησης φόρτου κανόνα εισερχομένων NAT |
| Microsoft.Network/networkInterfaces/*/read | Διαβάστε τις ιδιότητες ενός δικτύου περιβάλλοντος εργασίας (για παράδειγμα, όλα τα φόρτωσης balancers που το περιβάλλον εργασίας δικτύου είναι μέρος) |
| Microsoft.Network/networkInterfaces/join/action | Συμμετοχή σε μια εικονική μηχανή σε ένα περιβάλλον εργασίας δικτύου |
| Microsoft.Network/networkInterfaces/read | Διαβάστε διασυνδέσεις δικτύου |
| Microsoft.Network/networkInterfaces/write | Γράψτε διασυνδέσεις δικτύου |
| Microsoft.Network/publicIPAddresses/*/read | Διαβάστε τις ιδιότητες μια δημόσια διεύθυνση IP |
| Microsoft.Network/publicIPAddresses/join/action | Συμμετοχή σε μια δημόσια διεύθυνση IP |
| Microsoft.Network/publicIPAddresses/read | Ανάγνωση δικτύου δημόσιες διευθύνσεις IP |
| Microsoft.Network/virtualNetworks/subnets/join/action | Συμμετοχή σε ένα εικονικό δίκτυο |
| Microsoft.Resources/deployments/operations/read | Ανάπτυξη λειτουργίες ανάγνωσης |
| Microsoft.Resources/deployments/read | Διαβάστε αναπτύξεις |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Storage/storageAccounts/listKeys/action | Πλήκτρα για το λογαριασμό χώρου αποθήκευσης λίστας |

### <a name="dns-zone-contributor"></a>Ζώνη DNS συμβολής
Να διαχειριστείτε ζώνες DNS και εγγραφές.

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/\*/ανάγνωση | Διαβάστε τους ρόλους και εκχωρήσεις ρόλων |
| Microsoft.Insights/alertRules/\* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.Network/dnsZones/\* | Δημιουργία και διαχείριση ζώνες DNS και εγγραφές |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε την εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/\* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/\* | Δημιουργία και διαχείριση δελτίων υποστήριξης |


### <a name="documentdb-account-contributor"></a>Λογαριασμός DocumentDB συμβολής
Να διαχειριστείτε τους λογαριασμούς DocumentDB

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.DocumentDb/databaseAccounts/* | Δημιουργία και διαχείριση λογαριασμών DocumentDB |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="intelligent-systems-account-contributor"></a>Έξυπνη συστήματα λογαριασμό συμβολής
Να διαχειριστείτε τους λογαριασμούς έξυπνη συστήματα

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.IntelligentSystems/accounts/* | Δημιουργία και διαχείριση λογαριασμών έξυπνη συστήματα |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="network-contributor"></a>Συνεργάτης δικτύου
Μπορούν να διαχειριστούν όλους τους πόρους δικτύου

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.Network/* | Δημιουργία και διαχείριση δίκτυα |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="new-relic-apm-account-contributor"></a>Νέα Relic APM λογαριασμό συμβολής
Να διαχειριστείτε τους λογαριασμούς νέα διαχείριση επιδόσεων εφαρμογών Relic και εφαρμογές

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |
| NewRelic.APM/accounts/* | Δημιουργία και διαχείριση Relic νέα εφαρμογή επιδόσεων διαχείρισης λογαριασμών |

### <a name="owner"></a>Κάτοχος
Μπορούν να διαχειρίζονται τα πάντα, συμπεριλαμβανομένης της πρόσβασης

| **Ενέργειες** ||
| ------- | ------ |
| * | Δημιουργία και διαχείριση πόρων από όλους τους τύπους |

### <a name="reader"></a>Πρόγραμμα ανάγνωσης
Να προβάλετε τα πάντα, αλλά δεν είναι δυνατό να κάνετε αλλαγές

| **Ενέργειες** ||
| ------- | ------ |
| * / ανάγνωσης | Διαβάστε τους πόρους από όλους τους τύπους, εκτός από απορρήτου. |

### <a name="redis-cache-contributor"></a>Redis Cache συμβολής
Να διαχειριστείτε Redis μνήμης cache

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Cache/redis/* | Δημιουργία και διαχείριση μνήμης cache Redis |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="scheduler-job-collections-contributor"></a>Χρονοδιάγραμμα έργου συλλογές συμβολής
Να διαχειρίζεστε συλλογές χρονοδιαγράμματος έργου

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Scheduler/jobcollections/* | Δημιουργία και διαχείριση συλλογών εργασία |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης  |

### <a name="search-service-contributor"></a>Συνεργάτης υπηρεσίας αναζήτησης
Να διαχειριστείτε υπηρεσίες αναζήτησης

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Search/searchServices/* | Δημιουργία και διαχείριση των υπηρεσιών αναζήτησης |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης  |

### <a name="security-manager"></a>Διαχείριση ασφαλείας
Να διαχειριστείτε στοιχεία ασφαλείας, πολιτικές ασφάλειας και εικονικές μηχανές

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.ClassicCompute/*/read | Διαβάστε πληροφορίες ρύθμισης παραμέτρων κλασική τον υπολογισμό εικονικές μηχανές |
| Microsoft.ClassicCompute/virtualMachines/*/write | Ρύθμιση παραμέτρων για εικονικές μηχανές εγγραφής |
| Microsoft.ClassicNetwork/*/read | Διαβάστε πληροφορίες ρύθμισης παραμέτρων για κλασική δικτύου  |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Security/* | Δημιουργία και διαχείριση των στοιχείων ασφαλείας και τις πολιτικές |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης  |

### <a name="sql-db-contributor"></a>Συνεργάτης DB SQL
Να διαχειριστείτε βάσεις δεδομένων SQL, αλλά όχι τις πολιτικές που σχετίζονται με την ασφάλεια

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε τους ρόλους και ρόλων αναθέσεις |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων προειδοποίησης |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Sql/servers/databases/* | Δημιουργία και διαχείριση βάσεων δεδομένων SQL |
| Microsoft.Sql/servers/read | Διαβάστε τους διακομιστές SQL |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Δεν είναι δυνατή η επεξεργασία πολιτικές ελέγχου |
| Microsoft.Sql/servers/databases/auditingSettings/* | Δεν είναι δυνατή η επεξεργασία ρυθμίσεων ελέγχου |
| Microsoft.Sql/servers/databases/auditRecords/read  | Δεν μπορεί να διαβάσει εγγραφών ελέγχου |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Δεν είναι δυνατή η επεξεργασία πολιτικές σύνδεσης |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Δεν είναι δυνατή η επεξεργασία δεδομένων που masking πολιτικές |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Δεν είναι δυνατή η επεξεργασία ειδοποίησης πολιτικές ασφαλείας |
| Microsoft.Sql/servers/databases/securityMetrics/* | Δεν είναι δυνατή η επεξεργασία μετρικά ασφαλείας |

### <a name="sql-security-manager"></a>Διαχείριση ασφαλείας SQL
Να διαχειριστείτε τις πολιτικές που σχετίζονται με την ασφάλεια των διακομιστών SQL και βάσεων δεδομένων

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Ανάγνωση Microsoft εξουσιοδότησης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Sql/servers/auditingPolicies/* | Δημιουργία και διαχείριση πολιτικές ελέγχου του SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Δημιουργία και διαχείριση ελέγχου ρύθμιση του SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Δημιουργία και διαχείριση πολιτικές ελέγχου βάσης δεδομένων SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Δημιουργία και διαχείριση ρυθμίσεων ελέγχου βάσης δεδομένων SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read | Ανάγνωση εγγραφών ελέγχου |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Δημιουργία και διαχείριση πολιτικές σύνδεσης βάσης δεδομένων SQL server |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Δημιουργία και διαχείριση SQL server δεδομένα βάσης δεδομένων masking πολιτικές |
| Microsoft.Sql/servers/databases/read | Βάσεις δεδομένων SQL ανάγνωση |
| Microsoft.Sql/servers/databases/schemas/read | Ανάγνωση SQL server βάση δεδομένων σχημάτων |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Στήλες πίνακα SQL ανάγνωση διακομιστή βάσης δεδομένων |
| Microsoft.Sql/servers/databases/schemas/tables/read | Πίνακες βάσης δεδομένων ανάγνωση SQL server |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Δημιουργία και διαχείριση των πολιτικών προειδοποίησης ασφαλείας βάσης δεδομένων διακομιστή SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* | Δημιουργία και διαχείριση μετρικά ασφαλείας βάσης δεδομένων του SQL server |
| Microsoft.Sql/servers/read | Διαβάστε τους διακομιστές SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* | Δημιουργία και διαχείριση πολιτικών προειδοποίησης ασφαλείας SQL server |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="sql-server-contributor"></a>SQL Server συμβολής
Να διαχειριστείτε τους διακομιστές SQL και βάσεις δεδομένων αλλά όχι τις πολιτικές που σχετίζονται με την ασφάλεια

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης|
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Sql/servers/* | Δημιουργία και διαχείριση διακομιστών SQL |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Δεν είναι δυνατή η επεξεργασία του SQL server πολιτικές ελέγχου |
| Microsoft.Sql/servers/auditingSettings/* | Δεν είναι δυνατή η επεξεργασία του SQL server ρυθμίσεις ελέγχου |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Δεν είναι δυνατή η επεξεργασία πολιτικές ελέγχου βάσης δεδομένων SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Δεν είναι δυνατή η επεξεργασία ρυθμίσεις ελέγχου βάσης δεδομένων SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read  | Δεν μπορεί να διαβάσει εγγραφών ελέγχου |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Δεν είναι δυνατή η επεξεργασία πολιτικές σύνδεσης βάσης δεδομένων SQL server |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Δεν είναι δυνατή η επεξεργασία SQL server δεδομένα βάσης δεδομένων masking πολιτικές |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Δεν είναι δυνατή η επεξεργασία πολιτικές προειδοποίησης ασφαλείας βάσης δεδομένων διακομιστή SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* | Δεν είναι δυνατή η επεξεργασία μετρικά ασφαλείας βάσης δεδομένων του SQL server |
| Microsoft.Sql/servers/securityAlertPolicies/* | Δεν είναι δυνατή η επεξεργασία πολιτικές προειδοποίησης ασφαλείας SQL server |

### <a name="classic-storage-account-contributor"></a>Κλασική αποθήκευσης λογαριασμού συμβολής
Να διαχειριστείτε τους λογαριασμούς κλασική χώρου αποθήκευσης

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.ClassicStorage/storageAccounts/* | Δημιουργία και διαχείριση λογαριασμών χώρου αποθήκευσης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="storage-account-contributor"></a>Συνεργάτης λογαριασμού χώρου αποθήκευσης
Να διαχειριστείτε τους λογαριασμούς χώρου αποθήκευσης, αλλά δεν έχουν πρόσβαση σε αυτά.

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε όλα εξουσιοδότησης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.Insights/diagnosticSettings/* | Διαχείριση ρυθμίσεων διαγνωστικών |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Storage/storageAccounts/* | Δημιουργία και διαχείριση λογαριασμών χώρου αποθήκευσης |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="user-access-administrator"></a>Διαχειριστής πρόσβασης χρήστη
Να διαχειριστείτε την πρόσβαση χρηστών σε πόρους Azure

| **Ενέργειες** ||
| ------- | ------ |
| * / ανάγνωσης | Διαβάστε τους πόρους από όλους τους τύπους, εκτός από απορρήτου. |
| Microsoft.Authorization/* | Διαχείριση εξουσιοδότησης |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="classic-virtual-machine-contributor"></a>Κλασική εικονική μηχανή συμβολής
Να διαχειριστείτε κλασική εικονικές μηχανές, αλλά όχι το εικονικό δίκτυο ή χώρο αποθήκευσης λογαριασμό στο οποίο είστε συνδεδεμένοι

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.ClassicCompute/domainNames/* | Δημιουργία και διαχείριση ονομάτων τομέα κλασική υπολογισμού |
| Microsoft.ClassicCompute/virtualMachines/* | Δημιουργία και διαχείριση εικονικές μηχανές |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Συμμετοχή σε ομάδες ασφαλείας δικτύου |
| Microsoft.ClassicNetwork/reservedIps/link/action | Σύνδεση δεσμευμένες διευθύνσεις IP |
| Microsoft.ClassicNetwork/reservedIps/read | Ανάγνωση δεσμευμένες διευθύνσεις IP |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Συμμετοχή σε εικονικών δικτύων |
| Microsoft.ClassicNetwork/virtualNetworks/read | Διαβάστε εικονικών δικτύων |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Ανάγνωση δίσκων λογαριασμού χώρου αποθήκευσης |
| Microsoft.ClassicStorage/storageAccounts/images/read | Διαβάστε εικόνες λογαριασμού χώρου αποθήκευσης |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Πλήκτρα για το λογαριασμό χώρου αποθήκευσης λίστας |
| Microsoft.ClassicStorage/storageAccounts/read | Διαβάστε τους λογαριασμούς κλασική χώρου αποθήκευσης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="virtual-machine-contributor"></a>Εικονική μηχανή συμβολής
Να διαχειριστείτε εικονικές μηχανές, αλλά όχι το εικονικό δίκτυο ή χώρο αποθήκευσης λογαριασμό στο οποίο είστε συνδεδεμένοι

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.Compute/availabilitySets/* | Δημιουργία και διαχείριση συνόλων διαθεσιμότητα υπολογισμού |
| Microsoft.Compute/locations/* | Δημιουργία και διαχείριση θέσεις υπολογισμού |
| Microsoft.Compute/virtualMachines/* | Δημιουργία και διαχείριση εικονικές μηχανές |
| Microsoft.Compute/virtualMachineScaleSets/* | Δημιουργία και διαχείριση συνόλων κλίμακα εικονική μηχανή |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Συμμετοχή σε χώρους συγκέντρωσης διεύθυνση παρασκηνίου εφαρμογών πύλης δικτύου |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Συμμετοχή σε χώρους συγκέντρωσης διεύθυνση παρασκηνίου εξισορρόπησης φόρτου |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Συμμετοχή σε μονάδα εξισορρόπησης φόρτου εισερχομένων NAT χώρους συγκέντρωσης |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Συμμετοχή σε μονάδα εξισορρόπησης φόρτου NAT κανόνες εισερχομένων |
| Microsoft.Network/loadBalancers/read | Ανάγνωση φόρτωσης balancers |
| Microsoft.Network/locations/* | Δημιουργία και διαχείριση θέσεις δικτύου |
| Microsoft.Network/networkInterfaces/* | Δημιουργία και διαχείριση διασυνδέσεις δικτύου |
| Microsoft.Network/networkSecurityGroups/join/action | Συμμετοχή σε ομάδες ασφαλείας δικτύου |
| Microsoft.Network/networkSecurityGroups/read | Διαβάστε τις ομάδες ασφαλείας δικτύου |
| Microsoft.Network/publicIPAddresses/join/action | Συμμετοχή σε δίκτυο δημόσιων διευθύνσεων IP |
| Microsoft.Network/publicIPAddresses/read | Ανάγνωση δικτύου δημόσιες διευθύνσεις IP |
| Microsoft.Network/virtualNetworks/read | Διαβάστε εικονικών δικτύων |
| Microsoft.Network/virtualNetworks/subnets/join/action | Συμμετοχή σε δευτερεύοντα δίκτυα εικονικού δικτύου |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Storage/storageAccounts/listKeys/action | Πλήκτρα για το λογαριασμό χώρου αποθήκευσης λίστας |
| Microsoft.Storage/storageAccounts/read | Διαβάστε τους λογαριασμούς χώρου αποθήκευσης |
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="classic-network-contributor"></a>Κλασική δικτύου συμβολής
Να διαχειριστείτε κλασική εικονικών δικτύων και δεσμευμένες διευθύνσεις IP

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.ClassicNetwork/* | Δημιουργία και διαχείριση κλασική δίκτυα |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |

### <a name="web-plan-contributor"></a>Συνεργάτης σχεδίου Web
Μπορούν να διαχειρίζονται τα σχέδια web

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |
| Microsoft.Web/serverFarms/* | Δημιουργία και διαχείριση συστοιχιών διακομιστή |

### <a name="website-contributor"></a>Τοποθεσία Web συνεργάτη
Μπορούν να διαχειρίζονται τοποθεσίες Web, αλλά όχι τα σχέδια web στην οποία είστε συνδεδεμένοι

| **Ενέργειες** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Διαβάστε εξουσιοδότησης |
| Microsoft.Insights/alertRules/* | Δημιουργία και Διαχείριση κανόνων ειδοποίησης ιδέες |
| Microsoft.Insights/components/* | Δημιουργία και διαχείριση στοιχείων ιδέες |
| Microsoft.ResourceHealth/availabilityStatuses/read | Διαβάστε εύρυθμη λειτουργία των πόρων |
| Microsoft.Resources/deployments/* | Δημιουργία και διαχείριση αναπτύξεων ομάδα πόρων |
| Microsoft.Resources/subscriptions/resourceGroups/read | Διαβάστε τις ομάδες πόρων |  
| Microsoft.Support/* | Δημιουργία και διαχείριση δελτίων υποστήριξης |
| Microsoft.Web/certificates/* | Δημιουργία και διαχείριση πιστοποιητικών τοποθεσίας Web |
| Microsoft.Web/listSitesAssignedToHostName/read | Διαβάστε τις τοποθεσίες που έχουν εκχωρηθεί σε ένα όνομα κεντρικού υπολογιστή |
| Microsoft.Web/serverFarms/join/action | Συμμετοχή σε συστοιχίες διακομιστών |
| Microsoft.Web/serverFarms/read | Διαβάστε συστοιχίες διακομιστών |
| Microsoft.Web/sites/* | Δημιουργία και διαχείριση τοποθεσιών Web |

## <a name="see-also"></a>Δείτε επίσης
- [Έλεγχος πρόσβασης βάσει ρόλων](role-based-access-control-configure.md): γρήγορα αποτελέσματα με το RBAC στην πύλη του Azure.
- [Προσαρμογή τους ρόλους στο Azure RBAC](role-based-access-control-custom-roles.md): Μάθετε πώς μπορείτε να δημιουργήσετε προσαρμοσμένες ρόλους στις ανάγκες σας πρόσβαση.
- [Δημιουργία πρόσβαση σε αλλαγή αναφορά ιστορικού](role-based-access-control-access-change-history-report.md): παρακολούθηση της αλλαγής εκχωρήσεις ρόλων στο RBAC.
- [Έλεγχος πρόσβασης βάσει ρόλων αντιμετώπιση προβλημάτων](role-based-access-control-troubleshooting.md): λήψη υποδείξεων για τη διόρθωση κοινών ζητημάτων.
