<properties 
    pageTitle="Εφαρμογή εντοπισμού ρυθμίσεις μητρώου για τις υπηρεσίες του διακομιστή μεσολάβησης στο cloud | Microsoft Azure" 
    description="Στόχος αυτού του θέματος είναι να σας παρέχει τα βήματα που πρέπει να εκτελέσετε για να ρυθμίσετε την απαιτούμενη θύρα στον υπολογιστή που εκτελεί τον παράγοντα Cloud εφαρμογή εντοπισμού." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Ρυθμίσεις μητρώου εντοπισμού εφαρμογή cloud για τις υπηρεσίες του διακομιστή μεσολάβησης

Από προεπιλογή, τον παράγοντα Cloud εφαρμογή εντοπισμού έχει ρυθμιστεί ώστε να χρησιμοποιήσετε μόνο τις θύρες 80 ή 443. Εάν σχεδιάζετε σχετικά με την εγκατάσταση Cloud εντοπισμού εφαρμογών σε περιβάλλον με ένα διακομιστή μεσολάβησης που χρησιμοποιεί μια προσαρμοσμένη θύρα (80 ούτε 443), πρέπει να ρυθμίσετε τις παραμέτρους του παράγοντες για να χρησιμοποιήσετε αυτήν τη θύρα. Η ρύθμιση παραμέτρων βασίζεται σε έναν αριθμό-κλειδί μητρώου.


Στόχος αυτού του θέματος είναι να σας παρέχει τα βήματα που πρέπει να εκτελέσετε για να ρυθμίσετε την απαιτούμενη θύρα στον υπολογιστή που εκτελεί τον παράγοντα Cloud εφαρμογή εντοπισμού.



**Για να τροποποιήσετε τη θύρα που χρησιμοποιείται από τον υπολογιστή που εκτελεί τον παράγοντα Cloud εφαρμογή εντοπισμού, ακολουθήστε τα παρακάτω βήματα:**


1. Ξεκινήστε τον Επεξεργαστή μητρώου. <br> ![Εκτέλεση](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Μεταβείτε στο ή δημιουργήστε το ακόλουθο κλειδί μητρώου: <br> **Discovery\Endpoint εφαρμογή HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud** 

3. Δημιουργήστε μια νέα τιμή **πολλών συμβολοσειρών** που ονομάζεται **θύρες**. ![Νέα](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Για να ανοίξετε το παράθυρο διαλόγου **Επεξεργασία πολλαπλών συμβολοσειρών** , κάντε διπλό κλικ την τιμή θύρες.


5. Στο πλαίσιο κειμένου δεδομένων τιμή, πληκτρολογήστε τις παρακάτω τιμές και προσθέστε όλες οι προσαρμοσμένες θύρες που χρησιμοποιούνται από την εταιρεία σας: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Επεξεργασία πολλών συμβολοσειρών](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου **Επεξεργασία πολλαπλών συμβολοσειρών** .



**Πρόσθετοι πόροι**


* [Πώς μπορώ να να ανακαλύψετε unsanctioned cloud εφαρμογών που χρησιμοποιούνται στην εταιρεία μου](active-directory-cloudappdiscovery-whatis.md) 

