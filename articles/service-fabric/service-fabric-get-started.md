<properties
   pageTitle="Ρυθμίστε το περιβάλλον ανάπτυξης | Microsoft Azure"
   description="Εγκαταστήστε το περιβάλλον εκτέλεσης, SDK και εργαλεία και δημιουργήστε ένα σύμπλεγμα τοπικής ανάπτυξης. Αφού ολοκληρώσετε αυτό το πρόγραμμα εγκατάστασης, θα είστε έτοιμοι να δημιουργήσετε εφαρμογές."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Προετοιμάσετε το περιβάλλον ανάπτυξης

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Να δημιουργήσετε και να εκτελέσετε [εφαρμογές Azure Service ύφασμα] [ 1] στον υπολογιστή σας ανάπτυξης, εγκαταστήστε το περιβάλλον εκτέλεσης, SDK και εργαλεία. Πρέπει επίσης να ενεργοποιήσετε την εκτέλεση των δεσμών ενεργειών του Windows PowerShell που περιλαμβάνεται στο SDK.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
### <a name="supported-operating-system-versions"></a>Εκδόσεις υποστηριζόμενο λειτουργικό σύστημα
Υποστηρίζονται οι ακόλουθες εκδόσεις λειτουργικό σύστημα για την ανάπτυξη:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 περιλαμβάνει μόνο Windows PowerShell 2.0 από προεπιλογή. Cmdlet του PowerShell ύφασμα υπηρεσία απαιτεί PowerShell 3.0 ή νεότερη έκδοση. Μπορείτε να [κάνετε λήψη του Windows PowerShell 5.0] [ powershell5-download] από το Κέντρο λήψης της Microsoft.

## <a name="install-the-runtime-sdk-and-tools"></a>Εγκαταστήστε το περιβάλλον εκτέλεσης, SDK και εργαλεία

Το πρόγραμμα εγκατάστασης πλατφόρμας Web προσφέρει δύο ρυθμίσεις παραμέτρων για την ανάπτυξη ύφασμα υπηρεσίας:

- [Εγκατάσταση του χρόνου εκτέλεσης υπηρεσίας ύφασμα SDK και εργαλεία για Visual Studio 2015 (απαιτείται το Visual Studio 2015 ενημέρωση 2 ή νεότερες εκδόσεις)][full-bundle-vs2015]
- [Εγκαταστήστε το περιβάλλον εκτέλεσης υπηρεσίας ύφασμα και SDK μόνο (χωρίς εργαλεία του Visual Studio)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Ενεργοποίηση εκτέλεσης δέσμης ενεργειών του PowerShell

Υπηρεσία ύφασμα χρησιμοποιεί δεσμών ενεργειών του Windows PowerShell για τη δημιουργία μιας τοπικής ανάπτυξης σύμπλεγμα και για την ανάπτυξη εφαρμογών από το Visual Studio. Από προεπιλογή, τα Windows αποκλείει αυτές τις δέσμες ενεργειών εκτέλεση. Για να ενεργοποιήσετε τους, πρέπει να τροποποιήσετε την πολιτική εκτέλεσης PowerShell. Ανοίξτε PowerShell ως διαχειριστής και εισαγάγετε την ακόλουθη εντολή:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που έχετε ολοκληρώσει τη ρύθμιση του περιβάλλοντος ανάπτυξης, ξεκινήστε δημιουργία και εκτέλεση εφαρμογών.

- [Δημιουργήστε την πρώτη εφαρμογή υπηρεσίας ύφασμα στο Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Μάθετε πώς μπορείτε να αναπτύξετε και να διαχειριστείτε τις εφαρμογές στην τοπική συμπλέγματος](service-fabric-get-started-with-a-local-cluster.md)
- [Μάθετε περισσότερα σχετικά με τα μοντέλα προγραμματισμού: αξιόπιστων υπηρεσιών και αξιόπιστη ηθοποιών](service-fabric-choose-framework.md)
- [Δείτε τα δείγματα κώδικα ύφασμα υπηρεσίας στην GitHub](https://aka.ms/servicefabricsamples)
- [Απεικόνιση το σύμπλεγμά σας χρησιμοποιώντας την Εξερεύνηση ύφασμα υπηρεσίας](service-fabric-visualizing-your-cluster.md)
- [Ακολουθήστε τη διαδικασία εκμάθησης ύφασμα υπηρεσίας για να λάβετε μια ευρεία εισαγωγή της πλατφόρμας](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Υπηρεσία ύφασμα εκστρατείας σελίδας"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "ΣΎΓΚΡΙΣΗ RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Σύνδεση και στο WebPI 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Σύνδεση Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Σύνδεση SDK WebPI πυρήνα"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
