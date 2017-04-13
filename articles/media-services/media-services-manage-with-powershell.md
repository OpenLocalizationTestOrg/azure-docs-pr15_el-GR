<properties 
    pageTitle="Διαχείριση λογαριασμών υπηρεσίες πολυμέσων Azure με το PowerShell" 
    description="Μάθετε πώς μπορείτε να διαχειριστείτε τους λογαριασμούς των υπηρεσιών Azure Media Services με το cmdlet του PowerShell." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Διαχείριση λογαριασμών υπηρεσίες πολυμέσων Azure με το PowerShell

> [AZURE.SELECTOR]
- [Πύλη](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [ΥΠΌΛΟΙΠΟ](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Για να μπορέσετε να δημιουργήσετε ένα λογαριασμό των υπηρεσιών Azure Media Services, πρέπει να έχετε ένα λογαριασμό Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure δωρεάν δοκιμαστικής έκδοσης</a>.

##<a name="overview"></a>Επισκόπηση 

Σε αυτό το άρθρο παραθέτει τα cmdlet του Azure PowerShell για το Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) στο πλαίσιο διαχείριση πόρων Azure. Τα cmdlet υπάρχει στο χώρο ονομάτων **Microsoft.Azure.Commands.Media** .

## <a name="versions"></a>Εκδόσεις

**ApiVersion**: "2015 10--01"
               

## <a name="new-azurermmediaservice"></a>Νέα AzureRmMediaService

Δημιουργεί μια υπηρεσία πολυμέσων.

### <a name="syntax"></a>Σύνταξη

Ρύθμιση παραμέτρων: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Ρύθμιση παραμέτρων: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα | Κανένας
---|---
Απαιτείται;   |  TRUE
Θέση;   |  0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ;  |FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Όνομα
---|---
Απαιτείται; |TRUE
Θέση; |1
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |FALSE
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-Θέση &lt;συμβολοσειράς&gt;**

Καθορίζει τη θέση των πόρων της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |2
Προεπιλεγμένη τιμή  |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-StorageAccountId &lt;συμβολοσειράς&gt;**

Καθορίζει έναν λογαριασμό πρωτεύοντος χώρου αποθήκευσης που σχετίζεται με την υπηρεσία πολυμέσων.

- Νέο χώρο αποθήκευσης στο λογαριασμό (που δημιουργήθηκε με το API διαχείρισης πόρων) υποστηρίζεται μόνο.

- Το λογαριασμό χώρου αποθήκευσης πρέπει να υπάρχει και έχει την ίδια θέση με την υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |3
Προεπιλεγμένη τιμή  |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Η παράμετρος οριστεί όνομα |StorageAccountIdParamSet
Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Καθορίζει τους λογαριασμούς χώρου αποθήκευσης που σχετίζεται με την υπηρεσία πολυμέσων.

- Νέο χώρο αποθήκευσης στο λογαριασμό (που δημιουργήθηκε με το API διαχείρισης πόρων) υποστηρίζεται μόνο.

- Το λογαριασμό χώρου αποθήκευσης πρέπει να υπάρχει και έχει την ίδια θέση με την υπηρεσία πολυμέσων.

- Μόνο ένα λογαριασμό χώρου αποθήκευσης μπορεί να οριστεί ως κύρια.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται;  |TRUE
Θέση;  |3
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Η παράμετρος οριστεί όνομα |StorageAccountsParamSet
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-Των ετικετών &lt;Hashtable&gt;**

Καθορίζει έναν πίνακα κατακερματισμός από τις ετικέτες που σχετίζονται με την υπηρεσία πολυμέσων.

- Παράδειγμα:@{"tag1"="value1";"tag2"=:value2"}

Ψευδώνυμα |Κανένας
---|---
Απαιτείται;  |FALSE
Θέση;  |με το όνομα
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |FALSE
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="set-azurermmediaservice"></a>Ορισμός AzureRmMediaService

Ενημερώσεις υπηρεσίας πολυμέσων.

### <a name="syntax"></a>Σύνταξη

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται;  |TRUE
Θέση;  |0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Όνομα
---|---
Απαιτείται; |TRUE
Θέση; |1
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Καθορίζει τους λογαριασμούς χώρου αποθήκευσης που σχετίζεται με την υπηρεσία πολυμέσων.

- Νέο χώρο αποθήκευσης στο λογαριασμό (που δημιουργήθηκε με το API διαχείρισης πόρων) υποστηρίζεται μόνο.

- Το λογαριασμό χώρου αποθήκευσης πρέπει να υπάρχει και έχει την ίδια θέση με την υπηρεσία πολυμέσων.

- Μόνο ένα λογαριασμό χώρου αποθήκευσης μπορεί να οριστεί ως κύρια.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |FALSE
Θέση; |Με το όνομα
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Η παράμετρος οριστεί όνομα |StorageAccountsParamSet
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-Των ετικετών &lt;Hashtable&gt;**

Καθορίζει έναν πίνακα κατακερματισμός από τις ετικέτες που συσχετίζονται με αυτήν την υπηρεσία πολυμέσων.

- Οι ετικέτες που σχετίζονται με την υπηρεσία πολυμέσων αντικαθίστανται με τιμή που καθορίζεται από τον πελάτη.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |FALSE
Θέση;  |Με το όνομα
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="remove-azurermmediaservice"></a>Κατάργηση AzureRmMediaService

Καταργεί μια υπηρεσία πολυμέσων.

### <a name="syntax"></a>Σύνταξη

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |2
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου;  |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Λαμβάνει όλες τις υπηρεσίες πολυμέσων σε μια ομάδα πόρων ή μια υπηρεσία πολυμέσων με ένα συγκεκριμένο όνομα.

### <a name="syntax"></a>Σύνταξη

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση;  |0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Η παράμετρος οριστεί όνομα |ResourceGroupParameterSet, AccountNameParameterSet
Αποδοχή χαρακτήρες μπαλαντέρ;   FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση;  |1
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Η παράμετρος οριστεί όνομα  |AccountNameParameterSet
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Λαμβάνει πλήκτρα μιας υπηρεσίας πολυμέσων.

### <a name="syntax"></a>Σύνταξη

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση;  |0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |1
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="set-azurermmediaservicekey"></a>Ορισμός AzureRmMediaServiceKey

Δημιουργεί ξανά ένα πρωτεύον ή δευτερεύον κλειδί μιας υπηρεσίας πολυμέσων.

### <a name="syntax"></a>Σύνταξη

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται;  |TRUE
Θέση;  |0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου;  |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση;  |1
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου;   |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-Τύπο κλειδιού &lt;τύπο κλειδιού&gt;**

Καθορίζει τον τύπο κλειδιού της υπηρεσίας πολυμέσων.

- Πρωτεύον ή δευτερεύον

Ψευδώνυμα |Κανένας
---|---
Απαιτείται;  |TRUE
Θέση;  |2
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |FALSE
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="sync-azurermmediaservicestoragekeys"></a>Συγχρονισμός AzureRmMediaServiceStorageKeys

Συγχρονίζει πλήκτρα λογαριασμού χώρου αποθήκευσης για ένα λογαριασμό του χώρου αποθήκευσης που σχετίζεται με την υπηρεσία πολυμέσων.

### <a name="syntax"></a>Σύνταξη

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Παράμετροι

**-ResourceGroupName &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της ομάδας πόρων στην οποία ανήκει αυτή η υπηρεσία πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |0
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**Όνομα λογαριασμού - &lt;συμβολοσειράς&gt;**

Καθορίζει το όνομα της υπηρεσίας πολυμέσων.

Ψευδώνυμα |Κανένας
---|---
Απαιτείται; |TRUE
Θέση; |1
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**-StorageAccountId &lt;συμβολοσειράς&gt;**

Καθορίζει το λογαριασμό χώρου αποθήκευσης που σχετίζεται με την υπηρεσία πολυμέσων.

Ψευδώνυμα |Αναγνωριστικό
---|---
Απαιτείται; |TRUE
Θέση;  |2
Προεπιλεγμένη τιμή |Κανένας
Αποδοχή διοχέτευσης εισόδου; |      TRUE(ByPropertyName)
Αποδοχή χαρακτήρες μπαλαντέρ; |FALSE

**&lt;Παράμετροι_εντολής&gt;**

Αυτό το cmdlet υποστηρίζει τις κοινές παραμέτρους:-εντοπισμός σφαλμάτων - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - λεπτομερής, - WarningAction, και - WarningVariable.

### <a name="inputs"></a>Εισόδων

Ο τύπος εισόδου είναι ο τύπος των αντικειμένων που μπορείτε να διοχέτευση στο cmdlet.

### <a name="outputs"></a>Εξόδους

Ο τύπος εξόδου είναι ο τύπος των αντικειμένων που εκπέμπει το cmdlet.

## <a name="next-step"></a>Επόμενο βήμα 

Ανατρέξτε στο θέμα διαδικασίες εκμάθησης υπηρεσίες πολυμέσων.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
