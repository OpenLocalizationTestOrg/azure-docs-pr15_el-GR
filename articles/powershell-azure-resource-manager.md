<properties 
    pageTitle="Azure PowerShell με το διαχειριστή πόρων | Microsoft Azure" 
    description="Εισαγωγή στη χρήση του Azure PowerShell για την ανάπτυξη πολλούς πόρους ως μια ομάδα πόρων για Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Χρήση του Azure PowerShell με τη Διαχείριση Azure πόρων

> [AZURE.SELECTOR]
- [Πύλη](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API](resource-manager-rest-api.md)


Azure διαχείριση πόρων υλοποιεί μια σύγχρονη προσέγγιση για τον έλεγχο του κύκλου ζωής Azure πόρους. Αντί για τη δημιουργία και τη διαχείριση των μεμονωμένων πόρων, ξεκινάτε με φαντάζεται μια ολόκληρη λύση, όπως ένα ιστολόγιο, μια συλλογή φωτογραφιών, μια πύλη του SharePoint ή ένα wiki. Μπορείτε να χρησιμοποιήσετε ένα πρότυπο--μια δηλωτικό αναπαράσταση της λύσης--για να ορίσετε μια ομάδα πόρων που περιέχει όλους τους πόρους που χρειάζεστε για την υποστήριξη της λύσης. Στη συνέχεια, μπορείτε να αναπτύξετε και να διαχειριστείτε αυτήν την ομάδα πόρων ως λογική μονάδα. 

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε τον τρόπο χρήσης του Azure PowerShell με τη διαχείριση πόρων Azure. Σας καθοδηγεί κατά τη διαδικασία για την ανάπτυξη μιας λύσης, και την εργασία με αυτήν τη λύση. Θα χρησιμοποιήσετε Azure PowerShell και ένα πρότυπο από διαχειριστή πόρων για την ανάπτυξη:

- SQL server - για να φιλοξενήσει τη βάση δεδομένων
- Βάση δεδομένων SQL - για την αποθήκευση των δεδομένων
- Κανόνες τείχους προστασίας - για να επιτρέψετε την εφαρμογή web για να συνδεθείτε με τη βάση δεδομένων
- Πρόγραμμα εφαρμογής υπηρεσίας - για τον ορισμό του δυνατότητες και του κόστους της εφαρμογής web
- Τοποθεσία Web - για την εκτέλεση της εφαρμογής web
- Ρύθμιση παραμέτρων Web - για την αποθήκευση της συμβολοσειράς σύνδεσης με τη βάση δεδομένων 
- Ειδοποίηση κανόνες - για την παρακολούθηση των επιδόσεων και σφαλμάτων
- Εφαρμογή ιδέες - για τις ρυθμίσεις αυτόματης κλίμακας

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει:

- Azure λογαριασμού
  + Μπορείτε να [ανοίξετε ένα λογαριασμό Azure δωρεάν](/pricing/free-trial/?WT.mc_id=A261C142F): λάβετε πιστώσεων μπορείτε να χρησιμοποιήσετε για να δοκιμάσετε την πληρωμή Azure υπηρεσιών και ακόμα και αν χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και χρήση δωρεάν Azure υπηρεσίες, όπως τοποθεσίες Web που διαθέτετε. Η πιστωτική κάρτα σας θα χρεωθεί ποτέ, εκτός εάν αλλάξετε τις ρυθμίσεις σας και ζητήστε του να χρεωθεί ρητά.
  
  + Μπορείτε να κάνετε [Ενεργοποίηση πλεονεκτήματα συνδρομητή MSDN](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): τη συνδρομή σας MSDN παρέχει πιστώσεων κάθε μήνα που μπορείτε να χρησιμοποιήσετε για τις υπηρεσίες του Azure επί πληρωμή.
- Azure PowerShell 1.0. Για πληροφορίες σχετικά με αυτήν την έκδοση και πώς μπορείτε να το εγκαταστήσετε, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](powershell-install-configure.md).

Αυτό το πρόγραμμα εκμάθησης έχει σχεδιαστεί για αρχάριους PowerShell, αλλά αυτό προϋποθέτει ότι γνωρίζετε τις βασικές έννοιες, όπως λειτουργικές μονάδες, των cmdlet και οι περίοδοι λειτουργίας.

## <a name="get-help-for-cmdlets"></a>Λήψη Βοήθειας για τα cmdlet του

Για να λάβετε αναλυτικές πληροφορίες Βοήθειας για οποιαδήποτε cmdlet που βλέπετε σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε το cmdlet Get-Βοήθεια. 

    Get-Help <cmdlet-name> -Detailed

