<properties
   pageTitle="Επαναφορά μιας πύλης Azure VPN | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί σε την επαναφορά της πύλης VPN Azure. Το άρθρο ισχύει για πύλες VPN στο τόσο το κλασικό και τα μοντέλα ανάπτυξης διαχείρισης πόρων."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Επαναφορά μιας πύλης VPN Azure χρησιμοποιώντας το PowerShell


Σε αυτό το άρθρο σάς καθοδηγεί σε την επαναφορά της πύλης VPN Azure με χρήση των cmdlet του PowerShell. Αυτές οι οδηγίες περιλαμβάνουν το μοντέλο κλασική ανάπτυξης και το μοντέλο ανάπτυξης διαχείρισης πόρων.

Επαναφορά της πύλης Azure VPN είναι χρήσιμη αν τον χάσετε συνδεσιμότητα VPN σταυρό εσωτερικής εγκατάστασης σε μία ή περισσότερες σήραγγες S2S VPN. Σε αυτήν την περίπτωση, τις συσκευές σας στην εσωτερική εγκατάσταση VPN όλα λειτουργούν σωστά, αλλά δεν θα μπορείτε να δημιουργήσετε σήραγγες ασφαλείας IP με το Azure VPN πύλες. 

Κάθε πύλη Azure VPN αποτελείται από δύο Εικονική παρουσιών που εκτελούνται σε μια ρύθμιση παραμέτρων ενεργό την αναμονή. Όταν χρησιμοποιείτε το cmdlet του PowerShell για να επαναφέρετε την πύλη, το επανεκκίνηση της πύλης και, στη συνέχεια, εφαρμόζει ξανά τις ρυθμίσεις παραμέτρων σταυρό εσωτερικής εγκατάστασης σε αυτήν. Η πύλη διατηρεί στη δημόσια διεύθυνση IP που έχει ήδη. Αυτό σημαίνει ότι δεν θα πρέπει να ενημερώσετε τις ρυθμίσεις παραμέτρων δρομολογητή VPN με μια νέα δημόσια διεύθυνση IP για την πύλη Azure VPN.  

Όταν η εντολή εκδίδεται, την τρέχουσα ενεργή παρουσία της πύλης Azure VPN επανεκκίνηση αμέσως. Θα υπάρχει μια σύντομη διάκενο κατά τη διάρκεια του εφεδρικού από την ενεργή παρουσία (που επανεκκίνηση), για να την παρουσία αναμονής. Το κενό πρέπει να είναι μικρότερη από ένα λεπτό.

Εάν δεν γίνει επαναφορά της σύνδεσης μετά την πρώτη επανεκκίνηση, ζήτημα την ίδια εντολή ξανά να κάνετε επανεκκίνηση της δεύτερης εμφάνισης Εικονική (η νέα πύλη ενεργό). Εάν η επανεκκίνηση του δύο ζητούνται συνεχόμενα, θα υπάρχει λίγο περισσότερο χρόνο τελεία όπου είναι να γίνει επανεκκίνηση και οι δύο παρουσίες Εικονική (ενεργό και αναμονής). Αυτό θα προκαλέσει μεγαλύτερο κενού στη τη συνδεσιμότητα VPN, έως 2 έως 4 λεπτά για ΣΠΣ για να ολοκληρωθεί η επανεκκίνηση του.

Μετά την επανεκκίνηση του δύο, εάν εξακολουθείτε να αντιμετωπίζετε προβλήματα συνδεσιμότητας σταυρό εσωτερικής εγκατάστασης, ανοίξτε μια αίτηση υποστήριξης από την πύλη του Azure.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Πριν από την επαναφορά της πύλης, επιβεβαιώστε τα στοιχεία κλειδιού που παρατίθενται παρακάτω για κάθε διοχέτευσης VPN ασφαλείας IP--τοποθεσίας (S2S). Οποιαδήποτε δεν ταιριάζει με τα στοιχεία θα έχει ως αποτέλεσμα την αποσύνδεση σήραγγες S2S VPN. Επαλήθευση και τη διόρθωση τις ρυθμίσεις παραμέτρων εσωτερικής εγκατάστασης και Azure VPN πύλες εξοικονομείτε από επανεκκίνηση του δεν είναι απαραίτητες και διακοπές για τις άλλες συνδέσεις εργασίας πυλών.

Επαληθεύστε τα ακόλουθα στοιχεία πριν από την επαναφορά της πύλης:

- Οι διευθύνσεις Internet IP (VIPs) για την πύλη Azure VPN και την πύλη VPN εσωτερικής εγκατάστασης έχουν ρυθμιστεί σωστά τόσο το Azure και τις πολιτικές VPN εσωτερικής εγκατάστασης.
- Το κλειδί κοινής χρήσης πρέπει να είναι η ίδια σε εσωτερικής εγκατάστασης και Azure πύλες VPN.
- Εάν εφαρμόσετε συγκεκριμένη ρύθμιση παραμέτρων ασφαλείας IP/IKE, όπως κρυπτογράφηση, ο κατακερματισμός αλγορίθμους και PFS (τέλειο προς τα εμπρός απορρήτου), βεβαιωθείτε ότι οι Azure εσωτερικής VPN πύλες και έχουν τις ίδιες ρυθμίσεις παραμέτρων.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Επαναφορά μιας πύλης VPN χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων

Είναι το cmdlet του PowerShell για τη διαχείριση πόρων για την επαναφορά πύλης `Reset-AzureRmVirtualNetworkGateway`. Το παρακάτω παράδειγμα επαναφέρει το Azure πύλης VPN, "VNet1GW", στην ομάδα πόρων "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Επαναφορά μιας πύλης VPN χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης

Είναι το cmdlet του PowerShell για την επαναφορά πύλης Azure VPN `Reset-AzureVNetGateway`. Το παρακάτω παράδειγμα επαναφέρει της πύλης Azure VPN για το εικονικό δίκτυο που ονομάζεται "ContosoVNet".
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Αποτέλεσμα:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Επόμενα βήματα
    
Ανατρέξτε στο θέμα το [αναφορές για τα cmdlet του PowerShell υπηρεσίας διαχείρισης](https://msdn.microsoft.com/library/azure/mt617104.aspx) και την [αναφορά cmdlet του PowerShell για τη διαχείριση πόρων](http://go.microsoft.com/fwlink/?LinkId=828732) για περισσότερες πληροφορίες.






