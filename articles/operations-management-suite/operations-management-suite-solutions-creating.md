<properties
   pageTitle="Δημιουργία λύσεων διαχείρισης στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS) | Microsoft Azure"
   description="Διαχείριση λύσεων επεκτείνουν τη λειτουργικότητα της οικογένειας προγραμμάτων διαχείρισης λειτουργίες (OMS), παρέχοντας συσκευασμένη διαχείρισης σενάρια που μπορεί να προσθέσει τους πελάτες τους OMS χώρου εργασίας.  Σε αυτό το άρθρο παρέχει λεπτομέρειες σχετικά με πώς μπορείτε να δημιουργήσετε λύσεις διαχείρισης να χρησιμοποιηθεί με το δικό σας περιβάλλον ή καθίστανται διαθέσιμες στους πελάτες σας."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Δημιουργία λύσεων διαχείρισης σε λειτουργίες διαχείρισης οικογένεια προγραμμάτων (OMS) (έκδοση Preview)

>[AZURE.NOTE]Αυτό είναι προκαταρκτικού τεκμηρίωση για τη δημιουργία λύσεις διαχείρισης σε OMS που βρίσκονται τη συγκεκριμένη στιγμή σε προεπισκόπηση. Οποιαδήποτε διάταξη που περιγράφεται παρακάτω μπορεί να αλλάξουν.  

Διαχείριση λύσεων επεκτείνουν τη λειτουργικότητα της οικογένειας προγραμμάτων διαχείρισης λειτουργίες (OMS), παρέχοντας συσκευασμένη διαχείρισης σενάρια που μπορεί να προσθέσει τους πελάτες τους OMS χώρου εργασίας.  Σε αυτό το άρθρο παρέχει λεπτομέρειες σχετικά με τη δημιουργία του δικού σας λύσεις διαχείρισης που μπορείτε να χρησιμοποιήσετε στο δικό σας περιβάλλον ή να καταστήσετε διαθέσιμο στους πελάτες μέσω της Κοινότητας.

## <a name="planning-your-management-solution"></a>Σχεδιασμός τη λύση διαχείρισης
Οι λύσεις διαχείρισης στο OMS περιλαμβάνουν πολλούς πόρους υποστήριξης, ένα σενάριο συγκεκριμένο διαχείρισης.  Κατά το σχεδιασμό τη λύση σας, θα πρέπει να εστιάσετε σε το σενάριο που προσπαθείτε να επιτύχετε και όλους τους πόρους απαιτείται για την υποστήριξη του.  Κάθε λύση θα πρέπει να είναι που περιέχονται και τον ορισμό κάθε πόρο που απαιτεί, ακόμα και αν έναν ή περισσότερους πόρους επίσης ορίζονται από άλλες λύσεις.  Όταν έχει εγκατασταθεί μια λύση διαχείρισης, κάθε πόρο δημιουργείται εκτός αν υπάρχει ήδη, και μπορείτε να καθορίσετε τι συμβαίνει με πόρους όταν καταργείται μια λύση.  

Για παράδειγμα, μια λύση διαχείρισης ενδέχεται να περιλαμβάνουν μια [runbook αυτοματισμού Azure](../automation/automation-intro.md) που συλλέγει δεδομένα του αποθετηρίου καταγραφής αναλυτικών στοιχείων χρήσης ένα [Χρονοδιάγραμμα](../automation/automation-schedules.md) και μια [Προβολή](../log-analytics/log-analytics-view-designer.md) που παρέχει διάφορες απεικονίσεις των δεδομένων που έχουν συλλεχθεί.  Το ίδιο χρονοδιάγραμμα μπορεί να χρησιμοποιηθεί από μια άλλη λύση.  Ως συντάκτης λύσης διαχείρισης, που θα ορίσετε όλοι οι πόροι αλλά να καθορίσετε ότι η runbook και η προβολή θα πρέπει να καταργηθούν αυτόματα κατά την κατάργηση της λύσης.    Μπορείτε επίσης να ορίζουν το χρονοδιάγραμμα αλλά να καθορίσετε ότι αυτό θα πρέπει να παραμένουν στη θέση τους εάν η λύση έχουν καταργηθεί από την άλλη λύση σε περίπτωση που ήταν ακόμα.