Για παράδειγμα, για να λάβετε Βοήθεια για το cmdlet Get-AzureRmResource, πληκτρολογήστε:

    Get-Help Get-AzureRmResource -Detailed

Για να λάβετε μια λίστα των cmdlet του στη λειτουργική μονάδα πόρους με μια σύνοψη Βοήθεια, πληκτρολογήστε: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Το αποτέλεσμα θα είναι παρόμοιο με το παρακάτω απόσπασμα:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Για να λάβετε βοήθεια πλήρους για ένα cmdlet, πληκτρολογήστε μια εντολή με τη μορφή:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Συνδεθείτε στο λογαριασμό σας Azure

Πριν από την εργασία σας σε λύση σας, πρέπει να συνδεθείτε στο λογαριασμό σας.

Για να συνδεθείτε στο λογαριασμό σας Azure, χρησιμοποιήστε το cmdlet **Προσθήκη AzureRmAccount** .

    Add-AzureRmAccount

Το cmdlet σάς ζητά τα διαπιστευτήρια σύνδεσης για το λογαριασμό σας Azure. Μετά την καταγραφή στο, να κάνει λήψη ρυθμίσεις του λογαριασμού σας, ώστε να είναι διαθέσιμες για Azure PowerShell. 

Λήγει τις ρυθμίσεις λογαριασμού, έτσι θα πρέπει να ανανεώνετε τα μερικές φορές. Για να ανανεώσετε τις ρυθμίσεις λογαριασμού, εκτελέστε ξανά το **Πρόσθετο AzureRmAccount** . 

>[AZURE.NOTE] Οι λειτουργικές μονάδες από διαχειριστή πόρων απαιτεί Προσθήκη AzureRmAccount. Ένα αρχείο ρυθμίσεων δημοσίευση δεν επαρκεί.     

Εάν έχετε περισσότερες από μία συνδρομές, δώστε το αναγνωριστικό συνδρομής που θέλετε να χρησιμοποιήσετε για ανάπτυξη με το cmdlet **Set-AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Πριν αναπτύξετε πόρους για τη συνδρομή σας, πρέπει να δημιουργήσετε μια ομάδα πόρων που θα περιέχουν τους πόρους. 

Για να δημιουργήσετε μια ομάδα πόρων, χρησιμοποιήστε το cmdlet **New-AzureRmResourceGroup** .

Η εντολή χρησιμοποιεί την παράμετρο **όνομα** για να καθορίσετε ένα όνομα για την ομάδα των πόρων και την παράμετρο **θέση** για να καθορίσετε τη θέση του. Βασίζεται σε τι θα σας εντόπισε στην προηγούμενη ενότητα, θα χρησιμοποιήσουμε "Δυτική η.π.α." για τη θέση.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Το αποτέλεσμα θα είναι παρόμοιο με:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Ομάδα πόρων σας δημιουργήθηκε με επιτυχία.

## <a name="deploy-your-solution"></a>Ανάπτυξη της λύσης σας

