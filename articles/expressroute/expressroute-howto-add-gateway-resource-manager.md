<properties
   pageTitle="Προσθήκη μιας πύλης VNet σε δίκτυο εικονικού για χρήση της διαχείρισης πόρων και PowerShell ExpressRoute | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί Προσθήκη πύλης Vnet σε μια ήδη που έχουν δημιουργηθεί από διαχειριστή πόρων VNet για ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Ρύθμιση παραμέτρων μιας πύλης εικονικού δικτύου για χρήση της διαχείρισης πόρων και PowerShell ExpressRoute


> [AZURE.SELECTOR]
- [PowerShell - διαχείριση πόρων](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - κλασικό](expressroute-howto-add-gateway-classic.md)


Σε αυτό το άρθρο θα σας καθοδηγήσει τα βήματα για την προσθήκη, αλλαγή μεγέθους και κατάργηση μιας πύλης εικονικού δικτύου (VNet) για μια προ-υπαρχόντων VNet. Τα βήματα για αυτήν τη ρύθμιση παραμέτρων χρησιμοποιείτε ειδικά για VNets που έχουν δημιουργηθεί στο **μοντέλο ανάπτυξης διαχείρισης πόρων** και που θα χρησιμοποιηθεί σε μια ρύθμιση παραμέτρων ExpressRoute. 

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Πριν από την αρχή

Βεβαιωθείτε ότι έχετε εγκαταστήσει το cmdlet του Azure PowerShell που χρειάζονται για αυτήν τη ρύθμιση παραμέτρων (1.0.2 ή νεότερη έκδοση). Εάν δεν έχετε εγκαταστήσει τα cmdlet, θα πρέπει να το κάνετε πριν να ξεκινήσετε τα βήματα ρύθμισης παραμέτρων. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση του Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε την πύλη VNet, μπορείτε να συνδέσετε το VNet σε ένα κύκλωμα ExpressRoute. Ανατρέξτε στο θέμα [σύνδεση ένα εικονικό δίκτυο ένα κύκλωμα ExpressRoute](expressroute-howto-linkvnet-arm.md).
