<properties
   pageTitle="Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου σε κλασική λειτουργία χρησιμοποιώντας το PowerShell | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου σε κλασική λειτουργία χρήση του PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου (κλασικό) στο PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου χρησιμοποιώντας τη διαχείριση πόρων Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Ρύθμιση εξισορρόπηση φόρτου χρησιμοποιώντας PowerShell

Για να ρυθμίσετε μια μονάδα εξισορρόπησης φόρτου χρησιμοποιώντας το powershell, ακολουθήστε τα παρακάτω βήματα:

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../../articles/powershell-install-configure.md) και ακολουθήστε τις οδηγίες προς το τέλος για να συνδεθείτε στο Azure και επιλέξτε τη συνδρομή σας.


2. Αφού δημιουργήσετε μια εικονική μηχανή, μπορείτε να χρησιμοποιήσετε το cmdlet του PowerShell για να προσθέσετε μια μονάδα εξισορρόπησης φόρτου σε μια εικονική μηχανή εντός της ίδιας υπηρεσίας cloud.

Στο παρακάτω παράδειγμα θα προσθέσετε μια μονάδα εξισορρόπησης φόρτου που ονομάζεται "webfarm" με το cloud υπηρεσίας "mytestcloud" (ή myctestcloud.cloudapp.net), προσθέτοντας τα τελικά σημεία για τη μονάδα εξισορρόπησης φόρτου σε εικονικές μηχανές με το όνομα "web1" και "web2". Κίνηση του δικτύου στη θύρα 80 λαμβάνει την εξισορρόπηση φόρτου και τα υπόλοιπα φόρτωσης μεταξύ τις εικονικές μηχανές που ορίζονται από το τοπικό τελικό σημείο (σε αυτό πεζών-κεφαλαίων θύρα 80) χρησιμοποιώντας TCP.


### <a name="step-1"></a>Βήμα 1
Δημιουργήστε ένα τελικό σημείο εξισορρόπησης φόρτου για την πρώτη εικονική Μηχανή "web1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Βήμα 2

Δημιουργήστε ένα άλλο τελικό σημείο για το δεύτερο Εικονική "web2" χρησιμοποιώντας το ίδιο όνομα σύνολο εξισορρόπησης φόρτου

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Καταργήστε μια εικονική μηχανή από μια μονάδα εξισορρόπησης φόρτου

Μπορείτε να χρησιμοποιήσετε AzureEndpoint Κατάργηση για να καταργήσετε ένα τελικό σημείο εικονική μηχανή από τη μονάδα εξισορρόπησης φόρτου

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε επίσης [να ξεκινήσετε τη δημιουργία μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-classic-ps.md) και ρύθμιση παραμέτρων τι τύπο [λειτουργία διανομής](load-balancer-distribution-mode.md) για μια συμπεριφορά κίνηση especific φόρτωσης εξισορρόπησης δικτύου.

Εάν η εφαρμογή σας πρέπει να διατηρήσετε ενεργών για τους διακομιστές πίσω από μια μονάδα εξισορρόπησης φόρτου συνδέσεις, μπορείτε να μάθετε περισσότερα σχετικά με τις [ρυθμίσεις χρονικού ορίου αδράνειας TCP για μια μονάδα εξισορρόπησης φόρτου](load-balancer-tcp-idle-timeout.md). Θα σας βοηθήσει να μάθετε περισσότερα σχετικά με τη συμπεριφορά αδρανής σύνδεση όταν χρησιμοποιείτε Azure εξισορρόπηση φόρτου.