Αυτό το θέμα δεν σας δείχνουν πώς μπορείτε να δημιουργήσετε το πρότυπό σας ή να συζητήσετε για τη δομή του προτύπου. Για αυτές τις πληροφορίες, ανατρέξτε στο θέμα [σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα](resource-group-authoring-templates.md) και [Αναλυτικές οδηγίες για το πρότυπο διαχείρισης πόρων](resource-manager-template-walkthrough.md). Θα αναπτύξετε το πρότυπο προκαθορισμένες [παροχή μιας εφαρμογής Web με μια βάση δεδομένων SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) από τα [Πρότυπα γρήγορη έναρξη Azure](https://azure.microsoft.com/documentation/templates/).

Έχετε σας ομάδα πόρων και έχετε το πρότυπο, ώστε να είστε έτοιμοι για την ανάπτυξη της υποδομής ορίζεται στο πρότυπό σας, στην ομάδα πόρων. Μπορείτε να αναπτύξετε τους πόρους με τα cmdlet **New-AzureRmResourceGroupDeployment** . Το πρότυπο καθορίζει πολλές προεπιλεγμένες τιμές, η οποία θα χρησιμοποιήσουμε, οπότε δεν χρειάζεται να παράσχετε τιμές για αυτές τις παραμέτρους. Η βασική σύνταξη έχει την παρακάτω:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Μπορείτε να καθορίσετε την ομάδα των πόρων και τη θέση του προτύπου. Εάν το πρότυπό σας είναι ένα τοπικό αρχείο, μπορείτε χρησιμοποιήσετε την παράμετρο **- TemplateFile** και καθορίστε τη διαδρομή προς το πρότυπο. Μπορείτε να ορίσετε το **-λειτουργία** παράμετρος **προσαύξηση** ή **ολοκλήρωσης**. Από προεπιλογή, η διαχείριση πόρων εκτελεί βηματικής ενημέρωσης κατά την ανάπτυξη; Επομένως, δεν είναι απαραίτητες για να ορίσετε **-λειτουργία** όταν θέλετε **προσαύξηση**. Για να κατανοήσετε τις διαφορές ανάμεσα σε αυτές τις λειτουργίες ανάπτυξης, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Παράμετροι δυναμικό πρότυπο

Εάν είστε εξοικειωμένοι με το PowerShell, γνωρίζετε ότι μπορείτε να μετακινηθείτε στα τις διαθέσιμες παραμέτρους για ένα cmdlet πληκτρολογώντας ένα σύμβολο μείον (-) και, στη συνέχεια, πατώντας το πλήκτρο TAB. Αυτή η ίδια λειτουργία λειτουργεί επίσης με παραμέτρους που έχετε καθορίσει στο πρότυπό σας. Καθώς πληκτρολογείτε το όνομα του προτύπου, το cmdlet λαμβάνει το πρότυπο, αναλύει και προσθέτει τις παραμέτρους του προτύπου στην εντολή δυναμικά. Αυτό διευκολύνει πολύ για να καθορίσετε τις τιμές παραμέτρων προτύπου.

Όταν εισαγάγετε την εντολή, θα σας ζητηθεί για την παράμετρο που λείπουν υποχρεωτικό, **administratorLoginPassword**. Και, όταν πληκτρολογείτε τον κωδικό πρόσβασης, η τιμή της ασφαλούς συμβολοσειράς είναι καλύπτεται. Αυτή η στρατηγική αποκλείει τον κίνδυνο παρέχοντας έναν κωδικό πρόσβασης σε απλό κείμενο.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Εάν το πρότυπο περιλαμβάνει μια παράμετρο με όνομα που ταιριάζει με μία από τις παραμέτρους της εντολής για να αναπτύξετε το πρότυπο (όπως όπως μια παράμετρο που ονομάζεται **ResourceGroupName** στο πρότυπό σας, η οποία είναι η ίδια με την παράμετρο **ResourceGroupName** στο το cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) ), θα σας ζητηθεί να δώσετε μια τιμή για μια παράμετρο με το επιθεματική **FromTemplate** (όπως **ResourceGroupNameFromTemplate**). Σε γενικές γραμμές, θα πρέπει να αποφύγετε αυτό σύγχυση δίνοντάς δεν παραμέτρους με το ίδιο όνομα με παραμέτρους που χρησιμοποιούνται για λειτουργίες ανάπτυξης.

Η εντολή εκτελείται και επιστρέφει τα μηνύματα με τους πόρους που έχουν δημιουργηθεί. Τέλος, μπορείτε να δείτε το αποτέλεσμα της ανάπτυξής σας.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

Σε λίγα βήματα, δημιουργούνται και αναπτυχθεί τους πόρους που απαιτούνται για μια σύνθετη τοποθεσία Web. 

### <a name="log-debug-information"></a>Πληροφορίες αρχείου καταγραφής εντοπισμού σφαλμάτων

Κατά την ανάπτυξη ενός προτύπου, μπορείτε να συνδεθείτε πρόσθετες πληροφορίες σχετικά με το αίτησης και απόκρισης, καθορίζοντας την παράμετρο **- DeploymentDebugLogLevel** όταν εκτελείται το **Νέο AzureRmResourceGroupDeployment**. Αυτές οι πληροφορίες μπορεί να σας βοηθήσει να αντιμετωπίσετε τα σφάλματα ανάπτυξης. Η προεπιλεγμένη τιμή είναι **κανένα** που σημαίνει ότι δεν υπάρχει αίτηση ή απόκριση περιεχομένου έχει συνδεθεί. Μπορείτε να καθορίσετε την καταγραφή του περιεχομένου από το αίτημα, απάντηση ή και τα δύο.  Για περισσότερες πληροφορίες σχετικά με την αντιμετώπιση προβλημάτων αναπτύξεις και καταγραφή πληροφοριών εντοπισμού σφαλμάτων, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων αναπτύξεις ομάδα πόρων με το Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md). Το παρακάτω παράδειγμα καταγράφει το περιεχόμενο αίτησης και απόκρισης περιεχομένου για την ανάπτυξη.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Όταν ορίζετε την παράμετρο DeploymentDebugLogLevel, εξετάστε προσεκτικά τον τύπο των πληροφοριών που που περνά μέσα σε κατά την ανάπτυξη. Με πληροφορίες σύνδεσης για την αίτηση ή την απάντηση, θα μπορούσε να ενδεχομένως να εκθέτουν ευαίσθητα δεδομένα τα οποία γίνεται ανάκτηση μέσω τις λειτουργίες ανάπτυξης. 


