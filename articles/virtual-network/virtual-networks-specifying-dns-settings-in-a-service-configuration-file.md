<properties 
   pageTitle="Καθορισμός ρυθμίσεων DNS σε ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας | Microsoft Azure"
   description="Καθορισμός προσαρμοσμένες ρυθμίσεις DNS με χρήση του αρχείου ρύθμισης παραμέτρων υπηρεσίας για εικονικού δικτύου"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Καθορισμός ρυθμίσεων DNS σε ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας

## <a name="dns-elements"></a>Στοιχεία του DNS

Ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας μπορεί να περιέχει ένα στοιχείο DnsServers με μια λίστα με διευθύνσεις IPv4 για τους διακομιστές συστήματος ονομάτων τομέα (DNS) που θα χρησιμοποιήσει την υπηρεσία. Ρυθμίσεις στο αρχείο ρύθμισης παραμέτρων της υπηρεσίας προηγούνται των ρυθμίσεων στο αρχείο ρύθμισης παραμέτρων του δικτύου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Σχήματος ρύθμισης παραμέτρων υπηρεσίας Azure (.cscfg αρχείου)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**Στοιχείο NetworkConfiguration**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Το χαρακτηριστικό **ονόματος** στο στοιχείο **DnsServer** χρησιμοποιείται μόνο ως ένα όνομα αναφοράς. Δεν αντιπροσωπεύει το όνομα κεντρικού υπολογιστή για το διακομιστή DNS. Κάθε τιμή του χαρακτηριστικού **DnsServer** πρέπει να είναι μοναδικό σε ολόκληρη τη συνδρομή Microsoft Azure.

## <a name="see-also"></a>Δείτε επίσης

[Ρύθμιση παραμέτρων σχήματος Azure υπηρεσίας (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Σχήμα ρύθμισης παραμέτρων Azure εικονικού δικτύου](http://go.microsoft.com/fwlink/?LinkId=248093)

[Ρύθμιση παραμέτρων εικονικού δικτύου με χρήση αρχείων ρύθμισης παραμέτρων δικτύου](http://go.microsoft.com/fwlink/?LinkId=248094)

[Σχετικά με τις ρυθμίσεις εικονικού δικτύου στην πύλη διαχείρισης](http://go.microsoft.com/fwlink/?LinkId=248092)

