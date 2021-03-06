<properties
   pageTitle="Για την ανάπτυξη εικονικού συσκευές σε υψηλή διαθεσιμότητα | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το εικονικό συσκευές δικτύου υψηλής διαθεσιμότητας."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Για την ανάπτυξη εικονικού συσκευές σε υψηλή διαθεσιμότητα

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Σε αυτό το άρθρο περιγράφει ένα σύνολο πρακτικές για την ανάπτυξη εικονικού συσκευές δικτύου (NVAs) με ιδιαίτερα διαθέσιμη τρόπο. Πριν να συνεχίσετε, βεβαιωθείτε ότι γνωρίζετε πώς [δρομολογεί που ορίζονται από το χρήστη (UDR)] [ udr-overview] και [μονάδα εξισορρόπησης φόρτου] [ lb-overview] εργασίας στο Azure.

Μπορείτε να χρησιμοποιήσετε διαφορετικό NVAs διαθέσιμα από το Azure marketplace για να επεκτείνετε τη λειτουργικότητα του Azure με τον ίδιο τρόπο που μπορείτε να χρησιμοποιήσετε συσκευών στο κέντρο δεδομένων σας εσωτερικής εγκατάστασης. Η παρακάτω εικόνα δείχνει μια ανάπτυξη του δείγματος από ένα [μεμονωμένο NVA] [ nva-scenario] ως μια συσκευή τείχους προστασίας. 

![[0]][0]

Παρόλο που το προηγούμενο ανάπτυξης λειτουργεί, παρουσιάζει ένα μοναδικό σημείο αποτυχίας. Εάν αποτύχει η εικονική συσκευή, ρέει χωρίς κίνηση. Για να επιλύσετε αυτό το πρόβλημα, πρέπει να χρησιμοποιήσετε πολλά NVAs. Ωστόσο, η οποία επίσης απαιτεί άλλες ρυθμίσεις και τους πόρους που θα χρησιμοποιηθεί ανάλογα με τις απαιτήσεις σας.

Μπορείτε να χρησιμοποιήσετε μία από τις παρακάτω λύσεις για να αναπτύξετε ένα περιβάλλον NVA ιδιαίτερα διαθέσιμη.

|Λύση|Πλεονεκτήματα|Ζητήματα|
|---|---|---|
|Εισόδου με συσκευές εικονικού επίπεδο 7|Όλοι οι κόμβοι είναι ενεργό|Απαιτεί ένα NVA που να Τερματισμός συνδέσεις και να χρησιμοποιήσετε SNAT<br/>Απαιτεί ένα ξεχωριστό σύνολο NVAs για την κίνηση που προέρχονται από το Internet και από το Azure<br/>Μπορεί να χρησιμοποιηθεί μόνο για την κίνηση που προέρχονται εκτός του Azure|
|Εισόδου-εξόδου με συσκευές εικονικού επίπεδο 7|Όλοι οι κόμβοι είναι ενεργό<br/>Μπορείτε να χειριστείτε κίνηση που δημιουργήθηκε στο Azure |Απαιτεί ένα NVA που να Τερματισμός συνδέσεις και να χρησιμοποιήσετε SNAT<br/>Απαιτεί ένα ξεχωριστό σύνολο NVAs για την κίνηση που προέρχονται από το Internet και από το Azure|
|Εναλλαγή PIP UDR|Σύνολο NVAs για όλη την κυκλοφορία<br/>Μπορεί να χειριστεί όλη την κυκλοφορία (χωρίς όριο στη θύρα κανόνες)|Ενεργό παθητικές<br/>Απαιτεί μια διαδικασία ανακατεύθυνσης|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Εισόδου με συσκευές εικονικού επίπεδο 7
Μπορείτε να χρησιμοποιήσετε ένα σύνολο NVAs πίσω από μια μονάδα εξισορρόπησης φόρτου Azure για την παροχή συνδεσιμότητας με Azure φόρτους εργασίας πίσω από ένα μικρό σύνολο θυρών διακομιστή (όπως HTTP και HTTPS). Η παρακάτω εικόνα επισημαίνει τον τρόπο για την παροχή υψηλή διαθεσιμότητα σε αυτό το σενάριο στο επίπεδο NVA.