## <a name="get-information-about-your-resource-groups"></a>Λήψη πληροφοριών σχετικά με τις ομάδες πόρων

Αφού δημιουργήσετε μια ομάδα πόρων, μπορείτε να χρησιμοποιήσετε τα cmdlet στη λειτουργική μονάδα διαχείρισης πόρων για να διαχειριστείτε τις ομάδες πόρων.

- Για να λάβετε μια ομάδα πόρων στη συνδρομή σας, χρησιμοποιήστε το cmdlet **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Που επιστρέφει τις ακόλουθες πληροφορίες:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Εάν δεν καθορίσετε ένα όνομα ομάδας πόρων, το cmdlet επιστρέφει όλες τις ομάδες πόρων τη συνδρομή σας.

- Για να τους πόρους στην ομάδα πόρων, χρησιμοποιήστε το cmdlet **AzureRmResource Εύρεση** και την παράμετρο **ResourceGroupNameContains** . Χωρίς παραμέτρους, εύρεση AzureRmResource λαμβάνει όλους τους πόρους στην Azure τη συνδρομή σας.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Που επιστρέφει μια λίστα με τους πόρους διαμορφωμένη όπως:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Μπορείτε να χρησιμοποιήσετε ετικέτες για να λογικά οργανώσετε τους πόρους με τη συνδρομή σας, και να ανακτήσετε πόρους με τα cmdlet **AzureRmResource Εύρεση** και **Εύρεση AzureRmResourceGroup** .

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Προσθήκη σε ομάδα πόρων

Για να προσθέσετε έναν πόρο στην ομάδα πόρων, μπορείτε να χρησιμοποιήσετε το cmdlet **New-AzureRmResource** . Ωστόσο, η προσθήκη ενός πόρου με αυτόν τον τρόπο μπορεί να προκαλέσει σύγχυση μελλοντική επειδή δεν υπάρχει το νέο πόρο στο πρότυπό σας. Εάν αναπτύξει εκ νέου το παλιό πρότυπο, μπορείτε να αναπτύξετε μια μη ολοκληρωμένη λύση. Εάν αναπτύσσετε συχνά, θα τη βρείτε πιο εύκολη και πιο αξιόπιστη για να προσθέσετε το νέο πόρο στο πρότυπό σας και να αναπτύξετε ξανά.

## <a name="move-a-resource"></a>Μετακίνηση ενός πόρου

Μπορείτε να μετακινήσετε υπαρχόντων πόρων σε μια νέα ομάδα πόρων. Για παραδείγματα, ανατρέξτε στο θέμα [Μετακίνηση πόρων για νέα ομάδα πόρων ή τη συνδρομή](resource-group-move-resources.md).

## <a name="export-template"></a>Εξαγωγή προτύπου

Για μια υπάρχουσα ομάδα πόρων (αναπτύσσεται μέσω του PowerShell ή σε μία από τις άλλες μεθόδους όπως την πύλη), μπορείτε να προβάλετε το πρότυπο διαχείρισης πόρων για την ομάδα πόρων. Εξαγωγή του προτύπου προσφέρει δύο πλεονεκτήματα:

1. Μπορείτε εύκολα να αυτοματοποιήσετε μελλοντικές αναπτύξεις της λύσης επειδή όλα της υποδομής του ορίζεται στο πρότυπο.

2. Μπορείτε να εξοικειωθείτε με σύνταξης προτύπου, εάν ανατρέξετε στο το JavaScript αντικείμενο σημειογραφία (JSON) που αντιπροσωπεύει τη λύση σας.

Μέσω του PowerShell, μπορείτε να δημιουργήσετε ένα πρότυπο που αντιπροσωπεύει την τρέχουσα κατάσταση της ομάδας σας πόρων, ή ανάκτηση του προτύπου που χρησιμοποιήθηκε για μια συγκεκριμένη ανάπτυξη.

