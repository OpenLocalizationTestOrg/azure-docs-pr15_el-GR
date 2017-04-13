<properties
   pageTitle="Προβολές στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS) λύσεις διαχείρισης | Microsoft Azure"
   description="Οι λύσεις διαχείρισης στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS) συνήθως θα περιλαμβάνουν μία ή περισσότερες προβολές για την απεικόνιση δεδομένων.  Σε αυτό το άρθρο περιγράφει τον τρόπο για να εξαγάγετε μια προβολή που δημιουργήθηκε από την προβολή σχεδίασης και να συμπεριλάβετε σε μια λύση διαχείρισης. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Προβολές σε λύσεις διαχείρισης λειτουργίες διαχείρισης οικογένεια προγραμμάτων (OMS) (έκδοση Preview)

>[AZURE.NOTE]Αυτό είναι προκαταρκτικού τεκμηρίωση για τη δημιουργία λύσεις διαχείρισης σε OMS που βρίσκονται τη συγκεκριμένη στιγμή σε προεπισκόπηση. Οποιαδήποτε διάταξη που περιγράφεται παρακάτω μπορεί να αλλάξουν.    

[Οι λύσεις διαχείρισης στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS)](operations-management-suite-solutions.md) συνήθως θα περιλαμβάνουν μία ή περισσότερες προβολές για την απεικόνιση δεδομένων.  Σε αυτό το άρθρο περιγράφει τον τρόπο για να εξαγάγετε μια προβολή που δημιουργήθηκε από την [Προβολή σχεδίασης](../log-analytics/log-analytics-view-designer.md) και να συμπεριλάβετε σε μια λύση διαχείρισης.  

