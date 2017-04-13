<properties 
   pageTitle="Καθορισμός ρυθμίσεων DNS σε ένα αρχείο ρύθμισης παραμέτρων εικονικού δικτύου | Microsoft Azure"
   description="Πώς μπορείτε να αλλάξετε τις ρυθμίσεις του διακομιστή DNS σε ένα εικονικό δίκτυο χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων εικονικού δικτύου στο μοντέλο κλασική ανάπτυξης"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Καθορισμός ρυθμίσεων DNS σε ένα αρχείο ρύθμισης παραμέτρων εικονικού δικτύου

Ένα αρχείο ρύθμισης παραμέτρων δικτύου έχει δύο στοιχεία που μπορείτε να χρησιμοποιήσετε για να καθορίσετε ρυθμίσεις συστήματος ονομάτων τομέα (DNS): **DnsServers** και **DnsServerRef**. Μπορείτε να προσθέσετε μια λίστα με τους διακομιστές DNS, καθορίζοντας τις διευθύνσεις IP και τα ονόματα αναφοράς για το στοιχείο **DnsServers** . Στη συνέχεια, μπορείτε να χρησιμοποιήσετε ένα στοιχείο **DnsServerRef** για να καθορίσετε ποιες εγγραφές DNS server από το στοιχείο DnsServers που χρησιμοποιούνται για τις τοποθεσίες διαφορετικό δίκτυο εικονικό δίκτυο σας.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης.

Αρχείο ρύθμισης παραμέτρων του δικτύου μπορεί να περιλαμβάνει τα ακόλουθα στοιχεία. Ο τίτλος κάθε στοιχείο είναι συνδεδεμένο σε μια σελίδα που παρέχει πρόσθετες πληροφορίες σχετικά με τις ρυθμίσεις τιμή στοιχείου.

>[AZURE.IMPORTANT] Για πληροφορίες σχετικά με τον τρόπο ρύθμισης παραμέτρων του αρχείου ρύθμισης παραμέτρων δικτύου, ανατρέξτε στο θέμα [ρύθμιση των παραμέτρων ενός εικονικού δικτύου χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων δικτύου](virtual-networks-using-network-configuration-file.md). Για πληροφορίες σχετικά με κάθε στοιχείο που περιέχονται στο αρχείο ρύθμισης παραμέτρων του δικτύου, ανατρέξτε στο θέμα [Azure εικονικού σχήματος ρύθμισης παραμέτρων δικτύου](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[Στοιχείο DNS](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Το χαρακτηριστικό **ονόματος** στο στοιχείο **DnsServer** χρησιμοποιείται μόνο με μια αναφορά για το στοιχείο **DnsServerRef** . Δεν αντιπροσωπεύει το όνομα κεντρικού υπολογιστή για το διακομιστή DNS. Κάθε τιμή του χαρακτηριστικού **DnsServer** πρέπει να είναι μοναδικό σε ολόκληρη τη συνδρομή Microsoft Azure

[Στοιχείο τοποθεσίες εικονικού δικτύου](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Για να καθορίσετε αυτήν τη ρύθμιση για το στοιχείο εικονικές τοποθεσίες δικτύου, πρέπει να είναι προηγουμένως οριστεί στο στοιχείο DNS. Το DnsServerRef *ονόματος* στο στοιχείο εικονικές τοποθεσίες δικτύου πρέπει να αναφέρεται σε μια τιμή όνομα που καθορίζεται στο στοιχείο DNS για DnsServer *όνομα*.

## <a name="next-steps"></a>Επόμενα βήματα

- Κατανόηση του [σχήματος ρύθμισης παραμέτρων Azure εικονικού δικτύου](http://go.microsoft.com/fwlink/?LinkId=248093).
- Κατανόηση του [σχήματος ρύθμισης παραμέτρων Azure υπηρεσίας](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Ρύθμιση εικονικού δικτύου με τη χρήση των αρχείων ρύθμισης παραμέτρων δικτύου](virtual-networks-using-network-configuration-file.md).
