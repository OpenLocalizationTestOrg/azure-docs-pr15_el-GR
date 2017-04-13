<properties
   pageTitle="Εγκατάσταση Endpoint Protection στο Κέντρο ασφάλειας Azure | Microsoft Azure"
   description="Αυτό το έγγραφο που δείχνει πώς μπορείτε να υλοποιήσετε το Κέντρο ασφάλειας Azure σύσταση **Εγκατάσταση Endpoint Protection**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Εγκατάσταση Endpoint Protection στο Κέντρο ασφάλειας Azure

Κέντρο ασφάλειας Azure θα να προμηθεύσουν ένα πρόγραμμα λογισμικό κακόβουλης λειτουργίας για να σας Azure εικονικές μηχανές (ΣΠΣ), εάν το λογισμικό κακόβουλης λειτουργίας δεν είναι ήδη ενεργοποιημένη. Σύσταση αυτό ισχύει μόνο για το Windows ΣΠΣ.

> [AZURE.NOTE] Αυτό το έγγραφο παρουσιάζει την υπηρεσία, χρησιμοποιώντας μια ανάπτυξη παράδειγμα.  Δεν πρόκειται για αναλυτικές οδηγίες.

## <a name="implement-the-recommendation"></a>Υλοποίηση σύσταση

1. Στο το blade **συστάσεις** , επιλέξτε **Εγκατάσταση Endpoint Protection**.
![Επιλέξτε εγκατάσταση Endpoint Protection][1]

2. Η **Εγκατάσταση Endpoint Protection** blade ανοίγει εμφανίζει μια λίστα των ΣΠΣ χωρίς λογισμικό κακόβουλης λειτουργίας με δυνατότητα. Επιλέξτε από τη λίστα του ΣΠΣ που θέλετε να εγκαταστήσετε το λογισμικό κακόβουλης λειτουργίας σε και κάντε κλικ στην επιλογή **εγκατάσταση σε ΣΠΣ**.
![Επιλέξτε ΣΠΣ για να εγκαταστήσετε λογισμικό κακόβουλης λειτουργίας σε][2]

3. Η **Επιλογή Endpoint Protection** blade ανοίγει για να σας επιτρέπει να επιλέξετε τη λύση λογισμικό κακόβουλης λειτουργίας που θέλετε να χρησιμοποιήσετε. Σε αυτό το παράδειγμα, ας επιλέξουμε **Λογισμικό κακόβουλης λειτουργίας της Microsoft**.
![Επιλέξτε Endpoint Protection][3]

4. Εμφανίζονται πρόσθετες πληροφορίες σχετικά με τη λύση λογισμικό κακόβουλης λειτουργίας. Επιλέξτε **Δημιουργία**.
![Δημιουργία λύσεων λογισμικό κακόβουλης λειτουργίας][4]

5. Εισαγάγετε τις ρυθμίσεις παραμέτρων απαιτείται σε το blade **Προσθήκη επέκταση** και, στη συνέχεια, επιλέξτε **OK**. Για να μάθετε περισσότερα σχετικά με τις ρυθμίσεις παραμέτρων, ανατρέξτε στο θέμα [προεπιλεγμένες και προσαρμοσμένες ρυθμίσεις παραμέτρων λογισμικό κακόβουλης λειτουργίας](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

Τώρα είναι ενεργό στην του επιλεγμένου ΣΠΣ [Λογισμικό κακόβουλης λειτουργίας της Microsoft](../azure-security-antimalware.md) .

## <a name="see-also"></a>Δείτε επίσης

Σε αυτό το άρθρο εμφάνιζε πώς μπορείτε να υλοποιήσετε το Κέντρο ασφάλειας σύσταση "Εγκατάσταση Endpoint Protection." Για να μάθετε περισσότερα σχετικά με την ενεργοποίηση ενός προγράμματος λογισμικό κακόβουλης λειτουργίας στο Azure, ανατρέξτε στα παρακάτω:

- [Λογισμικό κακόβουλης λειτουργίας της Microsoft για τις υπηρεσίες Cloud και εικονικές μηχανές](../azure-security-antimalware.md) --μάθετε πώς μπορείτε να αναπτύξετε το λογισμικό κακόβουλης λειτουργίας της Microsoft.

Για να μάθετε περισσότερα σχετικά με το Κέντρο ασφάλειας, ανατρέξτε στα παρακάτω:

- [Ρύθμιση πολιτικών ασφάλειας στο Κέντρο ασφάλειας Azure](security-center-policies.md) --μάθετε πώς μπορείτε να ρυθμίσετε τις πολιτικές ασφαλείας.
- [Διαχείριση ασφαλείας συστάσεων στο Κέντρο ασφάλειας Azure](security-center-recommendations.md) --μάθετε πώς συστάσεις σας βοηθήσουν να προστατεύσετε Azure τους πόρους σας.
- [Παρακολούθηση εύρυθμης λειτουργίας ασφαλείας στο Κέντρο ασφάλειας Azure](security-center-monitoring.md) --μάθετε τον τρόπο για την παρακολούθηση της εύρυθμης λειτουργίας του Azure τους πόρους σας.
- [Διαχείριση και απάντηση σε ειδοποιήσεις ασφαλείας στο Κέντρο ασφάλειας Azure](security-center-managing-and-responding-alerts.md) --μάθετε πώς μπορείτε να διαχειριστείτε και να απαντούν σε ειδοποιήσεων ασφαλείας.
- [Λύσεις συνεργατών παρακολούθησης με το Κέντρο ασφάλειας Azure](security-center-partner-solutions.md) --μάθετε πώς μπορείτε να παρακολουθείτε την κατάσταση εύρυθμης λειτουργίας των λύσεων για συνεργάτες σας.
- [Συνήθεις Ερωτήσεις για το Κέντρο ασφάλειας azure](security-center-faq.md) --Εύρεση συνήθεις ερωτήσεις σχετικά με τη χρήση της υπηρεσίας.
- [Ιστολόγιο του azure ασφαλείας](http://blogs.msdn.com/b/azuresecurity/) --Εύρεση ιστολογίου καταχωρήσεις σχετικά με Azure ασφάλεια και συμμόρφωση.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