## <a name="management-solution-files"></a>Διαχείριση αρχείων λύσης
Διαχείριση λύσεων υλοποιούνται ως [πρότυπα διαχείρισης πόρων](../resource-manager-template-walkthrough.md).  Η κύρια εργασία στην εκμάθηση της σύνταξης λύσεις διαχείρισης εκμάθησης τον τρόπο για να [συντάξετε ένα πρότυπο](../resource-group-authoring-templates.md).  Σε αυτό το άρθρο παρέχει μοναδικά λεπτομέρειες των προτύπων που χρησιμοποιούνται για λύσεις και τον τρόπο για να ορίσετε μια τυπική λύση πόρους.

Η βασική δομή ενός αρχείου λύσης διαχείρισης είναι ίδια με ένα [Πρότυπο από διαχειριστή πόρων](resource-group-authoring-templates.md#template-format) που είναι ως εξής.  Κάθε μία από τις ακόλουθες ενότητες περιγράφονται τα στοιχεία ανώτατου επιπέδου και και τα περιεχόμενά τους σε μια λύση.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Παράμετροι

[Παράμετροι](../resource-group-authoring-templates.md#parameters) είναι τιμές που απαιτείται από το χρήστη κατά την εγκατάσταση της λύσης διαχείρισης.  Υπάρχουν βασικές παράμετροι που θα έχει όλες τις λύσεις και μπορείτε να προσθέσετε επιπλέον παραμέτρους όπως απαιτείται για τη συγκεκριμένη λύση.  Πώς οι χρήστες θα παρέχει τιμές παραμέτρων κατά την εγκατάσταση τη λύση σας θα εξαρτώνται από τη συγκεκριμένη παράμετρο και τον τρόπο εγκατάστασης της λύσης.


Όταν ένας χρήστης εγκαθιστά τη λύση διαχείρισης μέσω του [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) ή [πρότυπα Azure γρήγορη έναρξη](operations-management-suite-solutions.md#finding-and-installing-solutions) θα σας ζητηθεί να επιλέξετε ένα [OMS χώρου εργασίας και αυτοματισμού λογαριασμού](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account).  Χρησιμοποιούνται για να συμπληρώσετε τις τιμές κάθε μία από τις τυπικές παραμέτρους.  Ο χρήστης δεν σας ζητηθεί να δώσετε απευθείας τιμές για τις τυπικές παραμέτρους, αλλά θα σας ζητηθεί να παράσχετε τιμές για όλες τις πρόσθετες παραμέτρους.

Όταν ο χρήστης εγκαθιστά τη λύση [κάποια άλλη μέθοδο](operations-management-suite-solutions.md#finding-and-installing-solutions), πρέπει να παρέχουν μια τιμή για όλες τις παραμέτρους του τυπική και όλες τις πρόσθετες παραμέτρους.

Δείγμα παραμέτρου εμφανίζεται παρακάτω.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Ο παρακάτω πίνακας περιγράφει τα χαρακτηριστικά μιας παραμέτρου.

| Χαρακτηριστικό | Περιγραφή |
|:--|:--|
| Τύπος        | Τύπος δεδομένων για την παράμετρο. Το στοιχείο ελέγχου εισόδου που εμφανίζεται για το χρήστη εξαρτάται από τον τύπο δεδομένων.<br><br>bool - πλαίσιο αναπτυσσόμενης λίστας<br>συμβολοσειρά - πλαίσιο κειμένου<br>Int - πλαίσιο κειμένου<br>SecureString - τον κωδικό πρόσβασης<br> |
| κατηγορία    | Προαιρετικό κατηγορία για την παράμετρο.  Παράμετροι στην ίδια κατηγορία ομαδοποιούνται. |
| στοιχείο ελέγχου     | Πρόσθετες λειτουργίες για παραμέτρους συμβολοσειράς.<br><br>εμφανίζεται το DateTime - ελέγχου ημερομηνίας/ώρας.<br>GUID - τιμή Guid δημιουργείται αυτόματα και η παράμετρος δεν εμφανίζεται. |
| Περιγραφή | Προαιρετική περιγραφή για την παράμετρο.  Εμφανίζεται σε ένα συννεφάκι πληροφορίες δίπλα την παράμετρο. |


### <a name="standard-parameters"></a>Τυπικές παράμετροι
Ο παρακάτω πίνακας παραθέτει τις τυπικές παραμέτρους για όλες τις λύσεις διαχείρισης.  Αυτές οι τιμές συμπληρώνονται για το χρήστη αντί να γίνεται ερώτηση για τους όταν η λύση είναι εγκατεστημένο από το Azure Marketplace ή τα πρότυπα γρήγορη έναρξη.  Ο χρήστης πρέπει να παρέχουν τιμές για αυτές, εάν έχει εγκατασταθεί η λύση με κάποια άλλη μέθοδο.

>[AZURE.NOTE]Το περιβάλλον εργασίας χρήστη στην Azure Marketplace και πρότυπα γρήγορη έναρξη αναμένει τα ονόματα παραμέτρων στον πίνακα.  Εάν χρησιμοποιείτε τα ονόματα παραμέτρων διαφορετικά, στη συνέχεια, ο χρήστης θα σας ζητηθεί για τους και αυτές θα δεν είναι συμπληρώνεται αυτόματα.


| Παράμετρος | Τύπος | Περιγραφή |
|:--|:--|:--|
| όνομα λογαριασμού       | συμβολοσειρά |  Όνομα λογαριασμού Azure αυτοματισμού. |
| pricingTier       | συμβολοσειρά | Πληροφορίες για την τιμολόγηση επίπεδο του χώρου εργασίας καταγραφής ανάλυσης και αυτοματισμού Azure λογαριασμού. |
| regionId          | συμβολοσειρά | Περιοχή ο λογαριασμός Azure αυτοματισμού. |
| όνομα λύσης      | συμβολοσειρά | Όνομα της λύσης. |
| workspaceName     | συμβολοσειρά | Όνομα χώρου εργασίας ανάλυση καταγραφής. |
| workspaceRegionId | συμβολοσειρά | Περιοχή του χώρου εργασίας καταγραφής ανάλυσης. |





### <a name="sample"></a>Δείγμα
Ακολουθεί ένα δείγμα οντότητα παραμέτρου για μια λύση.  Περιλαμβάνει όλες τις τυπικές παραμέτρους και τις δύο παραμέτρους επιπλέον στην ίδια κατηγορία.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Αναφέρεστε σε τιμές παραμέτρων σε άλλα στοιχεία της λύσης με τη σύνταξη **Παράμετροι ('όνομα παραμέτρου')**.  Για παράδειγμα, για να αποκτήσετε πρόσβαση στο όνομα του χώρου εργασίας, μπορείτε να χρησιμοποιήσετε **parameters('workspaceName')** 

## <a name="variables"></a>Μεταβλητές

Το στοιχείο **μεταβλητές** περιλαμβάνει τιμές που θέλετε να χρησιμοποιήσετε στο υπόλοιπο της λύσης διαχείρισης.  Αυτές οι τιμές δεν είναι εκτεθειμένη στο χρήστη κατά την εγκατάσταση της λύσης.  Προορίζονται για την παροχή ο συντάκτης με μία μόνο θέση όπου μπορούν να διαχειρίζονται τις τιμές που μπορούν να χρησιμοποιηθούν πολλές φορές σε ολόκληρη τη λύση. Θα πρέπει να τοποθετείτε τυχόν τιμές συγκεκριμένα τη λύση σας σε μεταβλητές σε αντίθεση με hardcoding τους στο στοιχείο **πόρους** .  Αυτό διευκολύνει την ανάγνωση του κώδικα και σας επιτρέπει να αλλάξετε εύκολα αυτές τις τιμές σε νεότερες εκδόσεις.

Ακολουθεί ένα παράδειγμα ενός στοιχείου **μεταβλητές** με τυπικές παραμέτρους χρησιμοποιείται σε λύσεις.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Αναφέρεστε σε τιμές μεταβλητής μέσω της λύσης με τη σύνταξη **μεταβλητές ("όνομα μεταβλητής")**.  Για παράδειγμα, για να αποκτήσετε πρόσβαση στη μεταβλητή όνομα λύσης, θα χρησιμοποιούσατε **variables('solutionName')** 


## <a name="resources"></a>Πόροι

Το στοιχείο **πόρους** καθορίζει διαφορετικό πόρων που περιλαμβάνονται στη λύση σας διαχείρισης.  Αυτό θα είναι το μεγαλύτερο μέγεθος και πιο σύνθετες τμήμα του προτύπου.  Πόροι ορίζονται με την ακόλουθη δομή.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Εξαρτήσεις
Τα στοιχεία **dependsOn** καθορίζει μια [εξάρτηση](../resource-group-define-dependencies.md) σε άλλον πόρο.  Όταν η λύση είναι εγκατεστημένο, δημιουργείται ένας πόρος δεν έως ότου όλες τις εξαρτήσεις έχουν δημιουργηθεί.  Για παράδειγμα, η λύση ενδέχεται να [ξεκινήσετε μια runbook](operations-management-suite-solutions-resources-automation.md#runbooks) κατά την εγκατάσταση χρησιμοποιώντας έναν [πόρο εργασίας](operations-management-suite-solutions-resources-automation.md#automation-jobs).  Ο πόρος εργασίας θα ήταν εξαρτάται από τον πόρο runbook για να βεβαιωθείτε ότι δημιουργούνται runbook πριν από τη δημιουργία της εργασίας.

### <a name="oms-workspace-and-automation-account"></a>Χώρος εργασίας OMS και αυτοματισμού λογαριασμό
Λύσεις διαχείρισης απαιτεί μια [OMS χώρου εργασίας](../log-analytics/log-analytics-manage-access.md) ώστε να περιέχει προβολές και ένα [λογαριασμό αυτοματισμού](../automation/automation-security-overview.md#automation-account-overview) που θα περιέχουν runbooks και σχετικοί πόροι.  Αυτά πρέπει να είναι διαθέσιμη για να τους πόρους στη λύση δημιουργούνται και δεν θα πρέπει να οριστούν στη λύση ίδια.  Ο χρήστης θα [Καθορίστε ένα χώρο εργασίας και το λογαριασμό](operations-management-suite-solutions.md#oms-workspace-and-automation-account) όταν ανάπτυξη τη λύση σας, αλλά ως συντάκτης που πρέπει να λάβετε υπόψη τα εξής σημεία.


## <a name="solution-resource"></a>Λύση πόρων
Κάθε λύση απαιτεί μια εγγραφή πόρων στο στοιχείο **πόρους** που ορίζει τη λύση ίδια.  Αυτό θα έχει έναν τύπο **Microsoft.OperationsManagement/solutions**.  Ακολουθεί ένα παράδειγμα ενός πόρου λύση.  Στις παρακάτω ενότητες περιγράφονται τα διαφορετικά στοιχεία.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Όνομα της λύσης
Το όνομα της λύσης πρέπει να είναι μοναδικό στο Azure τη συνδρομή σας. Για να χρησιμοποιήσετε την προτεινόμενη τιμή είναι τα εξής.  Αυτό χρησιμοποιεί μια μεταβλητή που ονομάζεται **όνομα λύσης** για το όνομα βάσης και την παράμετρο **workspaceName** για να βεβαιωθείτε ότι το όνομα είναι μοναδικό.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Αυτό θα επιλύσει σε ένα όνομα τομέα, όπως τα εξής.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Εξαρτήσεις
Ο πόρος λύσης πρέπει να έχετε μια [εξάρτηση](../resource-group-define-dependencies.md) σε κάθε άλλο πόρο στη λύση δεδομένου ότι πρέπει να υπάρχει, προκειμένου να μπορούν να δημιουργηθούν με τη λύση.  Μπορείτε να το κάνετε αυτό, προσθέτοντας μια καταχώρηση για κάθε πόρο στο στοιχείο **dependsOn** .


### <a name="properties"></a>Ιδιότητες
Ο πόρος λύση έχει τις ιδιότητες στον παρακάτω πίνακα.  Περιλαμβάνονται και οι πόροι στα οποία γίνεται αναφορά και περιέχονται από τη λύση, η οποία καθορίζει τον τρόπο διαχείρισης του πόρου μετά την εγκατάσταση της λύσης.  Κάθε πόρο στη λύση θα πρέπει να αναγράφεται στην ιδιότητα **containedResources** ή το **referencedResources** .

| Ιδιότητα | Περιγραφή |
|:--|:--|
| workspaceResourceId | Αναγνωριστικό του χώρου εργασίας OMS στη φόρμα * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<όνομα χώρου εργασίας\>*. |
| referencedResources   | Λίστα πόρων στη λύση που δεν θα πρέπει να καταργηθούν κατά την κατάργηση της λύσης. |
| containedResources   | Λίστα πόρων στη λύση που θα πρέπει να καταργηθούν κατά την κατάργηση της λύσης. |

Το παραπάνω παράδειγμα είναι μια λύση με έναν runbook, ένα χρονοδιάγραμμα και προβολή.  Το χρονοδιάγραμμα και runbook είναι *αναφοράς* στο στοιχείο **Ιδιότητες** , ώστε να δεν καταργούνται κατά την κατάργηση της λύσης.  Η προβολή είναι *που περιέχονται* , ώστε να καταργείται κατά την κατάργηση της λύσης.


### <a name="plan"></a>Σχεδιασμός
Η οντότητα **πρόγραμμα** του πόρου λύση περιλαμβάνει τις ιδιότητες στον παρακάτω πίνακα. 

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Όνομα          | Όνομα της λύσης. |
| έκδοση       | Έκδοση της λύσης που καθορίζεται από το συντάκτη. |
| προϊόν       | Μοναδική συμβολοσειρά για τον προσδιορισμό της λύσης. |
| ο Publisher     | Εκδότης της λύσης. |


## <a name="other-resources"></a>Άλλοι πόροι
Μπορείτε να λάβετε τις λεπτομέρειες και δείγματα των πόρων που είναι κοινά και για λύσεις διαχείρισης στα ακόλουθα άρθρα.

- [Προβολές και πίνακες εργαλείων](operations-management-suite-solutions-resources-views.md)
- [Αυτοματοποίηση πόροι](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Δοκιμή μια λύση διαχείρισης
Πριν από την ανάπτυξη της λύσης διαχείρισης σας, συνιστάται να ελέγξετε χρησιμοποιώντας [AzureRmResourceGroupDeployment δοκιμής](../resource-group-template-deploy.md#deploy-with-powershell).  Αυτό θα επικυρώσει το αρχείο λύσης και σας βοηθούν να αναγνωρίσετε τυχόν προβλήματα πριν να επιχειρήσετε να το αναπτύξετε.



## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε τις λεπτομέρειες της [σύνταξης από διαχειριστή πόρων Azure πρότυπα](../resource-group-authoring-templates.md).
- Αναζήτηση [Προτύπων γρήγορη έναρξη Azure](https://azure.microsoft.com/documentation/templates) για δείγματα διαφορετικά πρότυπα για τη διαχείριση πόρων.
- Προβάλετε τις λεπτομέρειες για την [Προσθήκη προβολών σε μια λύση διαχείρισης](operations-management-suite-solutions-resources-views.md).
- Προβάλετε τις λεπτομέρειες για την [Προσθήκη αυτοματισμού πόρων για μια λύση διαχείρισης](operations-management-suite-solutions-resources-automation.md).
 