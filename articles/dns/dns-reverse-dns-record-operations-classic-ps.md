<properties
   pageTitle="Διαχείριση αντίστροφη των εγγραφών DNS για τις υπηρεσίες σας Azure (κλασικό) χρησιμοποιώντας το PowerShell | Microsoft Azure"
   description="Πώς μπορείτε να διαχειριστείτε αντίστροφη εγγραφές DNS ή PTR για Azure υπηρεσίες με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Πώς μπορείτε να διαχειριστείτε αντίστροφη εγγραφές DNS για το Azure υπηρεσίες (κλασικό) με χρήση του PowerShell Azure

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Επικύρωση αντίστροφης εγγραφές DNS
Για να εξασφαλίσετε άλλου κατασκευαστή δεν είναι δυνατό να δημιουργήσετε αντίστροφης τις εγγραφές DNS αντιστοίχιση στους τομείς σας DNS, Azure επιτρέπει μόνο τη δημιουργία εγγραφής DNS αντίστροφης όταν ένα από τα εξής είναι αληθής:

- Το αντίστροφο DNS FQDN είναι το όνομα της υπηρεσίας Cloud για την οποία έχει καθοριστεί ή οποιαδήποτε υπηρεσία Cloud όνομα μέσα σε την ίδια συνδρομή π.χ., αντίστροφη DNS είναι "contosoapp1.cloudapp.net.".
- Το αντίστροφη FQDN DNS προς τα εμπρός επιλύει το όνομα ή την IP της υπηρεσίας Cloud για που του έχει οριστεί ή σε οποιαδήποτε υπηρεσία Cloud όνομα ή IP μέσα την ίδια συνδρομή π.χ., αντίστροφη DNS είναι "app1.contoso.com." που είναι ένα ψευδώνυμο CName για contosoapp1.cloudapp.net.

Έλεγχοι επικύρωσης εκτελούνται μόνο όταν η ιδιότητα αντίστροφη DNS για μια υπηρεσία Cloud έχει τιμή ή τροποποίηση. Περιοδικές εκ νέου επικύρωσης δεν εκτελείται.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Προσθήκη αντίστροφη DNS σε υπάρχουσες υπηρεσίες Cloud
Μπορείτε να προσθέσετε μια αντίστροφη εγγραφή DNS σε μια υπάρχουσα υπηρεσία στο Cloud χρησιμοποιώντας το cmdlet "Ορισμός-AzureService":

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Δημιουργήστε μια υπηρεσία Cloud με αντίστροφη DNS
Μπορείτε να προσθέσετε μια νέα υπηρεσία στο Cloud με την αντίστροφη ιδιότητα DNS που καθορίζονται χρησιμοποιώντας το cmdlet "Ορισμός-AzureService":

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Προβολή αντίστροφη DNS για τις υπάρχουσες υπηρεσίες Cloud
Μπορείτε να προβάλετε την τιμή που έχει ρυθμιστεί για μια υπάρχουσα υπηρεσία Cloud χρησιμοποιώντας το cmdlet "Get-AzureService":

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Κατάργηση αντίστροφη DNS από υπάρχουσες υπηρεσίες Cloud
Μπορείτε να καταργήσετε μια αντίστροφη ιδιότητα DNS από μια υπάρχουσα υπηρεσία Cloud χρησιμοποιώντας το cmdlet "Ορισμός-AzureService". Αυτό γίνεται, ορίζοντας την αντίστροφη τιμή της ιδιότητας DNS σε κενό:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
