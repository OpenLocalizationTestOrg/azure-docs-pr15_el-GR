<properties
   pageTitle="Προϋποθέσεις για την υιοθέτηση ExpressRoute | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει μια λίστα με τις απαιτήσεις πρέπει να πληρούνται πριν να παραγγείλετε ένα κύκλωμα Azure ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute τις προϋποθέσεις και τη λίστα ελέγχου  

Για να συνδεθείτε χρησιμοποιώντας ExpressRoute υπηρεσίες cloud της Microsoft, θα πρέπει να επαληθεύσετε ότι πληρούνται οι ακόλουθες απαιτήσεις που παρατίθενται στις παρακάτω ενότητες.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Λογαριασμός Azure

- Ένας λογαριασμός Microsoft Azure έγκυρη και ενεργό. Αυτό είναι απαραίτητο να ρυθμίσετε το κύκλωμα ExpressRoute. Κυκλώματα ExpressRoute είναι πόροι μέσα σε συνδρομές του Azure. Μια συνδρομή του Azure αποτελεί προϋπόθεση, ακόμα και αν συνδεσιμότητας περιορίζεται στα Azure Microsoft cloud υπηρεσίες, όπως υπηρεσίες του Office 365 και CRM online.
- Μια ενεργή συνδρομή στο Office 365 (Εάν χρησιμοποιείτε τις υπηρεσίες του Office 365). Ανατρέξτε στην ενότητα [Office 365 συγκεκριμένες απαιτήσεις](#office-365-specific-requirements) αυτού του άρθρου για περισσότερες πληροφορίες.

## <a name="connectivity-provider"></a>Υπηρεσία παροχής σύνδεσης
- Μπορείτε να εργαστείτε με έναν [συνεργάτη συνδεσιμότητας ExpressRoute](expressroute-locations.md#partners) για να συνδεθείτε με το Microsoft cloud. Μπορείτε να ρυθμίσετε μια σύνδεση μεταξύ του δικτύου εσωτερικής εγκατάστασης και της Microsoft με [τρεις τρόπους](expressroute-introduction.md#howtoconnect). 
- Εάν την υπηρεσία παροχής δεν είναι ένας Συνεργάτης συνδεσιμότητας ExpressRoute, εξακολουθείτε να μπορείτε να συνδεθείτε στο cloud της Microsoft μέσω μιας [υπηρεσίας παροχής ανταλλαγής cloud](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Απαιτήσεις δικτύου
- **Πλεοναζόντων συνδεσιμότητας**: δεν απαιτείται πλεονασμού σε φυσική σύνδεση ανάμεσα σε εσάς και την υπηρεσία παροχής. Microsoft απαιτούν πλεονάζοντα το πρωτόκολλο BGP περιόδους λειτουργίας για να ρυθμιστεί μεταξύ δρομολογητές της Microsoft και οι peering δρομολογητές, ακόμα και όταν έχετε μόνο [μία φυσική σύνδεση με μια ανταλλαγή cloud](expressroute-faqs.md#onep2plink). 
- **Δρομολόγηση**: ανάλογα με τον τρόπο σύνδεσης στο cloud της Microsoft, που ή την υπηρεσία παροχής, πρέπει να ρυθμίσετε και να διαχειριστείτε τις περιόδους λειτουργίας το πρωτόκολλο BGP για [τους τομείς δρομολόγησης](expressroute-circuit-peerings.md). Ορισμένες παροχής συνδεσιμότητας Ethernet ή της υπηρεσίας παροχής exchange cloud ενδέχεται να προσφέρουν Διαχείριση το πρωτόκολλο BGP ως μια τιμή-Προσθήκη υπηρεσίας.
- **NAT**: Microsoft δέχεται μόνο δημόσιες διευθύνσεις IP μέσω του Microsoft διεισδύουν. Εάν χρησιμοποιείτε ιδιωτικών διευθύνσεων IP του δικτύου σας στην εσωτερική εγκατάσταση, εσείς ή οι ανάγκες σας υπηρεσία παροχής που χρησιμοποιείτε για να μεταφράσετε τα ιδιωτικά διευθύνσεις IP για το δημόσιο IP διευθύνσεις [χρησιμοποιώντας το NAT](expressroute-nat.md).
- **Ποιότητας υπηρεσίας**: Skype για επιχειρήσεις έχει διάφορες υπηρεσίες (π.χ. φωνή, βίντεο, κείμενο) που απαιτούν διαφέρει ποιότητας υπηρεσίας επεξεργασίας. Μπορείτε και την υπηρεσία παροχής πρέπει να ακολουθήσετε τις [απαιτήσεις ποιότητας υπηρεσίας](expressroute-qos.md).
- **Ασφάλεια δικτύου**: πρέπει να εξετάσετε [ασφάλειας του δικτύου](../best-practices-network-security.md) κατά τη σύνδεση στο cloud της Microsoft μέσω ExpressRoute.
 
## <a name="office-365"></a>Office 365

Εάν σκοπεύετε να ενεργοποιήσετε το Office 365 σε ExpressRoute, εξετάστε τα ακόλουθα έγγραφα για περισσότερες πληροφορίες σχετικά με τις απαιτήσεις του Office 365.


- [Επισκόπηση των ExpressRoute για Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Δρομολόγηση με ExpressRoute για Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Διευθύνσεις URL του Office 365 και περιοχές διευθύνσεων IP](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Σχεδιασμός δικτύου και ρύθμιση επιδόσεων για το Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Προγράμματα υπολογισμού εύρους ζώνης δικτύου και εργαλεία](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Ενοποίηση του Office 365 με περιβάλλοντα εσωτερικής εγκατάστασης](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Εάν σκοπεύετε να ενεργοποιήσετε CRM Online σε ExpressRoute, εξετάστε τα ακόλουθα έγγραφα για περισσότερες πληροφορίες σχετικά με το CRM Online

- [CRM Online διευθύνσεις URL](https://support.microsoft.com/kb/2655102) και [περιοχές διευθύνσεων IP](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Επόμενα βήματα

- Για περισσότερες πληροφορίες σχετικά με το ExpressRoute, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md).
- Εύρεση υπηρεσίας παροχής συνδεσιμότητας ExpressRoute. Ανατρέξτε στο θέμα [ExpressRoute συνεργάτες και διεισδύουν θέσεις](expressroute-locations.md).
- Ανατρέξτε στις απαιτήσεις για [δρομολόγηση](expressroute-routing.md), [NAT](expressroute-nat.md) και [ποιότητας υπηρεσίας](expressroute-qos.md).
- Ρυθμίστε τις παραμέτρους της σύνδεσής σας ExpressRoute.
    - [Δημιουργήστε ένα κύκλωμα ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Ρυθμίστε τις παραμέτρους δρομολόγησης](expressroute-howto-routing-classic.md)
    - [Σύνδεση ενός VNet με ένα κύκλωμα ExpressRoute](expressroute-howto-linkvnet-classic.md)

