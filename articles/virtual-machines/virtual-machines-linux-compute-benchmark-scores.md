<properties
 pageTitle="Υπολογισμός βαθμολογίες συγκριτικών για ΣΠΣ Linux | Microsoft Azure"
 description="Σύγκριση CoreMark υπολογισμού συγκριτικών βαθμολογίες για εκτέλεση Linux ΣΠΣ Azure"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-linux-vms"></a>Υπολογισμός βαθμολογίες συγκριτικών για ΣΠΣ Linux

Οι παρακάτω βαθμολογίες συγκριτικών CoreMark εμφάνιση επιδόσεις υπολογισμού για του Azure υψηλών επιδόσεων Εικονική κατάλογος εκτελείται Ubuntu. Πρέπει να υπολογίσετε συγκριτικών βαθμολογίες είναι επίσης διαθέσιμες για [Τα Windows ΣΠΣ](virtual-machines-windows-compute-benchmark-scores.md).




## <a name="a-series---compute-intensive"></a>A-σειρά-στενής υπολογισμού


Μέγεθος | vCPUs | Κόμβους NUMA | CPU | Εκτελείται | Αριθμός διαδοχικών προσεγγίσεων/sec | Τυπική απόκλιση
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon CPU E5-2670 0 @ 2.6 GHz| 179 | 110,294 | 554
Standard_A9 | 16 | 2 | Intel Xeon CPU E5-2670 0 @ 2.6 GHz| 189 | 210,816| 2,126
Standard_A10 | 8 | 1 | Intel Xeon CPU E5-2670 0 @ 2.6 GHz| 188 | 110,025 | 1,045
Standard_A11 | 16 | 2 | Intel Xeon CPU E5-2670 0 @ 2.6 GHz| 188 | 210,727| 2,073

## <a name="dv2-series"></a>Dv2 σειρά


Μέγεθος | vCPUs | Κόμβους NUMA | CPU | Εκτελείται | Αριθμός διαδοχικών προσεγγίσεων/sec | Τυπική απόκλιση
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 140 | 14,852 | 780
Standard_D2_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 133 | 29,467 | 1,863
Standard_D3_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 139 | 56,205 | 1,167
Standard_D4_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 126 | 108,543 | 3,446
Standard_D5_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 126 | 205,332 | 9,998
Standard_D11_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 125 | 28,598 | 1,510
Standard_D12_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 131 | 55,673 | 1,418
Standard_D13_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 140 | 107,986 | 3,089
Standard_D14_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 140 | 208,186 | 8,839
Standard_D15_v2 | 20 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz |28 | 268,560 | 4,667

## <a name="f-series"></a>Σειρά F

Μέγεθος | vCPUs | Κόμβους NUMA | CPU | Εκτελείται | Αριθμός διαδοχικών προσεγγίσεων/sec | Τυπική απόκλιση
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_F1 | 1 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 154 | 15,602 | 787
Standard_F2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 126 | 29,519 | 1,233
Standard_F4 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 147 | 58,709 | 1,227
Standard_F8 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 224 | 112,772 | 3,006
Standard_F16 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 42 | 218,571 | 5,113



## <a name="g-series"></a>Σειρά G


Μέγεθος | vCPUs | Κόμβους NUMA | CPU | Εκτελείται | Αριθμός διαδοχικών προσεγγίσεων/sec | Τυπική απόκλιση
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1 | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 31,310 | 2,891
Standard_G2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 60,112 | 3,537
Standard_G3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 107,522 | 4,537
Standard_G4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 195,116 | 5,024
Standard_G5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 360,329 | 14,212

## <a name="gs-series"></a>GS σειράς


Μέγεθος | vCPUs | Κόμβους NUMA | CPU | Εκτελείται | Αριθμός διαδοχικών προσεγγίσεων/sec | Τυπική απόκλιση
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_GS1 | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 28,613 | 1,884
Standard_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 54,348 | 3,474
Standard_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 104,564 | 1,834
Standard_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 194,111 | 4,735
Standard_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 357,396 | 16,228


## <a name="h-series"></a>H σειρά

Μέγεθος | vCPUs | Κόμβους NUMA | CPU | Εκτελείται | Αριθμός διαδοχικών προσεγγίσεων/sec | Τυπική απόκλιση
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 28 | 140,782 | 2,512
Standard_H16 | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 35 | 275,289 | 7,110 
Standard_H18m | 8 | 1 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 28 | 139,071 | 3,988 
Standard_H16m | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 28 | 275,988 | 6,963 
Standard_H16r | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 28 | 273,982 | 6,069 
Standard_H16mr | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 28 | 274,523 | 5.698 



## <a name="about-coremark"></a>Σχετικά με την CoreMark

Αριθμοί Linux έχουν υπολογιστεί εκτελώντας [CoreMark](http://www.eembc.org/coremark/faq.php) στον Ubuntu. CoreMark έχει ρυθμιστεί με τον αριθμό των νήματα ορίστε τον αριθμό των εικονικού CPU και ταυτόχρονης εκτέλεσης οριστεί σε PThreads. Ο αριθμός προορισμού των διαδοχικών προσεγγίσεων έχει ρυθμιστεί με βάση αναμενόμενη απόδοση για την παροχή χρόνο εκτέλεσης (συνήθως πολύ περισσότερο) τουλάχιστον 20 δευτερόλεπτα. Η τελική βαθμολογία αντιπροσωπεύει τον αριθμό των διαδοχικών προσεγγίσεων ολοκληρώθηκε διά τον αριθμό των δευτερολέπτων που χρειάστηκαν για να εκτελέσετε τον έλεγχο. Κάθε δοκιμή εκτελέστηκε τουλάχιστον επτά φορές κάθε Εικονική. Ελέγχει (εκτός από την για H series_ έχουν εκτελούνται στο Οκτωβρίου 2015 σε πολλές VM σε κάθε περιοχή Azure δημόσια η Εικονική ήταν υποστηρίζονται στις κατά την εκτέλεση ημερομηνία.

## <a name="next-steps"></a>Επόμενα βήματα



* Για χωρητικότητα αποθήκευσης, λεπτομέρειες δίσκου και επιπλέον ζητήματα για την επιλογή μεταξύ των μεγέθη Εικονική, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-linux-sizes.md).

* Για να εκτελέσετε τις δέσμες ενεργειών CoreMark σε VM Linux, κάντε λήψη του [πακέτου δέσμης ενεργειών CoreMark](http://download.microsoft.com/download/3/0/5/305A3707-4D3A-4599-9670-AAEB423B4663/AzureCoreMarkScriptPack.zip).