>[AZURE.NOTE]Χρησιμοποιήστε τα δείγματα σε αυτό το άρθρο παραμέτρους και μεταβλητές που απαιτείται ή κοινά και για λύσεις διαχείρισης και περιγράφεται στη [Δημιουργία λύσεις διαχείρισης στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι είστε ήδη εξοικειωμένοι με τον τρόπο για να [δημιουργήσετε μια λύση διαχείρισης](operations-management-suite-solutions-creating.md) και τη δομή ενός αρχείου λύσης.


## <a name="overview"></a>Επισκόπηση

Για να συμπεριλάβετε μια προβολή σε μια λύση διαχείρισης, δημιουργείτε έναν **πόρο** για αυτό στο [αρχείο λύσης](operations-management-suite-solutions-creating.md).  Το JSON που περιγράφει την προβολή λεπτομερή ρύθμισης παραμέτρων είναι συνήθως σύνθετες μέσω και δεν κάτι ότι ένας συντάκτης τυπικές λύση θα έχει τη δυνατότητα να δημιουργήσετε με μη αυτόματο τρόπο.  Η πιο συνηθισμένη μέθοδος είναι να δημιουργήσετε την προβολή χρησιμοποιώντας την [Προβολή σχεδίασης](../log-analytics/log-analytics-view-designer.md), εξαγάγετε και, στη συνέχεια, προσθέστε τη λεπτομερή ρύθμισης παραμέτρων στη λύση. 

Τα βασικά βήματα για να προσθέσετε μια προβολή σε μια λύση είναι ως εξής.  Κάθε βήμα περιγράφεται με περισσότερες λεπτομέρειες στις παρακάτω ενότητες.

1. Εξαγωγή της προβολής σε ένα αρχείο.
2. Δημιουργήστε την προβολή πόρων στη λύση.
3. Προσθέστε τις λεπτομέρειες της προβολής.

## <a name="export-the-view-to-a-file"></a>Εξαγωγή της προβολής σε ένα αρχείο
Ακολουθήστε τις οδηγίες στο [Αρχείο καταγραφής ανάλυσης προβολή σχεδίασης](../log-analytics/log-analytics-view-designer.md) για την εξαγωγή μιας προβολής σε ένα αρχείο.  Το εξαγόμενο αρχείο θα είναι σε μορφή JSON με τα ίδια [στοιχεία με το αρχείο λύσης](operations-management-suite-solutions-creating.md#management-solution-files).  

Το στοιχείο **πόροι** του αρχείου προβολή θα έχει έναν πόρο με έναν τύπο **Microsoft.OperationalInsights/workspaces** που αντιπροσωπεύει το χώρο εργασίας OMS.  Αυτό το στοιχείο θα έχει υπο-στοιχείο με έναν τύπο από τις **προβολές** που αντιπροσωπεύει την προβολή και περιέχει τη λεπτομερή ρύθμιση παραμέτρων.  Θα αντιγράψετε τις λεπτομέρειες του αυτό το στοιχείο και, στη συνέχεια, αντιγράψτε την σε λύση σας.


## <a name="create-the-view-resource-in-the-solution"></a>Δημιουργία του πόρου προβολή της λύσης
Προσθέστε στον παρακάτω πόρο προβολή στο στοιχείο **πόροι** του αρχείου σας λύση.  Αυτό χρησιμοποιεί μεταβλητές που περιγράφονται παρακάτω ότι πρέπει επίσης να προσθέσετε.  Σημειώστε ότι οι ιδιότητες **πίνακα εργαλείων** και **OverviewTile** είναι σύμβολα κράτησης θέσης που θα αντικαταστήσετε με τις αντίστοιχες ιδιότητες από την προβολή εξαγόμενου αρχείου.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Προσθέστε τις ακόλουθες μεταβλητές στο στοιχείο [μεταβλητές](operations-management-suite-solutions-creating.md#variables) του αρχείου λύσης και αντικαταστήστε τις τιμές με εκείνα για τη λύση.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Σημειώστε ότι θα μπορούσε να αντιγράψετε τον πόρο ολόκληρη την προβολή από το αρχείο εξαγόμενα προβολή, αλλά θα πρέπει να κάνετε τις ακόλουθες αλλαγές για να λειτουργήσει στη λύση σας.  

- Ο **Τύπος** για τον πόρο προβολή πρέπει να αλλάξει από **τις προβολές** για να **Microsoft.OperationalInsights/workspaces**.
- Η ιδιότητα **name** για τον πόρο προβολή πρέπει να αλλάξει για να συμπεριλάβετε το όνομα του χώρου εργασίας.
- Η εξάρτηση από το χώρο εργασίας πρέπει να καταργηθούν από τον πόρο χώρου εργασίας δεν ορίστηκε στη λύση.
- Ιδιότητα **DisplayName** πρέπει να προστεθεί στην προβολή.  Το **αναγνωριστικό**, το **όνομα**και **εμφανιζόμενο όνομα** πρέπει να όλα αντιστοιχεί.
- Τα ονόματα παραμέτρων πρέπει να αλλάξουν ώστε να ταιριάζει με το απαιτούμενο σύνολο των παραμέτρων.
- Μεταβλητές πρέπει να οριστούν στη λύση και να χρησιμοποιούνται στις κατάλληλες ιδιότητες.

## <a name="add-the-view-details"></a>Προσθήκη προβολή λεπτομερειών
Ο πόρος προβολή του αρχείου εξαγωγής προβολή θα περιέχει δύο στοιχεία στο στοιχείο **Ιδιότητες** με το όνομα **πίνακα εργαλείων** και **OverviewTile** που περιέχουν τη λεπτομερή ρύθμιση παραμέτρων της προβολής.  Αντιγραφή αυτά τα δύο στοιχεία και τα περιεχόμενά τους στο στοιχείο **Ιδιότητες** του πόρου προβολή στο αρχείο σας λύση. 

## <a name="example"></a>Παράδειγμα
Για παράδειγμα, το παρακάτω παράδειγμα εμφανίζει μια απλή λύση αρχείου με μια προβολή.  Αποσιωπητικά (...) εμφανίζονται για τα περιεχόμενα του **πίνακα εργαλείων** και **OverviewTile** για λόγους χώρο.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε όλες τις λεπτομέρειες της δημιουργίας [λύσεων διαχείρισης](operations-management-suite-solutions-creating.md).
- Συμπεριλάβετε [αυτοματισμού runbooks στο τη λύση διαχείρισης](operations-management-suite-solutions-resources-automation.md).