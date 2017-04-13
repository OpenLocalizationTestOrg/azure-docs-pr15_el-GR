<properties
   pageTitle="Ρύθμιση παραμέτρων μιας πύλης VNet για χρήση του PowerShell ExpressRoute | Microsoft Azure"
   description="Ρύθμιση παραμέτρων μιας πύλης VNet για μια κλασική ανάπτυξη του μοντέλου VNet χρήση του PowerShell για ρύθμιση παραμέτρων ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-classic-deployment-model-and-powershell"></a>Ρύθμιση παραμέτρων μιας πύλης εικονικού δικτύου για το ExpressRoute χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης και PowerShell

> [AZURE.SELECTOR]
- [PowerShell - διαχείριση πόρων](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - κλασικό](expressroute-howto-add-gateway-classic.md)

Σε αυτό το άρθρο θα σας καθοδηγήσει τα βήματα για την προσθήκη, αλλαγή μεγέθους και κατάργηση μιας πύλης εικονικού δικτύου (VNet) για μια προ-υπαρχόντων VNet. Τα βήματα για αυτήν τη ρύθμιση παραμέτρων χρησιμοποιείτε ειδικά για VNets που έχουν δημιουργηθεί στο **μοντέλο κλασική ανάπτυξης** και που θα χρησιμοποιηθεί σε μια ρύθμιση παραμέτρων ExpressRoute. 

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Πριν από την αρχή

Βεβαιωθείτε ότι έχετε εγκαταστήσει το cmdlet του Azure PowerShell που χρειάζονται για αυτήν τη ρύθμιση παραμέτρων (1.0.2 ή νεότερη έκδοση). Εάν δεν έχετε εγκαταστήσει τα cmdlet, θα πρέπει να το κάνετε πριν να ξεκινήσετε τα βήματα ρύθμισης παραμέτρων. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση του Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

    
## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε την πύλη VNet, μπορείτε να συνδέσετε το VNet σε ένα κύκλωμα ExpressRoute. Ανατρέξτε στο θέμα [σύνδεση ένα εικονικό δίκτυο ένα κύκλωμα ExpressRoute](expressroute-howto-linkvnet-classic.md).