Εξαγωγή του προτύπου για μια ομάδα πόρων είναι χρήσιμη όταν έχετε κάνει αλλαγές σε μια ομάδα πόρων και πρέπει να ανακτήσετε την αναπαράσταση JSON τρέχουσα κατάστασή. Ωστόσο, το πρότυπο που δημιουργήθηκε περιέχει μόνο έναν ελάχιστο αριθμό των παραμέτρων και δεν υπάρχουν μεταβλητές. Οι περισσότερες από τις τιμές στο πρότυπο είναι σχεδιασμένο. Πριν από την ανάπτυξη του προτύπου που δημιουργήθηκε, ίσως θελήσετε να μετατρέψετε περισσότερες από τις τιμές σε παραμέτρους, ώστε να μπορείτε να προσαρμόσετε την ανάπτυξη για διαφορετικά περιβάλλοντα.

Εξαγωγή του προτύπου για μια συγκεκριμένη ανάπτυξη είναι χρήσιμη όταν θέλετε να προβάλετε την πραγματική πρότυπο που χρησιμοποιήθηκε για την ανάπτυξη των πόρων. Το πρότυπο θα περιλαμβάνει όλες τις παραμέτρους και μεταβλητές που καθορίζονται για το αρχικό ανάπτυξη. Ωστόσο, εάν κάποιος στην εταιρεία σας έχει πραγματοποιήσει αλλαγές στην ομάδα πόρων εκτός του τι ορίζεται στο πρότυπο, αυτό το πρότυπο θα αντιπροσωπεύει την τρέχουσα κατάσταση της ομάδας πόρων.

> [AZURE.NOTE] Η δυνατότητα εξαγωγής πρότυπο είναι σε προεπισκόπηση και δεν όλους τους τύπους πόρων υποστηρίζεται επί του παρόντος εξαγωγή ενός προτύπου. Όταν επιχειρείτε να εξαγάγετε ένα πρότυπο, μπορείτε να δείτε ένα μήνυμα σφάλματος που αναφέρει δεν είχαν εξαχθεί ορισμένους πόρους. Εάν είναι απαραίτητο, μπορείτε να καθορίσετε με μη αυτόματο τρόπο αυτούς τους πόρους στο πρότυπό σας μετά τη λήψη της.

###<a name="export-template-from-resource-group"></a>Εξαγωγή προτύπου από ομάδα πόρων

Για να προβάλετε το πρότυπο για μια ομάδα πόρων, εκτελέστε το cmdlet **AzureRmResourceGroup εξαγωγής** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Λήψη προτύπου από ανάπτυξης

Για να κάνετε λήψη του προτύπου που χρησιμοποιείται για μια συγκεκριμένη ανάπτυξη, εκτελέστε το cmdlet **AzureRmResourceGroupDeploymentTemplate αποθήκευση** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Διαγραφή πόρους ή ομάδα πόρων

- Για να διαγράψετε έναν πόρο από την ομάδα πόρων, χρησιμοποιήστε το cmdlet **Κατάργηση AzureRmResource** . Αυτό το cmdlet διαγράφει τον πόρο, αλλά δεν διαγράφει την ομάδα των πόρων.

    Αυτή η εντολή καταργεί την τοποθεσία Web TestSite από την ομάδα πόρων TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Για να διαγράψετε μια ομάδα πόρων, χρησιμοποιήστε το cmdlet **Κατάργηση AzureRmResourceGroup** . Αυτό το cmdlet διαγράφει την ομάδα των πόρων και τους πόρους.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Σας ζητηθεί να επιβεβαιώσετε τη διαγραφή.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Δέσμη ενεργειών ανάπτυξης

Τα παραδείγματα προηγούμενες ανάπτυξης σε αυτό το θέμα εμφάνιζε μόνο τα μεμονωμένα cmdlet που απαιτούνται για να αναπτύξετε τους πόρους στους Azure. Το παρακάτω παράδειγμα εμφανίζει μια δέσμη ενεργειών ανάπτυξης που δημιουργεί την ομάδα των πόρων και αναπτύσσει τους πόρους.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με τη δημιουργία προτύπων για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](./resource-group-authoring-templates.md).
- Για να μάθετε περισσότερα σχετικά με την ανάπτυξη προτύπων, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](./resource-group-template-deploy.md).
- Για ένα αναλυτικό παράδειγμα ανάπτυξης του έργου, ανατρέξτε στο θέμα [Ανάπτυξη microservices προβλέψιμα στο Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Για να μάθετε περισσότερα σχετικά με την αντιμετώπιση προβλημάτων μιας ανάπτυξης που απέτυχε, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων αναπτύξεις ομάδα πόρων στο Azure](./resource-manager-troubleshoot-deployments-powershell.md).

