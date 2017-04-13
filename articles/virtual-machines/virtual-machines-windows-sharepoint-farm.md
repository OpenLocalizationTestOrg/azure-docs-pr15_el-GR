<properties
    pageTitle="Δημιουργία συστοιχίες SharePoint server | Microsoft Azure"
    description="Γρήγορη δημιουργία μιας νέας συστοιχίας του SharePoint 2013 ή 2016 του SharePoint στο Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>Δημιουργία συστοιχίες SharePoint server

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Κλασική μοντέλο.

## <a name="sharepoint-2013-farms"></a>Συστοιχίες SharePoint 2013

Με το Microsoft Azure marketplace πύλης, μπορείτε να δημιουργήσετε γρήγορα προκαθορισμένο συστοιχίες SharePoint Server 2013. Να εξοικονομείτε πολύ χρόνο όταν χρειάζεστε μια βασική ή υψηλή διαθεσιμότητα συστοιχία του SharePoint για ένα περιβάλλον ανάπτυξης/έλεγχο ή που την αξιολόγηση του SharePoint Server 2013 ως λύση συνεργασίας για την εταιρεία σας.

> [AZURE.NOTE] Το στοιχείο **Συστοιχία διακομιστών του SharePoint** από το Azure Marketplace της πύλης Azure έχει καταργηθεί. Έχει αντικατασταθεί με τα στοιχεία **Του SharePoint 2013 μη HA συστοιχίας** και της **Συστοιχίας HA του SharePoint 2013** .

Το βασικό συστοιχία του SharePoint αποτελείται από τρεις εικονικές μηχανές σε αυτήν τη ρύθμιση.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Μπορείτε να χρησιμοποιήσετε αυτήν τη ρύθμιση παραμέτρων συστοιχίας για μια απλοποιημένη εγκατάσταση για ανάπτυξη εφαρμογών του SharePoint ή αξιολόγησή σας του SharePoint 2013 για πρώτη φορά.

Για να δημιουργήσετε τη βασική συστοιχία του SharePoint (τριών-διακομιστή):

1. Κάντε κλικ [εδώ](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Κάντε κλικ στην επιλογή **Ανάπτυξη**.
3. Στο παράθυρο **Του SharePoint 2013 μη HA συστοιχίας** , κάντε κλικ στην επιλογή **Δημιουργία**.
4. Καθορισμός ρυθμίσεων στα βήματα του παραθύρου **Δημιουργία συστοιχία SharePoint 2013 μη-HA** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

Στη συστοιχία του SharePoint υψηλής διαθεσιμότητας αποτελείται από εννέα εικονικές μηχανές σε αυτήν τη ρύθμιση.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Μπορείτε να χρησιμοποιήσετε αυτήν τη ρύθμιση παραμέτρων συστοιχίας για να ελέγξετε υψηλότερη φορτία προγράμματος-πελάτη, υψηλή διαθεσιμότητα από την εξωτερική τοποθεσία του SharePoint και ομάδες διαθεσιμότητας AlwaysOn SQL Server για μια συστοιχία του SharePoint. Μπορείτε επίσης να χρησιμοποιήσετε αυτήν τη ρύθμιση παραμέτρων για ανάπτυξη εφαρμογών του SharePoint σε ένα περιβάλλον υψηλής διαθεσιμότητας.

Για να δημιουργήσετε τη συστοιχία του SharePoint (9-διακομιστή) υψηλής διαθεσιμότητας:

1. Κάντε κλικ [εδώ](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Κάντε κλικ στην επιλογή **Ανάπτυξη**.
3. Στο παράθυρο **Του SharePoint 2013 HA συστοιχία** , κάντε κλικ στην επιλογή **Δημιουργία**.
4. Καθορισμός ρυθμίσεων στα επτά βήματα του παραθύρου **Δημιουργία συστοιχίας του SharePoint 2013 HA** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

> [AZURE.NOTE] Δεν μπορείτε να δημιουργήσετε τη **Συστοιχία SharePoint 2013 μη-HA** ή **Συστοιχία του SharePoint 2013 HA** με μια δωρεάν δοκιμαστική έκδοση του Azure.

Πύλη του Azure και τα δύο από αυτές τις συστοιχίες δημιουργεί ένα μόνο στο cloud εικονικό δίκτυο με μια παρουσία στο web μέσω Internet. Δεν υπάρχει καμία σύνδεση VPN ή ExpressRoute τοποθεσίας σε τοποθεσία προς το δίκτυο του οργανισμού σας.

> [AZURE.NOTE] Όταν δημιουργείτε τη βασική ή με την πύλη Azure συστοιχίες SharePoint υψηλής διαθεσιμότητας, δεν μπορείτε να καθορίσετε μια υπάρχουσα ομάδα πόρων. Για να επιλύσετε αυτόν τον περιορισμό, δημιουργήστε αυτές τις συστοιχίες με Azure PowerShell. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία SharePoint 2013 ανάπτυξης/έλεγχο συστοιχίες με Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>Συστοιχίες SharePoint 2016

Ανατρέξτε [σε αυτό το άρθρο](https://technet.microsoft.com/library/mt723354.aspx) για τις οδηγίες για να δημιουργήσετε το παρακάτω συστοιχία του SharePoint Server 2016 μεμονωμένου διακομιστή.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Διαχείριση συστοιχιών του SharePoint

Μπορείτε να διαχειριστείτε τους διακομιστές από αυτές τις συστοιχίες μέσω συνδέσεων απομακρυσμένης επιφάνειας εργασίας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [συνδεθείτε με την εικονική μηχανή](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Από την τοποθεσία κεντρικής διαχείρισης του SharePoint, μπορείτε να ρυθμίσετε οι τοποθεσίες μου, τις εφαρμογές του SharePoint και άλλες λειτουργίες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων του SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

- Ανακαλύψτε πρόσθετες [ρυθμίσεις παραμέτρων του SharePoint](https://technet.microsoft.com/library/dn635309.aspx) στις υπηρεσίες υποδομής Azure.