![[1]][1]

Σε αυτό το σενάριο, συσκευής εικονικού δικτύου χρησιμοποιούνται πρέπει να Τερματισμός όλες οι συνδέσεις και αυτά μεταβιβάζουν στο υποδίκτυο φόρτο εργασίας. Το φόρτο εργασίας εικονικές μηχανές (ΣΠΣ) Απαντήστε το NVA λάβει μια αίτηση από και ροές κυκλοφορίας χωρίς προβλήματα. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Εισόδου-εξόδου με συσκευές εικονικού επίπεδο 7
Μπορείτε να επεκτείνετε την προηγούμενη αρχιτεκτονική και να προσθέσετε ένα άλλο σύνολο NVAs χειρισμού κίνηση που προέρχονται από το Azure να διαχειρίζεται NVAs, όπως φαίνεται στην παρακάτω εικόνα:

![[2]][2]

Σε αυτό το σενάριο, όλη την κυκλοφορία καταγωγής Azure δρομολογείται σε μια εσωτερική εξισορρόπηση φόρτου που κατανέμει φόρτωσης ανάμεσα σε ένα διαφορετικό σύνολο NVAs. Αυτές οι NVAs άμεσες κίνηση στο Internet με χρήση μεμονωμένων δημόσια διευθύνσεις IP. 

## <a name="pip-udr-switch"></a>Εναλλαγή PIP UDR
Μπορείτε να αποφύγετε τη δημιουργία πολλών NVA στοίβες με χρήση δύο NVAs σε λειτουργία παθητικές ενεργό. Σε αυτό το σενάριο, μπορείτε να αλλάξετε τη δημόσια διεύθυνση IP (PIP) και δρομολογεί που ορίζονται από το χρήστη (UDRs) όταν διακόπτει την ενεργό κόμβο.  

![[3]][3]

Αυτό το σενάριο είναι παρόμοιο με το μεμονωμένο σενάριο NVA. Η μόνη διαφορά είναι ότι η PIP και UDRs πρέπει να αλλάξουν για να αλλάξετε την κυκλοφορία μεταξύ του NVAs. Οι αλλαγές αυτές μπορεί να γίνει με μη αυτόματο τρόπο ή μπορείτε επίσης να αυτοματοποιήσετε τους. Για να αυτοματοποιήσετε, μπορείτε να αναπτύξετε μια εφαρμογή για να δύο NVAs που ελέγχει για την εύρυθμη λειτουργία του ενεργού κόμβου. Μόλις το ενεργό κόμβο προς τα κάτω, την εφαρμογή σας να αλλάξετε το PIP και UDRs για σύνδεση στον παθητικό κόμβο.

Μια πιθανή υλοποίηση του αυτή η λύση είναι να χρησιμοποιήσετε ένα [ZooKeeper] [ zookeeper] μονού στην το NVAs να χειρίζεται τις εκλογές επικεφαλής (αποφασίσετε ποια κόμβου είναι το ενεργό κόμβο). Όταν ένας επικεφαλής είναι αυτή κλήσεων για να το Azure REST API για να καταργήσετε το PIP του εσφαλμένου κόμβου και να το επισυνάψετε ο επικεφαλής. Πρέπει επίσης να αλλάξετε UDRs ώστε να οδηγεί τον νέο επικεφαλής ιδιωτική διεύθυνση IP.

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε πώς μπορείτε να [υλοποιήσετε ένα DMZ μεταξύ Azure και το κέντρο δεδομένων εσωτερικής εγκατάστασης] [ dmz-on-prem] χρησιμοποιώντας NVAs επιπέδου-7.
- Μάθετε πώς μπορείτε να [υλοποιήσετε ένα DMZ μεταξύ Azure και του Internet] [ dmz-internet] χρησιμοποιώντας NVAs επιπέδου-7.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Μία αρχιτεκτονική NVA"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Επίπεδο 7 εισόδου"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Επιπέδου 7 εισόδου και εξόδου"
[3]: ./media/guidance-nva-ha/active-passive.png "Ενεργό παθητικές συμπλέγματος"